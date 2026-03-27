# RemindMe Product Roadmap

## Current State (V1)

RemindMe has shipped a fully functional MVP, deployed on Vercel with a production PostgreSQL database (Neon), Firebase Cloud Messaging for push notifications, and Google OAuth authentication via NextAuth.

The V1 product delivers the core conversational creation loop: a user types a plain-language reminder statement, the AI assistant engages in up to 5 turns of dialogue to gather missing context, and the system saves a structured reminder with extracted fields including `reminder_time`, `reminder_date`, `is_recurring`, `location`, `participants`, `notes`, `theme_tag`, and `urgency_tag`. Completed reminders are delivered via both in-app notifications (notification bell with unread count) and Firebase push notifications to all registered devices.

The reminder lifecycle covers four states: `draft` (conversation in progress), `active` (saved and scheduled), `completed` (delivered and actioned), and `snoozed`. A Vercel cron job handles scheduled delivery by querying all active reminders whose fire time has passed.

The dual-LLM pipeline (Gemini 2.5 Flash → Claude 3 Haiku fallback) is live, with domain-specific conversation logic for nine reminder categories and a think-step prompting approach that produces natural, non-robotic assistant responses. The three-stage time parsing engine (pattern matching → LLM → default fallback) handles the full range of time expressions reliably.

## Upcoming Vision (V2)

The next phase focuses on two dimensions: **recurring reminder automation** and **expanded intelligence**.

On the automation side, V2 will implement true server-side recurrence — reminders flagged as `is_recurring: true` will automatically re-queue after delivery, with the cron job generating the next instance based on the `recurrence_pattern` and `frequency_description` stored in context. This removes the current V1 limitation where recurring reminders must be manually re-created each cycle.

On the intelligence side, V2 will add calendar write-back (creating Google Calendar events from reminders, not just reading calendar context), voice input on mobile via the Web Speech API, a "smart snooze" feature that understands context ("snooze until after my meeting"), and multi-device sync with real-time notification state across web and mobile clients.

Operationally, V2 introduces a native mobile wrapper (React Native or PWA with offline-first storage), reminder templates for high-frequency patterns (daily water reminders, monthly bill payments), and a natural language reminder search — "show me all my finance reminders this month."
