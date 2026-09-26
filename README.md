# Hospital CRM Documentation

This repository contains the business-requirement and product-requirement documentation for the clinic-focused Hospital CRM.

The repository is organized so a human or agent can enter at this README, move to the documentation index, and then follow the correct source hierarchy without guessing which file is authoritative.

## Agent continuity

For a replacement agent or a brand-new chat continuing prior Hospital CRM work, use:

**[Agent Continuity Handoff](docs/AGENT-CONTINUITY-HANDOFF.md#agent-continuity-start)**

Key deep links:

- [How the next agent must behave](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-mandatory)
- [User demeanor/preferences](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-user-demeanor)
- [Decision-authority model](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-decision-authority)
- [Refinement workflow](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-refinement-workflow)
- [Agent Two continuation record](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-appendix-c-agent-two)
- [Retirement manual from Agent One](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-appendix-a-retirement-manual)
- [Full Agent One raw conversation](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-appendix-b-agent-one-raw)

The handoff contains both generations: Agent One's full raw source/retirement material and Agent Two's continuation directives, repository-verified execution record, validations, reconciliations, merge, and documentation reorganization.

For current product truth, continue from the documentation index below; the handoff is the behavior/provenance layer, not a replacement for BRD/PRD authority.


## Start here

**Documentation entry point:** [docs/README.md](docs/README.md)

Use that file as the canonical navigation index for the documentation tree, document purposes, reading order, source-of-truth rules, and agent guidance.

## Documentation status

### Business baseline

- **BRD baseline:** v1.0
- **Status:** V1 Business Requirements — LOCKED
- **Lock date:** 2026-09-20
- **Location:** [docs/BRD/](docs/BRD/)

The V1 core business workflow is locked. Remaining business-side items are explicitly classified as configuration, technical architecture, compliance, or future-scope dependencies and do not authorize silent changes to locked business behavior.

### Product baseline

- **PRD:** v0.17
- **PRD Acceptance & Traceability:** v0.18
- **PRD refinement:** G1–G15 complete
- **Open refinement reminders:** 0
- **Validation:** final validation plus a second independent G1–G15 revalidation PASS
- **Location:** [docs/PRD/](docs/PRD/)

The completed PRD refinement was merged into `main` through PR #2. PRD documents derive from the locked BRD and may add product behavior, screens, interactions, states, acceptance criteria, and implementation-facing detail, but they do not override locked business requirements.

## Repository documentation structure

```text
.
├── README.md
└── docs/
    ├── README.md
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
        └── 10-prd-future-review-reminders.md
```

The numeric prefixes are intentionally retained. They preserve the original reading sequence across the complete documentation set even though the files are now grouped into BRD and PRD folders.

## Source-of-truth hierarchy

Use this order when documents appear to overlap:

1. **Locked BRD documents** define the V1 business requirements, business rules, workflow decisions, roles, and approved scope.
2. **PRD documents** derive product behavior from the BRD and make the product implementation-ready.
3. **PRD refinement ledger/reminders** record how refinement decisions were reviewed, validated, reconciled, and closed. They provide provenance and continuity; they do not independently authorize changes to locked business behavior.
4. **Technical implementation** must satisfy the BRD and PRD rather than inventing missing business policy.

If a PRD statement appears to contradict the locked BRD, treat that as a defect to investigate. Do not silently choose the PRD over the BRD.

## Documentation map

### BRD

| Document | Purpose |
| --- | --- |
| [01 — Business Requirements Document](docs/BRD/01-business-requirements-document.md) | Main locked business baseline: objectives, scope, roles, functional requirements, business rules, dependencies, and change-control boundary. |
| [02 — Workflows and State Model](docs/BRD/02-workflows-and-state-model.md) | Locked end-to-end patient journey, queue lifecycle, prescription/pharmacy flow, record lifecycle, and business state transitions. |
| [03 — V1 Decision Register and Edge Cases](docs/BRD/03-open-decisions-and-edge-cases.md) | Canonical record of confirmed, delegated/derived, configuration, technical, compliance, future-scope, and edge-case decisions. |
| [04 — Requirements Traceability](docs/BRD/04-requirements-traceability.md) | Maps discovery/decision evidence to BRD requirements and business rules and identifies future/out-of-scope items. |

### PRD

| Document | Purpose |
| --- | --- |
| [05 — Product Requirements Document](docs/PRD/05-product-requirements-document.md) | Product behavior derived from the locked BRD: users, workspaces, feature requirements, states, safety behavior, reporting, printing, dependencies, and release scope. |
| [06 — PRD Acceptance & Traceability](docs/PRD/06-prd-traceability-and-acceptance.md) | Product acceptance scenarios and traceability from PRD behavior back to the locked BRD. |
| [07 — Information Architecture & Screen Specification](docs/PRD/07-information-architecture-and-screen-specification.md) | Screen inventory, role navigation, screen contracts, visible states, actions, and transitions. |
| [08 — Interaction & Form Behavior Specification](docs/PRD/08-interaction-and-form-behavior-specification.md) | Detailed interaction contracts for forms, tables, queues, approvals, payments, inventory, errors, stale state, retries, audit, printing, and common UI behavior. |
| [09 — PRD Refinement State Ledger](docs/PRD/09-prd-refinement-state-ledger.md) | Persistent execution history for the group-by-group PRD refinement, including decisions, commits, validation, compatibility checks, reconciliations, and final audit results. |
| [10 — PRD Future Review Reminders](docs/PRD/10-prd-future-review-reminders.md) | Forward-dependency register used during refinement. All recorded reminders are resolved in the completed refinement. |

## Product boundary

The product covers the clinic journey from patient registration/retrieval through consultation, prescription, clinic-pharmacy dispensing, billing/payment-status recording, inventory accountability, Owner exception control, reporting, audit/history, staff/configuration administration, and A4 physical outputs.

The primary roles are:

- Owner
- Administrator
- Receptionist
- Doctor
- Pharmacist

One individual account may hold multiple roles, such as Owner + Doctor, while actions remain attributable to the effective role/workspace.

The initial product does **not** include laboratory management, inpatient/bed management, insurance processing, ambulance management, or HR/payroll.

## Core terminology

- **Patient ID** — permanent unique identifier for a patient.
- **Visit ID** — unique identifier for one patient visit/encounter.
- **Visit** — one clinic attendance and its associated operational/clinical flow.
- **Queue** — ordered set of visits waiting for or progressing through consultation.
- **Prescription** — medicines prescribed by the Doctor for a Visit.
- **Dispensing** — pharmacy action of providing available prescribed medicines to the patient.
- **Consultation payment** — externally executed consultation payment whose status is recorded in the CRM.
- **Pharmacy payment** — externally executed pharmacy payment whose status is recorded in the CRM.

## Rules for agents and contributors

Before modifying or implementing behavior:

1. Start at [docs/README.md](docs/README.md).
2. Identify whether the question is about **business policy** or **product implementation/detail**.
3. For business policy, read the relevant BRD files first.
4. For product work, read the relevant BRD source before the PRD layer that implements it.
5. Do not infer a new business rule from a UI detail, ledger note, technical convenience, or implementation constraint.
6. Do not rewrite history to make current state simpler. Important corrections, cancellations, replacements, approvals, and audit events remain attributable.
7. If a conflict appears, investigate and reconcile it explicitly rather than silently overriding one document.
8. Preserve locked requirements unless an explicit change-control decision authorizes a BRD change.

## Documentation rule

Locked requirements may only change through explicit change control. Derived/delegated decisions remain identified as such; configuration and technical/compliance dependencies must not be used to silently alter V1 business behavior.

Documentation-only organization, navigation, and path maintenance must preserve the underlying validated BRD/PRD content.
