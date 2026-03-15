# AI Architecture — MorphEd RAG Pipeline

## Overview

MorphEd uses a Retrieval-Augmented Generation (RAG) architecture to ensure that every MCQ is grounded in the teacher's actual syllabus document — not in the LLM's general world knowledge. The pipeline has five distinct stages: ingestion, chunking, embedding, retrieval, and generation.

---

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                        INGESTION LAYER                          │
│                                                                 │
│   Teacher uploads PDF/DOCX                                      │
│          │                                                      │
│          ▼                                                      │
│   Document Parser (PyMuPDF / python-docx)                       │
│          │                                                      │
│          ▼                                                      │
│   Raw Text Extraction + Layout Preservation                     │
│          │                                                      │
│          ▼                                                      │
│   Topic Hierarchy Extractor (Claude — structured output)        │
│          │                                                      │
│   Output: JSON topic tree {unit → chapter → subtopic}           │
└──────────────────────┬──────────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────────┐
│                      CHUNKING LAYER                             │
│                                                                 │
│   Semantic chunking by topic boundary                           │
│   Chunk size: ~500 tokens with 50-token overlap                 │
│   Each chunk tagged with: topic_id, unit, chapter, page_ref     │
└──────────────────────┬──────────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────────┐
│                      EMBEDDING LAYER                            │
│                                                                 │
│   Embedding model: text-embedding-3-small (OpenAI)             │
│   Vector store: Supabase pgvector (hosted Postgres)             │
│   Each chunk → 1536-dim vector + metadata stored                │
└──────────────────────┬──────────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────────┐
│                      RETRIEVAL LAYER                            │
│                                                                 │
│   Teacher selects topic(s) from hierarchy                       │
│          │                                                      │
│          ▼                                                      │
│   Query: embed topic label + retrieve top-k chunks              │
│   k = 8–12 chunks (tuned via precision/recall experiments)      │
│          │                                                      │
│   Metadata filter: restrict retrieval to selected topic_ids     │
│          │                                                      │
│   Output: Ranked context passages for generation                │
└──────────────────────┬──────────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────────┐
│                     GENERATION LAYER                            │
│                                                                 │
│   LLM: Claude 3.5 Sonnet (primary)                              │
│   Prompt: System + retrieved context + generation instructions  │
│          │                                                      │
│          ▼                                                      │
│   Structured output: JSON array of MCQ objects                  │
│   {question, option_a, option_b, option_c, option_d,            │
│    correct_option, explanation, topic_ref, difficulty}          │
│          │                                                      │
│          ▼                                                      │
│   Output validator (Pydantic schema check)                      │
│          │                                                      │
│          ▼                                                      │
│   MCQ Review UI → Teacher edits/approves → Export               │
└─────────────────────────────────────────────────────────────────┘
```

---

## Component Deep-Dive

### 1. Document Ingestion & Topic Hierarchy Extraction

**Challenge:** Syllabus PDFs are wildly inconsistent. Some are scanned images. Some have complex table layouts. Some mix Hindi and English. A naive text extraction misses structure.

**Solution:**
- PyMuPDF handles text-layer PDFs; pytesseract + OpenCV handles scanned docs
- After raw extraction, Claude is prompted to identify the topic hierarchy using structured output mode (JSON schema enforcement)
- The hierarchy extraction prompt uses chain-of-thought to handle edge cases like unnumbered sections, appendices, and multi-language syllabi

**Why Claude for hierarchy extraction (not a regex/NLP approach):** Rule-based parsers fail on the diversity of Indian university syllabus formats. Claude handles ambiguous formatting gracefully and can infer logical structure even when visual structure is absent.

---

### 2. Chunking Strategy

**Challenge:** Standard fixed-size chunking splits content mid-concept, leading to incoherent retrieval.

**Solution:** Semantic chunking aligned to topic boundaries identified in the hierarchy extraction step. If a sub-topic is short (<200 tokens), it's merged with adjacent sub-topics in the same chapter. If it's long (>800 tokens), it's split with overlap at sentence boundaries.

**Metadata tagging** on each chunk ensures that retrieval can be filtered by topic hierarchy — critical for preventing cross-topic contamination in MCQ generation.

---

### 3. Embedding & Vector Store

**Model choice:** `text-embedding-3-small` was chosen over `text-embedding-ada-002` (predecessor) for better cost/performance ratio and multilingual support (important for Hindi-medium syllabi). `text-embedding-3-large` was evaluated but offered marginal accuracy gains at 5x the cost.

**Vector store:** Supabase pgvector was chosen over Pinecone or Weaviate for three reasons:
1. Single infrastructure for both relational data (user accounts, quiz history) and vector search
2. Row-level security for multi-tenant access control (each teacher's syllabus is isolated)
3. Lower operational complexity for a solo founder building v1

**Trade-off acknowledged:** Supabase pgvector has known performance ceilings at >1M vectors. If MorphEd scales to millions of syllabus uploads, a dedicated vector DB (Pinecone, Qdrant) becomes the right move.

---

### 4. Retrieval

**Top-k tuning:** Experiments ran k=5, k=8, k=12, k=15. At k=5, MCQs sometimes missed important context. At k=15, the context window became noisy and hallucination risk increased. k=8–10 emerged as the sweet spot for standard chapter-length topics.

**Metadata filtering:** Retrieval is always filtered to the teacher-selected topic_ids. This prevents the model from pulling in context from other chapters (which would produce technically valid but out-of-scope questions).

**Hybrid retrieval (planned for v2):** Combining dense retrieval (vector similarity) with BM25 keyword search to better handle factual content like definitions, formulas, and proper nouns that may not embed well semantically.

---

### 5. Generation & Output Validation

**Primary LLM:** Claude 3.5 Sonnet
- Chosen for its instruction-following reliability, structured output capability, and strong performance on MCQ generation benchmarks run during prototyping
- System prompt enforces: "Generate questions ONLY from the provided context. Do not use outside knowledge."

**Structured output:** The generation prompt requests a strict JSON schema. Claude's structured output mode ensures fields are always present and correctly typed, eliminating downstream parsing failures.

**Pydantic validation:** Every LLM response is validated against a `MCQItem` Pydantic model before being shown to the teacher. Invalid or malformed outputs trigger a single automatic retry before surfacing a graceful error.

**Explanation field:** Every MCQ includes an explanation grounded in the retrieved context. This serves two purposes: (1) pedagogical value for students, and (2) as an indirect signal of generation quality — if the explanation doesn't cite a relevant passage, the question likely has low grounding.

---

## Tech Stack Summary

| Layer | Technology | Rationale |
|---|---|---|
| Frontend | Next.js + Tailwind CSS | Fast iteration, good DX, Vercel deployment |
| Backend | FastAPI (Python) | Async support, Pydantic integration, easy LLM orchestration |
| LLM (Generation) | Claude 3.5 Sonnet | Best MCQ quality in prototyping evaluations |
| LLM (Extraction) | Claude 3 Haiku | Cheaper for high-frequency hierarchy parsing |
| Embeddings | OpenAI text-embedding-3-small | Cost-effective, multilingual |
| Vector Store | Supabase pgvector | Unified infra, row-level security |
| Document Parsing | PyMuPDF, pytesseract | Handles both text-layer and scanned PDFs |
| Auth | Supabase Auth | Built-in, integrates with DB RLS |
| Hosting | Vercel (frontend) + Railway (backend) | Low-ops, suitable for v1 scale |

---

## Known Limitations & Future Improvements

1. **Scanned PDF quality dependency:** OCR accuracy drops below 85% on low-resolution or handwritten syllabi. Planned fix: image pre-processing pipeline + confidence scoring to flag low-quality extractions.

2. **Long-context retrieval degradation:** For syllabi >200 pages, retrieval precision drops. Hierarchical indexing (retrieve at chapter level first, then sub-topic) is the planned mitigation.

3. **Language support:** Currently optimised for English. Hindi-medium syllabus support is partially functional but not production-ready. Full multilingual support is a v2 commitment.

4. **No streaming:** MCQ generation currently waits for the full response before displaying. Streaming output with progressive card rendering is on the v2 UX roadmap to reduce perceived latency.
