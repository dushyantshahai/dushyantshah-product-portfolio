 * Repo: Dushyant Shah Product Portfolio
 * Author: Dushyant Shah
 * GitHub: https://github.com/dushyantshahai
 * This code is part of a public portfolio and is not licensed for reuse.
 * © 2026 Dushyant Shah
---
# Product Decisions — MorphEd

A record of the key technical and product choices made while building MorphEd — what was decided, why, and what we were trying to avoid.

---

## 1. User Role Segmentation

**Decision:** Four distinct roles — Admin, Professor, Student, and Founder (Product Owner).

This wasn't over-engineering. It mirrors how educational institutes actually work, and designing around that structure from day one prevented a lot of permission and access headaches later.

- **Admin** handles the infrastructure layer — managing professor and student accounts, uploading books, and setting up batches. They're the operators.
- **Professor** is where value is created — generating assessments, reviewing outputs, and publishing to student groups.
- **Student** is where value is consumed — taking assessments and viewing results.
- **Founder** has a cross-institute view — monitoring platform-wide health, NSM movement, and key metrics across all institutes on the platform.

Separating these roles cleanly meant each user sees exactly what they need, nothing more, and the product doesn't try to serve everyone from a single interface.

---

## 2. RAG Architecture over Direct Generation

**Decision:** Chose a RAG (Retrieval-Augmented Generation) pipeline rather than a simple "upload the book, generate questions" approach.

The simpler route — dump the textbook into a prompt and ask for MCQs — works until it doesn't. Questions start referencing things not in the book, or miss key sections entirely, or produce identical outputs on repeat runs. For an assessment platform, that's a trust problem.

RAG solves this in three ways:

- **One-time ingestion:** A book is uploaded and indexed once. After that, any professor can generate assessments from it as many times as they want without re-uploading or re-processing. The infrastructure does the heavy lifting upfront.
- **Structured TOC:** The ingestion step builds a permanent, navigable table of contents. Professors pick the chapter or topic they want to assess — they're not guessing what the AI will pull.
- **Grounding:** Questions are generated strictly from the retrieved source material. If it's not in the book, it doesn't end up in the question.

---

## 3. MCQ-Only in V1 ("Objective First" Strategy)

**Decision:** Restricted V1 to Multiple Choice Questions only.

There was a temptation to support short-answer questions, fill-in-the-blanks, and essay-style questions from the start. We pushed that out.

The reason is signal quality. MCQs are objective — there's a right answer, it's instantly verifiable, and the resulting data is structured. When a student gets a question wrong, we know exactly which topic, which chapter, and which difficulty level was the problem. That data feeds directly into analytics.

Subjective questions produce richer qualitative insights but introduce grading ambiguity at scale. For V1, where the goal was to prove that assessments could be generated, distributed, and measured end-to-end, MCQs gave the cleanest feedback loop. Subjective types are on the V2 roadmap once the core infrastructure is solid.

---

## 4. Dual-LLM Strategy — Claude for Generation, Gemini for Ingestion

**Decision:** Two different models for two different jobs, rather than forcing one model to do both.

This wasn't a default choice — it was the result of testing both models on both tasks and being honest about where each one wins.

**Claude 3 Haiku for MCQ generation:** Claude is fast, cost-effective, and exceptionally good at following structured output instructions. For a task that runs dozens of times a day — generating JSON arrays of questions with distractors, difficulty tags, and explanations — Haiku hits the right balance of quality and cost without the overhead of a larger model.

**Gemini 1.5/3 Flash for TOC extraction:** This is where Gemini's 1M+ token context window becomes a genuine advantage. Instead of chunking a 400-page textbook and stitching results together, the entire PDF is sent in a single shot. Gemini reads the document visually — picking up on font sizes, bold headings, and layout — and returns a structured hierarchy in one pass. Trying to replicate this with a standard-context model would require a fragile multi-step pipeline. Gemini makes it a single API call.

---

## 5. Passthrough Revenue Model

**Decision:** Pricing structured as a base platform fee plus token cost passed through to the institute.

The alternative — absorbing token costs into a flat subscription — looks simpler on the surface. But as usage scales, LLM inference costs become unpredictable. A heavy-use institute generating hundreds of assessments a month should not be subsidised by a light-use one.

The passthrough model ties cost to consumption. Institutes pay for what they use. It also puts the professor in a position to make conscious choices about volume and difficulty — higher difficulty levels and longer assessments use more tokens. That transparency is useful, not a burden. And for MorphEd, it means margins stay predictable regardless of how the usage distribution shifts over time.

---

## 6. Centralised Authentication with a Global AuthContext

**Decision:** Implemented a global `AuthContext` and `AuthProvider` wrapping the entire application.

In a multi-role SPA (Single Page Application), getting authentication state wrong causes subtle but painful bugs — pages flashing before redirecting, role-specific layouts rendering incorrectly, or users briefly seeing dashboards they shouldn't. These aren't critical failures, but they erode trust in a product quickly.

The global `AuthContext` ensures that from the moment a user logs in or registers, every part of the application — nested routes, layout components, conditional renders — has immediate access to their identity and role. There's no prop drilling, no race condition between route guards and API responses, and no moment where the app is uncertain about who it's serving.

It's the kind of architectural decision that disappears when done right and creates visible problems when skipped.
