# Product Decisions — MorphEd

A log of key product decisions made during the design and build of MorphEd. Each decision captures the context, the options considered, the choice made, and what we learned.

---

## Decision 1: RAG vs. Fine-Tuning for MCQ Generation

**Context:** The fundamental architectural question for MorphEd was how to make the LLM generate questions grounded in a specific syllabus rather than general world knowledge. Two approaches were viable.

**Options Considered:**

| Dimension | RAG | Fine-Tuning |
|---|---|---|
| Syllabus specificity | High — retrieves directly from the document | Medium — model learns patterns, not specific content |
| Cost | Low inference cost; one-time embedding cost | High upfront training cost; retraining per syllabus update |
| Latency | Adds retrieval step (~200–400ms) | No retrieval overhead; faster generation |
| Maintainability | Update the vector store, not the model | Re-fine-tune when syllabus changes |
| Speed to ship | Fast — no training pipeline needed | Slow — data collection, training, evaluation cycles |
| Hallucination risk | Lower (grounded in retrieved context) | Higher (model can confuse training examples) |

**Choice Made:** RAG

**Rationale:** Fine-tuning would require high-quality (question, syllabus-chunk, answer) training pairs at scale — data we don't have in v1. More importantly, syllabi change every year. Fine-tuning requires retraining per update, while RAG requires only re-embedding. For a solo founder optimising for speed and maintainability, RAG is the clear winner.

**Outcome:** RAG delivered usable MCQ quality in prototyping within 2 weeks. The retrieval layer also unlocked the topic hierarchy feature — we could filter retrieval by topic_id, which became a product differentiator. Fine-tuning remains a consideration for v3 if we collect enough teacher-validated MCQ data to build a proprietary training set.

---

## Decision 2: On-Demand Generation vs. Pre-Generated Question Banks

**Context:** Should MCQs be generated in real-time when the teacher requests them, or should we pre-generate and cache a question bank for each syllabus during upload?

**Options Considered:**

**On-Demand Generation**
- MCQs generated fresh per teacher request
- Higher LLM API cost per session
- Always uses latest model capabilities
- No storage overhead for pre-generated content
- Natural "regenerate this question" UX

**Pre-Generated Bank**
- Generate 50–100 questions per topic at upload time
- Lower per-session cost (one-time generation)
- Questions can go stale if model improves
- Requires storing and indexing large question banks
- Harder to personalise (teacher can't steer generation)

**Choice Made:** On-Demand Generation

**Rationale:** Pre-generation optimises for a use case we don't have yet (high-frequency repeat access to the same bank). In v1, most teachers will upload a syllabus and generate once or twice per unit. More importantly, on-demand generation enables the most valuable UX feature: regenerating individual questions and iterating on the output. This feel of "AI as a collaborative partner" is core to teacher trust.

**Outcome:** On-demand generation worked well for v1 scale. At higher usage, per-session LLM cost becomes a concern. The hybrid approach — cache commonly-requested topic combinations while keeping on-demand for novel requests — is the v2 cost optimisation plan.

---

## Decision 3: B2C (Students) vs. B2B (Institutions) Go-to-Market

**Context:** Who is the primary customer? There were two viable go-to-market paths.

**Options Considered:**

**B2C — Sell to Students**
- Direct consumer: student pays ₹199–499/month
- Large addressable market (350M+ students)
- High CAC via paid acquisition or organic content
- Short sales cycle
- Product needs to serve students directly (practice mode, adaptive learning)
- Risk: price-sensitive market; high churn

**B2B — Sell to Institutions (colleges, coaching institutes)**
- Buyer: HOD or institute director
- Smaller addressable market but higher ACV (₹20,000–2,00,000/year per institution)
- Long sales cycle (3–6 months for colleges)
- Product needs to serve teachers (assessment tool), not students
- Risk: procurement bureaucracy, slow decision-making

**Choice Made:** B2B-light (bottom-up via teachers, top-down institutional conversion)

**Rationale:** A pure B2C play requires student-facing features (practice mode, performance tracking) that are out of scope for v1. A pure top-down B2B play requires a long sales cycle we can't sustain without funding. The hybrid — start by making individual teachers advocates, then convert at the institutional level — mirrors the PLG motions of tools like Notion and Figma.

**The bet:** If a teacher at a college adopts MorphEd and uses it for 3+ months, they become an internal champion. When we approach the institution for a seat license, the teacher can demonstrate value to the HOD directly.

**Outcome:** Two teachers from the same college in our pilot cohort independently recommended MorphEd to their department. This bottom-up champion pattern confirmed the GTM hypothesis and shaped the referral mechanic in our campus adoption playbook.

---

## Decision 4: Multimodal Syllabus Support (Text + Image) vs. Text-Only in v1

**Context:** Many Indian university syllabi are scanned PDFs — not text-layer documents. Should v1 support image-based PDFs (requiring OCR), or restrict to text-layer PDFs only?

**Options Considered:**

**Text-only (no OCR)**
- Faster to ship
- Higher quality for supported documents
- Clear user communication: "Works best with text-layer PDFs"
- Excludes a significant portion of Indian university syllabi

**Text + OCR (multimodal)**
- Broader syllabus support from day one
- OCR adds latency and cost
- Quality depends heavily on scan resolution
- Adds complexity to the ingestion pipeline

**Choice Made:** Text + basic OCR, with quality flagging

**Rationale:** In testing, approximately 40% of syllabi shared by pilot teachers were scanned PDFs. Excluding them would have blocked nearly half the initial user cohort. We implemented basic OCR with a confidence score — if OCR confidence is below a threshold, we show a warning: "This syllabus appears to be a scanned document. Review the extracted topics carefully."

**Outcome:** The warning system worked. Teachers in low-confidence cases spent an extra 30 seconds reviewing the topic hierarchy before generating questions. No teacher reported incorrect MCQs attributed to OCR failure in the pilot. Advanced OCR pre-processing (image denoising, deskewing) is queued for v2.

---

## Decision 5: Login Required from Session 1 vs. Try-Before-Register

**Context:** Should teachers be required to create an account before they can upload a syllabus and generate MCQs?

**Options Considered:**

**Login Required (Immediate)**
- Better data capture from the start
- Enables saved syllabi and quiz history
- Adds friction to the first session
- Risk: significant drop-off before the teacher experiences the product

**Try-Before-Register**
- No account required for first session (upload → generate → export)
- Registration prompted only after first successful quiz export
- Reduces initial friction
- Risk: harder to retarget anonymous users

**Choice Made:** Try-Before-Register

**Rationale:** Teacher scepticism was the #1 risk in adoption. Making them register before experiencing the product's core value (generating good MCQs from their syllabus) adds a gate before the aha moment. By letting them experience the product first, we maximise the chance that registration happens in a moment of demonstrated value: "This actually works, I'll save my account."

**Outcome:** In the pilot, 73% of teachers who completed a quiz export went on to register. Among teachers who hit the registration gate before generating any questions, the conversion rate was 31%. Try-before-register yielded a 2.4x improvement in registration rate — validating the hypothesis.
