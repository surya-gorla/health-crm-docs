# Hospital CRM — Requirements Traceability

## 1. Purpose

This document maps the confirmed discovery decisions to the BRD requirements/business rules and to the active decision register.

The current baseline is v0.3.

---

# 2. Source Classification

The current baseline is derived from:

1. the original clinic workflow described by the product owner;
2. explicitly accepted refinements from the discovery discussion;
3. grouped decisions supplied for patient identity, queue behavior, clinical UX, prescription, pharmacy, inventory, payments, permissions, authentication, pilot deployment, and reporting;
4. pharmacy UX/edge-case decisions explicitly delegated for product-design resolution.

Documentation rule:

- confirmed information becomes a BRD requirement or business rule;
- delegated design decisions are recorded as such;
- unresolved information remains OPEN/TBD;
- safety-critical inventory behavior is constrained so expired stock cannot be dispensed;
- suggestions are not silently presented as user-confirmed facts.

---

# 3. Traceability Matrix

| Discovery Decision / Source | Classification | BRD / Decision Mapping |
| --- | --- | --- |
| Permanent Patient ID | CONFIRMED | FR-002, FR-003, BR-001 |
| Search by Patient ID, phone, and name | CONFIRMED | FR-001, BR-002 |
| Longitudinal patient archive across visits is a core product need | CONFIRMED | FR-027, FR-091, BR-038, BO-09 |
| Inventory-loss/theft visibility and prevention is a core business need | CONFIRMED | FR-068, FR-090, BR-031, BR-037, BO-08 |
| DOB is entered; age is derived | CONFIRMED | FR-075, OD-001 |
| Full name, phone, DOB, gender, address, email required | CONFIRMED | FR-075, OD-001 |
| Emergency contact, blood group, allergies, guardian/parent optional | CONFIRMED | FR-076, OD-001 |
| Government ID optional, not required | CONFIRMED | FR-077, OD-001 |
| Shared family phone numbers allowed | CONFIRMED | FR-078, BR-025, OD-003 |
| Similar patient candidates reviewed with patient using prior visit context if useful | CONFIRMED | FR-004, BR-005, OD-002 |
| If patient cannot confirm candidate, create normal profile marked Possible Duplicate | CONFIRMED | FR-004, FR-079, BR-005, OD-002 |
| Duplicate merge not required in current V1 | CONFIRMED CURRENT SCOPE | FR-079, OD-002 |
| Separate Visit ID per clinic attendance | CONFIRMED | FR-008 to FR-010, BR-003, BR-006 |
| Payment status is recorded; CRM does not process payment | CONFIRMED | FR-011 to FR-016, FR-060 to FR-063, BR-033, OD-004, OD-018 |
| No partial consultation payment | CONFIRMED | FR-013, BR-021, OD-004 |
| Unpaid consultation cannot enter queue | CONFIRMED | FR-016, BR-010, OD-004 |
| Consultation payment is non-refundable in V1 | CONFIRMED | BR-021, BR-050, OD-019 |
| Pharmacy payment has no partial payment and no refunds in V1 | CONFIRMED | FR-061, BR-047, BR-050, OD-019 |
| Pharmacy bill cancellation/void requires Pharmacist request + specific reason + Owner approval/rejection | CONFIRMED | FR-105 to FR-107, BR-048, OD-019 |
| Payment-method labels are Owner-configurable clinic values; CRM records but does not process payment | CONFIRMED | FR-063, FR-108, BR-051, OD-018 |
| Reception-requested waiver requires doctor approval | CONFIRMED | FR-015, FR-016, BR-022, OD-005 |
| Doctor can initiate waiver directly | CONFIRMED | FR-015, BR-022, OD-005 |
| Waiver requires reason and audit log | CONFIRMED | FR-074, BR-022, OD-005 |
| No CRM urgent-priority feature; receptionist tells doctor directly | CONFIRMED | FR-024, BR-026, OD-006 |
| Doctor-specific queues | CONFIRMED | FR-018, FR-020, FR-023, BR-027, OD-007 |
| Reception can reassign between doctor queues; reassignment logged | CONFIRMED | FR-080, BR-028, OD-007 |
| Unresponded patient moved five queue positions down; if fewer than five remain, move to end | DERIVED V1 DECISION | FR-025, OD-007 queue edge rules |
| Paid patient who leaves before consultation moved to end of assigned queue | DERIVED V1 DECISION | FR-025 |
| Reception cannot cancel visit | CONFIRMED | FR-081, BR-029 |
| Doctor requests visit/consultation cancellation with specific reason; Owner approves/rejects; approved item leaves active workflow but remains in history | CONFIRMED | FR-081, FR-074, BR-029, BR-049, OD-019 |
| Clinical entry should be low-complexity/guided | CONFIRMED BY DELEGATED DESIGN | FR-028 to FR-030, OD-008 |
| Completed consultation correction uses Doctor-only amendment/revision; original preserved; reason required | DERIVED FROM AUDIT MODEL | FR-032, BR-012, OD-009 |
| Clinical decision fields remain Doctor-role functions | CONFIRMED DESIGN CONTROL | FR-066, OD-008, OD-022 |
| Medicine selected by display name + strength/power + dosage form; optional generic searchable attribute | DERIVED V1 DESIGN | FR-033, OD-010 |
| Manufacturer stored | CONFIRMED | FR-033, FR-087, OD-010, OD-016 |
| Prescription instructions intentionally minimal | CONFIRMED | FR-037, OD-011 |
| Quantity auto-calculated when deterministic, otherwise entered by doctor | CONFIRMED | FR-038, OD-011 |
| Finalized prescription cannot be edited in place; correction creates replacement and marks old prescription Superseded | DERIVED FROM IMMUTABILITY/AUDIT | FR-039, BR-030, OD-012 |
| Pharmacist cannot edit prescription | CONFIRMED | FR-040, FR-067, BR-030, OD-012 |
| Doctor sees pharmacy availability while prescribing | CONFIRMED | FR-034 to FR-036, BR-014, BR-015 |
| Partial medicine fulfilment allowed | DELEGATED V1 DESIGN | FR-052 to FR-054, OD-013 |
| No back-order/collect-later workflow in V1 | DELEGATED V1 DESIGN | FR-052, OD-013 |
| Prevent over-dispensing beyond prescribed quantity | DELEGATED V1 DESIGN | FR-082, OD-013 |
| Pharmacist substitution requires doctor approval and audit | DELEGATED V1 DESIGN | FR-055, OD-014 |
| Medicine returns not supported in V1 | CONFIRMED + DELEGATED FLOW | FR-056, OD-017 |
| CRM pharmacy dispensing requires current finalized prescription | DELEGATED V1 DESIGN | FR-083, OD-032 |
| Inventory tracked with configured base unit + higher package conversion factors | DERIVED V1 DESIGN | FR-086, OD-015 |
| Batch, expiry, manufacturer, purchase price, selling price stored | CONFIRMED | FR-087, OD-016 |
| Low-stock and near-expiry alerts use Doctor/Admin-configurable thresholds | DERIVED V1 DESIGN | FR-088, OD-016 |
| Normal dispensing deducts stock automatically | CONFIRMED | FR-051, BR-017, BR-031 |
| Pharmacist non-dispensing inventory change requires doctor approval | CONFIRMED | FR-068, BR-031, OD-030 |
| Inventory-change request/reason/decision logged | CONFIRMED | FR-068, OD-030 |
| Expired stock requires doctor notification and logged disposition | CONFIRMED with safety constraint | FR-089, BR-032, OD-031 |
| Expired stock is not dispensable | SAFETY CONTROL | FR-089, BR-032, OD-031 |
| Reception limited to intake, demographics confirmation, payment status, queue | CONFIRMED | FR-065, OD-022 |
| Reception demographic correction becomes doctor approval request | CONFIRMED | FR-065, FR-066, OD-022 |
| Doctor can edit demographics and approve changes | CONFIRMED | FR-066, OD-022 |
| Owner and Doctor are separate roles; one user may hold both | CONFIRMED | FR-064, FR-097 to FR-099, BR-036, BR-039, OD-022, OD-023 |
| Owner has clinic-wide inventory oversight and approves non-dispensing stock changes/transfers | CONFIRMED | FR-068, FR-090, FR-096, FR-097, BR-031, BR-037, BR-041, BR-042 |
| Doctor-only users retain clinical authority but do not inherit Owner controls | CONFIRMED | FR-066, FR-099, BR-034, BR-039, BR-042 |
| Multiple doctors use separate doctor-specific queues | CONFIRMED | FR-092, BR-027, OD-007, OD-023 |
| Multiple receptionists use individual accounts in a shared reception workspace | CONFIRMED | FR-093, OD-023 |
| Multiple pharmacy units maintain separate stock ledgers with Owner consolidated view | CONFIRMED | FR-094 to FR-096, BR-040, BR-041, OD-023 |
| Pharmacist limited to prescriptions, allergies, dispensing/payment status, inventory requests | CONFIRMED | FR-067, FR-068, OD-022 |
| Individual user accounts in fixed clinic context | CONFIRMED | FR-069, FR-084, BR-035, OD-021, OD-022 |
| Group-based privileges including Owner/Doctor/Reception/Pharmacist/Admin | CONFIRMED | FR-064, FR-069, OD-022 |
| Owner-role accounts require password + Google Authenticator-compatible TOTP 2FA, including Owner + Doctor accounts | CONFIRMED | FR-085, BR-043, OD-021 |
| Non-Owner staff accounts do not require 2FA in V1 | CONFIRMED | FR-100, BR-043, OD-021 |
| Staff Forgot Password creates Owner reset request; Owner sets reset credential; old password is never exposed | CONFIRMED | FR-101, FR-102, BR-044, OD-021 |
| Staff must replace Owner-reset temporary password after next login; reset is audited | DERIVED SAFE V1 DESIGN | FR-103, BR-045, OD-021 |
| Owner TOTP setup generates one-time recovery codes; catastrophic recovery has no in-app bypass | DERIVED SAFE SECURITY DESIGN | FR-104, BR-046, OD-021 |
| Current pilot is single-branch/one owner-doctor, but the same clinic model supports additional doctors, receptionists, and pharmacy units | CONFIRMED | FR-092 to FR-099, BR-039 to BR-042, OD-023 |
| Web app, internet-dependent, no offline V1 | CONFIRMED | BRD Section 13, OD-024 |
| A4 printing; no thermal printer requirement | CONFIRMED | BRD Section 13, OD-020, OD-024 |
| Hosting decision deferred | OPEN | BRD Section 9.8, OD-024 |
| V1 reporting set selected | CONFIRMED SET; formulas TBD | BRD Section 8, OD-029 |

---

# 4. Requirement Group Coverage

| Requirement Group | BRD IDs |
| --- | --- |
| Patient Registration and Retrieval | FR-001 — FR-007, FR-075 — FR-079 |
| Visit Creation | FR-008 — FR-010 |
| Reception and Consultation Payment | FR-011 — FR-016 |
| Consultation Queue | FR-017 — FR-025, FR-080 — FR-081, FR-092 — FR-093 |
| Doctor Consultation and Clinical Record | FR-026 — FR-032 |
| Prescription Authoring and Medicine Availability | FR-033 — FR-040 |
| Printed Prescription | FR-041 — FR-044 |
| Pharmacy Prescription Retrieval and Access | FR-045 — FR-048 |
| Pharmacy Dispensing and Inventory | FR-049 — FR-056, FR-082 — FR-083, FR-086 — FR-090, FR-094 — FR-096 |
| Pharmacy Billing and Payment Status | FR-057 — FR-063, FR-105 — FR-108 |
| Roles and Access | FR-064 — FR-069, FR-097 — FR-099 |
| Audit, Cancellation, Authentication | FR-070 — FR-074, FR-084 — FR-085, FR-100 — FR-104 |

---

# 5. Business Rule Coverage

| Rule Area | BRD Rules |
| --- | --- |
| Patient identity | BR-001 — BR-005, BR-025 |
| Visit and queue | BR-006 — BR-010, BR-026 — BR-029 |
| Clinical record | BR-011 — BR-013 |
| Prescription and pharmacy | BR-014 — BR-018, BR-030 — BR-032 |
| Payment | BR-019 — BR-022, BR-033 |
| Audit | BR-023 — BR-024 |

---

# 6. Canonical Decision Register Coverage

The current decision IDs are defined in:

- [03 — Open Decisions and Edge Cases](03-open-decisions-and-edge-cases.md)

v0.2 corrected numbering inconsistencies from the initial v0.1 register; v0.3 adds the generalized Owner/multi-role security model.

Key genuinely open areas are now limited to:

- OD-026 — consultation fee/billing policy supplied by the clinic;
- OD-025 — backup/recovery targets, to be finalized in technical architecture;
- OD-027 / OD-028 — retention, privacy, healthcare/legal/compliance requirements requiring external validation;
- OD-024 — hosting/infrastructure selection, intentionally deferred.

Other former open items have either been closed as derived V1 design decisions or moved to downstream technical configuration rather than remaining business-policy questions.

---

# 7. Current Completeness Assessment

The v0.3 baseline now establishes:

- patient identity and registration-field model;
- duplicate fallback behavior;
- doctor-specific queues and reassignment;
- payment-to-queue gate;
- Owner-controlled fee waiver;
- no CRM urgent-priority workflow;
- non-response and patient-left queue behavior at high level;
- Doctor-requested / Owner-approved consultation cancellation;
- low-complexity doctor consultation entry;
- minimal immutable prescription;
- pharmacy availability and dispensing;
- partial dispensing;
- doctor-controlled substitution;
- no medicine returns in V1;
- multi-unit pharmacy inventory;
- batch/expiry/pricing metadata;
- stock and expiry alerts;
- Owner-controlled non-dispensing inventory changes and inter-pharmacy transfers;
- expired-stock non-dispensability;
- role/group access model;
- individual authentication model with Owner TOTP 2FA and Owner-controlled staff password recovery;
- single-branch online web pilot;
- selected V1 reporting set.

The business workflow is now substantially defined. Final implementation readiness still depends on the small set of clinic-policy, compliance, and technical-architecture items listed in Section 6.

---

# 8. Change-Control Guidance

When another decision is confirmed:

1. update the canonical OD entry;
2. update affected FR/BR wording;
3. update workflow/state documentation;
4. update this traceability map;
5. remove superseded wording;
6. preserve existing IDs rather than renumbering established requirements casually.
