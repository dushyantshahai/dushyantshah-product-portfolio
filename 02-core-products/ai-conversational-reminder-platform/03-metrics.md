# RemindMe Metrics & Success Framework

RemindMe's success hinges on a single North Star Metric: **Active Reminders Delivered per Day**. This captures the complete product value loop — users created a reminder, the conversation produced a valid structured output, the reminder reached its scheduled time, and the notification was delivered. A reminder that is created but never fires (broken context extraction, missed cron window, or unregistered FCM token) represents a failure of the entire product promise, not just a notification bug.

## Key Framework Components

**The metric tree branches into two critical pillars:**

The **creation side** tracks how many reminders reach `active` status from `draft`, broken down by conversation completion rate, average turns to completion, and the percentage of reminders that complete in under 3 turns (the "fast path" signal — indicating users are being served, not interrogated). The **delivery side** measures how many active reminders fire successfully, with sub-metrics on FCM delivery success rate, in-app notification read rate, and user action rate (mark complete / snooze / dismiss) as a proxy for reminder relevance.

## Six-Month Objectives

The product targets three primary goals:

1. **AI conversation quality:** 80% of reminder conversations complete in 3 turns or fewer, with an LLM failure rate (both Gemini and Claude failing) below 1%. Context extraction accuracy — measured by the percentage of saved reminders with a valid `reminder_time` and at least one enriched context field beyond the title — should exceed 85%.

2. **User retention:** Achieve a 7-day reminder creation retention rate of 40% — meaning 40% of users who create their first reminder return to create another within a week. Week-over-week active user growth of 20% or higher through the first 90 days.

3. **Delivery reliability:** Maintain a cron delivery success rate above 95%, FCM push delivery rate above 90% for users with registered tokens, and a notification-to-action conversion rate (user takes any action on the delivered reminder) of 60% or higher.

## Dual Funnel Strategy

Success requires monitoring both the creation funnel and the delivery funnel independently. The creation funnel progresses from landing page → sign-in → first reminder attempt → conversation completion → active reminder saved. The delivery funnel progresses from scheduled reminder → cron pickup → notification sent → notification delivered → user action taken.

Guardrail metrics protect against optimizing delivery volume at the expense of reminder quality: hallucinated context fields (e.g., a `reminder_time` that doesn't match what was agreed in conversation) must stay below 3%, and conversation abandonment rate (user closes the conversation mid-flow without saving) must stay below 20%.
