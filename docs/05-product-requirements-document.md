# Hospital CRM — Product Requirements Document

## Document Control

| Field | Value |
| --- | --- |
| Document | Product Requirements Document |
| Product | Hospital CRM for clinic operations |
| Version | 0.1 |
| Status | DRAFT — derived from locked BRD v1.0 |
| Date | 2026-09-20 |
| Source baseline | BRD v1.0 LOCKED |
| Working branch | `prd/v1-product-requirements` |
| Product stage | V1 product definition |

---

# 1. Purpose

This PRD translates the locked V1 Business Requirements Document into an implementation-grade product specification for design, engineering, QA, product review, and release planning.

It defines:

- who uses the product;
- what each role must be able to accomplish;
- how the product is organized into workspaces;
- the product behavior for each core workflow;
- product states and transitions visible to users;
- role and permission expectations;
- interaction and usability requirements;
- error-prevention and audit expectations;
- product analytics/reporting behavior;
- acceptance and release expectations.

This PRD does **not** reopen the locked BRD.

---

# 2. Source-of-Truth and Decision Hierarchy

The following precedence applies:

1. **BRD v1.0 LOCKED** is the authoritative business source.
2. **Workflows and State Model** clarifies the locked end-to-end behavior.
3. **V1 Decision Register and Edge Cases** clarifies confirmed, delegated, derived, future, configuration, technical, and compliance decisions.
4. **Requirements Traceability** proves business-source lineage.
5. This PRD may add product/UX detail only when it does not conflict with the locked business behavior.

If this PRD conflicts with the locked BRD, the BRD wins and the PRD must be corrected.

### Classification used in this PRD

- **INHERITED** — directly required by the locked BRD.
- **DERIVED PRODUCT DESIGN** — a product/UX decision needed to make inherited behavior usable without changing business policy.
- **CONFIGURATION** — clinic-supplied value such as fee, price, threshold, or optional output format.
- **TECHNICAL DEPENDENCY** — architecture/security/performance implementation detail to be resolved outside the PRD.
- **COMPLIANCE DEPENDENCY** — external legal/privacy/retention validation.
- **FUTURE / OUT OF V1** — explicitly outside the locked V1 behavior.

---

# 3. Product Vision

Provide a simple, high-accountability clinic operating system that connects patient identity, reception, doctor consultation, prescription, pharmacy fulfilment, inventory control, payment recording, and longitudinal patient history without forcing small-clinic staff into complex hospital software.

The product must work for the current pilot:

- one clinic;
- one Owner who is also the only Doctor;
- Reception users;
- one pharmacy operation;

while supporting same-clinic growth to:

- multiple Doctors;
- multiple Reception users;
- multiple pharmacy units;
- an Owner who may or may not also be a Doctor.

---

# 4. Product Outcomes

## 4.1 Primary outcomes

**PO-01 — Fast patient handling**  
Reception can identify/register the patient, create the visit, record consultation payment, and place an eligible visit into the correct Doctor queue with low interaction overhead.

**PO-02 — Reliable longitudinal patient record**  
The Doctor can retrieve the current visit and relevant historical consultations, diagnoses, prescriptions, amendments, and superseded records against the permanent Patient ID.

**PO-03 — Clear queue coordination**  
Reception and Doctors can operate doctor-specific queues with visible states, calling, reassignment, non-response handling, and controlled cancellation.

**PO-04 — Simple clinical workflow**  
Doctor entry remains intentionally low-complexity while preserving a reliable consultation and prescription history.

**PO-05 — Connected prescribing and pharmacy**  
The Doctor sees pharmacy availability while prescribing; Pharmacy sees the finalized prescription and records actual fulfilment.

**PO-06 — Inventory accountability**  
Normal dispensing automatically reduces stock, while non-dispensing changes, loss/damage/correction, transfers, and relevant price changes require traceable Owner control.

**PO-07 — Financial record integrity**  
The CRM records but does not process money. Payment, waiver, cancellation, void, and correction behavior remains explicit and auditable.

**PO-08 — Role separation without duplicate accounts**  
Owner, Doctor, Reception, Pharmacist, and Administrator authority stays distinct while one person may hold multiple roles on one account.

---

# 5. Non-Goals for V1

The PRD must not introduce the following into V1:

- laboratory management;
- inpatient/bed management;
- insurance processing;
- ambulance management;
- HR/payroll;
- full pharmacy purchasing/supplier management;
- payment gateway processing;
- medicine returns;
- offline mode;
- thermal receipt printing;
- general pharmacy retail without a current finalized CRM prescription;
- duplicate-record merge;
- QR/barcode workflow;
- broad patient CRM/reminder campaigns.

---

# 6. Product Principles

## 6.1 Role-first simplicity

Each user sees the work needed for that role without exposing unrelated controls.

## 6.2 Fast common path, explicit exceptional path

Normal flows such as patient search, payment recording, queue entry, prescribing, and dispensing should require minimal decision overhead. Exceptions such as waiver, cancellation, payment correction, inventory adjustment, and stock transfer use explicit request/approval flows.

## 6.3 No silent destructive change

Important clinical, financial, role, and inventory records are amended, superseded, voided, or corrected with history rather than silently overwritten.

## 6.4 Active work and history are visually distinct

Users should clearly understand whether they are viewing:

- current/active work;
- a pending approval;
- a cancelled/voided item;
- a superseded prescription;
- a historical record.

## 6.5 Authority follows action type

- clinical decision -> Doctor;
- ownership/financial/inventory control -> Owner;
- intake/queue operation -> Reception;
- dispensing/inventory request -> Pharmacist;
- non-clinical account/configuration administration -> Administrator within allowed boundaries.

## 6.6 Clinic-unit context must be visible

Where multiple Doctors or pharmacy units exist, the user should never have to guess which Doctor queue, pharmacy unit, or workspace an action belongs to.

---

# 7. Users and Product Jobs

## 7.1 Owner

Primary jobs:

- understand clinic-wide operations;
- review inventory and financial-control exceptions;
- approve/reject waivers, consultation cancellations, pharmacy bill voids, payment corrections, inventory adjustments, and pharmacy transfers;
- review audit history;
- oversee staff/account access;
- see consolidated and unit-level inventory/revenue status.

Owner role alone does not grant unrestricted clinical content.

## 7.2 Doctor

Primary jobs:

- see assigned queue;
- call patients;
- open the active visit;
- review relevant longitudinal history;
- record consultation;
- prescribe;
- see pharmacy availability;
- approve/reject medicine substitution;
- amend completed clinical content;
- replace/supersede erroneous finalized prescriptions;
- request visit cancellation.

Doctor role alone does not grant Owner controls.

## 7.3 Reception

Primary jobs:

- find or register patient;
- create Visit;
- record consultation payment;
- request waiver;
- assign/reassign Doctor;
- manage queue events;
- coordinate Doctor call;
- request patient-demographic correction.

## 7.4 Pharmacist

Primary jobs:

- retrieve finalized prescription;
- see current/previous prescriptions and allergies;
- dispense available quantity;
- record unsupplied quantity;
- prepare pharmacy bill;
- record external payment;
- request medicine substitution;
- submit inventory adjustments/transfers;
- request pharmacy-bill cancellation/void.

## 7.5 Administrator

Primary jobs:

- create/disable non-Owner staff accounts;
- assign non-Owner roles;
- manage permitted non-clinical configuration;
- manage medicine/inventory configuration where allowed;
- access authorized non-clinical operational/reporting/audit views.

Administrator cannot grant/revoke Owner authority, disable Owner, or inherit clinical authority.

---

# 8. Workspace Model

**Classification: DERIVED PRODUCT DESIGN based on locked role separation.**

The application should expose role-based workspaces.

A user sees only workspaces granted by their roles.

Recommended V1 workspaces:

1. **Reception**
2. **Doctor**
3. **Pharmacy**
4. **Owner**
5. **Administration**

A multi-role user uses one account and can switch between permitted workspaces.

For an Owner + Doctor account, Owner and Doctor actions remain visually separated so an action's authority is obvious.

### P-001 — Workspace identity

The active workspace must always be visible.

### P-002 — Role-safe navigation

Navigation must not expose actions the current account lacks permission to perform.

### P-003 — Multi-role switching

A multi-role account must switch workspace without requiring a second account.

### P-004 — Context preservation

Switching workspace must not silently change the patient, visit, Doctor queue, pharmacy unit, or record being acted on without explicit user selection.

---

# 9. Global Application Shell

**Classification: DERIVED PRODUCT DESIGN.**

The application shell should contain:

- clinic identity;
- active workspace;
- signed-in user identity;
- active role/workspace indicator;
- notification/approval indicator where applicable;
- safe logout;
- navigation appropriate to the current workspace.

For pharmacy-scoped users, active pharmacy-unit context must be visible when more than one pharmacy exists.

### P-005 — Permission-aware shell

Unavailable modules/actions are hidden or disabled based on effective role permissions.

### P-006 — Clear status language

System status labels must use the locked vocabulary, including Paid, Unpaid, Waived, Waiting, Called, Unresponded, With Doctor, Consultation Completed, Sent to Pharmacy, Completed, Cancelled/Voided, In Stock, Out of Stock, Not Stocked, and Superseded.

### P-007 — Explicit final actions

Financial, cancellation, approval, prescription-finalization, and inventory-adjustment actions require explicit final confirmation rather than triggering merely by selecting an option.

---

# 10. Authentication and Account Entry

Source: FR-084–FR-085, FR-100–FR-104; BR-035, BR-043–BR-046; OD-021.

### P-008 — Individual account login

Every user signs in through an individual username/login and password in the fixed clinic context.

### P-009 — Owner TOTP

Any account containing Owner role must complete Google Authenticator-compatible TOTP after password authentication.

### P-010 — Non-Owner login

Doctor-only, Reception, Pharmacist, and Administrator-only accounts do not require 2FA in V1.

### P-011 — Staff Forgot Password

Non-Owner staff can submit Forgot Password, which creates a reset request for Owner.

### P-012 — Owner reset

Owner can set a temporary/reset credential for the staff account without viewing the existing password.

### P-013 — Forced credential replacement

After successful login using an Owner-reset credential, staff must replace it before normal application use.

### P-014 — Owner recovery codes

Owner TOTP enrollment/regeneration exposes one-time recovery codes according to the locked derived security control.

### P-015 — Disabled account

Disabled staff accounts cannot sign in and remain preserved for historical attribution.

### Product states

- login default;
- invalid credentials;
- TOTP required;
- invalid/expired TOTP;
- reset request submitted;
- reset credential must be changed;
- account disabled.

Catastrophic Owner recovery remains a technical security dependency.

---

# 11. Reception Workspace

## 11.1 Reception Home

**Classification: DERIVED PRODUCT DESIGN.**

Reception Home should optimize the common clinic flow around:

- patient search;
- New Patient;
- active Doctor queues;
- visit/payment state;
- pending reception-originated requests;
- Doctor call notifications.

### P-016 — Search-first entry

Patient search is the primary entry action before new registration.

### P-017 — Search keys

Reception can search by Patient ID, phone, or name.

### P-018 — Shared-phone handling

Search results must not imply that a matching phone uniquely identifies a patient.

### P-019 — Duplicate candidates

When similar records exist, candidates are shown for receptionist + patient confirmation; similarity must never auto-select or auto-merge the patient.

### P-020 — Possible Duplicate

If identity cannot be confidently matched, Reception may create a normal patient record carrying a visible Possible Duplicate marker.

---

# 12. Patient Registration and Patient Profile

Source: FR-001–FR-007, FR-075–FR-079; BR-001–BR-005, BR-025.

## 12.1 New Patient

Required fields:

- full name;
- phone;
- DOB;
- gender;
- address;
- email.

Optional:

- emergency contact;
- blood group;
- known allergies;
- guardian/parent details;
- Government ID.

Age is derived from DOB.

### P-021 — Permanent ID

Successful registration creates a permanent Patient ID.

### P-022 — Non-semantic ID presentation

Displayed Patient/Visit IDs use stable non-semantic human-readable values; they must not expose DOB, phone, diagnosis, or other personal meaning.

### P-023 — Patient profile summary

Patient profile should clearly separate:

- identity/demographics;
- alerts/allergies;
- active visit if one exists;
- longitudinal visit history;
- Possible Duplicate marker if applicable.

### P-024 — Demographic correction

Reception does not directly overwrite established demographics. Reception submits a correction request to the Doctor assigned to the active Visit. Doctor may approve/reject, while Doctor can also directly correct demographics with audit history.

### P-025 — No physical-file dependency

Missing physical paper file must not force creation of another Patient ID.

---

# 13. Visit Creation and Consultation Payment

Source: FR-008–FR-016, FR-108–FR-114; BR-006, BR-010, BR-019–BR-022, BR-033, BR-047–BR-055.

## 13.1 Create Visit

### P-026 — New Visit ID

Every attendance creates a unique Visit ID linked to the permanent Patient ID.

### P-027 — Doctor assignment

Reception selects a Doctor before an eligible visit enters the queue.

### P-028 — Consultation fee display

The applicable consultation fee is shown from clinic configuration. Fee values are configuration, not PRD policy.

## 13.2 Fast payment capture

Normal consultation payment methods:

- UPI;
- Cash;
- Card;
- Other.

Selecting Other requires a text description.

Payment reference is optional.

### P-029 — External payment

The CRM does not initiate or settle the payment.

### P-030 — Fast Paid action

Amount is prefilled where known, Reception selects payment method, optionally enters reference, and explicitly confirms Paid.

### P-031 — Queue gate

- Paid -> eligible;
- Waived -> eligible;
- Unpaid -> blocked.

### P-032 — No partial consultation payment

Partial consultation payment is not available.

### P-033 — No consultation refund

A paid consultation is non-refundable in V1.

## 13.3 Waiver

### P-034 — Request waiver

Reception or Doctor can submit waiver request with mandatory reason.

### P-035 — Owner approval

Only Owner authority approves/rejects the waiver.

### P-036 — Owner direct waiver

Owner may initiate an immediate approved Waived outcome with mandatory reason/audit.

### P-037 — Pending waiver gate

Pending/rejected waiver does not make an Unpaid Visit queue-eligible.

## 13.4 Incorrect payment record

### P-038 — Payment correction request

Reception/Pharmacy cannot silently rewrite a payment state. Staff submit the proposed correction plus specific reason to Owner.

### P-039 — Payment correction decision

Owner approval creates the corrected effective state while preserving original state and request history. Rejection keeps current state unchanged.

---

# 14. Doctor Queue

Source: FR-017–FR-025, FR-080–FR-081, FR-092–FR-093; BR-007–BR-010, BR-026–BR-029.

## 14.1 Queue view

Each Doctor has a separate ordered queue.

Reception sees the Doctor queues needed to operate reception.

Owner sees clinic-wide queue status.

### P-040 — Queue row information

Each queue row should expose the minimum operational context required to act:

- queue position;
- patient identity;
- Visit ID;
- current queue state;
- payment eligibility state;
- relevant timestamps;
- actions allowed for the current role.

### P-041 — Doctor call

Doctor can call a selected queued patient. Reception receives the call action and physically calls/directs the patient.

### P-042 — Reassignment

Reception can move an active queued Visit to another Doctor queue. Reassignment is audited.

### P-043 — Unresponded

Reception can mark a Called patient Unresponded. The Visit is moved five positions down; if fewer than five later positions exist, it moves to the end, then returns to Waiting.

### P-044 — Patient leaves

A paid patient who leaves before consultation is moved to the end of the assigned Doctor queue.

### P-045 — Urgent case

No software priority control is shown. Urgent escalation remains direct Reception-to-Doctor communication outside the CRM.

## 14.2 Consultation cancellation

### P-046 — Doctor request

Doctor can request cancellation while the Visit remains active and must provide a specific reason.

### P-047 — Owner decision

Owner approves/rejects. Approved Visit becomes Cancelled/Voided and exits active workflow while preserving all existing history. Completed Visits are corrected through amendment/correction flows instead of cancellation.

---

# 15. Doctor Consultation Workspace

Source: FR-026–FR-032, FR-091; BR-011–BR-013, BR-034, BR-036, BR-038, BR-054.

## 15.1 Consultation layout

**Classification: DERIVED PRODUCT DESIGN.**

Recommended workspace structure:

1. patient identity header;
2. current Visit context;
3. allergies/important patient context;
4. relevant longitudinal history;
5. current consultation entry;
6. prescription area/action.

### P-048 — Assigned Visit access

Doctor opens the active Visit from the Doctor queue.

### P-049 — Longitudinal history

Doctor assigned to the current Visit can access relevant prior consultations, diagnoses, prescriptions, amendments, and superseded records.

### P-050 — Required clinical entry

Doctor consultation must support:

- chief complaint / patient problem;
- clinical assessment / diagnosis.

### P-051 — Optional clinical entry

Support optional:

- symptoms/history;
- examination findings;
- clinical notes;
- advice;
- follow-up information.

### P-052 — Complete consultation

Doctor can complete the consultation and advance the Visit according to the locked workflow.

### P-053 — Clinical amendment

Completed clinical content is not edited in place. Doctor creates an amendment/revision with mandatory reason; original remains visible in history.

### P-054 — Clinical access boundary

Owner-only, Admin-only, Reception, and Pharmacist authority does not expose unrestricted full clinical-note/diagnosis content.

---

# 16. Prescription Builder

Source: FR-033–FR-044, FR-116; BR-014–BR-016, BR-030, BR-058; OD-010–OD-012, OD-037.

## 16.1 Medicine selection

Search/select by:

- clinic medicine/display name;
- strength;
- dosage form.

Manufacturer is available as medicine/inventory context.

Optional generic/molecule may be searchable.

### P-055 — Availability at prescribing

Doctor sees In Stock, Out of Stock, or Not Stocked.

### P-056 — Multi-pharmacy availability

When multiple pharmacy units exist, Doctor can see clinic total and per-unit availability. Doctor cannot modify stock.

### P-057 — Unavailable prescribing

Out of Stock or Not Stocked does not block prescribing.

## 16.2 Prescription instructions

Per medicine:

- medicine;
- strength;
- dose amount;
- frequency;
- duration;
- optional timing/food/short instruction.

### P-058 — Quantity calculation

If deterministic, quantity is automatically calculated; otherwise Doctor enters it.

### P-059 — Finalize prescription

Doctor explicitly finalizes prescription.

### P-060 — Immutable finalized prescription

Finalized prescription is not edited in place.

### P-061 — Replacement prescription

Doctor corrects an erroneous finalized prescription by creating replacement, entering mandatory reason, and marking old prescription Superseded.

### P-062 — Prior dispensing preserved

Dispensing already performed against a superseded prescription remains preserved.

## 16.3 Prescription output

### P-063 — A4 print

Doctor can print/reprint finalized prescription without creating an unrelated version.

### P-064 — ** availability marker

At finalization, medicines with clinic-wide Out of Stock or Not Stocked status are marked ** with legend. Later partial dispensing does not retroactively change the prescription.

---

# 17. Pharmacy Workspace

Source: FR-045–FR-063, FR-082–FR-083, FR-105–FR-118; BR-013, BR-017–BR-018, BR-040–BR-042, BR-047–BR-058.

## 17.1 Pharmacy Home

**Classification: DERIVED PRODUCT DESIGN.**

Recommended focus:

- Patient ID / prescription lookup;
- current pharmacy unit;
- pending dispensing;
- pharmacy bill/payment;
- substitution requests;
- inventory alerts;
- inventory-change/transfer requests.

### P-065 — Patient ID lookup

Patient ID is the required V1 pharmacy lookup.

### P-066 — Clinical visibility

Pharmacist sees:

- current prescription;
- previous prescriptions;
- known allergies;
- dispensing-relevant instructions.

No unrestricted diagnosis/full clinical notes.

## 17.2 Dispensing

### P-067 — Dispense actual quantity

Pharmacist records quantity actually supplied per prescription item.

### P-068 — Automatic stock deduction

Inventory decreases only by actual quantity dispensed.

### P-069 — Partial dispensing

If available quantity is lower than prescribed quantity, Pharmacy can supply available quantity, bill only supplied quantity, and mark remainder unsupplied.

### P-070 — No back-order

V1 does not create collect-later reservations.

### P-071 — Over-dispense prevention

Cumulative quantity across all dispensing actions/pharmacy units cannot exceed active prescription quantity.

### P-072 — Substitution request

Pharmacist cannot independently substitute. Pharmacist requests alternative; Doctor approves/rejects before dispensing.

### P-073 — No medicine return

No medicine-return workflow exists in V1.

### P-074 — Prescription-required dispensing

General non-prescription retail is not available in V1 CRM.

## 17.3 Multi-pharmacy fulfilment

### P-075 — Unit-specific dispensing

Every dispensing transaction is associated with the pharmacy unit that supplied the medicine.

### P-076 — Unit-specific billing

If multiple units fulfil one prescription, each unit bills only the quantity it dispensed.

---

# 18. Pharmacy Billing and Payment

## 18.1 Bill

### P-077 — Bill only supplied items

Only actually supplied quantities appear as dispensed bill items.

### P-078 — External payment recording

Pharmacy records Paid/Unpaid and UPI/Cash/Card/Other using the same V1 payment model.

### P-079 — No partial pharmacy payment

Partial pharmacy payment is unavailable.

### P-080 — No pharmacy refund

Refunds are unavailable in V1.

## 18.2 Bill cancellation / void

### P-081 — Pharmacist void request

Pharmacist submits bill cancellation/void request with specific reason.

### P-082 — Pending bill remains active

Pending cancellation request does not alter active bill.

### P-083 — Owner decision

Owner approve -> bill exits active billing and becomes Cancelled/Voided. Reject -> bill stays active. Original bill/payment state and decision history remain preserved.

### P-084 — No automatic stock restoration

Bill void does not reverse dispensing or restore inventory.

---

# 19. Inventory Workspace

Source: FR-051, FR-068, FR-086–FR-090, FR-094–FR-096, FR-115–FR-118; BR-017, BR-031–BR-032, BR-037, BR-040–BR-041, BR-056, BR-058.

## 19.1 Inventory model

Inventory supports:

- base stock/dispensing unit;
- configured package conversions;
- batch/lot;
- expiry;
- manufacturer;
- purchase price;
- selling price;
- low-stock threshold;
- near-expiry threshold.

### P-085 — Pharmacy-unit stock

Each pharmacy unit maintains independent stock ledger.

### P-086 — Consolidated Owner view

Owner can see per-unit and clinic-total inventory.

### P-087 — Automatic movement visibility

Normal prescription dispensing appears as an attributable stock movement without Owner approval.

## 19.2 Manual/non-dispensing inventory changes

### P-088 — Change request

Pharmacist submits stock addition, loss, damage, correction, or permitted price-change request with reason.

### P-089 — No change while pending

The inventory record does not change until Owner approval.

### P-090 — Owner approve/reject

Decision, requester, reason, resulting stock change, and timestamps are auditable.

### P-091 — Expired stock

Expired stock cannot be dispensed. Owner receives the relevant disposition/adjustment control; physical disposal process is outside CRM scope.

## 19.3 Pharmacy transfer

### P-092 — Linked transfer

Inter-pharmacy transfer is one linked transaction with source, destination, medicine, quantity, requester, Owner decision, and shared transfer reference.

### P-093 — Atomic business outcome

Approved transfer decreases source and increases destination as one controlled business event; rejected transfer changes neither.

---

# 20. Owner Workspace

**Classification: DERIVED PRODUCT DESIGN based on Owner authority.**

The Owner workspace should consolidate clinic-wide control without exposing clinical content that requires Doctor role.

Recommended areas:

1. approvals;
2. inventory;
3. operations;
4. financial-status/revenue reporting;
5. staff/account oversight;
6. audit activity.

## 20.1 Approval Center

A single Owner Approval Center is recommended for:

- consultation waiver;
- consultation/Visit cancellation;
- pharmacy bill cancellation/void;
- payment correction;
- inventory adjustment;
- pharmacy stock transfer;
- staff password reset requests.

### P-094 — Approval queue

Each approval item shows:

- request type;
- requester;
- affected patient/Visit/bill/medicine/account as appropriate;
- specific reason;
- relevant current state;
- created time;
- Approve / Reject actions.

### P-095 — Approval audit

Owner decision records decision, actor, time, and resulting effective state.

### P-096 — Role-aware Owner + Doctor

When the same account is Owner + Doctor, the product must still record which authority was used for the action.

---

# 21. Administration Workspace

### P-097 — Staff account management

Admin/Owner can create staff accounts within their authority.

### P-098 — Non-Owner role management

Admin can assign/remove non-Owner roles.

### P-099 — Owner-role protection

Admin cannot grant/revoke Owner, modify Owner authority, or disable Owner.

### P-100 — Disable instead of delete

Staff leaving the clinic are disabled rather than deleted.

### P-101 — Configuration

Authorized users can manage applicable non-clinical configuration such as medicine catalogue, inventory conversion/threshold configuration, consultation fee values, pharmacy prices/tax values where applicable, and optional A4 output configuration.

Configuration permissions must respect the locked Owner/Admin boundaries.

---

# 22. Reporting and Owner Visibility

Source: BRD Section 8; OD-029.

V1 reports:

- patients seen per day;
- average waiting time;
- consultation revenue;
- pharmacy revenue;
- daily total revenue;
- medicine sales;
- current stock;
- low-stock medicines;
- out-of-stock medicines;
- expiring medicines;
- most-prescribed medicines;
- waivers;
- cancellations;
- returning patients;
- audit activity.

### P-102 — Owner reports

Owner can access clinic-wide operational, financial-status, inventory, approval, and audit reporting.

### P-103 — Admin reports

Authorized Admin sees non-clinical operational/revenue/inventory reports and authorized audit metadata.

### P-104 — Pharmacy reports

Pharmacist sees pharmacy/inventory reports for permitted pharmacy scope.

### P-105 — Reception operational view

Reception sees queue/reception operational information required for work without unrestricted clinical reporting.

### P-106 — Doctor reporting boundary

Doctor-only role does not inherit Owner clinic-wide financial/inventory reporting.

No numeric success target or chart layout is invented by this PRD.

---

# 23. Audit and History UX

### P-107 — Human-readable history

Where a record is amended, replaced, corrected, cancelled, or approved, authorized users must be able to distinguish the current effective state from historical state.

### P-108 — Attribution

Audit history includes actor and time and, where applicable, reason, prior state/value, resulting state/value, requester, and approver.

### P-109 — No normal-UI audit deletion

Audit events are not deletable through normal application UI.

### P-110 — Clinical-content boundary in audit

Owner/Admin audit access may expose event metadata without automatically revealing full clinical content.

---

# 24. Product State and Error Handling Requirements

**Classification: DERIVED PRODUCT DESIGN.**

Every major data-driven screen should explicitly support:

- loading;
- populated;
- empty;
- permission denied;
- record not found;
- validation error;
- request pending;
- success;
- rejected/failed action where relevant.

The product must not convert an error into silent success.

### P-111 — Retry safety

Retrying a user action must not intentionally create duplicate Patient, Visit, payment, dispensing, bill, adjustment, transfer, or approval records.

Exact technical idempotency implementation belongs to architecture.

### P-112 — Stale-state protection

When an action depends on current state, the product must detect that the record changed before silently applying an outdated decision.

Implementation mechanism belongs to architecture.

---

# 25. Usability Requirements

## 25.1 Reception and Pharmacy speed

The design should minimize unnecessary steps in high-frequency workflows:

- patient search;
- Visit creation;
- payment recording;
- queue entry;
- prescription lookup;
- dispensing;
- pharmacy payment recording.

No arbitrary numeric interaction or latency target is asserted until product/technical performance targets are separately approved.

## 25.2 Low-knowledge operational use

Reception/Pharmacy flows should use plain labels and role-appropriate language.

Clinical simplification must not transfer Doctor authority to non-Doctors.

## 25.3 Confirmation design

High-impact actions require clear confirmation and reason capture where required:

- prescription finalize/replacement;
- waiver;
- cancellation/void;
- payment correction;
- inventory adjustment;
- pharmacy transfer;
- role/Owner lifecycle action.

---

# 26. Product Notifications

**Classification: DERIVED PRODUCT DESIGN.**

V1 requires in-product attention mechanisms for workflow items already required by the BRD.

At minimum:

- Doctor call visible to Reception;
- Owner waiver request;
- Owner cancellation/void request;
- Owner payment-correction request;
- Owner inventory-adjustment request;
- Owner stock-transfer request;
- Owner staff password-reset request;
- Doctor substitution request;
- low-stock notification;
- near-expiry notification.

This section does not introduce email/SMS/push integrations.

---

# 27. Printing and Output

### P-113 — Prescription A4 output

Support finalized prescription print/reprint.

### P-114 — Unavailable medicine legend

Printed ** marker behavior follows P-064.

### P-115 — Pharmacy A4 output

A pharmacy bill/dispensing summary may be printed on standard A4 where the clinic needs physical output.

### P-116 — No thermal dependency

No V1 workflow depends on thermal printer.

---

# 28. Product Analytics / Measurement

The product must calculate the locked V1 reporting definitions from recorded events.

The PRD does not invent additional KPI targets.

Product analytics implementation should distinguish:

- prescribed quantity vs dispensed quantity;
- Paid vs Waived;
- active vs Cancelled/Voided;
- clinic total vs pharmacy-unit inventory;
- requested vs approved/rejected control actions.

---

# 29. Product Dependencies

## 29.1 Configuration dependencies

Before go-live:

- consultation fee values;
- any doctor/service-specific fee values;
- medicine catalogue;
- package conversion factors;
- purchase/selling prices;
- low-stock thresholds;
- near-expiry thresholds;
- pharmacy unit definitions;
- staff accounts/roles;
- optional clinic-specific A4 receipt/acknowledgement format;
- applicable price/tax configuration.

## 29.2 Technical dependencies

Outside this PRD:

- hosting/cloud;
- database/API architecture;
- backup/recovery targets;
- performance engineering;
- session implementation;
- encryption/key management;
- catastrophic Owner recovery implementation.

## 29.3 Compliance dependencies

Externally validate:

- privacy/patient-data obligations;
- retention;
- audit retention/export;
- incident/breach handling;
- printed patient-data handling;
- applicable healthcare requirements.

These dependencies must not redefine locked business behavior without change control.

---

# 30. Release Scope

V1 product release is functionally complete only when the accepted implementation covers:

1. authentication and role workspaces;
2. patient registration/retrieval;
3. Visit creation and payment gate;
4. doctor-specific queue;
5. consultation;
6. prescription and A4 output;
7. pharmacy retrieval/dispensing;
8. pharmacy billing/payment recording;
9. inventory tracking/control;
10. Owner approvals;
11. staff/account controls;
12. audit/history;
13. locked V1 reports.

---

# 31. Release Readiness Gates

A release candidate must not be considered product-ready if any of the following are true:

- a role can perform an action outside its locked authority;
- a payment/clinical/inventory material record can be silently overwritten;
- pharmacy can over-dispense a prescription;
- expired stock can be dispensed;
- bill void can automatically restore already-dispensed stock;
- Admin can self-grant Owner authority;
- Owner-only account can access unrestricted clinical content without Doctor role;
- unpaid/unwaived Visit can enter Doctor queue;
- finalized prescription can be edited in place;
- cancelled/voided history disappears;
- critical approval request lacks requester/reason/decision history;
- multi-pharmacy stock movements lose unit attribution.

Detailed acceptance scenarios are maintained in Document 06.

---

# 32. PRD Change Control

This PRD is currently **DRAFT**.

During PRD iteration:

- product/UX decisions may be refined;
- no refinement may contradict locked BRD behavior;
- any genuine business-policy change must return to BRD change control first;
- accepted PRD changes should preserve P-IDs once they become implementation references.

PRD lock will occur only after product behavior, interaction requirements, acceptance criteria, and BRD traceability have been reviewed and reconciled.
