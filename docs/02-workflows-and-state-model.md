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
Queue eligibility check (exact rule TBD)
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

Still TBD:

- the system matching criteria used to surface similar records;
- whether duplicate records can be merged in a later update and by whom.

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

The currently accepted operational queue/visit states are:

1. **Waiting**
2. **Called**
3. **With Doctor**
4. **Consultation Completed**
5. **Sent to Pharmacy**
6. **Completed**
7. **Cancelled**

The exact triggers for all transitions remain subject to business-rule confirmation.

## 4.2 Baseline State Flow

```text
Waiting
   |
   v
Called
   |
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

Possible alternative:

```text
Any eligible operational state
   |
   v
Cancelled
```

The exact states from which cancellation is permitted, and who can perform it, remain TBD.

---

# 5. Reception Workflow

## 5.1 Existing Patient

1. Reception searches by Patient ID, phone number, or patient name.
2. Reception selects the correct patient.
3. Reception creates a new Visit ID.
4. Reception handles the consultation payment/charge record.
5. The visit becomes eligible for queue entry according to the final payment/queue business rule.
6. Reception adds or confirms the visit in the consultation queue.

## 5.2 New Patient

1. Reception captures required patient information.
2. System generates a permanent Patient ID.
3. Patient ID is associated with the physical clinic file.
4. Reception creates the patient's first Visit ID.
5. Reception handles consultation payment/charge.
6. Queue eligibility is evaluated.
7. Visit enters the consultation queue.

## 5.3 Returning Patient Without Physical File

Confirmed intent:

- the existing digital patient record must still be retrievable;
- the patient must not receive a new Patient ID merely because the physical file is unavailable.

The operational process for locating/replacing the physical file remains TBD.

---

# 6. Consultation Payment State Model

Consultation payment must be recorded as a transaction, not only as a checkbox.

V1 does not process consultation payments. Reception records payment status/information after payment occurs outside the CRM.

Confirmed consultation rules:

- Paid -> eligible to enter the doctor queue.
- Unpaid -> must not enter the doctor queue unless an approved waiver applies.
- Partial consultation payment is not supported.
- Consultation payment is non-refundable in V1.
- If a paid consultation is cancelled, only the doctor may cancel it and a reason is required; the payment is not refunded.

Confirmed queue rule:

- **Paid** -> eligible to enter the doctor queue.
- **Unpaid** -> must not enter the doctor queue.

Waived consultation flow is confirmed at a high level:

### Reception-initiated waiver

1. reception raises a waiver request;
2. the request is shown to the doctor;
3. reception cannot approve the waiver;
4. if the doctor approves, the visit becomes eligible for the doctor queue despite no consultation payment;
5. while the waiver is pending or not approved, the visit remains ineligible for the queue.

### Doctor-initiated waiver

1. the doctor may initiate the consultation-fee waiver directly;
2. because the doctor is the approving authority, no second approval step is required;
3. the waiver is treated as approved immediately;
4. the visit/payment status is updated automatically;
5. the visit becomes eligible for the doctor queue.

The exact financial-record representation and reason requirements for the waiver remain TBD.

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
- if a paid patient leaves before consultation, reception moves the visit toward the end of that doctor's queue;
- reception cannot cancel a visit;
- the doctor may cancel a visit with a reason;
- cancelled visits remain visible in history.

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

Clinical decision functions remain within the Doctor role even though the interface itself is designed to be simple.

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
- mark unavailable or unsupplied items/quantities with **;
- include a legend explaining that ** means the clinic pharmacy did not supply that medicine/quantity and the patient should obtain it outside.

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

Additional search methods for pharmacy remain TBD.

---

# 12. Pharmacy Access Boundary

Pharmacy staff may access:

- patient identification needed for fulfilment;
- current prescription;
- medicine instructions;
- quantity;
- availability;
- bill/price information;
- pharmacy payment information.

Pharmacy staff must not receive unrestricted access to full doctor clinical notes solely because they are dispensing medicines.

The exact minimum clinical context beyond the prescription remains TBD.

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

Normal prescription dispensing automatically deducts actual dispensed quantity.

For pharmacy inventory upkeep or non-dispensing changes (for example stock additions, damage, loss, or corrections):

1. pharmacist submits an inventory-change request;
2. the request includes the proposed change and reason;
3. doctor reviews it;
4. inventory changes only after doctor approval;
5. request, decision, actor, reason, and resulting change are logged.

Expired stock is blocked from dispensing. The doctor is notified, and doctor action/approval controls the inventory disposition/adjustment record.

---

# 14. Pharmacy Billing and Payment Workflow

1. Pharmacy identifies medicines actually supplied.
2. Bill contains the medicines being supplied by the clinic pharmacy.
3. Medicines not supplied by the clinic pharmacy are not billed as dispensed items.
4. Patient is informed which medicines must be obtained outside.
5. Pharmacy payment is recorded as a transaction.

V1 does not process pharmacy payments. Pharmacy staff record the payment status/information after payment occurs outside the CRM.

Pharmacy payment methods, partial-payment behavior, refund/cancellation behavior, and receipt requirements remain TBD pending clinic confirmation.

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
- patient clinical history needed for care;
- current consultation record;
- diagnosis and notes;
- prescribing;
- payment/queue status;
- direct demographic edits and approval of reception demographic-change requests;
- inventory-upkeep approval;
- substitution approval;
- consultation waiver approval/initiation;
- visit cancellation with reason.

## 15.3 Pharmacist

Pharmacist access includes:

- current prescription;
- previous prescriptions;
- known allergies;
- dispensing;
- pharmacy bill/payment-status functions;
- inventory-upkeep request entry.

Pharmacists cannot edit a doctor prescription directly.

Non-dispensing inventory changes submitted by pharmacy require doctor approval.

## 15.4 Administrator

Administrator responsibilities include:

- create/disable staff accounts;
- assign permission groups;
- manage non-clinical clinic configuration;
- manage medicine/inventory configuration where permitted;
- view operational/revenue/inventory reports;
- view audit logs.

Administrator permission by itself does not grant clinical-authoring authority. Clinical notes, diagnosis, prescriptions, amendments, waiver approvals, and similar clinical actions require Doctor-group permission.

Staff accounts are disabled rather than deleted when a staff member leaves, preserving historical audit references.

## 15.5 Group-Based Access

Every staff member has an individual account.

Users belong to privilege groups such as:

- Doctor;
- Reception;
- Pharmacist.

Group membership determines role privileges.

V1 uses individual username/login + password accounts within the fixed single-clinic context. Password reset is owner/admin-assisted. 2FA/MFA is not required in V1.

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

# 17. Pilot Operating Model

Confirmed for the current pilot:

- one clinic branch;
- clinic owner is also the doctor managing the clinic;
- separate pharmacy operation;
- web application;
- internet-dependent operation;
- no offline mode in V1;
- normal A4 printing;
- no thermal receipt printer requirement;
- hosting/deployment provider remains TBD.

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

# 19. Remaining Genuine Open Areas

The current workflow itself is substantially defined. Remaining items requiring clinic policy, external validation, or downstream technical design are:

1. whether any non-doctor/lower-knowledge staff may only assist with data entry or are expected to perform clinical decision/finalization actions;
2. clinic-defined consultation and pharmacy payment methods/status conventions;
3. consultation fee and pharmacy billing policy details;
4. pharmacy partial-payment/refund/cancellation policy;
5. legal/privacy/compliance and retention requirements;
6. hosting, backup/recovery, and other technical architecture decisions.

Duplicate merging, offline mode, medicine returns, non-prescription pharmacy retail, and payment processing remain future/out of V1 unless explicitly added later.
