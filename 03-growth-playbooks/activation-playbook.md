# Activation Playbook: First-Session Experience for AI EdTech

*How to design the onboarding flow for MorphEd to maximise the percentage of new users who reach the aha moment — and what that moment actually is.*

---

## What is Activation?

Activation is the moment a new user first experiences the core value of your product. It's not signup. It's not watching a tutorial. It's the moment they think: *"Oh. This actually works."*

For MorphEd, that moment has a specific definition:

> **The MorphEd aha moment:** A teacher uploads their syllabus, selects a chapter, and sees 10 well-formed, syllabus-specific MCQs ready to review — in under 3 minutes.

Every onboarding decision should be evaluated against one question: **does this get a new teacher to that moment faster?**

---

## Defining the Aha Moment

We identified the aha moment through two signals from early users:

**Behavioural signal:** In session recordings, teachers who reviewed at least 8 MCQs and made at least 1 edit or export in their first session had a 68% Day-7 retention rate. Teachers who didn't reach this behaviour had a 21% Day-7 retention rate.

**Qualitative signal:** Teachers in exit interviews who described the product positively almost universally referenced a specific moment: "When I saw the questions — they were actually about what I had taught. Not generic stuff."

This is the aha moment: not just "MCQs were generated" but "they came from MY syllabus." The grounding is the magic. The onboarding flow must get teachers to witness that specific magic.

---

## The Activation Funnel (v1 Baseline)

```
New teacher visits MorphEd landing page
          ↓ 91% proceed
Upload syllabus screen
          ↓ 74% upload a file
Extraction complete, topic hierarchy displayed
          ↓ 68% select at least one topic
MCQ configuration screen
          ↓ 84% submit generation request
First MCQ batch displayed
          ↓ 61% review at least 8 MCQs
Quiz exported OR at least 3 edits made
          ← ACTIVATED (42% of all users who started)
```

The biggest drop: 74% upload a file, but only 68% select a topic after seeing the hierarchy. This is where the "extraction review paralysis" problem manifests — teachers aren't sure if the hierarchy is correct and stall.

---

## Onboarding Flow Design

### Principle 1: Remove Every Unnecessary Step Before the Aha Moment

The standard instinct is to educate users before letting them use the product: show a tour, explain the features, set expectations. For MorphEd, this is wrong.

Teachers don't need to understand how RAG works. They don't need a feature tour. They need to upload their syllabus and see good MCQs. Every screen between "sign up" and "MCQs generated" is a potential exit point.

**v1 change made:** Removed the 3-step "How MorphEd works" intro modal. It reduced time-to-first-MCQ by 47 seconds and improved first-session activation by 11 percentage points.

---

### Principle 2: Give Teachers a "Safety Net" in the Hierarchy Review Step

The hierarchy review screen (the biggest drop-off point) needs to reassure teachers that:
1. The extraction is trustworthy ("We found 4 units and 62 topics in your syllabus")
2. They can edit anything ("Tap any topic to rename or remove it")
3. Moving forward is safe ("You can regenerate from different topics anytime")

The specific copy matters. "You can regenerate from different topics anytime" is a reversibility signal — it removes the fear that selecting the "wrong" topic will waste the first experience. This copy addition improved the hierarchy → topic selection conversion from 68% to 79%.

---

### Principle 3: Pre-Select a Smart Default

Rather than showing an empty topic selector, pre-select the first chapter of the first unit. Show the teacher what a good configuration looks like: 10 questions, Medium difficulty, Chapter 1.

Include a one-line prompt: "Start here to see MorphEd in action — you can change this to any topic you've taught."

This is borrowed from good onboarding practice: show the user the expected path, let them deviate if they want. The pre-selection reduced configuration abandonment by 23%.

---

### Principle 4: The First MCQ Screen is a Trust Moment — Design for It

When MCQs first appear, the screen is doing heavy lifting. The teacher is evaluating the AI for the first time. Every design decision on this screen affects whether they stay or bounce.

Design choices that improved first-MCQ-screen engagement:
- Show 5 MCQs immediately (not all 10) with a "See 5 more" button — prevents overwhelming
- Highlight the "from your syllabus" provenance for the first MCQ: a subtle callout on card 1 that says "Based on [Chapter Title] from your uploaded syllabus"
- Make the edit button the most prominent action on each card — signals that the teacher is in control
- Show a "Looks good? Export your quiz →" CTA after the user has scrolled past MCQ 3 — catches the teacher at peak positive engagement

---

## Time-to-Value Optimisation

**Target:** A teacher who arrives with a syllabus PDF ready should reach the aha moment (10 MCQs displayed from their selected topic) in under 3 minutes.

| Step | Current Median Time | Target |
|---|---|---|
| Landing page → Upload | 48 seconds | 30 seconds |
| Upload → Hierarchy displayed | 55 seconds (processing) | 45 seconds |
| Hierarchy review → Generation submitted | 68 seconds | 50 seconds |
| Generation → First MCQs displayed | 22 seconds (API call) | 20 seconds |
| **Total** | **3 min 13 sec** | **2 min 45 sec** |

The biggest win is in the hierarchy review step — defaulting to a pre-selected topic and adding reassurance copy gets teachers through this step faster without reducing trust.

---

## Activation Metrics Dashboard

Track these metrics weekly to monitor onboarding health:

| Metric | Formula | Target |
|---|---|---|
| Activation rate | (Teachers who export OR make 3 edits) / (All teachers who upload) | ≥ 55% |
| Time-to-first-MCQ | Median minutes from upload to MCQ display | ≤ 3 min |
| Hierarchy conversion | (Teachers who select topic) / (Teachers who see hierarchy) | ≥ 80% |
| First-session export rate | (Teachers who export in session 1) / (All activated teachers) | ≥ 65% |
| Day-7 retention (activated vs. not) | Weekly cohort comparison | ≥ 60% for activated |

---

## The Onboarding Email Sequence (For Registered Teachers)

Not all activation happens in the first session. Teachers who register but don't complete the flow (e.g., uploaded a syllabus but didn't generate) need a re-engagement nudge.

**Email 1 — "Your syllabus is ready" (sent 30 min after upload with no generation):**
Subject: "Your [Subject] syllabus is ready — generate your first quiz"
Body: One link back to the topic selector. No friction. No re-explaining the product.

**Email 2 — "See what other teachers are generating" (Day 3 if no activation):**
Subject: "Teachers like you are saving 2+ hours per exam"
Body: Anonymous social proof (quiz volume, time saved) + a specific call to action: "Generate MCQs for Chapter 1 of your syllabus — takes 2 minutes."

**Email 3 — "Quick question" (Day 7 if still not activated):**
Subject: "Did something go wrong?"
Body: Short personal message from "Dushyant at MorphEd" — genuine, not templated. Asks if the syllabus upload worked or if they hit a problem. Highest reply rate of any email in the sequence.
