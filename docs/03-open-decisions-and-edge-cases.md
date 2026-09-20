# Hospital CRM — Open Decisions and Edge Cases

## 1. Purpose

This is the active decision register for the Hospital CRM.

It separates:

- **CONFIRMED** decisions already promoted into the BRD;
- **OPEN** items that still require a decision;
- **PARTIALLY CONFIRMED** items where the principal behavior is known but one or more implementation-significant business rules remain unresolved;
- **OUT OF SCOPE** items excluded from V1;
- **FUTURE / LATER EVALUATION** items that are not part of current V1 behavior.

Unknown behavior must remain visible here rather than being silently assumed.

---

# 2. Decision-ID Reconciliation

The v0.1 decision register contained numbering mismatches between its summary table and detailed sections, including reuse of some OD numbers for different subjects.

v0.2 establishes the canonical mapping below.

Existing references that were already used for core patient, queue, clinical, prescription, dispensing, return, payment, authentication, permission, topology, hosting, and backup topics have been preserved where practical. New IDs are introduced for topics that previously lacked a stable unique ID.

---

# 3. Decision Summary

| ID | Area | Decision | Status |
| --- | --- | --- | --- |
| OD-001 | Registration | Patient registration fields | CONFIRMED |
| OD-002 | Patient matching | Duplicate detection and fallback | PARTIALLY CONFIRMED |
| OD-003 | Patient matching | Shared family phone number | CONFIRMED |
| OD-004 | Queue | Consultation payment gate | CONFIRMED |
| OD-005 | Queue/Payment | Consultation-fee waiver | PARTIALLY CONFIRMED |
| OD-006 | Queue | Urgent-patient priority handling | CONFIRMED — outside CRM |
| OD-007 | Queue | Multiple doctors / reassignment | CONFIRMED |
| OD-008 | Clinical | V1 clinical-entry structure | CONFIRMED |
| OD-009 | Clinical | Editing completed consultation | OPEN |
| OD-010 | Prescription | Medicine catalogue identity | PARTIALLY CONFIRMED |
| OD-011 | Prescription | Minimal prescription structure | CONFIRMED |
| OD-012 | Prescription | Finalized prescription correction | PARTIALLY CONFIRMED |
| OD-013 | Pharmacy | Partial dispensing | CONFIRMED |
| OD-014 | Pharmacy | Medicine substitution | CONFIRMED |
| OD-015 | Inventory | Unit hierarchy | PARTIALLY CONFIRMED |
| OD-016 | Inventory | Batch/expiry/pricing metadata and alerts | CONFIRMED |
| OD-017 | Pharmacy | Medicine returns | CONFIRMED — not supported in V1 |
| OD-018 | Payment | Payment methods recorded by CRM | OPEN |
| OD-019 | Payment | Refund/cancellation rules | PARTIALLY CONFIRMED |
| OD-020 | Payment | Receipt/reference requirements | OPEN |
| OD-021 | Security | Authentication | PARTIALLY CONFIRMED |
| OD-022 | Security | Role/group permissions | PARTIALLY CONFIRMED |
| OD-023 | Deployment | Pilot clinic topology | CONFIRMED |
| OD-024 | Deployment | Client/device/hosting model | PARTIALLY CONFIRMED |
| OD-025 | Operations | Backup and recovery | OPEN |
| OD-026 | Billing | Consultation fee determination | OPEN |
| OD-027 | Audit | Audit retention and access | OPEN |
| OD-028 | Compliance | Legal/privacy/compliance requirements | OPEN |
| OD-029 | Reporting | V1 reports and metric definitions | PARTIALLY CONFIRMED |
| OD-030 | Inventory control | Pharmacist inventory-change approval | CONFIRMED |
| OD-031 | Inventory safety | Expired-stock handling | CONFIRMED |
| OD-032 | Pharmacy scope | Non-prescription/pharmacy-only CRM dispensing | OUT OF V1 |

---

# 4. Patient Registration and Identity

## OD-001 — Patient Registration Fields

**Status: CONFIRMED**

Required:

- full name;
- phone number;
- date of birth;
- gender;
- address;
- email.

Derived:

- age is calculated from date of birth and is not maintained as a separate required input.

Optional:

- emergency contact;
- blood group;
- known allergies;
- guardian/parent details;
- Government ID.

System-generated:

- Patient ID.

Government ID is not required.

## OD-002 — Duplicate Detection and Fallback

**Status: PARTIALLY CONFIRMED**

Confirmed:

1. The system surfaces similar existing patient candidates during search/registration.
2. Reception reviews candidate information with the patient.
3. Previous visit history and the patient's remembered purpose of a prior visit may be used to help identity confirmation.
4. If the patient confirms an existing candidate, reception uses that existing Patient ID.
5. If the patient cannot confidently identify any candidate, reception may create a new patient profile.
6. The newly created profile is marked **Possible Duplicate**.
7. A Possible Duplicate record remains usable as a normal patient profile.

Open:

- exact algorithm/criteria for surfacing possible matches;
- whether duplicate-record merge is added in a later update;
- who would be allowed to merge if that feature is introduced.

Current V1 does not require duplicate merge.

## OD-003 — Shared Phone Number

**Status: CONFIRMED**

Multiple patients may share one phone number, including family members.

Therefore:

- phone number is not globally unique;
- phone number is a search/matching attribute;
- Patient ID remains the unique patient identifier;
- a phone-number match alone cannot identify two records as the same patient.

---

# 5. Queue, Payment Gate, and Visit Control

## OD-004 — Consultation Payment Gate

**Status: CONFIRMED**

- Paid consultation -> queue eligible.
- Unpaid consultation -> not queue eligible.
- Partial consultation payment -> not supported.
- Approved doctor-controlled waiver -> queue eligible despite no payment.

V1 records payment status/information but does not process the payment itself.

## OD-005 — Consultation-Fee Waiver

**Status: PARTIALLY CONFIRMED**

Confirmed:

1. Reception may request a waiver.
2. Reception cannot approve a waiver.
3. A reception-requested waiver is shown to the doctor.
4. Doctor approves or rejects.
5. Until approval, an unpaid visit cannot enter the queue.
6. Approved waiver makes the visit queue-eligible.
7. Doctor may initiate the waiver directly.
8. Doctor-initiated waiver is immediately approved and needs no second approval.
9. Every waiver requires a reason.
10. Waiver action, actor, reason, and decision are logged.

Open:

- how an approved waiver is represented in the financial/status model.

## OD-006 — Urgent Patient Handling

**Status: CONFIRMED**

There is no priority feature in the CRM.

If an urgent case occurs:

1. receptionist directly informs the doctor outside the software;
2. CRM does not create a priority flag, priority request, or automated priority-reordering workflow.

## OD-007 — Multiple Doctors and Queue Assignment

**Status: CONFIRMED**

1. Queues are doctor-specific.
2. Reception selects the doctor before queue entry.
3. Visit enters the selected doctor's queue only.
4. Doctor sees patients assigned to that doctor's queue.
5. There is no shared clinic-wide consultation queue.
6. Reception may reassign a queued visit from one doctor to another.
7. Every reassignment is logged.

## Queue Edge Cases — Confirmed and Open

Confirmed:

- Called patient does not respond -> reception marks **Unresponded** and system moves the visit five positions downward.
- Paid patient leaves before consultation -> reception moves the visit toward the end of the doctor-specific queue.
- Reception cannot cancel a visit.
- Doctor may cancel a visit.
- Doctor cancellation requires a reason.
- Cancelled visit remains in history.
- Consultation payment is non-refundable in V1 even when the doctor cancels the paid consultation.

Open:

- if fewer than five later positions exist, exact Unresponded destination;
- exact slot definition of "toward the end" for a paid patient who leaves.

---

# 6. Clinical Documentation

## OD-008 — V1 Clinical Entry Structure

**Status: CONFIRMED**

The interface should remain simple enough to operate without requiring complex medical-software knowledge.

Doctor-facing V1 uses a low-complexity guided interface with plain labels.

Core entry:

- chief complaint / patient problem;
- clinical assessment / diagnosis.

Optional entry:

- symptoms/history details;
- examination findings;
- clinical notes;
- advice;
- follow-up information.

Clinical decision functions remain within the Doctor role even though the interface itself is designed to be simple.

## OD-009 — Editing a Completed Consultation

**Status: OPEN**

Need to decide:

- whether completed clinical notes/diagnosis can be amended;
- who may amend;
- whether amendments create a new version;
- whether original content remains visible;
- whether reason-for-change is mandatory.

---

# 7. Prescription

## OD-010 — Medicine Catalogue Identity

**Status: PARTIALLY CONFIRMED**

Confirmed:

- medicine name is a primary search/selection field;
- medicine strength ("power") is stored;
- manufacturer is stored.

Open:

- whether dosage form is required or optional;
- whether the clinic's medicine name is explicitly a brand name, generic/molecule name, or a clinic-defined catalogue name.

## OD-011 — Minimal Prescription Structure

**Status: CONFIRMED**

Because detailed verbal explanation is expected during consultation, V1 prescription entry remains minimal.

For each medicine:

- medicine name;
- strength;
- dose amount;
- frequency;
- duration.

Optional when needed:

- timing / food relation;
- short extra instruction.

Quantity behavior:

- calculate automatically when dose/frequency/duration and inventory unit are sufficient for deterministic calculation;
- otherwise doctor enters quantity.

## OD-012 — Finalized Prescription Changes

**Status: PARTIALLY CONFIRMED**

Confirmed:

- doctor cannot edit a finalized prescription in place;
- pharmacist cannot edit the finalized prescription;
- pharmacy opening the prescription does not change this rule.

Open:

- correction/replacement workflow when the finalized prescription contains an error;
- whether old prescription is cancelled and replaced, superseded, or handled through another explicit amendment model.

---

# 8. Pharmacy Dispensing

The decisions in this section were made under the delegated UX/edge-case authority provided for pharmacy V1.

## OD-013 — Partial Dispensing

**Status: CONFIRMED**

If prescribed quantity exceeds available quantity:

1. pharmacy may dispense the quantity actually available;
2. actual supplied quantity is recorded;
3. inventory decreases by supplied quantity only;
4. bill includes supplied quantity only;
5. remaining quantity is shown as not supplied by the clinic pharmacy;
6. pharmacist tells the patient to obtain the remainder outside;
7. V1 does not maintain back-order/collect-later inventory reservations;
8. system tracks already-dispensed quantity and prevents dispensing beyond prescribed quantity.

## OD-014 — Substitution

**Status: CONFIRMED**

Pharmacist cannot independently substitute a prescribed medicine.

Flow:

1. pharmacist raises substitution request;
2. request is shown to doctor;
3. doctor approves or rejects;
4. request and decision are logged;
5. only an approved substitution may be dispensed.

## OD-017 — Medicine Returns

**Status: CONFIRMED — NOT SUPPORTED IN V1**

Patients cannot return medicines through the V1 pharmacy workflow.

Returns/refunds may be reconsidered in a future version based on real-world V1 operation.

## OD-032 — Pharmacy-Only / Non-Prescription CRM Dispensing

**Status: OUT OF V1**

V1 CRM dispensing requires a current finalized prescription.

General pharmacy retail without a current CRM prescription is outside the confirmed V1 workflow.

---

# 9. Inventory

## OD-015 — Inventory Unit Hierarchy

**Status: PARTIALLY CONFIRMED**

Inventory must be trackable:

- at the lowest dispensable unit;
- at higher package levels.

Examples may include tablet/capsule, strip, bottle, pack, vial, etc., depending on medicine.

Open:

- exact conversion rules and unit relationships for each medicine type.

## OD-016 — Inventory Metadata and Alerts

**Status: CONFIRMED**

Inventory supports:

- batch/lot number;
- expiry date;
- manufacturer;
- purchase price;
- selling price;
- low-stock alert;
- near-expiry alert.

Threshold values/configuration remain open.

## OD-030 — Pharmacist Inventory-Change Approval

**Status: CONFIRMED**

Normal prescription dispensing automatically deducts actual dispensed stock.

For non-dispensing inventory upkeep/changes, including stock addition, damage, loss, or correction:

1. pharmacist submits a requested inventory change;
2. request includes change and reason;
3. doctor reviews it;
4. inventory changes only after doctor approval;
5. request, actor, reason, decision, and resulting change are logged.

This control is intended to make non-dispensing inventory losses/adjustments visible to the doctor rather than allowing unreviewed manual stock reductions.

## OD-031 — Expired Stock

**Status: CONFIRMED**

Expired stock is blocked from dispensing.

The doctor is notified.

Doctor response controls the inventory disposition/adjustment record, and the action is logged.

The doctor-approval step does not provide a path to dispense expired stock.

Open:

- final disposition choices after doctor review;
- near-expiry threshold;
- whether disposition includes destruction/return-to-supplier/other clinic-defined categories.

---

# 10. Payments and Billing

## OD-018 — Payment Methods

**Status: OPEN**

The CRM does not process payments in V1.

Clinic will later provide the payment methods/status information that should be recorded.

Open for:

- consultation payment methods to record;
- pharmacy payment methods to record.

## OD-019 — Refund and Cancellation

**Status: PARTIALLY CONFIRMED**

Consultation:

- consultation payment cannot be refunded in V1;
- if a paid consultation must be cancelled, only the doctor can cancel;
- doctor must provide a reason;
- cancellation is logged;
- payment is not refunded.

Pharmacy:

- refund/cancellation rules remain TBD.

## OD-020 — Receipt / Payment Reference

**Status: OPEN**

Need to decide:

- whether CRM generates any consultation payment acknowledgement/reference;
- whether pharmacy requires a CRM receipt/reference;
- exact A4 output requirements if any.

Thermal receipt printing is not required in V1.

## OD-026 — Consultation Fee Determination

**Status: OPEN**

Clinic will define:

- consultation fee;
- whether fee varies by doctor/service;
- how fee changes are configured.

---

# 11. Roles, Permissions, and Authentication

## OD-021 — Authentication

**Status: PARTIALLY CONFIRMED**

Confirmed:

- every staff member has an individual login;
- staff authenticate under the clinic/hospital context;
- login + password is used in V1;
- password-reset capability is required;
- 2FA/MFA is not required in V1.

Open:

- exact clinic identifier/login-screen behavior;
- session timeout;
- password-reset mechanism;
- password policy.

## OD-022 — Role / Group Permissions

**Status: PARTIALLY CONFIRMED**

Access is group-based, with individual accounts belonging to groups such as:

- Reception;
- Doctor;
- Pharmacist.

### Reception

Can:

- intake/search patient;
- confirm demographic details;
- record consultation payment status;
- assign/reassign doctor queue;
- manage permitted queue actions;
- request a demographic correction.

Cannot:

- directly change existing demographics after registration;
- approve its own demographic-change request;
- cancel visits;
- access general diagnosis/clinical notes.

Limited previous-visit information may be used for identity confirmation.

### Doctor

Can:

- see doctor-specific queue;
- see relevant clinical history;
- create clinical record;
- create/finalize prescription;
- see queue/payment status;
- directly edit demographics;
- approve/reject reception demographic-change requests;
- approve/reject consultation waiver;
- initiate consultation waiver;
- approve/reject substitution request;
- approve/reject non-dispensing inventory-change requests;
- cancel visit with reason.

### Pharmacist

Can see:

- current prescription;
- previous prescriptions;
- known allergies.

Can:

- dispense;
- perform pharmacy billing/payment-status functions;
- submit inventory-upkeep/change requests.

Cannot:

- directly edit doctor prescription;
- directly apply non-dispensing inventory changes without doctor approval.

### Administrator

Detailed Administrator privileges remain OPEN.

## OD-027 — Audit Retention and Access

**Status: OPEN**

Need to decide:

- who can view audit logs;
- how long audit history is retained;
- filtering/export requirements;
- whether doctor/owner sees inventory-control audit separately.

---

# 12. Pilot Deployment

## OD-023 — Pilot Clinic Topology

**Status: CONFIRMED**

Current pilot:

- single clinic branch;
- owner is also the doctor managing the clinic;
- clinic has a separate pharmacy operation.

Exact staff/workstation counts remain operational details to confirm when needed.

## OD-024 — Client / Device / Hosting Model

**Status: PARTIALLY CONFIRMED**

Confirmed:

- web application;
- internet-dependent V1;
- no offline mode;
- normal A4 printing;
- no thermal receipt printer requirement.

Open:

- hosting/deployment provider;
- supported browsers/computers;
- performance/concurrency targets.

## OD-025 — Backup and Recovery

**Status: OPEN**

Need to define:

- backup frequency;
- recovery target;
- acceptable data loss;
- downtime tolerance;
- restore responsibility;
- disaster-recovery process.

---

# 13. Legal / Privacy / Compliance

## OD-028 — Applicable Requirements

**Status: OPEN**

Requires explicit validation before final implementation:

- patient-data handling;
- role access;
- audit retention;
- data retention;
- printed patient information;
- incident/breach handling;
- applicable healthcare/privacy obligations.

No compliance regime is asserted as confirmed in this BRD yet.

---

# 14. Analytics and Reporting

## OD-029 — V1 Reporting

**Status: PARTIALLY CONFIRMED**

Confirmed V1 report/measurement set:

1. patients seen per day;
2. average waiting time;
3. consultation revenue;
4. pharmacy revenue;
5. daily total revenue;
6. medicine sales;
7. current stock;
8. low-stock medicines;
9. out-of-stock medicines;
10. expiring medicines;
11. most-prescribed medicines;
12. waivers;
13. cancellations;
14. returning patients;
15. audit activity.

Per-doctor patient count is not required for the current single-doctor pilot.

Open:

- exact formulas;
- report date/time boundaries;
- access permissions;
- export requirements;
- analytics/reporting platform;
- retention;
- refund reporting if a future pharmacy refund flow is introduced.

---

# 15. Future / Later Evaluation

Not in current confirmed V1:

- duplicate-record merge;
- medicine returns;
- general pharmacy retail without a current CRM prescription;
- offline mode;
- thermal receipt printing;
- payment processing integration.

Already identified as later-phase candidates:

- QR/barcode patient-file identification;
- pharmacy purchasing/supplier management;
- broader CRM reminders/follow-up engagement;
- richer stock automation beyond confirmed V1 controls.

---

# 16. Decision Closure Rule

When an open item is resolved:

1. record the explicit decision here;
2. update BRD requirement/business rule;
3. update workflow/state model if affected;
4. update traceability;
5. change decision status;
6. remove contradictory/obsolete wording from all documents.
