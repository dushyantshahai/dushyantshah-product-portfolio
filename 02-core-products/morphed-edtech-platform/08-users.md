# User Personas — MorphEd

---

## Persona 1 — Institute Admin

**Name:** Sunita Joshi
**Role:** Administrative Coordinator, HSC Coaching Institute, Pune
**Age:** 38 | **Tech comfort:** Moderate — comfortable with web portals, spreadsheets, and WhatsApp

### User Journey
- Sunita is the first user to interact with MorphEd at the institute level — she registers the institute, onboards professors and students, and organises content before anyone else can use the platform
- Her day-to-day role involves managing user accounts, grouping students into batches, maintaining the subject content library, and monitoring institute-wide performance
- She is not involved in assessment creation or delivery; her role is entirely setup and governance

### Jobs to Be Done
- Onboard and manage professor and student accounts without IT support
- Organise the institute's subject library so professors can generate assessments against the right content
- Group students into batches for assessment distribution and monitor institute-level performance without pulling manual reports

### Pain Points
- Student records, professor allocations, and batch groupings exist in silos — an Excel sheet, a WhatsApp group, and a physical register — causing duplication and errors
- No unified view of institute-level performance across subjects and batches makes it impossible to flag underperforming cohorts proactively
- Most admin tools require IT expertise to configure or carry high licensing costs that small coaching institutes can't justify

### Currently Available Solutions
- ERP tools like Fedena or Extramarks Admin require IT expertise and significant setup overhead
- Spreadsheets and WhatsApp groups handle user management informally but break at scale
- No affordable tool connects user management, content organisation, and performance analytics in a single interface

### How MorphEd Resolves This
- A single admin dashboard handles professors, students, and batches — with both manual and CSV bulk onboarding — eliminating the need for separate spreadsheets or tools
- The Content Library lets Sunita upload subject-specific PDFs with structured chapter/topic hierarchies that directly power professor-created assessments
- An aggregate analytics dashboard surfaces institute-wide performance without any manual reporting, accessible via a simple web interface with no IT dependency

---

## Persona 2 — Professor

**Name:** Rajan Kulkarni
**Role:** Faculty, Bookkeeping & Accountancy, Junior College, Mumbai
**Age:** 44 | **Tech comfort:** Low-to-moderate — uses WhatsApp, Google, and basic web tools

### User Journey
- Rajan teaches three batches and runs unit tests every month; he starts on MorphEd after Sunita has set up his account, the subject library, and his assigned batch
- His primary workflow is assessment creation: select the chapter just finished, configure test parameters, let AI generate questions, review them, and publish directly to his batch
- Post-assessment, he checks class-level analytics to decide whether to revisit any topic before moving forward

### Jobs to Be Done
- Create syllabus-accurate MCQ assessments quickly without spending hours writing questions manually
- Publish assessments to the right batch instantly and retain control to edit or retract them if needed
- Access class-level analytics to identify which topics need reinforcement without manual tallying

### Pain Points
- Writing MCQs manually — especially credible wrong options (distractors) — takes 60–90 minutes per test, consuming evenings and weekends
- Generic AI tools like ChatGPT produce questions not grounded in his specific textbook, creating misalignment with what was actually taught
- After assessments, there is no automated performance summary — he manually tallies scores or relies on student self-reporting

### Currently Available Solutions
- Google Forms and WhatsApp PDFs are the default workflow but require fully manual question entry
- Kahoot and Quizizz are too gamified for formal assessments and not tied to any specific syllabus
- LMS platforms like Moodle or Canvas are too complex for small coaching institutes and require institutional IT infrastructure
- Generic AI tools produce broad-subject content — not scoped to a specific textbook, chapter, or page range

### How MorphEd Resolves This
- The 4-step assessment wizard (Basic Details → Content Selection → Generate Questions → Review & Publish) takes Rajan from zero to a published assessment in under 2 minutes
- RAG-based generation via Vertex AI retrieves content scoped to the exact chapter, topic, and page range he selects — every question is grounded in what he actually taught
- Post-submission analytics show class average, score distribution, and per-student rankings automatically — no manual work required

---

## Persona 3 — Student

**Name:** Manisha Shah
**Role:** HSC Year 2 (Commerce), Mumbai
**Age:** 17 | **Tech comfort:** High — daily user of Instagram, YouTube, and study apps

### User Journey
- Manisha receives access to MorphEd after Sunita has enrolled her in a batch; she logs in, sees published assessments, and takes them — no configuration required
- Her experience is entirely consumption-side: attempt, submit, view results, and track progress over time
- She has no control over which assessments are available or when they're published, but gets instant feedback after every submission

### Jobs to Be Done
- Complete assessments that reflect exactly what her professor taught, not generic exam prep content
- Receive immediate, objective feedback after submitting to know where she stands without waiting for manual grading
- Track performance across subjects over time to direct revision rather than studying everything uniformly

### Pain Points
- Most practice apps cover topics not aligned with her college's specific syllabus and module, making practice feel irrelevant
- After institute tests, she waits days for results — the grading gap creates anxiety and delays any corrective revision
- No visibility into which topics she is consistently weak on; she studies uniformly with no data to guide prioritisation

### Currently Available Solutions
- Unacademy, Testbook, and BYJU's serve board exam syllabi — not institute-specific modules
- Past-paper PDFs from her professor give practice but no feedback or performance tracking
- YouTube and notes are passive — they don't test retention or identify knowledge gaps

### How MorphEd Resolves This
- Assessments are built directly from Manisha's actual textbook content — not a generic curriculum — so every question reflects what was taught in her specific class
- Instant auto-grading on submission eliminates the result wait entirely: she sees her score and per-question correctness breakdown the moment she submits
- The personal analytics dashboard tracks average score, completion rate, and recent submissions — giving her data to direct revision rather than studying blindly
