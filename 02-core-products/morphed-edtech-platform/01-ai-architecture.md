# AI Architecture — MorphEd RAG Pipeline

## Overview

MorphEd uses a Retrieval-Augmented Generation (RAG) architecture to ensure that every MCQ is grounded in the teacher's actual syllabus document — not in the LLM's general world knowledge. The pipeline has five distinct stages: ingestion, chunking, embedding, retrieval, and generation.

---

## Architecture Diagram

```
╔═════════════════════════════════════════════════════════════════════════╗
║               AaaS PLATFORM — USER FLOWS ARCHITECTURE                   ║
╚═════════════════════════════════════════════════════════════════════════╝

         ┌──────────────┐    ┌──────────────────┐    ┌──────────────┐
         │    ADMIN     │    │    PROFESSOR      │    │   STUDENT    │
         │ System Admin │    │ Educator/Creator  │    │Learner/Taker │
         └──────────────┘    └──────────────────┘    └──────────────┘
                │                      │                      │
                │ ① ②                  │ ⑤ ⑥ ⑦               │ ⑧ ⑨ ⑩
                ▼                      │                      │
╔══════════════════════════════════════╪══════════════════════╪══════════╗
║  MODULE 1 — USER MANAGEMENT SERVICE  │                      │          ║
╠══════════════════════════════════════╪══════════════════════╪══════════╣
║  Components:  [ User Profiles ]  [ Batch Management ]       │          ║
║                                                              │          ║
║  ①  Manage Users    Create or delete Professor & Student    │          ║
║                     accounts                                 │          ║
║  ②  Manage Batches  Create or delete student batches for    │          ║
║                     organisation                             │          ║
╚══════════════════════════════════╤══╪══════════════════════╪══════════╝
                                   │  │                      │
                                   ▼  │                      │
╔══════════════════════════════════════╪══════════════════════╪══════════╗
║  MODULE 2 — CONTENT INGESTION PIPELINE                      │          ║
╠══════════════════════════════════════╪══════════════════════╪══════════╣
║  Components:  [ Book Upload (PDF/DOCX) ]  [ TOC Parsing Engine ]       ║
║                                                              │          ║
║  ③  Upload Content  Admin uploads textbook into system      │          ║
║  ④  Extract TOC     System parses book & builds hierarchy   │          ║
║                                                              │          ║
║  Output — 4-Level Content Hierarchy:                         │          ║
║  ┌────────┐    ┌─────────┐    ┌───────┐    ┌───────────┐   │          ║
║  │  Book  │ ──►│ Chapter │ ──►│ Topic │ ──►│ Sub-topic │   │          ║
║  │  (L1)  │    │  (L2)   │    │  (L3) │    │   (L4)    │   │          ║
║  └────────┘    └─────────┘    └───────┘    └───────────┘   │          ║
╚══════════════════════════════════╤══╪══════════════════════╪══════════╝
                                   │  │                      │
                                   ▼  ▼                      ▼
╔═════════════════════════════════════════════════════════════════════════╗
║  MODULE 3 — ASSESSMENT ENGINE                                           ║
╠═════════════════════════════════════════════════════════════════════════╣
║  Components:  [ Generator ]  [ Publisher ]  [ Exam Execution ]          ║
║                                                                          ║
║  Professor Actions:                                                      ║
║  ⑤  Generate Assessment  Create MCQs from TOC (Chapter / Topic)         ║
║  ⑥  Manage Assessments   Publish to batches or Unpublish                ║
║                                                                          ║
║  Student Actions:                                                        ║
║  ⑧  Attempt Assessment   Start and progress through published quiz      ║
║  ⑨  Submit Assessment    Finalise and submit for grading                ║
╚══════════════════════════════════════════╤══════════════════════════════╝
                                           │
                                           ▼
╔═════════════════════════════════════════════════════════════════════════╗
║  MODULE 4 — ANALYTICS & REPORTING                                       ║
╠═════════════════════════════════════════════════════════════════════════╣
║  Components:  [ Results Engine ]  [ Dashboards ]                        ║
║                                                                          ║
║  ┌─────────────────────────────────┐  ┌───────────────────────────────┐ ║
║  │  PROFESSOR DASHBOARD            │  │  STUDENT DASHBOARD            │ ║
║  │                                 │  │                               │ ║
║  │  ⑦  View Analytics             │  │  ⑩  View Results             │ ║
║  │  •  Batch-level performance     │  │  •  Individual scores         │ ║
║  │  •  Student-level metrics       │  │  •  Detailed answer review    │ ║
║  │  •  Topic-level weak areas      │  │  •  Personal topic analytics  │ ║
║  └─────────────────────────────────┘  └───────────────────────────────┘ ║
╚═════════════════════════════════════════════════════════════════════════╝


  ACTION REFERENCE
  ─────────────────────────────────────────────────────────────────────
  ADMIN        ①  Manage Users   ②  Manage Batches   ③  Upload Content
               ④  Extract TOC

  PROFESSOR    ⑤  Generate Assessment   ⑥  Manage Assessments
               ⑦  View Analytics

  STUDENT      ⑧  Attempt Assessment   ⑨  Submit Assessment
               ⑩  View Results
```

---

## Component Deep-Dive

### 1. Document Ingestion & Topic Hierarchy Extraction

**Challenge:** Textbook PDFs are wildly inconsistent — scanned images, complex multi-column layouts, mixed languages. Traditional text extraction tools (PyMuPDF, Tesseract) often lose visual hierarchy cues like font size, bold headings, and page breaks, making it hard to reliably reconstruct the Chapter → Topic → Sub-topic structure.

**Solution:**
- **Vision-Language Ingestion:** Instead of OCR pipelines, the system uses Google Gemini 1.5/3 Flash's native multimodal PDF understanding. The entire document is converted to base64 and sent in a single shot, leveraging Gemini's 1M+ token context window — no page merging or pre-chunking required.
- **Structured Output Engine:** Gemini is prompted to visually read the document and extract the full topic hierarchy, returning it directly as enforced JSON (`application/json` mode).
- **Spillover Resolution:** A post-processing step handles edge cases where a topic ends on the same page the next one begins — a common layout quirk in Indian university materials.

**Why Gemini Vision over OCR + Claude:** Gemini natively processes visual layout cues (font weight, indentation, page breaks) without preprocessing. This makes it significantly more robust across the diverse formatting conventions found in Indian university syllabi.

---

### 2. Chunking Strategy

**Challenge:** Fixed-size string chunking splits content mid-concept and mixes distinct topics, causing cross-topic contamination during retrieval and generation.

**Solution:**
- **Topic-Aligned Semantic Chunking:** The system chunks text within the exact start and end pages of each Sub-Topic or Topic defined by the hierarchy extraction step — ensuring every chunk is semantically scoped to a specific section of the book.
- **Proportional Fallback:** Where sub-topic page ranges aren't perfectly bounded, chunks are proportionally distributed across their parent topic and sub-topic, preserving logical alignment.
- **Sentence-Boundary Splitting:** Text within each semantic boundary is split into ~1,000-character chunks at sentence boundaries (`[.!?]`), keeping contexts readable. Fragments under 50 characters are automatically skipped.
- **Deep Metadata Tagging:** Every chunk is stored with its precise `chapterId`, `topicId`, and `subTopicId`. This hierarchical metadata is what makes strict topic-level filtering possible at retrieval time.

---

### 3. Retrieval Layer — PostgreSQL Full-Text Search (V1)

**Challenge:** Retrieving accurate, topic-scoped context from 1,000+ page textbooks cost-effectively, without the infrastructure overhead of a dedicated vector database at MVP stage.

**Solution:**
- **No Vector Embeddings in V1:** The current implementation bypasses embedding models and vector databases entirely.
- **PostgreSQL Full-Text Search:** Native `to_tsvector` / `plainto_tsquery` with `ts_rank` scoring handles lexical retrieval directly within the existing relational database.
- **Hierarchical SQL Filtering:** Because every chunk is tagged with `chapterId`, `topicId`, and `subTopicId`, retrieval first applies hard SQL filters to narrow the context pool to the exact selected topic before ranking.

**Why PostgreSQL FTS over pgvector:** Zero additional infrastructure — relational data and content retrieval share the same database. For exact-domain retrieval (e.g., "Ohm's Law" from the "Electric Currents" sub-topic), keyword search with strict hierarchical filtering is faster, cheaper, and more predictable than semantic embeddings at V1 scale.

**Trade-off acknowledged:** Lexical search struggles with synonyms and implicit context. The `pgvector` schema (1536-dim vector column for OpenAI embeddings) is already prepared and commented out in the migration files. A move to pgvector or a dedicated vector DB (Pinecone, Qdrant) is planned for V2 to enable true semantic retrieval.

---

### 4. Generation & Retrieval Execution

**Challenge:** Ensuring questions stay strictly within the teacher's selected topic without overflowing the token window — while keeping retrieval simple enough to ship quickly.

**Solution:**
- **Prompt-Based Semantic Filtering (V1):** The system compiles the first 50 chronological chunks from the book and injects the teacher-selected Chapter and Topic titles directly into the LLM prompt as a `FOCUS AREA`. The LLM is instructed to filter context and generate questions solely about that topic.
- **Context Cap:** The hard limit of 50 chunks provides a large enough context window for the LLM to operate accurately while staying within token limits.

**Planned for V2 — Hybrid Retrieval:** Database-level metadata filtering by `topic_id` combined with dense vector retrieval (embeddings) to fetch the top-k most relevant chunks before passing them to the LLM — reducing hallucination risk and token costs.

---

### 5. Generation & Output Validation

**Primary LLM:** Claude 3 Haiku (`claude-3-haiku-20240307`)

Claude 3 Haiku was chosen for production for its cost-efficiency and fast response times — both critical for high-volume MCQ generation. Performance on structured generation from provided syllabus text is more than sufficient for the use case.

**Prompting Strategy:**
- The generation prompt instructs Claude to act as an "expert educational assessment designer"
- Difficulty scaling is instruction-driven: Easy (basic recall) → Medium (application and analysis) → Hard (synthesis and evaluation)
- The prompt grounds generation with "based on the following content" rather than a hard isolation constraint

**Output Validation:**
- Structured output is requested via prompt engineering (pure JSON array format) rather than native Tool Calling or schema enforcement APIs
- A custom `validateMCQ` function checks that all required fields are present and that the correct option is exactly `"A"`, `"B"`, `"C"`, or `"D"`
- Markdown fences (` ```json `) are stripped from the raw response before parsing
- If parsing fails, the system throws a graceful error — automatic retries are planned for V2

**Explanation Field:** Every MCQ includes a mandatory `explanation` field grounded in the source content. This serves two purposes: pedagogical value for students reviewing mistakes, and an indirect grounding signal — if the model can't justify its answer from the context, it's a strong hallucination indicator.

---

## Tech Stack Summary

| Layer | Technology | Rationale |
|---|---|---|
| Frontend | Next.js + Tailwind CSS | Fast iteration, good DX, Vercel deployment |
| Backend | Node.js + Express | Lightweight, fast API layer for V1 |
| LLM (Generation) | Claude 3 Haiku | Cost-efficient, fast, sufficient for structured MCQ generation |
| LLM (Extraction) | Google Gemini 1.5/3 Flash | Multimodal vision — natively reads PDF layout without OCR |
| Retrieval | PostgreSQL Full-Text Search | Zero infra overhead; `to_tsvector` + hierarchical SQL filters |
| Vector Store | pgvector (prepared, not active) | Schema ready for V2 semantic retrieval upgrade |
| Database | Supabase (PostgreSQL) | Unified relational + content store, row-level security |
| Auth | Supabase Auth | Built-in, integrates with DB RLS |
| Hosting | Vercel (frontend) + Railway (backend) | Low-ops, suitable for V1 scale |

---

## Known Limitations & Future Improvements

1. **Lexical retrieval ceiling:** PostgreSQL FTS works well for exact-domain content but struggles with synonyms and implicit context. Moving to pgvector + dense embeddings in V2 will unlock semantic retrieval and improve accuracy on broader or ambiguous topic queries.

2. **Prompt-based topic filtering:** V1 relies on the LLM to self-filter context by focus area rather than strict database-level metadata filtering. This is effective but less precise — V2 will enforce `topic_id`-level retrieval before passing context to the LLM.

3. **No automatic retry on parse failure:** If the LLM returns malformed JSON, the system currently throws a graceful error without retrying. Automatic retry logic with fallback parsing is planned for V2.

4. **No streaming output:** MCQ generation waits for the full LLM response before rendering. Streaming with progressive card display is on the V2 UX roadmap to reduce perceived latency.

5. **Hindi-medium support:** Gemini handles multilingual PDFs better than OCR pipelines, but generation quality for Hindi-medium syllabi is not yet production-ready. Full multilingual support is a V2 commitment.
