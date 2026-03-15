# Decision Logs

A running record of significant product decisions made while building MorphEd. Each log captures the context, the options considered, the choice made, the rationale, and — where enough time has passed — the outcome and reflection.

The purpose of this log is not to document every decision, but to document the non-obvious ones: where the right answer wasn't clear, where we had to make trade-offs between competing goods, and where the outcome either validated or challenged our reasoning.

---

## Log Index

| # | Title | Decision | Date | Status |
|---|---|---|---|---|
| 001 | [RAG vs. Fine-Tuning](./001-rag-vs-finetuning.md) | Chose RAG architecture over model fine-tuning for MCQ generation | Jan 2025 | Outcome confirmed ✅ |
| 002 | [B2C vs. B2B GTM](./002-b2c-vs-b2b-gtm.md) | Chose B2B-light (bottom-up teacher adoption → institutional conversion) over pure B2C or top-down B2B | Feb 2025 | In progress 🔄 |

---

## How to Read These Logs

Each decision log follows this format:

- **Date** — When the decision was made
- **Context** — What situation or question forced a decision
- **Decision** — The specific choice made, stated as a clear yes/no or A vs. B
- **Options Considered** — What alternatives were evaluated
- **Choice** — Which option was selected
- **Rationale** — Why this option was chosen over the alternatives
- **Outcome / Reflection** — What happened as a result (added retrospectively)

These logs are written honestly — including cases where the initial reasoning turned out to be wrong, or where an option we rejected later looked more attractive than it did at the time.

---

## Why Bother with Decision Logs?

Three reasons:

**1. Compounding institutional memory.** Product decisions get revisited. When a new team member or investor asks "why did you choose RAG over fine-tuning?", having a written record is faster and more accurate than reconstructing reasoning from memory.

**2. Better retrospection.** Writing down why you made a decision before you know the outcome forces you to be honest about your reasoning. Looking back at outcome vs. stated rationale is one of the most useful forms of PM self-improvement.

**3. Portfolio signal.** For anyone reviewing this portfolio: these logs show how I think through trade-offs — not just what I built, but why I made the choices I did.
