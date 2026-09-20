# Hospital CRM — Business Requirements Document

## Document Control

| Field | Value |
| --- | --- |
| Document | Business Requirements Document |
| Product | Hospital CRM for clinic operations |
| Version | 0.1 |
| Status | Baseline with open decisions |
| Date | 2026-09-20 |
| Scope stage | Initial clinic MVP |
| Requirement confidence | Confirmed requirements plus explicitly identified TBDs |

---

# 1. Purpose

This BRD defines the confirmed business requirements for a clinic-focused Hospital CRM covering the operational journey from patient identification/registration through reception payment, consultation queue, doctor consultation and clinical notes, prescription creation, pharmacy dispensing, and pharmacy payment.

The document is intended to provide a common scope baseline for product, engineering, QA, clinic operations, and future solution-design work.

The journey currently begins when a patient arrives at reception and ends when the applicable clinic/pharmacy activities for that visit have been completed.

---

# 2. Business Objectives

The following objectives are confirmed from the agreed workflow:

**BO-01 — Maintain a reusable patient identity.**  
Create a stable patient record with a permanent Patient ID so returning patients can be found and their visit history can remain associated with the same patient.

**BO-02 — Digitize the clinic visit journey.**  
Support the operational flow from reception through doctor consultation and pharmacy completion without relying solely on the physical patient file.

**BO-03 — Provide an ordered consultation queue.**  
Allow reception and doctors to coordinate patient consultation order and current visit status.

**BO-04 — Maintain visit-level clinical records.**  
Allow the doctor to record consultation information, diagnosis, notes, and prescriptions against the correct patient and visit.

**BO-05 — Connect prescribing with clinic pharmacy availability.**  
Allow the doctor to see whether a medicine is available in the clinic pharmacy while preparing a prescription.

**BO-06 — Support pharmacy fulfilment and billing.**  
Allow pharmacy staff to retrieve the visit prescription, identify available and unavailable items, dispense available medicines, bill the patient, and record pharmacy payment.

**BO-07 — Preserve accountability of important records.**  
Maintain auditability for clinically and financially important records and avoid silent destructive changes.

---

# 3. Scope

## 3.1 In Scope

1. Patient registration.
2. Permanent unique Patient ID generation.
3. Patient search using Patient ID, phone number, and patient name.
4. Duplicate-patient prevention/detection support.
5. Separate Visit ID for each clinic visit.
6. Physical-file association with the Patient ID.
7. Reception consultation billing/payment recording.
8. Consultation queue creation and management.
9. Doctor queue view.
10. Doctor-to-reception patient call workflow.
11. Doctor access to the selected patient's relevant patient/visit history.
12. Doctor consultation notes.
13. Diagnosis recording.
14. Prescription creation.
15. Medicine search/selection while prescribing.
16. Visibility of clinic pharmacy availability during prescribing.
17. Explicit distinction between medicines that are in stock, out of stock, or not stocked by the clinic pharmacy.
18. Printed prescription generation.
19. Visible marking on the printed prescription for medicines unavailable from the clinic pharmacy.
20. Pharmacy retrieval of the patient's current prescription.
21. Role-limited pharmacy access to information needed for dispensing and billing.
22. Pharmacy dispensing of available prescribed medicines.
23. Pharmacy inventory reduction when medicine is dispensed.
24. Pharmacy communication of medicines that must be obtained outside.
25. Pharmacy billing.
26. Pharmacy payment recording.
27. Receipt/transaction records for consultation and pharmacy payments.
28. Role-based access for Administrator, Receptionist, Doctor, and Pharmacist.
29. Audit history for important clinical and financial changes.
30. Cancellation/void behavior that preserves history rather than silently deleting important records.
31. Prescription reprint support.
32. Patient-detail correction while preserving appropriate history.
33. Handling of clinic workflow edge cases through explicit business rules as they are confirmed.

## 3.2 Out of Scope for Initial MVP

The following were explicitly accepted as items not to include in the initial MVP:

1. Laboratory management.
2. Inpatient/bed management.
3. Insurance processing.
4. Ambulance management.
5. HR/payroll.
6. Full purchasing/supplier management for pharmacy stock.

## 3.3 Future / Later-Phase Candidates

The following are accepted as later-phase capabilities rather than launch requirements:

1. QR code or barcode on the physical patient file for faster patient retrieval.
2. Pharmacy purchasing/supplier management.
3. Additional inventory automation such as low-stock warning behavior beyond the core dispensing deduction.
4. Broader CRM capabilities such as reminders, follow-up engagement, or relationship-management features beyond the confirmed clinic workflow.

---

# 4. Core Business and Technology Assumptions

Only confirmed assumptions are listed here.

## 4.1 Patient identity

1. Every registered patient receives one permanent unique Patient ID.
2. A Patient ID remains associated with the same patient across multiple visits.
3. Patient name and phone number are patient-search and matching attributes.
4. Patient name and phone number do not replace the Patient ID as the unique identifier.
5. Multiple patients may share the same phone number.
6. Every clinic attendance creates a separate Visit ID associated with the permanent Patient ID.

## 4.2 Visit model

1. Clinical notes, diagnosis, prescription, queue state, and visit-level financial events are associated with a specific Visit ID.
2. A returning patient reuses the Patient ID but receives a new Visit ID.

## 4.3 Record-history model

1. Important clinical and financial records must not be silently overwritten without trace.
2. Changes to important records require attributable audit history.
3. Cancellation or correction must preserve historical evidence of the prior record where applicable.

## 4.4 Existing systems

No existing clinic software platform, external EMR/EHR, billing platform, pharmacy platform, payment gateway, identity provider, or other system has yet been confirmed.

The source-of-truth boundaries inside the final technical architecture therefore remain an open solution-design item. This BRD specifies required business behavior, not an unconfirmed technical architecture.

---

# 5. System and Third-Party Integration Landscape

No third-party integration has been confirmed.

| Platform / Partner | Role | Scope / Status |
| --- | --- | --- |
| Hospital CRM application | Primary business application being defined | Confirmed product scope; exact architecture is TBD |
| External payment provider | None confirmed | Open Decision |
| External pharmacy/catalogue provider | None confirmed | Open Decision |
| External messaging/notification provider | None confirmed | Open Decision |
| External identity/authentication provider | None confirmed | Open Decision |

---

# 6. Functional Requirements

Priority values remain **TBD** because no priority model or requirement-by-requirement priority has yet been confirmed.

Primary KPI values remain **TBD** because analytics/KPI requirements have not yet been confirmed.

## 6.1 Patient Registration and Retrieval

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-001 | Receptionist shall be able to search for an existing patient using Patient ID, phone number, or patient name before creating a new patient record. | TBD | Hospital CRM | TBD |
| FR-002 | The system shall generate a permanent unique Patient ID when a new patient is registered. | TBD | Hospital CRM | TBD |
| FR-003 | The Patient ID shall remain stable across all future visits for that patient. | TBD | Hospital CRM | TBD |
| FR-004 | When the system finds similar existing patient records during registration/search, reception shall review the candidate details with the patient and seek confirmation before creating or selecting a patient profile. Prior visit history and remembered visit purpose may be used to help the patient confirm the correct record. The fallback when no candidate can be confidently confirmed remains TBD. | TBD | Hospital CRM | TBD |
| FR-005 | The receptionist shall be able to associate the generated Patient ID with the patient's physical clinic file. | TBD | Hospital CRM + physical file | TBD |
| FR-006 | The system shall allow correction of patient details without replacing the patient's Patient ID. The exact correction/audit rules for demographic fields are TBD. | TBD | Hospital CRM | TBD |
| FR-007 | The system shall support returning patients who arrive without the physical file by locating the existing digital patient record using the supported search attributes. | TBD | Hospital CRM | TBD |
| FR-075 | Patient registration shall capture full name, phone number, date of birth, age, gender, address, and email as required registration information. How age is entered or derived relative to date of birth remains TBD. | TBD | Hospital CRM | TBD |
| FR-076 | Emergency contact, blood group, known allergies, and guardian/parent details shall be optional patient-registration information. | TBD | Hospital CRM | TBD |
| FR-077 | Government ID shall not be required for patient registration. Whether the system permits optional Government ID capture remains TBD. | TBD | Hospital CRM | TBD |
| FR-078 | The system shall allow multiple different patient records to share the same phone number; phone number shall not be enforced as a globally unique patient identifier. | TBD | Hospital CRM | TBD |

## 6.2 Visit Creation

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-008 | Each clinic attendance shall create a unique Visit ID linked to the patient's permanent Patient ID. | TBD | Hospital CRM | TBD |
| FR-009 | Visit-level queue state, consultation record, prescription, and visit financial events shall be associated with the correct Visit ID. | TBD | Hospital CRM | TBD |
| FR-010 | A returning patient's new visit shall not create a new Patient ID. | TBD | Hospital CRM | TBD |

## 6.3 Reception and Consultation Payment

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-011 | Receptionist shall be able to create a consultation charge/payment record for the visit. | TBD | Hospital CRM | TBD |
| FR-012 | Consultation payment shall be represented as a transaction/record rather than only as a boolean checkbox. | TBD | Hospital CRM | TBD |
| FR-013 | Consultation payment records shall represent at minimum Paid, Unpaid, Refunded, and Cancelled states. Partial consultation payment is not supported. | TBD | Hospital CRM | TBD |
| FR-014 | The system shall retain a receipt/reference record for a consultation payment. Receipt numbering/format is TBD. | TBD | Hospital CRM | TBD |
| FR-015 | A consultation-fee waiver may be initiated either by reception or directly by the doctor. A reception-initiated waiver request shall be presented to the doctor for approval, and reception cannot grant the waiver independently. A doctor-initiated waiver shall be treated as approved immediately and shall update the visit/payment status accordingly without a separate approval step. | TBD | Hospital CRM | TBD |
| FR-016 | A Paid visit is eligible to enter the doctor queue. An Unpaid visit shall not enter the doctor queue unless its consultation-fee waiver request has been approved by the doctor. A pending or unapproved waiver request does not make the visit queue-eligible. | TBD | Hospital CRM | TBD |

## 6.4 Consultation Queue

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-017 | Eligible visits shall be added to a consultation queue. | TBD | Hospital CRM | TBD |
| FR-018 | The queue shall preserve an explicit consultation order visible to the doctor. | TBD | Hospital CRM | TBD |
| FR-019 | Queue entries shall support the states Waiting, Called, With Doctor, Consultation Completed, Sent to Pharmacy, Completed, and Cancelled. | TBD | Hospital CRM | TBD |
| FR-020 | The doctor shall be able to see waiting patients and identify the next patient according to the queue order. | TBD | Hospital CRM | TBD |
| FR-021 | The doctor shall be able to initiate a "call patient" action for a queued patient. | TBD | Hospital CRM | TBD |
| FR-022 | A doctor-initiated call shall be visible to reception so the receptionist can physically call/direct the patient. | TBD | Hospital CRM | TBD |
| FR-023 | The system shall show the current queue state of each visit to authorized roles. Exact role-specific visibility beyond doctor/reception is TBD. | TBD | Hospital CRM | TBD |
| FR-024 | Rules for urgent/out-of-order queue handling are not yet confirmed and shall remain an Open Decision. | TBD | Hospital CRM | TBD |
| FR-025 | Rules for patients who leave, skip, or fail to respond when called are not yet confirmed and shall remain an Open Decision. | TBD | Hospital CRM | TBD |

## 6.5 Doctor Consultation and Clinical Record

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-026 | The doctor shall be able to open the patient record for the selected queued visit. | TBD | Hospital CRM | TBD |
| FR-027 | The doctor shall be able to review previous patient visits relevant to clinical care. | TBD | Hospital CRM | TBD |
| FR-028 | The doctor shall be able to record chief complaint/symptoms/history information for the current visit. Exact field structure remains TBD. | TBD | Hospital CRM | TBD |
| FR-029 | The doctor shall be able to record diagnosis information for the current visit. | TBD | Hospital CRM | TBD |
| FR-030 | The doctor shall be able to record clinical notes for the current visit. | TBD | Hospital CRM | TBD |
| FR-031 | Clinical information shall remain linked to the correct Patient ID and Visit ID. | TBD | Hospital CRM | TBD |
| FR-032 | Material edits to clinical notes, diagnosis, or prescription shall retain auditable information identifying the change, actor, and time. Exact revision/versioning UX remains TBD. | TBD | Hospital CRM | TBD |

## 6.6 Prescription Authoring and Medicine Availability

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-033 | The doctor shall be able to add medicines to a prescription by searching, selecting, and/or typing medicines. The exact controlled-vocabulary behavior is TBD. | TBD | Hospital CRM | TBD |
| FR-034 | While prescribing, the doctor shall be shown the clinic pharmacy availability status for medicines known to the clinic pharmacy inventory. | TBD | Hospital CRM | TBD |
| FR-035 | Pharmacy availability shall distinguish at minimum: In Stock, Out of Stock, and Not Stocked. | TBD | Hospital CRM | TBD |
| FR-036 | A medicine's clinic-pharmacy unavailability shall not automatically prevent the doctor from prescribing it. | TBD | Hospital CRM | TBD |
| FR-037 | The doctor shall be able to specify prescription instructions such as dose/frequency/duration as applicable. Exact mandatory fields and formats are TBD. | TBD | Hospital CRM | TBD |
| FR-038 | The prescription shall support a required quantity when it can be derived or entered from the prescribed regimen. Exact derivation rules are TBD. | TBD | Hospital CRM | TBD |
| FR-039 | The system shall allow the doctor to finalize a prescription for the visit. | TBD | Hospital CRM | TBD |
| FR-040 | A finalized prescription shall remain associated with its Patient ID and Visit ID and be retrievable by authorized pharmacy staff. | TBD | Hospital CRM | TBD |

## 6.7 Printed Prescription

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-041 | The doctor shall be able to print the finalized prescription. | TBD | Hospital CRM + printer | TBD |
| FR-042 | Medicines unavailable from the clinic pharmacy shall be visibly marked on the printed prescription. The exact symbol/visual treatment remains TBD. | TBD | Hospital CRM + printer | TBD |
| FR-043 | The printed prescription shall include an explanation indicating that marked medicines are not currently available from the clinic pharmacy and must be obtained externally. Exact wording is TBD. | TBD | Hospital CRM + printer | TBD |
| FR-044 | The system shall support reprinting a prescription without silently creating a new prescription version. | TBD | Hospital CRM + printer | TBD |

## 6.8 Pharmacy Prescription Retrieval and Access

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-045 | Pharmacy staff shall be able to locate the relevant patient/visit using the Patient ID and retrieve the applicable prescription. Additional lookup methods are TBD. | TBD | Hospital CRM | TBD |
| FR-046 | Pharmacy staff shall see the information required for dispensing, medicine instructions, availability, quantity, price/bill preparation, and pharmacy payment. | TBD | Hospital CRM | TBD |
| FR-047 | Pharmacy staff shall not receive unrestricted access to the doctor's complete clinical notes solely because they are dispensing medicines. | TBD | Hospital CRM | TBD |
| FR-048 | The exact minimum clinical context visible to pharmacy staff beyond the prescription itself remains TBD. | TBD | Hospital CRM | TBD |

## 6.9 Pharmacy Dispensing and Inventory

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-049 | Pharmacy staff shall be able to see which prescribed medicines are available, out of stock, or not stocked. | TBD | Hospital CRM | TBD |
| FR-050 | Pharmacy staff shall be able to record the quantity of each available medicine actually dispensed. | TBD | Hospital CRM | TBD |
| FR-051 | When medicine is dispensed, clinic pharmacy inventory shall be reduced by the dispensed quantity. | TBD | Hospital CRM | TBD |
| FR-052 | The system shall support a case where only part of the prescribed quantity is available; the exact business and billing behavior for partial fulfilment remains an Open Decision. | TBD | Hospital CRM | TBD |
| FR-053 | Pharmacy staff shall be able to identify prescribed medicines that the patient must obtain outside the clinic pharmacy. | TBD | Hospital CRM | TBD |
| FR-054 | The pharmacist shall be able to explain to the patient which prescribed medicines were not supplied by the clinic pharmacy. | TBD | Hospital CRM | TBD |
| FR-055 | Rules for medicine substitution or alternate brands are not yet confirmed and shall remain an Open Decision. | TBD | Hospital CRM | TBD |
| FR-056 | Rules for medicine returns and inventory reversal are not yet confirmed and shall remain an Open Decision. | TBD | Hospital CRM | TBD |

## 6.10 Pharmacy Billing and Payment

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-057 | Pharmacy staff shall be able to prepare a bill for medicines being supplied by the clinic pharmacy. | TBD | Hospital CRM | TBD |
| FR-058 | The pharmacy bill shall identify what the patient is being charged for. | TBD | Hospital CRM | TBD |
| FR-059 | Medicines not supplied by the clinic pharmacy shall not be included as dispensed bill items. | TBD | Hospital CRM | TBD |
| FR-060 | Pharmacy payment shall be stored as a payment transaction/record rather than only as a boolean checkbox. | TBD | Hospital CRM | TBD |
| FR-061 | Pharmacy payment records shall support Paid, Unpaid, Partial, Refunded, and Cancelled states. Exact rules remain TBD. | TBD | Hospital CRM | TBD |
| FR-062 | The system shall retain a receipt/reference record for pharmacy payment. Receipt numbering/format is TBD. | TBD | Hospital CRM | TBD |
| FR-063 | Pharmacy staff shall update the pharmacy payment state after payment activity is completed. | TBD | Hospital CRM | TBD |

## 6.11 Roles and Access

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-064 | The initial product shall support the roles Administrator, Receptionist, Doctor, and Pharmacist. | TBD | Hospital CRM | TBD |
| FR-065 | Receptionist access shall include patient registration/retrieval, visit/reception workflow, consultation payment, and queue operations required for reception duties. | TBD | Hospital CRM | TBD |
| FR-066 | Doctor access shall include patient clinical history needed for care, current consultation record, diagnosis, clinical notes, prescribing, and queue actions needed for doctor duties. | TBD | Hospital CRM | TBD |
| FR-067 | Pharmacist access shall include prescription retrieval, relevant medication information, inventory/dispensing, billing, and pharmacy payment needed for pharmacy duties. | TBD | Hospital CRM | TBD |
| FR-068 | Administrator shall be able to manage system-level operational configuration and staff access. Exact admin permissions are TBD. | TBD | Hospital CRM | TBD |
| FR-069 | Users shall not automatically receive access to information outside the needs of their role. Detailed permission matrices remain TBD. | TBD | Hospital CRM | TBD |

## 6.12 Audit, Correction, Cancellation, and Reprint

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-070 | The system shall retain audit information for material changes to patient, clinical, prescription, payment, and dispensing records. | TBD | Hospital CRM | TBD |
| FR-071 | Audit information shall identify at minimum the actor and time of the material change; exact before/after retention requirements remain TBD. | TBD | Hospital CRM | TBD |
| FR-072 | Important clinical and financial records shall not be permanently removed from normal workflow through an unaudited hard-delete action. | TBD | Hospital CRM | TBD |
| FR-073 | Where a prescription, payment, or other important record is cancelled/voided, the record shall preserve the fact of cancellation. | TBD | Hospital CRM | TBD |
| FR-074 | Cancellation reasons shall be captured where required by the finalized business rules. The exact list of actions requiring a reason is TBD. | TBD | Hospital CRM | TBD |

---

# 7. Business Rules and Journey Controls

## 7.1 Patient Identity Rules

**BR-001** — Patient ID is the permanent unique patient identifier.

**BR-002** — Phone number and patient name are search/matching attributes and are not substitutes for the unique Patient ID.

**BR-003** — One patient may have multiple Visit IDs over time.

**BR-004** — A new clinic visit for an existing patient must link to the existing Patient ID rather than generating a new patient identity.

**BR-005** — When similar existing patient records are surfaced, reception must review the candidate details with the patient and seek confirmation before deciding that an existing profile is the same person. Prior visit history and remembered visit purpose may be used to support confirmation. The fallback when no candidate can be confidently confirmed remains TBD.

**BR-025** — Phone number is not a unique patient identifier. Multiple patients, including family members, may share the same phone number.

## 7.2 Visit and Queue Rules

**BR-006** — Each clinic attendance has its own Visit ID.

**BR-007** — The consultation queue is visit-based, not patient-master-based.

**BR-008** — Queue state must be explicit and persisted.

**BR-009** — The doctor selects/calls patients through the queue, with reception informed of the call.

**BR-010** — A visit with Unpaid consultation status must not enter the doctor queue unless a consultation-fee waiver has been approved by the doctor. A Paid consultation is eligible for queue entry. Partial consultation payment is not supported.

## 7.3 Clinical Record Rules

**BR-011** — Clinical notes, diagnosis, and prescription are recorded against the current Visit ID and linked Patient ID.

**BR-012** — Material clinical changes must remain auditable.

**BR-013** — Pharmacy access does not automatically grant access to the doctor's full clinical notes.

## 7.4 Prescription and Pharmacy Rules

**BR-014** — Pharmacy availability is informative to the doctor and does not by itself prohibit prescribing an unavailable medicine.

**BR-015** — Availability states must distinguish In Stock, Out of Stock, and Not Stocked.

**BR-016** — Unavailable medicines are marked on the printed prescription so the patient can identify medicines to obtain outside.

**BR-017** — Dispensed quantity reduces clinic pharmacy inventory.

**BR-018** — Medicines not dispensed by the clinic pharmacy must not be billed as dispensed items.

## 7.5 Payment Rules

**BR-019** — Consultation and pharmacy payment are separate financial events.

**BR-020** — Financial events must be stored as transactions/records with explicit status.

**BR-021** — Consultation payment must support Paid, Unpaid, Refunded, and Cancelled states. Partial consultation payment is not supported.

**BR-022** — A consultation-fee waiver is an exception controlled by the doctor. Reception may request the waiver but may not approve it. The doctor may either approve a reception-initiated request or initiate the waiver directly; a doctor-initiated waiver is immediately treated as approved and updates the visit/payment status without a second approval step. Exact payment methods, refund rules, reconciliation rules, and any required waiver-reason format remain unconfirmed.

## 7.6 Audit Rules

**BR-023** — Important clinical and financial records must not disappear through silent overwrite or unaudited hard deletion.

**BR-024** — Corrections/cancellations must preserve appropriate history.

---

# 8. Analytics and Event Tracking Requirements

Analytics scope has not yet been confirmed.

No KPI, event taxonomy, analytics platform, attribution model, or event-retention rule is treated as a requirement in this version.

Open items:

1. Business KPIs to measure.
2. Operational queue metrics to measure.
3. Pharmacy/inventory metrics to measure.
4. Financial reporting requirements.
5. Audit/reporting requirements.
6. Analytics platform, if any.
7. Event-tracking requirements, if any.

---

# 9. Dependencies and Open Delivery Items

The following items require explicit decisions before the BRD can be considered complete.

## 9.1 Patient Registration

1. How age is handled relative to date of birth (entered independently, derived, or another confirmed rule).
2. Whether Government ID may be captured optionally; it is confirmed as not required.
3. Exact algorithm/criteria used by the system to decide that records are "similar" enough to surface as possible matches.
4. Fallback behavior when the patient cannot confidently confirm any surfaced existing record.
5. Whether duplicate-record merge is required and who may perform it.
6. Patient-ID format and whether it has semantic meaning.
7. Whether QR/barcode is included at launch or later only.

## 9.2 Visit and Queue

7. How an approved waiver is represented in the financial record (payment state, exemption/authorization record, or another confirmed model).
8. Whether a reason is mandatory when reception submits a waiver request.
9. Whether the doctor must provide a reason when rejecting or declining a waiver request.
9. Rules for urgent patients and out-of-order queue handling.
10. Behavior when a patient leaves before consultation.
11. Behavior when a patient does not respond when called.
12. Whether multiple doctors/queues exist and how patients are assigned between them.

## 9.3 Clinical Documentation

13. Exact structure of clinical history/symptom fields.
14. Whether diagnosis is free text, controlled selection, or both.
15. Whether clinical-note templates are required.
16. Exact edit/version rules after consultation is completed.
17. Whether a completed visit may be reopened, and by whom.

## 9.4 Prescription

18. Mandatory prescription fields.
19. Whether prescriptions become immutable/final after printing/dispensing.
20. Change workflow when a doctor changes a prescription after the patient has already reached pharmacy.
21. Whether medicine selection is brand-based, generic/molecule-based, or both.
22. Rules for medicine substitution.
23. Exact visual marker and text for clinic-unavailable medicines.
24. Exact quantity derivation rules.

## 9.5 Pharmacy and Inventory

25. Inventory unit model, such as strip/tablet/bottle/pack.
26. Batch/lot/expiry tracking requirement.
27. Partial-dispensing behavior.
28. Medicine return/refund behavior.
29. Inventory reversal rules after cancelled dispensing.
30. Low-stock threshold and alert behavior.
31. Price ownership and medicine price-change behavior.
32. Whether patients may visit the pharmacy without an associated doctor visit/prescription.

## 9.6 Payments

33. Accepted consultation payment methods.
34. Accepted pharmacy payment methods.
35. Cash handling/reconciliation requirements.
36. UPI/card/payment-gateway integration, if any.
37. Consultation fee determination.
38. Pharmacy pricing/tax requirements.
39. Refund authorization rules.
40. Receipt numbering and printing requirements.
41. Treatment of partial payment for queue eligibility/dispensing.

## 9.7 Roles, Security, and Operations

42. Authentication method.
43. Detailed role-permission matrix.
44. Administrator capabilities.
45. Staff onboarding/offboarding.
46. Session/security requirements.
47. Data backup/recovery requirements.
48. Legal/privacy/compliance requirements applicable to the clinic.
49. Data retention requirements.
50. Audit-log retention and access.

## 9.8 Deployment and Architecture

51. Hosting/deployment model.
52. Device/browser requirements.
53. Printer requirements.
54. Offline behavior, if required.
55. Number of clinic locations.
56. Number of doctors/reception/pharmacy stations.
57. Expected concurrent users and performance expectations.
58. Existing data migration, if any.
59. External integrations, if any.

## 9.9 Analytics and Reporting

60. Required operational reports.
61. Required financial reports.
62. Required clinical reports.
63. Required inventory reports.
64. KPIs.
65. Analytics platform/event model.

---

# 10. Confirmed Edge Scenarios Requiring Defined Behavior

The following scenarios have been accepted as cases the design must address, but their exact behavior is not yet fully defined:

1. Returning patient arrives without the physical patient file.
2. Reception accidentally attempts to create a duplicate patient.
3. Multiple doctors operate simultaneously.
4. A patient needs to be seen outside normal queue order.
5. A patient pays but leaves before consultation.
6. The doctor changes a prescription after the patient reaches pharmacy.
7. Only part of the prescribed quantity is available.
8. Pharmacy carries an alternate brand/molecule presentation.
9. Medicine is returned/refunded.
10. Consultation payment is waived.
11. Patient comes only to pharmacy.
12. Prescription requires reprint.
13. Patient name/phone/other details are corrected later.
14. Multiple family members share one phone number.

These are not silently resolved in this BRD. Each scenario must be closed through an explicit business rule or confirmed workflow decision.

---

# 11. MVP Module Baseline

The accepted initial module set is:

1. Patient Registry.
2. Reception and Visit Creation.
3. Consultation Payment.
4. Doctor Queue.
5. Clinical Notes.
6. Prescription.
7. Pharmacy Inventory.
8. Pharmacy Billing.
9. Printed Prescription and Receipts.
10. Users, Roles, and Audit Logs.

---

# 12. Requirements Status Summary

| Category | Status |
| --- | --- |
| Patient identity model | Confirmed at high level |
| Visit identity model | Confirmed |
| Reception workflow | Confirmed at high level; waiver-record representation remains open |
| Doctor queue | Confirmed at high level; exception rules open |
| Clinical notes | Confirmed at high level; data structure open |
| Prescription | Confirmed at high level; medication model and edit rules open |
| Pharmacy availability | Confirmed |
| Pharmacy dispensing | Confirmed at high level; partial/return/substitution rules open |
| Consultation billing/payment | Confirmed at high level; methods and authorization rules open |
| Pharmacy billing/payment | Confirmed at high level; methods and refund rules open |
| Roles | Confirmed at high level; detailed permissions open |
| Audit history | Confirmed at high level |
| Analytics/KPIs | Not yet confirmed |
| Third-party integrations | None confirmed |
| Technical architecture | Not yet confirmed |
| Legal/privacy/compliance details | Not yet confirmed |

---

# 13. Finalization Status

This document is a structured requirements baseline, not a claim that discovery is complete.

It is suitable for continued product discovery and early solution planning, but implementation-significant open decisions in Section 9 must be resolved progressively before the BRD can be treated as final implementation scope.

No unconfirmed third-party vendor, payment method, authentication method, KPI, compliance regime, hosting architecture, or low-level technical design is asserted as fact in this version.
