# Problem Statement — MorphEd

## The Core Problem

MorphEd addresses the structural inefficiencies and data gaps in educational assessment. Three critical bottlenecks define the problem:

**1. The Post-Lecture Feedback Void**
Professors can't assess students after individual lectures — evaluations are pushed to mid-terms or finals. This means:
- Dozens of incremental learning checkpoints are missed every semester
- Knowledge gaps stay hidden until it's too late to address them
- By the time a student's struggle is identified, the curriculum has already moved on

**2. Inefficient & Fragmented Workflows**
Getting an assessment from concept to student is unnecessarily broken:
- Manual creation costs professors hours per week
- Where LLMs are used, they're used in silos — copy-pasted prompts producing de-grounded, inconsistent questions
- No end-to-end pipeline exists from AI generation to student distribution

**3. The Analytics Blind Spot**
Even when assessments are completed, the data is rarely actionable:
- Granular, lecture-level analytics are virtually non-existent
- Data is trapped in paper, spreadsheets, or fragmented systems
- Without structured data, institutes cannot intervene before a student fails

---

## Jobs-to-Be-Done (JTBD) Framing

### Professor JTBD

> *"When I finish a lecture, I want to quickly assess whether my students actually understood it — without spending hours creating and distributing a quiz — so I can identify who needs help before the next class, not after the semester exam."*

- **Functional job:** Generate a grounded, lecture-specific assessment in minutes and get it in front of students the same day
- **Emotional job:** Confidence that the assessment reflects exactly what was taught — not a generic question bank — and that the data coming back is reliable enough to act on
- **Social job:** Be seen as a thorough, data-informed educator who doesn't let students fall behind unnoticed

### Student JTBD

> *"When I finish a lecture, I want to know immediately whether I've actually understood the concepts — so I can address gaps now, not discover them during the exam."*

- **Functional job:** Attempt topic-specific assessments tied to what was just taught, and see where understanding broke down
- **Emotional job:** Reduce exam anxiety by knowing exactly which areas need attention, rather than feeling broadly underprepared
- **Social job:** Keep pace with peers and avoid being the student who gets caught off guard in class or on results day

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
