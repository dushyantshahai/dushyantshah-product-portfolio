# Activation Playbook: First-Session Experience for AI EdTech

*How to design the onboarding flow for MorphEd to maximise the percentage of new users who reach the aha moment — and what that moment actually is.*

---

## What is Activation?

Activation is the moment a new user first experiences the core value of your product. It's not signup. It's not watching a tutorial. It's the moment they think: *"Oh. This actually works."*

For MorphEd, the aha moment differs by role — and each role has a distinct first-session goal:

> **Admin:** Institute is set up, the first book is uploaded, the TOC is parsed, and chapters/topics are visible and ready — without IT support.
>
> **Professor:** A 10-question assessment is generated from a specific chapter of the institute's textbook, reviewed, and published to a batch — in under 2 minutes.
>
> **Student:** An assessment is submitted and an instant score with per-question correctness breakdown appears — with zero wait for manual grading.

Every onboarding decision should be evaluated against one question: **does this get each role to their moment faster?**

---

## Defining the Aha Moment

The professor's aha moment is the most commercially critical — it is the signal that drives institute renewal and word-of-mouth.

**Qualitative signal:** Professors who described the product positively in early testing almost universally referenced the same moment: *"The questions were actually from the chapter I selected — not random AI output."* The RAG grounding is the magic. The onboarding flow must get professors to witness that specific moment.

---

## The Activation Funnel (v1 Baseline)

```
Admin registers institute and logs in
          ↓
Content Library: first subject created + book uploaded
          ↓
Admin reviews and approves chapter/topic structure
          ← ADMIN ACTIVATED

Professor logs in for the first time
          ↓
Step 1: Basic Details completed (title, batch, difficulty)
          ↓
Step 2: Content Selection — chapter/topic selected
          ↓
Step 3: MCQs generated and displayed
          ↓
Step 4: Assessment published to batch
          ← PROFESSOR ACTIVATED

Student logs in and sees assigned assessment
          ↓
Assessment attempted and submitted
          ← STUDENT ACTIVATED
```

The most critical step for professors is Step 2 — content selection. Professors unfamiliar with the subject-book-chapter hierarchy need clear guidance to navigate to the right content without stalling.

---

## Onboarding Flow Design

### Principle 1: Remove Every Unnecessary Step Before the Aha Moment

The standard instinct is to educate users before letting them use the product: show a tour, explain the features, set expectations. For MorphEd, this is wrong.

Professors don't need to understand how RAG works. They need to select a chapter and see syllabus-grounded MCQs. Every screen between "first login" and "questions generated" is a potential exit point.

**v2 change planned:** A lightweight 3-step "How MorphEd works" intro modal will be shown to each user persona on first login — tailored to their role. Admin sees a setup overview (content library → professors → students → batches), Professor sees the 4-step assessment wizard at a glance, and Student sees how to find and attempt an assigned assessment. The goal is to orient each role in under 30 seconds so they arrive at their first action with context, not confusion.

---

### Principle 2: Make Content Selection Feel Familiar, Not Technical

The content selection step (Step 2) is designed intentionally to help professors with cascading dropdowns (Subject → Book → Chapter → Topic → Sub-topic) because the hierarchy feels familiar. It mirrors how professors mentally organise their curriculum — by subject, then book, then the chapter they just finished teaching.

Three supporting design decisions reinforce this:
- A breadcrumb trail that updates dynamically as each selection is made, showing the full path chosen so far
- Helper text: *"Select the chapter you most recently finished teaching"* — anchors the choice to something concrete
- A reversibility signal: *"You can create multiple assessments from different chapters anytime"* — removes the pressure of getting it right the first time

---

### Principle 3: Pre-Populate the First Assessment with Sensible Defaults

Rather than showing an empty Step 1 form, pre-fill Time Limit (60 minutes) and Difficulty (Medium) as defaults. Show the professor what a reasonable starting configuration looks like so they only need to add the title and select the batch.

The fewer decisions required before reaching Step 3 (Generate), the higher the completion rate. Pre-populated defaults reduce the decision load at the top of the funnel, keeping momentum towards the aha moment.

---

### Principle 4: The Generated Questions Screen is a Trust Moment — Design for It

When MCQs first appear, the screen is doing heavy lifting. The professor is evaluating the AI for the first time. Every design decision affects whether they publish or abandon.

Design choices that improve first-generation screen engagement:
- Show the first 3 questions immediately with a "Load more" — avoids overwhelming on first view
- Highlight the content source on Question 1: *"Generated from [Chapter Title] — [Book Name]"* — makes the RAG grounding explicit
- Make Edit the most prominent per-question action — signals the professor is in control, not the AI
- Surface a *"Looks good? Publish to [Batch Name] →"* CTA after the professor scrolls past Question 3 — catches at peak positive engagement

---

## Time-to-Value Optimisation

**Target:** A professor with batch and content library already configured should reach the aha moment (first assessment published) in under 2 minutes.

| Step | Current Median Time | Target |
|---|---|---|
| Login → Create Assessment landed | 18 seconds | 12 seconds |
| Step 1: Basic Details | 45 seconds | 35 seconds |
| Step 2: Content Selection | 58 seconds | 40 seconds |
| Step 3: Generate → MCQs displayed | 24 seconds (API call) | 20 seconds |
| Step 4: Review → Publish | 32 seconds | 25 seconds |
| **Total** | **2 min 57 sec** | **2 min 12 sec** |

The biggest win is Step 2 — smart defaults and breadcrumb clarity get professors through content selection without stalling, without sacrificing the precision of RAG retrieval.

---

## Activation Metrics Dashboard

Activation metrics are designed to be direct leading indicators of the NSM (Assessments Completed per Day) and its L1/L2 inputs. Track these weekly to monitor onboarding health:

| Metric | Maps To | Formula | Target |
|---|---|---|---|
| Professor activation rate | L2-3: Unique Professors Generating / Day | Professors who publish ≥1 assessment / All professors who log in | ≥ 60% |
| Time-to-first-assessment | L2-1: Assessments Generated / Day | Median minutes from Step 1 start to first assessment published | ≤ 2 min |
| Generation → Publish rate | L2-2: Gen → Pub Rate | Assessments published / Assessments generated | ≥ 80% |
| Admin activation rate | Prerequisite for all L2 supply metrics | Admins who upload ≥1 book with parsed TOC / All admins who log in | ≥ 75% |
| Student start rate | L2-4: Assessment Start Rate | Students who attempt assessment / Students assigned one | ≥ 90% |
| Start → Submit rate | L2-5: Start → Submit Rate | Assessments submitted / Assessments started | ≥ 95% |

---

## The Onboarding Email Sequence (For Registered Professors)

Not all activation happens in the first session. Professors who register but don't complete the flow need a re-engagement nudge.

**Email 1 — "Your content library is ready" (sent 30 min after login with no assessment created):**
Subject: "Your institute's content is ready — create your first assessment"
Body: One link directly to the Create Assessment wizard. No friction. No re-explaining the product.

**Email 2 — "See what other professors are doing" (Day 3 if no activation):**
Subject: "Professors like you are creating assessments in under 2 minutes"
Body: Anonymous social proof (assessments created, time saved) + a specific CTA: "Create a 10-question quiz from any chapter — takes 2 minutes."

**Email 3 — "Quick question" (Day 7 if still not activated):**
Subject: "Did something go wrong?"
Body: Short personal message from "Dushyant at MorphEd" — genuine, not templated. Asks if the content library setup worked or if they hit a problem navigating the wizard. Highest reply rate of any email in the sequence.
