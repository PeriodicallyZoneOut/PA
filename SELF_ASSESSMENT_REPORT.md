# Self-assessment — PA#1: Proposal and planning

**Project:** Maintenance Request Management

Submitted by:

- 23120101 — Nguyễn Minh Tú
- 23120105 — Huỳnh Mạnh Tường
- 23120111 — Phạm Quốc Nam Anh

**Repository:** [PA Maintenance Request Management Project](https://github.com/PeriodicallyZoneOut/PA.git)

**Total we claim: 90 / 100**

**Submission filename:** `23120101-23120105-23120111_90.zip`

The scores below assess the proposal and planning work. Planned interviews, implementation and evaluation are not claimed as completed. All scored evidence is in the main proposal; the detailed working plan is not required to substantiate these scores.

| Criterion | Max | We claim | Evidence |
|---|---:|---:|---|
| Problem and users | 20 | 18 | [Proposal §1 — Product, users and problem](maintainance-req-management-proposal-and-planning.md#1-product-users-and-problem) identifies a single-building setting, named illustrative resident/manager/technician users, a leaking-sink scenario, a one-sentence problem and the assumed phone/Zalo/spreadsheet alternative. The setting and workaround still need user validation. |
| The LLM feature and the cost of being wrong | 25 | 23 | [Proposal §2 — One LLM feature and the cost of being wrong](maintainance-req-management-proposal-and-planning.md#2-one-llm-feature-and-the-cost-of-being-wrong) defines assisted triage, manager approval, affected residents/staff, a 30–60-minute or one-visit routing-error estimate, and irreversible danger scenarios. It provides independent warnings, a keyword baseline, a development/test split and separate model/safeguard evaluation. Error costs are estimates and the label policy still needs validation. |
| Scope: in and out | 15 | 14 | [Proposal §3 — Semester scope](maintainance-req-management-proposal-and-planning.md#3-semester-scope) lists the three-role workflow, private images, review/assignment, history, reopening, basic dashboard and advisory AI; explicitly excludes emergency dispatch, payments, inventory, multiple buildings and other expansions. Priorities and the local-demo boundary match §4. Workload estimates remain provisional. |
| Plan and ownership | 20 | 16 | [Proposal §4 — Six-checkpoint plan and ownership](maintainance-req-management-proposal-and-planning.md#4-six-checkpoint-plan-and-ownership) assigns each checkpoint deliverables, exit evidence, a date and a named accountable member; the team table identifies responsibilities and student IDs. Dates run from 14 October to 23 December 2026, but compatibility with the official course calendar is not yet confirmed, so we do not claim the top band. |
| Risks | 10 | 10 | [Proposal §5 — Two project-threatening risks](maintainance-req-management-proposal-and-planning.md#5-two-project-threatening-risks) identifies unsafe/unhelpful triage and lack of realistic users/labels. Both have named owners, actions during 7–13 October, and explicit fallback or failure consequences. These are planned mitigations that can start this week, as required by the rubric. |
| Technology choices | 10 | 9 | [Proposal §6 — Technology and operating budget](maintainance-req-management-proposal-and-planning.md#6-technology-and-operating-budget) connects frontend, backend, database, storage, packaging and testing choices to project needs. It names the model/provider, dated pricing source, token assumptions, $7.20 base estimate, $14.40 allowance and $20 budget with manual fallback. Some grouped tool justifications are brief; feasibility and actual token usage remain to be measured. |
| **Total** | **100** | **90** | **18 + 23 + 14 + 16 + 10 + 9 = 90.** |

## What we did not manage

We have not confirmed the six dates against the official course calendar. The proposal's final presentation has not been checked against the two-page limit; Markdown alone does not establish pagination. This must be resolved before submission, and the separate detailed plan must not be appended to bypass that limit.

The named users are illustrative personas. Interviews, permitted real examples, independent domain review and the evaluation dataset are still planned. The 30% time-saving target and accuracy targets are not achieved results. Error-cost estimates and API usage assumptions also need validation. PA#1 does not require a completed implementation or feature specification, and we claim no credit for either.

## What we would do differently

Confirm the course calendar and access to a building manager before fixing milestones. Test a small keyword/LLM baseline and agree on urgency labels early, then refine the scope using observed errors. Prepare the submission layout earlier while keeping implementation planning in a separate working document.

## AI assistance

OpenAI Codex assisted with drafting, rubric review, planning and this self-assessment. The students supplied the project choice, team identities and repository link. The assistance history is recorded in [AI-LOG.md](AI-LOG.md). The team should review the evidence and claimed score before submitting.
