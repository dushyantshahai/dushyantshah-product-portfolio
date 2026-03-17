# Decision Log 001: User Role Segmentation

**Date:** January 2026
**Decision area:** Product architecture
**Status:** Outcome confirmed ✅

---

## Context

MorphEd serves multiple user types within a single institute — someone who sets up the platform, someone who creates assessments, and someone who takes them. The early design question was how granularly to model these roles. Go too coarse and you either over-permission users or create a product that tries to serve everyone from one interface. Go too fine and you create unnecessary complexity before you've validated anything.

The decision: **four distinct roles — Admin, Professor, Student, and Founder (Product Owner).**

---

## Options Considered

**Option A: Two roles — Teacher and Student**
Simple. Merge admin and professor functions into a single "Teacher" role. This is how most early-stage EdTech products are built.
- Fast to build, low upfront complexity
- Breaks quickly: a teacher who can also delete students or edit another teacher's assessments creates operational risk at the institute level
- No separation between platform-level management and content creation

**Option B: Three roles — Admin, Teacher, Student**
Separates management from content creation. Admin handles users and content library; teachers create and publish assessments; students attempt them.
- Cleaner than Option A, mirrors most institutional hierarchies
- Loses the Founder view — no cross-institute oversight for the product owner monitoring platform health

**Option C: Four roles — Admin, Professor, Student, Founder**
Adds a Founder layer with cross-institute visibility: platform-wide NSM tracking, institute-level health monitoring, and metrics access without touching operational data.
- Mirrors how educational institutes actually work
- Founder role keeps product analytics clean and separate from user-facing functionality
- More upfront build, but prevents permission and access bugs from compounding later

---

## Choice

**Option C — Four distinct roles.**

---

## Rationale

The separation between Admin (infrastructure operator) and Professor (value creator) was the critical insight. Merging these into a single "Teacher" role would mean giving every professor the ability to delete students, manage batches, and edit other professors' content — none of which should be possible. Designing around the real institutional structure from day one prevented a class of permission bugs that would have been painful to retrofit later.

The Founder role was a smaller but important decision. Without it, monitoring platform-wide metrics would require either logging in as an admin at each institute or building a separate internal dashboard. Adding it as a first-class role kept the architecture clean.

---

## Outcome / Reflection

The four-role model proved correct. The Admin/Professor separation in particular created clean product surfaces — each user sees exactly what they need, nothing more. No edge cases around cross-role permissions have emerged in testing.

The Founder role has been used primarily for NSM tracking and institute activation monitoring. In hindsight, this could have been implemented as an internal admin tool rather than a product role — but the distinction hasn't caused any problems, and the flexibility is useful.
