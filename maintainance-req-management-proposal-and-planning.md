# PA#1 — Maintenance Request Management

**Planning period:** 7 October–23 December 2026  
**Repository:** [PA Maintenance Request Management Project](https://github.com/PeriodicallyZoneOut/PA.git)

| Team member | Student ID | Responsibility |
|---|---|---|
| Nguyễn Minh Tú | 23120101 | User validation, backend/database, release integration |
| Huỳnh Mạnh Tường | 23120105 | Frontend, user workflows, usability testing |
| Phạm Quốc Nam Anh | 23120111 | LLM integration, evaluation data, model testing |

## 1. Product, users and problem

**Problem:** In one apartment building, scattered maintenance messages make it difficult for residents to track progress and for the manager to track responsibility.

Illustrative users are **Minh**, reporting a leaking sink; **Lan**, the manager reviewing reports; and **Hùng**, the technician completing repairs. Their assumed workaround is phone/Zalo messages plus a spreadsheet. Interviews with one manager and two residents will validate this assumption; no interview findings are claimed yet.

The web app provides **submit → review → assign → work/comments → resolve**, with manager-approved reopening. Its value is a shared record of ownership, progress and decisions. The target is 30% lower median triage time with AI assistance, without increasing critical errors in the pilot.

## 2. One LLM feature and the cost of being wrong

**Assisted triage:** Interpret Vietnamese/English descriptions into suggested category, urgency and service team, with a short explanation. Categories cover plumbing, electrical, HVAC, building/general and other. Unclear reports require manual review. Compare against keyword rules to establish whether language interpretation adds value.

The manager sees the original text and approves or corrects suggestions before selecting a technician. The model cannot assign work or change statuses; the system records suggestions, final decisions and override reasons.

**Error costs:** Wrong routing may waste an estimated 30–60 minutes or one visit; reassignment corrects the route but cannot recover that time. Excessive urgency diverts staff attention; understated urgency delays residents' repairs. A missed dangerous report can contribute to irreversible harm. These are scenarios to validate, not measured outcomes.

**Emergency boundary:** The app is not emergency dispatch. Show configured emergency contacts; a resident danger checkbox and independent text rules flag reports such as “Strong gas smell in kitchen.” Model output cannot remove warnings. All unreviewed requests remain visible. Neither missing keywords nor manager review guarantees safety or immediate response.

**Evaluation:** Prepare 120 labeled reports: 80 development and 40 locked test cases, including 10 critical test cases. Seek independent label review. Target ≥85% category and routing accuracy separately and urgency macro-F1 ≥0.85. Report raw-model errors separately from application safeguards; every critical test case must enter immediate human review. Critical misses block enabling suggestions. Use fresh unseen cases after test-driven fixes. Compare manual/assisted review on matched, nonrepeated cases with counterbalanced order; report timing, errors and participant limitations. Overrides prompt investigation, not automatic relabeling.

## 3. Semester scope

**In:** One building; resident/manager/technician authentication and server-side authorization; requests with private images; manager review and assignment/reassignment; comments; status history; resolution/reopening; basic workload dashboard; advisory triage. Residents see their own requests, technicians their assignments, managers all building requests. Archive requests without deleting history. Save submissions even when AI fails; use manual review on timeout, invalid output or budget exhaustion.

**Out:** Autonomous emergency response, automatic dispatch, image diagnosis, payments, inventory, procurement, multiple buildings, native mobile apps and 24/7 service guarantees. Deliver a reproducible local demonstration. Prioritize the complete manual workflow and evaluated triage over dashboard polish.

## 4. Six-checkpoint plan and ownership

Work starts **7 October 2026**. Dates are proposed internal completion targets; official calendar compatibility remains to be confirmed. Owners coordinate contributions from all three roles. Detailed specifications follow idea validation.

| Checkpoint | Due (2026) | Deliverable / exit evidence | Accountable owner |
|---|---|---|---|
| 1 — Validate | 14 Oct | Interview findings or access limits; scope, labels, backlog and repository setup | Nguyễn Minh Tú |
| 2 — Prove feasibility | 28 Oct | Later feature spec; keyword/LLM trial; working authenticated request submission with private images | Phạm Quốc Nam Anh |
| 3 — Manual workflow | 11 Nov | Demonstrate assignment, comments, history, resolution/reopening and dashboard | Huỳnh Mạnh Tường |
| 4 — Integrate AI | 25 Nov | Reviewed suggestions, warnings, override audit and outage fallback; development results | Phạm Quốc Nam Anh |
| 5 — Evaluate | 9 Dec | Locked-test metrics, timed pilot, authorization/concurrency/failure tests; prioritized fixes | Huỳnh Mạnh Tường |
| 6 — Release | 23 Dec | Critical fixes; fresh-clone setup, seeded demo, evaluation report and final documentation | Nguyễn Minh Tú |

## 5. Two project-threatening risks

1. **Unsafe or unhelpful triage:** During 7–13 October, Nam Anh prepares 20 danger/ambiguous cases and a label policy; Tường prototypes warnings and mandatory review. Test safeguards separately from model accuracy. Persistent critical misses disable suggestions and require reporting that the AI objective was not achieved.
2. **No realistic users or labels:** During 7–13 October, Tú invites one manager/two residents and requests permitted anonymized examples. If unavailable by checkpoint 1, use documented synthetic cases and peer review, limit claims to a prototype and record missing operational validation.

## 6. Technology and operating budget

| Choice | Project-specific justification |
|---|---|
| React, TypeScript, Vite, Tailwind CSS | Shared forms and typed role-specific workflows |
| Node.js, Express, TypeScript | Centralized permissions, transitions and server-only model calls |
| PostgreSQL, Prisma | Relational assignments and transactional history updates |
| Private file storage, Docker Compose | Protected attachments and reproducible local setup |
| Vitest, Playwright | Business-rule checks and complete role-based workflow tests |
| OpenAI GPT-4.1 mini, pinned snapshot `gpt-4.1-mini-2025-04-14` | Provisional structured-output baseline, retained only if evaluation supports it |

Send deidentified description text only, excluding images and identity/location fields; review typed personal data before API submission. Validate model output and keep credentials server-side.

Rates checked **6 October 2026**: $0.40/M input and $1.60/M output tokens ([source](https://developers.openai.com/api/docs/models/gpt-4.1-mini)). At 1,000 input + 200 output tokens, 10,000 calls cost **$7.20**, or **$14.40** with a 2× testing/retry allowance. Reserve **$20**, excluding taxes/hosting; enforce a usage cutoff and manual fallback. Recheck prices before implementation. The local demo requires no paid cloud hosting.
