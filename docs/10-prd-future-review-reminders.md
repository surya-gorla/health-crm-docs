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
| REM-001 | G1 | G2 | Confirm Owner-role authentication applies to any account containing Owner authority regardless of which workspace the user intends to enter, and reconcile this with single-/multi-workspace post-login entry. | G1 established one identity with workspace-specific authority; Owner security must not be bypassed by choosing Doctor or another workspace. | OPEN |
| REM-002 | G1 | G2 | Define the product expectation for authentication/session behavior after role revocation or account disable while the user is already signed in. Keep the implementation mechanism technical, but ensure stale authority/account access cannot remain indefinitely usable. | G1 requires revoked authority to stop working in already-open workspaces. G2 owns account/authentication state. | OPEN |
| REM-003 | G1 | G2 | Verify password-reset/forced-change flows cannot accidentally enter a normal workspace before required credential replacement is complete, and determine what happens for a multi-role account after the forced-change gate. | G1 defines workspace entry rules; G2 controls credential-recovery gates. | OPEN |
| REM-004 | G1 | G2 | Evaluate whether workspace switching within the same authenticated account should require re-authentication. Default direction from G1 is no second login; identify only security-sensitive exceptions if clearly justified. | G1 says multi-role switching uses one identity without second login. G2 must not accidentally contradict this. | OPEN |
| REM-005 | G1 | G11 | When reviewing Owner Approval Center, verify workflows where the same human holds requester and approver roles remain separately attributed by effective authority and are not silently collapsed. | P-096 now distinguishes human identity from effective role/workspace. | OPEN |
| REM-006 | G1 | G12 | When staff roles are added/removed/disabled, verify the Administration product behavior is compatible with G1's already-open workspace revocation rule and does not assume changes take effect only after a fresh login. | Role administration is the source of permission changes that G1 says must invalidate stale authority. | OPEN |
| REM-007 | G1 | G14 | Audit/history must preserve effective role/workspace for material multi-role actions, including same-human actions under different roles. | P-096 was strengthened in G1 and requires cross-product audit support. | OPEN |
| REM-008 | G1 | G14 | State-safety/concurrency review must include permission changes occurring while a protected workspace/tab is already open. | G1 requires safe handling of stale authority; G14 owns cross-product stale-state/safety behavior. | OPEN |
| REM-009 | G1 | G14 | Multi-tab/session behavior must not blur authority context across tabs, and state changes in one context must not silently make another tab authoritative for a different role. | G1 explicitly permits independent permitted workspace contexts per tab/window. | OPEN |

---

## Target G2 — Mandatory Reminders for Current Review

The G2 review must explicitly evaluate and disposition:

- REM-001
- REM-002
- REM-003
- REM-004

No G2 decision should be marked complete until these reminders are resolved or converted into explicit later dependencies.
