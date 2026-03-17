# Decision Log 006: Centralised AuthContext

**Date:** February 2026
**Decision area:** Technical architecture
**Status:** Outcome confirmed ✅

---

## Context

MorphEd is a multi-role SPA (Single Page Application) with four distinct user types — Admin, Professor, Student, and Founder — each with different dashboards, navigation, permissions, and data access. Early in development, the question was: **how should authentication state be managed across the application?**

Getting this wrong causes subtle but trust-eroding bugs: pages flashing before redirecting, role-specific layouts rendering incorrectly, or users briefly seeing dashboards they shouldn't. These aren't critical failures — but in a product used in formal academic settings, they undermine confidence quickly.

---

## Options Considered

**Option A: Local / decentralised auth state (prop drilling or per-page auth checks)**
Each page or component fetches and validates the current user's session independently. Auth state is passed down via props or fetched on demand.
- No upfront architecture required; auth is handled where it's needed
- Creates race conditions: route guards and API responses validate auth asynchronously, creating brief moments where the application doesn't know who it's serving
- Prop drilling auth state across nested components becomes unmaintainable as the component tree grows
- Every new page or component must independently implement auth checks — easy to miss one, creating permission gaps
- Page flashes (rendering a component, then redirecting away) are nearly unavoidable without a centralised state

**Option B: Global AuthContext with AuthProvider wrapping the entire application**
Implement a single `AuthContext` and `AuthProvider` at the application root. Every component in the tree — nested routes, layout components, conditional renders — has immediate, synchronous access to the current user's identity and role.
- One source of truth: auth state is resolved once at app load; no component ever operates in an auth-uncertain state
- Role-based rendering is clean: `if (role === 'professor')` checks work reliably from any component depth
- No prop drilling: auth state is accessed via `useAuth()` hook from anywhere in the tree
- Adds upfront architectural setup; requires careful implementation of token refresh and session expiry logic

---

## Choice

**Option B — Global AuthContext with AuthProvider.**

---

## Rationale

In a multi-role product, the cost of auth uncertainty is high. A student briefly seeing a professor's assessment dashboard, or an admin page rendering before redirecting — these are small failures that create disproportionate trust damage in an educational context.

The global `AuthContext` eliminates this class of problem entirely. From the moment a user logs in, every part of the application knows exactly who they are and what they can see. There's no prop drilling, no race condition, and no moment of uncertainty.

The upfront complexity (token refresh logic, session expiry handling, provider setup) is a one-time cost. The ongoing benefit — clean, reliable role-based rendering across a product that will continue to grow in complexity — justifies it.

---

## Outcome / Reflection

The global auth context has been one of the cleanest architectural decisions in the codebase. No auth-related bugs have surfaced in testing across any of the four roles. New pages and components access user identity and role via `useAuth()` with no additional setup — the pattern scales naturally as the product grows.

The one nuance encountered was handling token refresh for long sessions (professors leaving the application open during a teaching day). This required implementing a silent token refresh mechanism within the AuthProvider — straightforward to add, but something to account for upfront in any future multi-role SPA.
