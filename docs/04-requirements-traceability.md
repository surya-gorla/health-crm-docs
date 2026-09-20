# Hospital CRM — Requirements Traceability

## 1. Purpose

This document provides traceability between the confirmed product decisions from discovery and the formal requirements in the BRD.

It also separates confirmed requirements from future items, exclusions, and open decisions.

---

# 2. Source Classification

The current baseline is derived from:

1. the originally described clinic workflow;
2. the accepted refinements from product discussion;
3. the user's explicit acceptance of the proposed patient/visit model, queue improvements, role boundaries, auditability, transaction-based payments, and MVP module boundary.

The BRD authoring method follows the supplied zero-assumption rule:

- confirmed information becomes requirements;
- unresolved information remains open/TBD;
- suggestions are not silently promoted into scope.

---

# 3. Traceability Matrix

| Discovery Decision / Requirement Source | Classification | BRD Mapping |
| --- | --- | --- |
| Reception captures patient details | CONFIRMED | FR-001 to FR-007, FR-075 to FR-078 |
| Registration requires full name, phone, DOB, age, gender, address, and email | CONFIRMED | FR-075 |
| Emergency contact, blood group, known allergies, and guardian/parent details are optional | CONFIRMED | FR-076 |
| Government ID is not required | CONFIRMED | FR-077 |
| Multiple patients may share one phone number | CONFIRMED | FR-078, BR-025 |
| Suspected duplicate candidates are reviewed with the patient; prior visit history/purpose may help confirmation | CONFIRMED | FR-004, BR-005 |
| System generates unique Patient ID | CONFIRMED | FR-002, FR-003 |
| Patient ID remains permanent | CONFIRMED | FR-003, BR-001 |
| Patient searchable by Patient ID, phone, and name | CONFIRMED | FR-001, BR-002 |
| Returning patient should not get new patient identity | CONFIRMED | FR-007, FR-010, BR-004 |
| Each visit should have separate Visit ID | CONFIRMED | FR-008 to FR-010, BR-003, BR-006 |
| Consultation payment handled at reception | CONFIRMED | FR-011 to FR-016 |
| Payment should be a transaction, not only a checkbox | CONFIRMED | FR-012, BR-020 |
| Consultation payment supports Paid/Unpaid/Refunded/Cancelled; partial consultation payment is not supported | CONFIRMED | FR-013, BR-021 |
| Paid consultation may enter doctor queue; unpaid consultation may not | CONFIRMED | FR-016, BR-010, OD-004 |
| Reception may request consultation-fee waiver; doctor must approve; approved waiver permits queue entry | CONFIRMED | FR-015, FR-016, BR-010, BR-022, OD-005 |
| Patient goes into queue after reception workflow | CONFIRMED subject to payment eligibility | FR-017, FR-016 |
| Doctor sees queue | CONFIRMED | FR-018, FR-020 |
| Queue states should be explicit | CONFIRMED | FR-019, BR-008 |
| Doctor calls patient through reception | CONFIRMED | FR-021, FR-022, BR-009 |
| Doctor sees patient and records consultation | CONFIRMED | FR-026 to FR-032 |
| Doctor records diagnosis and notes | CONFIRMED | FR-028 to FR-032 |
| Doctor prescribes medicines from screen | CONFIRMED | FR-033 to FR-040 |
| Doctor can search/select/type medicines | CONFIRMED | FR-033 |
| Doctor sees pharmacy availability while prescribing | CONFIRMED | FR-034 |
| Availability distinguishes In Stock / Out of Stock / Not Stocked | CONFIRMED | FR-035, BR-015 |
| Doctor may still prescribe unavailable medicine | CONFIRMED | FR-036, BR-014 |
| Printed prescription marks unavailable medicines | CONFIRMED | FR-041 to FR-043, BR-016 |
| Patient takes prescription to pharmacy | CONFIRMED | FR-045 onward |
| Pharmacy retrieves by Patient ID | CONFIRMED | FR-045 |
| Pharmacy should not automatically see full clinical notes | CONFIRMED | FR-047, BR-013 |
| Pharmacy identifies available/unavailable medicines | CONFIRMED | FR-049, FR-053 |
| Pharmacy dispenses available items | CONFIRMED | FR-050 |
| Dispensing reduces inventory | CONFIRMED | FR-051, BR-017 |
| Pharmacy explains unavailable items/outside purchase | CONFIRMED | FR-053, FR-054 |
| Pharmacy bills only supplied medicines | CONFIRMED | FR-057 to FR-059, BR-018 |
| Pharmacy records payment | CONFIRMED | FR-060 to FR-063 |
| Initial roles: Admin, Receptionist, Doctor, Pharmacist | CONFIRMED | FR-064 to FR-069 |
| Important records need audit history | CONFIRMED | FR-070 to FR-074, BR-023, BR-024 |
| Important records should not be silently hard-deleted | CONFIRMED | FR-072, FR-073 |
| Prescription reprint required | CONFIRMED | FR-044 |
| Patient-detail correction required | CONFIRMED | FR-006 |
| Duplicate registration is an important scenario | PARTIALLY CONFIRMED; fallback remains open | FR-004, BR-005, OD-002 |
| Multiple doctors is an important scenario | CONFIRMED scenario, rule open | FR-024/OD-007 |
| Urgent queue override is an important scenario | CONFIRMED scenario, rule open | FR-024, OD-006 |
| Patient leaves after payment | CONFIRMED scenario, rule open | FR-025 |
| Prescription changed after reaching pharmacy | CONFIRMED scenario, rule open | OD-012 |
| Partial pharmacy availability | CONFIRMED scenario, rule open | FR-052, OD-013 |
| Alternate brands/substitution | CONFIRMED scenario, rule open | FR-055, OD-014 |
| Medicine return/refund | CONFIRMED scenario, rule open | FR-056, OD-017 |
| Consultation payment waiver | CONFIRMED scenario, exact rule open | FR-015, OD-005 |
| Pharmacy-only visit | CONFIRMED scenario, rule open | Open Decisions |
| Shared family phone number | CONFIRMED | FR-078, BR-025, OD-003 |
| QR/barcode patient-file identifier | FUTURE | BRD 3.3 |
| Pharmacy supplier/purchasing management | FUTURE / OUT OF MVP | BRD 3.2 and 3.3 |
| Laboratory management | OUT OF MVP | BRD 3.2 |
| Inpatient/bed management | OUT OF MVP | BRD 3.2 |
| Insurance processing | OUT OF MVP | BRD 3.2 |
| Ambulance management | OUT OF MVP | BRD 3.2 |
| HR/payroll | OUT OF MVP | BRD 3.2 |

---

# 4. Requirement Group Coverage

| Requirement Group | BRD IDs |
| --- | --- |
| Patient Registration and Retrieval | FR-001 — FR-007 |
| Visit Creation | FR-008 — FR-010 |
| Reception and Consultation Payment | FR-011 — FR-016 |
| Consultation Queue | FR-017 — FR-025 |
| Doctor Consultation and Clinical Record | FR-026 — FR-032 |
| Prescription Authoring and Medicine Availability | FR-033 — FR-040 |
| Printed Prescription | FR-041 — FR-044 |
| Pharmacy Prescription Retrieval and Access | FR-045 — FR-048 |
| Pharmacy Dispensing and Inventory | FR-049 — FR-056 |
| Pharmacy Billing and Payment | FR-057 — FR-063 |
| Roles and Access | FR-064 — FR-069 |
| Audit, Correction, Cancellation, and Reprint | FR-070 — FR-074 |

---

# 5. Business Rule Coverage

| Rule Area | BRD Rules |
| --- | --- |
| Patient identity | BR-001 — BR-005 |
| Visit and queue | BR-006 — BR-010 |
| Clinical record | BR-011 — BR-013 |
| Prescription and pharmacy | BR-014 — BR-018 |
| Payment | BR-019 — BR-022 |
| Audit | BR-023 — BR-024 |

---

# 6. Open Decision Coverage

Open requirements and edge cases are maintained in:

- [03 — Open Decisions and Edge Cases](03-open-decisions-and-edge-cases.md)

These items are intentionally not resolved through assumptions.

---

# 7. Current Completeness Assessment

The baseline is strong enough to establish:

- the main actors;
- the principal journey;
- the identity model;
- the visit model;
- the queue model at a high level;
- the consultation record;
- prescription/pharmacy integration;
- transaction-based payment recording;
- pharmacy dispensing and inventory deduction;
- initial role boundaries;
- audit-history expectations;
- MVP exclusions.

The baseline is **not yet implementation-final** because material decisions remain open around:

- remaining patient-field implementation details and duplicate fallback/merge behavior;
- waiver financial-record representation and reason requirements;
- multi-doctor routing;
- clinical field structure;
- medicine master/catalogue;
- prescription amendments;
- partial dispensing;
- substitution;
- inventory units/batches/expiry;
- payment methods;
- permissions/security;
- compliance;
- reporting;
- deployment architecture.

---

# 8. Change-Control Guidance

When a new requirement is confirmed:

1. assign or update its BRD requirement ID;
2. add/update the relevant business rule;
3. update workflow/state documentation if applicable;
4. remove or resolve the corresponding open decision;
5. add a traceability row;
6. check for contradictions with existing requirements;
7. preserve the meaning of previously confirmed decisions unless explicitly superseded.
