# Hospital CRM Documentation

This repository contains the business and product documentation for the clinic-focused Hospital CRM.

## Documentation status

**Baseline version:** v1.0  
**Status:** V1 Business Requirements — LOCKED  
**Date:** 2026-09-20

The V1 core business workflow is locked. Remaining items are explicitly classified as configuration, technical architecture, compliance, or future-scope dependencies and do not authorize silent changes to the locked business behavior.

## Documentation map

| Document | Purpose |
| --- | --- |
| [01 — Business Requirements Document](docs/01-business-requirements-document.md) | Main business requirements baseline covering objectives, scope, roles, functional requirements, business rules, and dependencies. |
| [02 — Workflows and State Model](docs/02-workflows-and-state-model.md) | End-to-end patient journey, queue lifecycle, prescription/pharmacy flow, and record lifecycle. |
| [03 — V1 Decision Register and Edge Cases](docs/03-open-decisions-and-edge-cases.md) | Canonical record of confirmed, derived/delegated, future, configuration, technical, and compliance decisions. |
| [04 — Requirements Traceability](docs/04-requirements-traceability.md) | Maps confirmed product decisions to BRD requirements and identifies future/out-of-scope items. |
| [05 — Product Requirements Document](docs/05-product-requirements-document.md) | DRAFT product specification derived from the locked BRD: users, workspaces, interaction behavior, feature requirements, product states, and release scope. |
| [06 — PRD Acceptance & Traceability](docs/06-prd-traceability-and-acceptance.md) | DRAFT product-level acceptance scenarios and traceability from PRD behavior back to the locked BRD. |
| [07 — Information Architecture & Screen Specification](docs/07-information-architecture-and-screen-specification.md) | DRAFT screen inventory, role navigation, screen contracts, visible states, actions, and transitions. |
| [08 — Interaction & Form Behavior Specification](docs/08-interaction-and-form-behavior-specification.md) | DRAFT interaction rules for forms, tables, queues, approvals, payments, inventory, errors, and common UI states. |

## Product boundary

The product currently covers the clinic journey from patient registration/retrieval through consultation, prescription, pharmacy dispensing, and payment recording.

The primary roles are:

- Owner
- Administrator
- Receptionist
- Doctor
- Pharmacist

One individual account may hold multiple roles, such as Owner + Doctor.

The initial product does **not** include laboratory management, inpatient/bed management, insurance processing, ambulance management, or HR/payroll.

## Core terminology

- **Patient ID** — permanent unique identifier for a patient.
- **Visit ID** — unique identifier for one patient visit/encounter.
- **Visit** — one clinic attendance and its associated operational/clinical flow.
- **Queue** — ordered set of visits waiting for or progressing through consultation.
- **Prescription** — medicines prescribed by the doctor for a visit.
- **Dispensing** — pharmacy action of providing available prescribed medicines to the patient.
- **Consultation payment** — payment transaction associated with the clinic consultation.
- **Pharmacy payment** — payment transaction associated with medicines dispensed by the clinic pharmacy.

## Documentation rule

Locked requirements may only change through explicit change control. Derived/delegated decisions remain identified as such; configuration and technical/compliance dependencies must not be used to silently alter V1 business behavior.


## PRD workstream

The initial PRD baseline has been merged into `main`. Further PRD refinement is performed in small, reviewable group branches and merged back through pull requests.

The locked BRD on `main` remains the business source of truth. PRD refinements may add product interaction detail but may not silently change locked business behavior.

Current refinement workstream: `prd/refine-group-01-workspace-navigation`.
