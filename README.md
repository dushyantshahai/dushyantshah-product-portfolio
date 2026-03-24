# Dushyant Shah — AI Product Manager

> I build products at the intersection of AI systems and human behaviour.
> Currently shipping **MorphEd** — a RAG-powered EdTech platform for syllabus-grounded MCQ generation.

![Open to Work](https://img.shields.io/badge/Open%20to-AI%20PM%20%7C%20Growth%20PM%20Roles-blue?style=flat-square)
![Domain](https://img.shields.io/badge/Domain-EdTech%20%7C%20B2C%20%7C%20B2B%20SaaS-green?style=flat-square)
![Location](https://img.shields.io/badge/Based%20in-Mumbai%2C%20India-orange?style=flat-square)

📍 Mumbai, India · [LinkedIn](https://www.linkedin.com/in/dushyantshah) · [Email](mailto:dushyantshah.ai@gmail.com)

---

## What I bring

Senior PM with 7+ years of experience and ~2.5 years building AI-native products — currently at Quantiphi Analytics, where I've driven a 40% increase in AI product engagement, launched 3 GenAI agents, and maintained a 60%+ demo-to-pilot conversion rate with CXO audiences.

My toolkit spans both sides of the PM role: on the strategic side — roadmap definition, OKR setting, funnel analysis, A/B testing, and user research. On the technical side — advanced prompt engineering, RAG architecture, LLM evaluation, and hands-on work with Claude and Claude Code.

**MorphEd** is my most complete demonstration of AI PM ownership — an end-to-end B2B SaaS platform built on a RAG pipeline powered by Anthropic Claude, reducing assessment creation time from 2+ hours to under 1 minute for Indian colleges and coaching institutes. Problem discovery, architecture decisions, UX flows, and launch — all mine.

I've built across B2B SaaS, growth consulting, and key accounts — which gives me an unusual ability to connect product decisions to revenue outcomes. MBA from JBIMS. I work best at the intersection of AI systems, growth loops, and cross-functional execution.

---

## 🚀 Core Products

### MorphEd — AI-powered assessment platform

> `RAG` `EdTech` `LLM` `On-demand MCQ generation`

**Problem**: Professors creating MCQ assessments today fall into one of three traps:

- **Manual creation & paper distribution** — time-intensive, requires pre-planning, and pulls professor attention away from teaching and student development
- **Using LLMs directly** — uploading book pages with a generic prompt produces hallucination-prone outputs with inconsistent difficulty levels; multiple review and re-edit cycles nullify the time saved
- **Competitor apps** — generate generic assessments ungrounded in the actual syllabus, still requiring multiple rounds of revision before they're fit to distribute

**What it does**: MorphEd is a truly RAG-based, AI-enabled EdTech platform that ingests an entire textbook, maps it to a 4-level content hierarchy, and empowers professors to generate contextually grounded MCQs on demand — in seconds, with varied difficulty levels and ~95% accuracy. No hard reviews. No re-edits. Beyond assessment creation, MorphEd closes the teaching loop with:

- **Batch-level analytics** — track class-wide performance per assessment
- **Student-level analytics** — identify individual learning gaps
- **Topic-level analytics** — pinpoint weak areas and enable targeted teaching interventions

**My role**: End-to-end — problem discovery, product strategy, RAG architecture decisions, UX flows, and shipping.

**Highlights**
- Multimodal syllabus extraction (PDF, text, structured docs)
- Hierarchical topic selection and curriculum mapping
- Context-grounded MCQ generation with difficulty tuning
- On-demand and batch generation modes

🔗 [Full case study](./02-core-products/morphed-edtech-platform/) · [AI architecture](./02-core-products/morphed-edtech-platform/01-ai-architecture.md)

---

## 🧪 AI Product Experiments

Small prototypes exploring AI product ideas — proof of curiosity and technical fluency.

| Project | What it does |
|---|---|
| [AI Study Companion](./03-ai-experiments/01-ai-study-companion/) | Conversational study planner grounded in syllabus context |
| [AI Syllabus Parser](./03-ai-experiments/02-syllabus-parser/) | Extracts and maps chapter/topic hierarchy from textbook PDFs |
| [ClearSign Prototype Spec](./03-ai-experiments/03-clearsign-prototype-spec/) | AI-powered document signing and verification prototype |

---

## 📈 Growth Playbooks

Frameworks and strategies for EdTech growth — GTM, activation, and retention.

| Playbook | Description |
|---|---|
| [EdTech GTM Strategy](./04-growth-playbooks/03-edtech-gtm-strategy.md) | Go-to-market for institution-facing EdTech products |
| [Campus Adoption Loops](./04-growth-playbooks/02-campus-adoption-loops.md) | Top-down B2B onboarding mechanics for campus rollout |
| [Activation Playbook](./04-growth-playbooks/01-activation-playbook.md) | First-session activation design for AI-powered tools |

---

## 🤖 AI Workflows

Reusable Claude SKILL.md docs and prompt frameworks for product work — built and battle-tested while shipping MorphEd.

| Workflow | What it does |
|---|---|
| [PRD Generator](./05-ai-workflows/skills/prd-generator/SKILL.md) | Generates structured PRDs from a problem brief |
| [Market Research Agent](./05-ai-workflows/skills/market-research/SKILL.md) | Synthesises competitive landscape and user insights |
| [Feature Prioritisation](./05-ai-workflows/skills/feature-prioritisation/SKILL.md) | RICE/ICE scoring with AI-assisted rationale |

---

## 🧑‍💻 Vibe Coding Skills

A production-grade reference library for building AI-powered products — distilled from real shipping experience building MorphEd and other AI products.

> `26 skill documents` · `Production-ready code patterns` · `Security guardrails` · `AGENT_CONTEXT.md system`

26 structured skill documents covering the complete AI product development stack. Each document contains the mental models, step-by-step build instructions, security guardrails, common patterns, and debugging loops for one specific domain — designed to be fed directly to an AI coding agent (Claude Code, Cursor, Codex) at the start of a session.

**What makes it different from generic tutorials:** Every skill ends with a `🗂️ Update Your AGENT_CONTEXT.md` callout — a template of the exact architectural decisions made while implementing that skill. As you work through skills, you populate a single `AGENT_CONTEXT.md` at the root of your project. Every future coding session starts by feeding this file to the agent, giving it full memory of every prior decision. No more re-debating already-resolved architecture.

| Category | Skills Covered |
|---|---|
| **AI-Specific** | RAG (re-ranking, evaluation, HyDE), prompt engineering, agent design, safety guardrails, model selection, single vs. multi-agent |
| **Backend** | Architecture (background jobs, connection pooling), API routes, database & storage, auth & authorization, agentic workflows, API integrations |
| **Frontend** | Architecture, UI components, AI streaming UI, responsive design |
| **DevOps** | Deployment & hosting, environment variables & secrets, monitoring & observability |
| **Testing** | Testing strategy, automated testing, debugging & error handling |
| **Product** | Documentation (AGENT_CONTEXT.md), UX design, analytics & experimentation, growth & distribution |

🔗 [Browse all 26 skills](./07-vibe-coding-skills/) · [Quick start & Claude Project setup](./07-vibe-coding-skills/02-CLAUDE_PROJECT_SETUP.md) · [Standalone repo](https://github.com/dushyantshahai/vibe-coding-skills)

---

## 📋 Decision Logs

Documented trade-offs and choices made while building — the thinking behind the product, not just the output.

→ [View decision logs](./06-decision-logs/)

---

## 📄 Resume

→ [View full resume](./01-resume/)

**Dushyant Shah** — Senior Product Manager with 7+ years of experience, ~2.5 years in AI product management.

| | |
|---|---|
| **Current role** | Senior PM (AI Products) — Quantiphi Analytics |
| **AI project** | MorphEd — 0→1 AI-native B2B SaaS (RAG-powered MCQ assessment platform) |
| **Key metrics** | 40% AI product engagement growth · 60%+ demo-to-pilot conversion · 229% app adoption |
| **Education** | MBA (Marketing) — JBIMS · BMS — St. Andrew's College, Mumbai |
| **Contact** | dushyant8shah@gmail.com · +91-9619369240 |

---

## What this portfolio demonstrates

| Signal | Where to look |
|---|---|
| Product thinking | MorphEd case study |
| AI fluency | RAG architecture + `05-ai-workflows/` |
| Technical depth | `07-vibe-coding-skills/` — 26 production skill docs covering the full AI product stack |
| Experimentation mindset | `06-decision-logs/` + `03-ai-experiments/` |
| Growth orientation | `04-growth-playbooks/` |
| Execution | Live product + shipped prototypes |
| Background | `01-resume/` |

---

## Repo structure

```
dushyantshah-product-portfolio/
│
├── README.md
├── 01-resume/
│   └── README.md
├── 02-core-products/
│   └── morphed-edtech-platform/
│       ├── 00-README.md
│       ├── 01-ai-architecture.md
│       ├── 02-metrics.md
│       ├── 03-problem.md
│       ├── 04-product-decisions.md
│       ├── 05-roadmap.md
│       ├── 06-solution.md
│       └── 07-users.md
├── 03-ai-experiments/
│   ├── 01-ai-study-companion/
│   ├── 02-syllabus-parser/
│   └── 03-clearsign-prototype-spec/
├── 04-growth-playbooks/
│   ├── 01-activation-playbook.md
│   ├── 02-campus-adoption-loops.md
│   └── 03-edtech-gtm-strategy.md
├── 05-ai-workflows/
│   └── skills/
│       ├── feature-prioritisation/
│       ├── market-research/
│       └── prd-generator/
├── 06-decision-logs/
│   ├── 001-user-role-segmentation.md
│   ├── 002-rag-architecture.md
│   ├── 003-mcq-only-v1.md
│   ├── 004-dual-llm-strategy.md
│   ├── 005-passthrough-revenue-model.md
│   └── 006-centralised-auth-context.md
└── 07-vibe-coding-skills/
    ├── 01-README.md
    ├── 02-CLAUDE_PROJECT_SETUP.md
    ├── 03-vibe-coder-assistant-system-prompt.md
    ├── ai-specific/          (6 skills)
    ├── backend/              (6 skills)
    ├── devops/               (3 skills)
    ├── frontend/             (4 skills)
    ├── product/              (4 skills)
    └── testing/              (3 skills)
```

---

## Let's talk

I'm actively exploring **AI PM** and **Growth PM** roles — early-stage startups to growth-stage product orgs.

If you're building something ambitious with AI and need a PM who can think in systems *and* ship in sprints — [reach out](mailto:dushyantshah.ai@gmail.com).

> *"Strategy without execution is hallucination."*

---
