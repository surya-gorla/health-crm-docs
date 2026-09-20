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
- age;
- gender;
- address;
- email.

Optional registration information:

- emergency contact;
- blood group;
- known allergies;
- guardian/parent details.

Government ID is not required. Whether it may be captured optionally remains TBD.

How age is entered or derived relative to date of birth remains TBD.

## 3.4 Duplicate Handling

The system must help prevent or detect accidental duplicate registration.

When similar existing records are found:

1. reception reviews the candidate details with the patient;
2. reception asks the patient to confirm whether a candidate is their existing profile;
3. prior visit history and the remembered purpose of a previous visit may be used to help confirmation;
4. if the patient confirms the candidate, reception uses the existing patient profile.

Still TBD:

- the system matching criteria used to surface similar records;
- what happens when the patient cannot confidently confirm any candidate;
- whether duplicate records can later be merged and by whom.

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

Supported consultation payment states:

- Paid
- Unpaid
- Refunded
- Cancelled

Partial consultation payment is not supported.

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

Still unresolved:

- whether reception may reassign an already queued visit from one doctor to another;
- skipped patient behavior;
- patient-does-not-respond behavior;
- patient-left-after-payment behavior.

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

The doctor can record:

- chief complaint/symptoms/history;
- diagnosis;
- clinical notes.

Exact field models remain TBD.

## 8.3 Auditability

Material changes to clinical content must remain attributable.

At minimum, audit behavior must preserve:

- actor;
- time;
- the fact that a material change occurred.

Exact versioning/display rules remain TBD.

---

# 9. Prescription Workflow

## 9.1 Medicine Entry

Doctor can:

- search medicines;
- select medicines;
- type medicine information where allowed.

The exact relationship between free-text and controlled medicine catalogue remains TBD.

## 9.2 Availability Status

Medicine availability must support:

- **In Stock**
- **Out of Stock**
- **Not Stocked**

## 9.3 Availability Rule

Clinic-pharmacy availability informs the doctor but does not automatically prohibit prescribing an unavailable medicine.

## 9.4 Prescription Finalization

The doctor can finalize a prescription with medication directions such as dose/frequency/duration where applicable.

Exact mandatory fields and quantity-derivation rules remain TBD.

---

# 10. Printed Prescription Workflow

The printed prescription must:

- contain the prescribed medicines;
- visibly identify medicines unavailable from the clinic pharmacy;
- explain that marked medicines must be obtained externally.

The exact visual marker, legend, typography, and layout remain TBD.

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

If prescribed quantity exceeds available stock, the system must support the scenario.

Exact behavior remains TBD, including:

- whether partial quantity can be dispensed;
- how remaining quantity is represented;
- how billing is handled;
- how the printed/digital prescription reflects the partial fulfilment.

## 13.2 Inventory Deduction

Inventory decreases by the quantity actually dispensed, not merely by the quantity prescribed.

## 13.3 Open Inventory Rules

Still unresolved:

- inventory unit model;
- batch tracking;
- expiry tracking;
- return/reversal rules;
- low-stock alerts;
- substitute-brand rules;
- price ownership.

---

# 14. Pharmacy Billing and Payment Workflow

1. Pharmacy identifies medicines actually supplied.
2. Bill contains the medicines being supplied by the clinic pharmacy.
3. Medicines not supplied by the clinic pharmacy are not billed as dispensed items.
4. Patient is informed which medicines must be obtained outside.
5. Pharmacy payment is recorded as a transaction.

Supported pharmacy payment states:

- Paid
- Unpaid
- Partial
- Refunded
- Cancelled

Payment methods and refund authorization rules remain TBD.

---

# 15. Role-Oriented Workflow Summary

## 15.1 Receptionist

Primary workflow responsibility:

- patient search;
- patient registration;
- Visit ID creation;
- consultation payment record;
- queue coordination;
- doctor-call coordination.

## 15.2 Doctor

Primary workflow responsibility:

- review queue;
- call patient;
- consultation;
- clinical documentation;
- diagnosis;
- prescription;
- prescription finalization/printing.

## 15.3 Pharmacist

Primary workflow responsibility:

- prescription retrieval;
- medicine availability review;
- dispensing;
- inventory deduction;
- pharmacy billing;
- pharmacy payment.

## 15.4 Administrator

Primary responsibility:

- staff/access management;
- system-level operational configuration.

Exact administrator permission scope remains TBD.

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

# 17. Open Workflow Scenarios

The following require explicit future workflow decisions:

1. Duplicate patient detected after multiple visits already exist.
2. Reassignment of a patient from one doctor's queue to another after initial assignment.
3. Patient does not respond when called.
5. Patient leaves after paying but before consultation.
6. Doctor edits prescription after pharmacy retrieval.
7. Doctor edits prescription after partial or complete dispensing.
8. Partial medicine fulfilment.
9. Medicine substitution.
10. Medicine return/refund.
11. Consultation payment waiver.
12. Pharmacy-only visit.
13. Prescription reprint.
14. Patient demographic correction.
15. Duplicate-profile fallback when the patient cannot confidently confirm an existing candidate.
