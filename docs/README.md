# Hospital CRM Documentation Index

This file is the **primary documentation entry point** for humans and agents working with the Hospital CRM repository.



## Active project guidelines

Before making changes, read the repository-level **[Guidelines.md](../Guidelines.md)**.

It is the current living operating baseline for product continuity, engineering, implementation, validation, and release work. It does not override the locked BRD or validated PRD; it defines **how** humans and AI agents should work with those sources and how the process itself may evolve when evidence shows a better rule is needed.

## Current continuity-agent lineage

See **[agents/AGENT-REGISTRY.md](agents/AGENT-REGISTRY.md)**.

- Agent 1 — RETIRED
- Agent 2 — RETIRED
- Agent 3 — ACTIVE

Only the higher continuity owner receives the sequential Agent number. Specialist Test/Evidence Runners produce evidence under Test Evidence IDs and are not part of the numbered lineage.

## Agent Continuity — replacement-agent entry point

If you are entering this repository from a new ChatGPT/agent conversation, start with:

**[Open the Agent Continuity Handoff](AGENT-CONTINUITY-HANDOFF.md#agent-continuity-start)**

The handoff is intentionally large because it preserves **source-level continuity rather than only a compressed summary**.

It contains both generations:

- **Agent One:** full retirement/replacement manual + full raw Hospital CRM conversation export;
- **Agent Two:** exact available continuation-session directives, evidence labeling, repository-verified execution chronology, reconciliations, validation passes, merge, docs reorganization, and the current continuity state.

### Complete stable anchor map

| Continuity section | Direct jump |
| --- | --- |
| Start / full handoff | [Open](AGENT-CONTINUITY-HANDOFF.md#agent-continuity-start) |
| Mandatory behavior | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-mandatory) |
| Repository/document state | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-repository-state) |
| Source-of-truth hierarchy | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-source-hierarchy) |
| User demeanor and working preferences | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-user-demeanor) |
| Decision-authority model | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-decision-authority) |
| Product-design principles | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-product-principles) |
| Core clinic/product model | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-core-product-model) |
| G1–G15 group model | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-group-model) |
| Per-group review template | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-group-review-template) |
| Persistent refinement workflow | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-refinement-workflow) |
| Four closure gates | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-closure-gates) |
| Retry/stale/concurrency/audit reasoning | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-safety-reasoning) |
| What Agent Two inherited/changed | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-agent-two-work) |
| Validated PRD state | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-validated-state) |
| Exact continuation-session user directives | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-user-directives) |
| Brand-new-chat resume checklist | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-resume-checklist) |
| Raw-source interpretation rules | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-raw-source-rule) |
| Appendix A — Agent One retirement manual | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-appendix-a-retirement-manual) |
| Appendix B — full Agent One raw conversation | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-appendix-b-agent-one-raw) |
| Appendix C — Agent Two continuation record | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-appendix-c-agent-two) |
| Agent Two evidence labels / limits | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-agent-two-evidence-labels) |
| Agent Two verbatim continuation directives | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-agent-two-verbatim-directives) |
| Agent Two repository-verified chronology | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-agent-two-execution-record) |
| Agent Two second audit / reconciliation | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-agent-two-validation) |
| Agent Two merge / docs organization | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-agent-two-merge-docs) |
| Agent Two retirement / exact resume state | [Open](AGENT-CONTINUITY-HANDOFF.md#continuity-agent-two-retirement) |

Use these anchors rather than reading the raw appendices linearly unless you need exact historical evidence.

**Authority warning:** this continuity handoff explains how prior agents reasoned and worked. It never overrides the locked BRD or current PRD.

Use it to determine:

- which folder contains the authoritative information you need;
- the correct reading order;
- which document governs business behavior versus product behavior;
- where screens, interactions, acceptance criteria, refinement history, and unresolved/resolved review dependencies are recorded;
- how to navigate without silently changing the locked V1 business baseline.

## Directory structure

```text
docs/
├── README.md
├── AGENT-CONTINUITY-HANDOFF.md
├── agents/
│   └── AGENT-REGISTRY.md
├── BRD/
│   ├── 01-business-requirements-document.md
│   ├── 02-workflows-and-state-model.md
│   ├── 03-open-decisions-and-edge-cases.md
│   └── 04-requirements-traceability.md
└── PRD/
    ├── 05-product-requirements-document.md
    ├── 06-prd-traceability-and-acceptance.md
    ├── 07-information-architecture-and-screen-specification.md
    ├── 08-interaction-and-form-behavior-specification.md
    ├── 09-prd-refinement-state-ledger.md
    ├── 10-prd-future-review-reminders.md
    └── 11-prd-change-control-ledger.md
```

The numeric prefixes remain intentionally global. Read `01` through `11` as one ordered documentation sequence, with the folder split clarifying whether each document belongs to the BRD or PRD layer.

---

# 1. Documentation layers and authority

## BRD — Business Requirements

Folder: [BRD/](BRD/)

The BRD layer defines **what the clinic/business requires**.

It is the authority for:

- V1 business scope;
- roles and role boundaries;
- business workflows;
- functional requirements;
- business rules;
- confirmed decisions and edge cases;
- business-state behavior;
- what is in scope versus future/out of scope;
- business-level change control.

**Current business baseline:** BRD v1.0 — LOCKED.

PRD or implementation work must not silently change these business rules.

## PRD — Product Requirements

Folder: [PRD/](PRD/)

The PRD layer defines **how the product implements the locked business requirements**.

It contains:

- product behavior and feature requirements;
- workspaces and navigation;
- role-aware interaction behavior;
- screens and information architecture;
- field/form/table/queue behavior;
- product states, errors, stale-state and retry safety;
- acceptance criteria;
- audit/history behavior;
- reporting behavior;
- printing/output behavior;
- refinement history, compatibility checks, and reminder resolution.

The PRD derives from the BRD. Where a genuine contradiction exists, investigate it as a defect; do not silently let the PRD override the locked BRD.

---

# 2. BRD documents

## [01 — Business Requirements Document](BRD/01-business-requirements-document.md)

**Role:** Primary locked business baseline.

Read this for:

- product/business objectives;
- V1 scope and exclusions;
- roles and responsibilities;
- functional requirements;
- business rules;
- dependencies;
- change-control boundary.

When a question is fundamentally “What must the clinic/business do?”, start here.

## [02 — Workflows and State Model](BRD/02-workflows-and-state-model.md)

**Role:** Locked workflow and business-state companion.

Read this for:

- reception → payment → queue → Doctor → prescription → pharmacy flow;
- Visit and queue state transitions;
- consultation lifecycle;
- prescription/pharmacy lifecycle;
- cancellation/void/history behavior;
- operational sequencing.

Use this when a requirement depends on **state transitions or end-to-end workflow order**.

## [03 — V1 Decision Register and Edge Cases](BRD/03-open-decisions-and-edge-cases.md)

**Role:** Canonical decision/edge-case register.

Read this for:

- confirmed V1 decisions;
- explicitly delegated/derived decisions;
- configuration decisions;
- technical dependencies;
- compliance dependencies;
- future/out-of-scope decisions;
- difficult edge cases and their accepted disposition.

Use this when you need to know **why a boundary exists or whether something was intentionally deferred/configured**.

## [04 — Requirements Traceability](BRD/04-requirements-traceability.md)

**Role:** Locked BRD traceability map.

Read this for:

- mapping of confirmed/delegated decisions to BRD requirements/business rules;
- decision provenance;
- confirmation that future/out-of-scope items were not accidentally promoted into V1.

Use this when validating **lineage from discovery/decisions into the locked BRD**.

---

# 3. PRD documents

## [05 — Product Requirements Document](PRD/05-product-requirements-document.md)

**Role:** Primary product-behavior specification.

Current PRD version: **v0.17**.

Read this for:

- user/workspace behavior;
- Patient, Visit, payment, queue, consultation, prescription, pharmacy, billing, inventory, Owner-control, administration, reporting, audit/safety, and printing requirements;
- product-state behavior;
- permissions and authority boundaries;
- release scope and product dependencies.

When the question is “What should the CRM product do?”, this is the primary PRD document after checking the relevant BRD source.

## [06 — PRD Acceptance and Traceability](PRD/06-prd-traceability-and-acceptance.md)

**Role:** Product validation and BRD-lineage companion.

Current version: **v0.18**, Parent PRD v0.17.

Contains:

- AC acceptance scenarios;
- RA role/authority scenarios;
- AU authentication scenarios;
- UXA interaction/UX scenarios;
- PRD-area traceability to the locked BRD.

The completed second independent audit established explicit acceptance coverage for **every P-001 through P-116**.

Use this when determining **how a product rule is proven/tested**.

## [07 — Information Architecture and Screen Specification](PRD/07-information-architecture-and-screen-specification.md)

**Role:** Screen and navigation contract.

Current version: **v0.16**, Parent PRD v0.17.

Contains:

- shared-shell screens;
- Reception screens;
- Doctor screens;
- Pharmacy screens;
- Owner screens;
- Administration screens;
- screen-level content, actions, restrictions, and states.

Use this when designing or implementing **what appears on each screen and which role can interact with it**.

## [08 — Interaction and Form Behavior Specification](PRD/08-interaction-and-form-behavior-specification.md)

**Role:** Detailed interaction contract.

Current version: **v0.17**, Parent PRD v0.17.

Contains:

- fields and forms;
- validation;
- buttons/final actions;
- queues;
- approvals;
- payment interactions;
- clinical/prescription behavior;
- dispensing/billing/inventory interactions;
- authentication;
- stale-state and retry behavior;
- audit/history safety;
- printing/physical-output behavior;
- common loading/error/permission states.

Use this when implementing **exact user interaction behavior**, especially when a screen-level description is not enough.

## [09 — PRD Refinement State Ledger](PRD/09-prd-refinement-state-ledger.md)

**Role:** CLOSED historical refinement execution history.

Contains:

- G1–G15 refinement state/history;
- source reviews;
- accepted product decisions;
- main/refinement/reconciliation commits;
- validation results;
- backward-compatibility checks;
- forward-impact analysis;
- final global validation;
- second independent G1–G15 revalidation.

This document is **provenance and continuity**, not a higher authority than the BRD/PRD requirements.

Use it when you need to understand **how a product decision was reached, validated, reconciled, or carried across groups**.

## [10 — PRD Future Review Reminders](PRD/10-prd-future-review-reminders.md)

**Role:** CLOSED historical cross-group dependency/reminder history.

Contains:

- REM-001 through REM-074;
- source group;
- target group;
- required future review;
- reason;
- final resolution/disposition.

The completed refinement has **0 OPEN reminders**.

Use it when auditing **cross-group dependencies and whether downstream review obligations were completed**.

## [11 — PRD Change Control Ledger](PRD/11-prd-change-control-ledger.md)

**Role:** ACTIVE post-baseline PRD controlled-amendment ledger.

Use this for substantive product findings discovered after the accepted baseline, including:

- PRD clarifications;
- PRD corrections;
- controlled product changes;
- BRD/business-policy escalation where required;
- Agent attribution;
- Change Package/Test Evidence linkage;
- validation/effective-commit provenance;
- supersession/reversal history.

Only an **EFFECTIVE** PRD-CHG modifies current accepted product truth.

---

# 4. Recommended reading order

## A. New agent onboarding

Read:

1. this `docs/README.md`;
2. [BRD/01](BRD/01-business-requirements-document.md);
3. [BRD/02](BRD/02-workflows-and-state-model.md);
4. [BRD/03](BRD/03-open-decisions-and-edge-cases.md);
5. [BRD/04](BRD/04-requirements-traceability.md);
6. [PRD/05](PRD/05-product-requirements-document.md);
7. [PRD/07](PRD/07-information-architecture-and-screen-specification.md);
8. [PRD/08](PRD/08-interaction-and-form-behavior-specification.md);
9. [PRD/06](PRD/06-prd-traceability-and-acceptance.md);
10. [PRD/09](PRD/09-prd-refinement-state-ledger.md) and [PRD/10](PRD/10-prd-future-review-reminders.md) when provenance/history is needed.

## B. Business-rule question

Read in this order:

1. relevant section of [BRD/01](BRD/01-business-requirements-document.md);
2. relevant workflow/state in [BRD/02](BRD/02-workflows-and-state-model.md);
3. relevant decision/edge case in [BRD/03](BRD/03-open-decisions-and-edge-cases.md);
4. [BRD/04](BRD/04-requirements-traceability.md) when provenance/traceability matters.

Do **not** decide new clinic policy from PRD implementation detail alone.

## C. Product/UX/implementation question

Read:

1. relevant BRD source first;
2. relevant P requirement in [PRD/05](PRD/05-product-requirements-document.md);
3. relevant screen in [PRD/07](PRD/07-information-architecture-and-screen-specification.md);
4. relevant interaction contract in [PRD/08](PRD/08-interaction-and-form-behavior-specification.md);
5. acceptance proof in [PRD/06](PRD/06-prd-traceability-and-acceptance.md).

## D. Why was a rule chosen?

Read:

1. current requirement in PRD/BRD;
2. relevant [PRD/09](PRD/09-prd-refinement-state-ledger.md) group history;
3. relevant [PRD/10](PRD/10-prd-future-review-reminders.md) reminder history where cross-group impact was involved.

## E. QA/test planning

Start with:

1. [PRD/06](PRD/06-prd-traceability-and-acceptance.md);
2. trace each scenario back to [PRD/05](PRD/05-product-requirements-document.md);
3. use [PRD/07](PRD/07-information-architecture-and-screen-specification.md) and [PRD/08](PRD/08-interaction-and-form-behavior-specification.md) for exact UI/state behavior;
4. use BRD documents to verify business-rule intent.

---

# 5. Rules future agents must preserve

1. **BRD first for business behavior.** The locked BRD is the V1 business source of truth.
2. **PRD derives; it does not silently override.**
3. **Do not restart discovery** when a decision is already locked and documented.
4. **Do not invent business policy** to fill an implementation gap.
5. **Distinguish business decisions from product/UX/technical decisions.**
6. **Preserve important history.** Corrections, replacements, cancellations, voids, approvals, rejected/stale work, audit events, and prior revisions remain attributable where the product specifies.
7. **Respect role and authority boundaries**, including effective role/workspace for multi-role users.
8. **Treat current state and historical state separately.**
9. **Use explicit final actions for high-impact state changes.**
10. **Revalidate current authority and state** before protected/state-changing actions where the product requires it.
11. **Do not silently resolve stale conflicts** by overwriting, clipping, merging, or reinterpreting history.
12. **Use acceptance criteria as verification**, not as permission to contradict BRD/PRD requirements.
13. **Use Documents 09–10 as historical provenance**, not as active implementation/change-control ledgers.
14. **Use Document 11 for substantive post-baseline PRD amendment.**
15. If a genuine conflict is discovered, **reconcile it explicitly** and preserve the evidence/history of the correction.

---

# 6. Current validated state

The completed V1 PRD refinement has:

- G1–G15 complete;
- 116 product requirements, P-001 through P-116;
- explicit acceptance coverage for every P requirement;
- 49 unique screen contracts;
- interaction contracts through Section 45;
- REM-001 through REM-074 resolved;
- 0 open reminders;
- 0 unresolved cross-group conflicts at final validation;
- a completed second independent G1–G15 revalidation.

This index describes the accepted V1 product baseline and its governance model. Any future change should first be classified as a BRD/business change, PRD controlled amendment, engineering-contract change, implementation defect, configuration/compliance dependency, Test/Evidence issue, or future-scope proposal.

---

# 7. Change discipline

Moving or reorganizing files must not alter their validated substantive content.

If file paths change:

- update navigation/index links;
- update literal internal path references where necessary;
- preserve document IDs, requirement IDs, reminder IDs, decision IDs, and substantive wording;
- verify that all links resolve after the move.

Business behavior changes require explicit BRD/business change control. Substantive post-baseline product changes use Document 11; only EFFECTIVE PRD-CHG records modify current accepted product truth. Documentation-only metadata/navigation/provenance work must not silently change validated product behavior.
