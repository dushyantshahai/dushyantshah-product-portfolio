# Decision Log 002: RAG Architecture over Direct Generation

**Date:** January 2026
**Decision area:** AI / technical
**Status:** Outcome confirmed ✅

---

## Context

MorphEd's core value proposition is MCQ generation that is grounded in a specific textbook — not generic AI output. Before writing production code, the architectural question was: **how do we make an LLM generate questions that are reliably tied to a specific uploaded document?**

Two serious approaches existed. The choice between them determined the data model, the ingestion pipeline, the cost structure, and the quality ceiling of the entire product.

---

## Options Considered

**Option A: Direct Generation ("dump and prompt")**
Upload the textbook, pass it (or large chunks of it) directly to the LLM in the prompt, and ask for MCQs.
- Simple to implement; no vector database or ingestion pipeline required
- Works for short documents; degrades significantly on 300–500 page textbooks
- Context window limits force chunking — losing structural context across chapter boundaries
- No permanent content index: every assessment generation requires re-uploading or re-processing the book
- Output inconsistency: repeated runs on the same content produce different questions, making quality control unreliable
- Hallucination risk increases as the document gets longer and the model loses precise grounding

**Option B: RAG (Retrieval-Augmented Generation)**
Index the textbook once into a vector store. At generation time, retrieve the most relevant chunks for the selected chapter/topic and pass them as scoped context to the LLM.
- One-time ingestion: book is uploaded and indexed once; all future assessments draw from the same index without reprocessing
- Structured TOC: ingestion builds a permanent chapter/topic hierarchy that professors navigate directly — no ambiguity about what content the AI will use
- Grounding: the LLM is explicitly constrained to generate from retrieved source material only
- Retrieval adds latency (~200–400ms per request) and requires a chunking/embedding pipeline
- Quality is bounded by retrieval quality — poor chunking degrades MCQ relevance

---

## Choice

**Option B — RAG pipeline.**

---

## Rationale

The core issue with direct generation is that it doesn't scale to the actual content format MorphEd targets: full-length textbooks of 300–500 pages. Even if it worked technically, the absence of a permanent content index would mean every assessment requires the book to be reprocessed — a poor user experience and a significant cost inefficiency.

RAG solves the two problems that matter most for a trust-sensitive product: consistency (the same chapter produces reliably similar quality on repeated runs) and grounding (questions can be traced back to specific source chunks). For an assessment platform used in formal academic settings, traceable grounding is not a nice-to-have.

The retrieval latency and pipeline complexity were real costs — but one-time architectural costs, not recurring ones.

---

## Outcome / Reflection

The RAG architecture has held up well. The one-time ingestion model has worked exactly as intended — books are uploaded once by the admin and used repeatedly by professors without any reprocessing overhead.

The main friction encountered was in TOC parsing quality, not retrieval quality. Getting the chapter/topic hierarchy right during ingestion turned out to be the harder problem. Gemini 2.0 Flash Preview's large context window (handling the full book in one pass) resolved most of this — but the parser still requires admin review for complex or non-standard textbook layouts.
