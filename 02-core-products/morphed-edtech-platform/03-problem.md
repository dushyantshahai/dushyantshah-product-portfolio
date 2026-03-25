 * Repo: Dushyant Shah Product Portfolio
 * Author: Dushyant Shah
 * GitHub: https://github.com/dushyantshahai
 * This code is part of a public portfolio and is not licensed for reuse.
 * © 2026 Dushyant Shah
---
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

A background in research and consulting across global firms sharpens one instinct above all else: the ability to spot where a process hasn't meaningfully changed, and ask why.

While reading an article on the Indian EdTech landscape, that instinct surfaced a pattern I had lived through firsthand. In high school and college, assessments were entirely manual — professors created tests by hand, printed them, and distributed them in class. By the time I reached my MBA at JBIMS, the format had shifted to LMS platforms, but the underlying effort hadn't. The toil of creating, distributing, and analysing assessments remained largely the same. The wrapper changed; the problem didn't.

That observation led me back to my own professors. I set up conversations, mapped their current workflows, and stress-tested the assumptions. The gaps were consistent: no time for post-lecture checks, no structured data on what students actually missed, and no tool that addressed both ends of the problem simultaneously.

MorphEd emerged from those conversations — not as a feature idea, but as a structural response to a process that had been underserved for decades. My experience in product management, combined with hands-on work in AI tools including Claude, Claude Code, and Antigravity IDE, gave me the foundation to move from insight to a working product.

---

## Problem Scope — What MorphEd Does and Doesn't Solve

**✅ What MorphEd Solves**
- **The Lecture-to-Assessment Gap:** Enables "daily" or "post-lecture" assessments that were previously impossible due to manual overhead
- **Verified RAG Efficiency:** Automates ~80% of drafting using strictly grounded documents, keeping the professor as the "Expert Editor"
- **Granular Mastery Tracking:** Replaces vague monthly averages with specific, lecture-level data on exactly what students missed today
- **Zero-Hallucination Testing:** Ensures all AI output is anchored to provided textbook/PDF source material, not general internet knowledge

**❌ What MorphEd Doesn't Solve (By Design)**
- **Autonomous Teaching:** It serves as an assistant to the professor, not a replacement for classroom instruction or mentorship
- **General Content Authoring:** It focuses strictly on Assessment-as-a-Service, not on creating lecture slides, videos, or full curricula
- **Classroom Management:** It does not handle physical attendance, student behavioural issues, or non-digital classroom logistics
- **Subjective Essay Grading:** The MVP focuses on high-frequency, objective learning signals (MCQs, fillers) rather than qualitative long-form essays
