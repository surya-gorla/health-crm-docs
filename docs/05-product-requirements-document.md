# Hospital CRM — Product Requirements Document

## Document Control

| Field | Value |
| --- | --- |
| Document | Product Requirements Document |
| Product | Hospital CRM for clinic operations |
| Version | 0.12 |
| Status | DRAFT — derived from locked BRD v1.0 |
| Date | 2026-09-20 |
| Source baseline | BRD v1.0 LOCKED |
| Working review | PR #2 — long-lived PRD refinement review |
| Product stage | V1 product definition — group-by-group refinement in progress |

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

The application exposes role-based workspaces.

A user sees only workspaces granted by their assigned roles.

Recommended V1 workspaces:

1. **Reception**
2. **Doctor**
3. **Pharmacy**
4. **Owner**
5. **Administration**

A user with exactly one permitted workspace enters that workspace directly after successful completion of all applicable authentication/credential gates. A user with more than one permitted workspace is presented with a workspace choice and must also have an always-reachable workspace-switch control in the application shell.

Multi-role behavior applies to every valid role combination, not only Owner + Doctor. One human uses one account; roles add permitted authority without creating separate identities.

Workspace context is deliberately scoped. Switching workspaces does not silently carry an active patient, Visit, Doctor queue, pharmacy unit, or protected record into the target workspace. A legitimate cross-workspace transition must explicitly enter the target workspace/authority before protected content or actions are exposed.

If material unsaved work exists, workspace switching must warn before that work can be discarded.

Separate browser tabs/windows may operate in different permitted workspaces at the same time. Each tab/window retains its own explicit workspace context; changing workspace in one must not silently change the authority context of another.

If a role is revoked while a workspace is already open, stale access must not remain usable. On the next protected navigation/action or permission refresh, access is re-evaluated and the user is returned to a permitted workspace if necessary. The exact session/permission-refresh mechanism is a technical design decision.

Cross-workspace attention indicators may show that another permitted workspace has pending work, but protected details/actions are opened only after entering the correct workspace/authority context.

### P-001 — Workspace identity

The active workspace/authority context must always be visible while the user is inside the application, including when different browser tabs/windows are using different permitted workspaces.

### P-002 — Role-safe navigation

Functionality for which the current account lacks authority must not be exposed as normal navigation/action choices, and direct navigation must also be denied. This is distinct from an action the user is authorized to perform but which is temporarily unavailable because of current record/state conditions.

### P-003 — Multi-role switching

A multi-role account uses one identity and may switch without a second username/password login among assigned workspaces whose applicable authentication gates are already satisfied for the current session. Single-workspace accounts enter directly after applicable authentication/credential gates succeed; multi-workspace accounts receive a workspace selector plus an always-reachable switch control.

Ordinary workspace switching does not repeat Owner TOTP merely because the workspace changes. If security-sensitive Owner authority is newly granted to a session that had not satisfied Owner authentication requirements, Owner-capable access remains gated by P-009 before that newly available authority can be used.

### P-004 — Workspace-scoped context preservation

Switching workspaces must not silently change or carry forward the patient, Visit, Doctor queue, pharmacy unit, or protected record being acted on. Material unsaved work must be protected by a leave/switch warning. Explicit cross-workspace transitions may pass a record reference only after the target workspace/authority is entered.

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

The shell distinguishes lack of authority from temporary state unavailability:

- if the user lacks authority, the module/action is omitted from normal navigation/action choices and direct access is denied;
- if the user has authority but the current record/state makes the action temporarily invalid, the action may remain visible but disabled with a meaningful explanation.

The shell must also provide an always-reachable workspace switcher for multi-role users and may show cross-workspace attention counts without exposing protected detail outside the correct authority context.

### P-006 — Clear status language

System status labels must use the locked vocabulary, including Paid, Unpaid, Waived, Waiting, Called, Unresponded, With Doctor, Consultation Completed, Sent to Pharmacy, Completed, Cancelled/Voided, In Stock, Out of Stock, Not Stocked, and Superseded.

### P-007 — Explicit final actions

Financial, cancellation, approval, prescription-finalization, and inventory-adjustment actions require explicit final confirmation rather than triggering merely by selecting an option.

---

# 10. Authentication and Account Entry

Source: FR-084–FR-085, FR-100–FR-104; BR-035, BR-043–BR-046; OD-021–OD-022.

The locked business policy is intentionally simple: one individual account per person, password authentication for everyone, mandatory TOTP for every account containing Owner authority, no required 2FA for accounts without Owner authority, and Owner-controlled password recovery for non-Owner staff.

The following sequencing and safety rules are **DERIVED PRODUCT DESIGN** and do not alter that business policy.

### P-008 — Individual account login

Every user signs in through an individual username/login and password in the fixed clinic context. V1 does not require a clinic/tenant selector.

Invalid username/password outcomes must use a generic authentication response that does not identify which credential was incorrect or confirm arbitrary account existence.

### P-009 — Owner TOTP

Any account containing Owner authority must satisfy the Owner second-factor requirement after password authentication and **before any normal application workspace is entered**, regardless of which workspace the user intends to use.

- If TOTP is already enrolled, the user completes Google Authenticator-compatible TOTP or an unused recovery-code alternative.
- If TOTP is not yet enrolled, successful password authentication routes to mandatory TOTP enrollment before normal workspace entry.
- Choosing Doctor or another non-Owner workspace cannot bypass the Owner 2FA requirement.
- If Owner authority is newly granted during an already-authenticated non-Owner session, Owner-capable workspace/actions remain unavailable until the Owner second-factor requirement is satisfied for that session. Exact step-up/session mechanics are technical design.

After an Owner-containing session has fully satisfied authentication, ordinary switching among already-permitted workspaces does not require another login or repeated TOTP solely because of the switch.

### P-010 — Non-Owner login

Doctor-only, Reception, Pharmacist, Administrator-only, and other valid multi-role accounts that do **not** contain Owner authority are not required to complete 2FA in V1.

After valid normal credentials, the user proceeds through the G1 single-/multi-workspace entry rules unless a mandatory credential-replacement gate applies.

### P-011 — Staff Forgot Password

The V1 in-app Forgot Password flow is for **enabled non-Owner staff accounts** and is not self-service password reset.

- submission uses the staff login identifier;
- the unauthenticated response is non-enumerating and does not confirm whether the identifier exists, is disabled, or has Owner authority;
- for an eligible non-Owner account, the system creates or retains one Pending Owner-visible reset request;
- repeat submissions while one request is Pending must not create multiple simultaneously actionable Owner requests;
- Owner accounts are not routed into this staff-reset workflow; Owner password/account recovery remains the controlled Owner recovery path.

### P-012 — Owner reset

From a Pending eligible staff-reset request, Owner authority may set/replace a temporary/reset credential without viewing or retrieving the existing password.

A successful reset:

- replaces the current sign-in credential;
- resolves that reset request so the same stale request cannot be actioned again as Pending;
- does not change assigned roles;
- does not enable a disabled account;
- records safe audit metadata without recording credential values.

The password-reset workflow is an Owner action to set a temporary credential; V1 does not invent a generic Approve/Reject decision for this request type.

### P-013 — Forced credential replacement

A valid Owner-reset credential may authenticate the staff member into the credential-replacement gate, but it does **not** grant normal workspace access.

The staff member must successfully enter and confirm a replacement password before normal product navigation/content becomes available.

After successful replacement:

- the temporary/reset credential is no longer valid;
- the user proceeds through the normal G1 workspace-entry rules;
- if replacement is abandoned or fails, the forced-change gate remains on the next valid reset-credential login.

If the account becomes disabled before replacement completes, the user cannot continue into the product.

Password-strength specifics remain a security/technical policy dependency.

### P-014 — Owner TOTP enrollment and recovery codes

For an Owner-containing account without an enrolled TOTP factor:

1. password authentication succeeds;
2. the product enters mandatory TOTP enrollment;
3. the user receives the authenticator secret/QR representation;
4. the user proves enrollment by entering a valid generated TOTP;
5. only after successful factor verification are recovery codes shown;
6. recovery codes are shown only for that enrollment/regeneration event and must be acknowledged before normal continuation.

A valid unused recovery code may satisfy the Owner second-factor step only after password authentication.

Recovery-code rules:

- each code becomes invalid after use;
- regenerating recovery codes invalidates the previous set;
- replacement codes are shown only at regeneration;
- recovery-code values, TOTP codes, TOTP secrets, passwords, and temporary/reset credentials are never exposed through normal audit/history.

Broader TOTP-factor replacement/device-migration design and catastrophic Owner recovery remain security/technical dependencies unless separately approved.

### P-015 — Disabled account

Disabled accounts remain preserved for historical attribution but cannot enter the product.

Account-disabled state overrides otherwise valid normal/reset credentials.

If an account is disabled while a session is already active, normal protected use must stop on the next protected navigation/action or authentication-state refresh and the user returns to the sign-in boundary. Exact real-time propagation/session invalidation is technical design.

Re-enabling an account does not revive a previously terminated disabled session; the user signs in again normally.

### Product states

Authentication/credential flows must explicitly support, where applicable:

- login default;
- submitting/authenticating;
- generic invalid credentials;
- account unavailable/disabled;
- Owner TOTP required;
- Owner TOTP enrollment required;
- TOTP/recovery-code verifying;
- invalid/used second-factor code;
- recovery code accepted;
- staff reset request submitted;
- staff reset request Pending;
- staff reset request Resolved;
- reset credential accepted but replacement required;
- forced password change submitting/success/failure;
- temporary authentication/service error.

### Explicitly deferred security/technical details

This PRD does not invent:

- inactivity/session timeout;
- password-strength rules;
- brute-force/rate-limit/lockout mechanics;
- cryptographic credential/TOTP-secret storage;
- exact live-session invalidation transport;
- catastrophic Owner recovery when usable password/authenticator/recovery credentials are unavailable;
- broader authenticator-factor replacement/device-migration behavior beyond the locked enrollment/recovery-code requirements.

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

Patient search is the primary entry action before new registration. New Patient remains deliberate and visually secondary.

The product must not rely only on an earlier manual search to prevent duplicate identity creation: the effective registration action performs a current duplicate-candidate check before creating the Patient.

### P-017 — Search keys and explicit selection

Reception can search by Patient ID, phone, or name.

- an exact valid Patient ID match is visually prioritized because Patient ID is unique;
- search never silently enters patient context solely because a result scored strongly;
- Reception explicitly selects the intended Patient before the workspace begins acting on that identity.

### P-018 — Shared-phone handling

Phone is a matching attribute, not identity proof.

A phone search may return multiple distinct patients, including family members. Those records remain separate result candidates and are not collapsed, auto-selected, or treated as the same person because they share a phone number.

### P-019 — Duplicate candidates

When similar records exist, candidates are shown for receptionist + patient confirmation.

Candidate presentation must provide enough non-clinical identity information to distinguish records, including Patient ID, name, phone, DOB/age context, and other limited identity cues where useful.

Limited prior-Visit context may be opened only to support identity confirmation. Reception does not gain unrestricted diagnosis, notes, or prescription access through the duplicate-review flow.

Similarity may surface candidates but must never auto-select or auto-merge the patient.

### P-020 — Possible Duplicate

If identity cannot be confidently matched, Reception may deliberately create a normal patient record carrying a visible **Possible Duplicate** marker.

When this path is used:

- the marker does not block normal patient/Visit use;
- the product retains enough non-clinical provenance to explain the marker, including the candidate Patient IDs surfaced at creation plus actor/time;
- no auto-merge, auto-link, or silent marker clearing is introduced in V1.

Duplicate resolution/merge remains outside V1.

---

# 12. Patient Registration and Patient Profile

Source: FR-001–FR-007, FR-075–FR-079; BR-001–BR-005, BR-025; OD-001–OD-003.

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

Before successful Patient creation:

- Reception may correct unsaved registration values directly;
- required-field validation applies without adding new business fields;
- phone is not globally unique;
- email is required but is not an identity key;
- DOB is the entered source value and age is derived;
- structurally impossible values such as a future DOB are rejected.

Immediately before effective Patient creation, the product evaluates current duplicate candidates from the entered identifying data. If candidates require review, creation pauses until Reception either selects a confirmed existing Patient or explicitly chooses **Create New as Possible Duplicate**.

If the outcome of Patient creation is unknown because of a service/client failure, the product must check effective state before another create attempt rather than blindly creating a second Patient.

### P-021 — Permanent ID

Successful effective registration creates one permanent Patient ID.

Patient ID is generated only after Patient creation succeeds, is displayed clearly/copy-friendly, and is not editable or replaceable through demographic correction.

### P-022 — Non-semantic ID presentation

Displayed Patient/Visit IDs use stable non-semantic human-readable values; they must not expose DOB, phone, diagnosis, or other personal meaning.

### P-023 — Reception patient profile summary

Reception Patient Profile is an identity/operational view, not longitudinal clinical-history access.

It may show:

- Patient ID and demographics;
- intake/operational safety context already permitted to Reception, including recorded allergy information where captured;
- active Visit operational summary;
- prior Visit identity/operational summaries sufficient for identity matching;
- Possible Duplicate marker and its safe provenance where applicable;
- physical-file Patient ID reference.

It must not expose unrestricted diagnosis, clinical notes, prescriptions, or Doctor longitudinal clinical history solely because Reception opened the patient profile.

### P-024 — Demographic correction

The boundary is explicit:

- before successful Patient creation, Reception may edit unsaved registration data;
- after creation, Reception may not directly overwrite established demographics;
- Patient ID is never a demographic-correction target.

For a Reception correction request:

1. current value and proposed value are captured for each changed demographic field;
2. the effective Patient value remains unchanged while Pending;
3. if an active Visit has an assigned Doctor, the request routes to that Doctor;
4. if an active Visit exists without an assigned Doctor, a Doctor must be selected/assigned before submission;
5. if no active Visit exists, the product does not create a fake Visit solely for correction; Reception selects an authorized Doctor reviewer and the request remains patient-level;
6. Approved applies the proposed value and preserves old/new value, requester, approving Doctor, and time;
7. Rejected leaves the effective value unchanged and preserves decision history.

A submitted request does not disappear merely because the Visit later completes.

If an active Visit is reassigned before decision, the pending correction follows the current assigned Doctor so the locked rule remains “route to the Visit's assigned Doctor.”

If the captured current value changes before Doctor decision, the request is stale for application purposes and cannot silently overwrite the newer value. The Doctor must review refreshed current data.

Doctor may also directly correct demographics without a Reception request, with old/new value, actor, and time preserved.

Exact concurrency/version-control mechanism is technical design.

### P-025 — No physical-file dependency

The generated Patient ID is shown in a copy-friendly form so Reception can associate it with the physical clinic file.

Missing/unavailable physical paper file:

- does not block use of the digital patient identity;
- does not block Visit creation, payment, queueing, or later retrieval;
- must never trigger creation of another Patient ID.

Locating/replacing the physical file remains an offline clinic procedure; V1 does not invent a CRM physical-file replacement workflow.

---

# 13. Visit Creation and Consultation Payment

Source: FR-008–FR-016, FR-108–FR-114; BR-006, BR-010, BR-019–BR-022, BR-033, BR-047–BR-055; OD-004–OD-005, OD-033.

## 13.1 Create Visit

### P-026 — New Visit ID and Patient boundary

Every legitimate clinic attendance creates a unique Visit ID linked to the already-selected permanent Patient ID.

Visit creation:

- never creates or replaces Patient identity;
- is allowed for a Patient carrying Possible Duplicate status;
- is allowed when the physical paper file is unavailable;
- generates the Visit ID only when effective Visit creation succeeds;
- starts the consultation financial outcome as **Unpaid**.

V1 does not invent a one-Visit-per-patient-per-day restriction. Retry safety must distinguish an accidental repeated create attempt from a genuinely separate attendance.

### P-027 — Doctor assignment and queue readiness

A Visit may exist before Doctor assignment.

Reception may assign the Doctor during Visit creation or later, but Doctor assignment is mandatory before queue entry.

This also supports the locked demographic-correction rule: if an active Visit has no Doctor and Reception needs to submit a Visit-linked demographic correction, Doctor selection/assignment must occur before that request submits.

No Visit is created solely to support a patient-level demographic correction when no active Visit exists.

### P-028 — Consultation fee display and Visit-level amount

The applicable consultation fee is shown from clinic configuration. Fee values remain configuration, not PRD policy.

When the Visit is created, the applied consultation fee is captured for that Visit so a later configuration change does not silently rewrite an already-created Visit's financial record.

A separately authorized Visit-specific financial correction may correct an incorrectly recorded Visit amount without changing clinic fee configuration.

## 13.2 Fast payment capture

Normal consultation payment methods:

- UPI;
- Cash;
- Card;
- Other.

Selecting Other requires a short free-text method description.

Payment reference/transaction number is optional.

### P-029 — External payment

The CRM does not initiate, authorize, settle, or wait for the underlying payment.

Reception verifies the external result and then records the payment information.

### P-030 — Explicit Paid action

A Visit begins Unpaid.

To record Paid:

- use the full effective consultation amount for the Visit;
- select UPI, Cash, Card, or Other;
- enter Other description when applicable;
- optionally enter external reference;
- explicitly perform the final **Mark Paid** action.

Selecting a payment method alone never changes financial state.

A Visit already Paid or Waived does not expose the normal Mark Paid action again. Incorrect payment information uses the controlled correction flow.

### P-031 — Queue gate combines financial eligibility and Doctor assignment

Queue entry requires both:

1. effective consultation outcome is **Paid** or **Waived**; and
2. a Doctor is assigned.

Therefore:

- Paid + no Doctor -> financially eligible but not queued;
- Waived + no Doctor -> financially eligible but not queued;
- Doctor assigned + Unpaid -> queue-blocked;
- Pending/rejected waiver + Unpaid -> queue-blocked.

A combined **Mark Paid & Add to Queue** action may be offered only when a Doctor is already assigned.

If payment is successfully recorded but queue insertion subsequently fails or becomes stale, the product preserves the confirmed Paid state, shows that the Visit is not queued, and allows a safe queue-entry retry after refresh. It must not erase a valid financial record merely to make the combined UI appear atomic.

If payment success itself is unknown, queue entry does not proceed until financial eligibility is confirmed.

### P-032 — No partial consultation payment

Partial consultation payment is not available.

V1 does not expose amount-paid/amount-due split entry for consultation payment.

### P-033 — No consultation refund

A paid consultation is non-refundable in V1.

Changing an incorrectly recorded Paid state through Owner-approved payment correction is a record correction, not a refund.

## 13.3 Waiver

### P-034 — Request waiver

Reception or Doctor may request waiver only while the effective consultation financial outcome is **Unpaid**.

Each request requires a mandatory specific reason.

Only one simultaneously actionable Pending waiver request may exist for a Visit. Repeated submission while Pending must not create duplicate Owner work.

Underlying financial outcome remains Unpaid while Pending.

Because an Unpaid Visit is not in the Doctor queue, an assigned Doctor must have a separate pre-queue financial-resolution context from which **Request Waiver** is reachable. This context is not an ordered queue and does not grant consultation access merely because the Doctor may request waiver.

A rejected waiver remains historical; while the Visit remains Unpaid, a later new waiver request may be submitted with a new reason.

### P-035 — Owner waiver decision

Only Owner authority approves/rejects the waiver.

Before decision, current financial state is revalidated.

- approval from the current Unpaid state -> effective outcome becomes Waived;
- rejection -> effective outcome remains Unpaid;
- if the Visit became Paid while the request was pending, the request is stale/non-actionable and cannot later be approved into Waived against the newer Paid state.

Requester, reason, Owner decision/authority, and timestamps remain attributable.

### P-036 — Owner direct waiver

For a currently Unpaid Visit, Owner may perform **Direct Waiver** as an immediate Owner-authority action:

- identify the Visit and current consultation amount;
- enter mandatory specific reason;
- explicitly confirm;
- effective outcome immediately becomes Waived;
- no second approval step or artificial pending request is created;
- actor/Owner authority/reason/time are audited.

Paid or already-Waived Visits do not expose normal Direct Waiver.

### P-037 — Waiver queue effect

Pending/rejected waiver does not make an Unpaid Visit queue-eligible.

Approved/direct Waived makes the Visit financially eligible, but actual queue entry still requires Doctor assignment under P-031.

## 13.4 Incorrect payment record

### P-038 — Payment correction request

Reception cannot silently rewrite an established consultation payment record. Pharmacy uses the same correction principle for its own payment records.

A consultation payment-correction request captures:

- Visit/payment identity;
- the current effective financial record used as the correction baseline;
- proposed corrected financial record;
- mandatory specific reason.

The proposal may correct, where applicable:

- Paid/Unpaid state when the recorded state was wrong;
- payment method;
- Other method description;
- optional external reference;
- the Visit-specific recorded consultation amount if that amount itself was recorded incorrectly.

Correcting the Visit-specific amount does not alter clinic fee configuration.

If the proposed effective state is Paid, it represents the full corrected effective consultation amount and must satisfy the same payment-method requirements as normal Paid capture. No partial-payment representation is introduced.

Waiver is a separate Owner-controlled financial exception and is not silently created/revoked through payment correction.

Only one simultaneously actionable correction request for the same current payment baseline should exist.

### P-039 — Payment correction decision and non-destructive effect

Owner approval creates the corrected effective financial record while preserving the original record, request, reason, Owner decision, and timestamps. Rejection keeps current effective state unchanged.

Before approval, the product revalidates the correction baseline. If the effective payment record changed after the request was submitted, the old request is stale and cannot silently apply against the newer record.

Financial correction does not retroactively delete or rewrite already-created queue/clinical history.

Current operational effect follows the Visit stage defined in G5:

- before queue entry, the corrected financial state governs future queue eligibility;
- while current queue state is Waiting/Called, correction to Unpaid may remove **current active queue membership** and return the Visit to non-queued financial resolution, while all prior queue events remain historical;
- at With Doctor or later, correction does not unwind active/past clinical workflow.

Removing current membership is not deletion of prior queue history. Re-entry after renewed eligibility is a new active queue-entry event rather than restoration of the old queue position.

A Paid -> Unpaid correction means the earlier Paid record was erroneous; it does not represent or create a refund.

### Retry/unknown-outcome safety

Visit creation and Paid recording are state-changing operations.

- disable duplicate final submit while the action is pending;
- if outcome is unknown, refresh/check effective Visit/payment state before another create/Paid attempt;
- do not knowingly create duplicate Visit/payment records through blind retry.

Exact idempotency implementation remains technical design.

---

# 14. Doctor Queue

Source: FR-017–FR-025, FR-080–FR-081, FR-092–FR-093; BR-007–BR-010, BR-026–BR-029, BR-049; OD-006–OD-007, OD-019.

## 14.1 Queue membership and ordering

Each Doctor has a separate persisted ordered queue.

A Visit becomes a queue member only after G4 requirements are satisfied:

- Doctor is assigned;
- effective consultation financial outcome is Paid or Waived.

On first queue entry:

- current queue state becomes **Waiting**;
- Visit is appended to the end of the selected Doctor's current queue;
- queue-entry event/time is recorded.

Assigned Visits that are not financially eligible remain outside queue membership, counts, and ordering.

Reception sees Doctor queues needed for operations. Each Doctor sees only that Doctor's queue actions. Owner may see clinic-wide queue status. Pharmacist does not gain Doctor-queue controls.

The product does not expose arbitrary queue-position editing, drag/drop reordering, hidden priority, or urgent-priority insertion.

Order changes only through defined operations: new entry, reassignment, Unresponded reposition, patient-leaves move-to-end, or cancellation/removal.

### P-040 — Queue row information and current membership

Each active queue row exposes the minimum operational context required to act:

- queue position;
- patient identity;
- Visit ID;
- current Doctor;
- current queue state;
- current financial eligibility;
- waiting/call timestamps as relevant;
- Pending Cancellation indicator where applicable;
- actions currently valid for the current role/state.

Historical queue events remain distinct from current queue membership.

Ordinary reassignment/repositioning preserves the existing queue-entry journey rather than pretending the Visit just arrived.

### P-041 — Doctor call and consultation start boundary

Doctor may **Call Patient** only while the Visit is currently Waiting in that Doctor's queue.

Call Patient:

- sets current queue state to Called;
- records Doctor/time;
- makes the call state visible to Reception.

Viewing/opening a queue row does not itself change state.

**Start Consultation** is a separate explicit Doctor action:

- allowed from Called only;
- requires Visit still assigned to that Doctor;
- transitions current Visit state to With Doctor.

If state/assignment changed first, stale Call/Start action is blocked and refreshed.

### P-042 — Reassignment

Reception may reassign a currently queued Visit only while current state is Waiting or Called.

Reassignment:

- requires explicit destination Doctor different from current Doctor;
- requires confirmation showing source/destination;
- removes current membership from source Doctor queue;
- changes assigned Doctor;
- inserts Visit at the **end** of destination Doctor queue as Waiting;
- preserves prior queue/call/reassignment history;
- if source state was Called, that Call remains historical but is no longer current.

Reassignment is unavailable from With Doctor or later.

If a pending Visit-linked demographic-correction request exists, reviewer routing follows the newly assigned Doctor and stale prior-Doctor decision is blocked.

### P-043 — Unresponded

Reception may mark Unresponded only from current Called state.

On success:

1. record Unresponded event/actor/time;
2. move Visit five positions down in current Doctor queue;
3. if fewer than five later positions exist, move to end;
4. set current queue state back to Waiting;
5. display new position.

Unresponded remains visible as history even though current state returns to Waiting.

If state/assignment changed first, stale Unresponded action does not apply.

### P-044 — Patient leaves before consultation

Before With Doctor, Reception may explicitly move a financially eligible queue member (Paid or Waived) to the end when the patient temporarily leaves.

Available from Waiting or Called.

On success:

- current state becomes/remains Waiting;
- Visit moves to end of same Doctor queue;
- prior Call remains historical if applicable;
- financial state is unchanged;
- action is not cancellation and creates no refund.

### Financial correction while queue is active

If Owner-approved payment correction changes the effective financial outcome to Unpaid while current state is Waiting or Called:

- remove current active queue membership;
- preserve all prior queue/call/reassignment/reposition history;
- show active Visit outside queue as Not Queued / Unpaid;
- renewed Paid/Waived eligibility does not restore old position; explicit re-entry appends at end and records a new active queue-entry event.

If correction to Unpaid occurs at With Doctor or later, do not unwind clinical workflow; show the corrected financial state/history without destructive rollback.

### P-045 — Urgent case

No software priority control is shown.

Do not add priority flag/star, urgency score, priority request, arbitrary reorder shortcut, or automated queue jump.

Urgent escalation remains direct Reception-to-Doctor communication outside the CRM.

Doctor may still call a selected Waiting Visit as allowed by the locked workflow; that selection does not silently rewrite persisted queue order.

## 14.2 Consultation cancellation

### P-046 — Doctor cancellation request

Doctor with authorized Visit access may request cancellation while current Visit state is:

- Waiting;
- Called;
- Unresponded where the transient state is still current/observable;
- With Doctor;
- Consultation Completed;
- Sent to Pharmacy.

Completed and Cancelled/Voided do not expose this request.

Request requires a mandatory specific free-text reason.

Only one simultaneously actionable Pending cancellation request may exist per Visit.

Submitting the request:

- does not change current Visit state;
- does not freeze active workflow;
- records requester/reason/time;
- shows Pending Cancellation to authorized operational users.

If Visit progresses while Pending, the request follows the Visit. If current state reaches Completed before Owner decision, the request becomes stale/non-actionable and cannot be approved through cancellation flow.

### P-047 — Owner cancellation decision

Before decision, Owner view revalidates:

- request still Pending;
- current Visit state remains cancellable;
- current downstream history/impact.

Approve:

- current Visit becomes Cancelled/Voided;
- Visit leaves active queue/workflow;
- all existing queue, financial, clinical, prescription, dispensing, billing, and request history remains;
- no refund is created.

Reject:

- current Visit state remains whatever valid state it has reached by decision time;
- request/rejection history remains.

If Visit was resolved/completed/cancelled by another state change/session first, stale Owner decision is blocked.

Same-human Owner+Doctor actions remain separately attributable by authority.

### Cancellation effect by current stage

When approval applies:

- Waiting/Called -> remove active queue membership;
- With Doctor -> stop further active consultation progression while preserving saved clinical history;
- Consultation Completed -> preserve clinical content but stop remaining active downstream Visit progression;
- Sent to Pharmacy -> stop future active Visit workflow while preserving already-created prescription/dispensing/billing history.

Completed Visits use applicable correction/amendment workflows instead of cancellation.

### Queue action concurrency

Every queue-changing action must revalidate current state and Doctor assignment before applying.

Stale action never silently overwrites newer queue state.

---

# 15. Doctor Consultation Workspace

Source: FR-026–FR-032, FR-091, FR-113; BR-011–BR-013, BR-034, BR-036, BR-038, BR-054; OD-008–OD-009, OD-035.

## 15.1 Consultation layout

**Classification: DERIVED PRODUCT DESIGN.**

Recommended workspace structure:

1. patient identity header;
2. current Visit context and state;
3. allergies/important patient context;
4. Possible Duplicate identity warning where applicable;
5. relevant longitudinal history for the current Patient ID;
6. current consultation entry;
7. demographic-correction task/context where applicable;
8. prescription area/action.

### P-048 — Assigned Visit access and authoring boundary

Doctor may inspect permitted patient/Visit context from the Doctor workspace, but active clinical authoring begins only after the explicit G5 transition **Called -> With Doctor**.

The editable current consultation requires:

- current Visit state = With Doctor;
- current Visit still assigned to that Doctor;
- Visit not Cancelled/Voided.

Opening a queue row or historical Visit does not itself start consultation or enable active authoring.

### P-049 — Longitudinal history

Doctor with authorized clinical access can review the longitudinal archive for the **current permanent Patient ID**, including relevant prior consultations, diagnoses/assessments, prescriptions, amendments, and superseded records.

Possible Duplicate status may be shown as identity-risk context, but candidate Patient IDs are not automatically merged and their clinical histories are not combined into one timeline.

Historical records are read-only unless a specific authorized correction/amendment flow is invoked.

### P-050 — Required clinical entry and draft behavior

The current consultation supports:

- chief complaint / patient problem;
- clinical assessment / diagnosis.

While Visit is With Doctor, Doctor may save an in-progress consultation draft before both required fields are complete.

**Save Draft**:

- does not complete the consultation;
- retains the current Visit/Patient association;
- records Doctor and save time;
- may overwrite the current in-progress draft only when the loaded draft/state is still current.

If another successful save or Visit-state change occurred first, stale save must not silently overwrite newer content.

V1 does not require a permanent revision chain for every intermediate draft save.

### P-051 — Optional clinical entry

Support optional:

- symptoms/history;
- examination findings;
- clinical notes;
- advice;
- follow-up information.

Optional fields do not become mandatory merely because they are present in the interface.

### P-052 — Complete consultation

**Complete Consultation** is an explicit Doctor-only action available from current With Doctor state.

Before completion:

- chief complaint / patient problem is present;
- assessment / diagnosis is present;
- current Doctor assignment and With Doctor state are revalidated;
- current clinical content is successfully persisted.

On success:

- the current clinical content becomes the completed effective consultation record;
- Doctor/time are recorded;
- the **Consultation Completed** event/state transition is recorded;
- ordinary clinical fields become read-only.

Completion does not itself finalize a prescription. After the Consultation Completed transition, apply the G7 pharmacy-readiness rule: if a current Finalized prescription already exists, the Visit may immediately advance from Consultation Completed to **Sent to Pharmacy**. If not, current state remains Consultation Completed awaiting prescription finalization.

A stale/duplicate completion action cannot create another completed clinical record.

### P-053 — Clinical amendment and revision chain

Completed consultation content is never edited in place.

Doctor chooses **Create Amendment** from the current effective completed clinical record.

Amendment:

- starts from the current effective revision;
- changes only the clinical content needing correction/addition;
- requires a mandatory specific amendment reason;
- uses an explicit final action;
- creates a new effective clinical revision;
- preserves all earlier revisions as read-only history;
- records Doctor/time/reason.

If another amendment became effective after the form was loaded, stale amendment submission is blocked pending refreshed review.

A completed clinical record may be amended after the Visit later becomes Completed or Cancelled/Voided. Amendment corrects historical clinical content only; it does not reopen the Visit, queue, payment, prescription, or pharmacy workflow.

If a Visit was Cancelled/Voided while With Doctor **before consultation completion**, already-saved draft content remains historical/read-only but is not relabelled as a completed consultation and does not receive the completed-consultation amendment flow.

### Demographic correction within Doctor workflow

Doctor workspace must make assigned demographic-correction tasks reachable.

For a Reception-originated request:

- show patient/Visit context where applicable;
- field, captured current value, proposed value, requester/time;
- revalidate current value and current reviewer authority before decision;
- Approved applies proposed value with old/new/requester/Doctor/time history;
- Rejected leaves the effective value unchanged;
- Stale current value or stale reviewer assignment blocks the old decision.

For a patient-level request with no active Visit, the selected Doctor reviewer may decide without creating a fake Visit.

Doctor direct demographic correction is a separate action with explicit current -> proposed value, Patient ID immutable, and old/new/Doctor/time audit.

### Cancellation concurrency

Pending cancellation does **not** freeze consultation.

While request remains Pending and Visit remains With Doctor, normal permitted draft save/completion may continue.

If Owner cancellation becomes effective while Doctor has the consultation open:

- future Save Draft / Complete Consultation / active clinical actions are blocked;
- already-saved clinical content remains preserved;
- stale local content is not silently committed over Cancelled/Voided state.

If completion becomes effective first, the completed record remains preserved and Owner may decide cancellation from Consultation Completed if still valid under G5.

### P-054 — Clinical access boundary

Full consultation/diagnosis/notes require Doctor authority and authorized patient/Visit context.

- Owner-only, Admin-only, Reception, and Pharmacist authority do not expose unrestricted full clinical content.
- Owner+Doctor receives full clinical access only through Doctor authority context.
- operational/audit views may identify that clinical content changed without exposing the content itself.

---

# 16. Prescription Builder and Lifecycle

Source: FR-033–FR-044, FR-083, FR-116; BR-014–BR-016, BR-030, BR-058; OD-010–OD-012, OD-032, OD-037.

## 16.1 Prescription draft and medicine selection

A Visit may have one current unfinalized prescription draft while active.

Doctor may create/edit that draft while the Visit is:

- **With Doctor**; or
- **Consultation Completed** and still awaiting a finalized prescription.

A draft is tied to the same Patient ID + Visit ID and is not dispensable merely because it exists.

Once a current prescription is Finalized, ordinary draft editing stops; correction uses replacement.

Completed or Cancelled/Voided Visits do not expose new active prescription authoring.

Search/select by:

- clinic medicine/display name;
- strength;
- dosage form.

Manufacturer is available as medicine/inventory context.

Optional generic/molecule may be searchable.

### P-055 — Availability at prescribing and finalization snapshot

During authoring, Doctor sees current clinic-wide availability:

- In Stock;
- Out of Stock;
- Not Stocked.

Immediately before finalization/replacement finalization, refresh the current availability used for the prescription's finalization record.

For each finalized item, retain the clinic-wide availability-at-finalization value used by the printed ** marker. Later inventory movement does not rewrite this snapshot.

### P-056 — Multi-pharmacy availability

When multiple pharmacy units exist, Doctor can see clinic-wide total plus per-unit availability where inventory is known.

This view is read-only and grants no stock-control action.

Per-unit/current availability may change after finalization; the printed unavailable marker continues to use the frozen clinic-wide finalization snapshot.

### P-057 — Unavailable prescribing

Out of Stock or Not Stocked does not block Add to Prescription or finalization.

The product informs the Doctor but does not turn inventory availability into clinical prescribing authority.

## 16.2 Prescription instructions and draft validation

Per medicine:

- medicine/display identity;
- strength;
- dose amount;
- frequency;
- duration;
- optional timing/food/short instruction;
- prescribed quantity.

Doctor may add/remove/edit rows while the prescription is unfinalized and the Visit remains in an allowed authoring state.

### P-058 — Quantity calculation

Where dose/frequency/duration plus configured dispensing unit make quantity deterministic, the system calculates and displays the quantity.

Where calculation is not deterministic, Doctor must enter quantity.

Prescription finalization is blocked while any item has unresolved required instructions or unresolved quantity.

The product does not require duplicate manual entry of a deterministically calculated quantity.

### P-059 — Finalize prescription and pharmacy-readiness rule

**Finalize Prescription** is an explicit Doctor-only action.

Before finalization:

- Visit remains active and is With Doctor or Consultation Completed;
- Visit is not Completed or Cancelled/Voided;
- at least one complete prescription item exists;
- every item has required medicine/strength/dose/frequency/duration;
- every item has resolved prescribed quantity;
- current prescription/draft baseline is revalidated;
- finalization-time availability is refreshed.

On success, freeze the finalized version with:

- Patient ID + Visit ID;
- Doctor and finalization time;
- medicine identities/instructions/quantities;
- finalization-time clinic-wide availability per item;
- current version identity/status.

Finalization and clinical completion are independent prerequisites for pharmacy handoff:

- Finalize while **With Doctor** -> prescription becomes Finalized, Visit remains With Doctor until consultation completion;
- if consultation becomes **Consultation Completed** while a current Finalized prescription already exists -> readiness condition is met and Visit advances to **Sent to Pharmacy**;
- Finalize while already **Consultation Completed** -> readiness condition is met and Visit advances to **Sent to Pharmacy**.

The transition records Consultation Completed as the clinical event even if the readiness rule immediately advances current Visit state to Sent to Pharmacy.

A finalized prescription that exists while the Visit is still With Doctor is not yet pharmacy-ready for dispensing.

The locked V1 sources do not define a no-prescription bypass from Consultation Completed directly to Completed; this PRD does not invent one.

### P-060 — Immutable finalized prescription

Finalized prescription is read-only in ordinary use.

Doctor may print/reprint the finalized version and, while the Visit remains active, use the explicit replacement flow when correction is required.

Pharmacist cannot edit or finalize it.

### P-061 — Replacement prescription

Doctor may replace the current finalized prescription while the Visit remains active in:

- With Doctor;
- Consultation Completed;
- Sent to Pharmacy.

Replacement is unavailable as a new active workflow action after Visit becomes Completed or Cancelled/Voided.

Replacement:

- starts from the current active finalized version;
- shows any known prior dispensing against the prescription/version lineage;
- requires mandatory specific correction reason;
- requires explicit final replacement action;
- refreshes finalization-time availability;
- atomically marks the previous active version **Superseded** and makes the replacement **Finalized/current**;
- links old/new versions and records Doctor/reason/time.

If the active version changed or Visit became Cancelled/Voided before submit, stale replacement is blocked.

If replacement is finalized while current Visit is Consultation Completed, the normal readiness rule advances to Sent to Pharmacy. If already Sent to Pharmacy, Visit remains Sent to Pharmacy.

### P-062 — Prior dispensing and version lineage preserved

Prescription replacement never reverses or deletes prior dispensing, inventory deduction, or billing history.

Prior dispensed quantity remains attributable to the version/item from which it was supplied.

Subsequent dispensing uses the latest current finalized prescription together with preserved prior dispensing history; exact remaining-dispensable calculation across a replacement is owned by the pharmacy-dispensing contract.

Pharmacy defaults to the latest current finalized prescription and must not silently continue an old Superseded version.

## 16.3 Prescription output

### P-063 — A4 print/reprint

Print and reprint use a finalized prescription version.

Normal print/reprint defaults to the current Finalized version.

Reprint:

- does not create a new prescription version;
- does not recompute medicine availability using current stock;
- uses the same finalization-time prescription data/availability snapshot.

Superseded prescriptions remain historical/read-only rather than becoming the default printable/dispensable prescription.

Detailed historical-copy labeling is finalized in the printing review.

### P-064 — ** availability marker

For each finalized version, medicines whose **clinic-wide availability at that version's finalization** is Out of Stock or Not Stocked receive ** plus the locked explanatory legend.

Later:

- stock changes;
- partial dispensing;
- another pharmacy unit's availability change

do not retroactively change the finalized version's marker.

A replacement prescription receives its own new finalization-time availability snapshot and print markers.

### Cancellation and amendment boundaries

Pending Visit cancellation does not by itself freeze otherwise-valid prescription work.

Once Visit becomes Cancelled/Voided:

- stop new draft saves/finalization/replacement/pharmacy-readiness progression;
- preserve existing Draft/Finalized/Superseded prescription versions and all prior dispensing history as read-only history.

A later clinical amendment never silently changes prescription content. If an active Visit still requires prescription correction, Doctor uses the explicit replacement flow.

### Concurrency safety

Finalize/replacement revalidates:

- Visit state;
- Doctor authority/current context;
- current draft or active prescription version;
- finalization-time availability refresh.

A stale action cannot overwrite a newer replacement or continue active prescription workflow after effective Visit cancellation.

---

# 17. Pharmacy Workspace

Source: FR-045–FR-056, FR-082–FR-083, FR-094–FR-095, FR-118; BR-013, BR-017–BR-018, BR-040–BR-042, BR-058; OD-013–OD-014, OD-017, OD-032, OD-037.

## 17.1 Pharmacy retrieval and access

**Classification: DERIVED PRODUCT DESIGN.**

Recommended focus:

- Patient ID / pharmacy-ready Visit lookup;
- current pharmacy unit;
- pending dispensing;
- substitution requests;
- pharmacy bill/payment;
- inventory alerts;
- inventory-change/transfer requests.

### P-065 — Patient ID lookup and pharmacy-ready selection

Patient ID is the required V1 pharmacy lookup.

Lookup results must distinguish Visits/prescriptions clearly enough to avoid selecting the wrong attendance:

- Patient ID;
- Visit ID;
- Visit date/time where useful;
- Visit state;
- prescription version/status;
- Doctor where useful.

Dispensing action is available only when:

- current Visit state is **Sent to Pharmacy**;
- a current **Finalized** prescription exists;
- current prescription version is still the latest current version.

A Finalized prescription attached to a Visit still With Doctor is not pharmacy-ready.

If more than one relevant Visit/prescription exists, Pharmacist explicitly selects the intended one; the product does not guess from name/phone or silently choose a historical Visit.

### P-066 — Clinical visibility boundary

Pharmacist may see only the clinical context required for fulfilment:

- patient identity needed for dispensing;
- current prescription;
- previous prescriptions as read-only history;
- known allergies;
- dispensing-relevant medicine instructions;
- prescribed/remaining quantities;
- pharmacy availability;
- billing/payment context.

Pharmacist must not receive unrestricted:

- diagnosis/assessment;
- consultation notes;
- Doctor longitudinal clinical history;
- unrelated clinical content.

Superseded/previous prescriptions remain visually historical and do not become the default dispensing source.

## 17.2 Dispensing

### Prescription-item fulfilment lineage

Each finalized prescription item participates in a fulfilment lineage across prescription replacement.

When replacement carries forward the same prescribed item identity, prior dispensing remains attributable to that lineage.

If Doctor materially changes/adds a medicine identity, it is a new fulfilment lineage. Removed lineages remain historical and cannot receive further dispensing.

The implementation may use stable item IDs or an equivalent mechanism; product behavior must preserve lineage outcomes.

### P-067 — Dispense actual quantity

For each current active prescription item show:

- current prescribed quantity;
- cumulative quantity already dispensed against that item lineage across all prescription versions/units;
- remaining allowable quantity;
- current active pharmacy-unit valid stock;
- quantity to dispense;
- resulting unsupplied remainder.

Remaining allowable is:

**max(0, current active prescribed quantity − cumulative prior dispensed quantity attributable to that lineage).**

If prior dispensing already exceeds a later corrected prescribed quantity, remaining allowable is 0. The historical excess is displayed/audited; stock/dispensing is not reversed.

Entered dispense quantity must be greater than 0 and no greater than both:

- remaining allowable quantity; and
- current valid available stock in the active pharmacy unit.

### P-068 — Atomic dispensing and automatic stock deduction

Final **Dispense** confirmation revalidates:

- Visit is still Sent to Pharmacy and not Cancelled/Voided;
- prescription version is still current Finalized;
- item lineage/current prescribed quantity;
- remaining allowable after any other dispensing action;
- active pharmacy unit;
- current valid/non-expired stock quantity.

On success:

- record actual supplied quantity;
- record prescription version/item lineage;
- record pharmacy unit and Pharmacist/time;
- reduce only that pharmacy unit's valid inventory by the actual supplied quantity.

Dispensing record and stock deduction must succeed as one effective operation. If outcome is unknown, current state is checked before retry so duplicate dispensing is not intentionally created.

### P-069 — Partial dispensing

Pharmacy may supply less than the current remaining allowable quantity.

For each item:

- record actual supplied quantity;
- deduct only supplied stock;
- mark/display the unsupplied remainder;
- bill only supplied quantity.

Partial supply never rewrites the prescription.

A remaining quantity is not an inventory reservation.

### P-070 — No back-order / no stock reservation

V1 does not create:

- collect-later orders;
- reserved stock;
- promised future pharmacy fulfilment;
- automatic replenishment/back-order records.

Another permitted pharmacy unit may later dispense a still-allowable remainder while the Visit/prescription remains active, but no unit has stock reserved by the CRM merely because a remainder exists.

### P-071 — Over-dispense and concurrency prevention

Across all dispensing actions, prescription versions in the same active item lineage, and pharmacy units, cumulative fulfilment cannot exceed the latest current active prescription allowance.

Before every dispense, recompute remaining allowance using current committed dispensing history.

If another session/unit dispensed first, stale quantity is rejected and Pharmacy refreshes current remaining allowance.

A Superseded prescription screen cannot be used to continue dispensing.

### P-072 — Substitution request and approved substitute fulfilment

Pharmacist cannot independently substitute.

A substitution request must identify:

- current prescription version;
- original prescription item/lineage;
- proposed substitute medicine;
- proposed substitute quantity;
- mandatory reason.

Pending substitute remains non-dispensable.

Doctor approval authorizes only the approved proposal.

If substitute strength/form/unit is not safely comparable, Pharmacist must not infer conversion; Doctor must explicitly confirm the substitute quantity/instruction needed for dispensing.

Approved substitute dispensing:

- records the substitution approval reference;
- records actual substitute supplied;
- consumes the approved amount against the original prescription item's remaining fulfilment allowance;
- prevents original + substitute supply from silently exceeding the permitted remaining fulfilment.

A substitution request becomes stale/non-actionable if:

- prescription version/item is superseded;
- Visit becomes Cancelled/Voided;
- requested remaining quantity is no longer available because dispensing occurred first;
- another state change makes the proposal no longer current.

### P-073 — No medicine return

No medicine-return, restock-from-return, or refund-through-return workflow exists in V1.

Already-dispensed medicine is corrected only through separately authorized inventory/financial mechanisms where applicable; it is not undone by a return UI.

### P-074 — Prescription-required and Visit-state-required dispensing

V1 CRM dispensing requires:

- pharmacy-ready Visit state = Sent to Pharmacy;
- a current Finalized prescription;
- active valid prescription item;
- permitted active pharmacy unit.

General non-prescription retail/pharmacy-only dispensing is outside V1.

Effective Visit cancellation immediately blocks future dispensing while preserving all prior dispensing/stock history.

Dispensing itself does not mark the Visit Completed; pharmacy billing/payment/completion is governed by G9.

## 17.3 Multi-pharmacy fulfilment

### P-075 — Unit-specific dispensing

Every dispensing transaction belongs to the pharmacy unit that physically supplied the medicine.

The active pharmacy unit remains visible throughout dispensing.

If a Pharmacist changes pharmacy unit while unsaved dispense quantities are entered, the product requires explicit discard/review; quantities never silently move between units.

Cumulative fulfilment allowance is clinic-wide across units even though stock ledgers remain unit-specific.

### P-076 — Unit-specific billing handoff

Each pharmacy unit hands off to billing only the quantities that unit actually dispensed.

If multiple units fulfil one prescription:

- each unit records its own dispensing;
- each unit later bills only its own supplied quantities;
- prescription-wide remaining allowance is still shared across all units;
- unit-level audit history remains intact even when Owner reporting later consolidates it.

### Replacement/cancellation stale-state rules

If Doctor replaces the prescription while Pharmacy has a dispense screen open:

- old version becomes non-dispensable immediately;
- pending unsaved quantities on the old version cannot be committed;
- refresh to latest current Finalized prescription;
- recompute remaining allowance from preserved prior dispensing.

If Visit cancellation becomes effective before Dispense commits:

- block the stale dispense;
- preserve already-committed dispensing and stock deduction;
- do not automatically restore stock.

Technical locking/transaction implementation is deferred, but these product outcomes are mandatory.

---

# 18. Pharmacy Billing and Payment

## 18.1 Bill creation and scope

### P-077 — Bill only supplied items and prevent double billing

A pharmacy bill is created only from **committed dispensing records** for medicine quantities actually supplied by the clinic pharmacy.

Bill creation must satisfy all of the following:

- prescribed-but-unsupplied quantity is never billed;
- each bill belongs to the pharmacy unit that physically supplied the billed quantity;
- Pharmacist explicitly creates a bill from supplied-but-not-yet-billed dispensing records for the current Visit and active pharmacy unit;
- one committed dispensing quantity cannot be included in more than one active/non-voided bill lineage;
- prescription replacement does not re-bill dispensing that was already billed under an earlier prescription version;
- if multiple pharmacy units supply one Visit, each unit may create its own bill for that unit's dispensing records.

On successful creation, the bill starts **Unpaid**.

### Bill snapshot and ordinary immutability

At bill creation, freeze the billing facts needed to understand that charge:

- Visit and Patient identity;
- pharmacy unit;
- source dispensing references;
- medicine/bill lines;
- supplied quantities;
- unit price / price basis;
- configured tax/amount fields where applicable;
- total amount.

Later medicine-price/configuration changes or prescription replacement do not silently rewrite an existing bill.

Established bill lines and total are not edited in place. If the bill itself is wrong, V1 uses the Owner-controlled bill-void path. Payment correction changes the **payment record**, not the bill lines or total.

### P-078 — External pharmacy payment recording

Pharmacy payment execution remains external to the CRM.

For an active or historical non-voided bill, Pharmacy may record:

- Unpaid or Paid;
- UPI, Cash, Card, or Other;
- mandatory short description when Other is selected;
- optional external payment/reference number.

Selecting a payment method does **not** mark the bill Paid. After staff verifies full external payment success, Pharmacist performs an explicit **Mark Paid** action.

Paid means the full current bill total was externally paid. Existing bills may still be marked Paid after the Visit itself is Completed; recording payment never reopens the Visit.

### Pharmacy payment correction

Pharmacy uses the P-038/P-039 correction principle for an incorrect established payment record.

A Pharmacy payment-correction request captures:

- bill/payment identity;
- current effective payment record as the correction baseline;
- proposed corrected payment record;
- mandatory specific reason.

The proposal may correct Paid/Unpaid state, method, Other description, optional reference, or other payment-record metadata. It does **not** change bill lines or bill total.

Owner approves/rejects. Before approval, the current payment baseline is revalidated; if it changed after request submission, the old request is stale and cannot overwrite the newer record. Paid -> Unpaid is a record correction, not a refund.

Only one simultaneously actionable correction request for the same current payment baseline should exist.

### P-079 — No partial pharmacy payment

Partial pharmacy payment is unavailable in V1.

Do not expose amount-paid/amount-due split controls for a pharmacy bill.

### P-080 — No pharmacy refund

Refunds are unavailable in V1.

A bill void or Paid -> Unpaid payment correction must never be presented as a refund.

## 18.2 Visit-level pharmacy completion

Dispensing or paying one bill does not by itself complete the Visit.

While the Visit is **Sent to Pharmacy**, Pharmacist may explicitly finish clinic-pharmacy fulfilment and transition the Visit to **Completed** only when the product revalidates all of the following:

- no committed dispensing record for the Visit remains unbilled;
- all intended clinic-pharmacy dispensing work is finished;
- any remaining prescription quantity is explicitly left unsupplied/outside with no reservation/back-order;
- committed dispensing from every pharmacy unit involved in the Visit is accounted for in that unit's bill(s);
- no actionable substitution request still represents intended clinic fulfilment.

Payment state does **not** gate pharmacy completion. Bills may remain Unpaid after Visit completion because V1 has no locked pharmacy-payment gate equivalent to the consultation queue gate.

Completion is Visit-level. One pharmacy unit's bill/payment cannot complete the Visit while another unit still has committed unbilled dispensing.

After Visit = Completed:

- no new dispensing is allowed;
- no new bill may be created for that Visit;
- existing bills may still receive external Paid recording, Owner-controlled payment correction, or Owner-controlled void administration;
- those financial-history actions do not reopen the Visit.

Unsupplied remainder is a legitimate final pharmacy outcome; Completed never means every prescribed unit was supplied.

## 18.3 Bill cancellation / void

### P-081 — Pharmacist void request

Pharmacist may request cancellation/void of an existing non-voided bill with a mandatory specific reason.

Only one simultaneously actionable Pending void request may exist for a bill.

Effective Visit cancellation or Visit completion does not erase an already-created bill and does not prevent record-level bill administration; however, it does block new dispensing and new bill creation.

### P-082 — Pending bill remains active

While bill-void request is Pending:

- the bill remains active;
- existing payment state remains effective;
- external payment may still be recorded because the bill has not yet been voided;
- payment correction may still occur through its separate Owner-controlled flow.

Therefore Owner must not rely on the payment state captured when the void request was first submitted; current payment state is revalidated at decision time.

### P-083 — Owner decision

Owner decision revalidates the bill, request state, and latest payment state.

- Approve -> bill exits active billing and becomes Cancelled/Voided.
- Reject -> active bill remains unchanged.

If the bill became Paid while the request was Pending, Owner may still approve void, but the UI must explicitly state that approval does **not** create a refund.

Original bill snapshot, source dispensing, original/current payment history, requester, reason, Owner decision, authority, and timestamps remain preserved.

A voided bill remains historical. V1 does not silently regenerate or automatically re-bill its source dispensing records.

### P-084 — No automatic stock restoration

Bill void does not reverse dispensing or restore inventory.

If a legitimate stock correction is required, it is a separate G10 inventory-adjustment workflow requiring the applicable Owner control.

## 18.4 State-change and retry safety

For bill creation, Mark Paid, Visit pharmacy completion, bill-void request/decision, and payment correction:

- disable duplicate final submit while the action is pending;
- revalidate current Visit/bill/request/payment state before applying;
- if the outcome is unknown, refresh/check effective state before blind retry;
- do not knowingly create duplicate bills, duplicate payment records, duplicate completion transitions, or duplicate requests/decisions.

Exact transaction/idempotency mechanics remain technical design.

---

# 19. Inventory Workspace

Source: FR-051, FR-068, FR-086–FR-090, FR-094–FR-096, FR-115–FR-118; BR-017, BR-031–BR-032, BR-037, BR-040–BR-041, BR-056, BR-058.

## 19.1 Inventory model and ledger truth

Inventory supports:

- medicine-specific base stock/dispensing unit;
- configured higher package units and conversion factors;
- batch/lot;
- expiry;
- manufacturer;
- purchase price;
- selling price;
- configurable low-stock threshold;
- configurable near-expiry threshold.

Quantity-changing inventory behavior is represented as attributable **movements/events**, not silent replacement of a current-stock number.

Normal dispensing, approved additions, approved reductions/corrections, expiry disposition, and approved transfers all leave movement history. Later corrections create new controlled movements rather than rewriting prior movement history.

### P-085 — Pharmacy-unit stock and movement ledger

Each pharmacy unit maintains an independent stock ledger.

For every quantity-changing movement preserve, as applicable:

- pharmacy unit;
- medicine;
- batch/lot;
- entered quantity and entered unit;
- normalized base-unit quantity;
- movement category;
- stock before;
- delta;
- stock after;
- source reference such as prescription/dispense, adjustment, or transfer;
- actor/effective authority;
- timestamp.

Clinic-wide stock is derived from pharmacy-unit ledgers. Consolidated totals must never replace unit-level attribution.

### Base/package conversion

Every medicine has a configured base unit. Higher package quantities may be used for entry/display when configured.

Before a material quantity-changing request is submitted or approved:

- show the package-to-base conversion;
- show the normalized base-unit effect;
- retain the conversion/result used for the movement so later configuration changes do not rewrite historical movement meaning.

### Batch attribution

Where inventory is held by batch/lot, quantity-changing movements identify the concrete batch/lot and preserve its expiry/manufacturer context.

A dispense must resolve actual valid/non-expired batch stock before commit. Expired or insufficient batch quantity cannot be used invisibly to satisfy a dispense.

Exact default ordering/suggestion among multiple valid batches remains implementation design; the committed batch attribution is mandatory.

### P-086 — Consolidated Owner view

Owner can view:

- current stock by pharmacy unit;
- clinic-wide consolidated totals;
- base/package representation;
- unit/batch drill-down;
- low/out-of-stock state;
- near-expiry/expired state;
- automatic dispensing movements;
- stock additions;
- damage/loss/corrections;
- expiry disposition;
- transfers;
- request/decision history;
- before/delta/after values where quantity changes.

### P-087 — Automatic dispensing movement visibility

Normal prescription dispensing automatically creates the attributable negative stock movement in the active pharmacy unit and does **not** require Owner approval.

Physical dispensing remains the stock-changing event. The following do not retroactively change inventory:

- prescription finalization/replacement;
- pharmacy bill creation;
- payment recording/correction;
- bill void;
- Visit completion/cancellation;
- reporting.

Prior stock deduction remains attributed to the original dispense/prescription version even if that prescription later becomes Superseded.

## 19.2 Manual/non-dispensing inventory changes

### P-088 — Pharmacist inventory-change request

Pharmacist cannot directly apply an operational manual stock change.

For stock addition, damage, loss, expired-stock disposition, or quantity correction, the request records:

- category;
- pharmacy unit;
- medicine;
- affected batch/lot where relevant;
- entered quantity and unit;
- normalized base-unit effect;
- current captured stock baseline;
- projected resulting quantity;
- mandatory specific reason.

For a physical stock-count correction, staff may enter the intended counted/resulting quantity; the product derives and displays the delta against the captured baseline before submission.

A Pharmacist-proposed purchase/selling-price change uses the same controlled request pattern but is explicitly **non-quantity-changing**. It captures current value, proposed value, reason, requester, and time.

### P-089 — No change or reservation while Pending

Submitting an inventory-change request does not change stock, reserve stock, or alter price while Pending.

The live ledger remains authoritative while the request waits for Owner review.

A Pending request does not reserve or own the underlying quantity; ordinary dispensing and other valid movements may still occur, so Owner approval must revalidate current state.

### P-090 — Owner inventory-adjustment decision

Owner sees:

- requester;
- category/reason;
- pharmacy unit;
- medicine/batch;
- entered and normalized quantity;
- captured stock baseline;
- current stock at review time;
- proposed delta/result;
- relevant movement history.

Before applying a quantity change, Owner decision revalidates current unit/batch stock.

If intervening dispensing/adjustment/transfer means the reviewed proposed result is stale or would create invalid/negative stock:

- do not silently apply the old proposal;
- do not partially apply it;
- show stale-state feedback and require refreshed review/new proposal.

Approval creates the controlled movement and records requester, Owner authority, reason, before/delta/after, and timestamps. Rejection leaves inventory unchanged and remains historical.

For a non-quantity price request, revalidate the captured current price/value; stale baseline blocks silent overwrite.

#### Owner direct adjustment

Owner may perform the same non-dispensing adjustment directly without a redundant self-approval request.

Direct Owner adjustment still requires:

- category;
- mandatory reason;
- current state review;
- explicit confirmation;
- the same movement/old-new audit detail.

Admin authority alone does not grant this operational stock-control power.

### P-091 — Expired stock and alerts

Expired batch quantity is immediately unavailable for dispensing and excluded from valid available-stock calculations.

Expiry itself does **not** silently erase the physically recorded quantity from inventory history.

The expired batch remains visible as expired/unavailable until its removal/disposition is recorded through the controlled Owner-authorized inventory-adjustment flow.

Owner receives expiry visibility/notification. Physical disposal remains outside CRM V1.

Low-stock and near-expiry state use configured medicine/inventory thresholds rather than hard-coded global values. Consolidated Owner views retain unit/batch drill-down.

## 19.3 Pharmacy transfer

### P-092 — Linked transfer request

A Pharmacist transfer request identifies:

- source pharmacy unit;
- different destination pharmacy unit;
- medicine;
- concrete batch/lot where relevant;
- entered quantity/unit;
- normalized base-unit quantity;
- captured transferable source availability;
- mandatory specific reason.

Submitting the request:

- changes neither source nor destination stock;
- creates no hidden stock reservation;
- retains one linked transfer identity/reference.

Source and destination must differ. Requested quantity must be positive and cannot exceed the captured transferable valid source quantity at submission.

### P-093 — Owner transfer decision and atomic business outcome

Before decision, Owner revalidates:

- request still Pending/current;
- source/destination;
- current source valid transferable quantity;
- medicine/batch identity;
- normalized quantity.

If source stock fell below requested transferable quantity, the request is stale/non-actionable until refreshed/replaced. V1 does not partially approve a stale transfer.

Approval is one atomic business event under one transfer ID/reference:

- source unit decreases by the normalized quantity;
- destination unit increases by the same normalized quantity;
- destination movement preserves the physical stock's batch/lot/expiry/manufacturer identity and transfer linkage;
- both movements remain separately visible in their unit ledgers and jointly traceable through the transfer.

Rejection/stale failure changes neither side.

Transfer does not rewrite prior source movement history or create an unrelated destination batch identity.

## 19.4 Inventory state-change and retry safety

For inventory adjustment submission, Owner direct adjustment, adjustment approval, transfer submission, and transfer approval:

- disable duplicate final submit while pending;
- revalidate current request/stock baseline before apply;
- if outcome is unknown, refresh/check current request and ledger state before blind retry;
- stale/resolved request cannot apply again;
- quantity-changing outcome cannot produce negative stock.

Exact transaction, lock, and movement-storage mechanisms remain technical design.

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

### P-096 — Effective authority attribution for multi-role users

When the same account holds multiple roles, the product must preserve both the human identity and the effective authority/workspace used for each material action. Audit semantics must distinguish at minimum the actor/account, effective role or workspace, action, affected target, and time.

If the same human legitimately performs different sides of a workflow through different assigned roles—for example requesting a Visit cancellation as Doctor and later approving it as Owner—the two actions remain separately attributable to the two authority contexts rather than being collapsed into one generic user action.

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


---

# 33. Companion Product Specifications

The following documents are normative companions to this PRD while the PRD remains in DRAFT:

1. **Document 06 — PRD Acceptance and Traceability**  
   Defines end-to-end acceptance, role/authority acceptance, negative release blockers, and BRD lineage.

2. **Document 07 — Information Architecture and Screen Specification**  
   Defines the V1 screen inventory, role navigation, screen-level contracts, entry conditions, visible data, primary/secondary actions, states, and transitions.

3. **Document 08 — Interaction and Form Behavior Specification**  
   Defines reusable product interaction rules for search, forms, payment capture, reason capture, approvals, queue operations, medicine selection, dispensing, inventory, tables, status language, errors, and unsaved/stale state handling.

These companion specifications may add **DERIVED PRODUCT DESIGN** detail but may not alter locked BRD business behavior.

If a companion document conflicts with this PRD, the PRD controls unless the PRD itself conflicts with the locked BRD, in which case the BRD controls.
