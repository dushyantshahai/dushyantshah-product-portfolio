# Syllabus Parser

## Problem

Syllabi are the foundational document of every educational experience — they define what students must learn and what teachers must assess. Yet they exist almost exclusively as **unstructured PDFs with no machine-readable topic structure.**

A syllabus might have 14 units, 60 chapters, and 200+ sub-topics. But this information is locked in a human-readable layout: inconsistent numbering, mixed heading styles, multi-column layouts, scanned pages, and typographic inconsistencies that differ between universities, departments, and even individual teachers.

This creates a real problem for any EdTech tool that wants to be syllabus-aware. Before you can build RAG-based MCQ generation, adaptive learning, or topic-level recommendations, you need a reliable, structured representation of what the syllabus actually contains.

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
Stage 3 — Claude Structured Extraction
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
| LLM | Claude 3 Haiku (cost-optimised for high-frequency extraction) |
| OCR | pytesseract + OpenCV (image pre-processing) |
| PDF parsing | PyMuPDF (text-layer), pdf2image (scanned) |
| Output schema | Pydantic v2 with nested models |
| Language detection | langdetect library |
| Backend | FastAPI (shared with MorphEd) |
| Storage | Supabase (syllabus + parsed hierarchy storage) |

**Why Claude Haiku for extraction (not Sonnet)?** The extraction task is structured and repetitive — it doesn't require deep reasoning, just reliable schema adherence. Haiku handles this at 10x lower cost than Sonnet, making it viable for parsing every syllabus upload without significant API spend.

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

**1. Table of contents is the single best signal — when it exists.**
When a syllabus has a ToC, using it as the primary structure source and cross-validating with the body text yields near-perfect extraction. The challenge: only ~55% of the syllabi tested had a ToC. For the rest, heading inference is necessary.

**2. Indian university syllabi have surprisingly diverse formatting conventions.**
Mumbai University, Pune University, and state board syllabi use fundamentally different numbering conventions, heading hierarchies, and layout styles. There is no universal template. The parser had to be flexible enough to handle: numbered (1.1, 1.2), lettered (A, B, C), Roman numeral (I, II, III), and unnumbered (heading-only) structures — sometimes mixed within the same document.

**3. Claude is better at hierarchy inference than rule-based systems — but needs examples.**
Early versions of the extraction prompt produced flat lists when the syllabus had ambiguous hierarchies. Adding few-shot examples of correct hierarchy extraction (showing the difference between a chapter-level and sub-topic-level heading in similar documents) improved accuracy from 74% to 91%.

**4. The human review interface is essential — and reveals a lot about trust.**
Teachers who received a parsed hierarchy and immediately trusted it (no review) had a higher rate of downstream MCQ quality complaints. Teachers who reviewed and made small corrections felt more ownership over the output and rated final MCQs higher — even when the corrections were minor. The review step isn't just a quality gate; it's a trust-building ritual.

**5. Confidence scoring changes teacher behaviour positively.**
Showing "extraction confidence: 94%" vs. a blank confidence indicator had a clear effect: teachers treated low-confidence extractions with appropriate scepticism. Counterintuitively, showing confidence scores increased overall satisfaction because it set expectations accurately — teachers knew when to review carefully and when to trust the output.
