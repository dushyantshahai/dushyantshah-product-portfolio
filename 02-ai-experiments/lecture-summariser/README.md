# Lecture Summariser

## Problem

Lecture recordings are one of the most underutilised study resources in Indian colleges. Students record or access hour-long lectures but rarely go back to them — they're too long, unstructured, and hard to scan.

When exam season hits, students re-read their handwritten notes (incomplete) or buy generic guides (not syllabus-specific). The lecture recording — the most accurate capture of what the teacher actually taught — sits unwatched.

The core problem: **lecture recordings are unstructured and time-consuming to revise from.** A 90-minute lecture can't be "skimmed." But the same content, restructured as a 2-page revision note, can be reviewed in 10 minutes.

---

## What It Does

The Lecture Summariser takes a lecture transcript (or audio file with auto-transcription) and converts it into a structured revision document using Claude + structured output.

### Output Format

For each lecture processed, the tool generates:

```
📌 Lecture Title (inferred or entered by user)
📅 Topic Coverage (mapped to syllabus if available)

## Key Concepts
- [Concept 1]: One-sentence definition + example from the lecture
- [Concept 2]: ...

## Definitions & Formulas
- [Term]: [Definition as stated in lecture]
- [Formula]: [Equation with context]

## Examples Discussed
1. [Example 1]: Brief description of the example and what it illustrates
2. [Example 2]: ...

## Common Mistakes / Warnings
(Populated from phrases like "students often confuse...", "don't mix up...", "this is a common exam mistake...")

## 3 Self-Check Questions
Q1: [Question based on key concept]
Q2: [Question based on formula/definition]
Q3: [Application question based on an example]

## Suggested MCQ Practice Topics
(Cross-references with syllabus if uploaded — "Based on this lecture, practise MCQs on: ...")
```

---

## How It Works

```
User uploads lecture transcript (TXT/DOCX) or audio file (MP3/M4A)
          ↓
[If audio] Transcription via Whisper API (OpenAI)
          ↓
Transcript passed to Claude with structured output prompt
System prompt instructs:
  - Identify key concepts, definitions, formulas, examples
  - Flag teaching moments ("students often...", "remember that...")
  - Generate self-check questions
  - Output strictly as the defined JSON schema
          ↓
JSON → Rendered revision note (Markdown / PDF)
          ↓
[Optional] Cross-reference with uploaded syllabus:
  - Map lecture content to specific syllabus topics
  - Surface gaps ("this lecture doesn't cover sub-topic 4.3")
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| LLM | Claude 3.5 Sonnet (structured output mode) |
| Transcription | OpenAI Whisper API (audio → text) |
| Output schema | Pydantic model with strict validation |
| Frontend | Minimal Next.js upload + preview UI |
| Export | Markdown → PDF via Pandoc |
| Storage | Supabase (transcript and summary storage) |

---

## Current Status

**Status: Prototype — tested on 12 lecture transcripts**

- Text transcript input: fully functional
- Audio transcription (Whisper): integrated, tested on 5 lecture recordings
- Structured output with schema validation: functional
- Syllabus cross-referencing: partial — maps to chapter level, not sub-topic
- PDF export: basic formatting, not print-ready
- UI: minimal, functional, not polished

**Sample outputs tested on:**
- B.Sc. Computer Science: Data Structures lectures (Arrays, Linked Lists)
- B.Com: Principles of Accounting lectures
- Engineering Mathematics: Differential Equations lecture series

---

## Key Learnings

**1. Whisper transcription quality varies dramatically by audio quality.**
Clean audio (recorded lecture with a decent mic) → 95%+ accuracy. Phone-recorded lecture in a noisy classroom → 70–80% accuracy with meaningful errors on technical terminology. Pre-processing audio (noise reduction, speaker diarisation) is needed before Whisper on low-quality recordings.

**2. Claude's structured output is excellent at extracting definitions but struggles with implicit knowledge.**
When a teacher explains a concept through a long analogy without stating "the definition is...", Claude often misses it. Adding a prompt instruction to look for implicit definitions ("the teacher is explaining what X is, even if they don't say 'X is defined as'") improved extraction significantly.

**3. The "common mistakes" section is the highest-value output for exam prep.**
Students rated the "Common Mistakes / Warnings" section as the most useful part of the summary — more useful than the concept list or definitions. Teachers naturally signal exam-important content by saying things like "this is where most students lose marks" or "don't confuse this with...". Mining these signals adds disproportionate value.

**4. Self-check questions work better when grounded in lecture examples.**
Generic questions about a topic are easy to find on Google. Questions that reference the specific example or framing used by the teacher in the lecture are unique to that class — they test whether the student was actually following along, not just whether they know the subject.

**5. The tool's value compounds when integrated with MorphEd.**
When lecture summaries are cross-referenced with the syllabus and the output suggests "practise MCQs on topics 4.2 and 4.3 based on today's lecture," it creates a natural handoff to MorphEd for practice. This integration is the core v2 product thesis for both tools.
