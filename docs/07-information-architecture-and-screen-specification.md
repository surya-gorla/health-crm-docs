# Hospital CRM — Information Architecture and Screen Specification

## Document Control

| Field | Value |
| --- | --- |
| Document | Information Architecture and Screen Specification |
| Version | 0.4 |
| Status | DRAFT — PRD companion |
| Date | 2026-09-20 |
| Parent | PRD v0.5 |
| Business source | BRD v1.0 LOCKED |
| Classification | DERIVED PRODUCT DESIGN unless explicitly marked INHERITED |

---

# 1. Purpose

This document converts the PRD into a concrete screen and navigation model.

It defines:

- workspace hierarchy;
- screen inventory;
- role access;
- entry and exit paths;
- required screen context;
- visible information;
- primary and secondary actions;
- loading/empty/error/pending states;
- permission boundaries;
- screen-level acceptance conditions.

This is **not** a URL/router contract and does not prescribe frontend technology.

The screen model may be implemented with pages, drawers, panels, dialogs, or other UI structures as long as the product behavior and access boundaries remain equivalent.

---

# 2. Information Architecture Principles

## IA-01 — Workspaces are authority contexts

Reception, Doctor, Pharmacy, Owner, and Administration are distinct workspaces.

A single-workspace user enters that workspace directly after all applicable authentication/credential gates succeed. A multi-role user uses one identity, selects among permitted workspaces, and can switch without signing into a second account. The switch control remains reachable from the application shell. A workspace whose security prerequisite has not yet been satisfied for the current session is not treated as normally switchable until that prerequisite is completed.

The active workspace/authority must remain visible. Multi-role behavior applies to all valid combinations, not only Owner + Doctor.

Workspace context is scoped: switching does not silently carry an active patient, Visit, queue, pharmacy unit, or protected record into the target workspace. Separate browser tabs/windows may hold different permitted workspace contexts simultaneously.

## IA-02 — Patient/Visit context is explicit

Whenever a screen acts on a patient or Visit, it must show enough identity to prevent acting on the wrong record.

At minimum, active patient context should expose:

- patient name;
- Patient ID;
- Visit ID when the action is Visit-specific.

Clinical screens may also expose age/sex and allergies where appropriate.

## IA-03 — Pharmacy-unit context is explicit

Where more than one pharmacy unit exists, pharmacy screens must show the active pharmacy unit.

Stock, dispensing, billing, and transfer operations must never silently cross unit context.

## IA-04 — Current state is distinct from history

Active/current records and historical/superseded/cancelled records must not look equivalent.

## IA-05 — Exceptional actions are separated from common actions

Waiver, cancellation, payment correction, inventory adjustment, transfer, password reset, and prescription replacement must not be placed where they can be triggered accidentally as part of normal data entry.

## IA-06 — No authority through navigation

A user cannot gain access to a forbidden action by direct navigation, bookmarked link, browser history, stale screen state, or an already-open workspace after permission revocation.

If the user lacks authority, the forbidden module/action is omitted from normal navigation and direct access is denied. If the user has authority but a current record/state temporarily prevents the action, the control may remain visible but disabled with an explanation.

---

# 3. Workspace Navigation Map

## 3.1 Reception workspace

Primary navigation:

1. **Home**
2. **Patients**
3. **Queues**
4. **My Requests**

Core flow:

```text
Reception Home
   |
   +--> Search Patient
   |      |
   |      +--> Existing Patient Profile
   |      |
   |      +--> New Patient Registration
   |
   +--> Create Visit
          |
          +--> Record Payment / Request Waiver
          |
          +--> Assign Doctor
          |
          +--> Doctor Queue
```

## 3.2 Doctor workspace

Primary navigation:

1. **My Queue**
2. **Current Consultation**
3. **Substitution Requests**
4. **Patient History** when entered through an authorized Visit

Core flow:

```text
My Queue
   |
   +--> Call Patient
   |
   +--> Open Consultation
          |
          +--> Review History
          +--> Record Consultation
          +--> Build Prescription
          +--> Finalize / Print
          +--> Complete Consultation
```

## 3.3 Pharmacy workspace

Primary navigation:

1. **Home**
2. **Prescription Lookup**
3. **Inventory**
4. **My Requests**

Core flow:

```text
Prescription Lookup
   |
   +--> Dispensing
          |
          +--> Substitution Request
          +--> Partial Dispense
          +--> Bill
                 |
                 +--> Record External Payment
```

## 3.4 Owner workspace

Primary navigation:

1. **Dashboard**
2. **Approvals**
3. **Inventory**
4. **Financial Status**
5. **Staff**
6. **Reports**
7. **Audit**

The Owner workspace is control-oriented and must not expose unrestricted clinical-note content unless the same account also has Doctor authority and explicitly enters Doctor workspace.

## 3.5 Administration workspace

Primary navigation:

1. **Home**
2. **Staff Accounts**
3. **Medicine Catalogue**
4. **Configuration**
5. **Authorized Reports/Audit**

Administrator navigation must not expose Owner-role lifecycle controls.

---

# 4. Screen ID Convention

Screen IDs are product-design references.

- **SH-xx** — shared/authentication
- **REC-xx** — Reception
- **DOC-xx** — Doctor
- **PHA-xx** — Pharmacy
- **OWN-xx** — Owner
- **ADM-xx** — Administration

A screen ID does not require a one-to-one browser route.

---

# 5. Shared and Authentication Screens

## SH-01 — Login

**Users:** all users attempting account entry  
**Source:** P-008–P-010, P-013, P-015

### Purpose

Authenticate an individual user into the fixed clinic context and route the account into the next required authentication/credential gate.

### Required content

- clinic identity;
- username/login field;
- password field;
- Sign In action;
- Forgot Password action.

### Behavior

1. Invalid credentials -> generic authentication error without identifying which credential failed or confirming arbitrary account existence.
2. Correct credentials for a disabled account -> block product entry with safe account-unavailable guidance.
3. Valid non-Owner normal credential -> enter the permitted workspace or SH-06 workspace selector according to G1.
4. Valid non-Owner Owner-reset credential -> proceed to SH-04 Forced Password Change before any normal workspace.
5. Valid Owner-containing account with enrolled TOTP -> proceed to SH-02 Owner TOTP Verification.
6. Valid Owner-containing account without enrolled TOTP -> proceed to mandatory SH-05 Owner TOTP Enrollment.
7. Forgot Password opens SH-03; it does not imply that Owner accounts use the staff reset process.

### States

- default;
- submitting;
- generic invalid credentials;
- account unavailable/disabled;
- next gate: forced credential change;
- next gate: Owner TOTP;
- next gate: Owner TOTP enrollment;
- temporary service error.

### Acceptance

- password alone never completes login for an Owner-containing account;
- a reset credential never enters normal work before SH-04 succeeds;
- disabled accounts cannot enter the application;
- failure does not expose another user's account details.

---

## SH-02 — Owner TOTP Verification

**Users:** account containing Owner role with TOTP already enrolled  
**Source:** P-009, P-014, AU-001, AU-010

### Purpose

Complete the mandatory Owner second-factor step after password authentication.

### Required content

- TOTP input;
- Verify action;
- recovery-code alternative;
- Back/Sign out action.

### Behavior

- a valid current TOTP completes the second-factor step;
- an unused valid recovery code may be used instead and becomes invalid after use;
- invalid, expired, already-used, or invalidated recovery codes/TOTP values do not complete authentication;
- second-factor values are never shown later in normal history/audit;
- once the fully authenticated session is established, ordinary switching among already-permitted workspaces does not repeat TOTP solely because of the switch.

### States

- awaiting code;
- verifying;
- invalid/expired code;
- invalid/used recovery code;
- recovery code accepted;
- temporary error.

### Acceptance

Successful second-factor verification is required before **any normal application workspace** is available to an account containing Owner authority.

---

## SH-03 — Staff Forgot Password Request

**Users:** unauthenticated staff entry; actionable requests apply only to enabled non-Owner accounts  
**Source:** P-011, AU-006–AU-007

### Purpose

Request Owner-controlled password recovery without providing self-service reset or account-enumeration signals.

### Required content

- staff login identifier;
- Submit Request action;
- non-sensitive generic confirmation;
- static guidance that Owner-account recovery is not performed through the non-Owner staff-reset workflow.

### Behavior

- submission never changes the password directly;
- for an eligible enabled non-Owner account, create a Pending reset request unless one is already Pending;
- repeat submissions while a request is Pending do not create duplicate simultaneously actionable Owner work;
- unknown, disabled, and Owner identifiers receive the same non-enumerating submission response and are not exposed as such to the unauthenticated user;
- Owner accounts are not routed into the non-Owner reset workflow.

### Acceptance

The screen does not reveal whether an arbitrary identifier exists, whether the account is disabled, or whether it contains Owner authority.

---

## SH-04 — Forced Password Change

**Users:** non-Owner staff after valid Owner-reset credential authentication  
**Source:** P-013, AU-009

### Purpose

Replace a temporary/reset credential before normal use.

### Required content

- new password;
- confirm new password;
- Save and Continue;
- sign-out/back-to-login action.

### Behavior

- no normal workspace navigation or protected product content is exposed while this gate is active;
- validation preserves entered values where safe and follows the configured password policy when that policy is defined;
- successful save invalidates the temporary/reset credential and continues through G1's normal single-/multi-workspace routing;
- if replacement is abandoned or fails, the next valid reset-credential login returns to this gate;
- if the account becomes disabled, the flow cannot continue into the product.

### Acceptance

Normal workspace navigation is blocked until replacement succeeds.

---

## SH-05 — Owner TOTP Enrollment & Recovery Codes

**Users:** Owner-containing account requiring initial TOTP enrollment; fully authenticated Owner when regenerating recovery codes  
**Source:** P-009, P-014, AU-004, AU-010

### Purpose

Complete initial mandatory Owner TOTP enrollment and manage recovery-code regeneration without exposing persistent readable secrets.

### Initial enrollment sequence

1. password authentication has already succeeded;
2. show the authenticator secret/QR representation as implemented;
3. require a generated TOTP to verify that enrollment works;
4. after successful factor verification, display the recovery-code set once;
5. require acknowledgement that recovery codes must be stored safely;
6. only then allow normal authenticated continuation.

### Recovery-code regeneration

For an already fully authenticated Owner:

- regeneration is an explicit security-sensitive action;
- warn that the previous recovery-code set will become invalid;
- after successful regeneration, show the replacement set only for that regeneration event;
- do not expose the prior or replacement recovery-code values in normal history/audit.

### Safety

- login is not considered complete for an Owner-containing account until required TOTP enrollment/verification succeeds;
- recovery codes are shown only at enrollment/regeneration and are not presented later as readable stored secrets;
- TOTP secrets/codes, recovery codes, passwords, and reset credentials are not normal audit/history content;
- broader authenticator-factor replacement/device-migration is a security/technical dependency and is not implied by this screen.

---

## SH-06 — Workspace Selector / Switcher

**Users:** multi-role accounts  
**Source:** P-001–P-005, P-096

### Purpose

Choose or switch the effective authority context when more than one workspace is permitted.

A single-workspace account bypasses this choice after all applicable authentication/credential gates succeed. A multi-workspace account sees the selector on entry and retains an always-reachable switch control in the application shell.

The selector/switcher exposes only authority that is currently safe to enter. Newly granted Owner authority that has not yet satisfied the Owner second-factor gate remains gated by the authentication flow rather than becoming an ordinary one-click workspace switch.

### Example

Owner + Doctor sees:

- Doctor Workspace
- Owner Workspace

The same pattern applies to other valid combinations such as Reception + Pharmacist.

### Switching rules

- material unsaved work must trigger a leave/switch warning before it can be discarded;
- switching does not silently carry an active patient, Visit, queue, pharmacy unit, or protected record into the target workspace;
- a legitimate cross-workspace transition enters the target workspace/authority before opening protected content;
- separate tabs/windows may retain different permitted workspace contexts independently;
- if the current role is revoked, the next protected navigation/action or permission refresh denies further access and routes the user to a permitted workspace;
- if the entire account becomes disabled, authentication rule P-015 applies and protected account use returns to the sign-in boundary rather than merely switching workspaces.

### Acceptance

The active workspace remains visible after switching, the human identity remains the same, and material actions are attributable to the effective authority actually used.

---

# 6. Reception Screens

## REC-01 — Reception Home

**Users:** Reception  
**Source:** P-016–P-020, P-040–P-045

### Purpose

Serve as the high-frequency operational starting point.

### Primary content

- prominent patient search;
- New Patient action;
- compact Doctor queue summary;
- recent Doctor call notifications;
- pending Reception-originated requests;
- quick access to active Visits.

### Primary actions

- Search Patient;
- Register New Patient;
- Open Queues;
- open patient/Visit;
- acknowledge Doctor call;
- open My Requests.

### States

- normal operating state;
- no active queues;
- no search result yet;
- temporary data error.

### Acceptance

Patient search is visually dominant over New Patient to reduce accidental duplicate registration.

---

## REC-02 — Patient Search and Identity Confirmation

**Users:** Reception  
**Source:** P-016–P-020

### Purpose

Find and explicitly confirm the correct existing patient before creating another identity.

### Search inputs

- Patient ID;
- phone;
- name.

### Result row

Each candidate remains a separate patient row and should expose enough identity to distinguish records without exposing unnecessary clinical detail:

- patient name;
- Patient ID;
- phone;
- DOB / derived age context;
- compact address cue where useful;
- Possible Duplicate marker;
- explicit indication of exact Patient ID match where applicable.

An exact Patient ID match is visually prioritized but still requires **Select Patient** before patient context changes.

### Identity-confirmation detail

Reception may deliberately open limited prior-Visit identity context permitted by the BRD, such as visit dates and other identity-confirmation cues needed to help the patient recognize the record.

This view must not expose unrestricted diagnosis, clinical notes, or prescription content.

### Actions

- Select Patient;
- View Patient;
- Review limited identity-confirmation context;
- Continue to New Patient when no candidate is confidently confirmed.

### Safety

- no auto-select solely from exact/similarity confidence;
- no auto-merge;
- shared phone number is not presented as unique proof;
- same-phone patients are not collapsed into one result;
- loading/error state is distinct from “no matching patient.”

### Empty state

Only after search completes successfully: “No matching patient confirmed” with a deliberate **Register New Patient** action.

---

## REC-03 — New Patient Registration

**Users:** Reception  
**Source:** P-016, P-020–P-022

### Required fields

- full name;
- phone;
- DOB;
- gender;
- address;
- email.

### Optional fields

- emergency contact;
- blood group;
- known allergies;
- guardian/parent details;
- Government ID.

### Derived display

Age is computed from DOB and not separately editable.

### Actions

- Register Patient;
- Cancel/return to search;
- when final duplicate review surfaces candidates: **Select Existing Patient** or **Create New as Possible Duplicate**.

### Validation

- required-field validation is field-specific;
- phone is not globally unique;
- email is required but is not an identity key;
- Government ID is not required;
- future/impossible DOB is rejected;
- registration submit performs a current duplicate-candidate check rather than trusting only an earlier manual search;
- if current candidates require review, effective Patient creation pauses until Reception makes an explicit identity decision.

### Pending/unknown outcome safety

- disable duplicate final submit while Patient creation is in progress;
- do not show a generated Patient ID until effective creation is confirmed;
- if the client cannot determine whether creation succeeded, refresh/check effective state before allowing another create attempt.

### Possible Duplicate path

When Reception deliberately creates a new record after unresolved candidate review:

- apply the visible Possible Duplicate marker automatically;
- retain candidate Patient IDs plus creating actor/time as safe provenance;
- do not block normal Patient/Visit use;
- do not auto-merge or silently clear the marker.

### Success

Display the newly created permanent Patient ID clearly/copy-friendly and proceed to patient profile / Visit creation.

---

## REC-04 — Reception Patient Profile

**Users:** Reception  
**Source:** P-021, P-023–P-025

### Purpose

Operate on patient identity without granting longitudinal clinical access.

### Visible sections

- permanent Patient ID and demographics;
- recorded allergies/intake safety context already permitted to Reception;
- Possible Duplicate marker and safe provenance where applicable;
- active Visit operational summary;
- prior Visit identity/operational summaries sufficient for matching;
- physical-file Patient ID reference.

### Actions

- Create Visit;
- submit Demographic Correction Request;
- view operational Visit state;
- copy Patient ID for physical-file association.

### Restricted

Do not expose unrestricted diagnosis, consultation notes, prescriptions, or Doctor longitudinal clinical history.

Patient ID is read-only and is never offered as an editable demographic field.

Missing physical file does not disable Create Visit or other digital patient workflow actions.

---

## REC-05 — Create Visit and Consultation Payment

**Users:** Reception  
**Source:** P-026–P-033

### Purpose

Create the Visit, record external payment state, assign Doctor, and enter queue.

### Required context

- Patient ID;
- new Visit ID;
- Doctor selection;
- configured consultation fee;
- payment state.

### Payment controls

Direct payment-method buttons:

- UPI
- Cash
- Card
- Other

If Other -> required description.

Optional external reference.

### Primary outcomes

- **Mark Paid & Add to Queue** when Doctor is selected;
- **Keep Unpaid**;
- **Request Waiver**.

### Safety

- method selection alone does not mark Paid;
- explicit final Paid confirmation is required;
- Unpaid cannot enter queue;
- partial payment option does not exist.

---

## REC-06 — Waiver Request

**Users:** Reception  
**Source:** P-034–P-037

### Required fields

- Patient/Visit context;
- consultation amount;
- mandatory specific reason.

### Action

Submit to Owner.

### After submission

Display Pending Owner Approval.

The Visit remains Unpaid and queue-blocked until approved.

---

## REC-07 — Reception Queue Board

**Users:** Reception  
**Source:** P-040–P-045

### Purpose

Coordinate doctor-specific queues.

### Layout

Support one column/section per Doctor or a Doctor filter while preserving clearly separated queues.

### Queue row

- position;
- patient name;
- Patient ID;
- Visit ID;
- status;
- payment eligibility;
- waiting duration;
- current Doctor.

### Actions

- mark Unresponded for Called Visit;
- reassign Doctor;
- open Visit;
- react to Doctor Call.

### Reassignment

Requires explicit destination Doctor and confirmation.

### Urgent case

No software priority/reorder action is provided.

---

## REC-08 — Demographic Correction Request

**Users:** Reception  
**Source:** P-024

### Required content

For each proposed demographic field change:

- field being corrected;
- current effective value;
- proposed new value;
- Doctor reviewer/routing context.

Optional:
- reason/context; the locked BRD does not require a mandatory reason for demographic correction.

Patient ID is never available as a correction field.

### Doctor routing

- active Visit + assigned Doctor -> route to that Doctor;
- active Visit + no assigned Doctor -> Doctor must be selected/assigned before submission;
- no active Visit -> select an authorized Doctor reviewer without creating a Visit solely for the correction.

### Behavior

- Reception does not directly change the patient record;
- effective value remains unchanged while Pending;
- if the active Visit is reassigned before decision, the pending request follows the current assigned Doctor;
- Visit completion does not silently discard a submitted request;
- if the captured current value changes before decision, the proposal is stale and cannot be applied without refreshed review.

### Result

One of:

- Pending;
- Approved — proposed value becomes effective with old/new/requester/Doctor/time history;
- Rejected — effective value remains unchanged and decision history is retained;
- Stale — current value changed before decision and refreshed review is required.

---

## REC-09 — Payment Correction Request

**Users:** Reception  
**Source:** P-038–P-039

### Required content

- current payment record;
- proposed corrected state;
- mandatory reason;
- optional payment reference change where relevant.

### Behavior

Submission does not change effective payment state.

Owner approval is required.

---

# 7. Doctor Screens

## DOC-01 — Doctor Home / My Queue

**Users:** Doctor  
**Source:** P-040–P-047

### Purpose

Make the next clinical action immediately obvious.

### Content

- Doctor identity;
- ordered Waiting/Called queue;
- current With Doctor patient if any;
- pending substitution requests;
- queue counts/status.

### Primary actions

- Call Patient;
- Open Consultation;
- view authorized patient history;
- open substitution request.

### Restricted

No Owner-only approval actions appear merely because the user is a Doctor.

---

## DOC-02 — Active Consultation Workspace

**Users:** Doctor assigned to current Visit  
**Source:** P-048–P-054

### Persistent patient header

- patient name;
- Patient ID;
- Visit ID;
- DOB/age;
- gender;
- known allergies.

### Main sections

1. Current Visit / chief complaint
2. assessment / diagnosis
3. optional history/symptoms
4. optional examination findings
5. optional clinical notes
6. optional advice/follow-up
7. Prescription action
8. longitudinal-history access

### Primary actions

- Save current consultation work according to implementation;
- Build Prescription;
- Complete Consultation;
- Request Cancellation while Visit is eligible.

### Safety

Required fields must be satisfied before consultation completion.

Owner controls are not embedded into the clinical form.

---

## DOC-03 — Longitudinal Patient History

**Users:** Doctor through authorized current Visit  
**Source:** P-049

### Purpose

Review previous care without confusing historical content with current editable content.

### Timeline/card content

- Visit date;
- Doctor;
- chief complaint;
- diagnosis/assessment;
- prescription summary;
- amendment indicator;
- superseded-prescription indicator.

### Actions

- open historical Visit read-only;
- inspect amendment/superseded history.

### Acceptance

Historical records do not become editable merely because they are opened.

---

## DOC-04 — Prescription Builder

**Users:** Doctor  
**Source:** P-055–P-062

### Medicine-row content

- medicine/display name;
- strength;
- dosage form;
- manufacturer context;
- optional generic/molecule context;
- clinic-wide availability;
- per-pharmacy availability when relevant;
- dose amount;
- frequency;
- duration;
- optional timing/food/instruction;
- quantity.

### Actions

- Add Medicine;
- Remove unfinalized row;
- Finalize Prescription.

### Quantity

Auto-calculate only when deterministic; otherwise require Doctor entry.

### Safety

Unavailable medicine remains prescribable.

---

## DOC-05 — Prescription Review / Print

**Users:** Doctor  
**Source:** P-059–P-064, P-113–P-114

### Purpose

Review the exact finalized output before printing/reprinting.

### Content

- patient/Visit;
- medicines/instructions;
- ** marker for Out of Stock/Not Stocked at finalization;
- explanatory legend;
- finalization status/version.

### Actions

- Print A4;
- Reprint;
- Create Replacement if correction is needed.

### Restricted

No Edit Finalized Prescription action.

---

## DOC-06 — Clinical Amendment

**Users:** Doctor  
**Source:** P-053

### Content

- current effective clinical record;
- original/historical revision reference;
- amendment fields;
- mandatory amendment reason.

### Outcome

Create new effective revision and retain old content.

---

## DOC-07 — Prescription Replacement

**Users:** Doctor  
**Source:** P-061–P-062

### Content

- current finalized prescription;
- any prior dispensing quantity;
- replacement prescription editor;
- mandatory correction reason.

### Warning

Clearly state that prior dispensing history will remain and cannot be erased.

### Outcome

Old prescription -> Superseded. New prescription -> active.

---

## DOC-08 — Substitution Request Detail

**Users:** Doctor  
**Source:** P-072

### Content

- patient/Visit;
- prescribing Doctor context;
- original medicine;
- proposed substitute;
- pharmacist/requester;
- request reason;
- relevant availability information.

### Actions

- Approve;
- Reject.

### Acceptance

No substitute may be dispensed before approval.

---

## DOC-09 — Visit Cancellation Request

**Users:** Doctor  
**Source:** P-046–P-047

### Content

- Visit state;
- existing clinical/prescription/dispensing/payment-history warning;
- mandatory specific reason.

### Action

Submit to Owner.

### Safety

Completed Visit does not offer this flow.

---

# 8. Pharmacy Screens

## PHA-01 — Pharmacy Home

**Users:** Pharmacist  
**Source:** P-065–P-076

### Content

- active pharmacy unit;
- Patient ID lookup;
- pending dispensing work;
- pending substitution requests;
- low-stock/near-expiry alerts;
- My Requests status.

### Acceptance

In a multi-pharmacy clinic, unit context is always visible.

---

## PHA-02 — Prescription Lookup

**Users:** Pharmacist  
**Source:** P-065–P-066

### Input

Patient ID.

### Results

Show relevant current finalized prescription/Visit.

### Restricted

No general diagnosis/full clinical-note access.

---

## PHA-03 — Dispensing Workspace

**Users:** Pharmacist  
**Source:** P-067–P-076

### Patient header

- patient name;
- Patient ID;
- Visit ID;
- allergies;
- current prescription identity/version.

### Medicine table

Per item:

- prescribed medicine;
- prescribed quantity;
- previously dispensed quantity;
- remaining allowable quantity;
- current pharmacy-unit availability;
- quantity to dispense;
- unsupplied remainder;
- selling price/billing context.

### Actions

- Dispense;
- Request Substitution;
- Continue to Bill.

### Validation

- cannot exceed remaining prescribed quantity;
- expired stock cannot be selected for dispensing;
- stock cannot go below available quantity;
- actual dispensed quantity drives stock deduction.

---

## PHA-04 — Pharmacy Bill and Payment

**Users:** Pharmacist  
**Source:** P-077–P-080

### Content

- only supplied items;
- quantities;
- prices;
- total;
- payment state;
- payment method;
- optional reference.

### Actions

- Record Paid;
- leave Unpaid;
- print configured A4 summary if needed;
- Request Void after creation.

### Restricted

No partial payment and no refund action.

---

## PHA-05 — Bill Void Request

**Users:** Pharmacist  
**Source:** P-081–P-084

### Content

- bill;
- payment state;
- dispensing summary;
- warning that bill void does not restore stock;
- mandatory specific reason.

### Outcome

Pending Owner approval. Bill remains active until approved.

---

## PHA-06 — Substitution Request

**Users:** Pharmacist  
**Source:** P-072

### Content

- original prescribed medicine;
- proposed substitute;
- availability;
- request reason.

### Outcome

Pending Doctor approval.

The proposed substitute remains non-dispensable until approved.

---

## PHA-07 — Inventory Overview

**Users:** Pharmacist within permitted pharmacy scope; Owner; authorized Admin  
**Source:** P-085–P-091

### Content

- active pharmacy unit;
- medicine;
- current available stock;
- base/package-unit representation;
- low-stock state;
- nearest expiry;
- Out of Stock state.

### Filters

- search medicine;
- low stock;
- out of stock;
- near expiry;
- expired.

### Actions for Pharmacist

- open Medicine Inventory Detail;
- Request Inventory Change;
- Request Transfer.

No direct unapproved manual quantity edit.

---

## PHA-08 — Medicine Inventory Detail

**Users:** Pharmacist within scope; Owner; authorized Admin  
**Source:** P-085–P-091

### Content

- medicine identity;
- base unit and package conversions;
- batches;
- expiry;
- manufacturer;
- purchase/selling price;
- current quantities;
- recent stock movements.

### Actions

Role-dependent:

- Pharmacist -> submit change request;
- Owner -> review control history / enter Owner-authorized actions where applicable;
- Admin -> permitted configuration only.

---

## PHA-09 — Inventory Change Request

**Users:** Pharmacist  
**Source:** P-088–P-091

### Request categories

- stock addition;
- damage;
- loss;
- correction;
- permitted price change;
- expired-stock disposition/adjustment where appropriate.

### Required fields

- medicine/batch where relevant;
- proposed change;
- quantity or price value;
- mandatory reason.

### Outcome

Pending Owner approval. Stock does not change while pending.

---

## PHA-10 — Pharmacy Stock Transfer Request

**Users:** Pharmacist  
**Source:** P-092–P-093

### Required fields

- source pharmacy;
- destination pharmacy;
- medicine/batch where relevant;
- quantity;
- mandatory reason.

### Validation

- source and destination must differ;
- requested quantity cannot exceed transferable source availability;
- request itself does not alter stock.

### Outcome

Pending Owner approval.

---

# 9. Owner Screens

## OWN-01 — Owner Dashboard

**Users:** Owner  
**Source:** P-094–P-096, P-102

### Purpose

Surface clinic-wide control needs without requiring the Owner to inspect every operational screen.

### Recommended sections

- pending approvals count by type;
- Doctor queue/clinic activity summary;
- consultation/pharmacy financial-status summary;
- inventory risk summary;
- low-stock/out-of-stock/expiring alerts;
- recent high-risk audit activity.

### Clinical boundary

Do not expose unrestricted diagnosis/clinical notes unless the account enters Doctor workspace using Doctor role.

---

## OWN-02 — Approval Center

**Users:** Owner  
**Source:** P-094

### Approval types

- consultation waiver;
- consultation/Visit cancellation;
- pharmacy bill void;
- payment correction;
- inventory adjustment;
- stock transfer;
- staff password reset request.

### List item

- type;
- requester;
- affected entity;
- specific reason summary;
- requested time;
- current status.

### Filters

- Pending;
- Approved;
- Rejected;
- request type;
- requester;
- date.

### Primary action

Open Approval Detail.

---

## OWN-03 — Approval Detail

**Users:** Owner  
**Source:** P-094–P-096

### Common layout

1. request type;
2. requester;
3. affected record;
4. current state;
5. proposed change/action;
6. exact reason;
7. risk/impact context;
8. Approve / Reject.

### Type-specific context

**Waiver:** amount, payment state, Visit.  
**Visit cancellation:** current Visit state and existing clinical/financial/dispensing history.  
**Bill void:** bill/payment and explicit “stock will not be restored” warning.  
**Payment correction:** original vs proposed payment state.  
**Inventory adjustment:** stock before, proposed delta, projected stock after.  
**Transfer:** source, destination, quantity.  
**Password reset:** staff identity, account status.

### Acceptance

Owner cannot approve an already-resolved request as if it were still Pending.

---

## OWN-04 — Owner Inventory Control

**Users:** Owner  
**Source:** P-085–P-093

### Views

- clinic consolidated;
- pharmacy-unit view;
- stock movement history;
- damage/loss/correction history;
- pending inventory approvals;
- transfers;
- expiry risk.

### Acceptance

Consolidated totals never replace unit-level attribution.

---

## OWN-05 — Financial Status Overview

**Users:** Owner  
**Source:** P-102

### Content

- consultation revenue;
- pharmacy revenue;
- total recorded revenue;
- payment method breakdown where data exists;
- Waived count/value context;
- cancellations/voids;
- payment corrections.

### Important language

This is recorded financial status, not bank settlement verification.

---

## OWN-06 — Staff and Access Oversight

**Users:** Owner  
**Source:** P-097–P-100

### Content

- staff accounts;
- roles;
- enabled/disabled state;
- pending password reset requests;
- recent role/account changes.

### Actions

- create/disable permitted staff;
- assign/revoke roles;
- act on password reset;
- Owner-level lifecycle control.

---

## OWN-07 — Audit Activity

**Users:** Owner  
**Source:** P-107–P-110

### Filters

- date;
- actor;
- event type;
- affected entity;
- pharmacy unit;
- approval type.

### Event content

- actor;
- time;
- action;
- affected entity;
- reason where applicable;
- prior/resulting state where applicable.

### Clinical boundary

Audit metadata does not automatically reveal unrestricted clinical-note content.

---

## OWN-08 — Reports

**Users:** Owner  
**Source:** P-102

### Report set

- patients seen/day;
- average waiting time;
- consultation revenue;
- pharmacy revenue;
- total revenue;
- medicine sales;
- current stock;
- low/out-of-stock;
- expiring medicines;
- most prescribed;
- waivers;
- cancellations;
- returning patients;
- audit activity.

Exact chart style remains design implementation.

---

## OWN-09 — Staff Password Reset

**Users:** Owner  
**Source:** P-011–P-013, AU-007–AU-009

### Content

- staff identity;
- role(s);
- account enabled/disabled status;
- request time;
- request state;
- Set Temporary Credential action for a current Pending request.

### Behavior

- only one reset request for the staff account is simultaneously actionable as Pending;
- Owner sets/replaces the temporary/reset credential but never sees/retrieves the old password;
- successful reset resolves the request;
- a stale/resolved copy cannot perform the reset again as though still Pending;
- password reset does not change assigned roles or enabled/disabled account state;
- if the account is disabled, the UI must not imply that setting a reset credential restores access;
- credential values are not written into normal audit/history.

This request type is resolved by the Owner reset action; it is not forced into a generic Approve/Reject pattern unless a later locked product decision explicitly changes that behavior.

---

# 10. Administration Screens

## ADM-01 — Administration Home

**Users:** Administrator  
**Source:** P-097–P-101

### Content

- staff account summary;
- medicine/configuration shortcuts;
- authorized operational notices.

No Owner approval center.

---

## ADM-02 — Staff Accounts

**Users:** Administrator; Owner  
**Source:** P-097–P-100

### Content

- staff identity;
- roles;
- enabled/disabled state;
- last relevant account-status change.

### Admin actions

- create non-Owner staff;
- assign non-Owner roles;
- disable/re-enable non-Owner staff.

### Restricted

Admin cannot grant/revoke Owner or disable Owner.

---

## ADM-03 — Staff Account Create/Edit

**Users:** Administrator within authority; Owner  
**Source:** P-097–P-100

### Fields

- staff identity;
- login;
- role(s);
- enabled/disabled state.

### Validation

If acting user is Admin, Owner role is unavailable.

Historical staff is disabled, not deleted.

---

## ADM-04 — Medicine Catalogue

**Users:** authorized Admin/Owner  
**Source:** P-101, P-055

### Content

- display name;
- strength;
- dosage form;
- manufacturer;
- optional generic/molecule;
- inventory unit configuration.

### Purpose

Maintain the clinic medicine identity used by Doctor and Pharmacy workflows.

---

## ADM-05 — Inventory Configuration

**Users:** authorized Admin/Owner  
**Source:** P-085–P-091, P-101

### Content

- base unit;
- package conversions;
- low-stock threshold;
- near-expiry threshold;
- permitted medicine inventory metadata.

### Safety

Configuration changes do not become unlogged manual stock movements.

---

## ADM-06 — Clinic Configuration

**Users:** authorized Owner/Admin according to permission boundary  
**Source:** P-028, P-101

### Configuration

- consultation fee values;
- doctor/service-specific fee values if used;
- pharmacy units;
- optional A4 output configuration;
- applicable price/tax configuration where supplied.

### Important

Configuration controls values; it does not redefine locked workflow behavior.

---

# 11. Cross-Workspace Screen Contracts

## 11.1 Patient context header

Where patient context exists, use a consistent identity region.

Minimum:

- patient name;
- Patient ID;
- Visit ID for Visit-specific work.

Clinical/dispensing screens may add:

- DOB/age;
- gender;
- allergies.

## 11.2 Approval status presentation

All approval-driven records expose one of:

- Pending;
- Approved;
- Rejected.

The source record remains unchanged while Pending unless the locked workflow explicitly says otherwise.

## 11.3 History presentation

Superseded, amended, corrected, cancelled, and voided records remain viewable to authorized roles.

Historical state should never be mistaken for a currently actionable state.

## 11.4 Multi-role and workspace context

When a user has multiple roles:

- the current workspace/authority remains visible;
- one human identity is used across all permitted workspaces;
- each browser tab/window retains its own explicit workspace context;
- Owner-only actions are performed in Owner authority context;
- Doctor-only clinical actions are performed in Doctor authority context;
- equivalent separation applies to every other valid role combination;
- workspace switching does not silently carry protected record context into the target workspace;
- cross-workspace notifications may expose an attention count but not protected detail/action until the target authority context is entered;
- audit history records the human actor and effective authority/workspace used;
- the same human may perform separate workflow actions through different legitimately assigned roles, with each action separately attributed.

---

# 12. Global Screen-State Requirements

Every data-backed screen must deliberately handle the states that are meaningful to that screen.

## 12.1 Loading

Show that content is being retrieved; do not present stale-looking empty content as final.

## 12.2 Empty

Explain what is empty and the correct next action where the user has authority.

Examples:

- no patients found;
- Doctor queue empty;
- no pending approvals;
- no inventory alerts.

## 12.3 Error

Show a clear failure and a safe retry/recovery action.

Do not claim that an operation succeeded unless the effective state is confirmed.

## 12.4 Permission denied

Do not expose sensitive content behind a disabled control.

If the user reaches a forbidden screen directly, show a safe access-denied state and route back to permitted navigation.

## 12.5 Pending approval

Pending items must clearly state:

- what is pending;
- who must decide;
- whether the underlying active record has changed.

## 12.6 Stale state

If a request/record has changed since the user loaded the screen, the UI must not silently apply the old action.

Refresh/review is required before the user retries.

---

# 13. Screen Inventory Summary

| Screen | Name | Primary Role |
| --- | --- | --- |
| SH-01 | Login | All |
| SH-02 | Owner TOTP Verification | Owner |
| SH-03 | Staff Forgot Password Request | Staff |
| SH-04 | Forced Password Change | Staff |
| SH-05 | Owner TOTP Enrollment / Recovery Codes | Owner |
| SH-06 | Workspace Selector | Multi-role |
| REC-01 | Reception Home | Reception |
| REC-02 | Patient Search and Identity Confirmation | Reception |
| REC-03 | New Patient Registration | Reception |
| REC-04 | Reception Patient Profile | Reception |
| REC-05 | Create Visit and Consultation Payment | Reception |
| REC-06 | Waiver Request | Reception |
| REC-07 | Reception Queue Board | Reception |
| REC-08 | Demographic Correction Request | Reception |
| REC-09 | Payment Correction Request | Reception |
| DOC-01 | Doctor Home / My Queue | Doctor |
| DOC-02 | Active Consultation Workspace | Doctor |
| DOC-03 | Longitudinal Patient History | Doctor |
| DOC-04 | Prescription Builder | Doctor |
| DOC-05 | Prescription Review / Print | Doctor |
| DOC-06 | Clinical Amendment | Doctor |
| DOC-07 | Prescription Replacement | Doctor |
| DOC-08 | Substitution Request Detail | Doctor |
| DOC-09 | Visit Cancellation Request | Doctor |
| PHA-01 | Pharmacy Home | Pharmacist |
| PHA-02 | Prescription Lookup | Pharmacist |
| PHA-03 | Dispensing Workspace | Pharmacist |
| PHA-04 | Pharmacy Bill and Payment | Pharmacist |
| PHA-05 | Bill Void Request | Pharmacist |
| PHA-06 | Substitution Request | Pharmacist |
| PHA-07 | Inventory Overview | Pharmacist |
| PHA-08 | Medicine Inventory Detail | Pharmacist |
| PHA-09 | Inventory Change Request | Pharmacist |
| PHA-10 | Pharmacy Stock Transfer Request | Pharmacist |
| OWN-01 | Owner Dashboard | Owner |
| OWN-02 | Approval Center | Owner |
| OWN-03 | Approval Detail | Owner |
| OWN-04 | Owner Inventory Control | Owner |
| OWN-05 | Financial Status Overview | Owner |
| OWN-06 | Staff and Access Oversight | Owner |
| OWN-07 | Audit Activity | Owner |
| OWN-08 | Reports | Owner |
| OWN-09 | Staff Password Reset | Owner |
| ADM-01 | Administration Home | Admin |
| ADM-02 | Staff Accounts | Admin/Owner |
| ADM-03 | Staff Account Create/Edit | Admin/Owner |
| ADM-04 | Medicine Catalogue | Admin/Owner |
| ADM-05 | Inventory Configuration | Admin/Owner |
| ADM-06 | Clinic Configuration | Admin/Owner |

Total screen contracts in this baseline: **49**.

---

# 14. Screen-Level Release Gate

The screen model is not ready for design/implementation sign-off if:

- a locked role action has no reachable screen;
- a forbidden action appears in an unauthorized workspace;
- a material request has no Pending/Approved/Rejected representation;
- a historical record is presented as current;
- multi-pharmacy unit context can become ambiguous;
- Unpaid Visit can reach queue action;
- finalized prescription exposes in-place edit;
- bill void implies inventory restoration;
- Admin can access Owner lifecycle controls;
- Owner-only screen exposes unrestricted clinical content;
- error/empty/pending states are undefined for a critical workflow.

---

# 15. Open Product-Design Work After This Document

This screen contract intentionally does not lock visual styling.

Next product-design layers include:

- component-level interaction patterns;
- field/form behavior;
- table/list behavior;
- detailed validation messages;
- keyboard and focus behavior;
- responsive layout rules;
- final visual hierarchy;
- exact information density;
- design-system tokens.

Those details are specified progressively without changing locked BRD behavior.
