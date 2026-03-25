# Product Requirements Document — MorphEd
**Version:** 1.0 | **Status:** Live (V1) | **Last Updated:** March 2026

---

## 1. Executive Summary

MorphEd is a B2B SaaS Assessment-as-a-Service (AaaS) platform built for Indian educational institutes. It enables professors to generate syllabus-grounded MCQ assessments in under 2 minutes using a RAG-based AI pipeline, distribute them to student batches, and collect structured, lecture-level performance data — all within a single platform.

The product eliminates manual assessment creation overhead, closes the feedback void between lectures and exams, and gives institutes their first source of granular, actionable learning analytics.

---

## 2. Problem Statement

Three structural bottlenecks define the problem:

**Post-Lecture Feedback Void:** Assessments in Indian institutes are deferred to mid-terms and finals. Dozens of incremental learning checkpoints are missed every semester, and knowledge gaps go undetected until it's too late to course-correct.

**Fragmented, Manual Workflows:** Manual MCQ creation costs professors 60–90 minutes per test. Where AI tools are used, they operate in silos — producing questions ungrounded in the specific syllabus taught — with no end-to-end pipeline from generation to student delivery.

**Analytics Blind Spot:** Granular lecture-level performance data is virtually non-existent. Data sits in paper, spreadsheets, or disconnected systems. Without it, institutes cannot intervene before a student fails.

---

## 3. Market Context

India has one of the world's largest and most assessment-heavy education systems — 350M+ students across K-12 and higher education, a ₹58,000+ crore coaching industry driven by JEE, NEET, and CUET entrance exams, and teachers creating 4–8 assessments per subject per term. Most still use MS Word or handwritten question papers.

Existing tools (Google Forms, Quizizz) offer generic question banks with no syllabus grounding. Platforms like Embibe or BYJU's serve students directly but don't empower teachers. **The whitespace:** a B2B tool that starts from the institute's own syllabus and puts the professor in control.

---

## 4. Target Users & Personas

**Institute Admin — Sunita Joshi** (38, Pune): Manages user accounts, content uploads, and batch groupings — currently fragmented across Excel, WhatsApp, and physical registers. Needs unified setup and governance without IT dependency.

**Professor — Rajan Kulkarni** (44, Mumbai): Spends evenings manually writing MCQs. Generic AI tools produce questions misaligned with his specific textbook. Needs fast, syllabus-accurate assessments and automatic post-submission analytics.

**Student — Manisha Shah** (17, Mumbai): No institute-specific practice aligned to her syllabus. Waits days for graded results. No data on which topics she's weak on. Needs assessments tied to what was actually taught, instant results, and performance tracking.

---

## 5. Jobs-to-Be-Done

**Professor:** *"When I finish a lecture, I want to quickly assess whether my students understood it — without spending hours creating a quiz — so I can identify who needs help before the next class, not after the semester exam."*

**Student:** *"When I finish a lecture, I want to know immediately whether I've understood the concepts — so I can address gaps now, not discover them during the exam."*

---

## 6. Solution Overview

MorphEd provides three role-specific interfaces — Admin (setup and governance), Professor (generation and publishing), and Student (assessment and results) — connected by an AI pipeline that automates ~95% of the assessment drafting process.

**What MorphEd solves:** The lecture-to-assessment gap; ~95% drafting automation via strictly document-grounded RAG; granular lecture-level mastery tracking; zero-hallucination design — all AI output anchored to uploaded source material.

**Deliberate scope boundaries (V1):** No autonomous teaching, no subjective essay grading, no general content authoring, no classroom management. Every boundary is intentional and tied to a clear V2 unlock condition.

---

## 7. AI Architecture

MorphEd's core intelligence runs on a **dual-LLM RAG pipeline** across five stages: ingestion, chunking, embedding, retrieval, and generation. The decision to use two different models — rather than one — was empirical: each model was tested on both tasks.

**Ingestion & TOC Extraction — Google Gemini 3 Flash:** Textbook PDFs are processed in a single multimodal pass, leveraging Gemini's 1M+ token context window. The model reads visual layout cues (font weight, indentation, page breaks) and returns a structured 4-level content hierarchy: Book → Chapter → Topic → Sub-topic, each node with page ranges. No OCR pipeline. No multi-step stitching.

**Semantic Chunking:** Content is chunked within the exact page boundaries of each node in the hierarchy — not by fixed character count. Every chunk carries deep hierarchical metadata, making strict topic-level filtering possible at retrieval time.

**Retrieval (V1):** PostgreSQL Full-Text Search with hierarchical SQL metadata filtering. No vector database at V1 — zero additional infrastructure, and for exact-domain retrieval, lexical search with hard metadata filters outperforms semantic search in precision and cost. Vector schema is already prepared for the V2 hybrid retrieval upgrade.

**MCQ Generation — Claude 3 Haiku:** Selected for cost-efficiency, speed, and structured output quality — the right balance for high-volume generation. Difficulty is instruction-driven: Easy (recall) → Medium (application/analysis) → Hard (synthesis/evaluation). Every MCQ includes a mandatory explanation grounded in source content — this serves dual purpose: pedagogical value for students and an indirect hallucination signal for the system.

---

## 8. Core User Flows

**Admin:** Onboards professors and students (individual or bulk CSV), creates batches, uploads textbooks with auto-parsed TOC hierarchies, monitors institute-wide performance.

**Professor (4-step wizard):** Basic Details → Content Selection (cascading Subject → Book → Chapter → Topic → Sub-topic, with optional page range) → AI Generation → Review & Publish. Manages published assessments (view, edit, unpublish, delete) and views class-level analytics post-submission.

**Student:** Attempts assigned assessments (one question at a time, with countdown timer and autosave), submits for instant auto-grading, views per-question correctness breakdown and personal performance analytics over time.

---

## 9. Key UX Decisions

| Decision | Rationale |
|---|---|
| 4-step wizard over single long form | One bounded decision per step; maps to how professors mentally sequence assessment creation and reduces drop-off |
| One question at a time for students | Mirrors formal exam format; prevents scanning ahead; reinforces platform credibility |
| Single-page Content Library | Admin verifies full Subject → Book → Chapter → Topic chain without navigation hops — critical for catching TOC parsing errors before they reach professors |
| Unpublish as first-class action | Professors occasionally need to retract mid-cycle; burying it created fear of publishing too early |
| Hierarchical topic view over flat list | Flat list removes the curriculum context professors rely on when scoping assessments |

---

## 10. Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js + Tailwind CSS |
| Backend | Node.js + Express |
| LLM — Generation | Claude 3 Haiku |
| LLM — Extraction | Google Gemini 3 Flash |
| Retrieval | PostgreSQL Full-Text Search + hierarchical SQL filters |
| Vector Store | pgvector (schema prepared, not active — V2) |
| Database + Auth | Supabase (PostgreSQL + Row-Level Security) |
| Hosting | Vercel (frontend) + Railway (backend) |

---

## 11. Metrics & Success Framework

**North Star Metric: Assessments Completed per Day.** This metric only moves when the full loop closes — generation, publishing, and student completion. It is equally sensitive to supply-side failures (professors not generating or publishing) and demand-side failures (students not attempting or submitting).

### NSM Metric Tree

| Level | Metric | Side |
|---|---|---|
| L0 | Assessments Completed / Day | North Star |
| L1-1 | Assessments Published / Day | Supply |
| L1-2 | Avg. Completions / Assessment | Demand |
| L2-1 | Assessments Generated / Day | Supply lever |
| L2-2 | Generation → Publish Rate | Supply lever |
| L2-3 | Unique Professors Generating / Day | Supply lever |
| L2-4 | Assessment Start Rate | Demand lever |
| L2-5 | Start → Submit Rate | Demand lever |
| L2-6 | Average Student Score | Quality signal |

### OKRs — First 6 Months

**O1: Prove genuine time value for professors**
— Median generation time (content selection → ready questions) < 2 min | % assessments published after generation ≥ 80% | Week 4 retention ≥ 50%

**O2: Establish habitual professor users**
— 50 Monthly Active Teachers (≥ 3 assessments / 4 weeks) by end of month 3 | WoW growth ≥ 25% | NPS ≥ 70

**O3: Validate MCQ quality as a durable product strength**
— % published after first generation (no re-gen required) ≥ 70% | Average teacher quality rating ≥ 3.0 / 5.0 | Hallucination rate ≤ 5%

### Guardrail Metrics

| Guardrail | Threshold |
|---|---|
| MCQ hallucination rate | ≤ 5% — most critical; one incorrect question in a formal assessment creates reputational damage that is hard to recover from in trust-sensitive institutional sales |
| Generation failure rate | ≤ 2% |
| Teacher edit rate per MCQ | Baseline being established; sudden spike is the quality drop signal |

---

## 12. Revenue Model

**Three-component pricing:** base platform fee per institute + fee per student per submitted test + LLM token cost passthrough. This structure ties revenue to value delivery (completions, not just seats), keeps margins predictable as usage scales, and ensures heavy-use institutes are not subsidised by light-use ones.

---

## 13. Key Product Decisions

| Decision | Chosen Approach | Rationale |
|---|---|---|
| Question types (V1) | MCQ-only | Objective signal; instant verifiability; structured data feeds directly into analytics. Subjective types introduce grading ambiguity at scale — deferred to V2. |
| AI architecture | RAG over direct generation | Direct prompting works until it doesn't — cross-topic contamination, inconsistent repeat outputs, trust erosion. RAG provides one-time ingestion, structured navigation, and strict grounding. |
| Dual-LLM strategy | Gemini for ingestion, Claude for generation | Empirically tested — each model won on its specific task. Gemini's multimodal vision handles document layout; Claude's instruction-following handles structured output. |
| Retrieval (V1) | PostgreSQL FTS + metadata filters | Zero additional infrastructure at MVP stage; vector schema prepared for V2 semantic upgrade. |

---

## 14. V2 Roadmap

**Mobile + Offline Mode:** Native app with offline-first assessment delivery — syncs to device, pushes results on reconnect. Unlocks semi-rural and coaching institutes with unreliable internet, a significant Indian market segment.

**Advanced Assessment Intelligence:** Expanded question types (fill-in-the-blank, match-the-following, short-answer) via the same RAG pipeline. Automatic difficulty calibration based on historical cohort performance — the system learns what's actually hard for which students, not just what difficulty label was applied at generation.

**Institutional Growth & AI Ops:** LMS integrations (Moodle, Google Classroom, Canvas), predictive analytics for at-risk cohort identification, self-serve onboarding, and billing automation to support the B2B sales motion at scale.

**Vernacular Language Support:** Full vernacular language support for assessments and interfaces — a significant unlock for Tier 2/3 markets and regional-medium institutes.

---

*MorphEd — Assessment-as-a-Service for Indian Education*
