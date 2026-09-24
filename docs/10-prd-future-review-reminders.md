# Hospital CRM — PRD Future Review Reminders

## Purpose

This document stores forward-looking issues raised by already-reviewed groups that must be evaluated when their target groups are reviewed.

A reminder is not an already-decided future requirement. It is a mandatory review input. The target group must evaluate it together with its own BRD/PRD evidence and may resolve, refine, supersede, or convert it into a cross-group reconciliation decision.

---

## Reminder Status

- **OPEN** — target group has not yet evaluated the reminder.
- **IN REVIEW** — target group is actively evaluating it.
- **RESOLVED** — target group evaluated and disposition is recorded.
- **SUPERSEDED** — a later accepted rule replaced the reminder before target review.
- **NOT APPLICABLE** — target review established that no product change/check is required.

---

## Open Reminder Register

| ID | Raised By | Target Group | Reminder | Why It Matters | Status |
| --- | --- | --- | --- | --- | --- |
| REM-005 | G1 | G11 | When reviewing Owner Approval Center, verify workflows where the same human holds requester and approver roles remain separately attributed by effective authority and are not silently collapsed. | P-096 distinguishes human identity from effective role/workspace. | OPEN |
| REM-006 | G1 | G12 | When staff roles are added/removed/disabled, verify Administration behavior is compatible with G1's already-open workspace revocation rule and does not assume changes take effect only after a fresh login. | Role administration is the source of permission changes that G1 says must invalidate stale authority. | OPEN |
| REM-007 | G1 | G14 | Audit/history must preserve effective role/workspace for material multi-role actions, including same-human actions under different roles. | P-096 requires cross-product audit support. | OPEN |
| REM-008 | G1 | G14 | State-safety/concurrency review must include permission changes occurring while a protected workspace/tab is already open. | G1 requires safe handling of stale authority; G14 owns cross-product stale-state/safety behavior. | OPEN |
| REM-009 | G1 | G14 | Multi-tab/session behavior must not blur authority context across tabs, and state changes in one context must not silently make another tab authoritative for a different role. | G1 explicitly permits independent permitted workspace contexts per tab/window. | OPEN |
| REM-010 | G2 | G11 | Reconcile the generic Approval Center model with staff password reset: reset is **Pending -> Resolved by Set Temporary Credential**, not generic Approve/Reject. Decide how OWN-02/OWN-03 and P-094 present this action-specific workflow without changing the locked Owner-controlled recovery policy. | Current G11 baseline lists password reset among approval types and assumes Pending/Approved/Rejected + Approve/Reject, while G2 intentionally defines a different resolution action. | OPEN |
| REM-011 | G2 | G11 | Ensure the Owner Approval Center prevents duplicate/stale password-reset action: only one reset request is simultaneously actionable as Pending, and a Resolved request cannot be acted again from a stale Owner screen/session. | G2 defines one active request and stale-request safety; G11 owns Owner request handling UI. | OPEN |
| REM-012 | G2 | G12 | When creating an Owner account or granting Owner authority to an existing account, ensure Owner capability cannot be used until the mandatory TOTP enrollment/verification or second-factor gate is satisfied. Admin still cannot grant Owner. | G2 makes Owner security an account-level prerequisite, including Owner authority added during an existing non-Owner session. | OPEN |
| REM-013 | G2 | G12 | Keep account lifecycle and password recovery separate: password reset must not re-enable an account; disablement stops protected use when detected; re-enable requires a fresh sign-in; role revocation removes only the affected authority/workspace. | G2 and reconciled G1 distinguish credential, account-status, and role-state transitions. | OPEN |
| REM-014 | G2 | G14 | Audit/history for authentication and recovery events may record safe metadata but must never expose passwords, reset credentials, TOTP secrets/codes, or recovery-code values. | G2 introduces explicit secret-handling boundaries that G14 must preserve in cross-product audit UX. | OPEN |
| REM-015 | G2 | G14 | Extend retry/stale-state review to authentication recovery: repeated Forgot Password must not create duplicate actionable requests, and a reset already resolved by another Owner session cannot be applied again as Pending. | P-111/P-112 currently emphasize other business records; G2 adds an auth-recovery state that needs the same safety discipline. | OPEN |
| REM-016 | G2 | G14 | Evaluate account disablement/role changes while multiple sessions or tabs are open. Preserve the product rule that stale authority/protected use stops when the state change is detected, while leaving exact propagation/session transport to technical design. | G1/G2 now define product behavior but not implementation mechanics. | OPEN |
| REM-017 | G2 | G14 | Include recovery-code lifecycle in state-safety review: used codes cannot authenticate again and regeneration invalidates the prior set. Do not expose code values in audit/state history. | Recovery codes are security-sensitive one-time state governed by G2. | OPEN |
| REM-022 | G3 | G13 | Returning-patient/unique-patient reporting must respect the permanent Patient IDs that actually exist. Possible Duplicate profiles remain separate identities in V1; reporting must not silently similarity-deduplicate them unless a later approved business definition explicitly says so. | V1 has no duplicate merge, so reporting logic can otherwise silently contradict the identity model. | OPEN |
| REM-023 | G3 | G14 | Extend P-111 retry/idempotency review to Patient creation: an unknown registration outcome must be resolved by checking effective state before another create attempt, including concurrent candidate changes discovered at final submit. | Patient identity duplication is high-impact and G3 now defines product-level retry behavior. | OPEN |
| REM-024 | G3 | G14 | Audit/history must cover Possible Duplicate provenance and demographic corrections without exposing unauthorized clinical content: candidate Patient IDs/actor/time for the marker; old/new/requester/Doctor decision/time for corrections; Doctor direct edits remain attributable. | G3 introduces auditable identity-risk and demographic-change metadata that G14 must present consistently. | OPEN |
| REM-025 | G3 | G14 | Stale-state protection must explicitly cover demographic correction: if the captured current value changed before Doctor decision, the old proposal cannot overwrite newer truth; exact concurrency/version mechanism stays technical. | G3 establishes the stale business behavior; G14 owns the cross-product stale-state contract. | OPEN |
| REM-028 | G4 | G9 | Reuse the common payment semantics for pharmacy: explicit Paid confirmation, UPI/Cash/Card/Other, Other description, optional reference, no partial/refund, baseline-aware correction, and unknown-outcome retry safety where applicable. Do not import consultation-waiver behavior into pharmacy billing. | G4 refined the shared payment interaction contract; G9 owns pharmacy billing. | OPEN |
| REM-029 | G4 | G11 | Approval Center must support consultation-waiver specifics: one actionable Pending request, Paid-before-decision makes the request stale/non-actionable, rejection preserves Unpaid and allows a later new request, and **Direct Waiver** is an Owner action rather than a request requiring self-approval. | G4 defines waiver lifecycle; G11 owns generic Owner approval UX/state. | OPEN |
| REM-030 | G4 | G11 | Payment-correction approval must display/revalidate the captured payment baseline, block stale application, preserve original/proposed/effective values, and make clear Paid->Unpaid is record correction rather than refund. | G4 defines correction safety; G11 owns Owner decision presentation. | OPEN |
| REM-031 | G4 | G12 | Consultation-fee configuration changes are prospective: changing clinic configuration must not silently rewrite the applied amount of already-created Visits. A Visit-specific correction does not change global fee configuration. | G4 establishes Visit-level fee snapshot semantics; G12 owns configuration UX. | OPEN |
| REM-032 | G4 | G13 | Consultation revenue/financial reporting should use the current effective recorded financial state after approved corrections, count Paid consultation amounts, exclude Waived, and avoid double-counting superseded/original payment records retained for audit. | G4 preserves original financial history while defining one corrected effective record; G13 owns reporting definitions/presentation. | OPEN |
| REM-033 | G4 | G14 | Cross-product retry/stale safety must cover Visit creation, Paid recording, combined Mark Paid + queue partial success, one Pending waiver/correction per relevant baseline, stale waiver after Paid, and stale payment-correction baseline. Exact idempotency/concurrency mechanisms remain technical. | G4 defines product-level safety outcomes that G14 must generalize. | OPEN |
| REM-036 | G5 | G8 | If cancellation becomes effective at Sent to Pharmacy, Pharmacy must stop future dispensing/fulfilment for that Visit while preserving all dispensing already performed. Each dispensing action should revalidate that Visit is still active. | G5 explicitly permits cancellation from Sent to Pharmacy and makes it non-destructive. | OPEN |
| REM-037 | G5 | G9 | Visit cancellation must not create pharmacy refund or erase existing bill/payment history. Evaluate whether new pharmacy billing/payment actions remain available after Visit cancellation and ensure cancelled Visit cannot continue active billing workflow. | G5 stops future active Visit workflow while preserving existing pharmacy history; G9 owns billing/payment. | OPEN |
| REM-038 | G5 | G11 | Owner cancellation approval must support one Pending request, current-state revalidation, Completed-before-decision stale behavior, non-freezing Pending workflow, rejection leaving current state unchanged, and same-human Doctor-request/Owner-decision attribution. | G5 defines the cancellation lifecycle; G11 owns Approval Center behavior. | OPEN |
| REM-039 | G5 | G13 | Average-wait reporting must respect queue history: ordinary reassignment/Unresponded/move-to-end preserve the queue-entry journey; actual removal for financial ineligibility plus later re-entry creates another queue-entry event. Define the deterministic reporting event without deleting earlier history. | OD-029 defines wait as queue entry -> With Doctor; G5 now allows multiple historical queue-entry events in one Visit. | OPEN |
| REM-040 | G5 | G14 | Cross-product stale/concurrency review must include races among Call, Start Consultation, Reassign, Unresponded, move-to-end, financial correction, cancellation request/decision, and Visit completion. One Pending cancellation per Visit and stale decisions/actions must be enforced. | G5 depends heavily on current state/assignment and concurrent actors. | OPEN |
| REM-042 | G6 | G8 | Preserve G6 clinical-content boundary in Pharmacy: dispensing may use current/previous prescriptions and known allergies, but must not expose unrestricted diagnosis, consultation notes, or Doctor longitudinal clinical history. | G6 explicitly limits full clinical content to Doctor authority; G8 owns Pharmacy retrieval/dispensing. | OPEN |
| REM-043 | G6 | G13 | Patients-seen/day and related consultation counts should anchor to the original Consultation Completed event. Later clinical amendments must not create a second completed consultation or change the original completion event; evaluate how later Visit cancellation affects reporting without rewriting historical completion. | G6 separates completion from later amendment/cancellation history; G13 owns reporting. | OPEN |
| REM-044 | G6 | G14 | Generalize stale/audit safety for clinical drafts and amendments: concurrent draft saves must not overwrite newer content, stale amendment baseline must refresh, effective cancellation must reject stale clinical writes, and clinical revision audit must preserve current vs prior revisions without exposing content to unauthorized audit roles. | G6 defines product-level clinical concurrency/revision behavior; G14 owns cross-product safety/audit. | OPEN |
| REM-045 | G7 | G8 | Pharmacy retrieval/dispensing must require Visit pharmacy-readiness (Sent to Pharmacy), default to the latest current Finalized prescription, reject a stale Superseded version, and compute remaining dispensable quantity against the active corrected prescription while preserving prior dispensing from superseded versions. | G7 defines prescription version lineage and readiness; G8 owns dispensing execution. | OPEN |
| REM-046 | G7 | G9 | Prescription replacement after some dispensing must never erase or silently rebill prior pharmacy bill/payment history. Billing should remain tied to actual supplied quantities/version lineage and only new subsequent dispensing may create new billable supply. | G7 preserves prior dispensing/billing through replacement; G9 owns pharmacy billing/payment. | OPEN |
| REM-047 | G7 | G10 | Prescription replacement/finalization must not retroactively alter stock. Inventory movements remain driven by actual dispensing; prior stock deductions stay attributed to the original dispensing/version even when that prescription becomes Superseded. | G7 makes replacement non-retroactive; G10 owns inventory accountability. | OPEN |
| REM-048 | G7 | G13 | Most-prescribed/prescription reporting must not double-count superseded replacement versions as independent new clinical intent. Define reporting around current/effective prescription lineage while preserving raw version history for audit. | G7 introduces version lineage with multiple finalized versions for one Visit. | OPEN |
| REM-049 | G7 | G14 | Cross-product stale/idempotency review must cover prescription draft/finalization/replacement: duplicate finalization must not create multiple current versions, stale replacement cannot supersede newer current version, effective cancellation blocks stale prescription writes, and current vs Superseded history remains explicit/auditable. | G7 defines version-state/concurrency behavior; G14 owns global safety/audit. | OPEN |
| REM-050 | G7 | G15 | Printing must use the selected finalized version's stored finalization-time availability snapshot. Current Finalized is the default print/reprint target; any historical Superseded/Cancelled-context copy must be unmistakably historical and must never look like the current dispensable prescription. | G7 fixes snapshot/reprint semantics but G15 owns physical-output presentation. | OPEN |

---

## Resolved Reminder History

| REM-035 | G5 | G7 | Resolved by G7: effective cancellation blocks new active draft/finalize/replacement/pharmacy-readiness progression while all Draft/Finalized/Superseded prescription history remains preserved. | RESOLVED |
| REM-041 | G6 | G7 | Resolved by G7 readiness model and reconciliation commit `e7020544866df081bd98e469890c61ea3f95c1c7`: consultation completion and current Finalized prescription are independent prerequisites; second prerequisite advances to Sent to Pharmacy. | RESOLVED |


| REM-021 | G3 | G6 | Resolved by G6 Doctor demographic-review/direct-correction contracts and Patient-ID-scoped longitudinal history: stale value/reviewer is blocked and Possible Duplicate candidate histories are not auto-combined. | RESOLVED |
| REM-034 | G5 | G6 | Resolved by G6 consultation/cancellation concurrency: authoring starts only at With Doctor, Pending cancellation does not freeze work, and effective cancellation blocks stale future writes while preserving saved clinical content. | RESOLVED |


| ID | Raised By | Target Group | Disposition | Status |
| --- | --- | --- | --- | --- |
| REM-001 | G1 | G2 | Resolved by D-G2-01 / P-009: every account containing Owner authority must satisfy Owner second factor before **any** normal workspace; choosing Doctor/non-Owner workspace cannot bypass it. | RESOLVED |
| REM-002 | G1 | G2 | Resolved across D-G2-07 plus G1 reconciliation: whole-account disablement ends protected account use when detected and returns to sign-in; role-only revocation removes affected authority/workspace. Exact propagation remains technical. | RESOLVED |
| REM-003 | G1 | G2 | Resolved by D-G2-06 / P-013: reset credential enters only the forced-change gate; successful replacement then resumes G1 normal single-/multi-workspace routing. | RESOLVED |
| REM-004 | G1 | G2 | Resolved by D-G2-01/D-G2-08 and reconciliation commit `5fe2bcc7151092307aa1d527d3b42863ec7e6144`: normal authenticated workspace switching does not repeat login/TOTP, while newly granted Owner authority remains second-factor gated before use. | RESOLVED |
| REM-018 | G3 | G4 | Resolved by P-026/P-031 and G4 decisions: Visit creation always links the selected Patient ID; Possible Duplicate/missing physical file are non-blocking; queue entry remains separate. | RESOLVED |
| REM-019 | G3 | G4 | Resolved by P-027 and REC-05/REC-08: active Visit may be Unassigned; Doctor is assigned before queue entry or Visit-linked demographic-correction submission; no fake Visit is created for no-active-Visit correction. | RESOLVED |
| REM-020 | G3 | G5 | Resolved by P-042 / queue interaction: reassignment transfers current demographic-correction reviewer authority to the new Doctor; stale prior-Doctor decision is blocked. | RESOLVED |
| REM-026 | G4 | G5 | Resolved by P-040/DOC-01/REC-07: Assigned Visits Awaiting Financial Eligibility remain outside queue membership/count/order. | RESOLVED |
| REM-027 | G4 | G5 | Resolved by G5 financial-correction rule plus G4 reconciliation commit `8423beaa1cf8f5796a5aae57b7ee708a4ae14d5f`: Waiting/Called may leave current queue non-destructively; With Doctor or later is not unwound. | RESOLVED |

---

## Target G11 — Mandatory Reminders

When G11 begins, explicitly evaluate:

- REM-005
- REM-010
- REM-011

## Target G12 — Mandatory Reminders

When G12 begins, explicitly evaluate:

- REM-006
- REM-012
- REM-013

## Target G14 — Mandatory Reminders

When G14 begins, explicitly evaluate:

- REM-007
- REM-008
- REM-009
- REM-014
- REM-015
- REM-016
- REM-017


## Target G4 — Resolved

REM-018 and REM-019 were resolved during the G4 review and are recorded in Resolved Reminder History.

## Target G5 — Resolved

REM-020 was resolved during G5. Additional G4 reminders REM-026/027 were also resolved and are recorded in Resolved Reminder History.

## Target G6 — Resolved

REM-021 was resolved during the G6 review.

## Target G13 — Mandatory Reminders

When G13 begins, explicitly evaluate:

- REM-022

## Additional Target G14 Reminders from G3

When G14 begins, also explicitly evaluate:

- REM-023
- REM-024
- REM-025


## Target G9 — Mandatory Reminders from G4

When G9 begins, explicitly evaluate:

- REM-028

## Target G11 — Additional Mandatory Reminders from G4

When G11 begins, also explicitly evaluate:

- REM-029
- REM-030

## Target G12 — Additional Mandatory Reminder from G4

When G12 begins, also explicitly evaluate:

- REM-031

## Target G13 — Additional Mandatory Reminder from G4

When G13 begins, also explicitly evaluate:

- REM-032

## Target G14 — Additional Mandatory Reminder from G4

When G14 begins, also explicitly evaluate:

- REM-033


## Target G6 — Additional Reminder Resolved

REM-034 was resolved during the G6 review.

## Target G7 — Reminder Resolved

REM-035 was resolved during G7.

## Target G8 — Mandatory Reminder from G5

When G8 begins, evaluate:
- REM-036

## Target G9 — Additional Mandatory Reminder from G5

When G9 begins, also evaluate:
- REM-037

## Target G11 — Additional Mandatory Reminder from G5

When G11 begins, also evaluate:
- REM-038

## Target G13 — Additional Mandatory Reminder from G5

When G13 begins, also evaluate:
- REM-039

## Target G14 — Additional Mandatory Reminder from G5

When G14 begins, also evaluate:
- REM-040


## Target G7 — Additional Reminder Resolved

REM-041 was resolved during G7.

## Target G8 — Additional Mandatory Reminder from G6
When G8 begins, also evaluate:
- REM-042

## Target G13 — Additional Mandatory Reminder from G6
When G13 begins, also evaluate:
- REM-043

## Target G14 — Additional Mandatory Reminder from G6
When G14 begins, also evaluate:
- REM-044


## Target G8 — Additional Mandatory Reminder from G7
When G8 begins, also evaluate:
- REM-045

## Target G9 — Additional Mandatory Reminder from G7
When G9 begins, also evaluate:
- REM-046

## Target G10 — Mandatory Reminder from G7
When G10 begins, evaluate:
- REM-047

## Target G13 — Additional Mandatory Reminder from G7
When G13 begins, also evaluate:
- REM-048

## Target G14 — Additional Mandatory Reminder from G7
When G14 begins, also evaluate:
- REM-049

## Target G15 — Mandatory Reminder from G7
When G15 begins, evaluate:
- REM-050
