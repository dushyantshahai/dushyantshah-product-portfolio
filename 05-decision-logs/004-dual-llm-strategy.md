# Decision Log 004: Dual-LLM Strategy

**Date:** January 2026
**Decision area:** AI / technical
**Status:** Outcome confirmed ✅

---

## Context

MorphEd's AI pipeline has two distinct tasks: (1) ingesting a textbook and extracting a structured chapter/topic hierarchy (TOC parsing), and (2) generating MCQs from retrieved content chunks. Both tasks require an LLM — but they have fundamentally different requirements.

The question was whether to use a single model for both tasks (simpler architecture, one API dependency) or to select the best model for each job (more complexity, better outcomes).

---

## Options Considered

**Option A: Single model for both tasks**
Use one LLM — either Claude or Gemini — for both TOC extraction and MCQ generation.
- Simpler architecture: one API key, one billing relationship, one failure mode to monitor
- Forces a compromise: the model best suited for generation may be poor at parsing long documents, and vice versa
- Claude 3 Haiku is fast and cost-efficient for generation but lacks the context window needed to process a full 400-page textbook in a single pass
- Gemini 2.0 Flash Preview handles long documents natively but is not optimised for the structured, repeatable JSON output that MCQ generation requires

**Option B: Two models, two jobs**
Use Claude 3 Haiku for MCQ generation and Gemini 2.0 Flash Preview for TOC extraction. Each model is selected for the specific task where it demonstrably outperforms the alternative.
- More architectural complexity: two API integrations, two billing relationships
- Each model is used at its point of maximum advantage
- Cost-efficient: Claude Haiku is among the cheapest capable models for structured generation; Gemini Flash is cost-effective for long-context document processing

---

## Choice

**Option B — Dual-LLM strategy.**

---

## Rationale

**Claude 3 Haiku for MCQ generation:** Generation is a structured, repeatable task — produce a JSON array of questions with four options, a correct answer, and a difficulty tag, grounded in provided context chunks. It runs dozens of times per day across all institutes. Haiku's combination of speed, low cost per token, and high fidelity to structured output instructions makes it the right tool. Using a larger model for this task would increase costs 5–10x without a meaningful quality gain.

**Gemini 2.0 Flash Preview for TOC extraction:** Parsing a 400-page textbook requires holding the entire document in context simultaneously — tracking heading hierarchies, page numbers, and nested structures across hundreds of pages. Gemini 2.0 Flash Preview's 1M+ token context window makes this a single API call rather than a fragile multi-step chunking pipeline. It also parses documents natively (including scanned PDFs and varied layouts), eliminating the need for a separate OCR preprocessing layer in most cases.

The decision was made after testing both models on both tasks. The performance gap — especially on TOC extraction — was large enough to justify the additional architectural complexity.

---

## Outcome / Reflection

The dual-model architecture has performed as expected. MCQ generation quality has been consistent and cost-efficient. TOC extraction via Gemini has handled diverse textbook formats (numbered, lettered, unnumbered, mixed) without requiring a separate OCR pipeline in the majority of cases.

The main ongoing challenge is admin review of TOC output for textbooks with non-standard layouts — particularly older books with inconsistent heading styles. This is a content quality problem, not a model selection problem. The parser flags low-confidence extractions for human review, which has been sufficient for V1.
