# PA#1 — Maintenance Request Management

**Team:** Three members · **Planning starts:** 7 October 2026  
**Project repository:** [INSERT YOUR GIT REPOSITORY URL HERE]

| Member | Student ID | Primary responsibility |
|---|---|---|
| Nguyễn Minh Tú | 23120101 | Product validation, backend/database and release integration |
| [Member 2 — full name] | [Student ID] | Frontend, user workflows and usability testing |
| [Member 3 — full name] | [Student ID] | LLM integration, evaluation dataset and model testing |

## 1. Problem and users

**Problem:** Residents and building managers need one place to report, triage and track maintenance requests because scattered messages make responsibility and progress difficult to follow.

The setting is one apartment building. Illustrative users are **Minh**, reporting a leaking sink; **Lan**, the manager reviewing and assigning reports; and **Hùng**, the technician recording repairs. The assumed workaround is phone/Zalo messages plus a spreadsheet, leaving residents asking for updates and managers manually tracking ownership. These hypotheses will be checked in interviews, not presented as established findings.

The workflow is **submit → manager review → assign → work/comments → resolved**, with requests to reopen. Success means traceable ownership and status, targeting 30% lower median triage time without increasing critical errors in the evaluation.

## 2. LLM feature and cost of being wrong

**One feature:** Interpret Vietnamese/English descriptions into suggested **category, urgency and service team**, with a short explanation. Categories cover plumbing, electrical, HVAC, building/general and other; urgency is low, normal, high or immediate human review. Ambiguity requires manual review. Compare against keyword rules to test whether language interpretation adds value.

The manager sees the original text, confirms or overrides suggestions and selects a technician. Only the saved human decision becomes operational; the LLM has no workflow-changing permissions. Overrides retain the suggestion, decision and reason.

**Emergency boundary:** This is not emergency dispatch. The reporting screen directs immediate danger to configured emergency contacts. A danger checkbox and independent text rules flag reports such as “Strong gas smell in kitchen”; model suggestions cannot remove the warning. All unreviewed requests stay visible, with flags highlighted. Missing keywords do not establish safety, and manager review cannot guarantee an immediate response.

**Wrong-output costs:** A wrong team may waste 30–60 minutes or one visit; excessive urgency consumes staff attention; understated urgency delays residents' repairs. These are estimates to validate. Reassignment corrects routing but cannot recover lost time. A missed dangerous report may contribute to irreversible injury or property damage; manager override alone is insufficient protection.

**Evaluation:** Label 120 synthetic/deidentified reports: 80 development and 40 locked test cases, including 10 critical cases in the test set. Cover Vietnamese/English, ambiguity and malicious instructions; seek independent label review and disclose its absence. Target ≥85% accuracy separately for category/routing and urgency macro-F1 ≥0.85. Measure raw-model under-triage separately; all critical test cases must trigger application-level immediate review. Critical misses block enabling suggestions. After test-driven fixes, use fresh unseen cases rather than claiming the old set remains held out. Pilot manual versus assisted triage using comparable, nonrepeated cases and counterbalanced order; report timing, errors and participant limits. Investigate overrides rather than treating them as ground truth. Passing a small test does not establish operational safety.

## 3. Semester scope

**In:** One building; authenticated resident, manager and technician roles; request creation, viewing and edits before assignment; optional image attachments; manager review, assignment/reassignment; comments; status history; resolution and manager-approved reopening; workload/status dashboard; advisory LLM triage. Residents access their own requests, technicians their assignments, and managers all building requests. Server authorization applies to images too. Requests are archived rather than erasing their audit history.

**Out:** Autonomous emergency response, automatic assignment, image diagnosis, payments, inventory, contractor procurement, predictive maintenance, multiple buildings, native mobile apps and 24/7 operational guarantees. The semester deliverable is a reproducible demonstration, not a live emergency service.

## 4. Plan, checkpoints and ownership

The plan starts **7 October 2026**. Dates below are proposed completion targets, not verified course deadlines; align them with the official calendar when available. Each checkpoint has one accountable owner, supported by the others in their roles above. Replace Member 2/3 with their full names before submission to meet the rubric's named-owner requirement. Detailed feature specifications come after idea validation, not with this proposal.

| Checkpoint | Date (2026) | Deliverable and completion evidence | Owner |
|---|---|---|---|
| 1 — Validate and plan | 14 Oct | Seek one manager/two resident interviews; record access limits; define labels, scope and backlog | Nguyễn Minh Tú |
| 2 — Feasibility and core | 28 Oct | After validation: core-feature spec, keyword/LLM trial; team demonstrates schema, roles, requests and private images | [Member 3 — full name] |
| 3 — Maintenance lifecycle | 11 Nov | Assignments, comments, history, reopening and dashboard; demonstrate complete manual workflow | [Member 2 — full name] |
| 4 — AI integration | 25 Nov | Advisory suggestions, validation, danger flags, manual fallback and override audit; development-set results | [Member 3 — full name] |
| 5 — Evaluation | 9 Dec | Locked-test results, timed comparison, permission/concurrency/failure tests; prioritize fixes | [Member 2 — full name] |
| 6 — Release | 23 Dec | Fix critical defects; reproducible setup, demo, evaluation limitations and final documentation | Nguyễn Minh Tú |

## 5. Two project-threatening risks

1. **Unsafe triage or excessive trust in AI.** Starting 7–13 October, Member 3 drafts the emergency policy and collects 20 danger/ambiguous examples; Member 2 prototypes the independent warning and review screen. Evaluate model errors and safeguard failures; disable suggestions if critical failures persist. Manual processing remains available.
2. **Insufficient access to users and realistic labels.** Starting 7–13 October, Nguyễn Minh Tú invites one manager and two residents to review the workflow and requests anonymized examples with permission. If access fails, use documented synthetic cases and peer review, narrow claims to a prototype, and report missing operational validation.

## 6. Technology choices and budget

| Choice | Project-specific reason |
|---|---|
| React + Vite + Tailwind CSS | Shared forms and role-specific dashboards keep the three user workflows manageable. |
| Node.js + Express | Centralizes authorization, workflow transitions and server-only model calls. |
| PostgreSQL + Prisma | Relational assignments and transactions keep status changes and audit entries consistent. |
| Private filesystem storage + Docker Compose | Supports authenticated image retrieval and a reproducible local demonstration without a paid storage dependency. |
| OpenAI GPT-4.1 mini, snapshot `gpt-4.1-mini-2025-04-14` | A provisional baseline for short classification, supporting schema-constrained output; retain it only if evaluation passes. |

Save requests before calling the model. Validate output against allowed values; timeout, refusal, invalid output or budget exhaustion produces “manual review required.” Treat descriptions as untrusted data. Send description text only, excluding images and structured identity/location fields. Use synthetic or deidentified demo data; typed personal information requires review/redaction before API submission. Keep API keys on the server.

Budget uses rates checked on **6 October 2026**: **$0.40/million input tokens, $1.60/million output tokens** ([OpenAI documentation](https://developers.openai.com/api/docs/models/gpt-4.1-mini)). At 1,000 input + 200 output tokens, a call costs **$0.00072**; 10,000 calls cost **$7.20**, or **$14.40** with a 2× testing/retry allowance. Reserve **$20**, excluding taxes/hosting; meter usage and stop calls at the application budget limit, falling back to manual review. Recheck rates before implementation. The reproducible local demo needs no paid cloud hosting.
