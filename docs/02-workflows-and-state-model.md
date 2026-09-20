# Hospital CRM — Workflows and State Model

## 1. Purpose

This document defines the currently confirmed end-to-end workflow and the business states that connect reception, doctor consultation, prescription, pharmacy fulfilment, and payment.

It does not invent unconfirmed exception behavior. Open rules are referenced explicitly.

---

# 2. End-to-End Patient Journey

```text
Patient arrives
    |
    v
Reception searches for patient
    |
    +--> Existing patient found -> reuse Patient ID
    |
    +--> New patient -> create patient -> generate Patient ID
    |
    v
Create Visit ID
    |
    v
Consultation payment/charge handling
    |
    v
Queue eligibility check: Paid or Owner-approved Waived -> eligible; Unpaid -> blocked
    |
    v
Add visit to consultation queue
    |
    v
Doctor views queue
    |
    v
Doctor calls patient through reception
    |
    v
Consultation starts
    |
    +--> Symptoms / history
    +--> Diagnosis
    +--> Clinical notes
    |
    v
Prescription creation
    |
    +--> Search/select/type medicine
    +--> Show pharmacy availability
    +--> In Stock / Out of Stock / Not Stocked
    |
    v
Finalize prescription
    |
    v
Print prescription
    |
    v
Patient goes to pharmacy
    |
    v
Pharmacy retrieves patient/visit/prescription
    |
    +--> Dispense available medicines
    +--> Explain unavailable medicines
    +--> Record actual dispensed quantity
    +--> Reduce stock
    |
    v
Prepare pharmacy bill
    |
    v
Record pharmacy payment
    |
    v
Visit reaches completed state when applicable
```

---

# 3. Patient Identity Lifecycle

## 3.1 Permanent Patient Record

A Patient ID is permanent for a patient and survives across multiple clinic visits.

Example:

```text
Patient: PAT-000124

Visit 1 -> VIS-001929
Visit 2 -> VIS-002381
Visit 3 -> VIS-004529
```

## 3.2 Patient Retrieval

Reception must be able to search using:

- Patient ID
- phone number
- patient name

Phone number and name are matching/search attributes. They are not unique patient identifiers by themselves.

Multiple patients may intentionally share the same phone number, including members of the same family.

## 3.3 Registration Information

Required registration information:

- full name;
- phone number;
- date of birth;
- gender;
- address;
- email.

Age is calculated from date of birth rather than independently maintained.

Optional registration information:

- emergency contact;
- blood group;
- known allergies;
- guardian/parent details;
- Government ID.

Government ID is not required.

## 3.4 Duplicate Handling

The system must help prevent or detect accidental duplicate registration.

When similar existing records are found:

1. reception reviews the candidate details with the patient;
2. reception asks the patient to confirm whether a candidate is their existing profile;
3. prior visit history and the remembered purpose of a previous visit may be used to help confirmation;
4. if the patient confirms the candidate, reception uses the existing patient profile.

If the patient cannot confidently confirm any candidate, reception may create a new patient profile and the system marks it **Possible Duplicate**. That profile remains usable like a normal patient profile.

Implementation/future notes:

- exact similarity-scoring thresholds are solution-design details; the system may surface candidates but never auto-merge or auto-select solely from a similarity score;
- duplicate-record merge is not part of V1 and may be evaluated in a later version.

---

# 4. Visit Lifecycle

Each attendance creates a separate Visit ID.

Confirmed visit-level associations include:

- queue state;
- consultation record;
- diagnosis;
- clinical notes;
- prescription;
- consultation payment event;
- pharmacy fulfilment;
- pharmacy payment event.

## 4.1 Baseline Visit State Model

The accepted operational queue/visit states are:

1. **Waiting**
2. **Called**
3. **Unresponded** — transient queue event/state before returning to Waiting at the repositioned slot
4. **With Doctor**
5. **Consultation Completed**
6. **Sent to Pharmacy**
7. **Completed**
8. **Cancelled/Voided**

## 4.2 Baseline State Flow

```text
Waiting
   |
   v
Called
   | \
   |  \ no response
   |   v
   | Unresponded
   |   |
   |   +--> reposition -> Waiting
   v
With Doctor
   |
   v
Consultation Completed
   |
   v
Sent to Pharmacy
   |
   v
Completed
```

Cancellation is not a direct unrestricted state transition. A Doctor submits a cancellation request with a specific reason; the visit remains active while pending. Owner approval moves it to **Cancelled/Voided** and removes it from active workflow. Owner rejection leaves the active state unchanged. Reception cannot initiate visit cancellation.

---

# 5. Reception Workflow

## 5.1 Existing Patient

1. Reception searches by Patient ID, phone number, or patient name.
2. Reception selects the correct patient.
3. Reception creates a new Visit ID.
4. Reception handles the consultation payment/charge record.
5. The visit becomes queue-eligible only when consultation status is Paid or Owner-approved Waived.
6. Reception adds or confirms the visit in the consultation queue.

## 5.2 New Patient

1. Reception captures required patient information.
2. System generates a permanent Patient ID.
3. Patient ID is associated with the physical clinic file.
4. Reception creates the patient's first Visit ID.
5. Reception handles consultation payment/charge.
6. Queue eligibility is evaluated: Paid or Owner-approved Waived is eligible; Unpaid is blocked.
7. Visit enters the consultation queue.

## 5.3 Returning Patient Without Physical File

Confirmed intent:

- the existing digital patient record must still be retrievable;
- the patient must not receive a new Patient ID merely because the physical file is unavailable.

Locating or replacing the physical paper file is an offline clinic procedure and is not a V1 CRM workflow dependency. The digital patient identity remains usable regardless of the paper file's availability.

---

# 6. Consultation Payment State Model

Consultation payment must be stored as an auditable payment-status/information record rather than an unaudited checkbox.

V1 does not process consultation payments. Reception records payment status/information after payment occurs outside the CRM.

Confirmed consultation rules:

- Paid -> eligible to enter the doctor queue.
- Unpaid -> must not enter the doctor queue unless an approved waiver applies.
- Partial consultation payment is not supported.
- Consultation payment is non-refundable in V1.
- If a paid consultation needs cancellation, a Doctor submits a request with a specific reason and the Owner approves/rejects it; approval does not refund the payment.

Confirmed queue rule:

- **Paid** -> eligible to enter the doctor queue.
- **Unpaid** -> must not enter the doctor queue.

Waived consultation flow:

### Reception- or Doctor-initiated request

1. Reception or a Doctor submits a waiver request.
2. A specific reason is mandatory.
3. The request is shown to the Owner.
4. Reception and Doctor role alone cannot approve it.
5. While pending or rejected, an Unpaid visit remains ineligible for the queue.
6. Owner approval records the consultation financial outcome as **Waived** and makes the visit queue-eligible.
7. Requester, approver, reason, decision, and timestamps are logged.

### Owner-initiated waiver

1. Owner may initiate a waiver directly.
2. No second approval step is required because the action already uses Owner authority.
3. The outcome is immediately **Waived**.
4. The visit becomes queue-eligible.
5. Reason and audit history remain mandatory.

A user who is both Owner and Doctor performs the approval through Owner authority, even though both roles are on the same account.

---

# 7. Queue and Calling Workflow

## 7.1 Doctor-Specific Queue Model

Consultation queues are doctor-specific.

Reception selects the doctor for an eligible visit before queue entry. The visit is then placed into that doctor's queue.

Each doctor must be able to:

- see waiting visits assigned to that doctor;
- identify queue order within that doctor's queue;
- identify current status;
- call the next or selected patient.

A doctor does not use a shared clinic-wide queue in the confirmed model.

## 7.2 Reception Call Coordination

When the doctor triggers the call action:

1. the selected queue entry moves to the appropriate called state;
2. reception is informed;
3. reception physically calls/directs the patient.

## 7.3 Urgent Cases and Remaining Queue Rules

Urgent cases do not use a priority feature in the CRM.

If an urgent situation occurs:

1. the receptionist directly informs the doctor outside the software;
2. the CRM does not create a priority request, priority flag, or automated priority reorder for that situation.

Confirmed additional queue behavior:

- reception may reassign a queued visit from one doctor to another doctor-specific queue;
- every doctor reassignment is logged;
- if a called patient does not respond, reception marks the visit **Unresponded** and the system moves it five queue positions downward;
- if a paid patient leaves before consultation, reception moves the visit to the end of that doctor's queue;
- reception cannot cancel a visit;
- a doctor may submit a consultation/visit cancellation request with a specific reason;
- the Owner approves or rejects the request;
- approved cancellation removes the visit from the active queue/workflow and marks it Cancelled/Voided;
- cancelled/voided visits remain visible in history/audit;
- rejected requests leave the active visit unchanged and are logged.

Boundary rules:

- if fewer than five later queue positions exist for an Unresponded patient, move the visit to the end of the queue;
- if a paid patient leaves before consultation, move the visit to the end of the assigned doctor's queue.

---

# 8. Doctor Consultation Workflow

## 8.1 Consultation Start

The doctor opens the selected Visit ID.

The doctor can access:

- patient identity;
- relevant previous visits;
- current visit;
- prior clinical history required for care.

## 8.2 Clinical Documentation

The V1 consultation interface is intentionally low-complexity and guided so it can be operated without requiring complex medical-software knowledge.

The doctor-facing record uses plain labels with limited mandatory entry:

- chief complaint / patient problem;
- clinical assessment / diagnosis.

Optional entry includes:

- symptoms/history details;
- examination findings;
- clinical notes;
- advice;
- follow-up information.

Clinical decision functions remain within the Doctor role even though the interface itself is designed to be simple. In the current pilot the Owner also holds the only Doctor role; if additional doctors are added, each Doctor-role user receives clinical authority independently while Owner-only authority remains separate.

If a completed consultation needs correction, only a Doctor-role user may create an amendment/new revision. The original content remains preserved; amendment reason, actor, and timestamp are recorded.

## 8.3 Auditability

Material changes to clinical content must remain attributable.

Audit behavior preserves:

- original content;
- amendment/revision content;
- actor;
- time;
- mandatory amendment reason.

---

# 9. Prescription Workflow

## 9.1 Medicine Entry

The doctor identifies medicines using:

- clinic medicine/display name;
- strength ("power");
- dosage form.

Manufacturer is stored for pharmacy/inventory context.

Generic/molecule name may be stored as an additional searchable attribute, while the clinic display name remains the primary selection label.

## 9.2 Availability Status

Medicine availability must support:

- **In Stock**
- **Out of Stock**
- **Not Stocked**

## 9.3 Availability Rule

Clinic-pharmacy availability informs the doctor but does not automatically prohibit prescribing an unavailable medicine.

## 9.4 Prescription Instructions and Finalization

Prescription entry is deliberately minimal because detailed verbal explanation is expected during consultation.

For each medicine, V1 captures:

- medicine name;
- strength;
- dose amount;
- frequency;
- duration.

Short timing/food/extra instructions may be added when needed.

Where quantity can be deterministically calculated from dose, frequency, duration, and stock unit, the system calculates it automatically; otherwise the doctor enters quantity.

After finalization, the prescription cannot be edited in place. Pharmacy cannot edit it either.

If a finalized prescription contains an error:

1. the doctor creates a replacement prescription;
2. the old prescription becomes **Superseded**;
3. correction reason is mandatory;
4. actor/time are logged;
5. pharmacy defaults to the latest active prescription and sees a superseded-prescription warning;
6. any prior dispensing history remains preserved.

---

# 10. Printed Prescription Workflow

The printed prescription must:

- contain the prescribed medicines;
- at doctor finalization/printing time, mark medicines with clinic-wide status **Out of Stock** or **Not Stocked** using **;
- include a legend explaining that ** means the medicine was unavailable from the clinic pharmacy when the prescription was finalized and should be obtained externally.

A partial quantity discovered later during dispensing does not retroactively alter the original prescription. The pharmacy dispensing/billing summary records the actual supplied quantity and any unsupplied remainder.

Prescription reprinting must not silently create an unrelated new prescription record.

---

# 11. Pharmacy Retrieval Workflow

Pharmacy staff:

1. receive the patient;
2. retrieve the patient/visit using Patient ID;
3. open the relevant prescription;
4. see medicine instructions needed for dispensing;
5. see medicine availability;
6. see billable clinic-pharmacy items;
7. proceed with dispensing and billing.

Patient ID is the required pharmacy lookup method for locked V1. Additional lookup methods are not required by the V1 business scope.

---

# 12. Pharmacy Access Boundary

Pharmacy staff may access:

- patient identification needed for fulfilment;
- current prescription;
- previous prescriptions;
- known allergies;
- medicine instructions;
- quantity;
- availability;
- bill/price information;
- pharmacy payment information.

Pharmacy staff must not receive unrestricted access to diagnosis or full doctor clinical notes solely because they are dispensing medicines.

---

# 13. Dispensing and Inventory Workflow

For each prescribed medicine:

```text
Prescription item
   |
   v
Check clinic pharmacy status
   |
   +--> In Stock -> dispense quantity -> reduce inventory
   |
   +--> Out of Stock -> do not dispense -> tell patient to obtain outside
   |
   +--> Not Stocked -> do not dispense -> tell patient to obtain outside
```

## 13.1 Partial Availability

If prescribed quantity exceeds available stock:

1. pharmacy may dispense the quantity actually available;
2. the actual supplied quantity is recorded;
3. inventory decreases only by the supplied quantity;
4. billing includes only the supplied quantity;
5. the remaining quantity is identified to the patient as not supplied by the clinic pharmacy and should be obtained outside;
6. V1 does not maintain a back-order/collect-later workflow.

The system tracks cumulative dispensed quantity and must prevent dispensing more than the prescribed amount.

## 13.2 Substitution

Pharmacy cannot independently substitute a prescribed medicine.

If pharmacy proposes an alternative:

1. pharmacist raises a substitution request;
2. doctor approves or rejects it;
3. the request and decision are logged;
4. only an approved substitution may be dispensed.

## 13.3 Returns and Non-Prescription Sales

Medicine returns are not supported in V1.

V1 CRM dispensing requires a current finalized prescription. Non-prescription/pharmacy-only retail is outside the confirmed V1 workflow.

## 13.4 Inventory Tracking and Controlled Adjustments

Inventory is tracked from the lowest dispensable unit through higher packaging levels where applicable.

Inventory records support:

- batch/lot number;
- expiry date;
- manufacturer;
- purchase price;
- selling price.

The system provides:

- low-stock notifications;
- near-expiry notifications.

Threshold values are configurable by authorized Owner/Admin users.

Normal prescription dispensing automatically deducts actual dispensed quantity.

For pharmacy inventory upkeep or non-dispensing changes (for example stock additions, damage, loss, corrections, or price changes):

1. pharmacist submits an inventory-change request;
2. the request includes the proposed change and reason;
3. Owner reviews it;
4. inventory changes only after Owner approval;
5. request, decision, actor, reason, and resulting change are logged.

Expired stock is blocked from dispensing. The Owner is notified, and Owner action/approval controls the inventory disposition/adjustment record.

## 13.5 Multiple Pharmacy Units

A clinic may operate more than one pharmacy unit.

For each pharmacy unit:

- stock ledger is independent;
- dispensing is recorded against that pharmacy unit;
- pharmacy staff can be scoped to one or more pharmacy units;
- Owner can see both unit-level and consolidated inventory.

A prescription may be dispensed from different clinic pharmacy units if operationally needed, but cumulative dispensing across all units must not exceed the active prescription quantity.

A stock transfer between pharmacy units uses one linked transfer record:

1. source pharmacy requests/records transfer;
2. Owner approves;
3. source stock decreases;
4. destination stock increases;
5. both sides retain the same transfer reference and audit history.

## 13.6 Multi-Pharmacy Prescribing Availability

When more than one pharmacy unit exists, the Doctor prescribing view shows:

- clinic-wide availability;
- availability by pharmacy unit where stock is known.

This is view-only for the Doctor. Pharmacy/Owner inventory controls remain unchanged.

## 13.7 Multi-Pharmacy Dispensing and Billing

Every dispensing transaction belongs to the pharmacy unit that physically dispensed the medicine.

If one prescription is fulfilled by more than one clinic pharmacy unit:

- each unit records only the quantity it dispenses;
- each unit bills only the medicines/quantity it dispenses;
- cumulative dispensing across all units cannot exceed the active prescription quantity;
- Owner reporting may consolidate the unit-level activity without replacing the unit-level audit trail.

---

# 14. Pharmacy Billing and Payment Workflow

1. Pharmacy identifies medicines actually supplied.
2. Bill contains the medicines being supplied by the clinic pharmacy.
3. Medicines not supplied by the clinic pharmacy are not billed as dispensed items.
4. Patient is informed which medicines must be obtained outside.
5. Pharmacy records the externally completed payment status and payment method.

V1 does not process pharmacy payments.

Confirmed pharmacy payment rules:

- no partial pharmacy payments;
- no pharmacy refunds;
- normal payment state is Unpaid or Paid;
- default payment methods are **UPI**, **Cash**, **Card**, and **Other**;
- selecting **Other** requires a short description of the actual method;
- payment/reference number is optional;
- payment method is recorded for reconciliation/information;
- V1 does not process the payment itself.

## 14.1 Pharmacy Bill Cancellation / Void

If pharmacy needs a bill cancelled:

1. pharmacist selects **Request Cancellation/Void**;
2. pharmacist must enter a specific free-text reason;
3. request is sent to Owner;
4. bill remains active while the request is pending;
5. Owner approves or rejects.

If Owner approves:

- bill disappears from the active billing workflow;
- bill is marked **Cancelled/Voided**;
- original bill and any recorded payment state remain in history;
- requester, exact reason, Owner, decision, and timestamps are retained;
- approval does not create a refund.

If Owner rejects:

- bill remains active and unchanged;
- request and rejection remain in audit history.

## 14.2 Doctor-Side Cancellation

For a consultation/visit cancellation:

1. Doctor may submit a request while the visit is active: Waiting, Called, Unresponded, With Doctor, Consultation Completed, or Sent to Pharmacy;
2. Doctor enters a specific free-text reason;
3. Owner approves or rejects;
4. approved cancellation removes the visit from active operational workflow and marks it Cancelled/Voided;
5. all clinical, prescription, dispensing, and financial history already created remains intact;
6. a Completed visit is not cancelled through this flow; later corrections use the applicable amendment/correction workflow;
7. rejected cancellation leaves the visit active;
8. request/decision history is retained.

Even if the same person holds Owner + Doctor, the action should be recorded as a Doctor-side request followed by an Owner-authority decision so the two authorities remain explicit in audit history.

## 14.3 High-Velocity Payment Capture

V1 keeps actual payment execution outside the CRM:

- **UPI:** patient pays using the clinic's external UPI/QR/payment app; staff verify success externally and then mark Paid.
- **Cash:** staff physically collect cash and then mark Paid.
- **Card:** payment is completed on the external POS/card terminal; staff verify success and then mark Paid.
- **Other:** staff complete the external method, select Other, and type the method name/description.

Reception/pharmacy capture should be short:

1. amount is prefilled where the applicable consultation fee or pharmacy bill is known;
2. user selects UPI / Cash / Card / Other;
3. if Other, method description is required;
4. optional reference/transaction number may be entered;
5. user confirms the payment record.

Payment APIs/gateways are not in the V1 critical workflow.

## 14.4 Payment Correction

If Reception or Pharmacy staff mark a payment incorrectly:

1. the original payment record is not silently overwritten;
2. staff submit a payment-correction request with the proposed correction and a specific reason;
3. Owner approves or rejects;
4. approval creates the corrected effective state while preserving the original state, requester, reason, Owner decision, and timestamps;
5. rejection leaves the active payment state unchanged.

This is a record correction, not a refund.

## 14.5 Bill Void Does Not Restore Stock

Approving a pharmacy-bill cancellation/void does not reverse dispensing or restore stock automatically.

If a genuine stock correction is required, Pharmacy submits a separate inventory-adjustment request and Owner approves/rejects it through the inventory-control workflow.

---

# 15. Role-Oriented Workflow Summary

## 15.1 Receptionist

Reception is limited to:

- patient intake/retrieval;
- confirming demographic information;
- recording consultation payment status;
- assigning/reassigning the doctor queue;
- queue operations;
- submitting demographic-change requests.

Reception cannot directly modify existing patient demographics after registration; a change request is sent to the doctor.

Reception does not receive general access to diagnosis or clinical notes. Limited previous-visit information used for identity confirmation remains permitted only to support patient matching.

## 15.2 Doctor

Doctor access includes:

- doctor-specific queue;
- the patient's longitudinal visit/archive history needed for care when clinically authorized;
- current consultation record;
- diagnosis and notes;
- prescribing;
- payment/queue status;
- direct demographic edits and approval of reception demographic-change requests;
- substitution approval;
- submission of consultation/visit cancellation requests with a specific reason.

Doctor role alone does not grant Owner-only inventory-control, financial-waiver, staff-management, or clinic-wide oversight privileges.

## 15.3 Pharmacist

Pharmacist access includes:

- current prescription;
- previous prescriptions;
- known allergies;
- dispensing;
- pharmacy bill/payment-status functions;
- inventory-upkeep request entry.

Pharmacists cannot edit a doctor prescription directly.

Non-dispensing inventory changes submitted by pharmacy require Owner approval.

## 15.4 Owner

Owner access includes:

- clinic-wide operational dashboard;
- all pharmacy-unit inventory views and consolidated inventory;
- inventory-adjustment and stock-transfer approval;
- consultation-fee waiver approval;
- consultation/visit cancellation approval;
- pharmacy-bill cancellation/void approval;
- clinic-wide financial-status/revenue reporting;
- staff/account/group oversight;
- staff password-reset request handling;
- audit-log visibility.

Owner role alone does not grant diagnosis, prescription authoring, clinical amendments, or unrestricted clinical-note/diagnosis content. Full clinical content requires Doctor-role authority. If the owner is also a practicing doctor, the same account receives Doctor-role permission.

For a multi-role user, Owner and Doctor workspaces/modes should be visibly separated.

## 15.5 Administrator

Administrator responsibilities include:

- create/disable non-Owner staff accounts;
- assign non-Owner permission groups;
- manage non-clinical clinic configuration;
- manage medicine/inventory configuration where permitted;
- view operational/revenue/inventory reports;
- view authorized audit logs.

Administrator permission by itself does not grant clinical-authoring authority, unrestricted clinical content, or Owner authority. Clinical notes, diagnosis, prescriptions, amendments, and medicine-substitution decisions require Doctor permission. Financial waiver, inventory-control, cancellation approval, and Owner-role lifecycle actions require Owner permission.

Administrator cannot grant/revoke Owner role authority or disable an Owner account.

Staff accounts are disabled rather than deleted when a staff member leaves, preserving historical audit references.

## 15.6 Group-Based Access

Every staff member has an individual account.

Users may belong to one or more privilege groups such as:

- Owner;
- Doctor;
- Reception;
- Pharmacist;
- Administrator.

A user holding Owner + Doctor uses one individual account and gains both permission sets without creating a second identity.

Group membership determines role privileges.

V1 uses individual username/login + password accounts within the fixed single-clinic context.

Authentication rules:

- any account with the **Owner** role requires password + Google Authenticator-compatible TOTP 2FA before login completes;
- this also applies when the same account is Owner + Doctor or Owner + another role;
- Doctor-only, Reception, Pharmacist, and Administrator-only accounts do not require 2FA in V1.

### Staff Forgot-Password Workflow

For a non-Owner staff account:

1. staff member selects **Forgot Password**;
2. system creates a reset request for the Owner;
3. Owner sees the request in the Owner workspace;
4. Owner may set a new/temporary password for that staff account;
5. the existing password is never displayed or retrievable;
6. staff signs in using the reset credential;
7. system requires the staff member to choose a new password before continuing normal use;
8. reset request and Owner reset action are logged.

Owner 2FA uses Google Authenticator-compatible TOTP. TOTP enrollment generates one-time recovery codes that are shown only at enrollment/regeneration and are invalidated after use. If password/authenticator access and all recovery codes are unavailable, recovery requires a controlled technical recovery process.

---

# 16. Record Lifecycle Rules

Important records include:

- patient identity records;
- clinical notes;
- diagnoses;
- prescriptions;
- consultation payments;
- pharmacy payments;
- dispensing records.

Confirmed rule:

> Important records must not disappear through silent overwrite or unaudited hard deletion.

Correction or cancellation must preserve appropriate historical evidence.

---

# 17. Pilot Operating Model and Scale Flexibility

Current pilot:

- one clinic branch;
- clinic Owner is also the only Doctor;
- one separate pharmacy operation.

The same clinic model supports growth to:

- multiple Doctors with separate doctor queues;
- multiple Reception users sharing reception operations through individual accounts;
- multiple pharmacy units with separate inventory ledgers;
- an Owner who may or may not also hold the Doctor role.

Confirmed V1 deployment characteristics:

- web application;
- internet-dependent operation;
- no offline mode;
- normal A4 printing;
- no thermal receipt printer requirement.

Hosting/deployment provider remains a downstream technical-architecture choice.

# 18. Confirmed Reporting Set

V1 reporting includes:

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

Per-doctor patient count is not required for the current single-doctor pilot.

---

# 19. Post-Lock Configuration and Delivery Dependencies

The core business workflow is closed. The following do not reopen V1 business discovery:

1. clinic-supplied consultation fee values and pharmacy price/tax configuration;
2. clinic-specific A4 receipt/acknowledgement layout, if desired;
3. legal/privacy/compliance and retention validation;
4. hosting, backup/recovery, performance, and other technical architecture decisions;
5. catastrophic Owner-account recovery implementation if password/authenticator/recovery codes are all unavailable.

Duplicate merging, offline mode, medicine returns, non-prescription pharmacy retail, and payment processing remain future/out of V1 unless introduced through formal change control.
