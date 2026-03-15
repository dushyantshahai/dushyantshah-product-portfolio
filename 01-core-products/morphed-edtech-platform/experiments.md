# Experiments — MorphEd

A record of key experiments run during the design, build, and early validation of MorphEd. Each experiment is structured as: Hypothesis → What was tested → Result → Learning.

---

## Experiment 1: Prompt Engineering — Instruction-Following for MCQ Quality

**Context:** The first version of the MCQ generation prompt produced technically valid JSON but resulted in questions that were often too easy, had obvious distractors, or rephrased the source text verbatim as the question stem.

**Hypothesis:**
If we add explicit distractor quality instructions and chain-of-thought prompting to the generation prompt, MCQ quality (as rated by teachers) will improve from ~3.2/5 to above 4.0/5.

**What Was Tested:**
Three prompt variants were run against the same 5 syllabus chunks, each generating 20 MCQs:

- **Variant A (baseline):** "Generate 5 MCQs from the following context."
- **Variant B:** Added explicit distractor rules: "Each wrong option should be plausible but clearly incorrect upon careful reading. Avoid trivially wrong options."
- **Variant C:** Added chain-of-thought + persona: "You are an experienced examiner designing questions for a university-level assessment. First, identify the key testable concepts in the passage. Then, for each concept, write a question that tests understanding, not just recall. Generate plausible distractors that reflect common misconceptions."

5 teachers rated 20 MCQs from each variant (blind — they didn't know which prompt generated which set). Rated on: accuracy, relevance, distractor quality, difficulty calibration.

**Result:**

| Prompt Variant | Avg Quality Score (out of 5) |
|---|---|
| Variant A (baseline) | 3.1 |
| Variant B (distractor rules) | 3.7 |
| Variant C (CoT + examiner persona) | 4.3 |

Variant C also produced 30% fewer "verbatim questions" (questions that copy-paste the source text as the stem).

**Learning:**
Prompt engineering has disproportionate ROI before any model fine-tuning. The examiner persona + chain-of-thought framing shifted the model's output from information retrieval to reasoning-based generation. Variant C prompt style became the production default. This experiment also revealed that teachers care most about distractor quality — it's the hardest part of MCQ writing and the clearest signal of AI value-add.

---

## Experiment 2: UX Flow — Topic Selection Before vs. After Viewing Syllabus Summary

**Context:** In the first prototype, teachers were taken directly to a topic selection screen after upload without seeing a summary of what was extracted. Several teachers in usability testing seemed uncertain about whether the extraction was correct.

**Hypothesis:**
If teachers see a brief "extraction summary" screen (number of units detected, sample topics found) before the topic selection screen, their confidence in the product will increase and time-to-first-generation will decrease (less second-guessing).

**What Was Tested:**
A/B test on 18 pilot teachers (9 per variant):

- **Control (Flow A):** Upload → Topic Selection directly
- **Test (Flow B):** Upload → Extraction Summary ("We found 4 units, 14 chapters, and 62 topics in your syllabus. Here's a preview...") → Topic Selection

Measured: time-to-first-generation, number of topic hierarchy edits made, and a post-session confidence rating.

**Result:**

| Metric | Flow A (Control) | Flow B (With Summary) |
|---|---|---|
| Avg time to first MCQ generation | 4 min 12 sec | 3 min 38 sec |
| Teachers who edited the hierarchy | 6/9 (67%) | 3/9 (33%) |
| Post-session confidence rating | 3.6/5 | 4.4/5 |

Teachers in Flow B were faster and more confident, and half as likely to manually edit the hierarchy.

**Learning:**
The summary screen built trust by making the AI's "work" visible before asking the teacher to act on it. Teachers who saw what was extracted didn't feel the need to verify every topic manually. This is a broader UX principle for AI products: **show the reasoning, not just the result.** The extraction summary became a permanent part of the onboarding flow.

---

## Experiment 3: Retrieval Parameter Tuning — Optimal k for Context Window

**Context:** The retrieval layer fetches the top-k most relevant chunks from the vector store before passing them to the LLM for generation. The right value of k affects MCQ quality and cost.

**Hypothesis:**
There is an optimal k value (number of retrieved chunks) beyond which additional context degrades generation quality due to noise rather than improving it.

**What Was Tested:**
Generated 30 MCQs per k value (k = 5, 8, 10, 12, 15) from the same topic across 3 different syllabi. Evaluated on: question accuracy (verified against source text), topic relevance, and hallucination rate (questions referencing content not in the syllabus).

**Result:**

| k value | Accuracy | Topic Relevance | Hallucination Rate |
|---|---|---|---|
| k = 5 | 76% | 82% | 4% |
| k = 8 | 88% | 91% | 3% |
| k = 10 | 90% | 92% | 3% |
| k = 12 | 89% | 90% | 5% |
| k = 15 | 85% | 87% | 9% |

k = 10 produced the best balance of accuracy and relevance. At k = 15, hallucination rate jumped to 9% — the model was pulling in context from tangentially related chunks and generating off-topic questions.

**Learning:**
More context is not always better. At high k values, the context window gets cluttered with marginally relevant chunks, and the model starts generating questions from those peripheral chunks. The sweet spot (k = 8–10) provides enough grounding without introducing noise. This experiment also confirmed that metadata filtering (restricting retrieval to the teacher-selected topic_ids) is essential — without it, k = 10 would include cross-topic contamination.

---

## Experiment 4: User Feedback Loop — Inline Ratings vs. Post-Session Survey

**Context:** We needed a mechanism to collect teacher feedback on MCQ quality to improve the prompt and retrieval pipeline over time. Two options were considered.

**Hypothesis:**
If we collect inline micro-feedback (thumbs up/down on individual MCQs) rather than a post-session survey, we'll get higher feedback volume and more actionable, question-level signal.

**What Was Tested:**
- **Group A (10 teachers):** Post-session survey after every quiz generation: "How would you rate the overall quality of this quiz? (1–5)" with an optional text field.
- **Group B (10 teachers):** Thumbs up/down on each MCQ card, with a single-click "flag this question" option. No survey.

Measured over 4 weeks: feedback response rate, number of data points collected, and qualitative usefulness.

**Result:**

| Metric | Post-Session Survey | Inline Micro-Feedback |
|---|---|---|
| Feedback response rate | 34% | 71% |
| Data points per teacher per week | 1.2 | 14.6 |
| Actionable signal (specific question) | Low | High |

**Learning:**
Inline micro-feedback vastly outperforms surveys in both volume and specificity. More importantly, question-level thumbs down data became a direct training signal — we could identify specific question types, topics, or prompt patterns that consistently produced low-quality outputs. The post-session survey was retired after this experiment. The flagged questions feed into a weekly review queue that informs prompt iteration cycles.

---

## Meta-Learning Across Experiments

A consistent pattern emerged: **teachers' biggest barrier is trust in AI accuracy, not usability.** Every experiment that addressed trust (showing extraction reasoning, making edits easy, surfacing confidence scores) produced outsized UX improvements. For AI products in high-stakes domains like education, trust UX is product design.
