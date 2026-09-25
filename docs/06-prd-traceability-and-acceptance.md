# Hospital CRM — PRD Acceptance and Traceability

## Document Control

| Field | Value |
| --- | --- |
| Document | PRD Acceptance and Traceability |
| Version | 0.16 |
| Status | DRAFT |
| Date | 2026-09-20 |
| Parent | Product Requirements Document v0.16 |
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

## AC-031 — Unit-specific bill uses only unbilled committed supply

**Given** Pharmacy Unit A has committed dispensing records for a Visit  
**When** Pharmacist creates a bill  
**Then** only Unit A quantities that were actually supplied and are not already assigned to another active/non-voided bill lineage may be billed.

Supports: P-077, P-076, FR-057–FR-059, FR-118.

## AC-032 — Bill snapshot is not rewritten

**Given** a pharmacy bill was created  
**When** medicine price/configuration later changes or Doctor replaces the prescription  
**Then** the existing bill retains its original line/quantity/price/total/unit/source-dispensing snapshot and is not silently recalculated.

Supports: P-077, REM-046, IX Section 16.

## AC-033 — Pharmacy payment correction is baseline-safe

**Given** Pharmacy submitted a payment-correction request with a captured current baseline  
**When** the effective payment record changes before Owner decision  
**Then** the stale request cannot overwrite the newer payment record; bill lines/total remain unchanged and Paid -> Unpaid is presented as record correction, not refund.

Supports: P-078, P-038–P-039, OD-033, OWN-03, IX Sections 8 and 16.

## AC-034 — Payment does not gate pharmacy completion

**Given** all intended clinic-pharmacy fulfilment is finished and all committed dispensing is billed  
**And** a pharmacy bill remains Unpaid  
**When** Pharmacist explicitly completes clinic-pharmacy fulfilment  
**Then** the Visit may move Sent to Pharmacy -> Completed while the Unpaid bill remains visible and actionable for later payment recording.

Supports: P-078–P-080, PHA-04, IX Section 16.

## AC-035 — Multi-unit and unsupplied remainder completion

**Given** one Visit was supplied by multiple pharmacy units and still has prescribed quantity not supplied by the clinic  
**When** every unit's committed dispensing is accounted for in its own bill, no actionable substitution remains, and the remainder is explicitly left unsupplied/outside  
**Then** the Visit may complete without a back-order and without requiring every prescribed unit to have been supplied.

Supports: P-069–P-076, PHA-04, REM-051, IX Section 16.

## AC-036 — Visit closure blocks new billing but preserves bill administration

**Given** a Visit is Completed or Cancelled/Voided  
**Then** new dispensing and new pharmacy-bill creation are unavailable.  
**And** an already-created non-voided bill may still be marked Paid after external payment, corrected through Owner-controlled payment correction, or voided through Owner control without reopening the Visit.

Supports: P-047, P-077–P-084, REM-037, IX Section 16.

## AC-037 — Pending void uses current payment state

**Given** a bill-void request is Pending and the active bill later becomes Paid  
**When** Owner reviews the request  
**Then** the Owner sees/revalidates the current Paid state and may approve void only with explicit no-refund consequence; payment history remains preserved.

Supports: P-081–P-084, PHA-05, OWN-03, IX Sections 9 and 16.

## AC-038 — Pharmacy billing state changes are retry-safe

**Given** the outcome of bill creation, Mark Paid, Visit completion, void request/decision, or payment correction is unknown  
**When** the user retries  
**Then** the product first retrieves current effective state and prevents a duplicate effective record/action.

Supports: P-077–P-084, IX Sections 8, 16 and 25.
## AC-039 — Inventory changes are ledger movements, not silent stock edits

**Given** an authorized quantity-changing inventory event occurs  
**Then** the product preserves an attributable movement with unit/medicine/batch where relevant, normalized quantity, before/delta/after, source/actor/time rather than silently overwriting current stock.

Supports: P-085, P-087–P-090, FR-071, BR-037.

## AC-040 — Package entry preserves base-unit effect

**Given** staff enters a configured package quantity for adjustment or transfer  
**Then** the product shows the conversion and normalized base-unit effect before submission and preserves that movement meaning historically.

Supports: P-085, FR-086, OD-015.

## AC-041 — Pending inventory adjustment changes nothing

**Given** Pharmacist submits a stock addition/loss/damage/expiry/correction request  
**When** request is Pending  
**Then** stock is neither changed nor reserved and the live ledger remains usable for legitimate dispensing/movements.

Supports: P-088–P-089, FR-068, OD-030.

## AC-042 — Stale stock adjustment cannot silently apply

**Given** an inventory request captured a stock baseline  
**And** intervening dispensing/adjustment/transfer changed the relevant unit/batch stock  
**When** Owner attempts approval  
**Then** the product revalidates the baseline/result and blocks stale or negative application instead of partially applying it.

Supports: P-090, OWN-03, PHA-09, IX Section 17.

## AC-043 — Owner direct adjustment remains controlled

**Given** Owner directly records a non-dispensing stock adjustment  
**Then** no redundant self-approval request is created, but category, mandatory reason, current-state review, confirmation, and full before/delta/after audit are required.

Supports: P-090, OWN-04, BR-031, BR-037.

## AC-044 — Expiry blocks availability without erasing quantity history

**Given** a batch reaches expiry  
**Then** it is excluded from valid dispensing/available stock immediately, remains visible as expired recorded quantity, and leaves inventory only through controlled disposition/adjustment.

Supports: P-091, FR-089, BR-032.

## AC-045 — Transfer Pending reserves nothing

**Given** Pharmacist submits an inter-pharmacy transfer request  
**When** it is Pending  
**Then** neither ledger changes and source stock is not reserved; later approval must revalidate current transferable source quantity.

Supports: P-092–P-093, FR-096, BR-041.

## AC-046 — Approved transfer is linked and atomic

**Given** a current valid transfer request  
**When** Owner approves  
**Then** source decreases and destination increases by the same normalized quantity under one transfer identity, preserving batch continuity and changing neither side partially.

Supports: P-093, FR-096, BR-041.

## AC-047 — Billing/void does not mutate stock

**Given** medicine was already dispensed  
**When** bill is created, paid, corrected, voided, or Visit later completes/cancels  
**Then** the original stock movement remains unchanged unless a separate Owner-authorized inventory adjustment is performed.

Supports: P-087, P-084, REM-056, BR-056.

## AC-048 — Inventory state changes are retry-safe

**Given** an adjustment/direct-adjustment/transfer submit or approval has unknown outcome  
**When** user attempts again  
**Then** the product checks request/ledger state first and prevents duplicate or double-applied stock movement.

Supports: P-088–P-093, IX Sections 17 and 25.

## AC-049 — Approval Center uses type-specific actions

**Given** Owner opens current work queue  
**Then** standard decision requests expose Approve/Reject, staff password reset exposes Set Temporary Credential, and direct Owner waiver/inventory actions are not represented as fake Pending self-approvals.

Supports: P-094–P-095, OWN-02–OWN-03, OWN-09.

## AC-050 — Same human can act under distinct legitimate authorities

**Given** one account is Doctor + Owner  
**When** the user requests a Visit cancellation as Doctor and later approves it under Owner authority  
**Then** both actions are permitted when otherwise valid and remain separately attributed to the same human with distinct effective authority/workspace.

Supports: P-096, REM-005.

## AC-051 — Waiver decision is current-state safe

**Given** consultation-waiver request is Pending  
**When** Visit becomes Paid/Waived before Owner decision  
**Then** old request becomes non-actionable/stale. Rejection of a still-valid request leaves Unpaid; Direct Owner Waiver remains a separate immediate Owner action.

Supports: P-094, REM-029.

## AC-052 — Cancellation request may become stale while workflow progresses

**Given** Visit cancellation request is Pending  
**When** Visit progresses to Completed before Owner decision  
**Then** cancellation cannot be approved through the old request, which remains historical/non-actionable.

Supports: P-094, P-046–P-047, REM-038.

## AC-053 — Bill-void decision uses latest payment state

**Given** bill-void request is Pending  
**And** bill becomes Paid before Owner decides  
**When** Owner reviews  
**Then** current Paid state is shown/revalidated and approval remains possible only with explicit no-refund consequence; stock is not restored.

Supports: P-094, P-081–P-084, REM-057.

## AC-054 — Payment correction cannot overwrite changed baseline

**Given** payment-correction request captured effective payment baseline  
**When** payment record changes before decision  
**Then** old request is stale/non-applicable and cannot overwrite newer truth; Pharmacy bill lines/total are not correction fields.

Supports: P-094, P-038–P-039, REM-030, REM-058.

## AC-055 — Inventory approval uses captured and current stock

**Given** inventory-adjustment request is Pending  
**When** relevant unit/batch stock changes  
**Then** Owner sees captured and current state and stale/invalid/negative application is blocked. Owner direct adjustment remains outside fake self-approval workflow.

Supports: P-094, P-090, REM-063.

## AC-056 — Transfer decision revalidates current source

**Given** transfer request is Pending  
**When** source quantity changes before Owner decision  
**Then** Owner sees current source quantity; insufficient source blocks approval with no partial transfer, while sufficient current source may still be explicitly approved as one linked transfer.

Supports: P-094, P-093, REM-063.

## AC-057 — Password reset resolves by credential set

**Given** eligible non-Owner staff reset request is Pending  
**When** Owner sets temporary credential  
**Then** request becomes Resolved, no Approve/Reject decision is invented, old password remains undisclosed, and reset does not enable a disabled account or alter roles.

Supports: P-094, P-011–P-013, REM-010–REM-011.

## AC-058 — Owner action unknown outcome is checked before retry

**Given** an Owner approval/reset/direct exception action returns unknown outcome  
**When** Owner retries  
**Then** product refreshes request/target/effective state and prevents a second effective action.

Supports: P-094–P-096, IX Sections 9–10 and 25.

## AC-059 — Admin cannot mutate Owner authority

**Given** Administrator opens staff management
**Then** Owner grant/revoke/disable actions are unavailable and direct protected action is denied.

Supports: P-098–P-099, FR-069, BR-057.

## AC-060 — Newly granted Owner capability is TOTP-gated

**Given** Owner grants Owner authority to an existing non-Owner account
**When** that user has not satisfied Owner TOTP enrollment/verification requirement
**Then** Owner workspace/capability remains unavailable while existing permitted non-Owner roles may continue.

Supports: P-099, REM-012, BR-043.

## AC-061 — Revoked role cannot survive in an open workspace

**Given** user has a protected workspace open
**When** that role is revoked
**Then** next protected action/navigation/permission refresh denies that authority and returns user to permitted context.

Supports: P-098, REM-006, IX Section 29.

## AC-062 — Disable/re-enable remains separate from credentials and roles

**Given** non-Owner staff is disabled and later re-enabled
**Then** historical roles remain unless separately changed, reset credential state is not silently changed, and protected use resumes only after fresh sign-in.

Supports: P-100, REM-013.

## AC-063 — Account changes preserve submitted request history

**Given** user submitted a valid business request
**When** requester account is later disabled or role is removed
**Then** request/audit history retains original requester/effective-authority attribution and is not silently deleted.

Supports: P-097–P-100, REM-067.

## AC-064 — Owner-role grant invalidates non-Owner reset path

**Given** non-Owner password reset is Pending
**When** target gains Owner authority
**Then** old reset becomes non-actionable and cannot bypass Owner recovery/TOTP policy.

Supports: P-099–P-100, REM-067.

## AC-065 — Consultation fee configuration is prospective

**Given** Visit already exists with applied fee snapshot
**When** Owner changes consultation fee configuration
**Then** existing Visit amount is unchanged and only future Visit creation uses new configured fee.

Supports: P-101, REM-031.

## AC-066 — Pharmacy price/tax configuration does not recalculate old bill

**Given** pharmacy bill already exists
**When** Owner changes price/tax configuration
**Then** bill retains frozen line/price/tax/total snapshot.

Supports: P-101, REM-059.

## AC-067 — Package conversion change preserves ledger truth

**Given** historical stock movement used prior package conversion
**When** authorized conversion configuration changes
**Then** current base-unit stock and historical normalized movement remain unchanged; new conversion applies prospectively.

Supports: P-101, REM-064.

## AC-068 — Medicine history survives catalogue retirement

**Given** medicine has historical prescription/dispense/bill use
**When** authorized user archives/disables medicine for future selection
**Then** historical references remain readable and are not rewritten/deleted.

Supports: P-101, BR-023–BR-024.

## AC-069 — Pharmacy-unit retirement preserves ledger history

**Given** pharmacy unit has historical stock/dispensing/billing
**When** Owner disables/archives unit
**Then** unit is unavailable for new operational use but remains visible in historical drill-down.

Supports: P-101, BR-040–BR-041.

## AC-070 — Material configuration change is auditable and retry-safe

**Given** account/role/configuration change is state-changing
**When** result is unknown or actor authority changed
**Then** product checks current effective state/authority before another final attempt and preserves prior/resulting configuration audit without secret values.

Supports: P-097–P-101, IX Section 42.

## AC-071 — Patients seen anchors to first completion event

**Given** Visit reaches Consultation Completed
**And** clinical amendment or later cancellation occurs afterward
**Then** patients-seen reporting counts that Visit once at its original first completion event and does not create/remove the historical seen event.

Supports: P-102, REM-043.

## AC-072 — Successful waiting journey is deterministic

**Given** Visit is reassigned/Unresponded/moved within the same queue journey
**Then** average wait retains the original queue-entry timestamp.

**Given** Visit is removed from queue membership and later re-enters before first With Doctor
**Then** the successful wait uses the later queue-entry segment immediately preceding first With Doctor.

Supports: P-102, REM-039.

## AC-073 — Consultation revenue uses effective Paid record

**Given** original consultation payment has later approved correction
**Then** revenue uses only the current effective Paid/Unpaid state/value, excludes Waived, and does not double-count preserved prior records.

Supports: P-102, REM-032.

## AC-074 — Paid cancellation remains recorded revenue without refund

**Given** consultation remains effectively Paid
**When** Visit is later Cancelled/Voided under no-refund V1 rules
**Then** recorded consultation revenue remains included while cancellation is reported separately.

Supports: P-102, BR-048, BR-050.

## AC-075 — Paid pharmacy bill later voided stays recorded money received

**Given** pharmacy payment remains effectively Paid
**When** bill is later Voided without refund
**Then** recorded pharmacy revenue still includes the frozen Paid bill amount and the void is separately visible; an approved correction to Unpaid removes it from effective Paid revenue.

Supports: P-102, REM-060.

## AC-076 — Medicine sales use dispensing, not prescription or stock movement

**Given** medicine was partially dispensed and other inventory adjustments/transfers occurred
**Then** medicine-sales quantity uses actual committed dispensing only; unsupplied, transfer, addition, loss, correction and expiry movements do not count as sales.

Supports: P-102, REM-065.

## AC-077 — Current stock derives unit ledgers and separates expired quantity

**Given** unit ledgers include valid and expired recorded stock
**Then** current available stock excludes expired/unavailable quantity, consolidated stock derives unit positions, and unit/batch drill-down remains attributable.

Supports: P-102, REM-065.

## AC-078 — Internal transfer is not clinic-wide stock gain or sale

**Given** approved linked transfer moves quantity from Pharmacy A to Pharmacy B
**Then** unit positions change by -Q/+Q, clinic consolidated quantity nets to zero, and the transfer contributes neither medicine sale nor stock addition.

Supports: P-102, REM-065.

## AC-079 — Most prescribed does not double-count Superseded versions

**Given** Doctor replaces finalized prescription
**Then** standard most-prescribed aggregation uses the Visit's current final prescription version and does not independently count the Superseded version as another current prescription contribution.

Supports: P-102, P-064–P-067.

## AC-080 — Request outcome report distinguishes effective from non-effective work

**Given** approval/request history contains Pending, Rejected, Stale, Approved, Resolved and direct Owner actions
**Then** effective waiver/cancellation metrics count only actual effective outcomes and direct actions are not double-counted as both request and approval.

Supports: P-102, REM-068.

## AC-081 — Archived entity remains historical report dimension

**Given** staff, medicine or pharmacy unit is later disabled/archived
**Then** prior report/audit attribution remains linked to the stable historical identity and can still be filtered/drilled into.

Supports: P-102–P-104, REM-070.

## AC-082 — Returning patient uses prior Visit on same Patient ID

**Given** a new Visit is created for a Patient ID with at least one earlier Visit
**Then** that Visit qualifies as returning-patient activity; Possible Duplicate profiles remain separate identities until merge exists.

Supports: P-102, OD-029.


## AC-083 — Approved substitute reports the medicine actually supplied

**Given** a prescribed medicine was replaced for fulfilment by an approved substitute
**And** Pharmacy actually dispensed that substitute from a specific pharmacy unit
**When** medicine-sales reporting is calculated
**Then** supplied quantity/value is attributed to the actual substitute medicine and dispensing unit, the original prescribed item remains lineage/context only for that substituted quantity, and unsupplied remainder contributes no sale.

Supports: P-102, REM-053.


## AC-084 — Audit preserves effective authority for same-human multi-role actions

**Given** one human performs related actions under different assigned roles
**When** those actions are audited
**Then** audit preserves the same human/account identity and separately records the effective role/workspace for each material event.

Supports: P-108, P-096, REM-007.

## AC-085 — Audit access does not reveal protected source content

**Given** Owner/Admin can view audit metadata for a clinical event
**When** that viewer lacks Doctor clinical-content authority
**Then** audit may show safe event metadata but does not reveal unrestricted diagnosis/notes or turn an audit link into clinical access.

Supports: P-110, REM-024, REM-044, REM-069.

## AC-086 — Authentication secrets never enter audit history

**Given** password reset, TOTP enrolment, recovery-code use or recovery-code regeneration occurs
**Then** audit may record safe event metadata but never password/reset credential/TOTP/recovery-code values.

**And** a used recovery code cannot authenticate again and regeneration invalidates the previous set.

Supports: P-110, REM-014, REM-017.

## AC-087 — Historical identity survives later archive or disablement

**Given** an actor/entity later becomes disabled or archived
**When** historical events are viewed
**Then** prior attribution remains linked to the stable historical identity and cannot be deleted/reassigned through normal UI.

Supports: P-107–P-109.

## AC-088 — Revoked authority cannot survive in an open tab

**Given** protected workspace is already open
**When** account is disabled or the active role is revoked
**Then** next protected navigation/final action re-evaluates authority, denies stale access and returns the user to an allowed/sign-in context.

Supports: P-111–P-112, REM-008, REM-016, REM-071.

## AC-089 — Separate tabs retain independent permitted contexts

**Given** the same account has two browser tabs in different permitted workspaces
**When** workspace changes in one tab
**Then** the other tab does not silently switch authority; every protected action still checks that tab's explicit context plus current account authority.

Supports: P-112, REM-009.

## AC-090 — Unknown state-changing outcome is checked before retry

**Given** a final state-changing request returns an unknown client outcome
**When** the user tries again
**Then** product first retrieves current target/request/effect state; if the intended effect already exists it is recovered rather than created twice.

Supports: P-111, REM-015, REM-023, REM-033, REM-054, REM-061, REM-066, REM-069, REM-071.

## AC-091 — Sequenced partial success preserves confirmed effect

**Given** Mark Paid succeeds
**And** subsequent queue insertion fails
**When** the workflow resumes
**Then** Paid remains effective and retry/recovery attempts queue insertion from current truth without recording payment again or rolling it back.

Supports: P-111, REM-033.

## AC-092 — Atomic business operation cannot recover as one-sided effect

**Given** dispense + stock deduction or a linked two-sided stock transfer has uncertain outcome
**When** recovery occurs
**Then** product checks the whole atomic business operation/reference before allowing another user attempt and never knowingly repeats only one side.

Supports: P-111, REM-054, REM-066.

## AC-093 — Patient creation rechecks duplicate/current state before retry

**Given** registration reaches final submit
**When** duplicate candidates changed or create result is unknown
**Then** current duplicate/effective Patient state is checked before another create attempt so accidental duplicate identity is not created.

Supports: P-111–P-112, REM-023.

## AC-094 — Demographic correction cannot overwrite changed baseline

**Given** demographic correction captured an earlier Patient value
**When** that value changes before Doctor decision
**Then** old proposal is stale/non-applicable and cannot overwrite the newer Patient truth.

Supports: P-112, REM-024–REM-025.

## AC-095 — Concurrent queue actions use current Visit truth

**Given** two actors attempt incompatible Call/Start/Reassign/Unresponded/cancellation/completion-related transitions
**When** one valid transition commits first
**Then** later action revalidates current Visit/queue/Doctor state and blocks/refreshes if no longer valid while preserving prior history.

Supports: P-112, REM-040.

## AC-096 — Stale clinical draft cannot overwrite newer clinical truth

**Given** Doctor has an older consultation draft open
**When** another save/revision or effective cancellation changes current clinical state
**Then** old save cannot overwrite newer truth and local unsaved input is preserved for review/recovery rather than silently merged or discarded.

Supports: P-112, REM-044.

## AC-097 — Prescription finalization/replacement preserves one current version

**Given** duplicate/stale finalization or replacement attempts occur
**Then** product does not create multiple current Finalized versions; stale replacement cannot supersede a newer current version and version lineage remains historical/auditable.

Supports: P-111–P-112, REM-049.

## AC-098 — Dispense revalidates lineage, allowance and stock across units

**Given** Pharmacy has a dispense action open
**When** prescription/version, substitution, remaining allowance, Visit state or unit stock changes before commit
**Then** final dispense uses current truth, blocks invalid stale supply, prevents cumulative overfill across units, and keeps dispense + stock deduction one effective operation.

Supports: P-111–P-112, REM-054.

## AC-099 — Billing/payment/void actions cannot duplicate or overwrite newer truth

**Given** bill/payment/correction/void/Visit-completion state changes concurrently
**Then** bill creation cannot re-bill committed supply, payment is not duplicated, stale correction cannot overwrite newer payment, void uses latest allowed payment state, and completion rechecks participating pharmacy fulfilment.

Supports: P-111–P-112, REM-061.

## AC-100 — Inventory concurrency preserves non-negative linked ledgers

**Given** stock changes after an adjustment/transfer form was loaded
**When** final action occurs
**Then** current stock is revalidated, stale/negative result is blocked, transfer remains linked source -Q/destination +Q, and the same movement/reference is not knowingly applied twice.

Supports: P-111–P-112, REM-066.

## AC-101 — Owner request/reset/direct action cannot execute twice

**Given** Owner work is already resolved/stale or target baseline changed
**When** another session attempts the old action
**Then** request/reset/decision is non-actionable or re-reviewed under its type-specific current-state rule; direct Owner actions remain separately attributed rather than becoming self-approval.

Supports: P-108, P-111–P-112, REM-069.

## AC-102 — Account/configuration safety uses current authority

**Given** role/account/configuration action is open
**When** Owner readiness, zero-active-Owner protection, account status, role authority or target configuration changes
**Then** current authority/state is revalidated before commit and unknown outcome is checked before retry without exposing auth secrets.

Supports: P-110–P-112, REM-071.

## AC-103 — Reporting remains read-only, reproducible and access-scoped

**Given** a user refreshes/filters/drills into a report
**Then** reporting does not mutate source records, current effective results remain derivable from preserved source/history, current role scope is enforced, and clinic-local date/time basis is applied consistently.

Supports: P-107–P-112, REM-073.


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

A user with more than one assigned role uses one identity and gains the permitted workspaces of those roles. The product keeps the effective workspace/authority explicit; combining roles does not convert one role into another.

Sources: FR-064, FR-098, BR-039, OD-022, OD-023.

---

# 5. Authentication Acceptance

## AU-001 — Owner 2FA mandatory for the whole account login

**Given** an account contains Owner authority  
**When** the password is valid  
**Then** the account cannot enter Doctor, Owner, or any other normal workspace until the Owner second-factor requirement is satisfied.

Sources: FR-085, BR-043, OD-021.

## AU-002 — Staff no 2FA

**Given** an account has no Owner authority  
**Then** Doctor/Reception/Pharmacist/Admin or other valid non-Owner role combinations are not required to complete 2FA in V1.

Sources: FR-100, BR-043, OD-021.

## AU-003 — Disabled account

**Given** an account is disabled  
**Then** it cannot enter the application even with otherwise valid normal/reset credentials and historical actions remain attributable.  
**And** if disablement occurs during an active session, protected use stops on the next protected action/navigation or authentication-state refresh.

Sources: OD-022 staff lifecycle; supports P-015 and G1 stale-authority handling.

## AU-004 — Initial Owner TOTP enrollment gate

**Given** valid password authentication for an Owner-containing account with no enrolled TOTP factor  
**Then** the product requires TOTP enrollment and successful generated-code verification before normal workspace entry.  
**And** recovery codes are shown only after factor verification.

Sources: FR-085, FR-104, BR-043, BR-046, OD-021.

## AU-005 — Owner authority added during an active non-Owner session

**Given** an authenticated session did not contain Owner authority when login completed  
**And** Owner authority is subsequently granted  
**Then** Owner-capable workspace/actions do not become usable until the Owner second-factor requirement is satisfied for that session.

Supports: P-009, BR-043, G1 authority-context rules. Exact step-up/session mechanism remains technical.

## AU-006 — Forgot Password does not enumerate accounts

**Given** an unauthenticated user submits Forgot Password  
**Then** the response does not reveal whether the identifier exists, is disabled, or contains Owner authority.  
**And** only an eligible enabled non-Owner account creates/retains an actionable Owner reset request.

Sources: FR-101, BR-044, OD-021.

## AU-007 — One actionable staff reset request

**Given** an eligible non-Owner staff account already has a Pending reset request  
**When** Forgot Password is submitted again  
**Then** the product does not create another simultaneously actionable Owner reset request.

Supports: P-011 derived request-safety behavior.

## AU-008 — Owner reset resolves request without changing account status

**Given** Owner acts on a Pending staff reset request  
**When** a temporary/reset credential is successfully set  
**Then** the request becomes Resolved, the old password remains undisclosed, roles remain unchanged, and disabled/enabled account state is not changed by the reset.

Sources: FR-102–FR-103, BR-044–BR-045, OD-021–OD-022.

## AU-009 — Forced password change blocks normal work

**Given** non-Owner staff signs in with a valid Owner-reset credential  
**Then** normal workspace navigation/content remains unavailable until replacement password save succeeds.  
**And** after success the temporary credential is no longer valid and normal G1 workspace routing resumes.

Sources: FR-103, BR-045, OD-021.

## AU-010 — Recovery code is a one-time second factor

**Given** an Owner has a valid unused recovery code  
**When** password authentication has already succeeded  
**Then** the recovery code may satisfy the second-factor step for that login and becomes invalid after use.  
**And** recovery-code regeneration invalidates the previous set.

Sources: FR-104, BR-046, OD-021.

## AU-011 — Authentication secrets are not audit content

**Given** authentication/recovery actions are logged or shown in history  
**Then** audit may identify safe metadata such as actor, target, action, outcome, and time, but must not expose passwords, reset credentials, TOTP secrets/codes, or recovery-code values.

Supports: FR-103–FR-104, BR-045–BR-046 and the locked secret-handling intent.

## AU-012 — Normal workspace switching does not repeat authentication

**Given** the current session has already satisfied all authentication requirements for its assigned roles  
**When** the user switches among already-permitted workspaces  
**Then** the switch does not require a second login or repeated TOTP solely because of the workspace change.

Supports: P-003, P-009, G1 multi-role contract.

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
20. allow Owner account to bypass required TOTP;
21. allow an Owner-containing account to enter a non-Owner workspace before required Owner 2FA completes;
22. treat Forgot Password as self-service password reset;
23. expose whether an arbitrary Forgot Password identifier exists, is disabled, or is an Owner account;
24. create duplicate simultaneously actionable staff reset requests for repeated submissions;
25. allow a reset credential to enter a normal workspace before forced password replacement succeeds;
26. let password reset implicitly re-enable a disabled account;
27. allow a used recovery code or a code from an invalidated recovery-code set to authenticate;
28. expose password/TOTP/recovery-code secret values in normal audit/history;
29. allow a disabled account's already-open session to continue protected use after disablement is detected;
30. auto-select or auto-merge a patient solely from similarity/phone matching;
31. create a new Patient after current duplicate candidates were surfaced without an explicit existing-vs-Possible-Duplicate decision;
32. treat a Possible Duplicate marker as a reason to block normal Patient/Visit use;
33. expose unrestricted clinical history through Reception patient search/profile;
34. let Reception directly overwrite established demographics after Patient creation;
35. allow a stale demographic correction to overwrite a newer effective value;
36. allow missing physical file to block digital patient use or trigger a new Patient ID;
37. blindly retry an unknown-outcome Patient creation in a way that may create a duplicate identity;
38. allow Patient ID to be edited/replaced through demographic correction;
39. create/replace Patient identity as part of Visit creation;
40. require Doctor assignment before a Visit can exist, despite the locked unassigned-active-Visit case;
41. queue a Visit unless both Doctor assignment and Paid/Waived eligibility are satisfied;
42. erase/rollback a confirmed Paid record merely because a combined queue step failed;
43. allow Doctor waiver authority only through the queue, making the locked pre-queue waiver request unreachable;
44. create duplicate Pending waiver requests for the same Visit;
45. approve a Pending waiver after the Visit has already become Paid;
46. make Owner direct waiver wait for a second approval;
47. apply a payment correction against a changed/stale payment baseline;
48. treat Paid-to-Unpaid correction as a refund or delete prior queue/clinical history;
49. silently rewrite an existing Visit amount when fee configuration changes;
50. blindly retry unknown-outcome Visit/payment creation in a way that may duplicate financial/Visit records;
51. count assigned-but-Unpaid pre-queue Visits as actual queue members;
52. start consultation directly from Waiting without the explicit Called -> With Doctor transition;
53. reassign With Doctor/later Visit or insert reassigned Visit at arbitrary priority position;
54. let old Doctor decide a Visit-linked demographic correction after reassignment;
55. erase Unresponded event when current state returns to Waiting;
56. keep a Waiting/Called Visit actively queued after its effective financial state becomes Unpaid;
57. delete queue history when financial correction removes current queue membership;
58. treat Pending cancellation as automatic workflow freeze;
59. approve cancellation after Visit reached Completed;
60. allow duplicate simultaneously actionable cancellation requests;
61. cancel/void by deleting existing clinical/prescription/dispensing/financial history or implying refund;
62. apply stale queue/cancellation action after current state or Doctor assignment changed;
63. allow clinical authoring before explicit With Doctor state;
64. make Save Draft implicitly complete the consultation;
65. allow stale Doctor draft save to overwrite newer saved content/state;
66. complete consultation without required complaint/problem and assessment/diagnosis;
67. keep ordinary editable clinical fields after Consultation Completed;
68. automatically merge Possible Duplicate candidate clinical histories;
69. approve a stale demographic-correction request after value/reviewer changed;
70. directly correct demographics without preserving old/new Doctor/time attribution;
71. destructively edit a completed consultation instead of creating a revision;
72. let a clinical amendment reopen queue/payment/cancellation workflow;
73. treat an unfinished cancelled draft as a completed consultation amendment target;
74. continue active clinical writes after effective cancellation;
75. expose unrestricted clinical content solely through Owner/Admin/Reception/Pharmacist authority;
76. treat an unfinalized prescription draft as pharmacy-ready;
77. auto-finalize prescription when consultation completes;
78. auto-complete consultation when prescription finalizes;
79. send a With Doctor Visit to Pharmacy merely because prescription was finalized;
80. allow finalization with incomplete prescription rows or unresolved quantity;
81. recompute a finalized version's ** markers from current stock during reprint;
82. edit a Finalized prescription in place instead of replacement;
83. replace a stale/non-current prescription version without refresh;
84. erase prior dispensing/stock/billing when prescription is replaced;
85. let Pharmacy silently continue a Superseded prescription as the current source;
86. allow new active prescription finalization/replacement after effective Visit cancellation or Completed state;
87. let a clinical amendment silently change prescription content;
88. invent a no-prescription Consultation Completed -> Completed shortcut outside locked V1;
89. allow dispensing from a Finalized prescription before Visit reaches Sent to Pharmacy;
90. silently select a historical/wrong Visit or Superseded prescription for dispensing;
91. expose unrestricted diagnosis/consultation notes/Doctor longitudinal history to Pharmacist;
92. reset prior dispensed quantity when a prescription is replaced;
93. allow cumulative original + substitute fulfilment beyond the current permitted allowance;
94. allow a stale pharmacy unit/session to over-dispense after another unit supplied first;
95. create stock deduction without matching dispensing record or dispensing record without its stock effect;
96. select/dispense expired stock;
97. create a back-order/reservation from unsupplied remainder;
98. let a stale substitution request survive prescription replacement/cancellation/changed remaining quantity;
99. continue dispensing after effective Visit cancellation;
100. carry unsaved dispense quantities silently across pharmacy-unit switch;
101. mark Visit Completed merely because dispensing occurred.

---

# 7. Product Requirement to BRD Traceability

| PRD Area | Product Requirements | Primary BRD Sources |
| --- | --- | --- |
| Workspace / multi-role context | P-001–P-005, P-096 | FR-064, FR-069, FR-097–FR-100, BR-034–BR-039, OD-022, OD-023 |
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
| Owner approvals | P-094–P-095 | FR-015, FR-068, FR-081, FR-097, FR-101–FR-102, FR-105–FR-107, FR-114 |
| Administration | P-097–P-101 | FR-064, FR-069, FR-117, OD-022 |
| Reporting | P-102–P-106 | BRD Section 8, OD-029 |
| Audit / history | P-107–P-110 | FR-070–FR-074, BR-023–BR-024 |
| Cross-product state / audit / safety | P-006–P-007, P-107–P-112 | FR-070–FR-074, BR-023–BR-024 plus locked non-destructive controls |
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


---

# 11. Screen and Interaction Acceptance

These scenarios validate Documents 07 and 08 without introducing new business policy.

## UXA-001 — Search-first Reception home

**Given** Reception opens the workspace  
**Then** patient search is immediately available and New Patient is a deliberate secondary action rather than the default automatic path.

Supports: REC-01, REC-02.

## UXA-002 — Shared phone is not unique proof

**Given** multiple patient records share one phone number  
**When** Reception searches the phone  
**Then** the UI presents separate candidates and does not auto-select one record.

Supports: REC-02, IX Section 5.

## UXA-003 — Payment method selection is not payment confirmation

**Given** a consultation or pharmacy payment form  
**When** user selects UPI, Cash, Card, or Other  
**Then** payment state does not become Paid until the explicit final payment action is performed.

Supports: REC-05, PHA-04, IX Section 8.

## UXA-004 — Other payment requires description

**Given** user selects Other  
**Then** a payment-method description becomes required before final payment confirmation.

Supports: REC-05, PHA-04.

## UXA-005 — Approval pending does not mutate source

**Given** a waiver, bill void, payment correction, inventory adjustment, or transfer request is Pending  
**Then** the underlying effective business state remains unchanged unless the locked workflow explicitly states otherwise.

Supports: OWN-02, OWN-03, IX Section 9.

## UXA-006 — Stale approval protection

**Given** Owner opened a Pending approval  
**And** another authorized session resolved it  
**When** Owner attempts to decide using the stale view  
**Then** the product blocks duplicate decision and requires refresh/review.

Supports: OWN-03, IX Section 25.

## UXA-007 — Unresponded state is operationally visible

**Given** Reception marks a Called patient Unresponded  
**Then** the event is visible in history and the Visit returns to Waiting at the rule-derived queue position.

Supports: REC-07.

## UXA-008 — Finalized prescription has no ordinary edit control

**Given** prescription is Finalized  
**Then** the screen offers reprint and replacement/correction flow, but not direct in-place edit.

Supports: DOC-05, DOC-07.

## UXA-009 — Partial dispensing preserves original prescription

**Given** pharmacy can only supply part of the prescribed quantity  
**Then** the dispensing screen shows supplied and unsupplied remainder without rewriting the original prescription.

Supports: PHA-03, DOC-05.

## UXA-010 — Pharmacy-unit context never disappears

**Given** clinic has more than one pharmacy unit  
**When** Pharmacist uses inventory, dispensing, bill, or transfer screens  
**Then** the active pharmacy unit/source-destination context remains visible.

Supports: PHA-01, PHA-03, PHA-04, PHA-07–PHA-10.

## UXA-011 — Bill void warning is explicit

**Given** Pharmacist or Owner views bill-void flow  
**Then** the UI states that approving the void does not restore already-dispensed stock.

Supports: PHA-05, OWN-03, IX Section 16.

## UXA-012 — Inventory adjustment shows projected result

**Given** Pharmacist prepares manual inventory change  
**Then** current stock, proposed delta, and resulting quantity are visible before submission when quantity-based.

Supports: PHA-09, OWN-03, IX Section 17.

## UXA-013 — Unauthorized direct navigation is safe

**Given** a user lacks permission for a screen/action  
**When** the user reaches it by direct URL/navigation/history  
**Then** protected content is not rendered and the action remains unavailable.

Supports: IA-06, IX Section 26.

## UXA-014 — Owner/Admin clinical boundary

**Given** an Owner-only or Admin-only user  
**When** they open operational/audit screens  
**Then** full diagnosis/clinical-note content is not exposed solely by those roles.

Supports: OWN-07, ADM screens, IX Section 34.

## UXA-015 — Multi-role authority is visible

**Given** one account has more than one assigned role  
**When** the user switches among permitted workspaces  
**Then** the active workspace/authority remains visible, the same human identity is retained, and material actions are attributed to the authority context actually used.

Supports: SH-06, IA-01, IX Section 29.

## UXA-016 — Unsaved material changes warning

**Given** a user changed a material form but has not successfully saved/submitted/finalized it  
**When** user navigates away  
**Then** the product warns before discarding the unsaved input.

Supports: IX Section 3.6.

## UXA-017 — Loading is not empty

**Given** a data-backed list is still loading  
**Then** the product does not show a final “No data” empty state until retrieval completes.

Supports: IA Section 12, IX Sections 22–23.

## UXA-018 — High-impact confirmation communicates consequence

**Given** user finalizes prescription, approves bill void, approves inventory loss, or disables a staff account  
**Then** the confirmation describes the actual consequence rather than a generic “Are you sure?”

Supports: IX Section 27.

## UXA-019 — Single-role entry bypasses workspace selector

**Given** an authenticated user has exactly one permitted workspace  
**Then** the user enters that workspace directly rather than being forced through a meaningless workspace choice.

Supports: P-003, SH-06, IX Section 29.

## UXA-020 — Multi-role switch control remains reachable

**Given** an authenticated user has more than one permitted workspace whose applicable authentication gates are satisfied for the current session  
**Then** the user has an always-reachable workspace-switch control without needing to sign out or use another account.  
**And** if newly granted Owner authority has not yet satisfied the Owner second-factor gate, Owner-capable access remains gated by AU-005 rather than being treated as an ordinary switch.

Supports: P-003, P-009, SH-06, AU-005, AU-012, IX Sections 29 and 35.

## UXA-021 — Permission absence versus state unavailability

**Given** the current account lacks authority for an action  
**Then** that action is not exposed as a normal navigation/action choice and direct access is denied.  
**Given** the user has authority but the current record/state makes the action temporarily invalid  
**Then** the action may remain visible but disabled with a meaningful explanation.

Supports: P-002, P-005, IA-06, IX Section 26.

## UXA-022 — Workspace switch does not leak active record context

**Given** a Doctor is working on a specific patient/Visit  
**When** the same account switches to Owner workspace  
**Then** the Owner workspace does not silently inherit that clinical record as its active context. Any legitimate cross-workspace opening is explicit and enters the target authority context first.

Supports: P-004, IA Section 11.4, IX Section 29.

## UXA-023 — Unsaved work protected during workspace switch

**Given** the current workspace contains material unsaved changes  
**When** the user attempts to switch workspaces  
**Then** the product warns before those changes can be discarded and allows the user to remain in the current workspace.

Supports: P-004, IX Section 3.6, IX Section 29.

## UXA-024 — Revoked role cannot remain usable through an open workspace

**Given** a user's role is revoked while that role's workspace is already open  
**When** the user performs the next protected navigation/action or the permission state refreshes  
**Then** the product denies the revoked authority and routes the user to a permitted workspace without rendering protected content.

Supports: P-002, P-005, IA-06, IX Sections 25–26, 29.

## UXA-025 — Multiple tabs retain independent authority context

**Given** a multi-role user has Doctor workspace open in one browser tab and Owner workspace open in another  
**Then** each tab visibly retains its own authority context and switching one tab does not silently change the other.

Supports: P-001, P-003, P-096, IX Section 29.

## UXA-026 — Cross-workspace attention does not leak protected detail

**Given** a multi-role user is in one workspace and another permitted workspace has pending attention items  
**Then** a count/attention indicator may be shown, but protected detail or actions appear only after entering the correct workspace/authority context.

Supports: P-005, IX Sections 21 and 29.

## UXA-027 — Same human can act through two legitimate authorities with separate attribution

**Given** one account legitimately holds Doctor and Owner roles  
**And** the Doctor authority submits an action requiring Owner approval  
**When** the same human later performs the approval through Owner authority  
**Then** the request and approval remain two separately attributable events showing the same human identity but different effective authorities.

Supports: P-096, FR-098, BR-039, IX Sections 28–29.

## UXA-028 — Exact Patient ID still requires explicit selection

**Given** search returns an exact Patient ID match  
**Then** that result is visually prioritized but the workspace does not silently enter patient context until Reception explicitly selects it.

Supports: P-017, REC-02, IX Sections 5 and 36.

## UXA-029 — Final registration rechecks current duplicate candidates

**Given** Reception completed a new-patient form  
**When** final registration is submitted  
**Then** current duplicate candidates are evaluated before effective Patient creation even if an earlier manual search found none.

Supports: P-016, P-019–P-020, REC-03, IX Section 6.

## UXA-030 — Candidate review requires an explicit identity branch

**Given** final duplicate review surfaces similar patients  
**Then** effective creation pauses until Reception either selects an existing confirmed patient or deliberately chooses **Create New as Possible Duplicate**.

Supports: P-019–P-020, REC-02–REC-03, IX Sections 5–6.

## UXA-031 — Possible Duplicate remains usable and traceable

**Given** Reception deliberately creates a new patient after unresolved candidate review  
**Then** the profile is usable, visibly marked Possible Duplicate, and retains candidate Patient IDs plus actor/time provenance without auto-merge.

Supports: P-020, REC-03–REC-04, IX Sections 6 and 36.

## UXA-032 — Reception patient profile does not become clinical history

**Given** Reception opens a Patient Profile or prior-Visit identity context  
**Then** the product may show identity/operational summaries needed for reception work but not unrestricted diagnosis, clinical notes, prescriptions, or Doctor longitudinal clinical history.

Supports: P-023, REC-02, REC-04, IX Sections 6 and 36.

## UXA-033 — No-active-Visit demographic correction remains Doctor-controlled

**Given** Reception needs to correct established demographics and no active Visit exists  
**Then** Reception selects an authorized Doctor reviewer and submits a patient-level correction without creating a fake Visit or directly changing the patient.

Supports: P-024, REC-08, IX Section 36.

## UXA-034 — Stale demographic correction cannot overwrite newer value

**Given** a Reception correction request captured an old current value  
**And** that patient value changed before Doctor decision  
**When** the Doctor tries to apply the pending request  
**Then** the product blocks silent overwrite and requires refreshed review.

Supports: P-024, REC-08, IX Sections 25 and 36.

## UXA-035 — Missing physical file never creates new identity

**Given** a returning patient has no physical paper file  
**When** Reception finds the existing digital patient  
**Then** the existing Patient ID remains usable and Visit creation is not blocked by the missing paper file.

Supports: P-025, REC-04.

## UXA-036 — Unknown Patient-create outcome is not blindly retried

**Given** Patient registration was submitted but the client cannot determine whether creation succeeded  
**Then** the product checks current effective state before allowing another create attempt so a second Patient is not created by retry.

Supports: P-021, P-111, REC-03, IX Sections 24 and 36.

## UXA-037 — Visit creation reuses Patient identity

**Given** Reception has selected an existing/newly-created Patient  
**When** a Visit is created  
**Then** one Visit ID is linked to that Patient ID and no new Patient identity is created because of Possible Duplicate status or missing paper file.

Supports: P-026, REC-05, REM-018.

## UXA-038 — Visit may exist before Doctor assignment

**Given** Reception creates a Visit without selecting a Doctor  
**Then** the Visit exists as Unassigned and Unpaid, but queue entry remains unavailable until a Doctor is assigned and financial eligibility is satisfied.

Supports: P-027, REC-05, REM-019.

## UXA-039 — Paid does not imply queued

**Given** external payment was verified and Reception records Paid  
**And** no Doctor is assigned or queue insertion fails  
**Then** Paid remains effective while the Visit stays not queued; payment is not rolled back or duplicated.

Supports: P-030–P-031, REC-05, IX Sections 8 and 37.

## UXA-040 — Doctor can request waiver before queue

**Given** an Unpaid Visit is assigned to a Doctor but cannot enter that Doctor's queue  
**Then** the Doctor can access a separate assigned-pre-queue financial context and submit a waiver request without gaining consultation access.

Supports: P-034, DOC-01, IX Section 37.

## UXA-041 — One Pending waiver per Visit

**Given** an Unpaid Visit already has an actionable Pending waiver request  
**When** Reception/Doctor tries to submit another  
**Then** the product does not create duplicate Owner work.

Supports: P-034, REC-06, IX Section 37.

## UXA-042 — Paid makes Pending waiver stale

**Given** a waiver request is Pending while Visit is Unpaid  
**When** the Visit becomes Paid before Owner decision  
**Then** Owner cannot approve the stale waiver into Waived and the request remains historical/non-actionable.

Supports: P-035, OWN-03, IX Sections 10 and 37.

## UXA-043 — Owner direct waiver has no self-approval step

**Given** Owner initiates Direct Waiver on an Unpaid Visit with a specific reason  
**When** Owner confirms  
**Then** outcome becomes Waived immediately and no second Pending approval is created.

Supports: P-036, OWN-02, IX Section 37.

## UXA-044 — Payment correction uses current baseline

**Given** a payment-correction request was prepared against one effective payment record  
**And** that effective record changed before Owner decision  
**Then** the old correction cannot be applied silently and refreshed review is required.

Supports: P-038–P-039, REC-09, OWN-03, IX Sections 25 and 37.

## UXA-045 — Paid-to-Unpaid correction is not refund

**Given** Owner approves a correction from incorrectly-recorded Paid to Unpaid  
**Then** the corrected effective state is Unpaid, original Paid record remains auditable, and the product does not create/refund money or erase prior Visit history.

Supports: P-033, P-039, REC-09, IX Sections 8 and 37.

## UXA-046 — Fee configuration change is prospective

**Given** a Visit already captured its applied consultation fee  
**When** clinic fee configuration changes later  
**Then** the existing Visit amount does not silently change.

Supports: P-028, REC-05, IX Section 37.

## UXA-047 — New queue entry appends to Doctor queue end

**Given** an eligible Visit has Paid/Waived and an assigned Doctor  
**When** Reception adds it to queue  
**Then** current state becomes Waiting and it is appended to that Doctor's queue end without arbitrary priority insertion.

Supports: P-040, REC-07, IX Sections 11 and 38.

## UXA-048 — Pre-queue financial Visits are not queue members

**Given** a Visit is assigned to Doctor but still Unpaid  
**Then** it may appear in Assigned Visits Awaiting Financial Eligibility but does not count/order as a Doctor queue Visit.

Supports: P-040, DOC-01, REM-026.

## UXA-049 — Start Consultation requires current Called assignment

**Given** Doctor sees a Waiting Visit  
**Then** opening it does not create With Doctor.  
**When** Doctor Calls it and later explicitly starts consultation while still assigned  
**Then** Called -> With Doctor.

Supports: P-041, DOC-01, IX Sections 11 and 38.

## UXA-050 — Reassignment goes to destination end and transfers correction reviewer

**Given** Reception reassigns a Waiting/Called Visit from Doctor A to Doctor B  
**Then** Visit appears at Doctor B queue end as Waiting, prior queue/call history remains, and any Pending Visit-linked demographic-correction reviewer becomes Doctor B.

Supports: P-042, REC-07, REM-020, IX Sections 11 and 38.

## UXA-051 — Unresponded is historical event plus current Waiting

**Given** current state is Called  
**When** Reception marks Unresponded  
**Then** Unresponded event is retained, Visit moves five places down/end, and current state becomes Waiting.

Supports: P-043, REC-07, IX Section 11.

## UXA-052 — Financial correction to Unpaid removes pre-consult queue membership non-destructively

**Given** a Waiting/Called queued Visit later receives an approved correction to Unpaid  
**Then** it leaves active queue membership, all prior queue history remains, and renewed eligibility requires explicit queue re-entry at end.

Supports: P-039, P-040, REM-027, IX Sections 11 and 38.

## UXA-053 — Pending cancellation does not freeze Visit

**Given** Doctor submits a valid cancellation request  
**Then** request becomes Pending but current Visit remains active and may progress until Owner decision.

Supports: P-046, DOC-09, IX Section 38.

## UXA-054 — Completed-before-decision makes cancellation stale

**Given** cancellation is Pending  
**When** Visit reaches Completed before Owner decides  
**Then** Owner cannot approve cancellation through this flow and request remains historical/non-actionable.

Supports: P-046–P-047, DOC-09, OWN-03, IX Section 38.

## UXA-055 — Cancellation approval preserves downstream history

**Given** cancellable Visit already has clinical/prescription/dispensing/financial history  
**When** Owner approves cancellation  
**Then** Visit becomes Cancelled/Voided and future active workflow stops, but existing history remains and no refund is created.

Supports: P-047, OWN-03, IX Section 38.

## UXA-056 — Stale queue action cannot overwrite newer state

**Given** one user loaded a queue row  
**And** another user changed its state/Doctor  
**When** the first user tries Call/Reassign/Unresponded/Start/Move using stale state  
**Then** the product blocks the outdated action and refreshes current state.

Supports: P-041–P-044, IX Sections 25 and 38.

## UXA-057 — Clinical authoring begins only With Doctor

**Given** a Visit is Waiting or Called  
**Then** viewing the Visit/history does not expose an editable active consultation.  
**When** assigned Doctor explicitly starts consultation from Called  
**Then** state becomes With Doctor and current consultation authoring becomes available.

Supports: P-048, DOC-01–DOC-02, REM-034, IX Sections 11, 12, 39.

## UXA-058 — Save Draft does not complete consultation

**Given** Visit is With Doctor  
**When** Doctor saves an incomplete current consultation draft  
**Then** saved content remains associated with the Visit, required completion fields may still be incomplete, and Visit remains With Doctor.

Supports: P-050–P-052, DOC-02, IX Sections 12 and 39.

## UXA-059 — Complete Consultation creates immutable boundary

**Given** required clinical fields are present and current state/assignment remain valid  
**When** Doctor explicitly completes consultation  
**Then** Visit becomes Consultation Completed, current clinical record is the completed effective record, and ordinary edit controls become read-only.

Supports: P-052–P-053, DOC-02, IX Sections 12 and 39.

## UXA-060 — Possible Duplicate does not merge clinical histories

**Given** Patient is marked Possible Duplicate with candidate Patient IDs  
**When** Doctor opens longitudinal history  
**Then** only the current Patient ID's authorized timeline is shown; candidate profiles are not automatically merged into it.

Supports: P-049, DOC-03, REM-021, IX Sections 12 and 39.

## UXA-061 — Demographic correction approval is stale-safe

**Given** Doctor opened an assigned demographic-correction request  
**And** current demographic value or Doctor routing changed before decision  
**When** Doctor tries to approve  
**Then** outdated decision is blocked and refreshed current value/reviewer context is required.

Supports: P-024, DOC-01–DOC-02, REM-021, IX Sections 25, 36, 39.

## UXA-062 — Direct Doctor demographic correction is not fake approval

**Given** authorized Doctor directly corrects an established demographic  
**Then** old/new value, Doctor, and time are recorded, Patient ID remains immutable, and no fabricated Reception request/approval is created.

Supports: P-024, IX Sections 36 and 39.

## UXA-063 — Amendment creates new effective revision

**Given** a completed consultation exists  
**When** Doctor creates an amendment with required reason  
**Then** a new effective clinical revision is created and every prior revision remains read-only history.

Supports: P-053, DOC-06, IX Sections 12 and 39.

## UXA-064 — Amendment does not reopen a closed Visit

**Given** a completed clinical record belongs to a Visit now Completed or Cancelled/Voided  
**When** Doctor creates a valid amendment  
**Then** clinical revision changes but queue/payment/cancellation/Visit state do not reopen.

Supports: P-053, DOC-06, IX Section 39.

## UXA-065 — Pending cancellation does not freeze consultation

**Given** cancellation request is Pending and Visit remains With Doctor  
**Then** Doctor may continue valid clinical work while Pending status remains visible.

Supports: P-046, P-048–P-052, REM-034, IX Sections 38 and 39.

## UXA-066 — Effective cancellation blocks stale clinical write

**Given** Doctor has unsaved/open consultation content  
**And** Owner cancellation becomes effective first  
**When** Doctor tries Save Draft or Complete Consultation  
**Then** product blocks the stale write, shows Cancelled/Voided current state, and preserves already-saved clinical content.

Supports: P-047, P-050–P-052, REM-034, IX Sections 25, 38, 39.

## UXA-067 — Non-Doctor authority does not reveal full clinical content

**Given** Owner-only, Admin-only, Reception, or Pharmacist authority  
**When** operational/audit/history context is opened  
**Then** unrestricted diagnosis/notes are not exposed solely by that authority.

Supports: P-054, IA role boundaries, IX Section 39.

## UXA-068 — Prescription can be finalized before consultation completion without pharmacy handoff

**Given** Visit is With Doctor  
**When** Doctor finalizes a valid prescription  
**Then** prescription becomes Finalized/read-only but Visit remains With Doctor and is not yet pharmacy-ready.

Supports: P-059, DOC-04, IX Sections 13 and 40.

## UXA-069 — Consultation Completed waits for prescription when needed

**Given** Doctor completes consultation before a current prescription is finalized  
**Then** Visit remains Consultation Completed and appears in Doctor's separate awaiting-prescription task context rather than re-entering the queue.

Supports: P-052, P-059, DOC-01, IX Section 40.

## UXA-070 — Second readiness prerequisite triggers Sent to Pharmacy

**Given** one of consultation completion or current prescription finalization is already satisfied  
**When** the other prerequisite becomes effective  
**Then** current Visit state advances to Sent to Pharmacy without changing the already-recorded clinical/finalization event.

Supports: P-052, P-059, REM-041, IX Section 40.

## UXA-071 — Availability refresh does not block unavailable prescribing

**Given** a prescribed item changes from In Stock to Out of Stock before finalization  
**When** Doctor finalizes  
**Then** finalization refreshes/stores Out of Stock, prescription remains valid, and that version receives the ** print marker.

Supports: P-055–P-057, P-064, IX Sections 13, 14, 40.

## UXA-072 — Reprint preserves the finalization snapshot

**Given** finalized prescription marked an item ** at finalization  
**And** stock later becomes available  
**When** Doctor reprints the same version  
**Then** the marker remains because reprint uses the stored finalization snapshot and creates no new version.

Supports: P-063–P-064, DOC-05, IX Section 14.

## UXA-073 — Replacement is atomic and stale-safe

**Given** Doctor opened replacement for current finalized version  
**And** another replacement became current first  
**When** Doctor submits the older replacement form  
**Then** it is blocked as stale and cannot supersede the newer current version.

Supports: P-061, DOC-07, IX Sections 25 and 40.

## UXA-074 — Replacement preserves prior dispensing

**Given** medicine was already dispensed from the current finalized prescription  
**When** Doctor finalizes a replacement  
**Then** old prescription becomes Superseded, new version becomes current, and prior dispensing/stock/billing history remains unchanged.

Supports: P-061–P-062, REM-035, DOC-07, IX Section 40.

## UXA-075 — Pharmacy defaults latest current version

**Given** an older prescription was Superseded  
**When** prescription context is opened for subsequent workflow  
**Then** latest current Finalized version is the default and the superseded lineage remains visible as history.

Supports: P-060–P-062, DOC-05, IX Section 40.

## UXA-076 — Effective cancellation blocks prescription continuation

**Given** Doctor has an active draft or replacement open  
**And** Visit becomes Cancelled/Voided first  
**When** Doctor attempts Save/Finalize/Finalize Replacement  
**Then** stale active action is blocked and existing prescription versions/history remain preserved.

Supports: P-047, P-059–P-062, REM-035, IX Sections 38 and 40.

## UXA-077 — Completed Visit does not reopen for active prescription replacement

**Given** Visit is Completed  
**Then** historical prescription versions remain viewable but new active Finalize/Replacement actions are unavailable.

Supports: P-059–P-062, IX Section 40.

## UXA-078 — Clinical amendment does not mutate prescription

**Given** Doctor amends a completed clinical record  
**Then** prescription versions remain unchanged unless a separate valid active-Visit prescription replacement is explicitly performed.

Supports: P-053, P-061, IX Sections 39 and 40.

## UXA-079 — No unapproved no-prescription bypass

**Given** Visit is Consultation Completed without a finalized prescription  
**Then** implementation does not silently mark it Completed through an invented no-prescription shortcut.

Supports: locked workflow baseline, P-059, IX Section 40.

## UXA-080 — Finalized but not pharmacy-ready cannot dispense

**Given** prescription is Finalized but Visit is still With Doctor  
**When** Pharmacist finds the Patient  
**Then** prescription is not offered as active dispensable work.

Supports: P-065, P-074, REM-045, IX Sections 15 and 41.

## UXA-081 — Patient ID lookup requires explicit Visit/version choice

**Given** Patient ID has multiple relevant Visits/prescriptions  
**When** Pharmacy lookup returns results  
**Then** Visit/state/version are shown and Pharmacist explicitly selects the intended pharmacy-ready current prescription.

Supports: P-065, PHA-02.

## UXA-082 — Pharmacy clinical boundary remains narrow

**Given** Pharmacist opens current/previous prescription context  
**Then** known allergies and dispensing instructions are visible but unrestricted diagnosis/notes/Doctor longitudinal history are not.

Supports: P-066, REM-042, PHA-02, IX Section 41.

## UXA-083 — Replacement lineage reduces future allowance

**Given** old prescription prescribed 10 units and 4 were already dispensed  
**And** replacement carries the same item lineage with corrected quantity 8  
**Then** current remaining allowable is 4, not 8 or a reset allowance.

Supports: P-062, P-067, P-071, REM-045, IX Sections 15 and 41.

## UXA-084 — Corrected quantity below prior dispense does not reverse history

**Given** 6 units were already dispensed  
**And** replacement corrects the same lineage to 4  
**Then** remaining allowable is 0, historical 6 remain recorded, and no stock is restored automatically.

Supports: P-062, P-067–P-071, IX Section 41.

## UXA-085 — Multi-unit concurrent dispense cannot overfill prescription

**Given** two pharmacy units view the same remaining quantity  
**When** Unit A dispenses first  
**Then** Unit B must recompute remaining allowance and cannot commit a stale quantity that would exceed the active prescription.

Supports: P-071, P-075, IX Sections 15, 30, 41.

## UXA-086 — Dispense and stock deduction stay together

**Given** Pharmacist confirms a valid dispense  
**Then** actual supplied quantity and active-unit stock deduction become effective together; unknown outcome is checked before retry.

Supports: P-067–P-068, PHA-03, IX Section 41.

## UXA-087 — Partial dispensing creates no reservation

**Given** Pharmacy supplies less than remaining prescription quantity  
**Then** supplied amount is recorded/billable, remainder is visible, and no collect-later reservation is created.

Supports: P-069–P-070, IX Section 15.

## UXA-088 — Approved substitute consumes original allowance

**Given** Doctor approved a substitute proposal for an original prescription item  
**When** Pharmacy dispenses the approved substitute  
**Then** supply is linked to the approval/original item and consumes that item's permitted fulfilment rather than adding an extra allowance.

Supports: P-072, PHA-06, DOC-08, IX Sections 15 and 41.

## UXA-089 — Stale substitution request cannot be approved/dispensed

**Given** a substitution request was created  
**And** prescription was replaced or remaining quantity changed before decision  
**Then** stale proposal cannot be approved/dispensed without refreshed current review.

Supports: P-072, DOC-08, PHA-06, IX Sections 25 and 41.

## UXA-090 — Effective Visit cancellation blocks future dispense

**Given** Pharmacy has a valid dispense form open  
**And** Owner cancellation becomes effective first  
**When** Pharmacist confirms dispense  
**Then** stale dispense is blocked while already-committed dispensing/stock remains.

Supports: P-047, P-074, REM-036, IX Sections 38 and 41.

## UXA-091 — Pharmacy-unit switch does not carry unsaved quantity silently

**Given** Pharmacist entered dispense quantities in Pharmacy Unit A  
**When** switching to Unit B  
**Then** product requires explicit discard/review and never submits those quantities under Unit B silently.

Supports: P-075, PHA-03, IX Sections 15 and 30.

## UXA-092 — Bill creation uses explicit unit-specific unbilled supply

**Given** Pharmacist opens billing for a Visit  
**Then** the product shows the active pharmacy unit and eligible supplied-but-not-yet-billed dispensing records, and never bills prescribed-but-unsupplied quantity.

Supports: P-077, PHA-04, IX Section 16.

## UXA-093 — Payment method selection does not mark pharmacy bill Paid

**Given** Pharmacist selects UPI/Cash/Card/Other on an Unpaid bill  
**Then** the bill stays Unpaid until explicit Mark Paid after external success.

Supports: P-078, PHA-04, IX Sections 8 and 16.

## UXA-094 — Pharmacy payment correction cannot edit bill amount

**Given** an established pharmacy payment record is wrong  
**When** Pharmacist requests correction  
**Then** payment fields may be proposed against the captured baseline, but bill lines/total remain immutable and the request requires Owner decision.

Supports: P-078, P-038–P-039, PHA-04, OWN-03.

## UXA-095 — Pending bill void leaves bill operational

**Given** a bill-void request is Pending  
**Then** the bill remains active, payment may still be recorded, and the request itself does not restore stock or create a refund.

Supports: P-081–P-084, PHA-05, IX Section 16.

## UXA-096 — Unpaid bill can coexist with Completed Visit

**Given** all pharmacy-fulfilment completion conditions are met  
**And** one or more bills remain Unpaid  
**Then** the completion action is not blocked solely by payment state and the outstanding financial status remains visible.

Supports: P-078–P-080, PHA-04, IX Section 16.

## UXA-097 — Visit completion considers all pharmacy units

**Given** multiple units dispensed for one Visit  
**When** one unit finishes billing  
**Then** the Visit cannot complete while another unit still has committed unbilled dispensing or an actionable substitution still represents intended clinic fulfilment.

Supports: P-075–P-077, PHA-04, REM-051.

## UXA-098 — Completed or Cancelled Visit cannot create a new bill

**Given** current Visit is Completed or Cancelled/Voided  
**Then** Pharmacy cannot create another bill or dispense more medicine, while existing bill payment/correction/void history remains reachable according to authority.

Supports: P-047, P-077–P-084, REM-037, IX Section 16.

## UXA-099 — Voiding never re-bills or restores stock automatically

**Given** Owner approves pharmacy bill void  
**Then** the bill becomes historical Cancelled/Voided, its source dispensing remains historical, stock is not restored, and those dispensing records are not silently turned into a new bill.

Supports: P-083–P-084, PHA-05, OWN-03.

## UXA-100 — Unknown pharmacy billing outcome is checked before retry

**Given** a pharmacy billing/completion/void/payment state-changing action returns an unknown outcome  
**When** user attempts again  
**Then** the current Visit/bill/request/payment state is refreshed before another effective action is allowed.

Supports: P-077–P-084, IX Sections 16 and 25.
## UXA-101 — Manual stock cannot be edited directly

**Given** Pharmacist views inventory quantity  
**Then** there is no ordinary direct quantity edit; exceptional stock changes use the request workflow and normal dispense remains automatic.

Supports: P-087–P-090, PHA-08–PHA-09.

## UXA-102 — Adjustment request shows entered and normalized effect

**Given** Pharmacist prepares quantity adjustment using a package unit  
**Then** the request shows entered quantity/unit, conversion to base units, current baseline, delta, and projected result before submission.

Supports: P-085, P-088, PHA-09, IX Section 17.

## UXA-103 — Stock-count correction exposes derived delta

**Given** Pharmacist enters a counted/resulting quantity  
**Then** product derives and displays the difference from captured current stock rather than hiding the correction as an unexplained replacement number.

Supports: P-088, PHA-09.

## UXA-104 — Owner sees current state before inventory approval

**Given** an adjustment request is Pending  
**When** Owner opens it after other inventory movement occurred  
**Then** captured baseline and current stock are distinguishable and stale application is blocked when result no longer matches reviewed proposal.

Supports: P-090, OWN-03, IX Section 17.

## UXA-105 — Expired quantity is visible but non-dispensable

**Given** batch is expired  
**Then** inventory views show it as expired/unavailable rather than silently zeroing/removing it, and dispensing controls cannot select it.

Supports: P-091, PHA-07–PHA-08.

## UXA-106 — Transfer keeps source and destination explicit

**Given** Pharmacist prepares transfer  
**Then** source, destination, medicine/batch, entered/base quantity, current transferable source quantity, and reason stay visible through request and Owner decision.

Supports: P-092–P-093, PHA-10, OWN-03.

## UXA-107 — Pending transfer does not reserve stock

**Given** transfer request is Pending  
**Then** source inventory remains ordinarily usable; approval rechecks actual source quantity and becomes stale rather than partially transferring if insufficient.

Supports: P-092–P-093, IX Section 17.

## UXA-108 — Transfer approval updates both ledgers together

**Given** Owner approves valid transfer  
**Then** both source and destination movements become effective under the same transfer reference; UI never presents only one side as successfully completed.

Supports: P-093, OWN-03, OWN-04.

## UXA-109 — Inventory history preserves physical movement despite billing lifecycle

**Given** stock was deducted by dispense  
**Then** later prescription replacement/bill/payment/void/Visit state changes do not rewrite the movement or its unit/batch/source attribution.

Supports: P-087, REM-047, REM-056.

## UXA-110 — Inventory unknown outcome is checked before retry

**Given** inventory request/direct action/approval/transfer returns unknown outcome  
**When** user retries  
**Then** current request and ledger state are refreshed first so the product cannot knowingly double-apply a movement.

Supports: P-088–P-093, IX Sections 17 and 25.

## UXA-111 — Approval queue defaults to actionable Pending work

**Given** Owner opens Approval Center  
**Then** actionable Pending items are primary; resolved history remains filterable by request type/requester/date/outcome without invented urgency ranking.

Supports: P-094, OWN-02.

## UXA-112 — Request detail shows only valid action type

**Given** Owner opens a request  
**Then** detail renders Approve/Reject only when that lifecycle supports it, renders Set Temporary Credential for password reset, and renders no executable control on stale/resolved history.

Supports: P-094, OWN-03, OWN-09.

## UXA-113 — Row selection never executes Owner decision

**Given** Owner selects an Approval Center row  
**Then** product opens detail/context and does not approve/reject/reset merely from row click.

Supports: P-094, IX Sections 9–10.

## UXA-114 — Owner approval context does not leak clinical content

**Given** Owner-only authority opens Visit cancellation or other patient-linked approval  
**Then** product shows operational state/history existence needed for decision but not unrestricted diagnosis/clinical-note content.

Supports: P-094–P-096, OWN-03, RA-001.

## UXA-115 — Stale/resolved request is visibly read-only

**Given** another session already resolved request or target became ineligible  
**When** Owner opens stale copy  
**Then** executable action is unavailable, current state is shown/refreshed, and request remains historical.

Supports: P-094–P-095, IX Section 10.

## UXA-116 — Direct Owner actions do not create self-approval artifacts

**Given** Owner performs Direct Consultation Waiver or Direct Inventory Adjustment  
**Then** action is explicitly confirmed/audited as direct Owner action and does not create requester=Owner -> approver=Owner Pending request.

Supports: P-095, P-090, REM-029, REM-063.

## UXA-117 — Password reset eligibility is revalidated

**Given** reset request was created for non-Owner staff  
**And** target later becomes Owner-role or otherwise ineligible for that path  
**When** Owner opens request  
**Then** old request is non-actionable and cannot bypass Owner recovery policy.

Supports: P-094, OWN-09, REM-010–REM-011.

## UXA-118 — Same-human requester/approver attribution stays visible

**Given** same account performs request under Doctor/Pharmacist role and decision under Owner role  
**Then** request detail/history shows the same human with distinct effective authority for each event.

Supports: P-096, REM-005.

## UXA-119 — Rejection preserves unchanged effective target where defined

**Given** Owner rejects valid waiver/cancellation/bill-void/payment-correction/inventory/transfer request  
**Then** UI shows request Rejected and clearly preserves the applicable current business record rather than implying the requested change happened.

Supports: P-094–P-095.

## UXA-120 — Paid bill void warning is current, not stale snapshot

**Given** bill became Paid after void request submission  
**When** Owner reviews  
**Then** current Paid state and no-refund/no-stock-restoration consequences are shown before approval.

Supports: P-094, REM-057.

## UXA-121 — Payment correction compares captured, current and proposed

**Given** Owner reviews payment correction  
**Then** captured baseline, current effective record and proposed correction are distinguishable; changed baseline blocks application.

Supports: P-094, REM-030, REM-058.

## UXA-122 — Inventory/transfer approval compares request baseline with current stock

**Given** Owner reviews inventory adjustment or transfer  
**Then** captured stock/source state and current state are visibly distinct and current-state revalidation determines whether action remains valid.

Supports: P-094, REM-063.

## UXA-123 — Owner decision unknown outcome refreshes before retry

**Given** final Owner action outcome is unknown  
**When** Owner attempts again  
**Then** current request/target/result is refreshed before another executable action is enabled.

Supports: P-094–P-095, IX Sections 10 and 25.

## UXA-124 — Owner accounts are protected in Admin staff UI

**Given** Administrator views staff list
**Then** Owner account may be visibly identified as protected, but Owner grant/revoke/disable actions are absent/denied.

Supports: P-099, ADM-02–ADM-03.

## UXA-125 — Role revocation updates active authority

**Given** current user loses active workspace role
**Then** already-open page cannot continue normal protected action after permission refresh/navigation.

Supports: P-098, IX Sections 26 and 29.

## UXA-126 — Account disablement does not look like password reset

**Given** staff account is disabled
**Then** UI distinguishes disabled status from credential/reset status and never suggests password reset re-enables access.

Supports: P-100, ADM-02, OWN-09.

## UXA-127 — Re-enable requires fresh sign-in

**Given** disabled account is re-enabled
**Then** prior protected session is not silently resumed; user must sign in again.

Supports: P-100, IX Section 35.

## UXA-128 — Pending request retains historical requester attribution after role change

**Given** requester later loses role or account becomes disabled
**Then** request detail/history still shows request-time actor/effective authority and does not disappear.

Supports: P-098–P-100, REM-067.

## UXA-129 — Medicine archive removes future selection, not history

**Given** current medicine is archived
**Then** new authoring/search excludes it as active choice while historical prescription/dispense/bill references remain readable.

Supports: P-101, ADM-04.

## UXA-130 — Conversion edit shows prospective consequence

**Given** authorized user changes package conversion
**Then** UI explains future package entry/display changes while base stock and historical movement quantities stay unchanged.

Supports: P-101, ADM-05, REM-064.

## UXA-131 — Threshold change updates current alert view without history rewrite

**Given** threshold changes
**Then** current low-stock/near-expiry classification may refresh, but movement history remains unchanged.

Supports: P-101, ADM-05.

## UXA-132 — Admin cannot publish Owner-controlled financial configuration

**Given** Administrator opens clinic configuration
**Then** consultation fee and pharmacy price/tax effective-value controls are absent/read-only unless the same account also enters valid Owner authority.

Supports: P-101, ADM-06.

## UXA-133 — Consultation fee edit warns old Visits stay unchanged

**Given** Owner edits consultation fee
**Then** confirmation states new value applies prospectively and existing Visit applied amounts are not rewritten.

Supports: P-101, REM-031.

## UXA-134 — Pharmacy price/tax edit warns old bills stay frozen

**Given** Owner edits price/tax configuration
**Then** confirmation states existing created bills remain unchanged.

Supports: P-101, REM-059.

## UXA-135 — Pharmacy unit archive is historical, not destructive

**Given** Owner archives unit with history
**Then** UI shows loss of new operational availability but preserves historical ledger/report visibility and never offers destructive delete.

Supports: P-101, ADM-06.

## UXA-136 — Configuration audit shows old and new safe values

**Given** material configuration was changed
**Then** authorized audit/history shows actor/effective authority, field/category, old/new value and time without secret credential material.

Supports: P-101, OWN-07, IX Section 42.

## UXA-137 — Unknown administration outcome refreshes before retry

**Given** account/role/config save returns unknown outcome
**When** user retries
**Then** current account/configuration state is refreshed first to avoid duplicate or reverted state.

Supports: P-097–P-101, IX Section 42.

## UXA-138 — No configuration edit silently cascades into historical records

**Given** current catalogue/fee/price/conversion/unit configuration changes
**Then** preserved Visit/prescription/dispense/bill/movement/approval history is unchanged except explicit future/current derived views defined by the product.

Supports: P-101, IX Section 42.

## UXA-139 — Report loading is not shown as zero data

**Given** report query is still loading
**Then** UI does not display a misleading zero/empty report until query completes.

Supports: P-102–P-105.

## UXA-140 — Report no-data state names active filters

**Given** completed report has no rows for current filter
**Then** UI distinguishes "no data for this filter/date range" from loading or access denial.

Supports: P-102–P-105.

## UXA-141 — Financial report says recorded status, not bank settlement

**Given** Owner views revenue
**Then** UI identifies figures as CRM-recorded Paid status and does not claim bank settlement verification.

Supports: P-102, OWN-05.

## UXA-142 — Void and payment remain separately understandable

**Given** Paid pharmacy bill was later Voided without refund
**Then** financial reporting can show recorded Paid amount and void status separately without implying refund or active bill equivalence.

Supports: P-102, REM-060.

## UXA-143 — Waiting-time drill-down preserves queue journey

**Given** Visit had reassignment/Unresponded and later successful consultation
**Then** report/drill-down can explain the queue-entry-to-With-Doctor interval without resetting for ordinary queue movements.

Supports: P-102, REM-039.

## UXA-144 — Stock report keeps unit context

**Given** Owner views consolidated stock
**Then** clinic total supports drill-down to pharmacy unit/batch and does not hide expired/unavailable recorded quantity inside valid available stock.

Supports: P-102, REM-065.

## UXA-145 — Archived entities remain filterable

**Given** medicine/unit/staff was archived or disabled
**Then** historical report filters/results retain that identity with current archived/disabled indication rather than dropping or reassiging old records.

Supports: P-102–P-104, REM-070.

## UXA-146 — Admin report scope remains non-clinical

**Given** Admin opens reports
**Then** clinic-wide non-clinical authorized metrics do not reveal unrestricted diagnosis/clinical-note content or Owner approval controls.

Supports: P-103.

## UXA-147 — Pharmacist report scope remains unit-limited

**Given** Pharmacist is scoped to Pharmacy A
**Then** pharmacy/inventory report does not silently include Pharmacy B unless the user separately holds authority that permits it.

Supports: P-104.

## UXA-148 — Effective outcome and workflow activity are distinguishable

**Given** Owner views waiver/cancellation activity
**Then** effective Approved/direct outcomes can be distinguished from Pending/Rejected/Stale request volume.

Supports: P-102, REM-068.

## UXA-149 — Most-prescribed view does not mix historical versions as current

**Given** prescriptions were replaced
**Then** standard aggregate reflects canonical current final version while historical version detail remains accessible where authorized.

Supports: P-102.

## UXA-150 — Revenue correction updates aggregate without duplicate history

**Given** approved payment correction changes effective state
**Then** aggregate updates to current effective value/state while audit/history still shows original record without double-counting it.

Supports: P-102, REM-032, REM-060.

## UXA-151 — Reporting filters never broaden authorization

**Given** user selects date/unit/entity filters
**Then** result remains restricted to the user's authorized scope even if another entity is named in URL/filter input.

Supports: P-102–P-106.


## UXA-152 — Substitute sales retain actual medicine and unit attribution

**Given** Owner/report user drills into medicine sales containing an approved substitution
**Then** the report identifies the actual substitute medicine supplied and preserves its pharmacy-unit attribution; the original prescription item may be shown only as lineage/context and is not presented as the sold medicine for that substituted quantity.

Supports: P-102, REM-053.


## UXA-153 — Audit distinguishes current from historical state

**Given** an entity has current and prior/superseded/cancelled/rejected/stale history
**Then** the audit/history UI makes current/effective state visually distinct and does not present historical rows as normally actionable.

Supports: P-107, OWN-07.

## UXA-154 — Audit event shows actor and effective authority

**Given** material event was performed by a multi-role user
**Then** event detail shows the human/account plus effective role/workspace used for that event.

Supports: P-108, REM-007.

## UXA-155 — Audit source link respects current access

**Given** audit row references a protected source record
**When** current viewer lacks authority to that source
**Then** safe metadata may remain visible but protected detail/link action is unavailable/denied.

Supports: P-110, OWN-07.

## UXA-156 — Audit never displays authentication secret values

**Given** authentication/recovery event appears in history
**Then** password/reset/TOTP/recovery-code values are never displayed, copied, searched, filtered or exposed in event detail.

Supports: P-110, REM-014, REM-017.

## UXA-157 — Normal audit UI provides no delete/edit action

**Given** authorized user views historical audit event
**Then** there is no normal Delete/Edit control that erases or rewrites that event.

Supports: P-109, OWN-07.

## UXA-158 — Hard stale action explains material change

**Given** final action depends on a baseline that changed materially
**Then** product blocks application, shows that current truth changed, refreshes relevant values and requires review/new proposal rather than silently merging/clipping/applying the old intent.

Supports: P-112.

## UXA-159 — Latest-state-capable request is not automatically stale

**Given** a workflow explicitly allows decision using current truth after related state change
**Then** detail shows the latest state/consequence and allows the valid current-state decision rather than rejecting solely because any field changed.

Supports: P-112, REM-061.

## UXA-160 — Unknown outcome is not shown as success or blind retry

**Given** client cannot determine whether high-impact action succeeded
**Then** UI enters a check-current-state/recovery path and does not show success or immediately repeat the final action.

Supports: P-111.

## UXA-161 — Duplicate final submit is unavailable while request is in flight

**Given** user has triggered a state-changing final action
**While** that request is still pending
**Then** the same final control cannot knowingly submit an identical second action from that UI.

Supports: P-111.

## UXA-162 — Conflict preserves local unsaved input for review

**Given** stale clinical or other material form data conflicts with newer current state
**Then** product does not silently discard the local input while forcing refresh; it preserves enough local content for comparison/recovery where practical.

Supports: P-112, REM-044.

## UXA-163 — Revoked authority removes protected open-tab use

**Given** a protected tab was valid when opened
**When** current role/account authority is revoked/disabled and detected
**Then** protected action/content is no longer treated as current authorized context and user is returned to an allowed/sign-in path.

Supports: P-112, REM-008, REM-016, REM-071.

## UXA-164 — Workspace changes do not silently cross tabs

**Given** two tabs use different permitted workspaces
**When** one switches workspace
**Then** the other keeps its explicit context until independently changed/revalidated; it is never silently promoted to another authority.

Supports: P-112, REM-009.

## UXA-165 — Recovery shows already-applied result instead of duplicating it

**Given** refresh after unknown outcome finds the intended effect already committed
**Then** product presents/re-enters the effective resulting state and does not offer the original action as though nothing happened.

Supports: P-111.

## UXA-166 — Atomic-operation recovery checks linked effects

**Given** dispense/stock or transfer outcome is uncertain
**Then** recovery status checks the linked operation/reference as a whole before another final attempt and does not invite user to repair one side manually.

Supports: P-111, REM-054, REM-066.

## UXA-167 — Historical archived identities remain understandable

**Given** actor, medicine or pharmacy unit is later disabled/archived
**Then** audit/history continues to identify the historical entity and may show its current Archived/Disabled status without reassigning old activity.

Supports: P-107–P-109.

## UXA-168 — Direct Owner action is visibly distinct from request/approval history

**Given** Owner performs an allowed direct waiver/inventory action
**Then** audit labels it as a direct Owner-authority action and does not render an artificial requester/approver pair.

Supports: P-108, REM-069.

## UXA-169 — Report interactions remain read-only and permission-scoped

**Given** user refreshes, filters or drills into reporting
**Then** no source business record changes and result/detail remains within current authorized scope.

Supports: P-110–P-112, REM-073.


---

# 12. Interaction Release Blockers

The PRD interaction layer is not acceptable if any of the following are possible:

1. payment becomes Paid merely by choosing a payment method;
2. high-impact action executes from a row click without explicit action;
3. a Pending approval silently changes the source record;
4. a stale approval can be applied twice;
5. an unauthorized direct route renders protected clinical/Owner content;
6. pharmacy-unit context disappears during dispensing or stock transfer;
7. finalized prescription exposes ordinary edit controls;
8. bill void UI implies dispensed stock will return automatically;
9. Admin UI can grant/revoke Owner authority;
10. Owner-only UI exposes unrestricted clinical notes;
11. loading is rendered as an empty final state;
12. material unsaved form data can be silently discarded.
