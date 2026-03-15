# Decision Log 001: RAG vs. Fine-Tuning for MCQ Generation

**Date:** January 2025
**Product:** MorphEd
**Decision area:** AI architecture
**Status:** Outcome confirmed ✅

---

## Context

MorphEd's core value proposition is syllabus-grounded MCQ generation — questions that come from the teacher's actual uploaded document, not from generic world knowledge. Before writing a single line of code, I had to decide the fundamental architectural question: **how do we make the LLM generate questions that are truly grounded in a specific document?**

Two serious approaches existed: Retrieval-Augmented Generation (RAG) and model fine-tuning. Both were technically feasible. Both had real advocates. I spent about two weeks researching, testing prototypes, and thinking through the trade-offs before committing.

---

## Options Considered

### Option A: RAG (Retrieval-Augmented Generation)

In a RAG architecture, the syllabus is chunked, embedded, and stored in a vector database. At generation time, the most relevant chunks are retrieved and passed as context to the LLM alongside the generation prompt. The model generates MCQs "from" the retrieved context.

**Advantages:**
- No training data required to get started — works immediately with any syllabus
- Highly interpretable — you can see exactly which chunks were retrieved and used
- Syllabus updates are handled by re-embedding the new document, not retraining the model
- Cost-efficient at v1 scale — no training cost, only inference + embedding
- Strong grounding: the model is explicitly told "use only this context"
- Rapid iteration — prompts can be changed without retraining

**Disadvantages:**
- Adds retrieval latency (~200–400ms additional per generation request)
- Quality is bounded by retrieval quality — poor chunking or embeddings = poor MCQs
- Requires careful prompt engineering to ensure the model honours the "use only context" instruction
- Complex context windows if syllabus is long or topic selection is broad

---

### Option B: Fine-Tuning

In a fine-tuned approach, a base LLM (GPT-3.5 or similar) is trained on a dataset of (syllabus chunk, topic, MCQ) examples. The model learns the MCQ generation pattern from training data and doesn't need explicit retrieval at inference time.

**Advantages:**
- Potentially faster inference (no retrieval step)
- Could produce more stylistically consistent MCQs if trained on high-quality examples
- Less prompt engineering required at inference time

**Disadvantages:**
- Requires a high-quality training dataset of (chunk → MCQ) pairs — which I didn't have
- Expensive: fine-tuning costs at v1 scale ($500–2,000+ depending on dataset size and model)
- Slow iteration: changing the model's behaviour requires retraining, not just updating a prompt
- Retraining required when the syllabus changes significantly or when a new subject domain is added
- Hallucination risk: fine-tuned models can still generate plausible-but-wrong questions not grounded in the specific input
- Not interpretable: harder to diagnose why a bad MCQ was generated
- Requires ongoing training data collection — a data flywheel that doesn't exist yet

---

### Option C: Hybrid (RAG + Fine-Tuning)

Use RAG for retrieval-grounded generation, but fine-tune the generation model on high-quality MCQ examples to improve stylistic quality and reduce prompt complexity.

**Assessment:** Technically the strongest long-term approach, but impractical for v1. Requires both a RAG pipeline and a training dataset + fine-tuning infrastructure. Too much complexity for a solo founder at prototype stage. Earmarked as a v3 consideration.

---

## Choice Made

**Option A: RAG**

---

## Rationale

The decision came down to three factors:

**1. Speed to value without a training dataset.**
Fine-tuning requires data I don't have. Collecting high-quality (chunk, MCQ, teacher-validated) pairs at scale takes months. RAG works on day one with any syllabus — no data collection required. Given the goal of getting to a working prototype and in front of real teachers quickly, RAG was the only option that respected timeline constraints.

**2. Maintenance costs at the pace of syllabus change.**
Indian university syllabi change every year. Some teachers update their module mid-semester. In a fine-tuned architecture, each syllabus update is a potential retraining event. In a RAG architecture, it's a re-embedding event — far cheaper, faster, and simpler. The total cost of ownership of RAG over a 2-year product horizon is significantly lower.

**3. Grounding reliability is more important than generation speed.**
The fine-tuned model might be 200ms faster. But it might also generate a convincing-sounding question about a topic the teacher never covered — because the model learned to generate plausible MCQs, not to respect specific source content. For a product whose core value proposition is *source-faithful* generation, the interpretability and explicit grounding of RAG is worth the latency trade-off.

A secondary factor: the RAG architecture unlocked the topic hierarchy feature naturally. By chunking the syllabus and tagging chunks with topic metadata, the same infrastructure that powers generation also powers the topic navigator UI. Fine-tuning would not have produced this architectural bonus.

---

## Outcome / Reflection

*(Written 6 weeks after the decision, after the first pilot cohort)*

RAG was the right choice.

The pilot with 18 teachers produced 0 reported cases of "the AI used information from outside my syllabus" — the hallucination guardrail held. The retrieval quality was good enough that 70%+ of MCQs were rated acceptable without edits by teachers.

The iteration speed advantage was real. Between pilot week 1 and week 4, I made 6 prompt changes and 2 chunking strategy changes. In a fine-tuned architecture, each of those iterations would have required a training run. In RAG, each was a 30-minute code change.

The latency (3.2 seconds median generation time for 10 MCQs) was not a reported user concern. Teachers are used to waiting for documents to process — the generation time felt fast relative to their mental benchmark (writing MCQs manually).

**What I'd change:** I underestimated the importance of the chunking strategy. My initial fixed-size chunking was too naive and produced retrieval that occasionally split concepts mid-explanation. Moving to topic-boundary-aware chunking in week 3 was the highest-leverage technical improvement I made. I should have invested in chunking strategy earlier — it matters more than prompt engineering at the retrieval stage.

**Revised view on fine-tuning:** After 6 weeks of pilot data, I have 300+ teacher-validated MCQ examples. The data flywheel is starting. Fine-tuning a generation model on this data — on top of the RAG pipeline — is now a credible v3 move. The sequence "RAG first, fine-tune later" turned out to be the right temporal ordering, not just the right initial choice.
