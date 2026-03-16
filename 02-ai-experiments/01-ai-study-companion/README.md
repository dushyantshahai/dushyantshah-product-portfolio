# AI Study Companion

## Problem

Students preparing for exams — especially Indian college students balancing coursework with competitive exam prep — face a fundamental planning problem: **they don't know what to study next.**

Existing study tools either dump all topics at once (overwhelming) or follow a rigid linear schedule (inflexible). Neither adapts to what the student has already covered, what they're weak on, or how much time they have left before the exam.

The result: students waste time re-reading topics they already know, avoid topics that feel daunting, and head into exams with uneven preparation.

---

## What It Does

The AI Study Companion is a conversational study planner grounded in syllabus context. It uses Claude + RAG to give students a personalised, adaptive recommendation for what to study next — based on their actual syllabus, their stated progress, and their exam timeline.

### Core Interactions

**"What should I study today?"**
The companion asks: how much time do you have, what's your exam date, and which topics have you covered. It then recommends the highest-priority topic from the syllabus with a structured study plan (what to read, what to practise, how long to spend).

**"I just finished Chapter 5 — what's next?"**
The companion acknowledges progress, updates its internal map of the student's coverage, and recommends the next logical topic — considering both syllabus sequence and knowledge dependencies (e.g., don't study integration before derivatives).

**"I'm weak on thermodynamics — help me focus."**
The companion generates a targeted revision plan: key concepts to review, common MCQ patterns for that topic, and a 3-question self-check to test retention before moving on.

**"How much of my syllabus is left?"**
A progress summary: % covered, high-priority remaining topics, and estimated time to complete at current pace.

---

## How It Works

```
Student opens chat interface
          ↓
Companion reads syllabus context from RAG store
(same pipeline as MorphEd — embeddings in Supabase pgvector)
          ↓
Conversation history tracked in session state
          ↓
Each message → Claude processes with:
  - System prompt defining companion persona + constraints
  - Retrieved syllabus chunks relevant to the current topic
  - Conversation history for continuity
          ↓
Response: Study recommendation, plan, or progress summary
```

The companion does not make up content. All study recommendations reference specific sections from the student's uploaded syllabus. If a student asks about a topic not in their syllabus, the companion flags it: "That topic doesn't appear in your uploaded syllabus — do you want to continue anyway?"

---

## Tech Stack

| Layer | Technology |
|---|---|
| LLM | Claude 3.5 Sonnet (via Anthropic API) |
| Retrieval | RAG on student's syllabus (Supabase pgvector) |
| Conversation state | In-memory session state (Redis for v2) |
| Frontend | Simple chat UI built with Next.js |
| Backend | FastAPI, shared with MorphEd backend |
| Auth | Supabase Auth (shared with MorphEd) |

---

## Current Status

**Status: Prototype — functional, not production-ready**

- Core chat loop works end-to-end
- RAG retrieval from syllabus is functional
- Conversation history persists within a session (resets on refresh)
- No persistent progress tracking across sessions yet
- Not yet integrated with MorphEd's quiz flow (student performance data not fed back to companion)

---

## Key Learnings

**1. The hardest part is session memory, not generation quality.**
Claude generates excellent recommendations. The challenge is maintaining continuity across sessions — "what did I study last time?" requires persistent state that the prototype doesn't yet have. This is the top engineering priority for v2.

**2. Students want to be pushed, not just advised.**
Early testers said passive recommendations ("you should study Chapter 6") felt forgettable. Recommendations that included a concrete micro-goal ("spend 25 minutes on Chapter 6 and answer these 3 questions before our next session") had significantly better follow-through.

**3. Syllabus grounding is both a strength and a constraint.**
The grounding prevents hallucination and keeps recommendations on-topic — but it means the companion can't help with topics outside the syllabus. For students using non-standard or custom course modules, this is a feature. For students asking generic subject questions, it's a limitation. A "syllabus mode / general mode" toggle is planned.

**4. The right persona matters.**
Testing two personas — "AI tutor" (authoritative, instructional) vs. "study buddy" (conversational, encouraging) — the study buddy persona produced higher session engagement and more follow-up questions. Students were more likely to admit weakness ("I don't understand this topic") with a peer-like persona than a teacher-like one.

---

## Connection to MorphEd

The Study Companion is designed as the student-facing complement to MorphEd. The intended flow:

1. Teacher creates quiz using MorphEd → shares with class
2. Student completes quiz → weak topics identified
3. Study Companion uses quiz performance data to prioritise what to study next
4. Companion recommends focused MCQ practice on weak topics → student uses MorphEd's student mode

This closes the teacher-student loop and creates a flywheel: better assessments → better feedback → better study recommendations → better outcomes.
