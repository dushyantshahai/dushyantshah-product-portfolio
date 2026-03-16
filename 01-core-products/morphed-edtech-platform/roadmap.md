# Product Roadmap — MorphEd

## V1 — The Foundation (Current)

V1 is live and running. The goal was to prove the full loop — from textbook upload to student assessment completion — works reliably enough to put in front of real institutes. Every decision in V1 was made in service of that, not feature completeness.

### What's Been Built

**Verified RAG Engine**
The core pipeline is working in production. Admin uploads a PDF or DOCX textbook, Gemini 1.5/3 Flash reads the document natively (no OCR, full multimodal pass) and returns a structured 4-level content hierarchy — Book → Chapter → Topic → Sub-topic. Professor selects a topic, Claude 3 Haiku generates grounded MCQs with distractors, difficulty tags, and explanations. Every question is anchored to the uploaded syllabus, not general LLM knowledge.

**Hierarchical Content Library**
Books are uploaded and indexed once. The resulting chapter-topic-subtopic hierarchy is permanently stored and navigable. Professors browse their syllabus structure, select the scope they want to assess, and generate — no re-uploading, no re-processing.

**Quad-Role Architecture**
Four distinct roles shipped: Admin (manages users, uploads books, creates batches), Professor (generates and publishes assessments, views analytics), Student (attempts and submits assessments, views results), and Founder (platform-wide health metrics and NSM visibility). Each role sees exactly what they need — nothing more.

**Premium Light Aesthetics**
The product is designed to feel like a professional tool, not a prototype. Clean layouts, consistent typography, and a UI that respects the educator's time. First impressions matter in institutional sales.

**Operational Foundation**
Deployed on Vercel (frontend) and Railway (backend), with Supabase handling the database and auth layer. Revenue model is live: institutes pay a base platform fee plus token cost passed through at usage. No subsidised tiers, no usage ambiguity.

---

## V2 — Scale & Sophistication (Upcoming)

V2 is built on what V1 proved. The RAG engine is validated. The role architecture works. Now the platform needs to go deeper on intelligence, broader on reach, and smoother on operations.

### What's Coming

**Mobile App with Offline Mode**
A native mobile app for both professors and students. The key feature is offline-first assessment delivery — assessments sync to device and can be attempted without a network connection, with results pushed when connectivity returns. This unlocks semi-rural institutes and coaching centres with spotty internet, which is a significant segment of the Indian market.

**Advanced Assessment Intelligence**
V1 is MCQ-only by design — the right call for proving the loop. V2 expands the assessment type set: fill-in-the-blanks, match-the-following, and short-answer questions, each grounded in the same RAG pipeline. Alongside new types, V2 introduces smart explanations (answer justifications that reference the exact source passage) and automatic difficulty calibration based on historical student performance data — the system learns which questions are actually hard for which cohorts, not just which difficulty label was applied at generation.

**Institutional Growth & AI Ops**
Three capabilities that unlock the B2B sales motion at scale. First, LMS integrations — direct sync with Moodle, Google Classroom, and Canvas so assessments flow into systems institutes already use. Second, predictive analytics — the platform surfaces early warning signals on student cohorts at risk of falling behind, giving professors and HODs actionable data before exam season. Third, billing automation — self-serve institute onboarding, automated invoice generation, and usage dashboards that reduce the operational overhead of scaling to new clients.
