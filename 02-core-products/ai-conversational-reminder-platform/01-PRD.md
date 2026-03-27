# RemindMe — Product Requirements Document

## 1. Executive Summary

RemindMe is a consumer-facing AI reminder app that replaces traditional form-based reminder creation with a conversational setup flow. Users type a plain-language statement — "remind me to pay my credit card bill this weekend" or "remind me to pick up my daughter from school at 3:30 tomorrow" — and an AI assistant handles the rest: asking targeted follow-up questions, computing schedules, and saving a richly structured reminder with theme tags, urgency classifications, and extracted context. The product was designed, built, and deployed to production as a personal AI PM portfolio project, demonstrating end-to-end ownership across product strategy, AI system design, backend architecture, and frontend implementation.

---

## 2. Problem Statement

Traditional reminder apps require users to context-switch from thinking about a task to operating a form — selecting dates, typing titles, setting times through pickers. For complex reminders (recurring health habits, bill due dates, event-based tasks), this friction compounds: users must mentally decompose intent before the app can record it. Existing solutions are also passive: they store exactly what you type, without asking whether a bill due on the 20th should actually fire a reminder on the 18th, or whether "water 6 times a day" means the system should compute a distribution schedule.

---

## 3. Market Context

The personal productivity market is large and competitive — Google Keep, Apple Reminders, Todoist, and Any.do collectively serve hundreds of millions of users. Yet no mainstream consumer reminder app has made the AI-first conversational creation flow the *primary* interaction paradigm. AI features in these tools are layered on as suggestions or sidebars, not as the core creation engine. RemindMe's hypothesis: the shift from form-fill to "tell me what you need" represents the same UX leap that transformed search and voice assistants — and the moment for it in personal task management has arrived.

---

## 4. Target Users

Three primary personas drive the product requirements:

**The Busy Professional (Arjun):** Sets reminders on the go, needs speed over richness. Values a creation experience as fast as dictating a note. Primary frustration: form UX breaks focus.

**The Health and Habits User (Priya):** Sets recurring reminders for water, medication, and exercise. Needs the system to compute distribution patterns and confirm the schedule, not ask open-ended questions.

**The Finance-Aware User (Vikram):** Manages multiple financial obligations with tight due dates. Needs proactive gap detection — the assistant should recognize that a bill due on the 20th requires a reminder on the 18th.

---

## 5. Jobs to Be Done

- When I need to remember something, I want to state it naturally so I don't have to break my flow to fill in a form.
- When I set a recurring health reminder, I want the system to figure out the schedule so I don't have to do the math.
- When I log a payment due date, I want the app to proactively remind me before the deadline — not on it.
- When I review my reminders, I want to see them organized by urgency and theme so I can act on what matters most.

---

## 6. Solution Overview

RemindMe's core loop: (1) User types a reminder statement. (2) App creates a draft reminder and launches the AI conversation. (3) Assistant asks up to 5 targeted follow-up questions, infers where it can, proposes specific times rather than asking open-ended questions. (4) On conversation completion, the system extracts structured context and saves the reminder as active. (5) A cron job delivers in-app and push notifications at the scheduled time. (6) Users see a theme- and urgency-tagged dashboard of all their reminders.

---

## 7. AI Architecture

The AI layer is built around three pipelines sharing a dual-LLM infrastructure (Gemini 2.5 Flash primary, Claude 3 Haiku fallback with automatic JSON-validation-based failover):

**Conversational Response Engine:** Uses think-step prompting — the model reasons silently through 7 questions before generating a response. Encodes domain-specific logic for 9 reminder categories (bills/EMIs, recurring health, event tickets, grocery/shopping, work tasks, social/calls, documents, transport, travel). Enforces a strict zero-repetition and forward-momentum rule per response.

**Context Extraction Engine:** Converts completed conversations into structured JSON records with 11 fields: reminder_time, reminder_date, is_recurring, recurrence_pattern, reminder_count, frequency_description, location, participants, notes, theme_tag, urgency_tag. Output is post-processed through a heuristic normalizer enforcing valid taxonomy membership.

**Time Parsing Engine:** Three-stage pipeline — regex pattern matching → LLM parsing → default fallback (9 AM tomorrow). Pattern matcher handles 90%+ of common cases without an LLM call.

---

## 8. User Flows

**Reminder Creation Flow:** Landing / Dashboard → Create Reminder → Type natural language statement → App creates draft + launches AI conversation → Up to 5 turn dialogue → Conversation completes (keyword / turn limit / explicit confirm) → Context extracted → Reminder saved as active → User returned to dashboard.

**Notification Delivery Flow:** Cron job polls active reminders past `reminder_time` → Fires in-app notification (persisted to Notification table) → Fires FCM push to all registered tokens → User sees notification bell badge → User opens notification → Takes action (complete / snooze / dismiss) → Reminder status updated.

---

## 9. UX Decisions

The assistant is designed to sound like a helpful friend — lowercase tone ("got it", "sure", "okay"), never repeating confirmed information, always moving the conversation forward. Context-specific warm closers ("enjoy your pizza!", "stay hydrated!") signal conversation end without robotic confirmation. For ambiguous times, the assistant proposes specifics ("would 7 PM work?") rather than asking open questions. A hard 5-turn cap prevents unbounded conversations and forces graceful completion even on vague inputs.

---

## 10. Tech Stack

- **Frontend:** Next.js 14 (App Router), TypeScript, Tailwind CSS, Lucide React
- **Backend:** Next.js API Routes (serverless), Prisma ORM, PostgreSQL (Neon)
- **Authentication:** NextAuth v4, Google OAuth, @next-auth/prisma-adapter
- **AI:** Google Gemini 2.5 Flash (@google/generative-ai), Anthropic Claude 3 Haiku (@anthropic-ai/sdk)
- **Push Notifications:** Firebase Cloud Messaging (firebase, firebase-admin)
- **Scheduling:** Vercel Cron Jobs
- **Deployment:** Vercel (serverless), Neon (managed PostgreSQL)
- **Utilities:** date-fns (time parsing), bcryptjs (password hashing), clsx / tailwind-merge

---

## 11. Metrics Framework

**North Star Metric:** Active Reminders Delivered per Day — captures full product value loop from creation through delivery.

**Creation-side L1/L2 metrics:** Conversation completion rate, average turns to completion, fast-path rate (≤3 turns), context extraction accuracy (valid reminder_time + at least one enriched field).

**Delivery-side L1/L2 metrics:** Cron delivery success rate (target: >95%), FCM push delivery rate (target: >90%), notification-to-action conversion rate (target: >60%).

**Guardrail metrics:** LLM failure rate <1%, hallucinated context fields <3%, conversation abandonment rate <20%.

---

## 12. Revenue Model

V1 is deployed as a personal portfolio and free-to-use application. A monetization model is defined for V2: a freemium tier (up to 20 active reminders, 3 AI conversations per day) and a Pro tier (~$5/month) unlocking unlimited reminders, full recurrence automation, calendar write-back, and multi-device sync.

---

## 13. Key Decisions

Six deliberate choices defined the V1 architecture: conversation-first over form-first (core UX bet), dual-LLM with automatic fallback (reliability without queue infrastructure), think-step prompting over template dialogue (natural responses at the cost of testability), Google OAuth only (eliminates auth infrastructure complexity), FCM for push (free tier + first-party browser support), PostgreSQL over MongoDB for context storage (query flexibility over schema flexibility). Full rationale documented in product-decisions.md.

---

## 14. V2 Roadmap

Priority V2 investments: true server-side recurrence automation, native mobile wrapper (PWA or React Native), calendar write-back to Google Calendar, voice input via Web Speech API, smart snooze with context awareness, reminder templates for high-frequency patterns, and natural language reminder search.
