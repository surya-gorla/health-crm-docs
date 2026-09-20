# Hospital CRM — Business Requirements Document

## Document Control

| Field | Value |
| --- | --- |
| Document | Business Requirements Document |
| Product | Hospital CRM for clinic operations |
| Version | 0.2 |
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
27. Consultation and pharmacy payment-status/information recording; payment processing itself is not part of V1.
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
7. In-CRM payment processing/payment-gateway integration for V1.
8. Medicine returns in V1.
9. Offline operation in V1.
10. Thermal receipt printing in V1.

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
| FR-004 | When the system finds similar existing patient records during registration/search, reception shall review the candidate details with the patient and seek confirmation before creating or selecting a patient profile. Prior visit history and remembered visit purpose may be used to help the patient confirm the correct record. If the patient cannot confidently identify an existing candidate, reception may create a new patient profile and the system shall mark it as **Possible Duplicate**. | TBD | Hospital CRM | TBD |
| FR-005 | The receptionist shall be able to associate the generated Patient ID with the patient's physical clinic file. | TBD | Hospital CRM + physical file | TBD |
| FR-006 | The system shall allow correction of patient details without replacing the patient's Patient ID. The exact correction/audit rules for demographic fields are TBD. | TBD | Hospital CRM | TBD |
| FR-007 | The system shall support returning patients who arrive without the physical file by locating the existing digital patient record using the supported search attributes. | TBD | Hospital CRM | TBD |
| FR-075 | Patient registration shall capture full name, phone number, date of birth, gender, address, and email as required registration information. Age shall be calculated from date of birth rather than maintained as an independent required input. | TBD | Hospital CRM | TBD |
| FR-076 | Emergency contact, blood group, known allergies, and guardian/parent details shall be optional patient-registration information. | TBD | Hospital CRM | TBD |
| FR-077 | Government ID shall be optional and shall not be required for patient registration. | TBD | Hospital CRM | TBD |
| FR-078 | The system shall allow multiple different patient records to share the same phone number; phone number shall not be enforced as a globally unique patient identifier. | TBD | Hospital CRM | TBD |
| FR-079 | A newly created record that could not be confidently distinguished from surfaced existing candidates shall remain usable as a normal patient profile while carrying a visible **Possible Duplicate** marker. Duplicate-profile merge is not part of the confirmed V1 workflow and remains for later evaluation. | TBD | Hospital CRM | TBD |

## 6.2 Visit Creation

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-008 | Each clinic attendance shall create a unique Visit ID linked to the patient's permanent Patient ID. | TBD | Hospital CRM | TBD |
| FR-009 | Visit-level queue state, consultation record, prescription, and visit financial events shall be associated with the correct Visit ID. | TBD | Hospital CRM | TBD |
| FR-010 | A returning patient's new visit shall not create a new Patient ID. | TBD | Hospital CRM | TBD |

## 6.3 Reception and Consultation Payment

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-011 | Receptionist shall be able to record the consultation fee/payment status information for the visit after payment activity occurs outside the CRM. | TBD | Hospital CRM | TBD |
| FR-012 | V1 shall not process consultation payments. It shall retain the consultation payment status/information required by the clinic for queue eligibility and reporting. | TBD | Hospital CRM | TBD |
| FR-013 | Consultation payment shall support Paid and Unpaid status for the normal flow. Partial consultation payment is not supported, and consultation payments are non-refundable in V1. Waiver is handled through the separate doctor-controlled waiver workflow. | TBD | Hospital CRM | TBD |
| FR-014 | Consultation receipt/reference behavior is TBD pending clinic confirmation; V1 does not require a payment-processing integration or thermal receipt workflow. | TBD | Hospital CRM | TBD |
| FR-015 | A consultation-fee waiver may be initiated either by reception or directly by the doctor. A reception-initiated waiver request shall be presented to the doctor for approval, and reception cannot grant the waiver independently. A doctor-initiated waiver shall be treated as approved immediately and shall update the visit/payment status accordingly without a separate approval step. | TBD | Hospital CRM | TBD |
| FR-016 | A Paid visit is eligible to enter the doctor queue. An Unpaid visit shall not enter the doctor queue unless its consultation-fee waiver request has been approved by the doctor. A pending or unapproved waiver request does not make the visit queue-eligible. | TBD | Hospital CRM | TBD |

## 6.4 Consultation Queue

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-017 | Eligible visits shall be added to a consultation queue. | TBD | Hospital CRM | TBD |
| FR-018 | The queue shall preserve an explicit consultation order within the queue assigned to the selected doctor. Each doctor shall have a doctor-specific queue. | TBD | Hospital CRM | TBD |
| FR-019 | Queue entries shall support the states Waiting, Called, With Doctor, Consultation Completed, Sent to Pharmacy, Completed, and Cancelled. | TBD | Hospital CRM | TBD |
| FR-020 | The doctor shall be able to see the waiting patients assigned to that doctor's queue and identify the next patient according to the queue order. | TBD | Hospital CRM | TBD |
| FR-021 | The doctor shall be able to initiate a "call patient" action for a queued patient. | TBD | Hospital CRM | TBD |
| FR-022 | A doctor-initiated call shall be visible to reception so the receptionist can physically call/direct the patient. | TBD | Hospital CRM | TBD |
| FR-023 | Reception shall assign the visit to a specific doctor before queue entry. The system shall show the current queue state of each visit to authorized roles. Exact role-specific visibility beyond doctor/reception is TBD. | TBD | Hospital CRM | TBD |
| FR-024 | The system shall not implement a priority flag, priority-request workflow, or automated priority reordering for urgent patients. If an urgent case arises, the receptionist shall communicate it directly to the doctor outside the CRM workflow. | TBD | Hospital CRM | TBD |
| FR-025 | When a called patient does not respond, reception shall mark the visit as Unresponded and the system shall move that visit five queue positions downward within the assigned doctor's queue. If a paid patient leaves before consultation, reception shall move the visit toward the end of the assigned doctor's queue. Exact boundary behavior remains TBD when fewer than five later slots remain or when "toward the end" needs a precise slot rule. | TBD | Hospital CRM | TBD |
| FR-080 | Reception shall be able to reassign a queued visit from one doctor-specific queue to another doctor-specific queue. Every reassignment shall be logged. | TBD | Hospital CRM | TBD |
| FR-081 | Reception shall not be permitted to cancel a visit. A doctor may cancel a visit, a cancellation reason shall be recorded, and the cancelled visit shall remain visible in history. | TBD | Hospital CRM | TBD |

## 6.5 Doctor Consultation and Clinical Record

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-026 | The doctor shall be able to open the patient record for the selected queued visit. | TBD | Hospital CRM | TBD |
| FR-027 | The doctor shall be able to review previous patient visits relevant to clinical care. | TBD | Hospital CRM | TBD |
| FR-028 | The doctor consultation interface shall use a low-complexity guided structure with plain labels and limited mandatory entry. It shall capture a chief complaint/patient problem and allow optional symptoms/history details. | TBD | Hospital CRM | TBD |
| FR-029 | The doctor shall be able to record a clinical assessment/diagnosis for the current visit using a simple doctor-facing entry field. | TBD | Hospital CRM | TBD |
| FR-030 | The doctor shall be able to add optional clinical notes, examination findings, advice, and follow-up information without requiring complex coding or extensive structured entry in V1. | TBD | Hospital CRM | TBD |
| FR-031 | Clinical information shall remain linked to the correct Patient ID and Visit ID. | TBD | Hospital CRM | TBD |
| FR-032 | Material edits to clinical notes, diagnosis, or prescription shall retain auditable information identifying the change, actor, and time. Exact revision/versioning UX remains TBD. | TBD | Hospital CRM | TBD |

## 6.6 Prescription Authoring and Medicine Availability

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-033 | The doctor shall be able to search/select a medicine primarily by medicine name and strength ("power"). Manufacturer information shall be stored for inventory/pharmacy context. Whether dosage form is mandatory remains TBD. | TBD | Hospital CRM | TBD |
| FR-034 | While prescribing, the doctor shall be shown the clinic pharmacy availability status for medicines known to the clinic pharmacy inventory. | TBD | Hospital CRM | TBD |
| FR-035 | Pharmacy availability shall distinguish at minimum: In Stock, Out of Stock, and Not Stocked. | TBD | Hospital CRM | TBD |
| FR-036 | A medicine's clinic-pharmacy unavailability shall not automatically prevent the doctor from prescribing it. | TBD | Hospital CRM | TBD |
| FR-037 | Prescription entry shall remain minimal because detailed verbal explanation is expected during consultation. For each medicine, V1 shall capture medicine name, strength, dose amount, frequency, and duration; short timing/food/extra instructions may be added when needed. | TBD | Hospital CRM | TBD |
| FR-038 | Where required quantity can be deterministically calculated from dose/frequency/duration and the inventory unit, the system shall calculate it automatically; otherwise the doctor shall enter the quantity. | TBD | Hospital CRM | TBD |
| FR-039 | The system shall allow the doctor to finalize a prescription for the visit. Once finalized, the prescription shall not be editable in place. | TBD | Hospital CRM | TBD |
| FR-040 | A finalized prescription shall remain associated with its Patient ID and Visit ID and be retrievable by authorized pharmacy staff. Pharmacy access shall not make the prescription editable. | TBD | Hospital CRM | TBD |

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
| FR-052 | If the clinic pharmacy has less than the prescribed quantity, pharmacy staff may partially dispense the quantity actually available. The supplied quantity shall be recorded and billed, inventory shall decrease only by the supplied quantity, and the remaining quantity shall be identified to the patient as not supplied by the clinic pharmacy. V1 does not maintain a back-order/collect-later workflow. | TBD | Hospital CRM | TBD |
| FR-053 | Pharmacy staff shall be able to identify prescribed medicines or remaining prescribed quantities that the patient must obtain outside the clinic pharmacy. | TBD | Hospital CRM | TBD |
| FR-054 | The pharmacist shall be able to explain to the patient which prescribed medicines or quantities were not supplied by the clinic pharmacy. | TBD | Hospital CRM | TBD |
| FR-055 | A pharmacist shall not independently substitute a prescribed medicine. Pharmacy may raise a substitution request to the doctor; only doctor approval permits the proposed substitution, and the request/decision shall be logged. | TBD | Hospital CRM | TBD |
| FR-056 | Medicine returns are not supported in V1. Any future return/refund workflow requires separate future evaluation. | TBD | Hospital CRM | TBD |
| FR-082 | The pharmacy workflow shall prevent dispensing more than the prescribed quantity across one or more dispensing actions and shall retain the quantity already dispensed against each prescription item. | TBD | Hospital CRM | TBD |
| FR-083 | V1 pharmacy dispensing through the CRM shall require a current finalized prescription; non-prescription retail/pharmacy-only dispensing is outside the confirmed V1 workflow. | TBD | Hospital CRM | TBD |

## 6.10 Pharmacy Billing and Payment

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-057 | Pharmacy staff shall be able to prepare a bill for medicines being supplied by the clinic pharmacy. | TBD | Hospital CRM | TBD |
| FR-058 | The pharmacy bill shall identify what the patient is being charged for. | TBD | Hospital CRM | TBD |
| FR-059 | Medicines not supplied by the clinic pharmacy shall not be included as dispensed bill items. | TBD | Hospital CRM | TBD |
| FR-060 | V1 shall record pharmacy payment status/information supplied by clinic staff but shall not process the payment itself. | TBD | Hospital CRM | TBD |
| FR-061 | Detailed pharmacy payment methods, partial-payment rules, refund rules, and cancellation rules remain TBD pending clinic confirmation. | TBD | Hospital CRM | TBD |
| FR-062 | Any pharmacy receipt/reference requirements remain TBD; V1 does not require a payment-processing integration. | TBD | Hospital CRM | TBD |
| FR-063 | Pharmacy staff shall update the recorded pharmacy payment status/information after the external payment activity is completed. | TBD | Hospital CRM | TBD |

## 6.11 Roles and Access

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-064 | The initial product shall support the roles Administrator, Receptionist, Doctor, and Pharmacist. | TBD | Hospital CRM | TBD |
| FR-065 | Receptionist access shall be limited to patient intake/retrieval, confirming demographic information, recording consultation payment status, selecting/reassigning the doctor queue, and queue operations required for reception duties. Reception shall not directly alter clinical information or directly apply demographic corrections after creation; a demographic-change request shall be sent to the doctor for approval. | TBD | Hospital CRM | TBD |
| FR-066 | Doctor access shall include patient clinical history needed for care, current consultation record, diagnosis, clinical notes, prescriptions, queue/payment status, demographic-change approval/editing, and inventory-control approvals. The doctor may directly edit patient demographics. | TBD | Hospital CRM | TBD |
| FR-067 | Pharmacist access shall be limited to current and previous prescriptions, known allergies, dispensing/billing/payment-status functions, and inventory-upkeep request entry. Pharmacists shall not directly edit a doctor's prescription. | TBD | Hospital CRM | TBD |
| FR-068 | Pharmacists may submit inventory-upkeep/change requests such as stock additions or non-dispensing stock adjustments. Such changes shall not alter inventory until approved by the doctor; the request, decision, actor, reason, and resulting change shall be logged. Normal prescription dispensing remains an automatic stock deduction and does not require a separate doctor approval. | TBD | Hospital CRM | TBD |
| FR-069 | Access shall be group-based (e.g., Doctor, Reception, Pharmacist groups) with privileges assigned to each group, while every staff member retains an individual account. Detailed Administrator privileges remain TBD. | TBD | Hospital CRM | TBD |

## 6.12 Audit, Correction, Cancellation, and Reprint

| ID | Functional Requirement | Priority | Primary Systems | Primary KPI |
| --- | --- | --- | --- | --- |
| FR-070 | The system shall retain audit information for material changes to patient, clinical, prescription, payment, and dispensing records. | TBD | Hospital CRM | TBD |
| FR-071 | Audit information shall identify at minimum the actor and time of the material change; exact before/after retention requirements remain TBD. | TBD | Hospital CRM | TBD |
| FR-072 | Important clinical and financial records shall not be permanently removed from normal workflow through an unaudited hard-delete action. | TBD | Hospital CRM | TBD |
| FR-073 | Where a prescription, payment, or other important record is cancelled/voided, the record shall preserve the fact of cancellation. | TBD | Hospital CRM | TBD |
| FR-074 | Doctor cancellation of a consultation and consultation-fee waiver decisions shall include a reason and shall be logged with the responsible actor. | TBD | Hospital CRM | TBD |
| FR-084 | Every staff member shall authenticate using an individual login under the clinic/hospital context. V1 shall use login credentials and password, and a password-reset capability is required. | TBD | Hospital CRM | TBD |
| FR-085 | V1 shall not require 2FA/MFA. | TBD | Hospital CRM | TBD |
| FR-086 | Pharmacy inventory shall support tracking from the lowest dispensable unit through higher packaging levels where applicable, with unit relationships sufficient to maintain stock accurately. | TBD | Hospital CRM | TBD |
| FR-087 | Inventory records shall support batch/lot number, expiry date, manufacturer, purchase price, and selling price. | TBD | Hospital CRM | TBD |
| FR-088 | The system shall provide low-stock and near-expiry notifications. | TBD | Hospital CRM | TBD |
| FR-089 | Expired stock shall be blocked from dispensing. The doctor shall be notified, and subsequent inventory disposition/adjustment shall require doctor action/approval and shall be logged. | TBD | Hospital CRM | TBD |

---

# 7. Business Rules and Journey Controls

## 7.1 Patient Identity Rules

**BR-001** — Patient ID is the permanent unique patient identifier.

**BR-002** — Phone number and patient name are search/matching attributes and are not substitutes for the unique Patient ID.

**BR-003** — One patient may have multiple Visit IDs over time.

**BR-004** — A new clinic visit for an existing patient must link to the existing Patient ID rather than generating a new patient identity.

**BR-005** — When similar existing patient records are surfaced, reception must review the candidate details with the patient and seek confirmation before deciding that an existing profile is the same person. Prior visit history and remembered visit purpose may be used to support confirmation. If no candidate can be confidently confirmed, reception may create a new profile marked **Possible Duplicate**.

**BR-025** — Phone number is not a unique patient identifier. Multiple patients, including family members, may share the same phone number.

## 7.2 Visit and Queue Rules

**BR-006** — Each clinic attendance has its own Visit ID.

**BR-007** — The consultation queue is visit-based, not patient-master-based.

**BR-008** — Queue state must be explicit and persisted.

**BR-009** — The doctor selects/calls patients through that doctor's assigned queue, with reception informed of the call.

**BR-027** — Consultation queues are doctor-specific. Reception assigns each eligible visit to a specific doctor before the visit enters the queue. A doctor sees the patients assigned to that doctor's queue rather than a shared clinic-wide queue.

**BR-028** — Reception may reassign a queued visit between doctor-specific queues; every reassignment is audited.

**BR-029** — Reception cannot cancel visits. The doctor may cancel a visit with a recorded reason, and cancelled visits remain in history.

**BR-030** — Finalized prescriptions are immutable in place. Pharmacists cannot edit them.

**BR-031** — Non-dispensing inventory adjustments initiated by pharmacy require doctor approval and audit logging. Prescription-linked dispensing deducts actual dispensed quantity automatically.

**BR-032** — Expired inventory is not dispensable. Doctor response controls inventory disposition/adjustment, not permission to dispense expired stock.

**BR-033** — V1 records payment status/information but does not process consultation or pharmacy payments.

**BR-010** — A visit with Unpaid consultation status must not enter the doctor queue unless a consultation-fee waiver has been approved by the doctor. A Paid consultation is eligible for queue entry. Partial consultation payment is not supported.

**BR-026** — Urgent-patient handling does not use a CRM priority mechanism. The receptionist communicates the urgent situation directly to the doctor outside the software; no priority flag, approval request, or automated priority reordering is required in the CRM.

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

**BR-020** — V1 stores payment status/information records for consultation and pharmacy events but does not process the underlying payment.

**BR-021** — Consultation payment is recorded as status/information only. Partial consultation payment is not supported, and consultation payments are non-refundable in V1.

**BR-022** — A consultation-fee waiver is an exception controlled by the doctor. Reception may request the waiver but may not approve it. The doctor may either approve a reception-initiated request or initiate the waiver directly. Every waiver requires a reason and is logged. Exact external payment methods remain clinic-defined/TBD.

## 7.6 Audit Rules

**BR-023** — Important clinical and financial records must not disappear through silent overwrite or unaudited hard deletion.

**BR-024** — Corrections/cancellations must preserve appropriate history.

---

# 8. Analytics and Reporting Requirements

The following V1 reporting/measurement needs are confirmed:

1. Patients seen per day.
2. Average waiting time.
3. Consultation revenue recorded by the system.
4. Pharmacy revenue recorded by the system.
5. Daily total recorded revenue.
6. Medicine sales.
7. Current stock.
8. Low-stock medicines.
9. Out-of-stock medicines.
10. Expiring medicines.
11. Most-prescribed medicines.
12. Consultation-fee waivers.
13. Cancellations; refund reporting applies only if a future/refined pharmacy refund flow is confirmed.
14. Returning patients.
15. Audit activity.

A per-doctor patient-count report is not required for the current single-doctor pilot.

The analytics/reporting implementation platform, event taxonomy, retention model, and exact KPI formulas remain TBD.

---

# 9. Dependencies and Open Delivery Items

The following items remain unresolved and must not be inferred.

## 9.1 Patient Registration

1. Exact algorithm/criteria used to surface possible duplicate candidates.
2. Whether duplicate-record merge is introduced in a later update and who may perform it.
3. Patient-ID format and whether it carries semantic meaning.
4. QR/barcode launch timing.

## 9.2 Visit and Queue

5. How an approved consultation-fee waiver is represented in the financial record.
6. Boundary behavior for the five-slot Unresponded move when fewer than five later slots remain.
7. Exact position rule for a paid patient moved toward the end after leaving before consultation.

## 9.3 Clinical Documentation

8. Rules for editing/amending a completed consultation record.
9. Whether a reason is mandatory for post-completion clinical amendments if such amendments are supported.

## 9.4 Prescription

10. Whether dosage form is required or optional.
11. Whether medicine naming is explicitly brand-based, generic/molecule-based, or a clinic-defined medicine name.
12. Correction/replacement workflow when a finalized immutable prescription contains an error.
13. Exact visual marker/text for clinic-unavailable medicines.

## 9.5 Pharmacy and Inventory

14. Exact unit-conversion rules between lowest dispensable units and higher package levels.
15. Low-stock threshold configuration.
16. Near-expiry threshold configuration.
17. Price-change approval/ownership behavior.
18. Detailed expired/damaged-stock disposition choices after doctor review.
19. Whether non-prescription retail is introduced in a future version.

## 9.6 Payments

20. Clinic-defined consultation payment methods to record.
21. Clinic-defined pharmacy payment methods to record.
22. Consultation fee determination/configuration.
23. Pharmacy partial-payment rule.
24. Pharmacy refund/cancellation rule.
25. Any receipt/reference requirements.
26. Pharmacy pricing/tax requirements.
27. Whether external payment integration is introduced later; it is not part of V1.

## 9.7 Roles, Security, and Operations

28. Detailed Administrator capabilities.
29. Staff onboarding/offboarding.
30. Session timeout/security requirements.
31. Exact password-reset flow.
32. Data backup/recovery requirements.
33. Legal/privacy/compliance requirements applicable to the clinic.
34. Data retention requirements.
35. Audit-log retention and access.

## 9.8 Deployment and Architecture

36. Hosting/deployment model.
37. Supported browser/computer requirements.
38. Number of reception/pharmacy workstations and expected concurrent users.
39. Performance expectations.
40. Existing data migration, if any.
41. External integrations, if any.

## 9.9 Analytics and Reporting

42. Exact formulas/definitions for confirmed reports.
43. Analytics/reporting platform.
44. Event model, if event tracking is required.
45. Report access permissions.
46. Data-retention period for reports.

---

# 10. Edge-Case Decisions and Remaining Gaps

Confirmed edge behavior now includes:

1. Suspected duplicate that cannot be confirmed -> create normal usable profile marked **Possible Duplicate**.
2. Multiple doctors -> doctor-specific queues; reception may reassign and reassignment is logged.
3. Urgent case -> reception informs the doctor outside the CRM; no priority feature.
4. Called patient does not respond -> mark Unresponded and move five queue positions down.
5. Paid patient leaves before consultation -> move visit toward the end of the queue.
6. Reception cannot cancel a visit; doctor may cancel with reason; cancelled visits remain in history.
7. Finalized prescription -> immutable in place.
8. Partial medicine availability -> dispense available quantity, bill/deduct only supplied quantity, and identify the remainder for outside purchase.
9. Substitution -> pharmacist requests; doctor approves/rejects; decision is logged.
10. Medicine returns -> not supported in V1.
11. Pharmacy-only/non-prescription dispensing -> outside confirmed V1 workflow.
12. Non-dispensing inventory changes -> pharmacist request; doctor approval; audit log.
13. Expired stock -> non-dispensable; doctor notified for disposition/adjustment; action logged.

Remaining edge details are retained in Section 9.

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
| Patient identity model | Confirmed; matching algorithm and future merge remain open |
| Visit identity model | Confirmed |
| Reception workflow | Confirmed at high level |
| Doctor queue | Doctor-specific queues and reassignment confirmed; boundary details remain open |
| Clinical notes | Guided low-complexity V1 model confirmed; post-completion edit rules remain open |
| Prescription | Minimal structure and immutability confirmed; naming/dosage-form/correction details remain open |
| Pharmacy availability | Confirmed |
| Pharmacy dispensing | Partial dispensing, substitution approval, no-return V1, and over-dispense prevention confirmed |
| Inventory controls | Multi-unit tracking, batch/expiry/pricing fields, alerts, and doctor-approved non-dispensing changes confirmed |
| Consultation payment | Status recording only; no CRM processing, no partial payment, no refund |
| Pharmacy payment | Status recording only; detailed payment/refund rules remain open |
| Roles | Group-based permissions with individual accounts confirmed; Admin details remain open |
| Authentication | Individual login/password + reset confirmed; no MFA in V1 |
| Audit history | Confirmed |
| Analytics/reporting | Confirmed report set; formulas/platform remain open |
| Deployment | Single-branch web/online-only pilot; A4 printing; hosting TBD |
| Third-party integrations | None confirmed |
| Legal/privacy/compliance details | Not yet confirmed |

---

# 13. Pilot Deployment Assumptions

Confirmed for the current pilot:

1. Single clinic branch.
2. Clinic owner is also the doctor managing the clinic.
3. Clinic has a separate pharmacy operation.
4. Product is web-based.
5. V1 requires internet connectivity; offline mode is not required.
6. Printing uses normal A4 printing for consultation/prescription outputs.
7. Thermal receipt printing is not required in V1.
8. Hosting/deployment provider remains TBD.

---

# 14. Finalization Status

This document is a structured requirements baseline, not a claim that discovery is complete.

It is suitable for continued product discovery and early solution planning, but remaining implementation-significant open decisions in Section 9 must be resolved progressively before the BRD can be treated as final implementation scope.

No unconfirmed third-party vendor, payment method, compliance regime, hosting architecture, or low-level technical design is asserted as fact in this version.
