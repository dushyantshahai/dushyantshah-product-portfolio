# Problem Statement — MorphEd

## The Core Problem

Teachers in India spend an average of **45–90 minutes per assessment** crafting Multiple Choice Questions (MCQs) manually. They cross-reference textbooks, rephrase questions from previous years' papers, and attempt to calibrate difficulty — all without tooling support. The result is slow, inconsistent, and disconnected from students' actual learning context.

On the student side, the MCQ practice material that does exist is overwhelmingly generic. It's sourced from coaching aggregators, past-paper dumps, or generic test platforms — none of it grounded in what a specific teacher taught, from a specific syllabus, in a specific week.

This gap between **what teachers intend to assess** and **what students actually get to practise** is the central problem MorphEd was built to solve.

---

## Jobs-to-Be-Done (JTBD) Framing

### Teacher JTBD
> *"When I need to create a unit test or weekly quiz, I want to generate accurate, syllabus-specific MCQs quickly, so I can spend more time on teaching and less time on administrative prep."*

The functional job is speed and accuracy. The emotional job is confidence — teachers want to know the questions are pedagogically sound, not cherry-picked from a random bank. The social job is credibility: assessments reflect on them professionally.

### Student JTBD
> *"When I'm preparing for my board exam or internal assessment, I want to practise questions that match exactly what my teacher covered, so I don't waste time on out-of-scope content."*

The functional job is exam readiness. The emotional job is reducing anxiety — targeted practice feels less overwhelming than open-ended study. The social job is competitive performance relative to peers.

---

## Market Context — Indian EdTech & Exam Culture

India has one of the world's largest and most assessment-heavy education systems:

- **350M+ students** across K-12 and higher education
- Board examinations (CBSE, ICSE, state boards) drive high-stakes academic pressure from Class 9 onwards
- Entrance exams — JEE, NEET, CUET, CA Foundation — create a ₹58,000+ crore coaching industry
- Teachers at colleges and coaching institutes create **4–8 assessments per subject per term**
- The shift to blended and hybrid learning post-COVID has increased demand for digital assessment tools, yet most teachers still use MS Word or handwritten question papers

Existing tools like Google Forms or Quizizz offer generic question banks but no syllabus-grounding. Platforms like Embibe or BYJU's serve students directly but don't empower teachers.

**The whitespace:** a tool that puts the teacher in control, starts from *their* syllabus, and generates contextually relevant MCQs at the click of a button.

---

## The Insight That Led to a RAG-Based Solution

The early hypothesis was simple: "Can an LLM generate good MCQs if we give it a topic?" Testing quickly revealed the problem — GPT-4, Claude, and Gemini would generate plausible-sounding MCQs that were subtly wrong, out-of-scope, or ungrounded in the specific content a teacher had taught.

The breakthrough insight came from a teacher interview:

> *"The question isn't just about the topic. It's about the exact definition I gave in class, the example I used, the diagram we discussed. Generic AI questions feel foreign to my students."*

This surfaced the need for **context-grounded generation** — not just topic-aware, but source-aware. The MCQs had to come from the actual syllabus document, not from the model's parametric knowledge.

That insight led directly to a Retrieval-Augmented Generation (RAG) architecture: ingest the teacher's syllabus, extract and map the topic hierarchy, retrieve the most relevant chunks at question-generation time, and force the LLM to generate only from what's in the document.

---

## Problem Scope — What MorphEd Does and Doesn't Solve

**In scope (v1):**
- MCQ generation from uploaded syllabus PDFs
- Topic-level granularity for question targeting
- Teacher-facing workflow (upload → select topic → generate → review)

**Out of scope (v1):**
- Adaptive difficulty based on student performance
- Integration with LMS platforms (Google Classroom, Moodle)
- Subjective question types (essays, short answers)
- Student-facing app (practice mode)

The deliberate constraint to v1 scope ensured fast iteration and a clean product hypothesis: **if a teacher can go from syllabus to quiz in under 5 minutes, we've solved the core problem.**
