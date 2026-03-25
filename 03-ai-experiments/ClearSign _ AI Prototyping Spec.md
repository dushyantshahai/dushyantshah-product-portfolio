# ClearSign – AI Prototyping Spec

## Product Overview

ClearSign is a Manifest V3 Chrome extension which automatically detects legal agreements—such as Terms of Service, End User License Agreements (EULA), and Privacy Policies—visible in the browser DOM and generates AI-powered summaries with risk scoring, distilled into plain English for maximal clarity. The product serves two primary audiences: privacy-conscious power users seeking detailed legal insights, and parents or guardians aiming to protect minors from risky online agreements. This prototype covers the core extension popup UI (including onboarding with mode selection, summary with risk badge, collapsible summary sections), a simulated “agreement detected” flow, and a minimal B2B admin dashboard for managing users and reviewing activity. Out of scope for this prototype are: real LLM API interactions (mock JSON will stand in), live DOM parsing (a trigger button simulates detection), Stripe/payments, and ad delivery. Later versions may add web LLM integration, real DOM parsing, and live payments.

---

## Design & Visual Style

* **Component Library**: shadcn/ui (all components)

* **Styling Framework**: Tailwind CSS for consistent, utility-first styles

* **Color Scheme**:

  * Primary: #1A1A2E (deep navy)

  * Secondary: #16213E (dark blue)

  * Accent: #0F3460 (corporate blue)

  * Risk Low: #22C55E (green)

  * Risk Medium: #F59E0B (amber)

  * Risk High: #EF4444 (red)

  * Background: #F8FAFC (light gray-white)

* **Theme**: Light mode by default; dark mode enabled via toggle

* **Layout Style**:

  * Extension popup: fixed 400px-wide slide-in panel from the right, modal overlay dims rest of content (30% opacity)

  * Onboarding: full-screen overlay, wizard steps

  * B2B dashboard: left sidebar navigation with icons and labels, content area for detail panes and data tables

* **Typography & Spacing**: Inter font throughout, generous line height (1.6), bolded section headers, spacious layout (not dense)

* **Reference Sites**:

  * [Linear.app](https://linear.app) (for minimal, dense UI with clean visual hierarchy)

  * [Notion](https://notion.so) (for readable, modular content sections and accordions)

---

## Tech Stack

* **Framework**:

  * Next.js 14 (App Router) for /dashboard (B2B web app)

  * Vite + React for the standalone Chrome extension popup

* **Language**: TypeScript everywhere

* **Database**:

  * Supabase (Postgres) for B2B dashboard data

  * Chrome extension uses `chrome.storage.local` and `chrome.storage.sync` for local/session storage

* **ORM**: Drizzle (with Supabase Postgres adapter)

* **Auth**:

  * Clerk (org-level, for dashboard access)

  * Extension: JWT token stored in `chrome.storage.sync` (simulated/session only)

* **Hosting Target**:

  * Next.js dashboard: Vercel

  * Chrome extension: loaded locally via Chrome’s “unpacked extension” mode

* **Key Libraries**:

  * **Framer Motion** (slide-in panel and panel transitions)

  * **Lucide React** (iconography)

  * **Recharts** (analytics charts in dashboard)

  * **react-hook-form** (for forms and wizard)

  * **zod** (validation)

  * **clsx + tailwind-merge** (utility class management)

---

## Pages & Navigation

### Extension

* **/onboarding** (Onboarding Wizard):

  * Full-screen overlay on first install; guides user through logo/tagline, mode selection (Power User vs. Parent/Guardian), data consent (toggle list), and an optional feature tour

  * Public; launches automatically on extension install

* **Popup (Summary Panel)**:

  * Triggered by clicking the extension icon

  * Slide-in panel with:

    * Risk badge

    * Collapsible summary sections (six)

    * Mode toggle

    * Usage counter (e.g., "3 of 10 summaries used today") for free tier

    * Action CTAs ("I Understand, Proceed", "I Need More Time")

    * Tabbar navigation: **Summary | History | Settings**

* **/history** (History Tab, Popup):

  * Paid/Parent mode only

  * Displays a scrollable list of previous agreement summaries (site, risk score, timestamp)

### B2B Dashboard (Web App under /dashboard)

* **/dashboard (Overview)**:

  * Requires Clerk login

  * Stat cards (Total Users, Agreements Detected This Month, Avg Risk Score)

  * Recent activity feed

  * 30-day line chart of agreements detected

* **/dashboard/team (Team Management)**:

  * User management: invite, remove, assign roles (admin/member), set default mode across org

* **/dashboard/audit (Audit Log)**:

  * Data table: User, Agreement URL, Risk (color-coded badge), Timestamp, Action

  * Filter by risk level (dropdown), sortable columns, Export CSV button

**Navigation Structures:**

* Extension Popup: Top tab bar for Summary, History, Settings

* Dashboard: Left sidebar with icons+labels (“Overview”, “Team”, “Audit Log”, “Settings”)

**Auth Requirements:**

* B2B dashboard pages require Clerk authentication

* Extension onboarding and popup are public

**Landing Experience:**

* Extension: Welcome/onboarding on first run; after setup, shows Summary panel on click

* Dashboard: Authenticated users see organization overview on /dashboard

---

## Core User Flows

### Flow 1: First-Time Extension Onboarding

1. User installs the extension; onboarding overlay auto-opens

2. Step 1: Welcome screen shows logo, tagline, “Get Started” button

3. Step 2: Mode selection — two cards (“Power User”, “Parent/Guardian”), highlight on select, required selection to proceed

4. Step 3: Data consent — bullet list with toggles for each data permission; ‘Accept & Continue’ is disabled until all required toggles are enabled

5. Step 4: Optional tour — guided stepper overlays highlighting the click-to-detect feature and summary panel

6. Completing onboarding closes the overlay and activates extension idle state

### Flow 2: Agreement Detection & Summary (Extension)

1. User is on any webpage and opens the extension popup

2. User clicks “Simulate Detection” button

3. Extension icon on browser bar pulses (using GlowAnimation CSS)

4. User clicks the icon; SlideInPanel animates in from the right

5. RiskBadge at top shows risk level (color + icon + label)

6. Six summary sections display as accordions:

  * “Must Know” (expanded), all others collapsed

  * Each with clear heading/icon and summary text (from mock JSON)

7. At the panel bottom, CTA buttons:

  * “I Understand, Proceed” closes the panel

  * “I Need More Time” keeps panel open, exposes export/share option

8. Usage counter is visible (“3 of 10 summaries used today”, updates on each use)

9. Happy path: User reads summary, proceeds, and first history event is stored

### Flow 3: B2B Admin Dashboard

1. Admin logs in via Clerk and lands on /dashboard

2. Overview: sees three stat cards, activity feed, analytics line chart

3. Sidebar: navigates to Team Management (invite/remove users, set roles/mode) and Audit Log

4. On /dashboard/audit, table loads with events; user can filter by risk, sort, export CSV

5. All actions update immediately with visual feedback (snackbar/toast on invite/remove, loading spinners)

---

## Data Model & Backend

### Entities

* **Organization**

  * id: uuid

  * name: string

  * created_at: timestamp

  * settings: jsonb (enforced_mode, retention_policy)

* **User**

  * id: uuid

  * org_id: uuid (FK)

  * email: string

  * role: enum('admin', 'member')

  * mode: enum('power', 'parent')

  * created_at: timestamp

* **AgreementEvent**

  * id: uuid

  * user_id: uuid (FK)

  * org_id: uuid (FK)

  * agreement_url: string

  * risk_score: enum('low', 'medium', 'high')

  * summary_json: jsonb (with all six summary sections)

  * action_taken: enum('proceeded', 'deferred')

  * detected_at: timestamp

* **ConsentLog**

  * id: uuid

  * user_id: uuid (FK)

  * event_type: string

  * metadata: jsonb

  * created_at: timestamp

### Relationships

* Organization has many Users

* User has many AgreementEvents

* Events/Logs reference User and Organization

### APIs / Actions

* **GET /api/dashboard/stats** — org aggregate stats (user count, activity, average risk)

* **GET /api/dashboard/events** — paginated, filterable activity log

* **POST /api/dashboard/team/invite** — invite user

* **DELETE /api/dashboard/team/:userId** — remove user

### Extension Data

* Use chrome.storage.local for summary history (simple array per session/user)

* Use chrome.storage.sync for session JWT token

### Data Seeding

* Mock B2B data: 3 orgs, 10-15 users, 50+ agreement events (mixed risks)

* Mock JSON LLM outputs: Three legal agreement summaries, each with populated six-section summaries (High risk = social media ToS; Medium risk = SaaS free trial; Low risk = newsletter signup)

---

## Key Components

1. **RiskBadge**

  * Pill-style, color-coded (green/amber/red), includes Lucide icon: “shield-check” for Low, “alert-triangle” for Medium, “alert-octagon” for High, plus textual label

  * Used in summary panel header, audit log, and history

2. **SummaryAccordion**

  * Collapsible accordion (shadcn/ui) with six sections

  * Each: heading, section icon, body text (readable prose)

  * “Must Know” section expanded by default, others collapsed

  * 200ms smooth height transition; fully keyboard-accessible

3. **GlowAnimation**

  * CSS keyframes: applies pulsing accent-color glow to extension icon div when detection is “active”

  * Infinite animation, 50% opacity shadow; stopped/started based on detection state

4. **SlideInPanel**

  * Framer Motion powered fixed div, right side, 400px wide, 100vh tall, background white, drop shadow on left edge

  * Dims rest of page with 30% overlay

  * Panel is focus-trapped and closes with close/X button or CTA

5. **ModeToggle**

  * shadcn/ui Switch in the panel header; left = “Power User”, right = “Parent/Guardian”

  * Mode switch re-renders summary with matching content style/depth

6. **AuditTable**

  * shadcn/ui DataTable + TanStack Table for back-end pagination

  * Columns: User, Agreement URL (truncated with ellipsis/tooltip), Risk (RiskBadge), Timestamp (relative date), Action

  * Row- and column-level sorting, filtering by risk via dropdown, export CSV button top-right

7. **OnboardingWizard**

  * Multi-step react-hook-form with progress indicator (step dots)

  * Each step: card with title, description, action CTA

  * Data consent uses zod schema to enforce required toggles

---

## AI Generation Notes

* **Skip / Stub**:

  * No real LLM API; just load mock JSON (sample summary per risk level)

  * DOM detection: replace with “Simulate: Agreement Detected” button in popup

  * Stripe/payment: show “Upgrade” modal (modal only, no checkout)

  * Email notifications: log to console

  * Ad delivery: do not implement

* **Performance**:

  * Use React Server Components for all data-fetching dashboard pages

  * Extension popup: keep lightweight, full client-side React

* **Patterns & Practices**:

  * All conditional classNames via shadcn/ui’s cn() utility

  * Co-locate all component files with their respective page

  * Maintain types from Drizzle schema in both front and back end

* **Seed Data**:

  * Include in-prototype mock summaries for social media ToS (high risk), SaaS trial (medium), newsletter (low), each with all six summary sections

* **Mobile Responsiveness**:

  * Dashboard: responsive down to 768px (sidebar collapses to hamburger menu)

  * Extension popup: fixed 400px, does not need to be responsive

* **Accessibility**:

  * All active elements must have aria-labels or accessible roles

  * RiskBadge never color-only; must include icon and label

  * Keyboard navigability required for OnboardingWizard and SummaryAccordion

---

### Flow 2: Agreement Detection & Summary (Extension)

1. User is on any webpage and opens the extension popup

2. User clicks “Simulate Detection” button

3. Extension icon on browser bar pulses (using GlowAnimation CSS)

4. User clicks the icon; SlideInPanel animates in from the right

5. RiskBadge at top shows risk level (color + icon + label)

6. Six summary sections display as accordions:

  * “Must Know” (expanded), all others collapsed

  * Each with clear heading/icon and summary text (from mock JSON)

7. At the panel bottom, CTA buttons:

  * “I Understand, Proceed” closes the panel

  * “I Need More Time” keeps panel open, exposes export/share option

8. Usage counter is visible (“3 of 10 summaries used today”, updates on each use)

9. Happy path: User reads summary, proceeds, and first history event is stored

### Flow 3: B2B Admin Dashboard

1. Admin logs in via Clerk and lands on /dashboard

2. Overview: sees three stat cards, activity feed, analytics line chart

3. Sidebar: navigates to Team Management (invite/remove users, set roles/mode) and Audit Log

4. On /dashboard/audit, table loads with events; user can filter by risk, sort, export CSV

5. All actions update immediately with visual feedback (snackbar/toast on invite/remove, loading spinners)

---

## Data Model & Backend

### Entities

* **Organization**

  * id: uuid

  * name: string

  * created_at: timestamp

  * settings: jsonb (enforced_mode, retention_policy)


* **User**

  * id: uuid

  * org_id: uuid (FK)

  * email: string

  * role: enum('admin', 'member')

  * mode: enum('power', 'parent')

  * created_at: timestamp


* **AgreementEvent**

  * id: uuid

  * user_id: uuid (FK)

  * org_id: uuid (FK)

  * agreement_url: string

  * risk_score: enum('low', 'medium', 'high')

  * summary_json: jsonb (with all six summary sections)

  * action_taken: enum('proceeded', 'deferred')

  * detected_at: timestamp


* **ConsentLog**

  * id: uuid

  * user_id: uuid (FK)

  * event_type: string

  * metadata: jsonb

  * created_at: timestamp

### Relationships

* Organization has many Users

* User has many AgreementEvents

* Events/Logs reference User and Organization

### APIs / Actions

* **GET /api/dashboard/stats** — org aggregate stats (user count, activity, average risk)

* **GET /api/dashboard/events** — paginated, filterable activity log

* **POST /api/dashboard/team/invite** — invite user

* **DELETE /api/dashboard/team/:userId** — remove user

### Extension Data

* Use chrome.storage.local for summary history (simple array per session/user)

* Use chrome.storage.sync for session JWT token

### Data Seeding

* Mock B2B data: 3 orgs, 10-15 users, 50+ agreement events (mixed risks)

* Mock JSON LLM outputs: Three legal agreement summaries, each with populated six-section summaries (High risk = social media ToS; Medium risk = SaaS free trial; Low risk = newsletter signup)

---

## Key Components

1. **RiskBadge**

  * Pill-style, color-coded (green/amber/red), includes Lucide icon: “shield-check” for Low, “alert-triangle” for Medium, “alert-octagon” for High, plus textual label

  * Used in summary panel header, audit log, and history


2. **SummaryAccordion**

  * Collapsible accordion (shadcn/ui) with six sections

  * Each: heading, section icon, body text (readable prose)

  * “Must Know” section expanded by default, others collapsed

  * 200ms smooth height transition; fully keyboard-accessible


3. **GlowAnimation**

  * CSS keyframes: applies pulsing accent-color glow to extension icon div when detection is “active”

  * Infinite animation, 50% opacity shadow; stopped/started based on detection state


4. **SlideInPanel**

  * Framer Motion powered fixed div, right side, 400px wide, 100vh tall, background white, drop shadow on left edge

  * Dims rest of page with 30% overlay

  * Panel is focus-trapped and closes with close/X button or CTA


5. **ModeToggle**

  * shadcn/ui Switch in the panel header; left = “Power User”, right = “Parent/Guardian”

  * Mode switch re-renders summary with matching content style/depth


6. **AuditTable**

  * shadcn/ui DataTable + TanStack Table for back-end pagination

  * Columns: User, Agreement URL (truncated with ellipsis/tooltip), Risk (RiskBadge), Timestamp (relative date), Action

  * Row- and column-level sorting, filtering by risk via dropdown, export CSV button top-right


7. **OnboardingWizard**

  * Multi-step react-hook-form with progress indicator (step dots)

  * Each step: card with title, description, action CTA

  * Data consent uses zod schema to enforce required toggles

---

## AI Generation Notes

* **Skip / Stub**:

  * No real LLM API; just load mock JSON (sample summary per risk level)

  * DOM detection: replace with “Simulate: Agreement Detected” button in popup

  * Stripe/payment: show “Upgrade” modal (modal only, no checkout)

  * Email notifications: log to console

  * Ad delivery: do not implement


* **Performance**:

  * Use React Server Components for all data-fetching dashboard pages

  * Extension popup: keep lightweight, full client-side React


* **Patterns & Practices**:

  * All conditional classNames via shadcn/ui’s cn() utility

  * Co-locate all component files with their respective page

  * Maintain types from Drizzle schema in both front and back end


* **Seed Data**:

  * Include in-prototype mock summaries for social media ToS (high risk), SaaS trial (medium), newsletter (low), each with all six summary sections


* **Mobile Responsiveness**:

  * Dashboard: responsive down to 768px (sidebar collapses to hamburger menu)

  * Extension popup: fixed 400px, does not need to be responsive


* **Accessibility**:

  * All active elements must have aria-labels or accessible roles

  * RiskBadge never color-only; must include icon and label

  * Keyboard navigability required for OnboardingWizard and SummaryAccordion

---

This spec is complete and sufficient for AI prototyping tools (e.g. v0, Bolt, Lovable, Replit) to generate the working ClearSign prototype as described, with all visual, behavioral, and technical requirements explicit.

This spec is complete and sufficient for AI prototyping tools (e.g. v0, Bolt, Lovable, Replit) to generate the working ClearSign prototype as described, with all visual, behavioral, and technical requirements explicit.