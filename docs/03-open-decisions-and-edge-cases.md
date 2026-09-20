# Hospital CRM — Open Decisions and Edge Cases

## 1. Purpose

This document is the active decision register for requirements that are relevant to the Hospital CRM but are not yet fully confirmed.

Items in this file are **not automatically requirements**. They remain open until explicitly resolved.

---

# 2. Decision Status Labels

- **OPEN** — requires business decision.
- **PROPOSED** — suggestion exists but has not been approved.
- **CONFIRMED** — decision has been explicitly accepted and should be promoted into the BRD.
- **OUT OF SCOPE** — explicitly excluded.
- **FUTURE** — confirmed for a later phase.

---

# 3. Highest-Priority Open Decisions

| ID | Area | Decision Required | Status |
| --- | --- | --- | --- |
| OD-001 | Registration | Mandatory patient fields beyond name and phone | OPEN |
| OD-002 | Patient matching | Duplicate detection/matching rule | OPEN |
| OD-003 | Patient matching | Whether family members may share one phone number | OPEN |
| OD-004 | Visit/queue | Exact condition for entering consultation queue | OPEN |
| OD-005 | Visit/queue | Treatment of Paid, Partial, Unpaid, and Waived consultation states | OPEN |
| OD-006 | Queue | Urgent/out-of-order queue rule | OPEN |
| OD-007 | Queue | Multiple doctors and queue-assignment model | OPEN |
| OD-008 | Clinical record | Required structure of symptoms/history/diagnosis/notes | OPEN |
| OD-009 | Clinical record | Rules for editing a completed consultation | OPEN |
| OD-010 | Prescription | Medicine catalogue model: brand, generic/molecule, or both | OPEN |
| OD-011 | Prescription | Required prescription fields and quantity derivation | OPEN |
| OD-012 | Prescription | Edit workflow after pharmacy has opened the prescription | OPEN |
| OD-013 | Pharmacy | Partial dispensing behavior | OPEN |
| OD-014 | Pharmacy | Substitute/alternate brand rules | OPEN |
| OD-015 | Inventory | Unit model: tablet/strip/bottle/pack/etc. | OPEN |
| OD-016 | Inventory | Batch and expiry tracking requirement | OPEN |
| OD-017 | Billing | Consultation fee determination | OPEN |
| OD-018 | Billing | Accepted payment methods | OPEN |
| OD-019 | Billing | Refund/void authorization rules | OPEN |
| OD-020 | Security | Authentication method | OPEN |
| OD-021 | Security | Detailed role-permission matrix | OPEN |
| OD-022 | Operations | Audit retention and access rules | OPEN |
| OD-023 | Compliance | Applicable legal/privacy requirements | OPEN |
| OD-024 | Deployment | Hosting/deployment model | OPEN |
| OD-025 | Reporting | Required business, operational, financial, and inventory reports | OPEN |

---

# 4. Registration Decisions

## OD-001 — Mandatory Patient Fields

Currently confirmed:

- patient name;
- phone number;
- Patient ID is system-generated.

Not yet confirmed:

- date of birth;
- age;
- gender;
- address;
- emergency contact;
- guardian information;
- email;
- government ID;
- blood group;
- allergy field;
- other clinical demographics.

No additional field should be treated as mandatory until explicitly approved.

## OD-002 — Duplicate Detection

Need to decide:

- exact-match vs fuzzy-match behavior;
- whether phone + name is sufficient for warning;
- how receptionist resolves a suspected duplicate;
- who may merge duplicate records;
- whether record merge is required in MVP.

## OD-003 — Shared Phone Number

Scenario:

> Multiple family members may use the same phone number.

Need to decide whether phone number can be non-unique and what additional fields distinguish patients.

---

# 5. Queue Decisions

## OD-004 — Queue Entry Rule

Need to define the exact prerequisite for a visit to enter the doctor queue.

Potential states already acknowledged:

- Paid
- Partial
- Unpaid
- Waived

No state is yet confirmed as universally eligible/ineligible.

## OD-005 — Payment Exception Handling

Need to define:

- who can waive consultation fees;
- whether waived visits display differently;
- whether unpaid/partial visits may proceed;
- whether payment can be collected after consultation.

## OD-006 — Urgent Patient Rule

Scenario:

> A patient needs to be seen before others in normal queue order.

Need to decide:

- who can change priority;
- whether a reason is required;
- whether queue history records the change;
- how waiting patients are represented after reorder.

## OD-007 — Multiple Doctors

Need to define:

- one shared queue vs doctor-specific queues;
- how reception assigns patients;
- whether doctor reassignment is allowed;
- whether doctors can pull from a shared pool.

---

# 6. Clinical Documentation Decisions

## OD-008 — Clinical Field Structure

Need to decide whether consultation documentation is:

- free text;
- structured fields;
- templates;
- combination.

Need to define required vs optional fields.

## OD-009 — Post-Completion Editing

Need to define:

- whether completed visits may be reopened;
- who may reopen;
- whether edits create amendments;
- whether original content remains visible;
- whether reason-for-change is mandatory.

---

# 7. Prescription Decisions

## OD-010 — Medicine Catalogue Model

Need to define whether medicines are represented by:

- brand;
- generic/molecule;
- strength;
- dosage form;
- manufacturer;
- some combination.

This decision affects pharmacy inventory, search, availability, substitution, and printed prescription behavior.

## OD-011 — Prescription Structure

Need to define required fields such as:

- medicine;
- strength;
- dosage form;
- dose;
- frequency;
- duration;
- route;
- timing/instructions;
- quantity.

Nothing beyond the already accepted high-level prescription behavior is mandatory yet.

## OD-012 — Prescription Changes After Pharmacy Retrieval

Scenario:

> Doctor updates prescription after the patient has already reached pharmacy.

Need to define:

- whether pharmacy receives immediate updated version;
- whether old version is invalidated;
- whether pharmacist receives a warning;
- behavior if some items have already been dispensed;
- whether doctor must issue an amendment.

---

# 8. Pharmacy and Inventory Decisions

## OD-013 — Partial Dispensing

Scenario:

> Prescription requests 20 tablets; only 8 are available.

Need to define:

- whether 8 may be dispensed;
- how remaining 12 are represented;
- whether patient may return later;
- whether outside purchase is advised;
- billing behavior;
- inventory behavior;
- status of prescription item after partial fulfilment.

## OD-014 — Substitution

Need to define whether pharmacist may supply an alternate:

- brand;
- strength;
- dosage form;
- molecule equivalent.

Need to define who authorizes substitution.

## OD-015 — Inventory Unit

Need to define stock unit and conversion model.

Examples may include tablet, strip, bottle, pack, vial, etc., but no unit model is yet confirmed.

## OD-016 — Batch / Expiry

Need to decide whether MVP tracks:

- batch/lot number;
- expiry date;
- manufacturer;
- purchase cost;
- sale price by batch.

## OD-017 — Returns and Reversals

Need to define:

- whether medicines can be returned;
- whether returns are allowed after leaving pharmacy;
- refund behavior;
- inventory reversal;
- audit requirements.

---

# 9. Billing Decisions

## OD-018 — Payment Methods

No payment method has yet been confirmed.

Need to decide supported modes, for example whether cash/UPI/card/etc. are accepted. These examples are not requirements.

## OD-019 — Refund / Cancellation Authorization

Need to define:

- who can refund;
- who can cancel;
- whether a reason is mandatory;
- whether manager/admin approval is needed;
- whether receipt/reference links are preserved.

## OD-020 — Receipt Format

Need to define:

- consultation receipt format;
- pharmacy receipt format;
- numbering;
- reprint behavior;
- tax fields if applicable.

---

# 10. Access and Security Decisions

## OD-021 — Authentication

Need to define:

- sign-in method;
- password requirements if applicable;
- session rules;
- account lockout;
- password reset;
- multi-factor authentication if applicable.

## OD-022 — Detailed Permissions

Need to define exact read/write permissions for:

- patient demographics;
- clinical history;
- diagnosis;
- notes;
- prescription;
- queue;
- consultation payment;
- pharmacy billing;
- inventory;
- reports;
- audit logs;
- user administration.

---

# 11. Deployment and Operations Decisions

## OD-023 — Clinic Topology

Need to confirm:

- single clinic vs multiple locations;
- number of doctors;
- number of reception desks;
- number of pharmacy terminals;
- expected concurrent users.

## OD-024 — Hosting and Device Model

Need to decide:

- web vs desktop vs other client model;
- hosting environment;
- network assumptions;
- browser/device support;
- printer type;
- offline requirement.

No technical architecture is confirmed yet.

## OD-025 — Backup and Recovery

Need to define:

- backup expectation;
- recovery expectation;
- acceptable data loss;
- downtime tolerance;
- disaster recovery responsibility.

---

# 12. Legal / Privacy / Compliance Decisions

No legal/privacy/compliance regime has been confirmed in the source material.

Need validation for:

- patient-data handling;
- staff access;
- audit retention;
- data retention;
- printed information;
- breach/incident handling;
- applicable healthcare/privacy obligations.

This must be validated rather than inferred.

---

# 13. Analytics and Reporting Decisions

Need to define whether the system requires:

- queue/wait-time reporting;
- consultation volumes;
- payment reporting;
- pharmacy sales;
- stock movement;
- low-stock reporting;
- medicine-out-of-stock reporting;
- doctor workload;
- audit reporting;
- patient return frequency.

These are investigation areas only, not confirmed KPIs.

---

# 14. Accepted Future / Out-of-Scope Items

## Future

- QR/barcode patient-file identification.
- Pharmacy purchasing/supplier management.
- Broader CRM reminders/follow-up engagement.
- Extended stock automation such as richer low-stock workflows.

## Out of Initial MVP

- laboratory management;
- inpatient/bed management;
- insurance processing;
- ambulance management;
- HR/payroll;
- full purchasing/supplier management.

---

# 15. Decision Closure Rule

When an open item is resolved:

1. record the explicit decision;
2. update the BRD requirement/business rule;
3. update the workflow/state model if affected;
4. update traceability;
5. mark the decision as CONFIRMED;
6. ensure no contradictory wording remains elsewhere.
