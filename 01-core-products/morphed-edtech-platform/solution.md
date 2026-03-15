# Solution Design — MorphEd

## Product Overview

MorphEd is a web-based AI tool that lets teachers upload a syllabus document, navigate a structured topic hierarchy, and generate syllabus-grounded MCQs on demand. The core promise: **from syllabus PDF to ready-to-use quiz in under 5 minutes.**

The product removes two manual tasks that consume the most teacher time: (1) parsing the syllabus to identify what to assess, and (2) writing questions that accurately reflect that content. MorphEd automates both.

---

## Core User Flow

### Flow 1 — Teacher Creates a Quiz

```
[Upload Syllabus PDF]
        ↓
[Automatic Topic Extraction & Hierarchy Display]
        ↓
[Teacher Selects Topic(s) / Chapter(s)]
        ↓
[Configure Quiz: Number of questions, difficulty level]
        ↓
[AI Generates MCQs with 4 options + correct answer marked]
        ↓
[Teacher Reviews, Edits, or Regenerates individual questions]
        ↓
[Export as PDF / Google Form / Copy to clipboard]
```

### Flow 2 — Student Practises (v2 roadmap)

```
[Teacher shares quiz link with class]
        ↓
[Student opens quiz in browser — no login required for attempt]
        ↓
[Student completes quiz]
        ↓
[Instant score + topic-level feedback shown]
        ↓
[Teacher sees class-level performance dashboard]
```

---

## Key Screens & UX Decisions

### Screen 1: Syllabus Upload

The landing screen is a single action: upload your syllabus. Drag-and-drop or file picker. Supported formats: PDF, DOCX.

**Why this design:** Teachers in early testing said they were intimidated by complex onboarding. A single action as the first screen lowers friction to zero. There's no account creation required for the first session (try-before-register).

**UX decision:** We show a live processing animation ("Extracting your syllabus topics...") rather than a blank loading screen. In usability testing, this reduced perceived wait time and increased trust in the AI extraction step.

---

### Screen 2: Topic Hierarchy

After processing, teachers see a collapsible tree of extracted topics — Unit → Chapter → Sub-topic. Each node shows an estimated question count.

**Example:**
```
📘 Unit 3: Data Structures
  └── Chapter 7: Arrays & Strings
        ├── 7.1 One-dimensional Arrays
        ├── 7.2 Multi-dimensional Arrays
        └── 7.3 String Manipulation Functions
```

**Why this design:** Early prototypes showed a flat topic list. Teachers complained it lost the structural context of the curriculum. A hierarchy mirrors how teachers mentally organise their content and makes topic selection faster.

**UX decision:** Teachers can select at any level — unit, chapter, or sub-topic — and the MCQ count adjusts to show what's possible. Selecting a parent node auto-selects all children (overrideable).

---

### Screen 3: MCQ Configuration

A simple form: number of questions (5–50), difficulty (Easy / Medium / Hard / Mixed), and an optional focus instruction ("emphasise application over recall").

**Why this design:** We deliberately kept this minimal. Power users wanted more controls; early users wanted fewer. We shipped minimal and will add advanced options behind a toggle in v2.

---

### Screen 4: MCQ Review & Edit

Generated MCQs appear in a card layout. Each card shows:
- The question stem
- 4 answer options (A–D), correct one highlighted in green
- A "Regenerate this question" button
- An inline edit field for manual tweaks

**Why this design:** Teachers do not trust AI outputs blindly (validated in research). The review-and-edit step is not just a safety net — it's a **trust-building mechanism**. By making it easy to edit, we signal that the teacher is in control. This also reduced the "will it make mistakes?" anxiety surfaced in user interviews.

---

### Screen 5: Export

One-click export options:
- **PDF** — formatted question paper with optional answer key on separate page
- **Copy to clipboard** — for pasting into WhatsApp, email, or Word
- **Google Form** (v2) — auto-populates a multiple choice form

**Why this design:** PDF is the default because it's the format teachers already share. Google Form integration is the highest-requested v2 feature and is on the roadmap.

---

## Design Principles

**1. Teacher confidence over AI autonomy**
Every AI action is reviewable. The product never auto-submits or auto-shares. The teacher is the final decision-maker at every step.

**2. Syllabus as the source of truth**
MCQs are generated only from what's in the uploaded document. The product explicitly tells users: "Questions are grounded in your syllabus." This is a key differentiator from generic AI MCQ tools.

**3. Speed as the primary UX metric**
The end-to-end flow — upload to exported quiz — should complete in under 5 minutes for a standard syllabus. Every design decision was evaluated against this target.

**4. Mobile-first layout with desktop optimisation**
Teachers are often on mobile when reviewing materials. The upload + generate flow works on a phone. The review and edit screen is optimised for desktop where precision is needed.

---

## What Was Left Out of v1 (and Why)

| Feature | Reason Deferred |
|---|---|
| Student-facing practice mode | Would double the UX surface area; teacher workflow is the core hypothesis to validate first |
| LMS integrations (Moodle, Canvas) | API complexity and institutional procurement cycles — a v2+ bet |
| Question type variety (fill-in-the-blank, match the pairs) | MCQs are 80% of teacher demand; broader types risk diluting quality |
| Real-time collaboration | Nice-to-have; teachers work solo on assessments in practice |
| Performance analytics | Requires student data; only possible after student mode is shipped |

Every deferred feature has a clear unlock condition tied to either usage milestones or the v2 roadmap.
