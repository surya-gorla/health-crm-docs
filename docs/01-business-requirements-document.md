# Hospital CRM — Business Requirements Document

## Document Control

| Field | Value |
| --- | --- |
| Document | Business Requirements Document |
| Product | Hospital CRM for clinic operations |
| Version | 0.3 |
| Status | Baseline with open decisions |
| Date | 2026-09-20 |
| Scope stage | Initial clinic MVP |
| Requirement confidence | Confirmed requirements plus explicitly identified TBDs |

---

# 1. Purpose

This BRD defines the confirmed business requirements for a clinic-focused Hospital CRM covering the operational journey from patient identification/registration through reception payment, consultation queue, doctor consultation and clinical notes, prescription creation, pharmacy dispensing, and pharmacy payment.

The initial product is being designed around one real clinic where the owner is currently also the only doctor. However, the V1 role and operational model shall remain flexible enough for the same clinic to add multiple doctors, multiple receptionists, and multiple pharmacy units without redesigning the core permission model. Two primary operating needs remain (a) maintaining a reliable longitudinal archive of patients and visits and (b) giving the clinic owner strong visibility and control over pharmacy inventory because inventory loss/theft is an existing operational concern.

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

**BO-08 — Reduce hidden pharmacy inventory loss.**  
Give the clinic Owner full visibility into stock, dispensing, stock adjustments, damage/loss entries, and the staff actions that change inventory so unexplained stock reductions cannot be performed silently.

**BO-09 — Maintain a longitudinal patient archive.**  
Preserve patient visit history, consultation records, diagnoses, prescriptions, and relevant corrections over time so the doctor can retrieve a patient's historical record across visits.

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
28. Role-based access for Owner, Administrator, Receptionist, Doctor, and Pharmacist.
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
3. Advanced inventory automation beyond the confirmed V1 low-stock, near-expiry, dispensing, adjustment, and transfer controls.
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

## 4.4 Pilot operating context

1. The current V1 pilot is for a single clinic.
2. The clinic owner is also the clinic's only doctor.
3. The system distinguishes **Owner**, **Doctor**, **Reception**, **Pharmacist**, and **Administrator** authority instead of assuming that ownership and medical practice are the same role.
4. A single staff account may hold multiple roles. Therefore, an owner who also practices medicine uses one account with both Owner and Doctor permissions.
5. Reception and pharmacy staff are operational users; they are not clinical decision-makers.
6. Diagnosis, prescription finalization, clinical amendments, consultation-cancellation requests, and medicine-substitution approval remain Doctor-role actions; cancellation approval remains Owner authority.
7. Financial/ownership controls such as consultation-fee waiver approval and non-dispensing pharmacy inventory adjustments belong to the Owner role, not to every Doctor role.
8. The Owner requires clinic-wide visibility into pharmacy inventory, dispensing, stock transfers, inventory-adjustment requests, approvals/rejections, and audit history across all pharmacy units.
9. Inventory accountability is a core business goal because stock loss/theft is an existing clinic problem.
10. Patient history is intended to function as a durable longitudinal archive across visits.
11. Each doctor retains a doctor-specific queue even when several doctors work in the clinic.
12. Each pharmacy unit maintains its own stock ledger while contributing to an Owner-level consolidated inventory view.

## 4.5 Existing systems

No existing clinic software platform, external EMR/EHR, billing platform, pharmacy platform, payment gateway, identity provider, or other system has yet been confirmed.

Source-of-truth boundaries inside the final technical architecture are intentionally deferred to solution design. This BRD locks required business behavior without inventing an unconfirmed technical architecture.

---

# 5. System and Third-Party Integration Landscape

No third-party integration has been confirmed.

| Platform / Partner | Role | Scope / Status |
| --- | --- | --- |
| Hospital CRM application | Primary business application being defined | Confirmed product scope; exact architecture deferred to solution design |
| External payment provider | None required for V1 | Payment execution remains external to the CRM |
| External pharmacy/catalogue provider | None required for V1 | Clinic-managed medicine catalogue/inventory |
| External messaging/notification provider | None required for V1 | No messaging integration in confirmed V1 scope |
| External identity provider | None required for V1 | Local individual accounts; Owner uses Google Authenticator-compatible TOTP |

---

# 6. Functional Requirements

Requirement-by-requirement priority scoring is intentionally omitted because no priority model was confirmed. Per-requirement KPI columns are also omitted; the confirmed V1 reporting definitions are maintained in Section 8.

## 6.1 Patient Registration and Retrieval

| ID | Functional Requirement | Primary Systems |
| --- | --- | --- |
| FR-001 | Receptionist shall be able to search for an existing patient using Patient ID, phone number, or patient name before creating a new patient record. | Hospital CRM |
| FR-002 | The system shall generate a permanent unique Patient ID when a new patient is registered. | Hospital CRM |
| FR-003 | The Patient ID shall remain stable across all future visits for that patient. | Hospital CRM |
| FR-004 | When the system finds similar existing patient records during registration/search, reception shall review the candidate details with the patient and seek confirmation before creating or selecting a patient profile. Prior visit history and remembered visit purpose may be used to help the patient confirm the correct record. If the patient cannot confidently identify an existing candidate, reception may create a new patient profile and the system shall mark it as **Possible Duplicate**. | Hospital CRM |
| FR-005 | The receptionist shall be able to associate the generated Patient ID with the patient's physical clinic file. | Hospital CRM + physical file |
| FR-006 | The system shall allow correction of patient demographics without replacing the Patient ID. Reception may submit a proposed correction but cannot apply it directly. For an active visit, the request is routed to that visit's assigned Doctor; if no doctor is yet assigned, a Doctor must be selected before the request is sent. An approved correction records prior value, new value, requester, approving Doctor, and time. A Doctor may directly correct demographics, with prior/new values, actor, and time audited. | Hospital CRM |
| FR-007 | The system shall support returning patients who arrive without the physical file by locating the existing digital patient record using the supported search attributes. | Hospital CRM |
| FR-075 | Patient registration shall capture full name, phone number, date of birth, gender, address, and email as required registration information. Age shall be calculated from date of birth rather than maintained as an independent required input. | Hospital CRM |
| FR-076 | Emergency contact, blood group, known allergies, and guardian/parent details shall be optional patient-registration information. | Hospital CRM |
| FR-077 | Government ID shall be optional and shall not be required for patient registration. | Hospital CRM |
| FR-078 | The system shall allow multiple different patient records to share the same phone number; phone number shall not be enforced as a globally unique patient identifier. | Hospital CRM |
| FR-079 | A newly created record that could not be confidently distinguished from surfaced existing candidates shall remain usable as a normal patient profile while carrying a visible **Possible Duplicate** marker. Duplicate-profile merge is not part of the confirmed V1 workflow and remains for later evaluation. | Hospital CRM |

## 6.2 Visit Creation

| ID | Functional Requirement | Primary Systems |
| --- | --- | --- |
| FR-008 | Each clinic attendance shall create a unique Visit ID linked to the patient's permanent Patient ID. | Hospital CRM |
| FR-009 | Visit-level queue state, consultation record, prescription, and visit financial events shall be associated with the correct Visit ID. | Hospital CRM |
| FR-010 | A returning patient's new visit shall not create a new Patient ID. | Hospital CRM |

## 6.3 Reception and Consultation Payment

| ID | Functional Requirement | Primary Systems |
| --- | --- | --- |
| FR-011 | Receptionist shall be able to record the consultation fee/payment status information for the visit after payment activity occurs outside the CRM. | Hospital CRM |
| FR-012 | V1 shall not process consultation payments. It shall retain the consultation payment status/information required by the clinic for queue eligibility and reporting. | Hospital CRM |
| FR-013 | Consultation payment shall support Paid and Unpaid status for the normal flow. Partial consultation payment is not supported, and consultation payments are non-refundable in V1. Waiver is handled through the separate Owner-controlled waiver workflow and is recorded as Waived rather than Paid. | Hospital CRM |
| FR-014 | Consultation payment recording shall support the same optional external reference number defined for V1 payments. A standalone consultation receipt format is not a core V1 requirement; any clinic-specific A4 acknowledgement/receipt layout may be configured without changing the payment workflow. Thermal receipt printing and payment processing are outside V1. | Hospital CRM |
| FR-015 | A consultation-fee waiver may be requested from reception or a doctor, but only the Owner may approve it. If the Owner initiates the waiver directly, it is immediately approved without a second approval step. Every waiver requires a reason and audit record. | Hospital CRM |
| FR-016 | A Paid visit is eligible to enter the doctor queue. An Unpaid visit shall not enter the doctor queue unless its consultation-fee waiver request has been approved by the Owner. A pending or unapproved waiver request does not make the visit queue-eligible. | Hospital CRM |

## 6.4 Consultation Queue

| ID | Functional Requirement | Primary Systems |
| --- | --- | --- |
| FR-017 | Eligible visits shall be added to a consultation queue. | Hospital CRM |
| FR-018 | The queue shall preserve an explicit consultation order within the queue assigned to the selected doctor. Each doctor shall have a doctor-specific queue. | Hospital CRM |
| FR-019 | Queue/visit workflow shall support Waiting, Called, Unresponded, With Doctor, Consultation Completed, Sent to Pharmacy, Completed, and Cancelled/Voided. Unresponded is a transient queue state/event that returns the visit to Waiting after repositioning it according to FR-025. | Hospital CRM |
| FR-020 | The doctor shall be able to see the waiting patients assigned to that doctor's queue and identify the next patient according to the queue order. | Hospital CRM |
| FR-021 | The doctor shall be able to initiate a "call patient" action for a queued patient. | Hospital CRM |
| FR-022 | A doctor-initiated call shall be visible to reception so the receptionist can physically call/direct the patient. | Hospital CRM |
| FR-023 | Reception shall assign the visit to a specific Doctor before queue entry. Reception sees all active doctor queues needed for reception operations; each Doctor sees that Doctor's assigned queue; Owner sees clinic-wide queue status; Pharmacist sees only pharmacy-ready visit/prescription work needed for dispensing rather than doctor-queue controls. | Hospital CRM |
| FR-024 | The system shall not implement a priority flag, priority-request workflow, or automated priority reordering for urgent patients. If an urgent case arises, the receptionist shall communicate it directly to the doctor outside the CRM workflow. | Hospital CRM |
| FR-025 | When a called patient does not respond, reception shall mark the visit as Unresponded and the system shall move that visit five queue positions downward within the assigned doctor's queue; if fewer than five later positions exist, the visit moves to the end. If a paid patient leaves before consultation, reception shall move the visit to the end of the assigned doctor's queue. | Hospital CRM |
| FR-080 | Reception shall be able to reassign a queued visit from one doctor-specific queue to another doctor-specific queue. Every reassignment shall be logged. | Hospital CRM |
| FR-081 | Reception shall not be permitted to cancel a visit. A Doctor may request cancellation while the visit is still active (Waiting, Called, Unresponded, With Doctor, Consultation Completed, or Sent to Pharmacy); the request requires a specific free-text reason and Owner approval. If approved, the visit is removed from active workflow and marked Cancelled/Voided while all already-created clinical, prescription, dispensing, and financial history remains intact. A Completed visit is not cancelled through this flow; later corrections use the applicable amendment/correction process. If rejected, the active visit remains unchanged and the rejection is logged. | Hospital CRM |

## 6.5 Doctor Consultation and Clinical Record

| ID | Functional Requirement | Primary Systems |
| --- | --- | --- |
| FR-026 | The doctor shall be able to open the patient record for the selected queued visit. | Hospital CRM |
| FR-027 | The doctor shall be able to review previous patient visits relevant to clinical care. | Hospital CRM |
| FR-028 | The doctor consultation interface shall use a low-complexity guided structure with plain labels and limited mandatory entry. It shall capture a chief complaint/patient problem and allow optional symptoms/history details. | Hospital CRM |
| FR-029 | The doctor shall be able to record a clinical assessment/diagnosis for the current visit using a simple doctor-facing entry field. | Hospital CRM |
| FR-030 | The doctor shall be able to add optional clinical notes, examination findings, advice, and follow-up information without requiring complex coding or extensive structured entry in V1. | Hospital CRM |
| FR-031 | Clinical information shall remain linked to the correct Patient ID and Visit ID. | Hospital CRM |
| FR-032 | A completed consultation shall not be destructively edited. If correction is required, only a Doctor-role user may create an amendment/new revision. The original content shall remain preserved, an amendment reason is mandatory, and actor/time shall be recorded. | Hospital CRM |

## 6.6 Prescription Authoring and Medicine Availability

| ID | Functional Requirement | Primary Systems |
| --- | --- | --- |
| FR-033 | The doctor shall be able to search/select a medicine by medicine/display name, strength ("power"), and dosage form. Manufacturer information shall be stored. Generic/molecule name may be stored as an additional searchable attribute, while the clinic catalogue display name remains the primary selection label. | Hospital CRM |
| FR-034 | While prescribing, the doctor shall be shown the clinic pharmacy availability status for medicines known to the clinic pharmacy inventory. | Hospital CRM |
| FR-035 | Pharmacy availability shall distinguish at minimum: In Stock, Out of Stock, and Not Stocked. | Hospital CRM |
| FR-036 | A medicine's clinic-pharmacy unavailability shall not automatically prevent the doctor from prescribing it. | Hospital CRM |
| FR-037 | Prescription entry shall remain minimal because detailed verbal explanation is expected during consultation. For each medicine, V1 shall capture medicine name, strength, dose amount, frequency, and duration; short timing/food/extra instructions may be added when needed. | Hospital CRM |
| FR-038 | Where required quantity can be deterministically calculated from dose/frequency/duration and the inventory unit, the system shall calculate it automatically; otherwise the doctor shall enter the quantity. | Hospital CRM |
| FR-039 | The system shall allow the doctor to finalize a prescription for the visit. Once finalized, it shall not be editable in place. If correction is required, the doctor shall issue a replacement prescription; the previous prescription shall be marked Superseded, preserved in history, and linked to the replacement with a mandatory correction reason. | Hospital CRM |
| FR-040 | A finalized prescription shall remain associated with its Patient ID and Visit ID and be retrievable by authorized pharmacy staff. Pharmacy access shall not make the prescription editable. | Hospital CRM |

## 6.7 Printed Prescription

| ID | Functional Requirement | Primary Systems |
| --- | --- | --- |
| FR-041 | The doctor shall be able to print the finalized prescription. | Hospital CRM + printer |
| FR-042 | At doctor finalization/printing time, medicines whose clinic-wide status is Out of Stock or Not Stocked shall be visibly marked on the printed prescription using a clear double-asterisk (**) marker. A partial quantity discovered later during pharmacy dispensing shall not retroactively alter the original prescription. | Hospital CRM + printer |
| FR-043 | The printed prescription shall include a legend explaining that medicines marked ** were unavailable from the clinic pharmacy at prescription finalization and should be obtained externally. Any later partial-dispensing remainder is communicated and recorded in the pharmacy dispensing/billing summary. | Hospital CRM + printer |
| FR-044 | The system shall support reprinting a prescription without silently creating a new prescription version. | Hospital CRM + printer |

## 6.8 Pharmacy Prescription Retrieval and Access

| ID | Functional Requirement | Primary Systems |
| --- | --- | --- |
| FR-045 | Pharmacy staff shall be able to locate the relevant patient/visit using the Patient ID and retrieve the applicable prescription. Additional pharmacy lookup methods are not required for the locked V1 scope. | Hospital CRM |
| FR-046 | Pharmacy staff shall see the information required for dispensing, medicine instructions, availability, quantity, price/bill preparation, and pharmacy payment. | Hospital CRM |
| FR-047 | Pharmacy staff shall not receive unrestricted access to the doctor's complete clinical notes solely because they are dispensing medicines. | Hospital CRM |
| FR-048 | Pharmacist clinical visibility shall be limited to the current prescription, previous prescriptions, and known allergies required for dispensing. Pharmacist access shall not include unrestricted diagnosis or full clinical notes. | Hospital CRM |

## 6.9 Pharmacy Dispensing and Inventory

| ID | Functional Requirement | Primary Systems |
| --- | --- | --- |
| FR-049 | Pharmacy staff shall be able to see which prescribed medicines are available, out of stock, or not stocked. | Hospital CRM |
| FR-050 | Pharmacy staff shall be able to record the quantity of each available medicine actually dispensed. | Hospital CRM |
| FR-051 | When medicine is dispensed, clinic pharmacy inventory shall be reduced by the dispensed quantity. | Hospital CRM |
| FR-052 | If the clinic pharmacy has less than the prescribed quantity, pharmacy staff may partially dispense the quantity actually available. The supplied quantity shall be recorded and billed, inventory shall decrease only by the supplied quantity, and the remaining quantity shall be identified to the patient as not supplied by the clinic pharmacy. V1 does not maintain a back-order/collect-later workflow. | Hospital CRM |
| FR-053 | Pharmacy staff shall be able to identify prescribed medicines or remaining prescribed quantities that the patient must obtain outside the clinic pharmacy. | Hospital CRM |
| FR-054 | The pharmacist shall be able to explain to the patient which prescribed medicines or quantities were not supplied by the clinic pharmacy. | Hospital CRM |
| FR-055 | A pharmacist shall not independently substitute a prescribed medicine. Pharmacy may raise a substitution request to the doctor; only doctor approval permits the proposed substitution, and the request/decision shall be logged. | Hospital CRM |
| FR-056 | Medicine returns are not supported in V1. Any future return/refund workflow requires separate future evaluation. | Hospital CRM |
| FR-082 | The pharmacy workflow shall prevent dispensing more than the prescribed quantity across one or more dispensing actions and shall retain the quantity already dispensed against each prescription item. | Hospital CRM |
| FR-083 | V1 pharmacy dispensing through the CRM shall require a current finalized prescription; non-prescription retail/pharmacy-only dispensing is outside the confirmed V1 workflow. | Hospital CRM |

## 6.10 Pharmacy Billing and Payment

| ID | Functional Requirement | Primary Systems |
| --- | --- | --- |
| FR-057 | Pharmacy staff shall be able to prepare a bill for medicines being supplied by the clinic pharmacy. | Hospital CRM |
| FR-058 | The pharmacy bill shall identify what the patient is being charged for. | Hospital CRM |
| FR-059 | Medicines not supplied by the clinic pharmacy shall not be included as dispensed bill items. | Hospital CRM |
| FR-060 | V1 shall record pharmacy payment status/information supplied by clinic staff but shall not process the payment itself. | Hospital CRM |
| FR-061 | Pharmacy bills shall not support partial payment or refunds in V1. A pharmacy bill is either Unpaid or Paid for the normal payment flow. | Hospital CRM |
| FR-062 | Any pharmacy receipt/reference requirements remain clinic-configurable; V1 does not require a payment-processing integration or thermal receipt workflow. | Hospital CRM |
| FR-063 | Pharmacy staff shall update the recorded pharmacy payment status/information after the external payment activity is completed. The CRM records the payment method used but does not process the payment. | Hospital CRM |
| FR-105 | Pharmacy staff may request cancellation/void of a pharmacy bill only by entering a specific free-text reason. The request shall be sent to the Owner and shall not alter the active bill until the Owner approves it. | Hospital CRM |
| FR-106 | The Owner may approve or reject a pharmacy-bill cancellation request. If approved, the bill is removed from the active billing workflow and marked Cancelled/Voided while the original bill, payment state, requester, exact reason, Owner decision, and timestamps remain preserved in history/audit. If rejected, the active bill remains unchanged and the rejection is logged. | Hospital CRM |
| FR-107 | Cancellation/void is not a refund. If a pharmacy bill was already recorded as Paid, an approved cancellation shall not create or imply a refund in V1; the historical paid state remains auditable. | Hospital CRM |
| FR-108 | The default V1 payment-method choices shall be UPI, Cash, Card, and Other. Bank Transfer shall not be a separate default choice; if used, it may be recorded through Other. Payment-method recording is informational/reconciliation data only and does not initiate payment processing. | Hospital CRM |
| FR-109 | When Other is selected as the payment method, the user shall be required to enter a short free-text description of the actual payment method before the payment record can be saved. | Hospital CRM |
| FR-110 | Consultation and pharmacy payment recording shall support an optional external payment/reference number. The reference shall never be mandatory for marking a payment Paid in V1. | Hospital CRM |
| FR-111 | V1 payment processing shall remain external to the CRM. Cash is physically collected outside the system; UPI is completed using the clinic's external UPI/QR/payment app; Card is completed on the clinic's external card/POS terminal; Other is completed through the externally described method. Staff then record the result in the CRM. | Hospital CRM |
| FR-112 | The high-velocity reception payment interaction shall minimize steps: amount is prefilled where known, the user selects one of the four payment-method choices, optionally enters a reference, and confirms the payment. The system shall not require unnecessary reference-entry or gateway-wait steps before queue entry. | Hospital CRM |
| FR-113 | Owner-only and Administrator-only access shall not grant unrestricted clinical-note or diagnosis content. Full clinical-record content requires Doctor-role authority; operational/audit views may expose that a clinical record changed, who changed it, and when without automatically exposing clinical content. | Hospital CRM |
| FR-114 | If Reception or Pharmacy staff record a payment incorrectly, they shall not silently overwrite the financial record. They may submit a payment-correction request with the proposed correction and a specific reason; Owner approves or rejects it, and the original and resulting states remain auditable. | Hospital CRM |
| FR-115 | Approving cancellation/void of a pharmacy bill shall not automatically restore dispensed inventory. Dispensing history and stock deduction remain intact; any genuine stock correction requires a separate Owner-approved inventory-adjustment request. | Hospital CRM |
| FR-116 | When multiple pharmacy units exist, the Doctor prescribing view shall show clinic-wide availability plus per-pharmacy-unit availability where inventory is known, without allowing the Doctor to alter pharmacy stock. | Hospital CRM |
| FR-117 | Administrator may not grant, revoke, or modify Owner role authority or disable an Owner account. Owner-role lifecycle and Owner-level authority changes require Owner authority and shall be audited. | Hospital CRM |
| FR-118 | In a clinic with multiple pharmacy units, each dispensing/billing transaction shall belong to the pharmacy unit that actually dispensed the medicines. If one prescription is fulfilled by more than one pharmacy unit, each unit records/bills only what it dispensed, while cumulative dispensing across units remains capped by the active prescription. | Hospital CRM |

## 6.11 Roles and Access

| ID | Functional Requirement | Primary Systems |
| --- | --- | --- |
| FR-064 | The product shall support Owner, Administrator, Receptionist, Doctor, and Pharmacist roles. A user may hold more than one role on the same individual account. | Hospital CRM |
| FR-065 | Receptionist access shall be limited to patient intake/retrieval, confirming demographic information, recording consultation payment status, selecting/reassigning the doctor queue, and queue operations required for reception duties. Reception shall not directly alter clinical information or directly apply demographic corrections after creation; a demographic-change request shall be sent to the doctor for approval. | Hospital CRM |
| FR-066 | Doctor access shall include patient clinical history needed for care, current consultation record, diagnosis, clinical notes, prescriptions, queue/payment status, demographic-change approval/editing, medicine-substitution approval, and submission of consultation/visit cancellation requests. Doctor role alone shall not grant Owner inventory-control, waiver, staff-management, or clinic-wide financial authority. | Hospital CRM |
| FR-067 | Pharmacist access shall be limited to current and previous prescriptions, known allergies, dispensing/billing/payment-status functions, and inventory-upkeep request entry. Pharmacists shall not directly edit a doctor's prescription. | Hospital CRM |
| FR-068 | Pharmacists may submit inventory-upkeep/change requests such as stock additions or non-dispensing stock adjustments. Such changes shall not alter inventory until approved by the Owner; the request, decision, actor, reason, and resulting change shall be logged. Normal prescription dispensing remains an automatic stock deduction and does not require separate approval. | Hospital CRM |
| FR-069 | Access shall be group-based with Owner, Administrator, Reception, Doctor, and Pharmacist roles, while every user retains an individual account. Administrator may manage non-Owner staff accounts/roles, non-clinical configuration, medicine/inventory configuration where permitted, operational reports, and audit access; Administrator role alone shall not grant clinical authority or Owner authority. | Hospital CRM |

## 6.12 Audit, Correction, Cancellation, and Reprint

| ID | Functional Requirement | Primary Systems |
| --- | --- | --- |
| FR-070 | The system shall retain audit information for material changes to patient, clinical, prescription, payment, and dispensing records. | Hospital CRM |
| FR-071 | Audit information for material corrections shall identify actor and time and preserve the prior and resulting state/value where applicable, including demographic corrections, clinical amendments, prescription replacement, payment-status corrections, role changes, inventory adjustments/transfers, and cancellations/voids. Retention duration is governed by downstream compliance policy. | Hospital CRM |
| FR-072 | Important clinical and financial records shall not be permanently removed from normal workflow through an unaudited hard-delete action. | Hospital CRM |
| FR-073 | Where a prescription, payment, or other important record is cancelled/voided, the record shall preserve the fact of cancellation. | Hospital CRM |
| FR-074 | Consultation cancellation requests, pharmacy-bill cancellation requests, and consultation-fee waiver decisions shall require a specific reason where applicable and shall be logged with requester, Owner decision, actor, and time. | Hospital CRM |
| FR-084 | Every staff member shall authenticate using an individual login under the clinic context. V1 shall use individual login credentials and password. | Hospital CRM |
| FR-085 | Any account holding the Owner role shall require two-factor authentication (2FA) in addition to the account password before login is completed. The second factor shall use Google Authenticator-compatible time-based one-time passwords (TOTP). A user who holds Owner + Doctor or Owner + another role remains subject to Owner 2FA because the account has Owner privileges. | Hospital CRM |
| FR-100 | Non-Owner staff accounts, including Doctor-only, Reception, Pharmacist, and Administrator-only accounts, shall not require 2FA in V1. | Hospital CRM |
| FR-101 | A non-Owner staff member who selects Forgot Password shall submit a password-reset request to the Owner. Staff shall not be able to self-reset the password without Owner action in V1. | Hospital CRM |
| FR-102 | The Owner shall be able to review a staff password-reset request and set a new/temporary password. The system shall never reveal the staff member's existing password to the Owner or any other user. | Hospital CRM |
| FR-103 | After an Owner resets a staff password, the affected staff member shall be required to change that temporary/reset password after the next successful login. Password-reset request, Owner action, target account, and timestamp shall be logged. | Hospital CRM |
| FR-104 | When Owner TOTP 2FA is enrolled, the system shall generate one-time recovery codes for emergency access recovery. Recovery codes shall be shown only at enrollment/regeneration, stored securely, and become invalid after use. Regenerating recovery codes invalidates the previous set. | Hospital CRM |
| FR-086 | Pharmacy inventory shall support a medicine-specific base stock/dispensing unit plus configured higher package levels and conversion factors. Inventory movements shall normalize to the base unit while allowing entry/display in configured package units. | Hospital CRM |
| FR-087 | Inventory records shall support batch/lot number, expiry date, manufacturer, purchase price, and selling price. | Hospital CRM |
| FR-088 | The system shall provide low-stock and near-expiry notifications using medicine/inventory thresholds configurable by an authorized Owner/Admin role rather than fixed global values. | Hospital CRM |
| FR-089 | Expired stock shall be blocked from dispensing. The Owner shall be notified, and removal/adjustment of expired, damaged, lost, or otherwise unavailable quantity shall use the Owner-approved inventory-adjustment workflow with category, reason, quantity, actor, and time logged. Physical disposal is outside the CRM V1 workflow. | Hospital CRM |
| FR-090 | The Owner shall have complete clinic-wide inventory oversight, including current stock by pharmacy unit, consolidated stock, package/base-unit quantities, dispensing history, stock additions, transfers, adjustments, damage/loss entries, expiry-related removals, request originator, approval/rejection, reason, and timestamp. | Hospital CRM |
| FR-091 | A Doctor assigned to the current visit shall be able to retrieve the patient's longitudinal archive across visits, including historical consultations, diagnoses, prescriptions, and preserved amendments/superseded records needed for care. | Hospital CRM |
| FR-092 | The system shall support multiple doctors within one clinic, each with a separate doctor-specific queue and individual account. Reception shall assign/reassign visits to a doctor without exposing one doctor's queue actions to another doctor's queue. | Hospital CRM |
| FR-093 | The system shall support multiple receptionists through individual accounts in the Reception group. Receptionists may share the same operational queue workspace while every action remains attributable to the individual staff account. | Hospital CRM |
| FR-094 | The system shall support multiple pharmacy units within one clinic. Each pharmacy unit shall maintain its own stock ledger, dispensing history, and pharmacy staff scope. | Hospital CRM |
| FR-095 | Every dispensing action shall be associated with a specific pharmacy unit. Dispensing across one or more pharmacy units shall not permit cumulative dispensed quantity to exceed the active prescription quantity. | Hospital CRM |
| FR-096 | Pharmacy-to-pharmacy stock transfer shall be represented as one controlled transfer transaction: source decrease and destination increase shall be linked, attributable, and Owner-approved before completion. | Hospital CRM |
| FR-097 | The Owner shall have an Owner Dashboard with clinic-wide operational, inventory, financial-status, staff, approval, and audit visibility. Owner role alone shall not grant clinical-authoring authority. | Hospital CRM |
| FR-098 | A user holding both Owner and Doctor roles shall use one account and shall have clearly separated Owner and Doctor workspaces/modes so elevated operational controls are not silently mixed into routine clinical actions. | Hospital CRM |
| FR-099 | A Doctor who is not the Owner shall not receive Owner-only inventory-adjustment, stock-transfer, staff-management, or financial-oversight privileges merely because that user has Doctor permission. | Hospital CRM |

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

**BR-010** — A visit with Unpaid consultation status must not enter the doctor queue unless a consultation-fee waiver has been approved by the Owner. A Paid consultation is eligible for queue entry. Partial consultation payment is not supported.

**BR-026** — Urgent-patient handling does not use a CRM priority mechanism. The receptionist communicates the urgent situation directly to the doctor outside the software; no priority flag, approval request, or automated priority reordering is required in the CRM.

**BR-027** — Consultation queues are doctor-specific. Reception assigns each eligible visit to a specific doctor before the visit enters the queue. A doctor sees the patients assigned to that doctor's queue rather than a shared clinic-wide queue.

**BR-028** — Reception may reassign a queued visit between doctor-specific queues; every reassignment is audited.

**BR-029** — Reception cannot cancel visits. A Doctor may request visit/consultation cancellation with a specific reason, but only the Owner can approve or reject the cancellation. Approved cancellation removes the visit from active workflow while preserving the cancelled/voided record and full audit history.

**BR-049** — Consultation/visit cancellation is an Owner-controlled workflow. Doctor requests with a specific reason; Owner approves/rejects; approval removes the visit from active operational workflow but preserves the cancelled record.

## 7.3 Clinical Record and Archive Rules

**BR-011** — Clinical notes, diagnosis, and prescription are recorded against the current Visit ID and linked Patient ID.

**BR-012** — Completed clinical records are amended through a doctor-authored revision rather than destructive editing. Original content is retained, amendment reason is mandatory, and actor/time are audited.

**BR-013** — Pharmacy access does not automatically grant access to the doctor's full clinical notes.

**BR-038** — Patient records form a longitudinal archive: historical visits and superseded/amended material records are preserved rather than replaced by only the latest state.

## 7.4 Prescription, Pharmacy, and Inventory Rules

**BR-014** — Pharmacy availability is informative to the doctor and does not by itself prohibit prescribing an unavailable medicine.

**BR-015** — Availability states must distinguish In Stock, Out of Stock, and Not Stocked.

**BR-016** — Unavailable medicines are marked on the printed prescription so the patient can identify medicines to obtain outside.

**BR-017** — Dispensed quantity reduces clinic pharmacy inventory.

**BR-018** — Medicines not dispensed by the clinic pharmacy must not be billed as dispensed items.

**BR-030** — Finalized prescriptions are immutable in place. Correction requires a doctor-issued replacement linked to a preserved Superseded prescription. Pharmacists cannot edit prescriptions.

**BR-031** — Non-dispensing inventory adjustments initiated by pharmacy require Owner approval and audit logging. Prescription-linked dispensing deducts actual dispensed quantity automatically.

**BR-032** — Expired inventory is not dispensable. Owner approval controls the inventory disposition/adjustment record, not permission to dispense expired stock.

**BR-037** — Inventory changes must be attributable. Normal prescription dispensing is automatically recorded; all non-dispensing stock additions/reductions/corrections and inter-pharmacy transfers require the controlled Owner-approval flow so unexplained inventory movement cannot be hidden.

**BR-040** — Multiple pharmacy units maintain separate stock ledgers. Clinic-wide totals are derived from those ledgers and must not replace unit-level accountability.

**BR-041** — Inventory transfer between pharmacy units is a linked, auditable movement and cannot be represented as unrelated manual source/destination adjustments.

**BR-056** — Pharmacy-bill cancellation/void never reverses physical dispensing automatically. Stock correction, when legitimate, is a separate Owner-approved inventory adjustment.

**BR-058** — In a multi-pharmacy clinic, prescribing availability is visible at both clinic-wide and pharmacy-unit levels; inventory control remains with pharmacy/Owner workflows.

## 7.5 Payment and Financial-Control Rules

**BR-019** — Consultation and pharmacy payment are separate financial events.

**BR-020** — V1 stores payment status/information records for consultation and pharmacy events but does not process the underlying payment.

**BR-021** — Consultation payment is recorded as status/information only. Partial consultation payment is not supported, and consultation payments are non-refundable in V1.

**BR-022** — A consultation-fee waiver is an ownership/financial exception controlled by the Owner. Reception or a Doctor may request the waiver but may not approve it unless that same user also holds the Owner role. The Owner may approve a request or initiate the waiver directly. Every waiver requires a reason and is logged.

**BR-033** — V1 records payment status/information but does not process consultation or pharmacy payments.

**BR-047** — Pharmacy bills do not support partial payment or refunds in V1.

**BR-048** — Pharmacy-bill cancellation is an Owner-controlled void workflow. Pharmacist requests with a specific reason; Owner approves/rejects; approval removes the bill from active billing but never deletes its historical/audit record.

**BR-050** — Cancellation/void is distinct from refund. A paid consultation or paid pharmacy bill is not refunded by approving cancellation under V1.

**BR-051** — The default V1 payment-method set is UPI, Cash, Card, and Other. Selecting Other requires a description of the actual method. The CRM records how an external payment occurred and does not initiate or settle the payment.

**BR-052** — Payment reference/transaction ID is optional in V1 for both consultation and pharmacy payments.

**BR-053** — V1 keeps payment processing outside the CRM so reception/pharmacy workflows are not blocked by payment-gateway latency or availability. Staff confirm successful external payment and then record Paid status in the CRM.

**BR-055** — Payment-status corrections are Owner-controlled and audited; staff may request a correction but may not silently rewrite a recorded payment.

## 7.6 Roles, Access, and Authentication Rules

**BR-034** — Administrator permission alone does not grant clinical-authoring authority. Clinical notes, diagnosis, prescriptions, clinical amendments, and doctor approvals require Doctor-group permission.

**BR-035** — The single-clinic pilot uses a fixed clinic context at login. Every user uses an individual username/password account. Staff forgotten-password recovery is Owner-controlled in V1.

**BR-036** — Ownership and clinical authority are separate permissions. Reception and pharmacy staff may perform their permitted operational/data-entry workflows but may not independently diagnose, prescribe, finalize clinical records, or perform Doctor-only clinical approvals. A clinic owner who also practices medicine receives those clinical permissions through a separate Doctor role on the same account.

**BR-039** — One individual account may hold multiple roles. Role combination adds the permissions of those roles but does not convert one role into another; for example, Owner+Doctor has both workspaces, while Doctor-only does not gain Owner controls.

**BR-042** — Owner role is the default approval authority for financial waiver approval and non-dispensing inventory control. Doctor role remains the authority for clinical decisions such as prescriptions and medicine substitutions.

**BR-043** — Owner privilege is security-sensitive: every account containing the Owner role requires Google Authenticator-compatible TOTP 2FA. Non-Owner staff accounts do not require 2FA in V1.

**BR-044** — Staff forgotten-password recovery is a request/Owner-reset workflow. The Owner may replace the password but may never view or retrieve the existing password.

**BR-045** — An Owner-performed staff password reset creates a temporary/reset credential that the staff member must replace after the next successful login. The reset action is auditable.

**BR-046** — Owner TOTP enrollment generates one-time recovery codes. Recovery codes are emergency credentials, are not visible again after enrollment/regeneration, and each code is invalid after use. If the Owner loses password/authenticator access and has no valid recovery code, recovery requires a controlled technical recovery process rather than an in-app bypass.

**BR-054** — Owner/Admin operational authority does not automatically grant access to full clinical content; clinical record content requires Doctor-role authority.

**BR-057** — Administrator cannot grant/revoke Owner authority or disable Owner accounts. Owner-role lifecycle changes require Owner authority and audit history.

## 7.7 Audit and Record-Preservation Rules

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
13. Cancellation/void requests and Owner decisions; refunds are not supported in V1.
14. Returning patients.
15. Audit activity.

A per-doctor patient-count report is not required for the current single-doctor pilot.

The business definitions for the confirmed V1 reports are fixed in the decision register. Chart/layout implementation, reporting technology, export format, and retention duration remain downstream implementation/policy decisions.

---

# 9. Post-Lock Configuration and Delivery Dependencies

The following items are intentionally outside the locked core workflow. They are configuration, future-scope, compliance, or technical-delivery dependencies and must not be silently inferred.

## 9.1 Patient Registration

1. Duplicate-record merge is a later-phase evaluation and is not a V1 blocker.
2. QR/barcode launch timing is a later-phase decision.
3. Patient matching thresholds and internal ID implementation are solution-design details, not remaining BRD policy decisions.

## 9.2 Visit and Queue

4. No remaining queue-policy decision is required for the current V1 baseline.

## 9.3 Clinical Documentation

5. No remaining V1 clinical-authority decision: clinical decision/finalization actions require the Doctor role. In the current pilot the Owner is also the only Doctor; if more doctors are added, each receives Doctor permissions independently.

## 9.4 Prescription

6. No remaining prescription-lifecycle decision is required for V1 beyond clinic validation of the medicine catalogue data it wants to preload.

## 9.5 Pharmacy and Inventory

7. Initial low-stock and near-expiry threshold values are operational configuration, not BRD decisions.
8. Pharmacy-proposed medicine price changes use the same pharmacist-request -> Owner-approval workflow as other non-dispensing inventory changes.
9. Non-prescription retail remains future/out of V1.

## 9.6 Payments

10. Default payment methods are UPI, Cash, Card, and Other; Other requires a description. Payment reference is optional.
11. Consultation fee amount and any doctor/service-specific fee values are clinic-supplied configuration before go-live.
12. Any clinic-specific formal A4 receipt/acknowledgement format is configuration, not a change to the payment workflow.
13. Pharmacy pricing/tax values are clinic-supplied configuration where applicable.
14. External payment integration remains out of V1 unless explicitly introduced in a later version.

## 9.7 Roles, Security, and Operations

15. Staff onboarding/offboarding operating procedure may be documented separately; account creation, disabling, role assignment, and password-reset authority are already defined.
16. Data backup/recovery targets are finalized in technical architecture.
17. Legal/privacy/compliance and data-retention requirements require external validation.
18. Audit-retention duration requires compliance/policy validation.

## 9.8 Deployment and Architecture

19. Hosting/deployment model is intentionally deferred to technical architecture.
20. Historical data migration is required only if the clinic later supplies an existing digital source to import.
21. No third-party integration is required by the locked V1 workflow; any later integration is subject to change control.

## 9.9 Analytics and Reporting

22. Analytics/reporting technology, export format, and report-retention duration are technical/policy items, not remaining business-discovery questions.

---

# 10. Edge-Case Decisions and Remaining Gaps

Confirmed edge behavior now includes:

1. Suspected duplicate that cannot be confirmed -> create normal usable profile marked **Possible Duplicate**.
2. Multiple doctors -> doctor-specific queues; reception may reassign and reassignment is logged.
3. Urgent case -> reception informs the doctor outside the CRM; no priority feature.
4. Called patient does not respond -> mark Unresponded and move five queue positions down.
5. Paid patient leaves before consultation -> move visit to the end of the assigned doctor's queue.
6. Reception cannot cancel a visit; Doctor may request cancellation with a specific reason; Owner approves/rejects; approved cancellation leaves active workflow but remains in history.
7. Finalized prescription -> immutable in place.
8. Partial medicine availability -> dispense available quantity, bill/deduct only supplied quantity, and identify the remainder for outside purchase.
9. Substitution -> pharmacist requests; doctor approves/rejects; decision is logged.
10. Medicine returns -> not supported in V1.
11. Pharmacy-only/non-prescription dispensing -> outside confirmed V1 workflow.
12. Non-dispensing inventory changes -> pharmacist request; Owner approval; audit log.
13. Expired stock -> non-dispensable; Owner notified for disposition/adjustment; action logged.

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
9. Printed Prescription and A4 Billing/Payment Outputs.
10. Users, Roles, and Audit Logs.

---

# 12. Requirements Status Summary

| Category | Status |
| --- | --- |
| Patient identity model | Confirmed for V1; duplicate merge is future and matching thresholds are solution design |
| Visit identity model | Confirmed |
| Reception workflow | Confirmed at high level |
| Doctor queue | Confirmed, including reassignment and non-response/leave boundary behavior |
| Clinical notes | Confirmed, including Doctor-only amendment/revision workflow |
| Prescription | Confirmed for V1, including medicine identity, immutable finalization, and supersede/replace correction |
| Pharmacy availability | Confirmed |
| Pharmacy dispensing | Partial dispensing, substitution approval, no-return V1, and over-dispense prevention confirmed |
| Inventory controls | Multi-unit tracking, batch/expiry/pricing fields, alerts, Owner-approved non-dispensing changes, and multi-pharmacy ledgers/transfers confirmed |
| Consultation payment | Status recording only; no CRM processing, no partial payment, no refund |
| Pharmacy payment | Status recording only; no partial payment, no refunds; Owner-controlled cancellation/void confirmed; UPI/Cash/Card/Other default methods; optional reference |
| Roles | Owner, Doctor, Reception, Pharmacist, and Admin roles with multi-role individual accounts confirmed |
| Authentication | Individual login/password confirmed; Owner accounts require 2FA; non-Owner staff do not; staff password reset is Owner-controlled |
| Audit history | Confirmed at business level; retention duration remains compliance policy |
| Analytics/reporting | V1 report set and business definitions confirmed; implementation platform remains technical design |
| Deployment | Single-branch web/online-only pilot; A4 printing; hosting deferred to technical architecture |
| Third-party integrations | None confirmed |
| Legal/privacy/compliance details | External validation dependency; not a core workflow decision |

---

# 13. Pilot Deployment Assumptions

Confirmed for the current pilot:

1. Single clinic branch.
2. Clinic owner is currently also the only doctor.
3. Clinic has a separate pharmacy operation.
4. The role/unit design supports adding multiple doctors, receptionists, and pharmacy units within the same clinic without changing the core permission model.
5. Product is web-based.
6. V1 requires internet connectivity; offline mode is not required.
7. Printing uses normal A4 printing for consultation/prescription and configured A4 billing/payment outputs.
8. Thermal receipt printing is not required in V1.
9. Hosting/deployment provider is deferred to technical architecture.

---

# 14. Finalization Status

The core V1 business workflow and product behavior are substantially defined and are suitable for BRD lock after the remaining clinic-supplied payment/billing policy items are either provided or explicitly marked as post-lock configuration dependencies.

Items such as hosting, backup/recovery targets, legal/privacy/compliance validation, audit-retention duration, and low-level security implementation remain downstream technical/compliance dependencies rather than reasons to reopen the core clinic workflow.

No unconfirmed third-party vendor, payment method, compliance regime, hosting architecture, or low-level technical design is asserted as fact in this version.
