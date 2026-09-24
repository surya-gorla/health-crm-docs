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
| REM-018 | G3 | G4 | Preserve the Patient-to-Visit boundary: Visit creation must link the already selected/newly created permanent Patient ID and must never create another Patient identity. Possible Duplicate status and missing physical file must not block first/subsequent Visit creation. | G3 makes Patient identity independently usable; G4 owns Visit creation/payment transition. | OPEN |
| REM-019 | G3 | G4 | When an active Visit exists without an assigned Doctor, verify G4 Doctor-selection/assignment behavior can satisfy G3's requirement that Reception cannot submit that Visit-linked demographic correction until a Doctor is selected. Do not create a Visit solely for a correction when no active Visit exists. | G3 correction routing depends on active Visit Doctor assignment, while G4 owns Visit/Doctor creation context. | OPEN |
| REM-020 | G3 | G5 | If an active Visit is reassigned while a demographic-correction request is Pending, the request must follow the current assigned Doctor and the previous Doctor must not remain able to apply the stale routing. | G3 derives correction authority from the Visit's assigned Doctor; G5 owns reassignment. | OPEN |
| REM-021 | G3 | G6 | Doctor review must support Reception demographic-correction approval/rejection and Doctor direct demographic correction with old/new audit detail and stale-value protection. Doctor longitudinal history may expose Possible Duplicate status/context but must not automatically combine clinical histories across candidate Patient IDs because V1 has no merge. | G3 defines the patient-level correction and duplicate identity rules; G6 owns Doctor clinical workspace/history. | OPEN |
| REM-022 | G3 | G13 | Returning-patient/unique-patient reporting must respect the permanent Patient IDs that actually exist. Possible Duplicate profiles remain separate identities in V1; reporting must not silently similarity-deduplicate them unless a later approved business definition explicitly says so. | V1 has no duplicate merge, so reporting logic can otherwise silently contradict the identity model. | OPEN |
| REM-023 | G3 | G14 | Extend P-111 retry/idempotency review to Patient creation: an unknown registration outcome must be resolved by checking effective state before another create attempt, including concurrent candidate changes discovered at final submit. | Patient identity duplication is high-impact and G3 now defines product-level retry behavior. | OPEN |
| REM-024 | G3 | G14 | Audit/history must cover Possible Duplicate provenance and demographic corrections without exposing unauthorized clinical content: candidate Patient IDs/actor/time for the marker; old/new/requester/Doctor decision/time for corrections; Doctor direct edits remain attributable. | G3 introduces auditable identity-risk and demographic-change metadata that G14 must present consistently. | OPEN |
| REM-025 | G3 | G14 | Stale-state protection must explicitly cover demographic correction: if the captured current value changed before Doctor decision, the old proposal cannot overwrite newer truth; exact concurrency/version mechanism stays technical. | G3 establishes the stale business behavior; G14 owns the cross-product stale-state contract. | OPEN |

---

## Resolved Reminder History

| ID | Raised By | Target Group | Disposition | Status |
| --- | --- | --- | --- | --- |
| REM-001 | G1 | G2 | Resolved by D-G2-01 / P-009: every account containing Owner authority must satisfy Owner second factor before **any** normal workspace; choosing Doctor/non-Owner workspace cannot bypass it. | RESOLVED |
| REM-002 | G1 | G2 | Resolved across D-G2-07 plus G1 reconciliation: whole-account disablement ends protected account use when detected and returns to sign-in; role-only revocation removes affected authority/workspace. Exact propagation remains technical. | RESOLVED |
| REM-003 | G1 | G2 | Resolved by D-G2-06 / P-013: reset credential enters only the forced-change gate; successful replacement then resumes G1 normal single-/multi-workspace routing. | RESOLVED |
| REM-004 | G1 | G2 | Resolved by D-G2-01/D-G2-08 and reconciliation commit `5fe2bcc7151092307aa1d527d3b42863ec7e6144`: normal authenticated workspace switching does not repeat login/TOTP, while newly granted Owner authority remains second-factor gated before use. | RESOLVED |

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


## Target G4 — Mandatory Reminders

When G4 begins, explicitly evaluate:

- REM-018
- REM-019

## Target G5 — Mandatory Reminders

When G5 begins, explicitly evaluate:

- REM-020

## Target G6 — Mandatory Reminders

When G6 begins, explicitly evaluate:

- REM-021

## Target G13 — Mandatory Reminders

When G13 begins, explicitly evaluate:

- REM-022

## Additional Target G14 Reminders from G3

When G14 begins, also explicitly evaluate:

- REM-023
- REM-024
- REM-025
