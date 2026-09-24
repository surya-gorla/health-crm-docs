# Hospital CRM — PRD Refinement State Ledger

## Purpose

This ledger is the persistent execution state for the long-lived V1 PRD refinement review.

It exists so the review can resume safely across sessions without relying on conversation memory alone. It records the current group, current stage, source files reviewed, accepted product decisions, commits, validation outcomes, backward-compatibility checks, forward-impact analysis, unresolved blockers, and the next exact action.

This ledger records decision-grade reasoning and evidence, not private/internal chain-of-thought.

---

## Global Refinement Status

| Field | Current Value |
| --- | --- |
| Refinement PR | PR #2 — `docs: refine V1 PRD group by group` |
| Working branch | `prd/refine-group-01-workspace-navigation` |
| Source baseline | BRD v1.0 LOCKED on `main` |
| Current PRD version | v0.3 DRAFT |
| Current group | G2 — Authentication, Account Access & Credential Recovery |
| Current stage | DECISIONS RESOLVED / READY TO EDIT |
| Completed groups | G1 |
| In-progress groups | G2 |
| Not started | G3–G15 |
| Open cross-group conflicts | 0 |
| Open future reminders | See Document 10 |
| Latest checkpoint commit | `c52c0371e246ac914bebef4d279c55e981da8627` |

---

## Group Lifecycle

Each group progresses through:

1. NOT STARTED
2. PREPARING
3. SOURCE REVIEW
4. GROUP REASONING
5. DECISIONS RESOLVED
6. CHANGES COMMITTED
7. COMMIT VALIDATED
8. BACKWARD COMPATIBILITY CHECK
9. RECONCILIATION, if required
10. FORWARD IMPACT ANALYSIS
11. FUTURE REMINDERS RECORDED
12. FINAL GROUP VALIDATION
13. COMPLETE

A group is COMPLETE only when all four gates pass:

- **Gate A — Current-group validation:** PASS
- **Gate B — Backward compatibility:** PASS
- **Gate C — Forward impact/reminders:** COMPLETE
- **Gate D — Ledger/checkpoint state:** CURRENT

---

# Group Status Table

| Group | Name | Status | Main Group Commit | Backward Compatibility | Forward Review | Notes |
| --- | --- | --- | --- | --- | --- | --- |
| G1 | Workspace, Navigation & Multi-Role Context | COMPLETE | `6dab93810f89d10d2606594ffbbd1bdbd70214e9` | PASS — no earlier reviewed groups | COMPLETE | Accepted edge-case refinements plus follow-up workflow/version alignment in `00a4cd86a4465f52d3abc374d420273f027960c5` |
| G2 | Authentication, Account Access & Credential Recovery | DECISIONS RESOLVED | — | — | — | Current group |
| G3 | Patient Search, Identity, Registration & Patient Profile | NOT STARTED | — | — | — | |
| G4 | Visit Creation, Consultation Payment, Waiver & Payment Correction | NOT STARTED | — | — | — | |
| G5 | Doctor Queue & Visit Flow Control | NOT STARTED | — | — | — | |
| G6 | Consultation & Longitudinal Clinical Record | NOT STARTED | — | — | — | |
| G7 | Prescription Authoring & Prescription Lifecycle | NOT STARTED | — | — | — | |
| G8 | Pharmacy Access, Prescription Retrieval & Dispensing | NOT STARTED | — | — | — | |
| G9 | Pharmacy Billing, Payment & Bill Cancellation | NOT STARTED | — | — | — | |
| G10 | Inventory, Stock Accountability & Pharmacy Transfers | NOT STARTED | — | — | — | |
| G11 | Owner Approval Center & Exception Control | NOT STARTED | — | — | — | |
| G12 | Staff Administration & Clinic Configuration | NOT STARTED | — | — | — | |
| G13 | Reporting & Management Visibility | NOT STARTED | — | — | — | |
| G14 | Cross-Product State, Audit, History & Safety | NOT STARTED | — | — | — | |
| G15 | Printing & Physical Outputs | NOT STARTED | — | — | — | |

---

# G1 — Workspace, Navigation & Multi-Role Context

## Final status

**COMPLETE**

### Primary requirements

- P-001–P-005
- P-096

### Source material reviewed

- `docs/01-business-requirements-document.md`
- `docs/03-open-decisions-and-edge-cases.md`
- `docs/05-product-requirements-document.md`
- `docs/06-prd-traceability-and-acceptance.md`
- `docs/07-information-architecture-and-screen-specification.md`
- `docs/08-interaction-and-form-behavior-specification.md`

### Accepted product decisions

1. A single-workspace user enters that workspace directly after authentication.
2. A multi-role user uses one account and receives a workspace selector plus an always-reachable workspace switch control.
3. Lack of authority and temporary state unavailability are distinct:
   - unauthorized functionality is omitted from normal navigation and direct access is denied;
   - authorized-but-currently-invalid actions may remain visible but disabled with explanation.
4. Workspace context is scoped and protected patient/Visit/pharmacy context does not silently carry into another authority context.
5. Material unsaved work is protected before workspace switching.
6. Role revocation invalidates stale authority in an already-open workspace on the next protected action/navigation or permission refresh.
7. Separate browser tabs/windows may operate under different permitted workspaces; each retains explicit authority context.
8. Cross-workspace attention counts may be shown without exposing protected detail/action before entering the proper workspace.
9. P-096 records both human identity and effective role/workspace for material actions.
10. Multi-role behavior applies to all valid role combinations, not only Owner + Doctor.
11. The same human may perform different workflow actions through separately assigned roles when the BRD permits both; audit preserves each authority context separately.

### Commits

- `6dab93810f89d10d2606594ffbbd1bdbd70214e9` — Group 1 refinement.
- `00a4cd86a4465f52d3abc374d420273f027960c5` — refinement-workflow and document-version alignment.

### Validation

PASS.

### Backward compatibility

No earlier reviewed group existed. Gate B passed by definition.

### Forward impact

Targeted future reminders were identified for authentication/session behavior, role administration, Owner approval flows, and audit/state safety. These are recorded in Document 10.

---

# G2 — Authentication, Account Access & Credential Recovery

## Current checkpoint

**Stage:** DECISIONS RESOLVED / READY TO EDIT

### Primary requirements

- P-008 — Individual account login
- P-009 — Owner TOTP
- P-010 — Non-Owner login
- P-011 — Staff Forgot Password
- P-012 — Owner reset
- P-013 — Forced credential replacement
- P-014 — Owner recovery codes
- P-015 — Disabled account

### Required inputs before reasoning

- locked BRD authentication/account requirements and business rules;
- workflow/state implications for authentication and password recovery;
- V1 decision register entries for authentication, roles, lifecycle, and Owner security;
- existing PRD Group 2 requirements;
- shared/authentication screen contracts;
- interaction rules affecting forms, permission changes, stale sessions, errors, and multi-role users;
- G1 reminders targeted to G2 from Document 10.

### Current action

Apply the resolved G2 product decisions to PRD, acceptance, screen, and interaction specifications in one logical Group 2 commit.

### Source-review files scheduled

- `docs/01-business-requirements-document.md`
- `docs/02-workflows-and-state-model.md`
- `docs/03-open-decisions-and-edge-cases.md`
- `docs/04-requirements-traceability.md`
- `docs/05-product-requirements-document.md`
- `docs/06-prd-traceability-and-acceptance.md`
- `docs/07-information-architecture-and-screen-specification.md`
- `docs/08-interaction-and-form-behavior-specification.md`
- `docs/10-prd-future-review-reminders.md`

### Blockers

None currently identified.

---

# Chronological Execution Log

## 2026-09-24 — Refinement workflow established

- Initial PRD baseline was squash-merged into `main`.
- Long-lived refinement PR #2 was created and subsequently converted to draft.
- Workflow changed from per-group PRs to one long-lived PR with logical group/cross-group commits.
- Group 1 was reviewed, refined, committed, and validated.

## 2026-09-24 — G2 PREPARING checkpoint

- User instructed the process to begin G2.
- New protocol requires state ledger and future-reminder register to be read/updated continuously.
- Before G2 reasoning, Documents 09 and 10 are being established as persistent process state.
- Next exact action: read G2-relevant BRD, workflows, decision register, traceability, PRD, screen specification, interaction specification, and all reminders targeting G2.


## 2026-09-24 — G2 SOURCE REVIEW START

- Ledger/reminder infrastructure commit `c979b0d1e0fa96be4a0d94c8e3e6c2ca9b1e6427` revalidated: PASS.
- G2 moved from PREPARING to SOURCE REVIEW.
- Mandatory prior-group reminders: REM-001, REM-002, REM-003, REM-004.
- No product decision will be written until the scheduled source review is complete.


## 2026-09-24 — G2 SOURCE REVIEW COMPLETE

### Files read

- `docs/01-business-requirements-document.md`
- `docs/02-workflows-and-state-model.md`
- `docs/03-open-decisions-and-edge-cases.md`
- `docs/04-requirements-traceability.md`
- `docs/05-product-requirements-document.md`
- `docs/06-prd-traceability-and-acceptance.md`
- `docs/07-information-architecture-and-screen-specification.md`
- `docs/08-interaction-and-form-behavior-specification.md`
- `docs/10-prd-future-review-reminders.md`

### Locked business conclusions confirmed

1. Every user has one individual username/login + password account in the fixed clinic context.
2. Any account containing Owner role requires Google Authenticator-compatible TOTP before login completes; choosing a non-Owner workspace cannot bypass this.
3. Accounts with no Owner role do not require 2FA in V1.
4. Non-Owner Forgot Password creates an Owner-controlled reset request; it is not self-service.
5. Owner may set a new/temporary credential but never view the old credential.
6. Reset credential must be replaced after the next successful login before normal use.
7. Reset request and Owner reset action are auditable.
8. Owner TOTP enrollment produces one-time recovery codes; regeneration invalidates the prior set and used codes are invalid.
9. Catastrophic Owner recovery, inactivity/session timeout, and password-strength policy remain security/technical dependencies.
10. Disabled accounts are preserved, cannot sign in, and may later be re-enabled through authorized staff lifecycle controls.

### Product-definition gaps identified for G2 reasoning

- Owner TOTP must gate the entire login for an Owner-containing account, not merely the Owner workspace.
- Initial Owner TOTP enrollment/bootstrap is not sequenced clearly enough.
- Recovery-code use/regeneration behavior needs a clearer product contract without inventing the deferred catastrophic-recovery mechanism.
- Forgot Password must remain non-enumerating and must not route Owner accounts into the non-Owner staff reset process.
- Repeated Forgot Password submissions should not create duplicate simultaneously actionable Owner work.
- Staff reset request needs an explicit lifecycle and stale/resolved handling.
- Forced password change must precede all normal workspace entry and then return to G1 single-/multi-workspace routing.
- Password reset must not re-enable a disabled account.
- Account disable during an active session needs a product expectation consistent with G1 stale-authority rules.
- If Owner authority is added during an already-authenticated non-Owner session, Owner-capable access cannot become usable until the Owner TOTP requirement is satisfied.
- Normal workspace switching after a fully authenticated session should not require repeated login/TOTP.
- Session timeout, password-strength details, rate limiting/lockout, and exact session-invalidation mechanics remain technical/security design and must not be invented in the PRD.

### Mandatory reminder status entering reasoning

- REM-001 — ACTIVE IN REVIEW
- REM-002 — ACTIVE IN REVIEW
- REM-003 — ACTIVE IN REVIEW
- REM-004 — ACTIVE IN REVIEW


## 2026-09-24 — G2 DECISIONS RESOLVED

No new clinic/business input is required. The following are DERIVED PRODUCT DESIGN / security-boundary clarifications consistent with the locked BRD.

### D-G2-01 — Owner-role 2FA gates the entire login

- Any account containing Owner authority must satisfy password + Owner TOTP/recovery-code second factor before **any** normal application workspace is entered.
- The user cannot choose Doctor or another non-Owner workspace to bypass Owner 2FA.
- After a fully authenticated Owner-containing session is established, normal switching among already-permitted workspaces does not require a second login or repeated TOTP solely because of the switch.

Disposition: resolves REM-001 and REM-004, subject to post-commit compatibility validation with G1.

### D-G2-02 — Initial Owner TOTP enrollment is a mandatory pre-workspace gate

- Valid Owner password with no enrolled TOTP factor routes to mandatory enrollment before normal workspace entry.
- Enrollment requires the generated authenticator secret/QR plus successful TOTP verification.
- Recovery codes are shown only after successful factor verification and are shown only in the enrollment/regeneration event.
- Login is not considered complete until the required factor enrollment/verification sequence succeeds.
- Exact cryptographic storage and authenticator implementation remain technical.

### D-G2-03 — Recovery-code behavior is explicit but bounded

- A valid unused recovery code may satisfy the Owner second-factor step only after password authentication.
- A used recovery code becomes invalid immediately.
- Regenerating recovery codes invalidates the previous set and shows the replacement set only at regeneration.
- Recovery-code values, TOTP codes, TOTP secrets, and passwords are never written into normal audit/history output.
- Replacing/resetting the TOTP factor beyond initial enrollment is not silently invented here; catastrophic/no-credential recovery remains the controlled technical-security dependency already locked by BR-046/OD-021.

### D-G2-04 — Forgot Password is non-Owner, non-self-service, and non-enumerating

- The in-app staff Forgot Password flow applies to non-Owner accounts only.
- Submitting the form never resets the password directly.
- The unauthenticated response must not confirm whether the supplied identifier exists, is disabled, or carries Owner authority.
- An Owner account must not be routed into the non-Owner Owner-reset workflow; Owner password/account recovery remains the controlled Owner recovery path.
- At most one simultaneously actionable reset request exists per eligible staff account; repeat submissions while one is pending must not create duplicate Owner work.

### D-G2-05 — Staff reset request has a simple lifecycle

- Eligible request -> Pending.
- Owner sets/replaces the temporary credential -> request becomes Resolved.
- A resolved/stale request cannot be actioned again as if still pending.
- No generic Approve/Reject semantics are invented for password reset; the Owner's effective action is setting the temporary credential.
- The reset action replaces the current sign-in credential but does not change role membership or enabled/disabled account status.
- If the account is disabled, a password reset never implicitly re-enables it.

This creates a future compatibility reminder for G11 because the Owner Approval Center currently uses generic Approve/Reject language.

### D-G2-06 — Forced password change is an authentication gate

- A valid Owner-reset credential may establish identity for the reset flow but does not grant normal workspace access.
- The user must successfully set/confirm a replacement password before any normal workspace navigation/content is available.
- After successful replacement, the temporary/reset credential is no longer valid and the user proceeds through G1's normal single-/multi-workspace entry behavior.
- If the change is abandoned or fails, the forced-change gate remains on the next valid reset-credential login.
- Password-strength specifics remain the deferred security policy.

Disposition: resolves REM-003, subject to post-commit compatibility validation with G1.

### D-G2-07 — Disabled account state overrides credentials and recovery

- Disabled accounts cannot enter the product even with otherwise valid normal/reset credentials.
- Password reset does not change disabled status.
- If an account is disabled while a session is active, normal protected use must stop on the next protected navigation/action or authentication-state refresh and the user returns to the sign-in boundary.
- Re-enabling an account does not revive an already-terminated disabled session; the user signs in again.
- Exact real-time revocation/session-propagation mechanism remains technical.

Disposition: resolves the account-disable portion of REM-002; role-only revocation remains governed by G1 and future G12/G14 checks.

### D-G2-08 — Newly granted Owner authority requires second-factor satisfaction before use

- If an authenticated non-Owner session later receives Owner authority, Owner-capable workspace/actions do not become usable merely because the role assignment changed.
- The already-authenticated password may count as the first factor for that session, but Owner access requires the Owner TOTP gate (or mandatory enrollment if no factor exists) before Owner-capable access is exposed.
- Exact step-up/session implementation remains technical.
- This is a security-sensitive exception to the ordinary G1 rule that workspace switching itself does not require re-authentication; a backward-compatibility check will determine whether G1 wording needs clarification.

### D-G2-09 — Safe authentication errors and secret handling

- Invalid username/password responses remain generic.
- Forgot Password responses remain non-enumerating.
- TOTP/recovery-code errors are shown only after password-authenticated Owner flow and do not expose stored secrets.
- Passwords, reset credentials, TOTP secrets/codes, and recovery codes never appear in normal audit/history.
- Audit may record safe metadata such as requester, target account, actor, action, outcome, and timestamp where required.

### Explicitly deferred, not invented in G2

- inactivity/session timeout;
- password-strength rules;
- brute-force/rate-limit/lockout mechanics;
- cryptographic storage/key handling;
- exact live-session invalidation transport;
- catastrophic Owner recovery mechanism when usable credentials/recovery codes are unavailable;
- broader TOTP-factor replacement/device-migration mechanism beyond the locked enrollment/recovery-code behavior.
