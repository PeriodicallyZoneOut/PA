# Maintenance Request Management — Detailed Project Plan

**Team:** Nguyễn Minh Tú (23120101), Huỳnh Mạnh Tường (23120105), Phạm Quốc Nam Anh (23120111).  
**Repository:** https://github.com/PeriodicallyZoneOut/PA.git  
**Planning baseline:** 7 October 2026.  
**Companion to:** [PA#1 proposal](maintainance-req-management-proposal-and-planning.md).

This working document expands implementation planning; it is not an additional section of the two-page PA#1 submission or the later feature specification. Dates are internal targets pending the official course calendar. All interviews, tests and results below are planned, not completed.

## 1. Product outcome and validation

Provide one place for residents to submit repairs, managers to approve and assign work, and technicians to report progress. The manager remains responsible for triage and assignment. The LLM reduces description-reading effort without controlling operations.

During 7–13 October, Tú will seek one building manager and two residents. Ask how requests arrive, how ownership is tracked, what information is missing, how urgency is decided and how residents obtain updates. Request permission before collecting examples. Record observations separately from assumptions; do not claim a general building-management need from a small sample.

Checkpoint 1 decision: retain the workflow if feedback supports the problem; otherwise document a narrower workflow. Lack of access triggers synthetic examples and a disclosed limitation. It does not count as successful user validation.

## 2. Work packages and ownership

| Work package | Accountable lead | Collaborator / reviewer | Expected evidence |
|---|---|---|---|
| User validation, scope, backlog | Nguyễn Minh Tú | Huỳnh Mạnh Tường | Interview notes or access limits, prioritized issues |
| Authentication, permissions, data model | Nguyễn Minh Tú | Phạm Quốc Nam Anh | Role/access tests, migration and seed scripts |
| Resident and technician screens | Huỳnh Mạnh Tường | Nguyễn Minh Tú | Working submission-to-resolution demo |
| Manager review and dashboard | Huỳnh Mạnh Tường | Phạm Quốc Nam Anh | Original text, suggestion, decision and history visible |
| LLM adapter and development evaluation | Phạm Quốc Nam Anh | Nguyễn Minh Tú | Versioned prompts, baseline and error report |
| Locked evaluation and usability pilot | Huỳnh Mạnh Tường | Phạm Quốc Nam Anh | Frozen fixtures, metrics, timed observations |
| Integration, setup and release | Nguyễn Minh Tú | Both teammates | Fresh-clone walkthrough and release checklist |

Owners coordinate delivery; they do not implement every component alone. A teammate reviews changes before merge. Keep tasks small enough to demonstrate at the twice-weekly team check-in. Record decisions, blocked dependencies and scope changes in repository issues. Use weekly demos to catch integration failures early.

## 3. Prioritized backlog

### Required for the final demonstration

- Provision demo accounts for all three roles; enforce record-level access on the server.
- Submit and view a request with optional private images; allow resident edits before assignment.
- Review a request, confirm category/priority, assign or reassign a technician.
- Record progress, comments and status history; resolve and request reopening with manager approval.
- Display basic counts and lists by status/assignee, using the same permissions as request views.
- Generate advisory triage with explicit loading/unavailable states, manager approval and override audit.
- Preserve request submission and manual processing during model failure.
- Provide reproducible setup, seeded examples, meaningful tests and an evaluation report.

### Defer first if the schedule slips

Visual dashboard refinements, extra filtering and optional convenience interactions. Preserve the core dashboard, request lifecycle, authorization, emergency boundary and evaluation. Do not silently drop the LLM objective: a manual-only release remains useful but fails that project objective and must be reported honestly.

### Explicit exclusions

Payments, inventory, procurement, multiple buildings, public registration workflows, automatic dispatch, emergency response, image interpretation, native apps and production availability promises. Seeded accounts are sufficient for the semester demo; account lifecycle expansion requires a documented scope change.

## 4. Architecture and data responsibilities

Use a React client, one Express backend, PostgreSQL and private file storage, packaged with Docker Compose. Keep the backend a single service for the semester. Browser requests go through backend authorization; neither the model API key nor private storage paths are exposed to clients.

Planned records include users/roles, requests, attachments, assignments, comments, status events, triage suggestions, manager decisions and model-usage records. Store the suggestion separately from the approved decision so an AI recommendation never becomes an operational fact implicitly.

Assignment and its history entry commit together. Detect competing edits instead of silently overwriting another manager's decision. The later specification will define precise transitions, error responses, image limits and edit-conflict behavior.

Persist a request before attempting AI triage. Track model processing independently from maintenance status. If processing fails, mark the suggestion unavailable while preserving the saved request. Any retry must be bounded and associated with the correct request version so stale results cannot replace newer decisions. Use a simple persistent pending-job mechanism if needed; no separate distributed queue is committed at this stage.

Private image access follows current request permissions. Validate uploads server-side. Demo data is synthetic or deidentified. The server sends only reviewed description text to the model; images and structured personal/location fields stay out of prompts. Exact authentication and retention settings belong in the later design review.

## 5. LLM behavior, limits and release decisions

The adapter returns a constrained category, urgency, service-team suggestion and brief explanation. Fixed allowed values and backend validation reject malformed or unsupported output. Treat report contents as untrusted data, including instructions embedded in descriptions. The model has no tools that can modify assignments, statuses or external systems.

The independent danger checkbox/text rules and configured emergency contacts are available without the model. A flag cannot be removed by a low-urgency suggestion. Review queues must not hide an unreviewed report because AI called it low priority. Keyword rules are incomplete and do not certify safety.

Version prompts, label policy, model snapshot and evaluation configuration. Record latency, token use, invalid responses and errors without unnecessarily logging personal text. A call cap, bounded retries and a token budget protect the $20 API allocation; reserve estimated maximum call cost before starting concurrent calls. Exhaustion disables further calls and preserves manual handling.

## 6. Evaluation protocol

### Dataset and labels

Nam Anh prepares 120 Vietnamese/English examples covering supported categories, urgency levels, ambiguity and prompt injection. Split by underlying scenario, not just wording, to avoid near-duplicate leakage: 80 development and 40 locked test examples, including 10 critical test examples. Document whether each example is synthetic or permitted/deidentified real data.

Define category, urgency and team labels before measuring performance. Tú reviews labels; seek a domain-informed reviewer for urgency and disclose if unavailable. Tường maintains the locked set and releases it only for the scheduled evaluation. Developer-created cases still limit independence even when prompt tuning cannot access the test set.

### Comparisons and measurements

- Compare a fixed keyword baseline and the LLM on the same cases, including cost and failure rate.
- Report category accuracy and routing accuracy separately, with targets of at least 85% each; report urgency confusion matrix and macro-F1, targeting at least 0.85.
- Report abstentions/manual-review responses and invalid outputs separately, and do not remove them from the denominator to inflate success.
- Measure raw-model critical under-triage separately from application warning coverage. Every critical test case must reach immediate human review in the application.
- When routing is effectively a category-to-team lookup, report that dependency rather than treating both scores as independent evidence.
- Preserve the first locked-test result. If it drives fixes, evaluate those fixes on fresh unseen cases and disclose the new sample size.

These targets are provisional project criteria. They are not evidence of real-world emergency reliability.

### Timed pilot

Seek three reviewers, ideally including the building manager. If using classmates, report that they are proxies rather than domain experts. Train participants on the same label policy. Give matched, nonrepeated reports in manual and assisted modes, counterbalancing mode order across participants where possible.

Measure time from opening the report to saving a decision, decision correctness and correction effort. Report participant-level and overall median times, critical errors and observed limitations. The goal is 30% lower median triage time without increasing critical errors. A small pilot cannot establish statistical or operational reliability. Investigate overrides against the agreed policy; they are feedback, not automatically correct labels.

## 7. Checkpoint execution and dependencies

| Work window | Checkpoint due | Work and exit condition |
|---|---|---|
| 7–14 Oct | 1: 14 Oct — Tú | Validate problem; document scope and labels; establish repository tasks and permitted data. Exit: assumptions and access limitations explicit. |
| 15–28 Oct | 2: 28 Oct — Nam Anh | Trial keyword/LLM approaches while Tú builds data/auth and Tường builds submission UI. Draft the later feature spec after validation. Exit: authenticated submission with private images and an initial error/cost report. |
| 29 Oct–11 Nov | 3: 11 Nov — Tường | Integrate assignment, comments, transitions, history, reopening and basic dashboard. Exit: full manual lifecycle demonstrated across all roles. |
| 12–25 Nov | 4: 25 Nov — Nam Anh | Connect suggestions, review decisions, independent flags, usage limits and fallback. Exit: AI outage cannot lose a submitted request or block manual assignment. |
| 26 Nov–9 Dec | 5: 9 Dec — Tường | Run locked evaluation, timed pilot and cross-role/concurrency/failure checks. Exit: reproducible report with failures and prioritized fixes. |
| 10–23 Dec | 6: 23 Dec — Tú | Resolve critical defects, rerun affected checks, document limitations and rehearse release. Exit: fresh-clone demo and final evidence package. |

Start AI feasibility before completing the whole web application. Complete the manual lifecycle before connecting AI decisions to its review interface. Dataset preparation and interface mockups can proceed alongside backend development. Align internal dates with the official calendar when supplied.

## 8. Quality checks and definition of done

A deliverable is done when its core behavior works in the integrated application, a teammate has reviewed it, relevant tests pass and setup/documentation reflect the change.

Critical checks include resident access to another resident's request/image being denied; technician access after reassignment; manager permission boundaries; competing assignments; consistent status/history writes; archived request history; invalid or timed-out AI output; danger flags despite low AI urgency; and usage-limit fallback.

Use Vitest for business rules and adapter validation, database integration checks for permissions/transactions, and Playwright for the three-role workflow. Do not replace real adapter evaluation with mocked outputs: mocks test application behavior, while labeled cases assess model quality.

The final release includes code, migrations, safe configuration examples, synthetic seeds, setup instructions, test commands, model/prompt versions, evaluation results and a recorded or live walkthrough. Exclude credentials and identifying resident information. Demonstrate from a clean local environment; hosted production deployment is outside the current commitment.

## 9. Risk response and change control

| Risk | Early action / owner | Trigger and response |
|---|---|---|
| Unsafe or ineffective triage | Nam Anh: 20 difficult cases and label policy this week; Tường: warning/review mockup | Any critical safeguard miss blocks enabling suggestions. Persistent quality failure prompts narrower supported cases and an honest unmet-objective report. |
| No realistic users or credible labels | Tú: invitations and permission requests this week | No access by checkpoint 1 triggers synthetic-data/proxy-user fallback and explicit limits on conclusions. |

Track delivery issues such as integration delays in the backlog without replacing these two principal risks. Change scope only after documenting the reason, affected checkpoint, owner and evaluation impact. An inaccessible model requires a documented provider/model substitution and re-evaluation, not assumed equivalent quality.

## 10. Submission boundary

The main proposal contains the six PA#1 rubric sections, team identities and repository link. This companion is for team execution and later refinement; do not append it to the two-page proposal to bypass the limit. Markdown has no fixed pagination, so the final submission layout still needs a two-page check without requiring PDF generation here.

Before submission, confirm official checkpoint alignment and create the PA-specific `SELF_ASSESSMENT_REPORT.md` with one evidenced row per criterion and a total matching the ZIP name. For this team the naming pattern is `23120101-23120105-23120111_<total>.zip`. Keep the PA AI log; do not reuse the IA self-assessment. No score or completed evaluation is claimed by this plan.·
