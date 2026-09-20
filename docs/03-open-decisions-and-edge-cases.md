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
| OD-002 | Patient matching | Duplicate detection and fallback | CONFIRMED at BRD level; matching implementation belongs to solution design |
| OD-003 | Patient matching | Shared family phone number | CONFIRMED |
| OD-004 | Queue | Consultation payment gate | CONFIRMED |
| OD-005 | Queue/Payment | Consultation-fee waiver | CONFIRMED |
| OD-006 | Queue | Urgent-patient priority handling | CONFIRMED — outside CRM |
| OD-007 | Queue | Multiple doctors / reassignment | CONFIRMED |
| OD-008 | Clinical | V1 clinical-entry structure | CONFIRMED |
| OD-009 | Clinical | Editing completed consultation | CONFIRMED — doctor amendment model |
| OD-010 | Prescription | Medicine catalogue identity | CONFIRMED |
| OD-011 | Prescription | Minimal prescription structure | CONFIRMED |
| OD-012 | Prescription | Finalized prescription correction | CONFIRMED — supersede/replace model |
| OD-013 | Pharmacy | Partial dispensing | CONFIRMED |
| OD-014 | Pharmacy | Medicine substitution | CONFIRMED |
| OD-015 | Inventory | Unit hierarchy | CONFIRMED at business level; conversions are medicine configuration |
| OD-016 | Inventory | Batch/expiry/pricing metadata and alerts | CONFIRMED — thresholds configurable |
| OD-017 | Pharmacy | Medicine returns | CONFIRMED — not supported in V1 |
| OD-018 | Payment | Payment methods recorded by CRM | OPEN |
| OD-019 | Payment | Refund/cancellation rules | PARTIALLY CONFIRMED |
| OD-020 | Payment | Receipt/reference requirements | CONFIRMED for V1 outputs; payment receipt detail remains clinic-dependent |
| OD-021 | Security | Authentication | CONFIRMED at V1 business level |
| OD-022 | Security | Role/group permissions | CONFIRMED at V1 business level |
| OD-023 | Deployment | Single-clinic topology with multi-doctor/reception/pharmacy scalability | CONFIRMED |
| OD-024 | Deployment | Client/device model confirmed; hosting deferred to technical design |
| OD-025 | Operations | Backup and recovery | OPEN |
| OD-026 | Billing | Consultation fee determination | OPEN |
| OD-027 | Audit | Audit retention and access | OPEN |
| OD-028 | Compliance | Legal/privacy/compliance requirements | OPEN |
| OD-029 | Reporting | CONFIRMED at V1 business level; implementation details belong to reporting design |
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

### Human-readable ID format — derived V1 decision

The displayed Patient ID and Visit ID should be simple, stable, non-semantic identifiers. They must not encode phone number, DOB, diagnosis, or other personal/clinical meaning.

V1 display convention may use forms such as `PAT-000001` and `VIS-000001`. Exact database/internal key implementation belongs to technical design.

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

Business-level V1 decision:

- candidate matching uses the confirmed identifying attributes available to the system, including Patient ID, name, phone number, and date of birth;
- the system may use exact and similarity matching to surface candidates but shall never auto-merge or auto-select a patient solely from a similarity score;
- the final identity confirmation remains a receptionist + patient decision;
- exact scoring/fuzzy-matching thresholds are solution-design details, not an unresolved BRD decision.

Current V1 does not include duplicate merge. A merge capability may be evaluated later.

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
- Approved Owner-controlled waiver -> queue eligible despite no payment.

V1 records payment status/information but does not process the payment itself.

## OD-005 — Consultation-Fee Waiver

**Status: CONFIRMED**

Confirmed:

1. Reception or a Doctor may request a consultation-fee waiver.
2. Reception and Doctor role alone cannot approve the waiver.
3. The request is shown to the Owner.
4. Owner is the approving authority.
5. Until Owner approval is received, an unpaid visit remains ineligible for the doctor queue.
6. Owner approval makes the visit queue-eligible without consultation payment.
7. If a user holding the Owner role initiates the waiver directly, no second approval step is required.
8. Every waiver requires a reason.
9. Waiver request, decision, actor, reason, and timestamp are logged.
10. A user who is both Owner and Doctor may initiate/approve through the Owner authority on the same account.

V1 financial representation:

- normal consultation status remains **Paid** or **Unpaid**;
- an approved waiver is stored as a separate **Waived** financial outcome, not falsely represented as Paid;
- **Waived** is queue-eligible.

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

Derived V1 boundary rules:

- **Unresponded:** move five positions down; if fewer than five later positions exist, move to the end of the current queue.
- **Paid patient leaves before consultation:** move the visit to the end of the current doctor-specific queue.
- A later call can use the normal call workflow again.

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

**Status: CONFIRMED — DERIVED FROM THE AUDIT MODEL**

Completed clinical records are not edited destructively.

If correction is required:

1. only a Doctor-role user may amend the completed consultation;
2. the original completed content remains preserved;
3. the doctor creates an amendment/new revision;
4. reason for amendment is mandatory;
5. actor and timestamp are recorded;
6. the current view shows the latest effective record while retaining access to prior history.

This follows the already-confirmed rule that material clinical information cannot be silently overwritten.

---

# 7. Prescription

## OD-010 — Medicine Catalogue Identity

**Status: CONFIRMED — DERIVED V1 MODEL**

The medicine catalogue identifies a medicine using:

- display/medicine name;
- strength ("power");
- dosage form;
- manufacturer.

Dosage form is required because the same medicine/strength can exist in materially different forms.

An optional generic/molecule name may be stored as an additional searchable attribute. V1 does not require the doctor to prescribe by generic name; the clinic catalogue display name remains the primary selection label.

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

**Status: CONFIRMED — DERIVED FROM IMMUTABILITY/AUDIT RULES**

A finalized prescription is never edited in place.

If the doctor discovers an error:

1. doctor creates a replacement prescription;
2. the previous prescription is marked **Superseded** rather than deleted;
3. correction reason is mandatory;
4. actor and timestamp are logged;
5. pharmacy is shown only the latest active prescription as the default dispensing source, with a clear warning that an earlier prescription was superseded;
6. if any quantity was already dispensed from the superseded prescription, that dispensing history remains intact and cannot be erased;
7. subsequent dispensing is limited by the active corrected prescription and recorded prior dispensing history.

Pharmacist cannot perform this correction.

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

Derived V1 model:

- each medicine defines a **base stock/dispensing unit**;
- each higher package level stores its conversion into the next lower/base unit (for example, a strip may contain a configured number of tablets);
- inventory calculations normalize movements to the configured base unit while allowing staff to enter/view higher package quantities.

The actual conversion value is medicine-specific configuration, not a new business-policy decision.

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

Low-stock and near-expiry thresholds are configurable by an authorized Doctor/Admin role rather than hard-coded globally. Initial values are operational configuration, not a BRD decision.

## OD-030 — Pharmacist Inventory-Change Approval

**Status: CONFIRMED**

Normal prescription dispensing automatically deducts actual dispensed stock.

For non-dispensing inventory upkeep/changes, including stock addition, damage, loss, or correction:

1. pharmacist submits a requested inventory change;
2. request includes change and reason;
3. Owner reviews it;
4. inventory changes only after Owner approval;
5. request, actor, reason, decision, and resulting change are logged.

This control is intended to make non-dispensing inventory losses/adjustments visible to the Owner rather than allowing unreviewed manual stock reductions.

Medicine selling-price or purchase-price changes proposed by pharmacy use the same request -> Owner-approval -> logged-change workflow.

## OD-031 — Expired Stock

**Status: CONFIRMED**

Expired stock is blocked from dispensing.

The Owner is notified.

Owner response controls the inventory disposition/adjustment record, and the action is logged.

The Owner-approval step does not provide a path to dispense expired stock.

Derived V1 handling:

- expired/damaged/lost quantities are removed from **available** stock only through the Owner-approved adjustment workflow;
- the adjustment records a reason/category and quantity;
- the CRM does not prescribe the clinic's physical disposal process in V1;
- supplier-return/procurement workflows remain outside V1;
- near-expiry threshold is configurable.

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

## OD-020 — Printable Outputs / Payment Reference

**Status: CONFIRMED AT V1 BUSINESS LEVEL**

V1 supports A4 printing for clinic outputs already in scope, including prescription and pharmacy bill/dispensing summary where needed.

The CRM does not require thermal printing and does not require payment-gateway receipts because it does not process payments.

If the clinic later requires a formal payment receipt/reference format, that can be configured after the clinic supplies its billing requirements.

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

Derived V1 design:

- because the pilot is a single clinic, no clinic/tenant selector is required at login;
- staff use individual username/login + password accounts within the fixed clinic context;
- password reset is administrator/owner-assisted in V1 rather than requiring email/SMS infrastructure;
- inactivity timeout and password-strength rules are security configuration to be finalized during technical/security design, not remaining business-discovery questions.

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

In the current pilot, the owner is the only Doctor-role user and is the sole clinical decision/finalization authority.

Can:

- see doctor-specific queue;
- see relevant clinical history;
- create clinical record;
- create/finalize prescription;
- see queue/payment status;
- directly edit demographics;
- approve/reject reception demographic-change requests;
- approve/reject substitution request;
- cancel visit with reason.

Doctor role alone does not grant clinic-owner financial, inventory-control, staff-management, or clinic-wide audit privileges.

### Owner

Can:

- see clinic-wide operational/revenue/inventory dashboards;
- see all pharmacy units individually and in consolidated view;
- approve/reject consultation-fee waiver requests;
- approve/reject non-dispensing inventory changes;
- approve/reject pharmacy-to-pharmacy stock transfers;
- inspect complete inventory movement/adjustment history;
- manage/oversee staff accounts and group assignment subject to Admin design.

Owner role alone does not grant clinical-authoring authority.

A user who is both Owner and Doctor has one account with both roles.

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
- directly apply non-dispensing inventory changes without Owner approval.

### Administrator

Derived V1 Admin privileges:

- create/disable staff accounts;
- assign users to permission groups;
- manage clinic-level non-clinical configuration;
- manage medicine catalogue/inventory configuration where allowed;
- view operational/revenue/inventory reports;
- view audit logs.

Admin permission by itself does **not** grant authority to create/edit doctor clinical notes, diagnosis, prescriptions, waiver approvals, or clinical amendments. A person who is both owner/admin and doctor receives those clinical permissions through the Doctor group.

### Staff lifecycle — derived V1 decision

- Admin/Owner creates each staff account.
- Admin/Owner assigns one or more permission groups.
- Role/group changes are logged.
- When a staff member leaves, the account is **disabled**, not deleted, so historical audit references remain valid.
- Disabled users cannot sign in.
- Re-enabling an account is an Admin/Owner action and is logged.

## OD-027 — Audit Retention and Access

**Status: PARTIALLY CONFIRMED**

Derived V1 access:

- Doctor/Owner and Admin may view audit logs;
- pharmacist/reception users do not receive unrestricted audit-log access;
- inventory-adjustment audit is filterable as part of the audit log;
- audit events are not deletable through normal application UI.

Still requires external policy/compliance validation:

- minimum/maximum retention period;
- export/archive requirements.

---

# 12. Pilot Deployment

## OD-023 — Single-Clinic Topology with Role/Unit Scaling

**Status: CONFIRMED**

Current pilot:

- single clinic branch;
- clinic owner is also the clinic's only doctor;
- clinic has a separate pharmacy operation.

The design must also support the same clinic growing to:

- multiple doctors;
- multiple receptionists;
- multiple pharmacy units.

Rules:

1. Owner is a distinct role from Doctor.
2. A user may hold both Owner and Doctor on one account.
3. Every doctor retains an individual account and doctor-specific queue.
4. Multiple receptionists use individual accounts while sharing the reception operating view.
5. Each pharmacy unit has an independent stock ledger.
6. Owner sees unit-level and consolidated inventory.
7. Non-dispensing inventory adjustments and pharmacy-to-pharmacy stock transfers require Owner approval.
8. Clinical actions remain Doctor permissions; Owner role alone does not grant clinical-authoring authority.
9. A doctor who is not Owner does not inherit Owner inventory/financial/staff privileges.
10. Reception has no Owner privileges.

The system continues to serve as the clinic's longitudinal patient archive across visits.

## OD-024 — Client / Device / Hosting Model

**Status: PARTIALLY CONFIRMED**

Confirmed:

- web application;
- internet-dependent V1;
- no offline mode;
- normal A4 printing;
- no thermal receipt printer requirement.

Derived V1 client scope:

- desktop/laptop web browser is the primary client;
- responsive support for tablet-sized screens may be provided, but a separate mobile app is not required;
- target current mainstream browsers used by the clinic.

Hosting provider, concrete infrastructure, and performance targets belong to downstream technical architecture and remain intentionally deferred.

No historical-system migration is assumed for V1 unless the clinic later supplies a specific digital data source for import. No third-party integration is required in initial V1 unless explicitly added later.

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

Derived V1 definitions:

- **patients seen per day** = visits reaching consultation-completed state during the clinic's local calendar day;
- **average waiting time** = average time from queue entry to With Doctor;
- **consultation revenue** = sum of recorded Paid consultation fees; Waived is excluded;
- **pharmacy revenue** = sum of recorded pharmacy bill amounts treated as paid under the clinic's recorded payment status;
- **daily total revenue** = consultation revenue + pharmacy revenue;
- **medicine sales** = quantities and value actually dispensed, not prescribed;
- **current stock** = available stock after approved adjustments and dispensing;
- **low/out-of-stock/expiring** = inventory state based on configured thresholds and expiry;
- **most prescribed medicines** = prescription-item count/quantity over the selected period;
- **waivers/cancellations/returning patients/audit activity** use the corresponding recorded system events.

Access:

- Doctor/Owner and Admin may view all V1 reports;
- Pharmacist may view pharmacy/inventory reports;
- Reception may view reception/queue operational information required for its work, but not unrestricted clinical reporting.

Exact chart layout, export format, analytics technology, and data-retention duration are implementation/policy details rather than remaining business questions.

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
