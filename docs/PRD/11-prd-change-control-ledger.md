# Hospital CRM — PRD Change Control Ledger

## Document Control

| Field | Value |
| --- | --- |
| Document | PRD Change Control Ledger |
| Status | ACTIVE |
| Established | 2026-09-26 |
| Business baseline | BRD v1.0 — LOCKED |
| Product baseline | PRD v0.17 + accepted companion Documents 06–08 |
| Prior refinement history | Documents 09–10 — CLOSED historical process records |
| Current continuity owner | Agent 3 |
| Next PRD change ID | PRD-CHG-001 |

---

# 1. Purpose

This is the active post-baseline ledger for substantive PRD amendments discovered during engineering, implementation, testing, integration, or later product work.

The PRD is an **accepted current product baseline under controlled amendment**.

It is not an untouchable fossil, and it is not an informal living document that agents may rewrite whenever implementation becomes inconvenient.

The current accepted PRD remains effective until a recorded PRD change reaches **EFFECTIVE** status.

This ledger preserves:

- what triggered the proposed change;
- whether the issue is clarification, correction, or a real product change;
- which product sources are affected;
- whether BRD/business policy is implicated;
- which continuity agent reconciled it;
- what evidence/Change Package/Test Evidence supported it;
- how it was validated;
- the commit where the new product truth became effective;
- downstream impacts and supersession history.

---

# 2. Authority boundary

The hierarchy remains:

1. BRD v1.0 — LOCKED business truth.
2. Current accepted PRD baseline, including all EFFECTIVE PRD-CHG records.
3. Engineering contracts.
4. Executable implementation.

A PRD amendment may clarify or correct product behavior without reopening business policy when the locked BRD and accepted product principles provide a safe basis.

If a proposed PRD change would change clinic/business policy, it cannot become EFFECTIVE through PRD change control alone. It must return to BRD/business change control and user/clinic authority where required.

Code convenience is never sufficient justification to change product truth.

---

# 3. Change classifications

## CLARIFICATION

Use when intended product behavior is already supported by governing sources but wording/coverage is ambiguous or incomplete.

A clarification must not change the intended business/product outcome.

## CORRECTION

Use when the accepted PRD contains a real contradiction, omission, or derived-product defect and reconciliation is required to restore coherent behavior compatible with the BRD.

The continuity agent may normally reconcile this autonomously when no new clinic policy is introduced.

## PRODUCT CHANGE

Use when the accepted product behavior itself is intentionally changing.

Determine whether the change:

- remains a derivable product-design change inside the locked BRD; or
- changes business/clinic policy.

If it changes business/clinic policy, move to BRD/business change control and obtain the required authority before the PRD is updated.

---

# 4. Status lifecycle

A PRD change uses these statuses:

- **PROPOSED** — finding has been recorded but not yet fully assessed.
- **IN REVIEW** — source/impact analysis is active.
- **NEEDS BUSINESS DECISION** — cannot proceed without genuine business/clinic authority.
- **APPROVED FOR RECONCILIATION** — decision is resolved and document updates may be applied.
- **EFFECTIVE** — affected canonical documents are committed and validation passes.
- **REJECTED** — evidence did not justify changing product truth.
- **WITHDRAWN** — proposal no longer applies before becoming effective.
- **SUPERSEDED** — a later effective PRD change replaced this effective product decision.

Only **EFFECTIVE** entries modify current accepted product truth.

---

# 5. Required PRD change record

Each PRD-CHG entry should record, where applicable:

- ID;
- title;
- status;
- classification;
- date raised;
- trigger/finding;
- raised by / evidence source;
- continuity agent responsible;
- Change Package ID;
- Test Evidence ID(s);
- affected P requirements;
- affected acceptance IDs;
- affected screen/interaction contracts;
- affected BRD FR/BR/OD sources;
- previous effective product truth;
- proposed/new effective product truth;
- business-policy impact assessment;
- engineering-contract impacts;
- completed-package impacts;
- validation performed;
- user/business decision reference if required;
- effective commit;
- supersedes / superseded by;
- forward impacts / required follow-up.

Do not invent IDs or evidence links that do not exist.

---

# 6. Change process

1. A finding appears during product work, implementation, integration, testing, release validation, or later operation.
2. The continuity agent first decides whether it is:
   - implementation defect;
   - Test/Evidence defect;
   - engineering-contract defect;
   - PRD issue;
   - BRD/business-policy issue;
   - environment/tooling issue.
3. If product truth is affected, create the next PRD-CHG entry.
4. Read the relevant BRD, PRD, acceptance, screen, interaction, and historical provenance.
5. Classify the PRD change.
6. Map backward compatibility and downstream impact.
7. If genuine business policy is affected, stop at NEEDS BUSINESS DECISION until authorized.
8. Reconcile every affected current product layer; do not patch only one sentence.
9. Update acceptance coverage whenever required.
10. Validate the committed result.
11. Run cumulative compatibility checks against affected completed behavior.
12. Record downstream engineering/Change Package impacts.
13. Mark the PRD-CHG EFFECTIVE only after the new canonical product truth is committed and validation passes.

If validation fails, the change remains open; do not mark it effective because the intended edit looked correct.

---

# 7. Test evidence rule

A Test/Evidence Runner may trigger a PRD review, but its verdict does not itself change product truth.

For ordinary implementation failures, a structured pasted test result may be sufficient to begin triage.

Before using a testing finding to justify a PRD amendment, architecture/security change, destructive migration, business-rule change, or release-blocking product conclusion, the continuity agent should obtain enough underlying evidence to reproduce or independently validate the finding.

Examples include:

- screenshots/video;
- raw logs;
- console/network output;
- exact tested commit/environment;
- reproduction steps;
- artifact ZIP;
- equivalent direct evidence.

---

# 8. Supersession and reversal

Do not erase an effective historical PRD change merely because the project later reverses direction.

If a later decision replaces it:

- create a new PRD-CHG;
- explain why;
- set the older entry to SUPERSEDED when the new change becomes EFFECTIVE;
- preserve both historical records;
- update current product documents to the latest effective truth.

This follows the project's general rule:

> Historical truth remains; current effective truth may change.

---

# 9. Documentation-only changes that do not require a PRD-CHG

A PRD-CHG is not required for changes that do not alter substantive product behavior, such as:

- correcting stale lifecycle/status metadata;
- fixing navigation links/paths;
- formatting;
- typo-only fixes;
- adding proven provenance/agent attribution;
- reorganizing documentation without substantive change.

Such work must still be attributable through Git and must not be mislabeled as substantive product change.

If there is doubt whether wording changes product meaning, treat it as a PRD change and review it explicitly.

---

# 10. Baseline initialization

The original group-by-group refinement completed G1–G15 with:

- P-001 through P-116;
- explicit acceptance coverage for every P requirement;
- 49 screen contracts;
- interaction sections 1–45;
- REM-001 through REM-074 resolved;
- 0 open reminders;
- 0 unresolved cross-group conflicts;
- final validation plus a second independent G1–G15 audit PASS.

Documents 09 and 10 retain the historical refinement/reminder process.

Agent 3 established this post-baseline change-control ledger during product-baseline lifecycle cleanup. That setup is documentation/process governance and does **not** itself change V1 product behavior.

No substantive post-baseline PRD amendment is active at ledger creation.

---

# 11. Active PRD Change Register

| ID | Status | Classification | Title | Trigger / evidence | Continuity agent | Effective commit |
| --- | --- | --- | --- | --- | --- | --- |

No PRD-CHG entry has been raised yet.

---

# 12. Closed / Superseded PRD Changes

None at ledger creation.
