# User Personas — MorphEd

## Research Methodology

User research was conducted across two rounds:

**Round 1 — Discovery (5 interviews):** Semi-structured 30-minute interviews with college and coaching institute teachers in Mumbai and Pune. Goal: understand current assessment creation workflow, pain points, and tool usage.

**Round 2 — Validation (8 interviews + 2 usability sessions):** Tested early prototypes with teachers and ran diary studies with students preparing for semester exams. Supplemented with a 47-response screener survey distributed through college WhatsApp groups.

Key insight from research: **teachers are the primary buyers and the primary blockers.** Students are the beneficiaries but don't control tool adoption. This shaped the v1 go-to-market decision to build teacher-first.

---

## Persona 1 — The Assessment-Burdened Teacher

**Name:** Priya Nair
**Role:** Lecturer, B.Sc. Computer Science, Mumbai University affiliated college
**Age:** 32
**Tech comfort:** Moderate — uses Google Forms, WhatsApp, YouTube. Doesn't code.

### Context
Priya teaches 3 subjects across two batches. Every month she has to set 2 unit tests and 1 internal assessment per subject — that's 9 question papers per month. She spends her weekends on this. She's frustrated, but she doesn't have a better option.

### Jobs-to-Be-Done
- **Functional:** Generate accurate, curriculum-aligned MCQs without spending hours on it
- **Emotional:** Feel confident that the questions are fair and reflect what she actually taught
- **Social:** Be seen as a thorough, professional teacher whose assessments are respected by students and heads of department

### Pain Points
1. **Time sink:** Crafting MCQs manually takes 45–90 minutes per test. Distractors (wrong options) are the hardest part.
2. **Quality inconsistency:** Questions from previous years' papers may be outdated or misaligned with the current syllabus.
3. **Generic AI outputs:** She tried ChatGPT once — "it gave me questions about topics I never taught this semester."
4. **Version management:** Syllabus changes every year; her question bank doesn't update automatically.
5. **No tooling:** Most affordable tools (Google Forms, Kahoot) require manual question entry — they don't generate.

### Behavioural Signals (from interviews)
- Saves old question papers in Google Drive, reuses them with minor edits
- Shares question papers on WhatsApp groups with colleagues for feedback
- Prefers WhatsApp and simple web interfaces over desktop software
- Highly sceptical of AI accuracy — "will it make up answers?"

### What Success Looks Like for Priya
> *"I upload my syllabus on Monday, select the chapter we finished, and have 20 MCQs ready to review in 5 minutes. I tweak 2–3, download the PDF, and I'm done."*

---

## Persona 2 — The Exam-Anxious Student

**Name:** Rohan Mehta
**Role:** B.Com Year 2, preparing for semester exams + CA Foundation
**Age:** 20
**Tech comfort:** High — uses YouTube, Unacademy, Instagram, ChatGPT for study help

### Context
Rohan juggles college coursework with CA Foundation prep. He's constantly trying to optimise his study time. He downloads past-paper PDFs, watches YouTube lectures, and quizzes himself using random apps. But nothing he finds is specific to his college's syllabus — his professor uses a custom module, not the standard textbook.

### Jobs-to-Be-Done
- **Functional:** Practise MCQs that match what his teacher actually covered, not just the broad subject
- **Emotional:** Reduce exam anxiety by feeling "covered" on the right topics
- **Social:** Perform well relative to peers in the class — avoid being caught off-guard by unexpected questions

### Pain Points
1. **Syllabus mismatch:** Generic practice apps cover topics his college skipped, or miss topics unique to his module.
2. **Passive study problem:** Re-reading notes doesn't test retention — he knows he needs active recall but good tools are hard to find.
3. **Fragmented tools:** He uses 4+ apps for different subjects — no single place for syllabus-specific practice.
4. **No feedback loop:** After practising, he doesn't know *why* he got questions wrong — just that he did.

### Behavioural Signals (from research)
- Studies in short bursts (Pomodoro-style) — needs a tool that loads fast and gets him into practice mode immediately
- Shares good study resources with 5–6 friends on WhatsApp — strong word-of-mouth potential
- High willingness to pay for tools that "actually help" (~₹200–500/month range)
- Trusts peer recommendations and YouTube study channels over institutional tools

### What Success Looks Like for Rohan
> *"I open MorphEd, pick Chapter 4 from my actual college syllabus, and get 15 practice MCQs instantly. After I finish, I see which topics I'm weak on. I share it with my study group."*

---

## Relationship Between Personas

MorphEd's v1 flywheel depends on **teachers activating students**, not the reverse:

1. Teacher uploads syllabus → generates quiz → shares with class
2. Students access the quiz → practice → request more
3. Student demand creates pull for teacher adoption

This teacher-first, student-benefit dynamic is analogous to how Google Classroom or Notion Education operates — the educator is the economic buyer and the distribution channel simultaneously.

In v2, the roadmap includes a **student-facing self-study mode** where students can directly access their institution's syllabus (if the teacher has shared it) and practise on demand without teacher initiation.
