# Hospital CRM — PRD Acceptance and Traceability

## Document Control

| Field | Value |
| --- | --- |
| Document | PRD Acceptance and Traceability |
| Version | 0.1 |
| Status | DRAFT |
| Date | 2026-09-20 |
| Parent | Product Requirements Document v0.1 |
| Business source | BRD v1.0 LOCKED |

---

# 1. Purpose

This document makes the PRD testable.

It provides:

- critical end-to-end product acceptance scenarios;
- role/authority acceptance;
- state-transition acceptance;
- financial/inventory control acceptance;
- release-blocking negative scenarios;
- PRD-to-BRD traceability.

The locked BRD remains authoritative.

---

# 2. Acceptance Conventions

The scenarios use:

- **Given** — starting business/product state;
- **When** — user action;
- **Then** — required product result.

A scenario fails if the product reaches the right visual result while violating authority, history, or audit requirements.

---

# 3. Critical End-to-End Acceptance

## AC-001 — Existing patient normal consultation flow

**Given** an existing Patient can be found and has no conflicting identity ambiguity  
**When** Reception selects the Patient, creates a Visit, records Paid consultation, assigns Doctor, and queues the Visit  
**Then** the same Patient ID is reused, a new Visit ID is created, the Visit enters only the assigned Doctor queue, and the payment/queue actions are attributable.

Sources: FR-001, FR-003, FR-008–FR-010, FR-016–FR-018, BR-001, BR-006, BR-010.

## AC-002 — New patient

**Given** no existing patient can be confidently identified  
**When** Reception completes required registration  
**Then** the system creates permanent Patient ID and first Visit uses that Patient ID.

Sources: FR-002, FR-004, FR-075–FR-079, OD-001, OD-002.

## AC-003 — Possible duplicate fallback

**Given** similar candidates are shown but the patient cannot confidently identify an existing profile  
**When** Reception proceeds with new registration  
**Then** a usable profile is created with Possible Duplicate marker and no auto-merge occurs.

Sources: FR-004, FR-079, BR-005, OD-002.

## AC-004 — Shared family phone

**Given** two patients share a phone  
**When** Reception searches that number  
**Then** both may appear and neither is silently treated as the same patient.

Sources: FR-078, BR-025, OD-003.

## AC-005 — Unpaid queue block

**Given** consultation status is Unpaid and no Owner-approved waiver exists  
**When** Reception attempts queue entry  
**Then** the product blocks queue entry.

Sources: FR-016, BR-010, OD-004.

## AC-006 — Waiver

**Given** Reception or Doctor submits waiver with reason  
**When** Owner approves  
**Then** outcome becomes Waived, not Paid, the Visit becomes queue-eligible, and requester/reason/decision/time are preserved.

Sources: FR-015, FR-016, FR-074, BR-022, OD-005.

## AC-007 — Queue call

**Given** a Waiting Visit is in Doctor A queue  
**When** Doctor A triggers Call Patient  
**Then** queue state becomes Called and Reception can see the call.

Sources: FR-020–FR-022.

## AC-008 — Unresponded reposition

**Given** a Called patient does not respond  
**When** Reception marks Unresponded  
**Then** event is recorded and Visit returns to Waiting five positions lower, or at end if fewer positions remain.

Sources: FR-019, FR-025, OD-007.

## AC-009 — Queue reassignment

**Given** an active Visit is in Doctor A queue  
**When** Reception reassigns to Doctor B  
**Then** it leaves Doctor A queue, appears in Doctor B queue, and reassignment is logged.

Sources: FR-080, BR-028, OD-007.

## AC-010 — Consultation and archive

**Given** Doctor opens assigned Visit  
**When** Doctor records required consultation fields  
**Then** content is linked to Patient + Visit and relevant longitudinal history remains accessible.

Sources: FR-026–FR-031, FR-091.

## AC-011 — Completed consultation amendment

**Given** consultation is completed  
**When** Doctor corrects clinical content  
**Then** original is preserved, amendment requires reason, and latest effective content is distinguishable from history.

Sources: FR-032, BR-012, OD-009.

## AC-012 — Prescription availability

**Given** medicine is in catalogue  
**When** Doctor selects it  
**Then** product shows clinic pharmacy availability without blocking prescription solely because it is unavailable.

Sources: FR-033–FR-036.

## AC-013 — Finalized prescription replacement

**Given** prescription is finalized  
**When** Doctor discovers an error  
**Then** in-place edit is unavailable; replacement requires reason; old prescription is Superseded; history remains.

Sources: FR-039, BR-030, OD-012.

## AC-014 — Printed unavailable medicine

**Given** medicine is Out of Stock or Not Stocked at finalization  
**When** prescription is printed  
**Then** it contains ** marker and explanatory legend.

Sources: FR-042, FR-043.

## AC-015 — Pharmacy visibility boundary

**Given** Pharmacist opens current prescription  
**Then** current/previous prescriptions and allergies are available, but unrestricted diagnosis/full clinical notes are not.

Sources: FR-047, FR-048, FR-067, BR-013.

## AC-016 — Partial dispensing

**Given** prescribed quantity exceeds stock  
**When** Pharmacist dispenses available quantity  
**Then** actual quantity is recorded, stock drops only by supplied quantity, bill includes only supplied quantity, and remainder is unsupplied.

Sources: FR-052–FR-054, OD-013.

## AC-017 — Over-dispense prevention

**Given** some prescription quantity was already dispensed  
**When** another dispense would exceed prescribed total  
**Then** product blocks the excess across all pharmacy units/actions.

Sources: FR-082, FR-095, OD-013.

## AC-018 — Substitution

**Given** Pharmacist wants alternative medicine  
**When** substitution is requested  
**Then** alternative cannot be dispensed until Doctor approves; decision is logged.

Sources: FR-055, OD-014.

## AC-019 — External pharmacy payment

**Given** pharmacy bill is ready  
**When** external payment succeeds  
**Then** Pharmacist records Paid with UPI/Cash/Card/Other and optional reference; Other requires description.

Sources: FR-060–FR-063, FR-108–FR-112, OD-018.

## AC-020 — Pharmacy no partial/no refund

**Given** pharmacy bill exists  
**Then** partial-payment and refund actions are not available in V1.

Sources: FR-061, BR-047, BR-050, OD-019.

## AC-021 — Pharmacy bill void

**Given** Pharmacist requests void with specific reason  
**When** Owner approves  
**Then** bill becomes Cancelled/Voided, exits active billing, history remains, and no refund is created.

Sources: FR-105–FR-107, BR-048, OD-019.

## AC-022 — Bill void does not restore stock

**Given** medicine was physically dispensed and stock reduced  
**When** related bill is later voided  
**Then** dispensing and stock deduction remain unchanged unless separate Owner-approved inventory adjustment occurs.

Sources: FR-115, BR-056, OD-034.

## AC-023 — Normal dispensing stock movement

**Given** finalized prescription and available stock  
**When** Pharmacist dispenses  
**Then** inventory automatically deducts actual quantity and movement is attributable without Owner approval.

Sources: FR-051, BR-017, BR-031.

## AC-024 — Inventory adjustment approval

**Given** Pharmacist proposes damage/loss/correction/addition  
**When** request is pending  
**Then** stock does not change.  
**When** Owner approves  
**Then** stock changes and request/reason/decision/result are logged.

Sources: FR-068, BR-031, OD-030.

## AC-025 — Expired stock

**Given** batch is expired  
**Then** product does not allow dispensing, regardless of Owner decision; Owner may only control disposition/adjustment record.

Sources: FR-089, BR-032, OD-031.

## AC-026 — Pharmacy transfer

**Given** stock moves Pharmacy A -> Pharmacy B  
**When** Owner approves linked transfer  
**Then** source decreases, destination increases, and both sides share one transfer reference.

Sources: FR-096, BR-041, OD-023.

## AC-027 — Multi-pharmacy fulfilment

**Given** one prescription is fulfilled by Pharmacy A and B  
**Then** each unit records/bills only its own supplied quantity and cumulative quantity cannot exceed prescription.

Sources: FR-095, FR-118, OD-037.

## AC-028 — Consultation cancellation

**Given** Doctor submits cancellation request on active Visit with reason  
**When** Owner approves  
**Then** Visit becomes Cancelled/Voided and leaves active workflow, no refund is created, and already-created history remains.

Sources: FR-081, BR-029, BR-049, BR-050, OD-019.

## AC-029 — Payment correction

**Given** Reception/Pharmacy incorrectly recorded payment  
**When** staff submits correction with reason and Owner approves  
**Then** effective state is corrected without erasing original payment record.

Sources: FR-114, BR-055, OD-033.

## AC-030 — Staff Forgot Password

**Given** non-Owner staff forgot password  
**When** staff submits reset and Owner sets temporary credential  
**Then** old password is never exposed and staff must change reset credential after next successful login.

Sources: FR-101–FR-103, BR-044–BR-045, OD-021.

---

# 4. Role and Authority Acceptance

## RA-001 — Owner-only is not Doctor

Owner-only account must not author diagnosis, prescription, clinical amendment, or view unrestricted full clinical content.

Sources: FR-113, BR-054, OD-035.

## RA-002 — Doctor-only is not Owner

Doctor-only account must not approve waiver, manual inventory change, transfer, payment correction, bill void, or staff Owner-level controls.

Sources: FR-066, FR-099, BR-039, BR-042.

## RA-003 — Pharmacist is not Doctor

Pharmacist cannot edit/finalize prescription or approve substitution.

Sources: FR-040, FR-055, FR-067.

## RA-004 — Reception is not Doctor

Reception cannot directly overwrite established patient demographics, diagnose, prescribe, or cancel Visit.

Sources: FR-006, FR-065, FR-081.

## RA-005 — Admin is not Owner

Admin cannot grant/revoke Owner or disable Owner.

Sources: FR-117, BR-057, OD-036.

## RA-006 — Multi-role account

Owner + Doctor uses one identity but both permission sets; product must keep authority context explicit.

Sources: FR-064, FR-098, BR-039, OD-023.

---

# 5. Authentication Acceptance

## AU-001 — Owner 2FA mandatory

Account containing Owner role cannot complete normal login using password alone.

Sources: FR-085, BR-043, OD-021.

## AU-002 — Staff no 2FA

Non-Owner Doctor/Reception/Pharmacist/Admin accounts are not required to complete 2FA in V1.

Sources: FR-100, BR-043, OD-021.

## AU-003 — Disabled account

Disabled account cannot log in and historical actions remain attributable.

Sources: OD-022 staff lifecycle.

---

# 6. Negative / Release-Blocking Scenarios

Release must fail if any of these are possible:

1. create second Patient ID merely because physical file is missing;
2. auto-merge duplicate candidates;
3. enter queue while Unpaid without approved waiver;
4. Reception approve its own waiver;
5. Doctor approve Owner-only financial/inventory action solely through Doctor role;
6. Pharmacist substitute without Doctor approval;
7. edit finalized prescription in place;
8. erase original completed consultation during correction;
9. over-dispense a prescription;
10. dispense expired stock;
11. manually reduce stock without Owner approval;
12. void a pharmacy bill and automatically restore already-dispensed stock;
13. record partial pharmacy payment;
14. refund consultation/pharmacy payment through V1;
15. Admin self-grant Owner;
16. Owner-only read unrestricted full clinical notes;
17. mark payment correction without preserving original state;
18. cancel/void and hard-delete the original history;
19. mix Pharmacy A and Pharmacy B stock without unit attribution;
20. allow Owner account to bypass required TOTP.

---

# 7. Product Requirement to BRD Traceability

| PRD Area | Product Requirements | Primary BRD Sources |
| --- | --- | --- |
| Workspace / shell | P-001–P-007 | FR-064–FR-069, FR-097–FR-100, BR-034–BR-043 |
| Authentication | P-008–P-015 | FR-084–FR-085, FR-100–FR-104, BR-035, BR-043–BR-046 |
| Reception / search | P-016–P-020 | FR-001–FR-004, FR-078–FR-079 |
| Patient profile | P-021–P-025 | FR-002–FR-007, FR-075–FR-079, FR-091 |
| Visit / consultation payment | P-026–P-039 | FR-008–FR-016, FR-108–FR-114 |
| Queue | P-040–P-047 | FR-017–FR-025, FR-080–FR-081, FR-092–FR-093 |
| Consultation | P-048–P-054 | FR-026–FR-032, FR-091, FR-113 |
| Prescription | P-055–P-064 | FR-033–FR-044, FR-116 |
| Pharmacy fulfilment | P-065–P-076 | FR-045–FR-056, FR-082–FR-083, FR-095, FR-118 |
| Pharmacy billing | P-077–P-084 | FR-057–FR-063, FR-105–FR-115, FR-118 |
| Inventory | P-085–P-093 | FR-051, FR-068, FR-086–FR-090, FR-094–FR-096, FR-115 |
| Owner approvals | P-094–P-096 | FR-015, FR-068, FR-081, FR-097, FR-101–FR-102, FR-105–FR-107, FR-114 |
| Administration | P-097–P-101 | FR-064, FR-069, FR-117, OD-022 |
| Reporting | P-102–P-106 | BRD Section 8, OD-029 |
| Audit / history | P-107–P-110 | FR-070–FR-074, BR-023–BR-024 |
| Product state safety | P-111–P-112 | BR-023–BR-024 plus locked non-destructive controls |
| Printing | P-113–P-116 | FR-041–FR-044, FR-062 |

---

# 8. BRD Coverage Gate

Before PRD lock, verify all locked BRD behavior is represented either:

- directly in PRD feature behavior;
- in acceptance scenarios;
- as a configuration dependency;
- as a technical/compliance dependency;
- or explicitly as Future/Out of V1.

No locked BRD requirement may disappear because it is inconvenient for UI design.

---

# 9. PRD Review Checklist

The PRD can move from DRAFT to LOCKED only when:

- every role has a complete primary journey;
- every approval path has requester/reason/decision/history behavior;
- every material state has current/history presentation behavior;
- multi-Doctor and multi-pharmacy behavior is represented;
- owner/doctor/admin boundaries are reconciled;
- payment and inventory correction paths are represented;
- BRD traceability has no unexplained gap;
- release-blocking negative scenarios are accepted;
- remaining unknowns are configuration/technical/compliance, not hidden business policy.

---

# 10. Change Control

If PRD review identifies a need to change locked business behavior, do not modify the PRD alone.

The process is:

1. identify affected BRD FR/BR/OD;
2. reopen via BRD change control;
3. approve business change;
4. update locked business docs;
5. then update PRD and acceptance mapping.

Product design refinement that does not change business behavior can remain within the PRD workstream.
