# Hospital CRM Documentation

This repository contains the business and product documentation for the clinic-focused Hospital CRM.

## Documentation status

**Baseline version:** v0.1  
**Status:** Requirements baseline with open decisions  
**Date:** 2026-09-20

The current documentation captures the workflow and requirements explicitly confirmed during discovery. Unknown details are intentionally marked as TBD or Open Decision rather than being assumed.

## Documentation map

| Document | Purpose |
| --- | --- |
| [01 — Business Requirements Document](docs/01-business-requirements-document.md) | Main business requirements baseline covering objectives, scope, roles, functional requirements, business rules, and dependencies. |
| [02 — Workflows and State Model](docs/02-workflows-and-state-model.md) | End-to-end patient journey, queue lifecycle, prescription/pharmacy flow, and record lifecycle. |
| [03 — Open Decisions and Edge Cases](docs/03-open-decisions-and-edge-cases.md) | Unresolved questions and operational edge cases that must be decided before implementation is considered complete. |
| [04 — Requirements Traceability](docs/04-requirements-traceability.md) | Maps confirmed product decisions to BRD requirements and identifies future/out-of-scope items. |

## Product boundary

The product currently covers the clinic journey from patient registration/retrieval through consultation, prescription, pharmacy dispensing, and payment recording.

The initial roles are:

- Administrator
- Receptionist
- Doctor
- Pharmacist

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

A requirement is only treated as confirmed when it has been explicitly stated or approved. Recommendations, examples, and unresolved behavior remain separate until confirmed.
