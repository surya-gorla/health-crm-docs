# Hospital CRM — Requirements Traceability

## 1. Purpose

This document maps the confirmed discovery decisions, explicitly delegated product decisions, and derived control decisions to the BRD requirements/business rules and canonical decision register.

The current pre-lock baseline is v0.3.

---

# 2. Source Classification

The baseline is derived from:

1. the original clinic workflow described by the product owner;
2. explicitly confirmed decisions from iterative discovery;
3. grouped answers covering patient identity, queue behavior, clinical UX, prescription, pharmacy, inventory, payments, permissions, authentication, deployment, and reporting;
4. pharmacy UX/edge-case decisions explicitly delegated for product-design resolution;
5. narrow derived safety/control decisions that follow directly from the confirmed audit, role-separation, inventory-accountability, and financial-integrity models.

Documentation rules:

- **CONFIRMED** means directly stated/approved by the product owner;
- **DELEGATED / DERIVED** decisions are identified as such and must not be misrepresented as verbatim clinic input;
- configuration values and technical/compliance dependencies are not promoted into business requirements as invented facts;
- material records are not silently overwritten or hard-deleted;
- Owner, Doctor, Reception, Pharmacist, and Administrator authority must remain distinct even when one account holds multiple roles.

---

# 3. Core Traceability Matrix

| Decision / Source | Classification | BRD / Decision Mapping |
| --- | --- | --- |
| Permanent Patient ID | CONFIRMED | FR-002, FR-003, BR-001 |
| Search by Patient ID, phone, and name | CONFIRMED | FR-001, BR-002 |
| Longitudinal patient archive across visits | CONFIRMED | FR-027, FR-091, BR-038, BO-09 |
| Inventory-loss/theft visibility and prevention | CONFIRMED | FR-068, FR-090, BR-031, BR-037, BO-08 |
| DOB entered; age derived | CONFIRMED | FR-075, OD-001 |
| Required patient fields | CONFIRMED | FR-075, OD-001 |
| Optional patient fields including Government ID | CONFIRMED | FR-076, FR-077, OD-001 |
| Shared family phone numbers allowed | CONFIRMED | FR-078, BR-025, OD-003 |
| Similar patient candidates reviewed with patient | CONFIRMED | FR-004, BR-005, OD-002 |
| Unconfirmed candidate -> new usable profile marked Possible Duplicate | CONFIRMED | FR-004, FR-079, BR-005, OD-002 |
| Duplicate merge not required in V1 | CONFIRMED CURRENT SCOPE | FR-079, OD-002 |
| Demographic correction request is Doctor-controlled; history preserved | CONFIRMED + DERIVED AUDIT DETAIL | FR-006, FR-065, FR-066, FR-071, OD-022 |
| Separate Visit ID per clinic attendance | CONFIRMED | FR-008 to FR-010, BR-003, BR-006 |
| CRM records payments but does not process them | CONFIRMED | FR-011 to FR-016, FR-060 to FR-063, FR-108 to FR-112, BR-020, BR-033, BR-053, OD-018 |
| Default methods UPI/Cash/Card/Other; Other requires description; reference optional | CONFIRMED | FR-108 to FR-110, BR-051, BR-052, OD-018 |
| No partial consultation payment | CONFIRMED | FR-013, BR-021, OD-004 |
| Consultation payment non-refundable | CONFIRMED | FR-013, BR-021, BR-050, OD-019 |
| Unpaid consultation cannot enter queue | CONFIRMED | FR-016, BR-010, OD-004 |
| Reception or Doctor may request waiver; Owner approves | CONFIRMED | FR-015, FR-016, BR-022, OD-005 |
| Owner may directly initiate waiver; reason/audit required | CONFIRMED | FR-015, FR-074, BR-022, OD-005 |
| Approved waiver is Waived, not Paid | DERIVED FINANCIAL MODEL | FR-013, OD-005 |
| Doctor-specific queues | CONFIRMED | FR-018, FR-020, FR-023, FR-092, BR-027, OD-007 |
| Reception may reassign doctor queue; reassignment logged | CONFIRMED | FR-080, BR-028, OD-007 |
| No CRM urgent-priority feature | CONFIRMED | FR-024, BR-026, OD-006 |
| Unresponded -> move five positions; if unavailable, move to end | CONFIRMED + DERIVED BOUNDARY | FR-019, FR-025, OD-007 |
| Paid patient leaves before consultation -> move to end | CONFIRMED + DERIVED BOUNDARY | FR-025, OD-007 |
| Doctor requests consultation cancellation; Owner approves/rejects | CONFIRMED | FR-074, FR-081, BR-029, BR-049, OD-019 |
| Active visit cancellation: Doctor request + reason -> Owner decision; approved void preserves all existing history and does not refund | CONFIRMED | FR-073, FR-081, BR-024, BR-049, BR-050, OD-019 |
| Guided low-complexity clinical entry | DELEGATED DESIGN | FR-028 to FR-030, OD-008 |
| Completed consultation corrected by Doctor amendment/revision; original retained | DERIVED FROM AUDIT MODEL | FR-032, BR-012, OD-009 |
| Clinical authority comes from Doctor role | CONFIRMED | FR-066, BR-034, BR-036, OD-008, OD-022 |
| Medicine identity: display name + strength + dosage form; manufacturer stored | CONFIRMED + DERIVED MODEL | FR-033, FR-087, OD-010 |
| Minimal prescription instruction set | CONFIRMED | FR-037, OD-011 |
| Quantity auto-calculated when deterministic | CONFIRMED | FR-038, OD-011 |
| Finalized prescription immutable; correction supersedes/replaces | CONFIRMED + DERIVED AUDIT MODEL | FR-039, BR-030, OD-012 |
| Pharmacist cannot edit prescription | CONFIRMED | FR-040, FR-067, BR-030, OD-012 |
| Doctor sees pharmacy availability while prescribing | CONFIRMED | FR-034 to FR-036, BR-014, BR-015 |
| Printed ** marker reflects Out of Stock/Not Stocked status known at prescription finalization; later partial remainder belongs to dispensing summary | SOURCE-FIDELITY CLARIFICATION | FR-042, FR-043, OD-012 |
| Multi-pharmacy view shows clinic total + unit availability | DERIVED FROM CONFIRMED MULTI-PHARMACY MODEL | FR-116, BR-058, OD-037 |
| Partial medicine fulfilment allowed | DELEGATED V1 DESIGN | FR-052 to FR-054, OD-013 |
| No back-order/collect-later workflow | DELEGATED V1 DESIGN | FR-052, OD-013 |
| Cumulative dispensing cannot exceed prescription | DELEGATED V1 DESIGN | FR-082, FR-095, OD-013 |
| Pharmacist substitution requires Doctor approval | DELEGATED V1 DESIGN | FR-055, BR-042, OD-014 |
| Medicine returns not supported | CONFIRMED | FR-056, OD-017 |
| Non-prescription/pharmacy-only CRM dispensing out of V1 | DELEGATED V1 DESIGN | FR-083, OD-032 |
| Inventory uses base unit + package conversions | CONFIRMED + DERIVED MODEL | FR-086, OD-015 |
| Batch, expiry, manufacturer, purchase/selling price stored | CONFIRMED | FR-087, OD-016 |
| Low-stock/near-expiry alerts; thresholds Owner/Admin configurable | CONFIRMED + DERIVED CONFIGURATION | FR-088, OD-016 |
| Normal dispensing deducts stock automatically | CONFIRMED | FR-051, BR-017, BR-031 |
| Non-dispensing inventory changes require Owner approval | CONFIRMED | FR-068, BR-031, BR-037, OD-030 |
| Expired stock blocked; Owner controls disposition adjustment | CONFIRMED | FR-089, BR-032, OD-031 |
| Bill void does not restore dispensed stock | DERIVED FROM INVENTORY-ACCOUNTABILITY MODEL | FR-115, BR-056, OD-034 |
| Multiple pharmacy units keep separate ledgers | CONFIRMED | FR-094 to FR-096, BR-040, BR-041, OD-023 |
| Multi-pharmacy dispensing/billing remains unit-specific while cumulative prescription quantity is enforced clinic-wide | DERIVED FROM UNIT-LEDGER MODEL | FR-095, FR-118, OD-037 |
| Owner sees unit-level and consolidated inventory | CONFIRMED | FR-090, FR-097, OD-023 |
| Reception limited to intake/payment-status/queue operations | CONFIRMED | FR-065, OD-022 |
| Pharmacist visibility limited to current/previous prescriptions + allergies | CONFIRMED | FR-048, FR-067, BR-013, OD-022 |
| Owner and Doctor are distinct roles; same account may hold both | CONFIRMED | FR-064, FR-097 to FR-099, BR-036, BR-039, OD-022, OD-023 |
| Doctor-only users do not inherit Owner controls | CONFIRMED | FR-066, FR-099, BR-039, BR-042, OD-022 |
| Owner-only/Admin-only users do not inherit unrestricted clinical content | DERIVED ROLE-SEPARATION CONTROL | FR-113, BR-054, OD-035 |
| Multiple receptionists use individual accounts in shared workspace | CONFIRMED | FR-093, OD-023 |
| Administrator cannot grant/revoke Owner or disable Owner | DERIVED SECURITY CONTROL | FR-117, BR-057, OD-036 |
| Individual user accounts | CONFIRMED | FR-084, BR-035, OD-021 |
| Owner-role account requires Google Authenticator-compatible TOTP 2FA | CONFIRMED | FR-085, BR-043, OD-021 |
| Non-Owner staff do not require 2FA | CONFIRMED | FR-100, BR-043, OD-021 |
| Staff Forgot Password -> Owner reset request | CONFIRMED | FR-101, FR-102, BR-044, OD-021 |
| Reset temporary credential must be changed after next login | DERIVED SAFE V1 DESIGN | FR-103, BR-045, OD-021 |
| Owner TOTP recovery codes / no in-app catastrophic bypass | DERIVED SECURITY DESIGN | FR-104, BR-046, OD-021 |
| Incorrect payment record -> staff correction request + Owner approval | DERIVED FINANCIAL AUDIT CONTROL | FR-114, BR-055, OD-033 |
| Pharmacy payment: no partial payment, no refund | CONFIRMED | FR-061, BR-047, BR-050, OD-019 |
| Pharmacy bill void: Pharmacist request + specific reason + Owner decision | CONFIRMED | FR-105 to FR-107, BR-048, OD-019 |
| Current pilot single branch/one Owner+Doctor; scalable within same clinic | CONFIRMED | FR-092 to FR-099, BR-039 to BR-042, OD-023 |
| Web app, online-only V1 | CONFIRMED | BRD Section 13, OD-024 |
| A4 printing; no thermal printer | CONFIRMED | BRD Section 13, OD-020, OD-024 |
| V1 reporting set and business definitions | CONFIRMED + DERIVED DEFINITIONS | BRD Section 8, OD-029 |

---

# 4. Requirement Group Coverage

| Requirement Group | BRD IDs |
| --- | --- |
| Patient Registration and Retrieval | FR-001 — FR-007, FR-075 — FR-079 |
| Visit Creation | FR-008 — FR-010 |
| Reception and Consultation Payment | FR-011 — FR-016, FR-108 — FR-114 |
| Consultation Queue and Multi-Doctor Operations | FR-017 — FR-025, FR-080 — FR-081, FR-092 — FR-093 |
| Doctor Consultation and Longitudinal Clinical Record | FR-026 — FR-032, FR-091 |
| Prescription Authoring and Availability | FR-033 — FR-040, FR-116 |
| Printed Prescription | FR-041 — FR-044 |
| Pharmacy Prescription Retrieval and Access | FR-045 — FR-048 |
| Pharmacy Dispensing and Inventory | FR-049 — FR-056, FR-082 — FR-083, FR-086 — FR-090, FR-094 — FR-096, FR-115 — FR-116, FR-118 |
| Pharmacy Billing and Payment | FR-057 — FR-063, FR-105 — FR-115, FR-118 |
| Roles and Access | FR-064 — FR-069, FR-097 — FR-100, FR-113, FR-117 |
| Audit, Cancellation, and Authentication | FR-070 — FR-074, FR-084 — FR-085, FR-100 — FR-104, FR-114 — FR-117 |

---

# 5. Business Rule Coverage

| Rule Area | BRD Rules |
| --- | --- |
| Patient identity | BR-001 — BR-005, BR-025 |
| Visit and queue | BR-006 — BR-010, BR-026 — BR-029, BR-049 |
| Clinical record / clinical authority | BR-011 — BR-013, BR-034, BR-036, BR-038, BR-054 |
| Prescription and pharmacy | BR-014 — BR-018, BR-030 — BR-032, BR-040 — BR-042, BR-056, BR-058 |
| Payment / financial control | BR-019 — BR-022, BR-033, BR-047 — BR-053, BR-055 |
| Roles / authentication / account control | BR-035, BR-039, BR-043 — BR-046, BR-057 |
| Audit / history | BR-023 — BR-024, plus the audit obligations embedded in BR-029 — BR-058 |

---

# 6. Canonical Decision Register

The canonical decision IDs are maintained in:

- [03 — Open Decisions and Edge Cases](03-open-decisions-and-edge-cases.md)

The active business decisions are closed for the core V1 workflow.

Remaining non-workflow dependencies are:

- **OD-024** — hosting/infrastructure implementation;
- **OD-025** — backup/recovery targets;
- **OD-026** — clinic-supplied consultation fee values;
- **OD-027** — audit retention/export policy;
- **OD-028** — legal/privacy/healthcare compliance validation.

These are configuration, technical architecture, or external compliance dependencies and do not authorize implementation teams to change the locked business workflow.

---

# 7. Completeness Assessment

The v0.3 pre-lock baseline establishes:

- patient identity and duplicate-fallback model;
- longitudinal patient archive;
- doctor-specific queues and reassignment;
- explicit Unresponded behavior;
- payment-to-queue gate;
- Owner-controlled fee waiver;
- no CRM urgent-priority feature;
- Doctor-requested / Owner-approved consultation cancellation;
- guided clinical entry and Doctor-only clinical authority;
- immutable clinical amendments and prescription replacement;
- pharmacy availability, partial dispensing, and Doctor-controlled substitution;
- no medicine returns / no non-prescription CRM retail;
- multi-unit and multi-pharmacy inventory;
- Owner-controlled manual inventory changes/transfers;
- bill-void versus stock separation;
- Owner/Doctor/Reception/Pharmacist/Admin role separation;
- Owner TOTP 2FA and Owner-controlled staff password recovery;
- external high-velocity payment recording with UPI/Cash/Card/Other;
- no consultation/pharmacy refunds and no partial payments;
- Owner-controlled payment corrections and pharmacy bill voids;
- selected reporting definitions;
- single-branch online web pilot with same-clinic scale flexibility.

No remaining item requires reopening core V1 business discovery.

---

# 8. Lock / Change-Control Rule

After the BRD is marked LOCKED:

1. confirmed V1 requirements and business rules must not be silently changed;
2. configuration values may be supplied without reopening the BRD where the BRD explicitly classifies them as configuration;
3. technical/compliance decisions must not redefine business behavior without a formal BRD change;
4. any requested behavioral change must identify affected FR/BR/OD IDs;
5. change impact must be propagated to the BRD, workflow/state model, decision register, and this traceability document;
6. superseded wording must be removed rather than left contradictory;
7. existing identifiers should remain stable unless a deliberate migration is documented.
