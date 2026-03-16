# Syllabus Parser

## Problem

Syllabi are the foundational document of every educational experience — they define what students must learn and what teachers must assess. Yet they exist almost exclusively as **unstructured PDFs with no machine-readable topic structure.**

A syllabus might have 14 units, 60 chapters, and 200+ sub-topics — all locked in inconsistent numbering, mixed heading styles, scanned pages, and typographic quirks that vary between universities, departments, and individual teachers. Before you can build RAG-based MCQ generation, adaptive learning, or topic-level recommendations, you need a reliable structured representation of what the syllabus actually contains.

The Syllabus Parser solves this: it takes a raw, unstructured syllabus PDF and outputs a clean, machine-readable topic hierarchy.

---

## What It Does

The Syllabus Parser extracts and maps topics from raw syllabus PDFs into a structured JSON hierarchy that downstream tools (MorphEd, AI Study Companion) can consume.

### Input

Any syllabus document in PDF or DOCX format. Including:
- Text-layer PDFs (standard)
- Scanned PDFs (requires OCR step)
- Multi-column syllabi
- Syllabi with irregular numbering or formatting
- Hindi-medium and mixed-language syllabi

### Output

```json
{
  "syllabus_id": "uuid",
  "institution": "Mumbai University",
  "subject": "Data Structures and Algorithms",
  "semester": "4",
  "total_units": 4,
  "units": [
    {
      "unit_id": "U1",
      "title": "Introduction to Data Structures",
      "chapters": [
        {
          "chapter_id": "C1.1",
          "title": "Arrays and Strings",
          "subtopics": [
            { "id": "C1.1.1", "title": "One-dimensional Arrays", "page_ref": 3 },
            { "id": "C1.1.2", "title": "Multi-dimensional Arrays", "page_ref": 3 },
            { "id": "C1.1.3", "title": "String Manipulation Functions", "page_ref": 4 }
          ],
          "estimated_hours": 6
        }
      ]
    }
  ],
  "extraction_confidence": 0.92,
  "flags": ["Scanned document — verify topics on page 7", "Table of contents found and used for cross-validation"]
}
```

---

## How It Works

```
Input: Syllabus PDF
          ↓
Stage 1 — Document Classification
  Is it text-layer or scanned?
  → Text-layer: PyMuPDF extraction
  → Scanned: pytesseract + OpenCV pre-processing
          ↓
Stage 2 — Table of Contents Detection
  Does a ToC exist? (regex + layout analysis)
  → If yes: use as primary structure source + verify with body text
  → If no: infer structure from heading patterns in body text
          ↓
Stage 3 — Gemini 2.0 Flash Preview Structured Extraction
  Prompt: "Extract the complete topic hierarchy from this syllabus.
  Output a JSON object with units, chapters, and sub-topics.
  Use the exact titles as they appear in the document."
  Mode: Structured output with JSON schema enforcement
          ↓
Stage 4 — Confidence Scoring & Flagging
  Heuristics to score extraction quality:
  - % of detected headings that mapped to the hierarchy
  - Presence of page references (indicates good structure)
  - Language consistency (flags mixed-language sections)
  - Cross-validation between ToC and body (if both present)
          ↓
Stage 5 — Human Review Interface
  Low-confidence extractions surface a review UI:
  teacher can correct misclassified headings, merge/split topics
          ↓
Output: JSON topic hierarchy + confidence score + flags
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| **LLM** | **Gemini 2.0 Flash Preview** |
| OCR | pytesseract + OpenCV (image pre-processing) |
| PDF parsing | PyMuPDF (text-layer), pdf2image (scanned) |
| Output schema | Pydantic v2 with nested models |
| Language detection | langdetect library |
| Backend | FastAPI (shared with MorphEd) |
| Storage | Supabase (syllabus + parsed hierarchy storage) |

**Why Gemini 2.0 Flash Preview?** Syllabus parsing demands a model that can hold an entire textbook in context without losing structural coherence mid-document. Gemini 2.0 Flash Preview was chosen for three reasons:
- **Very large context window (1M tokens)** — handles long, formatting-heavy documents without chunking or context loss
- **Native multimodal parsing** — reads both text-layer and scanned PDFs/DOCX files directly, eliminating the need for a separate OCR pre-processing step in most cases
- **High structured extraction accuracy** — consistently adheres to JSON schema output on complex, nested hierarchies across diverse document formats

---

## Current Status

**Status: Integrated into MorphEd — production component**

The Syllabus Parser is not a standalone product — it's the ingestion layer of MorphEd. It has been tested on:
- 47 real syllabi from Mumbai University, Pune University, and 3 Maharashtra engineering colleges
- Subjects: Computer Science, Commerce, Engineering, Arts
- Languages: English (38), Hindi (5), mixed Hindi-English (4)

**Performance on test set:**

| Metric | Value |
|---|---|
| Topic hierarchy accuracy (units + chapters correct) | 91% |
| Sub-topic accuracy | 83% |
| High-confidence extractions (score ≥ 0.85) | 74% |
| Scanned PDF accuracy | 79% |
| Hindi-medium accuracy | 68% |

---

## Key Learnings

**1. Table of contents is the single best signal — but not deep enough on its own.**
The ToC captures top-level chapter titles reliably, but rarely reflects content depth at topic and sub-topic levels. To solve this, the TOC parser was built to read each page and identify headers, bullet points, and start/end pages across all levels — producing a true full-book hierarchy rather than relying on the document's own ToC alone.

**2. Different books approved by the same university have surprisingly diverse formatting conventions.**
There is no universal template — even within a single university's approved book list. The parser had to handle all of the following, sometimes mixed within the same document:
- Numbered structures (1.1, 1.2), lettered (A, B, C), Roman numeral (I, II, III), and unnumbered heading-only layouts

**3. Gemini 2.0 Flash Preview is better at hierarchy inference than rule-based systems — but needs examples.**
Early extraction prompts produced flat lists when syllabi had ambiguous hierarchies. Adding few-shot examples that illustrated the distinction between chapter-level and sub-topic-level headings improved accuracy from 74% to 91% — showing that model capability and prompt design are jointly responsible for output quality.

**4. Human review across all three user roles is essential for confidence and reliability.**
Embedding review interfaces at each role's workflow — Admin reviewing parsed book hierarchies in the Content Library, Professors editing generated MCQs before publishing, Students checking per-question results — produced significantly greater trust in AI outputs across the board. Each touchpoint acts as a role-specific quality gate, making the end-to-end system more reliable than any single centralised review step.

**5. Confidence scoring changes teacher behaviour positively.**
Showing "extraction confidence: 94%" vs. a blank confidence indicator had a clear effect: teachers treated low-confidence extractions with appropriate scepticism. Counterintuitively, showing confidence scores increased overall satisfaction because it set expectations accurately — teachers knew when to review carefully and when to trust the output.
