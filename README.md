# Hospital CRM Documentation

This repository contains the business-requirement and product-requirement documentation for the clinic-focused Hospital CRM.

The repository is organized so a human or agent can enter at this README, move to the documentation index, and then follow the correct source hierarchy without guessing which file is authoritative.

## Project operating guidelines

Before doing product, engineering, implementation, validation, or release work, read:

**[Guidelines.md](Guidelines.md)**

This is the project's **active living operating baseline** for humans and AI agents. It consolidates the proven BRD/PRD-era working rules, records how those rules changed for engineering/implementation, and defines how the guidelines themselves may be revised when project evidence shows that a rule is not working well.

It is process authority, **not product authority**: the locked BRD and current accepted PRD baseline remain the governing sources for what the product must do.

## Agent lineage

The compact continuity-agent registry is **[docs/agents/AGENT-REGISTRY.md](docs/agents/AGENT-REGISTRY.md)**.

- Agent 1 — RETIRED
- Agent 2 — RETIRED
- Agent 3 — ACTIVE

Only continuity-owning higher agents receive sequential Agent numbers. Specialist Test/Evidence Runners use Test Evidence IDs and do not consume the lineage.

## Agent continuity

For a replacement agent or a brand-new chat continuing prior Hospital CRM work:

1. read **[Guidelines.md](Guidelines.md)**;
2. read the **[Agent Registry](docs/agents/AGENT-REGISTRY.md)** to identify the current/latest numbered continuity agent and retirement handoff;
3. use the historical **[Agent 1/2 Continuity Handoff](docs/AGENT-CONTINUITY-HANDOFF.md#agent-continuity-start)** when inherited Agent 1/2 reasoning/provenance is needed.

The historical handoff intentionally contains **both earlier agent generations**:

- **Agent One (retired):** the user-supplied retirement/replacement manual plus the full raw Hospital CRM conversation export;
- **Agent Two (continuation agent):** exact continuation-session user directives available in this chat, evidence labels/limits, repository-verified execution chronology, G13 reconciliation, G14/G15 completion, both validation passes, acceptance reconciliation, PR #2 merge, documentation reorganization, and the continuity work itself.

Use the anchor map below to jump directly to the relevant section instead of scrolling the 11k+ line handoff.

| Continuity section | Direct jump |
| --- | --- |
| Start / full handoff | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#agent-continuity-start) |
| Mandatory behavior | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-mandatory) |
| Repository/document state | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-repository-state) |
| Source-of-truth hierarchy | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-source-hierarchy) |
| User demeanor and working preferences | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-user-demeanor) |
| Decision-authority model | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-decision-authority) |
| Product-design principles | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-product-principles) |
| Core clinic/product model | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-core-product-model) |
| G1–G15 group model | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-group-model) |
| Per-group review template | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-group-review-template) |
| Persistent refinement workflow | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-refinement-workflow) |
| Four closure gates | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-closure-gates) |
| Retry/stale/concurrency/audit reasoning | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-safety-reasoning) |
| What Agent Two inherited/changed | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-agent-two-work) |
| Validated PRD state | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-validated-state) |
| Exact continuation-session user directives | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-user-directives) |
| Brand-new-chat resume checklist | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-resume-checklist) |
| Raw-source interpretation rules | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-raw-source-rule) |
| Appendix A — Agent One retirement manual | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-appendix-a-retirement-manual) |
| Appendix B — full Agent One raw conversation | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-appendix-b-agent-one-raw) |
| Appendix C — Agent Two continuation record | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-appendix-c-agent-two) |
| Agent Two evidence labels / limits | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-agent-two-evidence-labels) |
| Agent Two verbatim continuation directives | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-agent-two-verbatim-directives) |
| Agent Two repository-verified chronology | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-agent-two-execution-record) |
| Agent Two second audit / reconciliation | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-agent-two-validation) |
| Agent Two merge / docs organization | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-agent-two-merge-docs) |
| Agent Two retirement / exact resume state | [Open](docs/AGENT-CONTINUITY-HANDOFF.md#continuity-agent-two-retirement) |

**Authority warning:** the handoff is the behavior/provenance continuity layer. It does **not** replace the locked BRD or current PRD as product truth.

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

- **PRD:** v0.17 — ACCEPTED current V1 product baseline under controlled amendment
- **PRD Acceptance & Traceability:** v0.18 — ACCEPTED
- **Screen/IA companion:** v0.16 — ACCEPTED
- **Interaction companion:** v0.17 — ACCEPTED
- **PRD refinement:** G1–G15 complete
- **Open refinement reminders:** 0
- **Validation:** final validation plus a second independent G1–G15 revalidation PASS
- **Active post-baseline change control:** [Document 11](docs/PRD/11-prd-change-control-ledger.md)
- **Location:** [docs/PRD/](docs/PRD/)

The completed PRD refinement was merged into `main` through PR #2. The PRD is now an accepted current product baseline, not an informal draft. It may be amended only through controlled PRD change records; locked business behavior still remains governed by the BRD.

## Repository documentation structure

```text
.
├── README.md
└── docs/
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

The numeric prefixes are intentionally retained. They preserve the original reading sequence across the complete documentation set even though the files are now grouped into BRD and PRD folders.

## Source-of-truth hierarchy

Use this order when documents appear to overlap:

1. **Locked BRD documents** define the V1 business requirements, business rules, workflow decisions, roles, and approved scope.
2. **Current accepted PRD documents** define the active product behavior, including any later EFFECTIVE PRD-CHG amendments.
3. **Document 11 — PRD Change Control Ledger** governs substantive post-baseline PRD amendment and supersession.
4. **Documents 09–10** are closed historical refinement/reminder provenance; they do not independently authorize new product changes.
5. **Engineering contracts/implementation** must satisfy the current BRD/PRD rather than inventing missing business policy.

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
| [09 — PRD Refinement State Ledger](docs/PRD/09-prd-refinement-state-ledger.md) | **CLOSED historical** execution history for G1–G15 refinement, decisions, commits, validation, compatibility checks, reconciliations, and final audits. |
| [10 — PRD Future Review Reminders](docs/PRD/10-prd-future-review-reminders.md) | **CLOSED historical** refinement dependency register. REM-001–REM-074 are resolved; no new implementation-era REM IDs are added here. |
| [11 — PRD Change Control Ledger](docs/PRD/11-prd-change-control-ledger.md) | **ACTIVE** post-baseline product-change ledger for PRD clarifications, corrections, controlled product changes, evidence, attribution, validation, and supersession. |

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

1. Read [Guidelines.md](Guidelines.md) and start at [docs/README.md](docs/README.md).
2. For continuity ownership/provenance, check [docs/agents/AGENT-REGISTRY.md](docs/agents/AGENT-REGISTRY.md).
3. Identify whether the question is about **business policy**, **current product truth**, **engineering contract**, or **implementation detail**.
4. For business policy, read the relevant BRD files first.
5. For product work, read the relevant BRD source and current accepted PRD/change-control state before implementation.
6. Do not infer a new business rule from a UI detail, ledger note, technical convenience, or implementation constraint.
7. Do not rewrite history to make current state simpler. Important corrections, cancellations, replacements, approvals, PRD amendments, and audit events remain attributable.
8. If a conflict appears, investigate and reconcile it explicitly rather than silently overriding one document.
9. Preserve locked requirements unless an explicit change-control decision authorizes a BRD change.

## Documentation rule

Locked BRD requirements may only change through explicit BRD/business change control. The accepted PRD may change only through controlled amendment recorded in Document 11; only EFFECTIVE PRD-CHG records alter current product truth.

Documentation-only organization, navigation, path/status metadata, provenance, and formatting maintenance must preserve underlying validated product behavior.
