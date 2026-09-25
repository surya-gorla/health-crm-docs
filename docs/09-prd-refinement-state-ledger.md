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
| Current PRD version | v0.17 DRAFT |
| Current group | None — all 15 refinement groups complete |
| Current stage | FINAL GLOBAL VALIDATION |
| Completed groups | G1, G2, G3, G4, G5, G6, G7, G8, G9, G10, G11, G12, G13, G14, G15 |
| In-progress groups | None |
| Not started | None |
| Open cross-group conflicts | 0 |
| Open future reminders | 0 — see Document 10 |
| Latest completed group main commit | `359cf7f734494b4c6446479142986125efc768f2` |

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
| G2 | Authentication, Account Access & Credential Recovery | COMPLETE | `f81210f2a7e99bcd21a32d5a4e288c0b530e68e0` | PASS after `5fe2bcc7151092307aa1d527d3b42863ec7e6144` | COMPLETE — REM-010–REM-017 recorded | Closed |
| G3 | Patient Search, Identity, Registration & Patient Profile | COMPLETE | `b7db51edcd73f650a8e3f27bc7e98d21f4e3a438` | PASS vs G1–G2 | COMPLETE — REM-018–REM-025 recorded | Closed |
| G4 | Visit Creation, Consultation Payment, Waiver & Payment Correction | COMPLETE | `38df611096205195a219abbaf498187563c10e90` | PASS vs G1–G3 | COMPLETE — REM-026–REM-033 recorded | Closed |
| G5 | Doctor Queue & Visit Flow Control | COMPLETE | `eec1ae4e4b951456798eb008c710e0d305a1aa50` | PASS after `8423beaa1cf8f5796a5aae57b7ee708a4ae14d5f` | COMPLETE — REM-034–REM-040 recorded | Closed |
| G6 | Consultation & Longitudinal Clinical Record | COMPLETE | `762524428d1c62976519d7cd126ebe199054e2b2` | PASS vs G1–G5 | COMPLETE — REM-041–REM-044 recorded | Closed |
| G7 | Prescription Authoring & Prescription Lifecycle | COMPLETE | `7dfa93fe469ae36b793aa0b8ca17d4931db34832` | PASS after `e7020544866df081bd98e469890c61ea3f95c1c7` | COMPLETE — REM-045–REM-050 recorded | Closed |
| G8 | Pharmacy Access, Prescription Retrieval & Dispensing | COMPLETE | `f9cff596b9124cb2a5659b43882d5f19209b8b2a` | PASS vs G1–G7 | COMPLETE — REM-051–REM-055 recorded | Closed |
| G9 | Pharmacy Billing, Payment & Bill Cancellation | COMPLETE | `d317a8d50cfa5baeb7506920ffa75c4e19776f00` | PASS vs G1–G8 | COMPLETE — REM-056–REM-062 recorded | Closed |
| G10 | Inventory, Stock Accountability & Pharmacy Transfers | COMPLETE | `4a5356d85a9c88b80b4dac1485e5cf445e34b1d7` | PASS vs G1–G9 | COMPLETE — REM-063–REM-066 recorded | Closed |
| G11 | Owner Approval Center & Exception Control | COMPLETE | `0f03e96b76247cd4accbc9d47a874bb376b5b549` | PASS vs G1–G10 | COMPLETE — REM-067–REM-069 recorded | Closed |
| G12 | Staff Administration & Clinic Configuration | COMPLETE | `2eeb8aea4047fc321eb8104faf20cbb22ca5f63e` | PASS vs G1–G11 | COMPLETE — REM-070–REM-072 recorded | Closed |
| G13 | Reporting & Management Visibility | COMPLETE | `6b301301da5f9e92c76297947c54d471663100dd` | PASS vs G1–G12 | COMPLETE after reconciliation `b78dd22ffd5d17b37110286c4a6063d6b325a432`; REM-073 recorded | Closed |
| G14 | Cross-Product State, Audit, History & Safety | COMPLETE | `8613e6123d71a198f0b8f7d880f5fe9bf12533bd` | PASS vs G1–G13 | COMPLETE — all 20 inherited reminders resolved; REM-074 recorded | Closed |
| G15 | Printing & Physical Outputs | COMPLETE | `359cf7f734494b4c6446479142986125efc768f2` | PASS vs G1–G14 | COMPLETE — all five inherited reminders resolved; no future groups | Closed |

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

**Stage:** COMMIT VALIDATED / BACKWARD COMPATIBILITY CHECK

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

Compare the committed G2 authentication behavior against every accepted G1 workspace/multi-role decision and reconcile any ambiguity or conflict before forward-impact analysis.

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


## 2026-09-24 — G2 GROUP COMMIT + COMMIT VALIDATION

### Group commit

- Commit: `f81210f2a7e99bcd21a32d5a4e288c0b530e68e0`
- Files changed:
  - `docs/05-product-requirements-document.md`
  - `docs/06-prd-traceability-and-acceptance.md`
  - `docs/07-information-architecture-and-screen-specification.md`
  - `docs/08-interaction-and-form-behavior-specification.md`

### Validation result

PASS.

- P requirements remain exactly P-001 through P-116 with no duplicate IDs.
- Authentication acceptance now contains AU-001 through AU-012 with no duplicate IDs.
- Screen inventory remains 49 unique screen IDs.
- PRD/companion document versions and parent references align.
- Mandatory Owner whole-login 2FA, initial enrollment, non-enumerating recovery, single Pending reset request, forced-change gate, disabled-session handling, one-time recovery codes, secret exclusion from audit, and deferred security boundaries are all present.
- Initial automated check produced one false negative because the validator expected the phrase “do not require 2FA” while P-010 correctly says “are not required to complete 2FA.” The validator was corrected; product text was not changed to satisfy the check.

### Next exact action

Read G1 accepted requirements/contracts and compare each relevant G2 change against them.


## 2026-09-24 — G2 BACKWARD COMPATIBILITY FINDING

### Compared against G1 accepted decisions

Reviewed G2 against P-001–P-005, P-096, IA-01/IA-06, SH-06, IX Section 29, UXA-019 through UXA-027, and the G1 ledger decisions.

### Result

**RECONCILIATION REQUIRED — one wording ambiguity; no BRD/business-policy conflict.**

### Finding BC-G2-01

G1 currently states that a multi-role account may switch among “all assigned workspaces without a second login.”

G2 correctly establishes that:
- ordinary switching among workspaces whose authentication requirements are already satisfied does not repeat username/password or TOTP;
- if Owner authority is added after a non-Owner session has already authenticated, Owner-capable access must remain gated until Owner second-factor requirements are satisfied.

These rules are compatible in intent, but G1 wording should distinguish:
- **assigned workspace** from
- **workspace whose security/authentication prerequisites are satisfied for the current session**.

### Planned reconciliation

Clarify G1 contracts so:
1. ordinary switching among already-authentication-qualified workspaces remains frictionless;
2. newly granted Owner authority is a security-sensitive exception requiring the Owner second-factor gate before Owner-capable use;
3. this gate is not described as a second username/password login;
4. account disablement is distinguished from role-only revocation: disablement ends all protected account use when detected, while role revocation removes the affected authority/workspace.

No other G1 decision conflicts with G2.


## 2026-09-24 — G2 BACKWARD COMPATIBILITY RECONCILED

### Reconciliation commit

- `5fe2bcc7151092307aa1d527d3b42863ec7e6144` — reconciled G2 authentication gates with G1 workspace/multi-role wording.

### Changes made

- G1 now distinguishes assigned workspaces from workspaces whose applicable authentication prerequisites are satisfied for the current session.
- Normal switching among already-authentication-qualified workspaces remains free of second username/password login and repeated Owner TOTP.
- Newly granted Owner authority remains gated until Owner second-factor requirements are satisfied.
- Role-only revocation continues to route the user to remaining permitted workspaces.
- Whole-account disablement is explicitly treated differently: protected account use returns to the sign-in boundary.

### Validation

PASS.

### Backward compatibility verdict

**G2 vs G1: PASS.**

No remaining business/product contradiction was found.

## 2026-09-24 — G2 FORWARD IMPACT ANALYSIS START

### Future groups scanned

- G3 Patient Identity
- G4 Visit/Payment/Waiver
- G5 Queue
- G6 Consultation
- G7 Prescription
- G8 Dispensing
- G9 Pharmacy Billing
- G10 Inventory
- G11 Owner Approval Center
- G12 Staff Administration
- G13 Reporting
- G14 Cross-Product State/Audit/Safety
- G15 Printing

### Preliminary result

No material G2-specific reminder is required for G3–G10, G13, or G15 beyond the global role/audit rules already tracked elsewhere.

Mandatory future coupling is concentrated in:
- **G11** — password reset is an action-specific Pending -> Resolved workflow, not generic Approve/Reject;
- **G12** — Owner role creation/grant, disable/re-enable, and active-session permission changes must respect G2 authentication gates;
- **G14** — authentication secret exclusion from audit, reset-request retry/stale safety, and session/account-state changes need cross-product safety coverage.

Next exact action: write targeted reminders and resolve REM-001 through REM-004 in Document 10.


## 2026-09-24 — G2 FORWARD IMPACT ANALYSIS COMPLETE

### No targeted G2 reminder required

After reviewing the current baseline of all remaining groups, no G2-specific reminder is required for:
- G3–G10;
- G13;
- G15.

Their normal role/authentication dependence is already covered by accepted G1/G2 contracts and does not create a unique unresolved future decision.

### New mandatory reminders recorded

**G11 — Owner Approval Center**
- REM-010 — password reset is Pending -> Resolved by Set Temporary Credential, not generic Approve/Reject.
- REM-011 — one actionable Pending reset request; stale/resolved Owner screens cannot reset again.

**G12 — Staff Administration**
- REM-012 — Owner creation/grant requires TOTP enrollment/second-factor qualification before Owner capability can be used.
- REM-013 — reset, disable/re-enable, and role-state changes remain separate lifecycles.

**G14 — Cross-Product State/Audit/Safety**
- REM-014 — auth secrets excluded from audit/history.
- REM-015 — retry/stale safety covers password-reset request lifecycle.
- REM-016 — session/tab handling after account/role state change.
- REM-017 — one-time recovery-code lifecycle and regeneration invalidation.

### Previous reminders resolved

REM-001 through REM-004 are now RESOLVED with explicit dispositions in Document 10.

### Next exact action

Run the complete G2 closure gate:
- Gate A current-group validation;
- Gate B backward compatibility;
- Gate C forward reminders;
- Gate D ledger currency;
then update PR #2 progress if all pass.


## 2026-09-24 — G2 FINAL CLOSURE

### Final gate results

- **Gate A — Current-group validation:** PASS
- **Gate B — Backward compatibility with G1:** PASS
- **Gate C — Forward impact/reminders:** COMPLETE
- **Gate D — Ledger/checkpoint currency:** PASS

### Main Group 2 commit

`f81210f2a7e99bcd21a32d5a4e288c0b530e68e0` — refined authentication, account access, password recovery, TOTP enrollment/recovery, forced-change, disabled-account, and auth-secret handling behavior.

### Backward reconciliation

`5fe2bcc7151092307aa1d527d3b42863ec7e6144` — clarified G1 workspace switching so ordinary authenticated switching remains frictionless while newly granted Owner authority remains second-factor gated.

### Forward-reminder commit

`521f67e2733f1d049c1595567356e353a1f62cb8` — resolved REM-001–REM-004 and created REM-010–REM-017 for G11/G12/G14.

### G2 final product verdict

- Locked BRD alignment: PASS
- Product completeness: HIGH
- Implementation readiness at PRD level: HIGH
- New clinic/business input required: NONE
- Deferred security architecture remains clearly separated from product policy.
- Known future-group coupling is captured in Document 10 rather than silently pre-solving G11/G12/G14.

### Stop condition

G2 is complete. Do not start G3 until explicitly instructed.


# G3 — Patient Search, Identity, Registration & Patient Profile

## Current checkpoint

**Stage:** COMMIT VALIDATED / BACKWARD COMPATIBILITY CHECK

### Primary requirements

- P-016 — Search first
- P-017 — Search result identity
- P-018 — Duplicate candidates
- P-019 — Possible Duplicate
- P-020 — New patient registration
- P-021 — Permanent Patient ID
- P-022 — Patient profile
- P-023 — Demographic correction
- P-024 — Doctor direct demographic correction
- P-025 — Missing physical file

### Mandatory prior-group reminders

Document 10 currently contains no reminder targeted to G3.

### Required source review

- locked BRD patient identity/search/registration/demographic-correction requirements and business rules;
- workflow/state model patient search, registration, duplicate handling, and correction paths;
- V1 decision-register entries for patient identity and duplicates;
- locked requirements traceability;
- current PRD P-016–P-025;
- Reception/Doctor patient-related screen contracts;
- interaction rules for search, forms, duplicate candidates, history, stale state, and role boundaries;
- accepted G1/G2 contracts to ensure patient context and access behavior stay compatible.

### Current action

Scan G4–G15 for dependencies or likely conflicts introduced by G3, record only targeted future reminders, then run final closure gates.

### Blockers

None.

---

## 2026-09-25 — G3 PREPARING checkpoint

- User instructed the refinement process to continue after G2 completion.
- Ledger moved to G3 PREPARING.
- No prior reminder is currently targeted to G3.
- PRD version checkpoint corrected from stale v0.3 to current v0.4.
- Next exact action: perform full G3 source review before product reasoning.


## 2026-09-25 — G3 SOURCE REVIEW COMPLETE

### Files read

- `docs/01-business-requirements-document.md`
- `docs/02-workflows-and-state-model.md`
- `docs/03-open-decisions-and-edge-cases.md`
- `docs/04-requirements-traceability.md`
- `docs/05-product-requirements-document.md`
- `docs/06-prd-traceability-and-acceptance.md`
- `docs/07-information-architecture-and-screen-specification.md`
- `docs/08-interaction-and-form-behavior-specification.md`
- `docs/09-prd-refinement-state-ledger.md`
- `docs/10-prd-future-review-reminders.md`

### Locked business conclusions confirmed

1. Patient ID is the permanent unique patient identifier and survives across visits.
2. Reception searches by Patient ID, phone, or name before new registration.
3. Phone/name are matching attributes, not unique identifiers; multiple patients may share a phone.
4. Similar candidates must be reviewed with the patient; similarity cannot auto-select or auto-merge.
5. If no candidate can be confidently confirmed, Reception may create a usable new profile marked **Possible Duplicate**.
6. Duplicate merge is outside V1.
7. Required registration fields are full name, phone, DOB, gender, address, email; age is derived.
8. Emergency contact, blood group, allergies, guardian/parent, and Government ID are optional.
9. Missing physical file does not justify a new Patient ID.
10. Reception cannot directly overwrite established demographics after registration.
11. Reception correction requests require Doctor authority; for an active Visit they route to the assigned Doctor, and an unassigned active Visit requires Doctor selection before submission.
12. Doctor may directly correct demographics; Patient ID never changes and prior/new values, actor, and time are auditable.
13. Reception may use only limited prior-Visit information for identity confirmation; unrestricted clinical history remains outside Reception authority.

### Product-definition gaps identified

- Exact Patient ID results should be prioritized without silently auto-entering patient context.
- Search result rows need a precise minimum identity set and a controlled way to expose limited prior-Visit identity context.
- New registration should re-check duplicate candidates at final submit so it does not rely only on an earlier manual search.
- When duplicate candidates exist, the product needs an explicit branch between selecting an existing patient and deliberately creating a Possible Duplicate.
- A Possible Duplicate marker needs useful provenance without creating V1 merge/resolution behavior.
- Reception patient profile wording currently risks implying longitudinal clinical-history access; it should be explicitly operational/identity-level only.
- The boundary between editable pre-registration data and post-registration demographic correction should be explicit.
- Demographic correction needs a safe no-active-Visit path while preserving Doctor authority.
- Pending demographic correction needs stale-value protection if the patient value changes before Doctor decision.
- Patient ID must remain immutable through every correction path.
- Unknown registration-submit outcome/retry must not create a second patient record.
- Physical-file association must not become a blocking CRM dependency or create a duplicate patient workflow.

### Prior reminders

No Document 10 reminder targets G3.

### Business input required

None. All identified gaps can be resolved as derived product design without changing the locked BRD.


## 2026-09-25 — G3 DECISIONS RESOLVED

No new clinic/business input is required. These are DERIVED PRODUCT DESIGN clarifications that preserve the locked patient-identity model.

### D-G3-01 — Search results require explicit human confirmation

- Search supports Patient ID, phone, and name exactly as locked.
- A valid exact Patient ID match is visually prioritized as the strongest identity result because Patient ID is unique, but the product still requires an explicit **Select Patient** action before entering patient context.
- Phone/name matches remain candidate results, never automatic identity proof.
- Each candidate row shows the minimum useful identity set: patient name, Patient ID, phone, DOB/derived age context, a compact address cue where useful, and Possible Duplicate status if present.
- Limited prior-Visit identity-confirmation context may be deliberately opened where needed, but Reception does not receive unrestricted diagnosis, clinical notes, or prescription content.
- Patients sharing the same phone remain distinct rows; results are never collapsed into one identity because of phone equality.

### D-G3-02 — Final registration performs a current duplicate-candidate check

- Search remains the visually dominant entry path, but duplicate protection cannot rely only on the receptionist having performed an earlier manual search.
- Before effective Patient creation, the product re-evaluates current candidate matches using the entered identifying data.
- If no candidate requires review, registration may proceed.
- If candidates are surfaced, Patient creation pauses and Reception must explicitly either:
  1. select an existing confirmed patient, abandoning the new identity creation; or
  2. deliberately continue with **Create New as Possible Duplicate** when the patient cannot confidently confirm an existing candidate.
- A candidate appearing at final submit because of concurrent/recent data is handled the same way; the system does not silently create a second Patient.
- Exact matching/scoring thresholds remain solution design.

### D-G3-03 — Possible Duplicate is usable, visible, and traceable

- Creating a new patient after unresolved candidate review automatically applies the visible **Possible Duplicate** marker.
- The marker is informational and does not block Visit creation or normal patient use.
- The product retains non-clinical provenance sufficient to understand why the marker exists: candidate Patient IDs surfaced at creation, creating actor, and time.
- No auto-merge, auto-link, or silent marker clearing is introduced in V1.
- Duplicate resolution/merge remains future scope.

### D-G3-04 — Registration edit boundary is explicit

- Before successful Patient creation, Reception may freely correct the unsaved registration fields.
- Required fields remain exactly: full name, phone, DOB, gender, address, email.
- Optional fields remain exactly: emergency contact, blood group, known allergies, guardian/parent details, Government ID.
- Phone is not unique; email is required but not an identity key; age remains derived from DOB.
- Structurally invalid input is rejected (for example, unusable required values or an impossible/future DOB), without inventing a new business field taxonomy.
- Patient ID is generated only after effective Patient creation succeeds, then becomes non-editable and immutable.
- After creation, established demographics move to the Doctor-controlled correction workflow rather than direct Reception overwrite.

### D-G3-05 — Registration retry/unknown outcome must protect identity

- Duplicate final submission is disabled while Patient creation is pending.
- If the client cannot determine whether Patient creation succeeded, it must not blindly submit another create request.
- The product first checks/refreshes effective state so a successful Patient creation is recovered rather than producing a second Patient.
- Detailed idempotency implementation remains technical and is a future G14 safety check.

### D-G3-06 — Physical file is an association, never an identity gate

- After Patient creation, the generated Patient ID is displayed copy-friendly so Reception can associate it with the physical clinic file.
- Missing/unavailable physical paper file does not block digital patient use, Visit creation, payment, queueing, or later retrieval.
- A missing file never causes another Patient ID.
- Locating/replacing the physical file remains offline clinic procedure; G3 does not invent a CRM file-replacement workflow.

### D-G3-07 — Reception patient profile is identity/operational, not clinical history

Reception Patient Profile may show:
- Patient ID and demographics;
- optional intake/operational safety context already permitted to Reception, including recorded allergy information where captured;
- Possible Duplicate marker;
- active Visit operational summary;
- prior Visit **identity/operational summaries sufficient for matching**, not longitudinal clinical content;
- physical-file Patient ID reference.

Unrestricted diagnoses, consultation notes, prescriptions, and Doctor longitudinal clinical history remain outside Reception authority.

### D-G3-08 — Demographic correction lifecycle is explicit

- Patient ID itself is immutable and is never a demographic-correction target.
- Reception cannot directly overwrite an established demographic.
- Each proposed demographic field change is independently attributable with current value and proposed value; the UI may collect more than one change, but each field must preserve its own old/new audit detail.
- While a Reception request is Pending, the effective patient value does not change.
- Approved -> proposed value becomes effective and old/new, requester, Doctor decision, and time are retained.
- Rejected -> effective value remains unchanged and decision history remains.
- A Doctor may directly correct demographics without a Reception request, but old/new value, actor, and time remain audited.

### D-G3-09 — Doctor routing works with and without an active Visit

For a Reception-originated correction:
- if an active Visit has an assigned Doctor, route to that Doctor;
- if an active Visit exists but has no assigned Doctor, a Doctor must be selected/assigned before the request can be submitted, matching FR-006;
- if there is **no active Visit**, do not create a fake Visit merely to correct demographics; Reception selects an authorized Doctor reviewer and the request remains a patient-level correction.

A submitted request does not disappear merely because the Visit later completes.

If the active Visit is reassigned before the pending request is decided, the correction should follow the current assigned Doctor so the business rule “route to the Visit's assigned Doctor” remains true. This requires a future G5 compatibility check.

### D-G3-10 — Stale demographic correction cannot overwrite newer truth

- A Reception correction request captures the current field value it is proposing to replace.
- If that value has changed before Doctor decision, the pending request becomes stale for application purposes.
- The Doctor must review refreshed current data; the stale proposal cannot silently overwrite the newer value.
- The Doctor may reject/recreate the request path or directly perform an audited correction using current data.
- Exact concurrency/version mechanism remains technical and is a future G14 safety check.

### Explicitly not introduced

- duplicate merge;
- automatic identity selection solely by similarity score;
- retrospective auto-merging/linking;
- DOB-only, email-only, Government-ID-only, or pharmacy-expanded search requirements;
- mandatory clinical detail exposure to Reception;
- physical-file replacement workflow;
- new required registration fields;
- Patient ID editing/replacement.


## 2026-09-25 — G3 GROUP COMMIT + COMMIT VALIDATION

### Group commit

- `b7db51edcd73f650a8e3f27bc7e98d21f4e3a438`
- Files changed:
  - `docs/05-product-requirements-document.md`
  - `docs/06-prd-traceability-and-acceptance.md`
  - `docs/07-information-architecture-and-screen-specification.md`
  - `docs/08-interaction-and-form-behavior-specification.md`

### Validation result

PASS.

- P requirements remain exactly P-001 through P-116 with no duplicate IDs.
- Screen inventory remains 49 unique screen IDs.
- UX interaction scenarios remain unique and now extend through UXA-036.
- PRD/companion versions align at PRD v0.5.
- Explicit Patient selection, current duplicate recheck, Possible Duplicate provenance/usability, Reception clinical boundary, no-active-Visit correction routing, stale correction protection, physical-file non-blocking behavior, unknown-create safety, and Patient ID immutability are all present.

### Next exact action

Evaluate G3 compatibility cumulatively against G1 workspace/authority rules and G2 authentication/account-state rules.


## 2026-09-25 — G3 BACKWARD COMPATIBILITY COMPLETE

### Compared against

- G1 workspace, patient-context, permission, multi-role, and audit-authority rules;
- G2 authentication, account-state, and disabled-session rules.

### Result

**PASS — no reconciliation commit required.**

### Compatibility reasoning

- G3's explicit **Select Patient** before patient context strengthens G1's rule against silent context changes.
- Reception Patient Profile remains identity/operational only and does not violate G1's clinical-content boundary.
- G3 Doctor-controlled corrections preserve role authority rather than giving Reception new write authority.
- G3 Patient ID immutability does not conflict with any prior group.
- All G3 screens remain behind the normal G2 authentication/account-state boundary.
- No G3 rule weakens Owner 2FA, disabled-account behavior, workspace authority, or multi-role attribution.

### Next exact action

Evaluate G3 against all future groups G4–G15 and write targeted reminders only where a real dependency exists.


## 2026-09-25 — G3 FORWARD IMPACT ANALYSIS COMPLETE

### Future groups scanned

G4 through G15 were reviewed against the committed G3 identity/correction behavior.

### Targeted reminders created

**G4 — Visit Creation & Payment**
- REM-018 — Visit creation links the existing/newly-created permanent Patient ID; Possible Duplicate/missing physical file never create/block identity.
- REM-019 — active Visit without Doctor must support Doctor selection before a Visit-linked demographic correction can submit; no fake Visit for no-active-Visit correction.

**G5 — Queue & Visit Flow**
- REM-020 — pending demographic correction follows current assigned Doctor after reassignment.

**G6 — Consultation & Clinical History**
- REM-021 — Doctor correction/approval + stale protection; no automatic clinical-history combination across Possible Duplicate Patient IDs.

**G13 — Reporting**
- REM-022 — unique/returning patient metrics respect actual Patient IDs; no similarity-based reporting dedupe in V1.

**G14 — Cross-Product State/Audit/Safety**
- REM-023 — Patient-create unknown outcome/idempotency and concurrent duplicate-candidate check.
- REM-024 — Possible Duplicate provenance and demographic-correction audit metadata.
- REM-025 — stale demographic-correction protection.

### No targeted G3 reminder required

No unique unresolved G3 dependency was found for G7–G12 except the groups explicitly listed above, or for G15. Existing permanent Patient ID and role rules are sufficient for those areas.

### Next exact action

Run all four G3 closure gates, then update the PR checklist if all pass.


## 2026-09-25 — G3 FINAL CLOSURE

### Final gate results

- **Gate A — Current-group validation:** PASS
- **Gate B — Backward compatibility with G1–G2:** PASS
- **Gate C — Forward impact/reminders:** COMPLETE
- **Gate D — Ledger/checkpoint currency:** PASS

### Main Group 3 commit

`b7db51edcd73f650a8e3f27bc7e98d21f4e3a438` — refined patient search, candidate confirmation, registration duplicate protection, Possible Duplicate behavior, Reception patient profile boundaries, demographic correction, physical-file independence, and Patient-create retry safety.

### Backward compatibility

No reconciliation commit was required.

G3 strengthens G1 patient-context safety through explicit selection, stays within Reception/Doctor authority boundaries, and remains behind G2 authentication/account-state gates.

### Forward-reminder commit

`b8691d8c526aa5953d1e48cd67afe088abf15493` — created REM-018–REM-025 for G4, G5, G6, G13, and G14.

### G3 final product verdict

- Locked BRD alignment: PASS
- Product completeness: HIGH
- Implementation readiness at PRD level: HIGH
- New clinic/business input required: NONE
- Duplicate merge remains outside V1.
- Technical matching thresholds, idempotency implementation, and concurrency/version mechanisms remain correctly deferred.
- No unresolved backward conflict remains.

### Stop condition

G3 is complete. G4 has not been started.


# G4 — Visit Creation, Consultation Payment, Waiver & Payment Correction

## Current checkpoint

**Stage:** COMMIT VALIDATED / BACKWARD COMPATIBILITY CHECK

### Primary requirements

- P-026 — New Visit ID
- P-027 — Doctor assignment
- P-028 — Consultation fee display
- P-029 — External payment
- P-030 — Fast Paid action
- P-031 — Queue gate
- P-032 — No partial consultation payment
- P-033 — No consultation refund
- P-034 — Request waiver
- P-035 — Owner approval
- P-036 — Owner direct waiver
- P-037 — Pending waiver gate
- P-038 — Payment correction request
- P-039 — Payment correction decision

### Mandatory prior-group reminders

- REM-018 — preserve the Patient-to-Visit boundary; Visit creation links the existing/newly-created Patient ID; Possible Duplicate/missing physical file do not block Visit creation.
- REM-019 — active Visit without Doctor must support Doctor selection before a Visit-linked demographic correction can submit; no fake Visit is created solely for a correction when no active Visit exists.

### Required source review

- locked BRD Visit/payment/waiver/payment-correction requirements and business rules;
- workflow/state model for Visit creation, payment gate, waiver, payment corrections;
- decision-register entries OD-004, OD-005 and payment-related decisions;
- locked traceability;
- current PRD P-026–P-039;
- Reception/Owner Visit/payment/waiver/correction screens;
- interaction contracts for payment capture, approvals, reason fields, stale state, retries, confirmations;
- accepted G1–G3 contracts and REM-018/REM-019.

### Current action

Scan G5–G15 for future dependencies introduced by G4, record targeted reminders, resolve REM-018/REM-019, then run final closure gates.

### Blockers

None.

---

## 2026-09-25 — G4 PREPARING checkpoint

- G4 started immediately after G3 closure.
- Mandatory prior reminders: REM-018, REM-019.
- No product changes are being made until source review is complete.


## 2026-09-25 — G4 SOURCE REVIEW COMPLETE

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

1. Each clinic attendance creates a unique Visit ID linked to the permanent Patient ID; returning patients do not receive a new Patient ID.
2. Consultation payment execution remains outside the CRM; the CRM records payment state/information only.
3. Normal consultation financial states are Paid and Unpaid; approved waiver is the separate Waived outcome.
4. Paid and Waived are queue-eligible; Unpaid is blocked.
5. Partial consultation payment and consultation refund do not exist in V1.
6. UPI/Cash/Card/Other are the default methods; Other requires description; reference is optional.
7. Reception or Doctor may request waiver with a mandatory reason; Owner alone approves/rejects unless the user also holds Owner authority.
8. Owner may directly waive without a second approval; reason/audit remain mandatory.
9. Pending/rejected waiver leaves the Visit financially Unpaid and queue-blocked.
10. Incorrect payment records use staff request -> Owner decision; original and resulting record remain auditable; correction is not a refund.
11. Doctor-specific queue entry requires a Doctor assignment.
12. FR-006/G3 explicitly allow an active Visit to exist before Doctor assignment.
13. Fee values are clinic configuration rather than PRD policy.

### Mandatory reminder dispositions entering reasoning

- REM-018: ACTIVE IN REVIEW — G4 must ensure Visit creation reuses the selected Patient ID and Possible Duplicate/missing paper file do not block Visit creation.
- REM-019: ACTIVE IN REVIEW — G4 must support an active unassigned Visit and Doctor assignment before queue entry / Visit-linked demographic-correction submission.

### Product-definition gaps identified

- current REC-05 wording implies Doctor selection/new Visit/payment/queue are one inseparable operation even though locked rules permit an unassigned active Visit;
- initial Visit financial state and fee-snapshot behavior are not explicit;
- Paid recording and queue insertion can be a combined high-velocity action, but partial success/failure behavior is undefined;
- Doctor-requested waiver has no reachable product path because an Unpaid Visit is not yet in the Doctor queue;
- waiver request applicability and duplicate/stale request behavior are underspecified;
- pending waiver can become stale if the patient pays externally before Owner decision;
- Owner direct waiver needs an explicit product path;
- payment-correction fields/state transitions and stale-request behavior are underspecified;
- payment correction can occur after the Visit has already progressed, but no rule should silently rewind clinical/queue history;
- payment-record retries/unknown outcomes need identity-like duplicate protection;
- consultation-fee configuration changes must not silently rewrite historical Visit financial records.

### Business input required

None. These gaps can be resolved as derived product behavior without changing the locked payment/waiver policy.


## 2026-09-25 — G4 DECISIONS RESOLVED

No new clinic/business input is required. These decisions are derived product behavior consistent with the locked Visit/payment rules.

### D-G4-01 — Visit creation is distinct from queue entry

- A Visit is created only against the already selected/newly created permanent Patient ID; Visit creation never creates or replaces patient identity.
- Possible Duplicate status and missing paper file do not block Visit creation.
- Effective Visit creation generates the Visit ID and establishes the Visit as the attendance record.
- The initial consultation financial outcome is **Unpaid** until Paid or Waived becomes effective.
- Doctor assignment is optional at the moment the Visit is created because FR-006/G3 explicitly allow an active Visit with no Doctor yet assigned.
- Doctor assignment becomes mandatory before queue entry.
- Reception may assign the Doctor at Visit creation or later.
- If G3 needs a Visit-linked demographic correction while the Visit is unassigned, Doctor selection/assignment must occur before that request submits.
- No fake Visit is created for a demographic correction when no active Visit exists.
- No artificial “one Visit per patient per day” rule is introduced; separate legitimate attendances remain possible.

Disposition: REM-018 and REM-019 are satisfied subject to post-commit validation.

### D-G4-02 — Consultation fee is a Visit-level applied amount

- REC-05 displays the applicable fee from clinic configuration.
- When the Visit is effectively created, the applied consultation fee is captured for that Visit so later configuration changes do not silently rewrite historical/current Visit financial records.
- Future fee-configuration changes are prospective for newly created Visits unless a separately authorized correction is applied to a specific Visit.
- This does not define the clinic fee value itself; the value remains configuration.

### D-G4-03 — Consultation financial-state model

Normal effective outcomes:

- **Unpaid** — default at Visit creation; queue-blocked.
- **Paid** — queue-eligible.
- **Waived** — Owner-approved/direct financial exception; queue-eligible and distinct from Paid.

Normal transitions:
- Unpaid -> Paid through explicit payment confirmation;
- Unpaid -> Waived through Owner-approved request or Owner direct waiver.

Normal UI does not provide:
- Paid -> refund;
- partial payment;
- Waived -> Paid;
- Paid -> Waived.

Incorrect recorded payment information is handled only through the controlled payment-correction workflow.

### D-G4-04 — Paid capture is explicit and auditable

To record Paid:
- use the Visit's full effective consultation amount;
- select UPI/Cash/Card/Other;
- Other requires description;
- reference is optional;
- perform an explicit final **Mark Paid** action;
- record actor/time and the effective payment information.

Selecting a payment method alone never changes state.

A Visit already Paid or Waived does not expose the normal Mark Paid action again; incorrect records use payment correction.

### D-G4-05 — Financial eligibility and Doctor assignment independently gate queue entry

Queue entry requires both:
1. financial eligibility = Paid or Waived;
2. Doctor assigned.

Therefore:
- Paid + no Doctor -> Paid, not queued; assign Doctor later.
- Waived + no Doctor -> Waived, not queued; assign Doctor later.
- Doctor assigned + Unpaid -> not queued.
- Pending/rejected waiver + Unpaid -> not queued.

A high-velocity **Mark Paid & Add to Queue** action may be offered only when a Doctor is already assigned.

If Paid is confirmed but queue insertion subsequently fails or becomes stale:
- do not hide/rollback the confirmed Paid record merely to make the combined UI look atomic;
- show the Visit as Paid but not queued;
- allow safe queue-entry retry after current state is refreshed.

If payment outcome itself is unknown, do not queue until Paid is confirmed.

### D-G4-06 — Waiver request applicability and lifecycle

A waiver request may be created only while the effective consultation financial outcome is Unpaid.

For Reception/Doctor request:
- mandatory specific reason;
- one simultaneously actionable Pending waiver request per Visit;
- repeat submission while Pending does not create duplicate Owner work;
- underlying financial outcome remains Unpaid while Pending;
- Owner approval -> Waived;
- Owner rejection -> remains Unpaid;
- rejected request remains historical and a later new request may be submitted while still Unpaid with a new reason.

If the Visit becomes Paid while a waiver request is Pending:
- the pending waiver becomes stale/non-actionable;
- Owner cannot later approve it into Waived against the newer Paid state;
- request/history remains visible.
- Exact generic stale/no-longer-applicable representation is deferred to G11/G14, but duplicate/stale approval is prohibited now.

Waiver is not offered for already Paid or already Waived Visits.

### D-G4-07 — Doctor-requested waiver must be reachable without queue entry

Because an Unpaid Visit cannot enter the Doctor queue, Doctor waiver authority cannot depend on queue presence.

For a Visit assigned to a Doctor but not yet queue-eligible:
- Doctor workspace exposes a separate **assigned Visits awaiting financial eligibility** context;
- this context is not part of the ordered Doctor queue;
- it shows only enough Patient/Visit/payment context for the Doctor to submit a waiver request;
- it does not open clinical consultation merely because the Doctor can request waiver.

This must be reconciled with G5 Doctor Home/queue design later.

### D-G4-08 — Owner direct waiver is an immediate Owner-authority action

For an Unpaid Visit, Owner may explicitly choose **Direct Waiver**:
- identify Visit/current Unpaid amount;
- enter mandatory specific reason;
- explicitly confirm;
- effective outcome immediately becomes Waived;
- no second approval/request record is created;
- actor/Owner authority/reason/time are audited.

A multi-role Owner+Doctor uses Owner authority for this action.

Paid/Waived Visits do not expose direct waiver as a normal action.

G11 must provide a reachable Owner path for this action without pretending it is a pending request.

### D-G4-09 — Payment correction operates on a captured baseline

A Reception consultation-payment correction request includes:
- Visit/payment identity;
- current effective financial record;
- proposed corrected financial record;
- mandatory specific reason.

The proposed correction may adjust:
- Paid/Unpaid status where the recorded status itself was wrong;
- payment method;
- Other method description;
- optional external reference;
- the Visit-specific recorded consultation amount if that amount itself was recorded incorrectly.

A Visit-specific amount correction does not change clinic fee configuration.

If proposed effective state is Paid:
- it represents the full corrected effective consultation amount;
- method requirements apply;
- no partial-payment representation is introduced.

Waiver is a separate Owner financial-exception record and is not silently created/revoked through payment correction.

### D-G4-10 — Payment correction is non-destructive and stale-safe

- only one simultaneously actionable payment-correction request for the same current payment baseline should exist;
- Pending request does not change effective payment/queue state;
- Owner approval creates the corrected effective financial record while preserving original/request/reason/decision/time;
- rejection leaves effective record unchanged;
- if the effective payment baseline changes before Owner decision, the pending request is stale and cannot be applied against the newer record.

A Paid -> Unpaid correction means the original Paid record was wrong; it is explicitly **not a refund**.

### D-G4-11 — Payment correction does not rewind already-created clinical/queue history

Financial correction changes the effective financial record, not historical reality.

- before queue entry, corrected state immediately governs future queue eligibility;
- after the Visit has already entered/progressed through the queue/consultation workflow, a later correction does not delete, rewind, or silently undo queue/clinical history;
- the corrected financial state remains visible/auditable.

G5 must evaluate how an already-active queued Visit displays/handles a later financial correction without inventing retroactive deletion.

### D-G4-12 — Payment/Visit retry safety

- prevent duplicate final payment confirmation while submitting;
- if payment outcome is unknown, refresh/check effective Visit/payment state before another Paid attempt;
- do not create duplicate payment records by blind retry;
- Visit creation has the same unknown-outcome protection: determine whether the Visit already exists before repeating create;
- exact idempotency mechanism remains technical and is a G14 review obligation.

### Explicitly not introduced

- CRM payment processing/gateway wait;
- partial consultation payment;
- refund;
- automatic Paid state from method selection;
- auto queue entry without Doctor assignment;
- automatic rollback of a valid Paid record because queue insertion failed;
- waiver approval by Reception or Doctor-only authority;
- normal waiver of a Paid Visit;
- direct Owner payment correction without a staff request;
- retroactive deletion/rewind of queue or clinical history after financial correction;
- one-Visit-per-day restrictions;
- fee configuration values.


## 2026-09-25 — G4 GROUP COMMIT + COMMIT VALIDATION

### Group commit

- `38df611096205195a219abbaf498187563c10e90`
- Files changed:
  - `docs/05-product-requirements-document.md`
  - `docs/06-prd-traceability-and-acceptance.md`
  - `docs/07-information-architecture-and-screen-specification.md`
  - `docs/08-interaction-and-form-behavior-specification.md`

### Validation result

PASS.

- P requirements remain exactly P-001 through P-116 with no duplicate IDs.
- Screen inventory remains 49 unique screen IDs.
- UX acceptance remains unique and now extends through UXA-046.
- PRD/companion versions align at PRD v0.6.
- Visit starts Unpaid, Doctor assignment may occur later, queue requires Paid/Waived + Doctor, Possible Duplicate/missing paper file do not block Visit creation, Doctor pre-queue waiver path is reachable, one Pending waiver is enforced, stale waiver/payment corrections are blocked, Owner direct waiver has no redundant approval, fee snapshot behavior is present, and Visit/payment unknown-outcome retry is protected.
- Two automated checks initially returned false negatives because their regex expected different wording for “Possible Duplicate does not block Visit” and “Visit starts Unpaid.” The source text was verified as correct; product text was not distorted to satisfy the validator.

### Next exact action

Evaluate G4 compatibility cumulatively against G1 workspace/authority rules, G2 authentication/account-state rules, and G3 patient identity/demographic-correction rules.


## 2026-09-25 — G4 BACKWARD COMPATIBILITY COMPLETE

### Compared against

- G1 workspace, authority, multi-role, direct-navigation, and audit-attribution contracts;
- G2 authentication/account-state contracts;
- G3 Patient identity, Possible Duplicate, physical-file, and demographic-correction contracts.

### Result

**PASS — no reconciliation commit required.**

### Compatibility reasoning

- Owner direct waiver and Owner waiver decisions remain explicitly Owner-authority actions, consistent with G1.
- Doctor-requested waiver remains Doctor authority and does not grant Owner approval power.
- G2 authentication boundaries remain unchanged.
- Visit creation always links the already-selected Patient ID and never creates/replaces patient identity.
- Possible Duplicate and missing paper file remain non-blocking for Visit creation.
- Active Visit may exist Unassigned, matching the G3/BRD demographic-correction edge case.
- Doctor assignment is required before a Visit-linked demographic correction can submit, satisfying REM-019.
- No fake Visit is created solely for a no-active-Visit demographic correction.
- REM-018 and REM-019 are therefore resolved by G4 subject to final reminder-register update.

### Next exact action

Evaluate G4 against G5–G15 and create only targeted future reminders.


## 2026-09-25 — G4 FORWARD IMPACT ANALYSIS COMPLETE

### Targeted reminders created

**G5 — Doctor Queue**
- REM-026 — pre-queue financial list remains separate from actual queue; queue requires Doctor + Paid/Waived.
- REM-027 — later financial correction does not rewind queue history; G5 must define safe active presentation/actions.

**G9 — Pharmacy Billing**
- REM-028 — reuse common payment/correction/retry semantics without importing consultation waiver.

**G11 — Owner Approval Center**
- REM-029 — waiver lifecycle, stale-after-Paid behavior, later request after rejection, Direct Waiver as non-request.
- REM-030 — payment-correction baseline revalidation and non-refund presentation.

**G12 — Configuration**
- REM-031 — fee configuration is prospective; existing Visit amount is not silently rewritten.

**G13 — Reporting**
- REM-032 — revenue uses current effective Paid records, excludes Waived, and does not double-count audit originals.

**G14 — Cross-Product State/Safety**
- REM-033 — Visit/payment idempotency, combined-action partial success, one Pending request, stale waiver/correction.

### Prior reminders resolved

- REM-018 — RESOLVED.
- REM-019 — RESOLVED.

### No targeted G4 reminder required

No unique unresolved G4 dependency was found for G6–G8, G10, or G15 beyond existing shared Patient/Visit/payment/audit contracts.

### Next exact action

Run all four G4 closure gates, update ledger/PR if PASS, then proceed directly to G5.


## 2026-09-25 — G4 FINAL CLOSURE

### Final gate results

- **Gate A — Current-group validation:** PASS
- **Gate B — Backward compatibility with G1–G3:** PASS
- **Gate C — Forward impact/reminders:** COMPLETE
- **Gate D — Ledger/checkpoint currency:** PASS

### Main Group 4 commit

`38df611096205195a219abbaf498187563c10e90` — refined Visit creation, consultation financial states, Doctor assignment, queue gating, payment capture, waiver, direct waiver, payment correction, fee snapshot, and retry behavior.

### Backward compatibility

No reconciliation commit was required.

### Forward-reminder commit

`1e0d53ae6072131274bf99b103683f6717acaf67` — resolved REM-018/019 and created REM-026–REM-033.

### G4 final product verdict

- Locked BRD alignment: PASS
- Product completeness: HIGH
- Implementation readiness at PRD level: HIGH
- New clinic/business input required: NONE
- Payment processing/refund/partial-payment remain outside V1.
- No unresolved backward conflict remains.

### Transition

Proceed directly to G5 under the user's continuous-review instruction.


# G5 — Doctor Queue & Visit Flow Control

## Current checkpoint

**Stage:** COMMIT VALIDATED / BACKWARD COMPATIBILITY CHECK

### Primary requirements

- P-040 — Queue row information
- P-041 — Doctor call
- P-042 — Reassignment
- P-043 — Unresponded
- P-044 — Patient leaves
- P-045 — Urgent case
- P-046 — Doctor cancellation request
- P-047 — Owner cancellation decision

### Mandatory prior-group reminders

- REM-020 — pending demographic correction follows the current assigned Doctor after queue reassignment; previous Doctor must not retain stale decision authority.
- REM-026 — assigned pre-queue financial Visits remain separate from ordered Doctor queue; queue requires Doctor + Paid/Waived.
- REM-027 — later payment correction must not erase/rewind queue history; G5 must define safe active presentation/actions when effective financial state changes after queue entry.

### Required source review

- locked BRD queue states, doctor-specific queue, calling, reassignment, Unresponded, patient-leave, urgent, and Visit-cancellation rules;
- workflow/state model for queue and cancellation;
- OD-006/OD-007/OD-019 and related edge decisions;
- current PRD P-040–P-047;
- Reception/Doctor/Owner queue/cancellation screens;
- interaction contracts for queue, state transitions, stale data, confirmations, approvals;
- G1–G4 accepted contracts and REM-020/REM-026/REM-027.

### Current action

Compare committed G5 behavior against completed G1–G4 contracts; reconcile any ambiguity/conflict before forward-impact analysis.

### Blockers

None.

---

## 2026-09-25 — G5 PREPARING checkpoint

- G5 started immediately after G4 closure.
- Mandatory reminders loaded: REM-020, REM-026, REM-027.
- Next exact action: complete source review before product decisions.


## 2026-09-25 — G5 SOURCE REVIEW COMPLETE

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

1. Consultation queues are Visit-based and Doctor-specific.
2. Reception sees active Doctor queues; each Doctor sees that Doctor's queue; Owner sees clinic-wide status; Pharmacy has no Doctor-queue controls.
3. Queue states are Waiting, Called, Unresponded, With Doctor, Consultation Completed, Sent to Pharmacy, Completed, Cancelled/Voided.
4. Unresponded is transient and returns to Waiting after repositioning five positions downward, or end if fewer remain.
5. Doctor may call a selected queued patient; Reception sees the call.
6. Reception may reassign a queued Visit between Doctors and reassignment is audited.
7. No urgent-priority flag/request/reordering exists.
8. Paid patient leaving before consultation is moved to the end of the assigned Doctor queue.
9. Reception cannot cancel Visit.
10. Doctor may request cancellation from Waiting, Called, Unresponded, With Doctor, Consultation Completed, or Sent to Pharmacy with specific reason.
11. Owner approves/rejects; Pending leaves Visit active; approval -> Cancelled/Voided and removes it from active workflow while preserving all history; Completed is not cancellable.
12. No refund occurs because of Visit cancellation.

### Mandatory reminder issues confirmed

- REM-020: reassignment must transfer pending demographic-correction reviewer authority to current Doctor.
- REM-026: pre-queue assigned financial Visits must remain visually/semantically separate from actual queue.
- REM-027: later financial correction must preserve queue history while defining safe current operational behavior.

### Product-definition gaps identified

- default queue insertion/order semantics are not explicit;
- callable/start-consultation state boundaries are underspecified;
- reassignment eligibility, destination position, and Called-state behavior are not explicit;
- reassignment impact on pending demographic correction is not yet represented in queue interaction;
- Unresponded reposition needs current-state/stale protection;
- patient-leaves action scope and state result need explicit behavior;
- G4's Paid/Waived pre-queue list must not bleed into actual queue counts/order;
- G4 financial correction to Unpaid after queue entry needs safe current handling without erasing queue history;
- pending cancellation may coexist with active workflow, but stale decision after Visit becomes Completed is not defined;
- duplicate Pending cancellation requests should be prevented;
- cancellation approval during concurrent Doctor/Pharmacy work needs stale-state protection;
- queue waiting-time timestamp must not be silently reset by ordinary reassignment/repositioning.

### Business input required

None. The gaps can be resolved as derived product design while preserving the locked state model.


## 2026-09-25 — G5 DECISIONS RESOLVED

No new business input is required.

1. **Queue membership/order:** actual queue contains only Doctor-assigned, Paid/Waived Visits. New entries append to the selected Doctor's queue end. Pre-queue financial Visits remain outside queue counts/order. No drag/drop/manual priority reorder.
2. **Queue timing:** preserve queue-entry history through reassignment, Unresponded reposition, and move-to-end. A true removal/re-entry records a new active queue-entry event while retaining prior history.
3. **Call/start:** Call Patient is Doctor-only from Waiting and creates Called. Start Consultation is explicit from Called, current assigned Doctor only, and creates With Doctor. Viewing a row does not change state.
4. **Reassignment:** Reception may reassign Waiting or Called Visit only. Destination Doctor must differ. Destination receives the Visit at queue end as Waiting. Called history is retained. With Doctor or later cannot be reassigned.
5. **Demographic correction routing:** reassignment moves any pending Visit-linked demographic-correction reviewer authority to the new Doctor; old Doctor stale view cannot decide. Resolves REM-020.
6. **Unresponded:** Reception-only from Called; record event, move five positions down or to end, return current state to Waiting; stale action is blocked.
7. **Patient leaves before consultation:** from Waiting/Called, move financially eligible Paid/Waived Visit to same-Doctor queue end and return/keep Waiting. This is not cancellation/refund. Waived follows Paid operationally because G4 made both queue-eligible.
8. **Financial correction before consultation:** if effective state becomes Unpaid while Waiting/Called, remove current queue membership but preserve queue history; show Not Queued/Unpaid. Re-entry after renewed eligibility is explicit and goes to queue end.
9. **Financial correction after With Doctor:** do not unwind clinical workflow; corrected financial state remains visible/auditable.
10. **Urgency:** no priority flag, urgency score, priority request, drag reorder, or automatic jump.
11. **Cancellation request:** Doctor-only with specific reason from Waiting/Called/Unresponded/With Doctor/Consultation Completed/Sent to Pharmacy; not from Completed/Cancelled. One actionable Pending request per Visit.
12. **Pending cancellation:** Visit remains active and may progress. If it reaches Completed before Owner decision, request becomes non-actionable/stale.
13. **Owner cancellation decision:** revalidate current state. Approval -> Cancelled/Voided and active workflow removal while preserving all existing history and no refund. Rejection leaves current state unchanged. Same-human Owner+Doctor remains separately attributed.
14. **Stage effects:** cancellation removes Waiting/Called from queue; stops further active progression from With Doctor/Consultation Completed/Sent to Pharmacy while preserving already-created clinical/pharmacy history.
15. **Concurrency:** every queue action revalidates state and Doctor assignment before applying; stale call/start/reassign/Unresponded/move/cancellation actions refresh instead of applying outdated state.

Future checks are required for G6 clinical concurrency, G8/G9 cancellation after pharmacy handoff, G11 cancellation approvals, G13 waiting-time reporting, and G14 state/concurrency safety.


## 2026-09-25 — G5 GROUP COMMIT + COMMIT VALIDATION

### Group commit

- `eec1ae4e4b951456798eb008c710e0d305a1aa50`
- Files changed:
  - `docs/05-product-requirements-document.md`
  - `docs/06-prd-traceability-and-acceptance.md`
  - `docs/07-information-architecture-and-screen-specification.md`
  - `docs/08-interaction-and-form-behavior-specification.md`

### Validation result

PASS.

- P requirements remain P-001 through P-116 with no duplicate IDs.
- Screen inventory remains 49 unique screen IDs.
- UX acceptance remains unique and now extends through UXA-056.
- PRD/companion versions align at PRD v0.7.
- Queue-end insertion, pre-queue separation, Waiting->Called->With Doctor boundary, reassignment-at-end, demographic reviewer transfer, Unresponded history, financial removal before consultation, non-destructive later correction, pending-cancellation behavior, Completed stale-cancellation protection, and queue-action concurrency are present.
- One automated validation initially missed the cancellation-history requirement due to regex wording. P-047 was manually verified to preserve queue, financial, clinical, prescription, dispensing, billing, and request history; product text was not changed to satisfy the validator.

### Next exact action

Evaluate G5 cumulatively against G1 workspace/authority, G2 auth/account-state, G3 patient/demographic-correction, and G4 Visit/payment/waiver contracts.


## 2026-09-25 — G5 BACKWARD COMPATIBILITY FINDING

### Result

**RECONCILIATION REQUIRED — one G4 wording ambiguity; no BRD/business-policy conflict.**

### Finding BC-G5-01

G4 P-039 / IX 37.8 says a later financial correction does not delete/rewind prior queue/clinical history, but it does not explicitly distinguish:
- preserving historical queue events; from
- preserving current active queue membership.

G5 now defines the operational consequence:
- if correction to Unpaid occurs while Waiting/Called, remove current queue membership non-destructively and preserve all queue history;
- if correction occurs With Doctor or later, do not unwind clinical workflow.

### Planned reconciliation

Clarify G4 wording to reference the G5 stage-sensitive operational effect while retaining the original non-destructive principle.

No compatibility conflict was found with G1, G2, or G3.


## 2026-09-25 — G5 BACKWARD COMPATIBILITY RECONCILED

- G1–G3: PASS without changes.
- G4 ambiguity reconciled by `8423beaa1cf8f5796a5aae57b7ee708a4ae14d5f`.
- G4 now explicitly distinguishes immutable queue history from current queue membership after financial correction.
- Waiting/Called may leave current queue when corrected to Unpaid; With Doctor or later is not unwound.

## 2026-09-25 — G5 FORWARD IMPACT ANALYSIS COMPLETE

Created:
- REM-034 -> G6 clinical start/cancellation concurrency.
- REM-035 -> G7 prescription behavior after Visit cancellation.
- REM-036 -> G8 stop future dispensing after Visit cancellation.
- REM-037 -> G9 billing/payment after Visit cancellation.
- REM-038 -> G11 cancellation approval lifecycle.
- REM-039 -> G13 waiting-time reporting with multiple queue-entry events.
- REM-040 -> G14 queue/cancellation concurrency.

Resolved:
- REM-020
- REM-026
- REM-027

No unique G5-specific reminder was required for G10, G12, or G15.

### Next exact action

Run all four G5 closure gates; if PASS, close G5 and proceed directly to G6.


## 2026-09-25 — G5 FINAL CLOSURE

### Final gate results

- Gate A — Current-group validation: PASS
- Gate B — Backward compatibility: PASS after G4 reconciliation `8423beaa1cf8f5796a5aae57b7ee708a4ae14d5f`
- Gate C — Forward impact/reminders: COMPLETE
- Gate D — Ledger/checkpoint currency: PASS

### Main Group 5 commit

`eec1ae4e4b951456798eb008c710e0d305a1aa50`

### Final verdict

- BRD alignment: PASS
- Product completeness: HIGH
- Implementation readiness: HIGH
- New business input required: NONE
- REM-020/026/027 resolved.
- REM-034–REM-040 created.
- No unresolved backward conflict remains.

### Transition

Proceed directly to G6.


# G6 — Consultation & Longitudinal Clinical Record

## Current checkpoint

**Stage:** COMMIT VALIDATED / BACKWARD COMPATIBILITY CHECK

### Primary requirements

- P-048 — Assigned Visit access
- P-049 — Longitudinal history
- P-050 — Required clinical entry
- P-051 — Optional clinical entry
- P-052 — Complete consultation
- P-053 — Clinical amendment
- P-054 — Clinical access boundary

### Mandatory prior-group reminders

- REM-021 — Doctor demographic-correction approval/direct edit must preserve old/new audit and stale protection; Possible Duplicate Patient IDs must never cause automatic clinical-history combination.
- REM-034 — Start Consultation must preserve Called -> With Doctor; Pending cancellation does not freeze consultation, but effective cancellation while With Doctor stops future active authoring/saves while preserving already-saved clinical history.

### Required source review

- locked BRD clinical documentation, Doctor authority, longitudinal archive, amendments, and access boundaries;
- workflow/state model for consultation start/completion/history/corrections;
- clinical OD decisions;
- current PRD P-048–P-054;
- Doctor consultation/history/amendment screens and demographic-correction paths;
- interaction contracts for forms, history, stale state, unsaved changes, cancellation concurrency;
- G1–G5 accepted contracts and REM-021/REM-034.

### Current action

Scan G7–G15 for downstream dependencies introduced by G6, resolve REM-021/REM-034, record targeted reminders, then run final closure gates.

### Blockers

None.

---

## 2026-09-25 — G6 PREPARING checkpoint

- G6 started immediately after G5 closure.
- Mandatory reminders loaded: REM-021, REM-034.
- Next exact action: complete source review before product decisions.


## 2026-09-25 — G6 SOURCE REVIEW COMPLETE

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

1. Clinical authoring authority comes from Doctor role, not ownership.
2. Current consultation content belongs to the correct Patient ID + Visit ID.
3. Required clinical entry is chief complaint/patient problem plus assessment/diagnosis.
4. Symptoms/history, examination, notes, advice, and follow-up are optional.
5. Doctor interface is intentionally low-complexity with plain labels and limited mandatory entry.
6. Doctor assigned to the current Visit may retrieve relevant longitudinal clinical archive across that Patient ID.
7. Completed consultation is not destructively edited.
8. Correction uses Doctor-authored amendment/new revision; original stays preserved; reason, actor, and time are mandatory/auditable.
9. Current view shows latest effective clinical record while prior revisions remain accessible.
10. Doctor may approve/reject Reception demographic-correction requests and may directly correct demographics with old/new audit detail.
11. Owner/Admin/Reception/Pharmacist authority alone does not expose unrestricted diagnosis/clinical notes.
12. G5 established explicit Called -> With Doctor for consultation start.
13. Pending Visit cancellation does not freeze consultation, but effective cancellation stops future active workflow while preserving existing history.

### Mandatory reminders

- REM-021 — active in review.
- REM-034 — active in review.

### Product-definition gaps identified

- authoring versus read-only access by Visit state is not explicit enough;
- in-progress clinical draft/save behavior is underspecified;
- consultation completion needs an explicit immutable-state boundary and state transition;
- stale/concurrent Doctor saves need protection;
- cancellation approved while Doctor has the form open needs safe write blocking without deleting already-saved clinical content;
- longitudinal history must not combine Possible Duplicate Patient IDs;
- Doctor demographic-correction review needs a reachable clinical-workspace contract;
- completed consultation amendment needs explicit revision-chain semantics and stale amendment safety;
- amendment behavior for Completed/Cancelled historical Visits needs clarification;
- current patient identity/allergy/demographic context versus historical clinical content needs clearer visual separation.

### Business input required

None. All identified gaps can be resolved as derived product design without changing the locked clinical policy.


## 2026-09-25 — G6 DECISIONS RESOLVED

1. Clinical authoring begins only after explicit Called -> With Doctor and only for the currently assigned Doctor.
2. With Doctor uses an editable in-progress consultation draft. Save Draft is explicit and does not complete the Visit. Required complaint + assessment fields are enforced at completion, not every draft save.
3. Draft saves are stale-safe: changed Visit state, Doctor assignment, cancellation, or newer saved draft blocks silent overwrite.
4. Complete Consultation is explicit, Doctor-only, revalidates state/assignment, requires the two mandatory fields, creates the completed effective clinical record, records Doctor/time, moves Visit to Consultation Completed, and makes ordinary clinical fields read-only.
5. Consultation completion does not itself finalize prescription or perform pharmacy progression; G7 owns that boundary.
6. Longitudinal history remains scoped to the actual Patient ID. Possible Duplicate context may be shown, but candidate Patient histories are never auto-combined.
7. Doctor workspace exposes demographic-correction tasks. Approval/rejection revalidates current value and reviewer authority; stale requests cannot overwrite newer demographics. Doctor direct demographic correction remains a separate audited action. Resolves REM-021.
8. Completed consultation correction uses a new effective amendment revision with mandatory reason. Prior revisions remain read-only and the latest effective revision is default. Stale amendment baseline blocks overwrite.
9. Amendment corrects an already-completed clinical record and may remain available after the Visit later becomes Completed or Cancelled/Voided; it does not reopen workflow.
10. If cancellation occurs while With Doctor before completion, saved partial content remains historical/read-only but is not relabelled as a completed consultation and does not get the completed-consultation amendment flow.
11. Pending cancellation does not freeze consultation. Effective cancellation blocks future saves/completion from stale Doctor views while preserving already-saved content. Resolves REM-034.
12. Owner-only/Admin-only/Reception/Pharmacist authority does not expose unrestricted clinical content; Owner+Doctor uses Doctor authority for full clinical access.
13. Exact save-version/concurrency mechanics remain technical and must be generalized in G14.

No new business input is required.


## 2026-09-25 — G6 GROUP COMMIT + COMMIT VALIDATION

### Group commit

- `762524428d1c62976519d7cd126ebe199054e2b2`
- Changed docs 05–08.

### Validation

PASS.

- P-001..P-116 remain unique.
- Screen inventory remains 49 unique IDs.
- UX scenarios remain unique and extend through UXA-067.
- PRD/companion versions align at PRD v0.8.
- With Doctor authoring gate, explicit draft save, stale-save protection, completion -> Consultation Completed/read-only, Patient-ID-scoped history, demographic-review/direct-correction behavior, amendment revision chain, post-closure amendment semantics, and cancellation-safe writes are present.

### Next exact action

Run cumulative backward compatibility against G1–G5.


## 2026-09-25 — G6 BACKWARD COMPATIBILITY COMPLETE

**PASS — no reconciliation commit required.**

- G1: Doctor clinical authority/workspace boundaries preserved.
- G2: authentication/account-state boundaries unchanged.
- G3: Possible Duplicate histories remain separate; demographic correction stale/routing/direct-edit rules are implemented and reachable.
- G4: later financial correction at With Doctor or later does not unwind clinical workflow.
- G5: Called -> With Doctor remains the authoring gate; Pending cancellation does not freeze work; effective cancellation blocks future active writes while preserving saved content.
- Historical amendment after Visit closure is not active Visit progression and therefore does not conflict with G5 cancellation semantics.

REM-021 and REM-034 are satisfied subject to reminder-register update.


## 2026-09-25 — G6 FORWARD IMPACT ANALYSIS COMPLETE

Created:
- REM-041 -> G7 prescription progression after clinical completion/cancellation.
- REM-042 -> G8 Pharmacy clinical-content boundary.
- REM-043 -> G13 consultation reporting vs amendments/cancellation.
- REM-044 -> G14 clinical draft/amendment/cancellation concurrency and audit.

Resolved:
- REM-021
- REM-034

No unique G6-specific reminder was required for G9–G12 or G15 beyond existing shared Visit/audit contracts.

### Next exact action

Run all four G6 closure gates; if PASS, close G6 and proceed directly to G7.


## 2026-09-25 — G6 FINAL CLOSURE

### Final gate results

- Gate A — Current-group validation: PASS
- Gate B — Backward compatibility with G1–G5: PASS
- Gate C — Forward impact/reminders: COMPLETE
- Gate D — Ledger/checkpoint currency: PASS

### Main Group 6 commit

`762524428d1c62976519d7cd126ebe199054e2b2`

### Forward-reminder commit

`1dd3be10d12e0ae52eac6ecf62093ce199502b68`

### Final verdict

- BRD alignment: PASS
- Product completeness: HIGH
- Implementation readiness: HIGH
- New business input required: NONE
- REM-021/034 resolved.
- REM-041–REM-044 created.
- No unresolved backward conflict remains.

### Transition

Proceed directly to G7.


# G7 — Prescription Authoring & Prescription Lifecycle

## Current checkpoint

**Stage:** COMMIT VALIDATED / BACKWARD COMPATIBILITY CHECK

### Primary requirements

- P-055 — Availability at prescribing
- P-056 — Multi-pharmacy availability
- P-057 — Unavailable prescribing
- P-058 — Quantity calculation
- P-059 — Finalize prescription
- P-060 — Immutable finalized prescription
- P-061 — Replacement prescription
- P-062 — Prior dispensing preserved
- P-063 — Reprint
- P-064 — Printed unavailable marker

### Mandatory prior-group reminders

- REM-035 — effective Visit cancellation must preserve existing prescription history but block new active prescription authoring/finalization/replacement that would continue a cancelled Visit.
- REM-041 — clinical completion does not itself finalize prescription or send to Pharmacy; define when prescription authoring/finalization remains available, what moves Visit to Sent to Pharmacy, and how cancellation interacts.

### Required source review

- locked BRD prescription authoring, medicine identity, availability, finalization, printing, replacement, dispensing-preservation rules;
- workflow/state model for prescription and Visit progression;
- OD-010–OD-012 and multi-pharmacy availability decisions;
- current PRD P-055–P-064;
- DOC-04, DOC-05, DOC-07 and related pharmacy/readiness screens;
- interaction rules for medicine search, prescription rows, stale state, history, confirmation;
- G1–G6 accepted contracts and REM-035/REM-041.

### Current action

Compare committed G7 behavior against G1–G6; reconcile any ambiguity before forward-impact analysis.

### Blockers

None.

---

## 2026-09-25 — G7 PREPARING checkpoint

- G7 started immediately after G6 closure.
- Mandatory reminders loaded: REM-035, REM-041.


## 2026-09-25 — G7 SOURCE REVIEW COMPLETE

### Locked conclusions confirmed

- medicine identity uses display name + strength + dosage form; manufacturer is stored/context; generic/molecule may be searchable;
- prescribing availability is In Stock / Out of Stock / Not Stocked and is informational, not a prescribing block;
- multi-pharmacy prescribing shows clinic total + per-unit availability read-only;
- item fields are medicine, strength, dose, frequency, duration, optional timing/food/instruction, and quantity;
- quantity is auto-calculated when deterministic, otherwise Doctor enters it;
- prescription finalization is explicit Doctor authority;
- finalized prescription is immutable in place;
- correction uses Doctor replacement; old version becomes Superseded; mandatory reason/actor/time; pharmacy defaults latest active;
- prior dispensing against superseded prescription remains;
- print ** marker uses clinic-wide Out of Stock/Not Stocked status at finalization; later partial dispensing/current stock changes do not rewrite it;
- reprint does not create a new version;
- current finalized prescription is required for V1 dispensing;
- G6 consultation completion does not itself finalize prescription or define pharmacy progression;
- G5 cancellation stops future active Visit progression but preserves prescription history.

### Product gaps identified

- prescription draft availability across With Doctor vs Consultation Completed;
- exact readiness rule for Sent to Pharmacy;
- finalization validation and availability refresh/snapshot;
- whether finalization before consultation completion changes Visit state;
- replacement availability by Visit state;
- replacement stale-state behavior;
- handling prior dispensing during replacement;
- latest-active vs Superseded pharmacy/default print behavior;
- cancellation after finalization;
- reprint behavior when a newer replacement exists or Visit is cancelled;
- no-prescription bypass is not defined in the locked source and must not be invented silently.

### Mandatory reminders

REM-035 and REM-041 are active in this review.

### Business input required

None for the locked prescription path. A no-prescription direct-completion bypass is not introduced because the locked V1 flow does not define one.


## 2026-09-25 — G7 DECISIONS RESOLVED

1. **Authoring states:** one current unfinalized prescription draft may exist for the Visit while it is active. Doctor may build/edit it during With Doctor and Consultation Completed. Sent to Pharmacy uses replacement, not ordinary draft editing. Completed/Cancelled/Voided does not allow new active authoring.
2. **Draft identity:** prescription draft is tied to Patient ID + Visit ID + Doctor context. Draft save does not finalize, print, or make it dispensable.
3. **Finalization validation:** explicit Doctor action; at least one complete medicine row; required medicine/strength/dose/frequency/duration and a resolved quantity for every row; current Visit must be active and not Cancelled/Voided/Completed.
4. **Quantity:** deterministic quantity is system-calculated from configured inputs/unit; otherwise Doctor quantity is required. Finalization cannot proceed with unresolved quantity.
5. **Availability:** while editing, availability is current informational data and never blocks prescribing. Immediately before finalization, refresh current clinic-wide/per-unit availability. Freeze clinic-wide availability-at-finalization per item for print marker/history.
6. **Finalized version:** finalization freezes Patient/Visit, Doctor/time, item instructions/quantities, and availability snapshot. Ordinary in-place editing is disabled.
7. **Pharmacy readiness:** Consultation Completed and a current Finalized prescription are independent prerequisites for **Sent to Pharmacy**. Whichever event satisfies the second prerequisite triggers the state transition. Finalizing during With Doctor does not itself move Visit; completing consultation later can satisfy readiness. Finalizing after Consultation Completed can satisfy readiness immediately.
8. **No-prescription bypass:** locked V1 does not define a direct Consultation Completed -> Completed path without prescription/pharmacy. G7 does not invent one.
9. **Replacement:** Doctor may replace the current finalized prescription while Visit remains active (With Doctor, Consultation Completed, Sent to Pharmacy). Replacement begins from current active version, requires mandatory correction reason, is stale-safe, makes old version Superseded and new version Finalized/current, and preserves links/actor/time.
10. **Prior dispensing:** replacement never reverses prior dispensing, stock, or bill history. Prior dispensed quantities are shown in replacement context. Subsequent dispensing must respect the active corrected prescription plus preserved prior dispensing; G8 owns exact remaining-dispensable behavior.
11. **Pharmacy latest version:** Pharmacy defaults to the latest current finalized prescription and receives a superseded-version warning. An old pharmacy screen cannot continue dispensing silently after replacement; G8/G14 will enforce refresh/stale behavior.
12. **Cancellation:** Pending cancellation does not freeze otherwise-valid prescription work. Effective Cancelled/Voided blocks new draft save/finalize/replacement/pharmacy handoff; existing Draft/Finalized/Superseded versions and dispensing history remain read-only history.
13. **Completed Visit:** no new active prescription finalization/replacement is introduced after Completed because that would reopen fulfillment and is not defined in locked V1.
14. **Reprint:** reprint uses the same finalized version and its finalization-time availability snapshot; current stock changes do not rewrite ** markers or create a new version. Default print/reprint follows the current finalized version rather than a Superseded version.
15. **Clinical amendment isolation:** later clinical amendment does not silently alter prescription. A prescription change, while the Visit is still active, requires the prescription replacement workflow.
16. **Concurrency:** finalization/replacement revalidates Visit state, current prescription version, and finalization-time availability. Stale action cannot overwrite a newer replacement or effective cancellation.

REM-035 and REM-041 are resolved by these rules subject to final validation.


## 2026-09-25 — G7 GROUP COMMIT + COMMIT VALIDATION

### Group commit

- `7dfa93fe469ae36b793aa0b8ca17d4931db34832`
- Changed docs 05–08.

### Validation

PASS.

- P requirements remain exactly P-001 through P-116.
- Screen inventory remains 49 unique IDs.
- UX acceptance extends uniquely through UXA-079.
- PRD/companion versions align at PRD v0.9.
- Draft-state eligibility, finalization validation, finalization-time availability snapshot, independent consultation/prescription readiness, Sent to Pharmacy transition, immutable finalized versions, replacement lineage/stale protection, prior-dispensing preservation, reprint snapshot semantics, cancellation/Completed blocking, and no-prescription-bypass guard are present.

### Next exact action

Run backward compatibility against G1–G6, with special attention to G6 completion wording and G5 cancellation semantics.


## 2026-09-25 — G7 BACKWARD COMPATIBILITY COMPLETE

### Result

PASS after a precision clarification.

### Reconciliation

`e7020544866df081bd98e469890c61ea3f95c1c7` clarifies G6 completion wording:

- Consultation Completed is always recorded as the clinical completion event;
- completion still never auto-finalizes prescription;
- G7 readiness is then evaluated;
- if a current Finalized prescription already exists, current Visit may immediately advance to Sent to Pharmacy;
- otherwise Visit remains Consultation Completed awaiting prescription finalization.

No business-policy conflict exists with G1–G6.

REM-035 and REM-041 are satisfied subject to final reminder-register update.


## 2026-09-25 — G7 FORWARD IMPACT ANALYSIS COMPLETE

Created:
- REM-045 -> G8 version-aware pharmacy retrieval/remaining dispensing.
- REM-046 -> G9 billing preservation across prescription replacement.
- REM-047 -> G10 non-retroactive stock accounting across replacement.
- REM-048 -> G13 prescription analytics without superseded double-count.
- REM-049 -> G14 prescription finalization/replacement concurrency and audit.
- REM-050 -> G15 current-vs-historical print labeling with frozen availability snapshot.

Resolved:
- REM-035
- REM-041

No unique G7-specific reminder required for G11 or G12.

### Next exact action

Run all four G7 closure gates; if PASS, close G7 and proceed directly to G8.


## 2026-09-25 — G7 FINAL CLOSURE

### Final gate results

- Gate A — Current-group validation: PASS
- Gate B — Backward compatibility: PASS after G6 wording clarification
- Gate C — Forward impact/reminders: COMPLETE
- Gate D — Ledger/checkpoint currency: PASS

### Main Group 7 commit

`7dfa93fe469ae36b793aa0b8ca17d4931db34832`

### Reconciliation commit

`e7020544866df081bd98e469890c61ea3f95c1c7`

### Final verdict

- BRD alignment: PASS
- Product completeness: HIGH
- Implementation readiness: HIGH
- New business input required: NONE
- REM-035/041 resolved.
- REM-045–REM-050 created.
- No unresolved backward conflict remains.

### Transition

Proceed directly to G8.


# G8 — Pharmacy Access, Prescription Retrieval & Dispensing

## Current checkpoint

**Stage:** COMMIT VALIDATED / BACKWARD COMPATIBILITY CHECK

### Primary requirements

- P-065 — Patient ID lookup
- P-066 — Clinical visibility
- P-067 — Dispense actual quantity
- P-068 — Automatic stock deduction
- P-069 — Partial dispensing
- P-070 — No back-order
- P-071 — Over-dispense prevention
- P-072 — Substitution request
- P-073 — No medicine return
- P-074 — Prescription-required dispensing
- P-075 — Unit-specific dispensing
- P-076 — Unit-specific billing

### Mandatory prior-group reminders

- REM-036 — effective Visit cancellation at Sent to Pharmacy stops future dispensing while preserving already-completed dispensing.
- REM-042 — Pharmacy may see current/previous prescriptions and known allergies needed for dispensing, but not unrestricted diagnosis/notes/Doctor longitudinal history.
- REM-045 — Pharmacy retrieval requires pharmacy-ready Visit/current Finalized prescription, defaults latest current version, rejects stale Superseded version, and accounts for prior dispensing across replacement lineage.

### Required source review

- locked BRD Pharmacy retrieval/access, partial fulfilment, quantity, substitution, returns, prescription-required dispensing, multi-pharmacy, inventory deduction rules;
- workflow/OD dispensing state models;
- current PRD P-065–P-076;
- PHA-01/02/03/06 and related Doctor substitution screens;
- interaction contracts for quantity, multi-unit context, stale prescription, cancellation, partial dispensing;
- G1–G7 accepted contracts and REM-036/042/045.

### Current action

Compare committed G8 behavior cumulatively against G1–G7; reconcile any conflict before forward-impact analysis.

### Blockers

None.


## 2026-09-25 — G8 SOURCE REVIEW + DECISIONS RESOLVED

No new clinic/business input is required.

1. **Pharmacy-ready gate:** active dispensing requires current Visit state = Sent to Pharmacy plus a current Finalized prescription. A Finalized prescription still attached to With Doctor is not dispensable.
2. **Patient ID lookup:** exact Patient ID is the required V1 lookup. If more than one relevant Visit/prescription is returned, show Visit ID/date/status/version and require explicit selection; never guess from name/phone.
3. **Clinical boundary:** Pharmacist may see patient identity needed for fulfilment, current/previous prescriptions, known allergies, instructions, quantities, availability, billing/payment context. No unrestricted diagnosis, consultation notes, or Doctor longitudinal clinical history.
4. **Version default:** current Finalized prescription is the only default dispensing source. Superseded/previous versions are read-only history. If the current version changes while a screen is open, dispense is stale and blocked.
5. **Prescription-item lineage:** each finalized item has a stable fulfilment lineage across replacement when the same prescribed item is carried forward. A changed/new medicine identity creates a new lineage; removed lineages remain historical and cannot receive further dispensing.
6. **Remaining allowable:** for each active item lineage, remaining allowable = max(0, current active prescribed quantity - cumulative quantity already dispensed against that lineage across all prescription versions and pharmacy units). If prior dispensing exceeds a reduced corrected quantity, remaining is 0 and the excess is shown as historical; nothing is reversed.
7. **Dispense upper bound:** entered quantity must be >0 and cannot exceed both remaining allowable and valid available stock in the active pharmacy unit.
8. **Partial fulfilment:** actual supplied quantity is recorded; only that quantity deducts stock and becomes billable. Unsupplied remainder is visible. No reservation/back-order/collect-later obligation is created.
9. **Multiple units:** every dispensing transaction has one explicit pharmacy unit. Cumulative remaining allowance is clinic-wide across units. Another unit may subsequently dispense remaining quantity if the Visit/prescription is still active, but no stock is reserved for it.
10. **Atomic dispense:** final dispense confirmation revalidates Visit active state, current prescription version, remaining allowance, active unit, non-expired valid stock, and current stock quantity. Success records dispensing and stock deduction together; unknown result is checked before retry.
11. **Expired stock:** never selectable/dispensable. Owner cannot override the expired-dispense block through inventory approval.
12. **Replacement race:** if Doctor replaces the prescription while Pharmacy is preparing dispense, the old screen cannot dispense. Refresh to latest current Finalized prescription and recompute remaining allowances.
13. **Cancellation race:** if Visit becomes Cancelled/Voided before dispense commits, dispense is blocked. Already-committed dispensing and stock deduction remain. Resolves REM-036.
14. **Substitution request:** request identifies current prescription version/item lineage, original item, proposed substitute, requested substitute quantity, and reason. Pending substitute is non-dispensable.
15. **Substitution approval safety:** Doctor approval authorizes only the exact proposal. Pharmacist cannot infer a different strength/form/quantity. If units are not safely comparable, Doctor must explicitly confirm the substitute quantity; no automatic clinical conversion is invented.
16. **Substitute fulfilment:** approved substitute dispensing is recorded with the approval reference and consumes the approved amount against the original item fulfilment allowance so original + substitute cannot silently exceed the permitted remaining fulfilment.
17. **Substitution stale state:** request becomes non-actionable if prescription version/item is superseded, Visit is cancelled, or requested remaining quantity is no longer available because dispensing occurred first.
18. **Unit context:** active pharmacy unit stays visible throughout dispensing. Changing unit with entered unsaved quantities requires explicit discard/review; no quantities silently move to another unit.
19. **No returns:** no medicine-return/restock workflow is introduced.
20. **Visit state:** dispensing itself does not mark Visit Completed; G9 owns billing/payment and final pharmacy completion.
21. **G7 replacement lineage:** prior dispensing, stock, and bills remain attached to their original version/item/unit while the current active prescription controls future allowance. Resolves REM-045.
22. **Clinical visibility:** G6 boundary is preserved exactly. Resolves REM-042.

Technical implementation may choose row/version identifiers, optimistic locking, or transactions, but must preserve these product outcomes.


## 2026-09-25 — G8 GROUP COMMIT + COMMIT VALIDATION

### Group commit

- `f9cff596b9124cb2a5659b43882d5f19209b8b2a`
- Changed docs 05–08.

### Validation

PASS.

- P requirements remain P-001 through P-116.
- Screen inventory remains 49 unique IDs.
- UX acceptance extends uniquely through UXA-091.
- PRD/companion versions align at PRD v0.10.
- Sent-to-Pharmacy gating, latest-current prescription enforcement, narrow clinical visibility, item-lineage remaining-quantity formula, reduced-corrected-quantity handling, atomic dispensing/stock deduction, partial no-reservation behavior, substitution quantity safety, multi-unit stale prevention, cancellation blocking, and non-completion-on-dispense are present.

### Next exact action

Run cumulative backward compatibility against G1–G7.


## 2026-09-25 — G8 BACKWARD COMPATIBILITY COMPLETE

**PASS — no reconciliation commit required.**

- G5 effective cancellation remains non-destructive but blocks future dispense.
- G6 clinical-content boundary is preserved.
- G7 latest-current prescription/version lineage is implemented rather than redefined.
- Prior dispensing remains immutable and constrains future allowance.
- Multi-role/auth/patient/financial contracts remain compatible.

## 2026-09-25 — G8 FORWARD IMPACT ANALYSIS COMPLETE

Created:
- REM-051 -> G9 final pharmacy/Visit completion.
- REM-052 -> G10 atomic unit/batch stock behavior.
- REM-053 -> G13 actual-supplied medicine-sales reporting.
- REM-054 -> G14 dispensing/version/substitution concurrency.
- REM-055 -> G15 supplied/unsupplied/substitute pharmacy output.

Resolved:
- REM-036
- REM-042
- REM-045

### Next exact action

Run all four G8 closure gates; if PASS, close G8 and proceed directly to G9.


## 2026-09-25 — G8 FINAL CLOSURE

### Final gate results

- Gate A — Current-group validation: PASS
- Gate B — Backward compatibility with G1–G7: PASS
- Gate C — Forward impact/reminders: COMPLETE
- Gate D — Ledger/checkpoint currency: PASS

### Main Group 8 commit

`f9cff596b9124cb2a5659b43882d5f19209b8b2a`

### Final verdict

- BRD alignment: PASS
- Product completeness: HIGH
- Implementation readiness: HIGH
- New business input required: NONE
- REM-036/042/045 resolved.
- REM-051–REM-055 created.
- No unresolved backward conflict remains.

### Transition

Proceed directly to G9.


# G9 — Pharmacy Billing, Payment & Bill Cancellation

## Current checkpoint

**Stage:** DECISIONS RESOLVED / READY TO EDIT

### Primary requirements

- P-077 — Bill only supplied items
- P-078 — External payment recording
- P-079 — No partial pharmacy payment
- P-080 — No pharmacy refund
- P-081 — Pharmacist void request
- P-082 — Pending bill remains active
- P-083 — Owner decision
- P-084 — No automatic stock restoration

### Mandatory prior-group reminders

- REM-028 — reuse explicit external payment model, methods, optional reference, correction workflow, no partial/refund, unknown-outcome safety.
- REM-037 — Visit cancellation must not refund/erase existing pharmacy bill/payment history; cancelled Visit cannot continue new active billing.
- REM-046 — prescription replacement after prior dispensing must preserve earlier pharmacy bill/payment history; only newly supplied quantities become new billable supply.
- REM-051 — define final pharmacy/Visit completion after partial/multi-unit fulfilment and payment, without treating one dispense as completion.

### Required source review

- locked BRD pharmacy billing/payment, bill void, payment correction, no-refund, multi-pharmacy billing rules;
- workflow/OD billing state models;
- current PRD P-077–P-084;
- PHA-04/PHA-05, Owner approval details, payment correction screens;
- G1–G8 accepted contracts and REM-028/037/046/051.

### Current action

Apply resolved G9 bill creation, payment, Visit-completion, payment-correction, and bill-void rules to PRD layers.

### Blockers

None.


## 2026-09-25 — G9 SOURCE REVIEW + DECISIONS RESOLVED

No new clinic/business input is required.

1. **Bill source:** Pharmacy bill lines come only from committed actual dispensing records, never from prescribed-but-unsupplied quantity.
2. **Unit-specific bill:** every bill belongs to the pharmacy unit whose dispensing records it bills. Multi-unit fulfilment may therefore create multiple bills for one Visit.
3. **No double billing:** a committed dispensing quantity can be included in only one active/non-voided bill lineage at a time. Prescription replacement does not rebill earlier dispensing. Resolves REM-046.
4. **Bill creation:** Pharmacist explicitly creates a bill from supplied-but-not-yet-billed dispensing records for the current Visit/unit. Bill starts Unpaid.
5. **Bill snapshot:** on creation freeze bill lines, supplied quantities, unit-price/price basis, configured tax/amount fields where applicable, total, unit, Visit, and source dispensing references. Later price/config changes or prescription replacement do not silently rewrite an existing bill.
6. **Bill ordinary immutability:** established bill lines/total are not edited in place. V1 bill correction is the Owner-controlled void path; payment correction changes payment record, not bill lines/total.
7. **External payment:** use UPI/Cash/Card/Other; Other description required; reference optional; selecting method never marks Paid; explicit final Mark Paid after external success.
8. **No partial/no refund:** Paid means the full current bill amount was externally paid; Unpaid is valid; no partial-payment or refund UI.
9. **Payment correction:** Pharmacy may request correction of Paid/Unpaid, method, Other description, reference, or other payment-record metadata; mandatory reason + captured baseline; Owner approves/rejects; bill amount/lines are not changed. Paid->Unpaid correction is not a refund. Resolves REM-028.
10. **Bill void request:** Pharmacist requests with mandatory specific reason; one actionable Pending request per bill; pending leaves bill/payment active.
11. **Bill void decision:** Owner revalidates bill/request, sees latest payment state, and approves/rejects. Approval -> bill Cancelled/Voided and leaves active billing; rejection -> active bill unchanged. If Paid, approval does not refund. Original bill/payment/request/decision history remains.
12. **Void and stock:** bill void never reverses dispensing or restores stock. A legitimate stock correction is separate G10 Owner-approved inventory adjustment.
13. **Visit cancellation:** effective Visit cancellation blocks new dispensing and new bill creation, but never auto-voids/refunds existing bill. Existing bill/payment history remains. Existing bill may still receive record-level payment capture/correction/void administration without reopening Visit or dispensing. Resolves REM-037.
14. **Visit Completed condition:** Visit may transition Sent to Pharmacy -> Completed when Pharmacist explicitly finishes clinic-pharmacy fulfilment and:
    - no committed dispensing record remains unbilled;
    - all current intended clinic dispensing work is finished;
    - any remaining prescription quantity is explicitly left unsupplied/outside with no reservation/back-order;
    - any same-Visit multi-unit committed dispensing is accounted for in its unit bill(s).
15. **Payment does not gate completion:** existing bill payment state may be Paid or Unpaid. There is no locked pharmacy-payment gate equivalent to consultation queue payment. Financial status remains visible/reportable after Visit completion.
16. **Multiple units:** one unit's bill/payment does not automatically Complete the Visit if other committed/unbilled unit dispensing still exists. Final completion is Visit-level and considers all committed pharmacy units.
17. **Pending substitution:** Visit cannot be finalized as pharmacy-fulfilment-complete while an actionable substitution request still represents intended clinic fulfilment. Staff must resolve/abandon that fulfilment path; no hidden back-order.
18. **After Completed:** no new dispensing or new bill creation for that Visit. Existing bills can still be marked Paid after an external payment, corrected through Owner-controlled payment correction, or voided through Owner control; these do not reopen Visit.
19. **Void/payment concurrency:** payment may change while void request is Pending because bill remains active. Owner decision must show current payment state; a newly Paid bill can still be voided with explicit no-refund warning.
20. **Unknown outcome safety:** bill creation, Paid recording, Visit completion, void request/decision, and payment correction use refresh/check before blind retry.
21. **No automatic re-billing after void:** voided bill remains historical; V1 does not silently regenerate/rebill its dispensing records.
22. **Visit completion and unsupplied remainder:** completion never means every prescribed unit was supplied; unsupplied remainder is a legitimate final clinic-pharmacy outcome because V1 has no back-order. Resolves REM-051.

Technical transaction/idempotency mechanisms remain downstream, but these effective outcomes are mandatory.

## 2026-09-25 — G9 GROUP COMMIT + COMMIT VALIDATION

### Main group commit

- `d317a8d50cfa5baeb7506920ffa75c4e19776f00`
- Changed Documents 05–08.
- PRD advanced to v0.11.

### Validation-fix commit

- `4c4c6622b940e1db6d5efca2e872488a01d580fe`
- Corrected section-boundary duplication introduced by the main edit in Documents 05, 07 and 08.
- No product rule changed in this fix.

### Validation result

**PASS after validation-fix commit.**

- P requirements remain exactly P-001 through P-116 with no duplicate IDs.
- Acceptance scenarios remain unique and extend through AC-038.
- UX acceptance scenarios remain unique and extend through UXA-100.
- Existing screen IDs remain unique; G9 reused existing PHA-04/PHA-05 and shared Owner/payment-correction surfaces rather than inventing a new screen.
- PRD/acceptance/interaction versions align at PRD v0.11; Document 07 is a v0.10 companion whose Parent points to PRD v0.11.
- Unit-specific supplied-only billing, duplicate-billing prevention, bill snapshot immutability, external-payment confirmation, baseline-safe pharmacy payment correction, Visit-level completion, multi-unit completion, post-completion bill administration, void/payment concurrency, no-stock-restoration, and retry safety are now represented across Documents 05–08.
- The four malformed replacement-boundary headings found during initial commit inspection were repaired and revalidated.

### Next exact action

Run cumulative backward compatibility against G1–G8, reconciling any conflict before forward-impact analysis.

## 2026-09-25 — G9 BACKWARD COMPATIBILITY COMPLETE

**PASS — no reconciliation commit required.**

- **G1:** role/workspace authority remains intact. Pharmacist performs bill/payment/request actions; Owner performs approvals; same-human multi-role actions remain separately attributable.
- **G2:** G9 introduces no authentication/account-state exception. Owner decisions remain behind the existing Owner authentication boundary.
- **G3:** Patient identity and Possible Duplicate behavior are unchanged; billing references the existing Visit/Patient and never creates or merges identity.
- **G4:** pharmacy payment uses the same explicit external-payment and baseline-safe correction principles without importing consultation waiver. Paid -> Unpaid remains correction, not refund.
- **G5:** effective Visit cancellation blocks new active dispensing/bill creation while preserving existing financial history. Existing-bill administration after cancellation does not reopen Visit workflow and therefore does not conflict with cancellation semantics.
- **G6:** consultation completion/amendment behavior is unaffected. Pharmacy financial administration does not reopen clinical authoring.
- **G7:** prescription replacement never erases or re-bills prior dispensing/billing history. G9 bills committed supply and preserves the original prescription/version lineage.
- **G8:** dispensing remains the stock-changing event; G9 does not change or reverse it. G8 intentionally delegated final Visit completion to G9, and G9 now defines that Visit-level completion without treating one dispense or one bill/payment as completion.

### Mandatory reminder dispositions

- REM-028 — SATISFIED by explicit external payment/correction/no-partial/no-refund rules.
- REM-037 — SATISFIED by cancellation preserving existing bill/payment history while blocking new billing/dispensing.
- REM-046 — SATISFIED by source-dispensing bill lineage and no rebilling after prescription replacement.
- REM-051 — SATISFIED by Visit-level completion across all units, explicit unsupplied remainder, and payment-independent completion.

### Next exact action

Scan G10–G15 for downstream dependencies introduced by G9 and write targeted reminders before final closure.

## 2026-09-25 — G9 FORWARD IMPACT ANALYSIS COMPLETE

### Resolved inherited reminders

- REM-028 — shared pharmacy payment/correction semantics.
- REM-037 — Visit cancellation versus existing pharmacy financial history.
- REM-046 — prescription replacement versus prior billing.
- REM-051 — final Visit completion after multi-unit/partial fulfilment.

### Targeted reminders created

- REM-056 -> G10: dispensing remains the stock-changing event; billing/void/payment/completion do not restore or mutate stock.
- REM-057 -> G11: bill-void approval must use latest bill/request/payment state and explicit no-refund consequences.
- REM-058 -> G11: pharmacy payment correction remains baseline-aware, stale-safe, and bill-total immutable.
- REM-059 -> G12: billing configuration/price changes are prospective and cannot rewrite frozen bill snapshots.
- REM-060 -> G13: reporting must distinguish effective payment state, historical Paid+Voided records, multi-unit bills, and active charge status without double-counting.
- REM-061 -> G14: global stale/idempotency review must cover bill creation/payment/correction/void/completion concurrency and unknown outcomes.
- REM-062 -> G15: A4 pharmacy output must use frozen bill snapshot and clearly distinguish active versus historical Cancelled/Voided copies.

### Reminder-register commit

`63811be1c6a5f6a6a7493ba2f1ca3e862c2e1460`

Open reminder count after G9: **42**.

## 2026-09-25 — G9 FINAL CLOSURE

### Final gate results

- **Gate A — Current-group validation:** PASS
- **Gate B — Backward compatibility with G1–G8:** PASS
- **Gate C — Forward impact/reminders:** COMPLETE
- **Gate D — Ledger/checkpoint state:** CURRENT

### Main Group 9 commit

`d317a8d50cfa5baeb7506920ffa75c4e19776f00`

### Validation-fix commit

`4c4c6622b940e1db6d5efca2e872488a01d580fe`

The validation-fix corrected only section-boundary duplication introduced by the main documentation edit; it did not change product behavior.

### Final verdict

- Locked BRD alignment: PASS
- Product completeness: HIGH
- Implementation readiness at PRD level: HIGH
- New clinic/business input required: NONE
- REM-028/037/046/051 resolved.
- REM-056–REM-062 created.
- No unresolved backward conflict remains.
- Pharmacy payment remains external; no partial payment or refund was introduced.
- Visit completion is pharmacy-fulfilment based rather than payment-gated.

### Transition

Proceed directly to G10 under the user's continuous-review instruction.

# G10 — Inventory, Stock Accountability & Pharmacy Transfers

## Current checkpoint

**Stage:** PREPARING

### Primary requirements

- P-085 — Pharmacy-unit stock
- P-086 — Consolidated Owner view
- P-087 — Automatic movement visibility
- P-088 — Inventory adjustment request
- P-089 — No change while adjustment is Pending
- P-090 — Owner inventory-adjustment decision
- P-091 — Expired stock
- P-092 — Pharmacy stock-transfer request
- P-093 — Owner transfer decision

### Mandatory prior-group reminders

- REM-047 — prescription replacement/finalization must never retroactively alter stock; actual dispensing remains the stock movement and preserves original version attribution.
- REM-052 — preserve atomic dispense -> unit stock deduction, valid/non-expired stock only, no reservation for unsupplied remainder, and safe concurrent unit stock behavior.
- REM-056 — billing/payment/Visit completion/bill void must not become stock-changing events; legitimate correction remains separate Owner-controlled inventory adjustment.

### Required source review

- locked BRD inventory model, units/package conversion, batch/expiry/pricing metadata, low-stock/expiry rules, manual adjustment approval, inventory-theft controls, and multi-pharmacy transfer behavior;
- workflow/state and OD inventory decisions;
- current PRD P-085–P-093;
- Pharmacy inventory screens, Owner inventory control/approval screens, and interaction contracts;
- G1–G9 accepted contracts and REM-047/052/056.

### Current action

Perform the full G10 source review before autonomous product reasoning.

### Blockers

None.

## 2026-09-25 — G10 SOURCE REVIEW + DECISIONS RESOLVED

No new clinic/business input is required. These decisions preserve the locked anti-theft/accountability model and the already-accepted G7–G9 stock boundaries.

1. **Authoritative unit ledger:** every pharmacy unit has its own attributable inventory ledger. Clinic-total stock is derived from unit ledgers and never replaces unit-level truth.
2. **Movement model:** stock quantity is changed by ledger movements/events, not by silently editing a current-stock number. Normal dispensing creates its automatic negative movement; approved addition/loss/damage/expiry/correction and approved transfer create explicit movements. Historical movements are not rewritten when later corrections occur.
3. **Dispensing remains authoritative physical movement:** prescription replacement, bill creation/payment/correction/void, Visit completion/cancellation, or reporting never retroactively changes stock. Resolves REM-047 and REM-056.
4. **Base-unit normalization:** every medicine has a configured base stock/dispensing unit. Staff may enter/display a configured package quantity, but every quantity-changing request shows the conversion and stores the normalized base-unit delta/result. Historical movement retains the conversion/result used at the time rather than silently changing after future configuration edits.
5. **Batch attribution:** where stock is held by batch/lot, quantity-changing movements identify the concrete batch/lot and preserve its expiry/manufacturer context. A dispense must resolve actual valid/non-expired batch quantity before commit; hidden use of expired or insufficient batch stock is not allowed. The exact UI default/order for choosing among multiple valid batches is implementation design, but the committed batch attribution is mandatory.
6. **Available versus physically recorded expired stock:** reaching expiry makes the batch immediately unavailable for dispensing/available-stock calculations. Expiry alone does not silently erase physical quantity from the ledger. Removal/disposition quantity is recorded only through the controlled Owner-authorized adjustment, preserving the expired quantity and subsequent disposition history.
7. **Low-stock/near-expiry evaluation:** use configured medicine/inventory thresholds against current valid stock/batch expiry context. Thresholds are configuration, not hard-coded product policy. Owner consolidated views preserve the underlying unit/batch drill-down.
8. **Pharmacist adjustment request:** stock addition, damage, loss, expired disposition, and quantity correction require explicit category, pharmacy unit, medicine, affected batch where relevant, entered quantity/unit, normalized base-unit effect, mandatory specific reason, and captured stock baseline sufficient to show projected result.
9. **Correction semantics:** a stock-count correction may express the intended counted/resulting quantity; the product derives and displays the delta against the captured baseline so the Owner never approves an unexplained replacement number.
10. **No stock mutation while Pending:** a Pharmacist request changes nothing and reserves nothing while Pending. This keeps the live ledger truthful and avoids hidden unavailable stock.
11. **Stale adjustment protection:** Owner decision revalidates the relevant current unit/batch stock and request baseline. If intervening dispensing/adjustment/transfer means the proposed result is no longer the reviewed result or would go negative, the old request cannot silently apply; refreshed review/new proposal is required. Do not partially apply a stale request.
12. **Owner direct inventory action:** Owner may perform the same non-dispensing inventory adjustment directly without creating a self-approval request, because the locked rule requires Owner authorization rather than Owner self-approval. Direct adjustment still requires category/reason, current-state review, explicit confirmation, and full audit attribution.
13. **Price-change request is non-quantity:** Pharmacist-proposed purchase/selling-price change uses the same Owner-control/request pattern but does not create a quantity movement. Capture current value, proposed value, reason, requester, Owner decision, and effective time. Historical dispensing/bills remain unchanged; G12 owns prospective configuration behavior.
14. **Transfer request:** transfer specifies source unit, different destination unit, medicine, concrete batch where relevant, entered quantity/unit, normalized base quantity, mandatory reason, and captured source availability. Submitting the request does not change or reserve either unit.
15. **Transfer approval:** Owner decision revalidates source transferable valid quantity and request state. Approval is one atomic business outcome with a shared transfer ID/reference: source decreases and destination increases by the same normalized quantity; rejection/stale failure changes neither side. No partial transfer approval is introduced.
16. **Transferred batch continuity:** the destination movement preserves the physical stock's batch/lot/expiry/manufacturer identity and transfer linkage rather than inventing a new unrelated batch. Transfer does not rewrite prior source history.
17. **Transfer concurrency:** if source stock falls below requested transferable quantity before approval, the request is stale/non-actionable until refreshed/replaced; destination does not receive stock and source is not partially deducted.
18. **Owner visibility:** Owner inventory control shows current available stock and recorded stock context by unit, consolidated totals, batch/expiry risk, automatic dispensing movements, additions, adjustments, damage/loss, expiry disposition, transfers, requester/approver/reason/time, and before/delta/after values where quantity changes.
19. **Admin boundary:** authorized Admin may configure medicine/inventory metadata and thresholds where permitted, but Admin authority alone cannot approve/apply operational stock loss/addition/correction/transfer. Owner authority remains the operational control point.
20. **Retry/unknown-outcome safety:** adjustment submission/direct adjustment/approval and transfer submission/approval are state-changing operations. Duplicate submit is prevented while pending; unknown outcome is checked against the ledger/request before blind retry. Exact transaction/locking implementation remains technical and will be generalized in G14.
21. **G8 stock safety preserved:** dispensing continues to revalidate current valid/non-expired batch stock and succeeds with stock deduction as one effective operation; inventory work here does not create a reservation for unsupplied prescription remainder. Resolves REM-052.

### Current action

Apply these decisions across PRD requirements, acceptance/traceability, inventory/Owner screens, and interaction contracts; then commit and validate G10 as one logical product refinement.

## 2026-09-25 — G10 GROUP COMMIT + COMMIT VALIDATION

### Main group commit

- `4a5356d85a9c88b80b4dac1485e5cf445e34b1d7`
- Changed Documents 05–08.
- PRD advanced to v0.12.

### Validation-fix commit

- `e15ca69e32e3f2ba95a4017d687e7232d7ed4083`
- Clarified one ambiguous P-089 sentence so Pending inventory requests explicitly do not reserve/own quantity.
- No product behavior changed.

### Validation result

**PASS after validation-fix commit.**

- P requirements remain P-001 through P-116 with no duplicate IDs.
- Acceptance scenarios extend through AC-048 with no duplicate IDs.
- UX acceptance scenarios extend through UXA-110 with no duplicate IDs.
- Screen contracts remain 49 with no duplicate screen IDs; G10 refined PHA-07–PHA-10 and OWN-03/OWN-04 rather than inventing new screens.
- PRD/acceptance/interaction align at v0.12; Document 07 is v0.11 with Parent PRD v0.12.
- Unit-ledger movement truth, package/base normalization, batch attribution, stale adjustment safety, Owner direct adjustment, expired-stock visibility, linked transfer atomicity, Admin boundary, and retry safety are represented across Documents 05–08.

### Next exact action

Run cumulative backward compatibility against G1–G9; reconcile any conflict before G10 forward-impact analysis.

## 2026-09-25 — G10 BACKWARD COMPATIBILITY COMPLETE

**PASS — no reconciliation commit required.**

- **G1:** composable-role/authority model remains explicit. Pharmacist requests operational inventory changes, Owner authorizes/applies Owner-level controls, Admin configuration does not become Owner operational authority, and multi-role attribution remains visible.
- **G2:** no authentication/account-state behavior changes; Owner inventory actions remain behind existing Owner authentication requirements.
- **G3:** patient identity/demographic workflows are untouched.
- **G4:** consultation-payment/fee behavior is untouched; G10 price-change control does not rewrite historical Visit financial records.
- **G5:** queue/cancellation behavior is unchanged. Visit cancellation preserves already-committed dispensing/stock history.
- **G6:** clinical records/authority remain isolated from inventory operations.
- **G7:** prescription replacement/finalization never retroactively changes stock; prior dispense movement remains attributed to the original version. REM-047 is satisfied.
- **G8:** atomic dispense -> active-unit stock deduction remains the normal stock-changing path, expired stock remains non-dispensable, unsupplied remainder creates no reservation, and batch/current-stock revalidation strengthens rather than changes G8. REM-052 is satisfied.
- **G9:** billing/payment/correction/void/Visit completion remain financially/operationally separate from physical stock movement. Bill void still never restores stock; a legitimate correction uses this G10 adjustment flow. REM-056 is satisfied.

### Owner direct adjustment compatibility

Allowing Owner direct adjustment does not bypass the locked anti-theft rule. The locked requirement is that **pharmacy-initiated** non-dispensing changes require Owner authorization. A direct Owner-authority action therefore does not need a redundant Owner self-approval request, but still requires category, reason, current-state review, explicit confirmation, movement history, and audit attribution. This is consistent with the earlier direct-Owner-waiver principle and with OWN-04's pre-existing Owner-authorized-action boundary.

### Mandatory reminder dispositions

- REM-047 — SATISFIED.
- REM-052 — SATISFIED.
- REM-056 — SATISFIED.

### Next exact action

Evaluate G10 impact on G11–G15 and create only targeted reminders for real downstream dependencies.

## 2026-09-25 — G10 FORWARD IMPACT ANALYSIS COMPLETE

### Resolved inherited reminders

- REM-047 — prescription replacement/finalization never retroactively alters stock.
- REM-052 — atomic dispense/valid stock/no-reservation/concurrent stock safety.
- REM-056 — billing/payment/Visit completion/bill void remain non-stock-changing.

### Targeted reminders created

- REM-063 -> G11: Owner adjustment/transfer decisions need captured/current stock, stale blocking, and direct-Owner-action distinction.
- REM-064 -> G12: package/threshold/price/config changes are prospective and must preserve historical base-unit movement truth; reconcile Admin configuration with Owner-controlled operational price requests.
- REM-065 -> G13: inventory reporting must derive from unit movement ledgers, distinguish valid vs expired recorded quantity, and classify transfers/corrections correctly.
- REM-066 -> G14: global safety/audit must cover concurrent dispense/adjustment/transfer, atomic two-sided transfer, no-negative stock, immutable movement history, and retry/idempotency.

### Reminder-register commit

`7f2976d2695810a71a92fd327bc59002be04e11d`

Open reminder count after G10: **43**.

## 2026-09-25 — G10 FINAL CLOSURE

### Final gate results

- **Gate A — Current-group validation:** PASS
- **Gate B — Backward compatibility with G1–G9:** PASS
- **Gate C — Forward impact/reminders:** COMPLETE
- **Gate D — Ledger/checkpoint state:** CURRENT

### Main Group 10 commit

`4a5356d85a9c88b80b4dac1485e5cf445e34b1d7`

### Validation-fix commit

`e15ca69e32e3f2ba95a4017d687e7232d7ed4083`

### Final verdict

- Locked BRD alignment: PASS
- Product completeness: HIGH
- Implementation readiness at PRD level: HIGH
- New clinic/business input required: NONE
- REM-047/052/056 resolved.
- REM-063–REM-066 created.
- No unresolved backward conflict remains.
- Inventory remains movement-ledger based; normal dispensing is automatic, non-dispensing operational changes remain Owner-controlled, and stock transfer is one linked two-sided event.

### Transition

Proceed directly to G11 under the user's continuous-review instruction.

# G11 — Owner Approval Center & Exception Control

## Current checkpoint

**Stage:** PREPARING

### Primary requirements

- P-094 — Owner approval queue
- P-095 — Owner decision
- P-096 — Multi-role audit attribution

### Mandatory prior-group reminders

- REM-005 — same human requester/approver roles must remain separately attributed by effective authority.
- REM-010 — staff password reset is Pending -> Resolved by Set Temporary Credential, not generic Approve/Reject.
- REM-011 — password-reset action must reject duplicate/stale action.
- REM-029 — consultation-waiver approval: one Pending, Paid-before-decision stale, rejection keeps Unpaid, Direct Waiver is an Owner action not self-approval.
- REM-030 — payment correction: captured baseline, stale blocking, original/proposed/effective history, correction not refund.
- REM-038 — Visit cancellation: one Pending, current-state revalidation, Completed-before-decision stale, Pending does not freeze workflow, same-human Doctor/Owner attribution.
- REM-057 — bill void: latest bill/request/payment state, bill remains active while Pending, Paid-before-decision no-refund warning, post-Visit administration.
- REM-058 — pharmacy payment correction: baseline-aware/stale-safe, bill lines/total immutable, one actionable correction per current payment baseline.
- REM-063 — inventory adjustment/transfer decisions use captured/current stock; stale baseline blocks application; direct Owner inventory adjustment is not a fake self-approval request.

### Required source review

- locked BRD Owner authority, approval, waiver, cancellation, payment correction, inventory control, bill void, password reset, and audit rules;
- current P-094–P-096;
- OWN-02 Approval Center, OWN-03 Approval Detail, OWN-09 Staff Password Reset and relevant source screens;
- shared Reason-and-Approval / Owner Decision patterns;
- G1–G10 accepted lifecycle contracts and all reminders listed above.

### Current action

Perform full G11 source review before autonomous product reasoning.

### Blockers

None.

## 2026-09-25 — G11 SOURCE REVIEW + DECISIONS RESOLVED

No new clinic/business input is required. G11 unifies Owner work discovery/presentation while preserving the distinct lifecycle already accepted for each exception type.

1. **One inbox, type-specific lifecycle:** OWN-02 is a single Owner work queue for actionable Owner-controlled requests, but the product must not force every item into identical Approve/Reject semantics.
2. **Pending-first work queue:** default view prioritizes currently actionable Pending work. History remains filterable by type, requester, time, and final outcome. No unconfirmed urgency/risk ranking is invented.
3. **Common request identity:** every queued request preserves request type, request ID, requester human identity, requester effective authority/workspace, affected target, mandatory source reason where required, created time, current request state, and type-specific baseline.
4. **Common stale rule:** loading a request never freezes underlying business state. At final Owner action the product revalidates request state, target state, authority, and the type-specific baseline/current facts. A stale/resolved request cannot apply twice.
5. **Type-specific action controls:** OWN-03 renders only the valid action for that request type. Standard approval types use Approve/Reject; staff password reset uses **Set Temporary Credential**; direct Owner actions such as Direct Consultation Waiver and Direct Inventory Adjustment are not turned into self-approval requests.
6. **Same-human multi-role behavior:** a user who legitimately holds requester role + Owner may perform separate sides of a workflow if the locked rules allow it. The product does not invent a separation-of-duties prohibition. The two events remain separately attributed to the same human under the distinct effective authorities/workspaces. Resolves REM-005.
7. **Clinical-content boundary:** Owner Approval Center shows operational state/risk context required for decision, but Owner-only authority does not reveal unrestricted diagnosis/clinical notes. If the same account also has Doctor authority, clinical access occurs under Doctor context rather than being silently leaked into Owner approval UI.
8. **Consultation waiver request:** only one actionable Pending waiver request for an eligible Unpaid Visit. Owner revalidates current financial outcome before decision. Paid/Waived makes old request non-actionable/stale. Approval -> Waived; rejection leaves Unpaid and historical request, and a later new request may be submitted. Resolves REM-029.
9. **Direct Consultation Waiver:** Owner may apply waiver directly to an eligible current Unpaid Visit with mandatory reason and explicit confirmation. It is an immediate Owner action, not a Pending request requiring self-approval.
10. **Visit cancellation:** one actionable Pending cancellation request per Visit. Pending does not freeze workflow. Owner decision revalidates current Visit state; Completed-before-decision makes cancellation request non-actionable/stale. Approval -> Cancelled/Voided with prior history preserved/no refund; rejection leaves current active state unchanged. Same-human Doctor request/Owner decision remains separately attributed. Resolves REM-038.
11. **Bill void:** one actionable Pending request per bill. Pending leaves bill/payment active. Payment may change while Pending, so current payment state is revalidated at decision time rather than treating that change as automatic staleness. Approval -> Cancelled/Voided; Paid approval requires explicit no-refund warning and never restores stock. Existing bill administration remains possible after Visit Completed/Cancelled where G9 permits. Resolves REM-057.
12. **Payment correction:** consultation/pharmacy payment correction preserves captured baseline, proposed record, current effective record, and original/effective history. If the captured baseline no longer matches current effective payment record, the request is stale/non-applicable; it cannot overwrite newer truth. Approval applies proposed payment record; rejection preserves current effective state. Paid->Unpaid is correction, not refund. Pharmacy correction cannot change bill lines/total. Resolves REM-030 and REM-058.
13. **Inventory adjustment:** Owner sees captured stock/value baseline alongside current unit/medicine/batch state and proposed effect. Stale/invalid/negative application is blocked. Quantity approval creates one controlled movement; rejection changes nothing. Price-only approval changes the controlled value, not stock quantity. Direct Owner adjustment stays outside the Pending approval queue. Resolves REM-063 adjustment portion.
14. **Stock transfer:** Owner sees source/destination, medicine/batch, requested normalized quantity, captured source availability, and current source availability. If current source cannot satisfy full request, approval is blocked/stale; no partial approval. If source changed but still valid, show the current value and apply only after explicit current-state confirmation. Approval creates linked source/destination movements under one transfer reference; rejection changes neither. Resolves REM-063 transfer portion.
15. **Staff password reset:** this request type is Pending -> Resolved by **Set Temporary Credential**, not Approved/Rejected. One actionable Pending reset per eligible non-Owner staff account. Owner never sees old password. Successful set resolves request; stale/resolved copy cannot act again. Reset does not enable disabled account or change roles. If target is no longer eligible for the non-Owner reset flow (for example now holds Owner authority), the old request becomes non-actionable rather than bypassing Owner recovery policy. Resolves REM-010 and REM-011.
16. **Approval list outcomes:** history supports at least Pending, Approved, Rejected, Resolved, and Stale/Non-actionable presentation as applicable to request type. Do not label password reset “Approved” when its effective outcome is Resolved by credential set.
17. **Action-specific confirmations:** high-impact Owner actions require an explicit final action with consequence text. Avoid generic row-click approval and avoid extra double-confirmation unless the consequence materially benefits from it.
18. **Decision outcome visibility:** after successful Owner action, show the resulting effective state and keep request history read-only. Rejection explicitly says the underlying business record remains unchanged where applicable.
19. **Unknown outcome/retry:** Owner decision/reset action and direct Owner exception actions are protected against duplicate final submit. If action result is unknown, refresh request/target/effective state before retry. Exact transaction/idempotency mechanism remains technical for G14.
20. **Cross-workspace request visibility:** notification/badge may identify that Owner work exists, but protected details and decision controls belong to the Owner workspace. Workspace switching follows G1/G2 authority/authentication rules.
21. **No generic self-approval artifacts:** Direct Waiver and Direct Inventory Adjustment are recorded/audited as direct Owner actions. They do not create artificial requester=Owner + approver=Owner Pending records solely to fit OWN-02.
22. **Audit minimum:** material request/decision history retains actor account, effective authority/workspace, request type, affected entity, source reason where applicable, outcome, prior/resulting state/value where applicable, and timestamps, while secrets and unauthorized clinical content remain excluded.

### Current action

Apply G11 lifecycle-specific Approval Center behavior across PRD P-094–P-096, acceptance/traceability, OWN-02/OWN-03/OWN-09, and shared approval interactions; then commit and validate.

## 2026-09-25 — G11 GROUP COMMIT + COMMIT VALIDATION

### Main group commit

- `0f03e96b76247cd4accbc9d47a874bb376b5b549`
- Changed Documents 05–08.
- PRD advanced to v0.13.

### Validation result

**PASS.**

- P requirements remain P-001 through P-116 with no duplicate IDs.
- Acceptance scenarios extend through AC-058 with no duplicate IDs.
- UX acceptance scenarios extend through UXA-123 with no duplicate IDs.
- Screen contracts remain 49 with no duplicate screen IDs; G11 refines OWN-02, OWN-03 and OWN-09 rather than adding screens.
- PRD/acceptance/interaction align at v0.13; Document 07 is v0.12 with Parent PRD v0.13.
- Type-specific Owner action semantics, password-reset resolution, stale/current-state comparison, same-human multi-role attribution, direct Owner action separation, and clinical-content boundary are represented across Documents 05–08.

### Next exact action

Run cumulative backward compatibility against G1–G10 and reconcile any conflict before forward-impact analysis.

## 2026-09-25 — G11 BACKWARD COMPATIBILITY COMPLETE

**PASS — no reconciliation commit required.**

- **G1:** same-human multi-role actions remain legal where authority permits, but requester and Owner-decision events retain distinct effective authority/workspace attribution. Owner-only approval screens do not inherit Doctor clinical access.
- **G2:** password reset remains Pending -> Resolved by Set Temporary Credential, never generic Approve/Reject; disabled account is not re-enabled; target becoming Owner/ineligible cannot use the non-Owner reset path. Owner actions remain behind Owner authentication.
- **G3:** demographic correction remains Doctor-controlled and is intentionally not absorbed into Owner Approval Center.
- **G4:** waiver lifecycle, Direct Owner Waiver, and baseline-safe payment correction are preserved exactly. REM-029/030 are satisfied.
- **G5:** Visit-cancellation Pending does not freeze workflow, Completed-before-decision becomes stale, and same-human Doctor request/Owner approval remains separately attributable. REM-038 is satisfied.
- **G6:** Owner operational approval context continues to exclude unrestricted clinical content.
- **G7:** prescription correction/replacement remains Doctor-controlled and is not misclassified as Owner approval.
- **G8:** substitution approval remains prescribing/responsible-Doctor authority and is not moved into Owner Approval Center.
- **G9:** bill void/payment correction use latest/current payment truth, preserve no-refund/no-stock-restoration rules, and support post-Visit bill administration without reopening Visit. REM-057/058 are satisfied.
- **G10:** inventory adjustment/transfer approval uses captured vs current stock, stale blocking and linked transfer semantics; Direct Owner Adjustment remains outside fake self-approval. REM-063 is satisfied.

### Mandatory reminder dispositions

- REM-005 — SATISFIED.
- REM-010 — SATISFIED.
- REM-011 — SATISFIED.
- REM-029 — SATISFIED.
- REM-030 — SATISFIED.
- REM-038 — SATISFIED.
- REM-057 — SATISFIED.
- REM-058 — SATISFIED.
- REM-063 — SATISFIED.

### Next exact action

Evaluate G11 impact on G12–G15 and create only targeted reminders for downstream dependencies.

## 2026-09-25 — G11 FORWARD IMPACT ANALYSIS COMPLETE

### Resolved inherited reminders

- REM-005
- REM-010
- REM-011
- REM-029
- REM-030
- REM-038
- REM-057
- REM-058
- REM-063

### Targeted reminders created

- REM-067 -> G12: role/account lifecycle changes must preserve request history and current reset/Owner eligibility.
- REM-068 -> G13: exception reporting must distinguish effective/direct/resolved outcomes from rejected/stale work.
- REM-069 -> G14: global request/audit safety must preserve authority attribution, stale/duplicate protection, unknown-outcome handling, and content/secret boundaries.

### Reminder-register commit

`2cc84b573246ebc8d78234e60786b39a03e07833`

Open reminder count after G11: **37**.

## 2026-09-25 — G11 FINAL CLOSURE

### Final gate results

- **Gate A — Current-group validation:** PASS
- **Gate B — Backward compatibility with G1–G10:** PASS
- **Gate C — Forward impact/reminders:** COMPLETE
- **Gate D — Ledger/checkpoint state:** CURRENT

### Main Group 11 commit

`0f03e96b76247cd4accbc9d47a874bb376b5b549`

### Final verdict

- Locked BRD alignment: PASS
- Product completeness: HIGH
- Implementation readiness at PRD level: HIGH
- New clinic/business input required: NONE
- Nine inherited G11 reminders resolved.
- REM-067–REM-069 created.
- No unresolved backward conflict remains.
- Owner Approval Center is unified for work discovery but lifecycle-specific for actions.

### Transition

Proceed directly to G12 under the user's continuous-review instruction.

# G12 — Administration, Staff Access & Clinic Configuration

## Current checkpoint

**Stage:** PREPARING

### Primary requirements

- P-097 — Staff accounts
- P-098 — Role assignment
- P-099 — Account disablement
- P-100 — Owner role boundary
- P-101 — Configuration

### Mandatory prior-group reminders

- REM-006 — role changes must be compatible with already-open workspace revocation.
- REM-012 — newly created/granted Owner authority cannot be used until Owner TOTP enrollment/second-factor requirement is satisfied; Admin cannot grant Owner.
- REM-013 — account lifecycle and password recovery remain separate; reset never re-enables; disablement stops protected use; re-enable needs fresh sign-in.
- REM-031 — consultation-fee configuration changes are prospective and do not rewrite existing Visit applied amounts.
- REM-059 — medicine price/tax/billing configuration changes are prospective and do not rewrite frozen pharmacy bills.
- REM-064 — package conversions/thresholds/medicine metadata/prices must preserve historical base-unit movement truth; reconcile Admin configuration authority with Owner-controlled operational price requests.
- REM-067 — role/account lifecycle changes must preserve approval/reset history and current eligibility; pending non-Owner reset becomes non-actionable if target gains Owner authority.

### Required source review

- locked BRD account/role/Admin/Owner boundaries and clinic-configuration rules;
- current P-097–P-101 and adjacent configuration requirements;
- Administration screens ADM-01 onward and Owner staff/access screen;
- authentication/account-state interactions from G1/G2;
- financial/inventory configuration snapshots from G4/G9/G10;
- G11 request-history and reset-eligibility rules.

### Current action

Perform full G12 source review before autonomous product reasoning.

### Blockers

None.

## 2026-09-25 — G12 SOURCE REVIEW + DECISIONS RESOLVED

No new clinic/business input is required. These decisions preserve the locked Owner/Admin separation, account-security model, and the snapshot/history semantics already established by G4/G7/G9/G10/G11.

1. **Individual account lifecycle:** staff accounts are individual clinic-context accounts. Historical accounts are disabled, never hard-deleted from normal administration.
2. **Admin account scope:** Administrator may create/manage/disable/re-enable **non-Owner** staff and assign/revoke non-Owner roles. Admin cannot grant/revoke Owner authority, disable an Owner account, or use account-edit UI as an Owner-lifecycle bypass.
3. **Owner account scope:** Owner authority controls Owner-role lifecycle. Product prevents a role/account change that would leave the clinic with zero active Owner accounts.
4. **Owner grant security gate:** when Owner authority is newly granted to an existing/non-Owner account, that new Owner capability is unavailable until required Owner TOTP enrollment/verification gate is satisfied. Existing non-Owner roles may continue according to their normal permissions. Admin still cannot perform the grant. Resolves REM-012.
5. **Role revocation while signed in:** removing a role invalidates that authority/workspace on next protected navigation/action/permission refresh; already-open page does not preserve stale authority. Whole-account disablement ends protected use when detected. Resolves REM-006.
6. **Disable versus roles:** disablement is whole-account status; role revocation is authority-specific. Disabling does not silently delete role/history. Re-enable restores account availability but requires a fresh sign-in; it is separate from role edits.
7. **Password recovery separation:** password/reset credential changes never enable/disable an account and never assign/revoke roles. Disabling/re-enabling never silently changes password-reset state. Resolves REM-013.
8. **Pending business-request preservation:** disabling a requester or later changing their roles does not erase already-submitted waiver/cancellation/bill/inventory/transfer/payment-correction history. The request keeps the request-time actor/effective-authority attribution and remains governed by its target/current-state lifecycle rather than being silently deleted.
9. **Pending staff-reset eligibility:** if a non-Owner password-reset target later gains Owner authority, the old non-Owner reset request becomes non-actionable. Disabling the target does not make reset enable the account; if reset is performed while disabled, the account stays disabled. Resolves REM-067.
10. **Safe Owner/admin account display:** Admin may see enough Owner-account metadata to understand that the account exists/has protected Owner authority, but Owner-management actions are absent/denied. Admin cannot use direct URL/API action to modify protected Owner authority.
11. **Configuration categories are permission-specific:** P-101 is not a blanket “Admin can edit anything.” Configuration surfaces distinguish non-financial catalogue/inventory configuration from Owner-controlled financial/operational settings.
12. **Medicine catalogue:** authorized Admin/Owner may maintain current medicine catalogue identity/configuration used for future authoring. Existing finalized prescriptions, dispensing, bills, and audit history retain their stored historical medicine identity/context and are not silently rewritten by later catalogue edits.
13. **Medicine retirement:** a medicine with history is archived/disabled for future selection rather than hard-deleted. Historical references remain readable. Exact preload content remains clinic-supplied.
14. **Inventory base/package configuration:** authorized Admin/Owner may manage base/package conversion and threshold configuration where permitted, but changing conversion affects future entry/display only; current stock remains base-unit ledger truth and historical movements retain their original normalized quantity/conversion meaning. Resolves REM-064 conversion portion.
15. **Threshold configuration:** low-stock/near-expiry threshold changes may immediately change current alert classification because alerts are derived current-state views; they do not rewrite movement history.
16. **Operational stock boundary:** Admin configuration cannot create/add/remove/correct physical stock, approve inventory adjustments/transfers, or perform Direct Owner Adjustment. Operational stock changes remain G10 Owner-controlled movement workflows.
17. **Financial configuration authority:** consultation fee values and pharmacy price/tax values are Owner-controlled financial configuration in V1. Admin may view or maintain non-financial configuration but cannot directly publish a financial value change merely through Admin role.
18. **Pharmacist-proposed price change:** remains request -> Owner approval from G10/G11. Owner may directly set financial configuration. Neither path creates stock quantity movement.
19. **Consultation-fee prospectivity:** new fee configuration applies only to future Visits created after the configuration becomes effective; existing Visit applied amounts stay frozen and are corrected only through Visit/payment-specific workflow. Resolves REM-031.
20. **Pharmacy billing-price/tax prospectivity:** changed price/tax configuration applies to future bill snapshots/eligible future dispensing-billing use; existing created bills remain frozen and are not recalculated. Resolves REM-059.
21. **Price/config versus historical stock:** purchase/selling-price/config changes do not reinterpret historical movement quantities or already-created bill/prescription records. Resolves REM-064 price/history portion.
22. **Pharmacy unit lifecycle:** pharmacy units with history are archived/disabled rather than deleted. Disabled unit remains available in history/report drill-down but is unavailable for new dispensing or as a normal transfer destination/source. Unit lifecycle is Owner-controlled because it affects operational inventory accountability; Admin may maintain permitted non-operational metadata.
23. **Configuration audit:** material configuration changes record actor/account, effective authority, field/category, prior value, resulting value, and effective time. Secret/authenticator values are never included.
24. **No historical rewrite:** consultation fee, medicine identity, package conversion, thresholds, prices/tax, pharmacy-unit metadata, and output configuration changes never mutate already-stored historical business records merely because current configuration changed.
25. **Configuration validation:** do not allow invalid package conversion (non-positive), impossible self-referential unit setup, or configuration save that would erase required base-unit identity. Exact internal conversion representation remains technical.
26. **A4 output configuration boundary:** layout/config may be maintained as configured, but historical clinical/bill facts used in reprint remain the preserved record snapshots. Exact reprint-template behavior is reviewed in G15.
27. **Permission/current-state safety:** all Admin/Owner configuration actions revalidate current account authority before save. An already-open Admin/Owner page cannot preserve revoked authority.
28. **Unknown outcome/retry:** create account, disable/re-enable, role assignment/removal, Owner-role lifecycle action, and material configuration save check current effective account/config state before blind retry if outcome is unknown. Exact idempotency mechanism remains G14 technical design.
29. **No silent cascading deletion:** disabling/archiving staff, medicine, or pharmacy-unit configuration never erases existing Visits, prescriptions, dispensing, bills, inventory movements, approval history, or audit lineage.

### Current action

Apply G12 account/role/configuration boundaries across P-097–P-101, administration screens, acceptance/traceability, and interaction contracts; then commit and validate.

## 2026-09-25 — G12 GROUP COMMIT + COMMIT VALIDATION

### Main group commit

- `2eeb8aea4047fc321eb8104faf20cbb22ca5f63e`
- Changed Documents 05–08.
- PRD advanced to v0.14.
- Four file blobs were staged separately and committed atomically after the earlier oversized tool call was blocked; no partial product edit occurred before this commit.

### Validation result

**PASS.**

- P requirements remain P-001 through P-116 with no duplicate IDs.
- Acceptance scenarios extend through AC-070 with no duplicate IDs.
- UX acceptance scenarios extend through UXA-138 with no duplicate IDs.
- Screen contracts remain 49 with no duplicate screen IDs.
- PRD/acceptance/interaction align at v0.14; Document 07 is v0.13 with Parent PRD v0.14.
- Section 42 appears exactly once and carries administration/account/configuration interaction rules.
- Owner protection/TOTP gating, live role revocation, account/reset separation, prospective configuration, catalogue/unit archival, and no-historical-rewrite rules are explicit across Documents 05–08.

### Next exact action

Run cumulative backward compatibility against G1–G11; reconcile any conflict before G12 forward-impact analysis.

## 2026-09-25 — G12 BACKWARD COMPATIBILITY COMPLETE

**PASS — no reconciliation commit required.**

- **G1/G2:** Admin remains non-Owner authority; role revocation/disablement removes stale authority from open work; newly granted Owner capability remains TOTP-gated; password recovery stays separate from account/role state.
- **G3/G6/G7:** catalogue/account/config changes do not rewrite patient identity, clinical history, or finalized prescription history.
- **G4:** consultation-fee changes are prospective and existing Visit applied amounts remain frozen. REM-031 is satisfied.
- **G5:** account/config changes do not alter queue/Visit state.
- **G8:** medicine/package configuration never becomes a physical stock movement.
- **G9:** pharmacy price/tax changes are prospective and existing bill snapshots remain frozen. REM-059 is satisfied.
- **G10:** base-unit ledger truth and historical movement conversion are preserved; Admin configuration does not bypass Owner operational inventory control. REM-064 is satisfied.
- **G11:** role/account changes preserve request-time actor/authority history; non-Owner reset becomes non-actionable after Owner grant; revoked Owner/Admin authority cannot survive an open page. REM-067 is satisfied.

### Mandatory reminder dispositions

- REM-006 — SATISFIED.
- REM-012 — SATISFIED.
- REM-013 — SATISFIED.
- REM-031 — SATISFIED.
- REM-059 — SATISFIED.
- REM-064 — SATISFIED.
- REM-067 — SATISFIED.

### Next exact action

Evaluate G12 impact on G13–G15 and create only targeted downstream reminders.

## 2026-09-25 — G12 FORWARD IMPACT ANALYSIS COMPLETE

### Resolved inherited reminders

- REM-006
- REM-012
- REM-013
- REM-031
- REM-059
- REM-064
- REM-067

### Targeted reminders created

- REM-070 -> G13: reports must preserve historical staff/medicine/pharmacy-unit identity after archive/disable/config changes.
- REM-071 -> G14: security/state safety must cover TOTP-gated Owner grants, zero-active-Owner protection, live role/account revocation, account/config retry safety, and secret-free audit.
- REM-072 -> G15: output-template changes must not alter preserved historical facts/reprint semantics.

### Reminder-register commit

`658a0855ddc7fd5cf1f2432d843ef995f4e83191`

Open reminder count after G12: **33**.

## 2026-09-25 — G12 FINAL CLOSURE

### Final gate results

- **Gate A — Current-group validation:** PASS
- **Gate B — Backward compatibility with G1–G11:** PASS
- **Gate C — Forward impact/reminders:** COMPLETE
- **Gate D — Ledger/checkpoint state:** CURRENT

### Main Group 12 commit

`2eeb8aea4047fc321eb8104faf20cbb22ca5f63e`

### Final verdict

- Locked BRD alignment: PASS
- Product completeness: HIGH
- Implementation readiness at PRD level: HIGH
- New clinic/business input required: NONE
- Seven inherited G12 reminders resolved.
- REM-070–REM-072 created.
- No unresolved backward conflict remains.
- Admin remains non-Owner authority; configuration is prospective; historical business facts are non-destructive.

### Transition

Proceed directly to G13.

# G13 — Reporting, Analytics & Owner Visibility

## Current checkpoint

**Stage:** PREPARING

### Required source review

- locked BRD Section 8 analytics/report definitions and OD reporting decisions;
- current P-102 and reporting/Owner screens;
- queue timing history from G5;
- consultation financial correction/waiver semantics from G4;
- pharmacy bill/payment/void semantics from G9;
- inventory movement/reporting semantics from G10;
- exception outcome semantics from G11;
- archive/configuration history semantics from G12;
- all reminders targeting G13.

### Current action

Perform full G13 source review before autonomous product reasoning.

### Blockers

None.

## 2026-09-25 — G13 SOURCE REVIEW + DECISIONS RESOLVED

No new clinic/business input is required. The locked OD-029 definitions remain authoritative; G13 makes them deterministic across the history/correction semantics introduced in G4–G12.

1. **Clinic local date/time basis:** day-based reports use the configured clinic local date/time context. Exact timezone implementation remains technical.
2. **Patients seen per day:** count each Visit once when it first reaches Consultation Completed. Later clinical amendment or later Visit cancellation does not create another seen event or erase the original historical completion count. Resolves REM-043.
3. **Average waiting time:** measure from the queue-entry event for the waiting journey that actually leads to the Visit's first With Doctor transition.
4. **Same waiting journey:** doctor reassignment, Called, Unresponded, move-five-slots, or move-to-end preserve the existing waiting journey and original queue-entry timestamp.
5. **Removed/re-entered waiting journey:** when financial ineligibility or another rule removes the Visit from queue membership and a later eligible action inserts it again, that re-entry starts a new waiting segment. The report uses the segment that immediately precedes the first With Doctor transition; earlier abandoned segments remain history but do not inflate the successful wait duration. Resolves REM-039.
6. **Consultation revenue:** sum the current effective consultation payment records whose effective state is Paid; Waived/Unpaid are excluded. Approved corrections replace the effective reporting value/state while original/superseded records remain audit history and are never double-counted. Resolves REM-032.
7. **Consultation cancellation after Paid:** because V1 cancellation is not a refund, later cancellation/void does not erase a still-effective Paid external payment from recorded revenue. Cancellation is reported separately.
8. **Pharmacy revenue:** sum current effective pharmacy payment records in Paid state, using each bill's frozen amount and pharmacy-unit attribution. A later bill void without refund does not erase a still-effective Paid external payment from recorded revenue; the void is separately visible. Payment correction to Unpaid removes it from effective paid revenue. Resolves REM-060.
9. **Recorded revenue date:** for day-bucketed financial reporting, attribute recorded revenue to the effective Paid recording date/time, not merely Visit/bill creation date. A later approved correction that changes the effective Paid/Unpaid outcome must be reflected without double-counting prior historical state; exact historical-as-of reporting is not introduced in V1.
10. **Daily total recorded revenue:** consultation recorded revenue + pharmacy recorded revenue under the same selected date/time/filter basis.
11. **Payment-method breakdown:** use the current effective Paid payment method; historical superseded payment methods remain audit history but are not double-counted.
12. **Medicine sales quantity:** actual committed dispensed base quantity, not prescribed quantity, not billed quantity, and not stock adjustment/transfer quantity.
13. **Medicine sales value:** use the actual frozen bill-line value for billed supplied medicine where available; do not recalculate past sales from current price configuration. Unsupplied quantity contributes zero sales.
14. **Inventory current stock:** derive from current pharmacy-unit movement ledgers. Consolidated total is the sum of units; preserve unit drill-down.
15. **Valid versus expired stock:** current available stock excludes expired/unavailable quantity; expired recorded quantity is separately reportable and not silently merged into available stock.
16. **Low/out-of-stock/expiring:** use the current configured thresholds/current batch-expiry state. These are current-state reports and may change when threshold configuration changes; that does not rewrite movement history.
17. **Transfers:** linked pharmacy-to-pharmacy transfer is an internal movement, not medicine sale, stock addition, or clinic-wide stock gain/loss. Consolidated stock should net to zero for the transfer while unit positions change.
18. **Corrections/adjustments:** inventory corrections, loss/damage, expired disposition, and Owner direct adjustments retain their movement category and must not be counted as medicine sales. Resolves REM-065.
19. **Most prescribed medicines:** count finalized prescription-line prescribing events/quantities from the finalized version that was active for the Visit at the relevant time; replacement/supersession must not silently double-count both versions as independent current prescriptions. Historical version history remains inspectable. For the standard V1 aggregate, use the final current prescription version per Visit as the canonical prescription contribution.
20. **Waivers:** report effective approved/direct Owner waivers, not Pending/Rejected/Stale waiver requests. Preserve request counts/status separately where approval activity is shown.
21. **Cancellations/voids:** report effective approved/direct cancellation/void outcomes separately from request volume. Pending, Rejected, and Stale/Non-actionable requests may be reported as workflow activity but never as effective cancellations.
22. **Password reset / approval activity:** when audit/approval reporting includes Owner work, distinguish Pending, Approved, Rejected, Resolved, Stale/Non-actionable and direct Owner actions; never count a direct action twice as both request and approval. Resolves REM-068.
23. **Returning patients:** a Patient is returning when a new Visit is created for a Patient ID that already has at least one earlier Visit. Multiple Visits for the same Patient ID on the same day still follow that historical Visit rule. Possible Duplicate profiles remain separate Patients until a future merge workflow exists.
24. **Archived identities:** disabling staff, archiving medicine, or archiving pharmacy unit does not remove or reassign historical report attribution. Reports may show current label plus archived indicator, but stable identity/history remains preserved. Resolves REM-070.
25. **Audit activity reporting:** counts/events use the actual audit event stream, preserve actor/effective authority, and respect the clinical-content/secret boundaries. Audit-event count is not a proxy for successful business outcomes.
26. **Filters:** Owner reports support at least date range and relevant entity/unit filters where the report has that dimension. Filtering must not silently broaden unauthorized Admin/Pharmacist scope.
27. **Role access:** Owner receives clinic-wide report scope; Admin only authorized non-clinical report scope; Pharmacist unit-scoped pharmacy/inventory reports; Reception operational queue/reception information; Doctor-only does not inherit clinic-wide financial/inventory analytics.
28. **No bank-settlement claim:** financial reports remain recorded CRM financial status, not proof of bank settlement.
29. **No invented KPI targets:** G13 defines calculations and scope but does not invent business success thresholds or chart preferences.
30. **No destructive report recomputation:** current reports may reflect current effective corrections/configured thresholds, but source historical events/snapshots remain preserved.
31. **Empty/loading distinction:** reporting UI must distinguish loading, no-data-for-filter, and access-restricted states.
32. **Export:** exact report export format remains downstream/technical unless later configured; G13 does not introduce a required export file format.

### Current action

Apply deterministic report definitions to P-102–P-106, Owner/report screens, acceptance/traceability, and interaction contracts; then commit and validate.

## 2026-09-25 — G13 GROUP COMMIT + COMMIT VALIDATION

### Main group commit

- `6b301301da5f9e92c76297947c54d471663100dd`
- Changed Documents 05–08.
- PRD advanced to v0.15.

### Validation result

**PASS.**

- P requirements remain P-001 through P-116 with no duplicate IDs.
- Acceptance scenarios extend through AC-082 with no duplicate IDs.
- UX acceptance scenarios extend through UXA-151 with no duplicate IDs.
- Screen contracts remain 49 with no duplicate IDs.
- PRD/acceptance/interaction align at v0.15; Document 07 is v0.14 with Parent PRD v0.15.
- Reporting interaction Section 43 appears exactly once.
- Patients-seen, successful waiting journey, effective financial state, Paid+Voided no-refund reporting, dispense-based sales, movement-ledger inventory, canonical current prescription, effective exception outcomes, returning-patient identity and archived dimensions are explicit.

### Next exact action

Run cumulative backward compatibility against G1–G12; reconcile any conflict before G13 forward-impact analysis.

## 2026-09-25 — G13 BACKWARD COMPATIBILITY COMPLETE

**PASS — no reconciliation commit required.**

- **G1/G2/G12:** report scope never expands role authority; archived/disabled actor identity remains historical without preserving revoked access.
- **G3:** Possible Duplicate Patient IDs remain separate, so returning-patient calculation does not silently merge identities.
- **G4:** consultation revenue uses current effective Paid state, excludes Waived, preserves prior corrected records only as audit history, and keeps paid cancellation as recorded revenue because cancellation is not refund. REM-032 is satisfied.
- **G5:** average wait uses the successful waiting segment while ordinary reassignment/Unresponded/move-to-end preserves the same queue journey. REM-039 is satisfied.
- **G6:** patients seen anchors to original first Consultation Completed event; later amendment/cancellation does not create a second completion or erase historical completion. REM-043 is satisfied.
- **G7:** most-prescribed standard aggregate uses canonical current final prescription version and does not double-count Superseded versions.
- **G8:** medicine sales use committed dispensing rather than prescribed/unsupplied quantity.
- **G9:** pharmacy revenue uses current effective Paid record/frozen bill total; Paid+Voided without refund remains recorded money received while void is shown separately. REM-060 is satisfied.
- **G10:** stock reporting derives unit movement ledgers, separates expired/unavailable quantity, excludes transfers/adjustments from sales, and preserves unit/batch attribution. REM-065 is satisfied.
- **G11:** effective business outcomes remain distinct from Pending/Rejected/Stale workflow activity and direct Owner actions are not double-counted. REM-068 is satisfied.
- **G12:** archived staff/medicine/pharmacy-unit identities remain historical reporting dimensions. REM-070 is satisfied.

### Mandatory reminder dispositions

- REM-032 — SATISFIED.
- REM-039 — SATISFIED.
- REM-043 — SATISFIED.
- REM-060 — SATISFIED.
- REM-065 — SATISFIED.
- REM-068 — SATISFIED.
- REM-070 — SATISFIED.

### Next exact action

Evaluate G13 impact on G14–G15 and create only real downstream reminders.

## 2026-09-25 — G13 FORWARD IMPACT ANALYSIS COMPLETE

### Resolved inherited reminders

- REM-032
- REM-039
- REM-043
- REM-060
- REM-065
- REM-068
- REM-070

### Targeted reminder created

- REM-073 -> G14: reporting is derived/read-only, access-scoped, time-bucketed consistently, and reproducible from preserved source history.

### Reminder-register commit

`9d7538959c2181878af3942c7f7a9e7177ba8e18`

Open reminder count after G13: **27**.

## 2026-09-25 — G13 FINAL CLOSURE

### Final gate results

- **Gate A — Current-group validation:** PASS
- **Gate B — Backward compatibility with G1–G12:** PASS
- **Gate C — Forward impact/reminders:** COMPLETE
- **Gate D — Ledger/checkpoint state:** CURRENT

### Main Group 13 commit

`6b301301da5f9e92c76297947c54d471663100dd`

### Final verdict

- Locked BRD alignment: PASS
- Product completeness: HIGH
- Implementation readiness at PRD level: HIGH
- New clinic/business input required: NONE
- Seven inherited G13 reminders resolved.
- REM-073 created.
- No unresolved backward conflict remains.
- The locked report set is unchanged; calculations are now deterministic.

### Transition

Proceed directly to G14.

## 2026-09-25 — G13 POST-CLOSURE RECONCILIATION BEFORE G14

A live repository audit found that G13's product work was substantially correct but its recorded Gate C/reminder closure was incomplete.

### Findings corrected

- REM-022 remained OPEN even though G13 already defined returning patients by actual Patient ID and kept Possible Duplicate profiles separate.
- REM-048 remained OPEN even though G13 already prevented Superseded prescription versions from being double-counted in most-prescribed reporting.
- REM-053 was mostly covered by actual-committed-dispensing sales logic but needed one explicit product rule: approved substitution is reported against the **actual substitute medicine supplied**, with pharmacy-unit provenance retained.

### Reconciliation commit

`b78dd22ffd5d17b37110286c4a6063d6b325a432` — reconciled Documents 05–08 and Document 10.

### Revalidation

- P requirements: 116 unique.
- Acceptance scenarios: 83 unique through AC-083.
- UX acceptance scenarios: 152 unique through UXA-152.
- Screen contracts: 49 unique.
- REM-022 / REM-048 / REM-053: RESOLVED.
- Open reminders: 24 total.
- G14 reminders: 20.
- G15 reminders: 4.
- Backward compatibility: PASS; the substitute-sales clarification directly preserves G8 fulfilment truth and does not change prior workflow/state behavior.
- G13 closure Gates A–D: RECONFIRMED PASS after reconciliation.

### Bookkeeping correction

The Group Status Table, latest-completed-group commit, open-reminder count, G14 requirement ownership, and PR checklist were found stale relative to the detailed execution log and are being corrected before G14 source review.

---

# G14 — Cross-Product Audit, State Safety & Global Interaction Controls

## Current checkpoint

**Stage:** PREPARING

### Primary requirements

- P-107 — Human-readable history
- P-108 — Attribution
- P-109 — No normal-UI audit deletion
- P-110 — Clinical-content boundary in audit
- P-111 — Retry safety
- P-112 — Stale-state protection

P-113–P-116 belong to G15 Printing & Physical Outputs and are reviewed in G14 only for forward compatibility, not as G14-owned requirements.

### Required source review

- current P-107–P-112 and global interaction/audit/state-safety sections;
- all 20 open reminders targeting G14;
- audit/record-preservation rules in locked BRD;
- every completed G1–G13 stale-state, retry, authority, history, and secret-handling contract;
- responsive/loading/confirmation/audit patterns;
- P-113–P-116 only as G15 forward-impact context.

### Current action

Perform cumulative G14 source review before autonomous product reasoning.

## 2026-09-25 — G14 CUMULATIVE SOURCE REVIEW + DECISIONS RESOLVED

### Sources reviewed

- locked BRD audit/accountability/history/role boundaries;
- workflow/state model across Patient, Visit, consultation, prescription, pharmacy, payment, inventory and Owner control;
- V1 decision register and requirements traceability;
- current PRD P-107–P-112;
- existing acceptance, Owner Audit Activity screen, and interaction contracts;
- all completed G1–G13 ledger decisions relevant to stale state, retry, authority, history and secrets;
- all 20 OPEN reminders targeting G14.

### Decision result

No new clinic/business input is required. G14 generalizes already-accepted product safety behavior; it does not invent a new workflow or technical locking architecture.

1. **One global audit principle:** material business changes append attributable evidence. Correction, replacement, cancellation, void, approval, rejection, resolution and direct Owner action never erase the prior material record.
2. **Current vs historical:** every history surface distinguishes current/effective state from Superseded, Cancelled/Voided, Rejected, Resolved, Stale/Non-actionable and prior revision/value history. Historical records are read-only unless an explicit authorized correction workflow is invoked.
3. **Stable historical identity:** later staff disablement/role change, medicine archive, pharmacy-unit archive, Patient/Visit lifecycle change, or configuration change never reassigns or removes the historical actor/entity identity.
4. **Audit attribution minimum:** material event records actor account/human identity, effective role/workspace, event/action type, affected stable entity/reference, event time, and—where applicable—reason, request/decision relationship, prior/captured state/value, resulting state/value and source reference.
5. **Same-human multi-role attribution:** if one human performs requester and Owner/Doctor actions under distinct legitimate roles, audit preserves the same human identity but distinct effective-role/workspace events. Resolves REM-007.
6. **Direct action is not self-approval:** Direct Owner Waiver and Direct Owner Inventory Adjustment remain direct Owner-authority events. They are not fabricated as requester=Owner -> approver=Owner records.
7. **Audit visibility never expands source-data authority:** an audit viewer may see that an event occurred, but field/value/detail access follows the viewer's authority to the underlying data. Generic Owner/Admin audit does not reveal unrestricted clinical content merely because the audit event exists.
8. **Secret exclusion:** passwords, old/new/reset credentials, TOTP secrets/codes, recovery-code values and equivalent authentication secrets never enter normal audit/history. Safe metadata such as reset requested/resolved, TOTP enrolled, recovery codes regenerated/used may be recorded without the secret value. Resolves REM-014.
9. **Recovery-code state:** a used recovery code cannot authenticate again; regeneration invalidates all earlier recovery codes. Audit may record safe use/regeneration metadata, never code values. Resolves REM-017.
10. **No normal audit deletion/edit:** audit events cannot be deleted or rewritten through normal product UI. Exact legal retention/export/immutable-storage implementation remains compliance/technical design.
11. **Current authority beats loaded-page authority:** every protected final action rechecks current account status/role/effective authority. A role/account revocation detected while a tab is open removes protected use; the tab does not retain authority because it loaded earlier. Resolves REM-008, REM-016 and REM-071 authority portion.
12. **Independent tab/workspace context:** separate tabs may remain in different permitted workspaces, but one tab switching role never silently changes another tab's authority context. Each protected action checks that tab's explicit current workspace plus current account authority. Resolves REM-009.
13. **No cross-tab privilege promotion:** gaining a role in one session does not silently turn an already-open different-role page into that new authority. Newly granted Owner capability remains TOTP-gated before use.
14. **Duplicate-submit protection:** while a state-changing final request is in flight, its final control is not knowingly submitted twice from the same UI. Exact idempotency key/transaction mechanism remains architecture.
15. **Known failure vs unknown outcome:** a confirmed failure that produced no effect may be retried safely; an **unknown outcome** must first retrieve current effective state and linked effects before another final attempt. The UI must not claim success or blindly repeat. This generalizes P-111.
16. **Already-applied recovery:** if refresh discovers the intended effect already happened, recover/show that effective result instead of creating a second business effect.
17. **Sequenced partial-effect recovery:** where a user journey intentionally contains separately committed effects (for example Mark Paid succeeds but queue insertion fails), preserve the confirmed effect and resume from current truth rather than rollback/repeat it.
18. **Atomic business operations stay atomic:** where the product contract defines one atomic effective operation (dispense + stock deduction; linked source/destination transfer), a partial visible business result is invalid. Recovery checks/reconciles the operation before permitting another user attempt; the product never knowingly repeats only one side.
19. **Stale-state is baseline-specific, not 'any change = stale':** final action revalidates the facts that the decision/action depends on. If a material baseline changed so applying old intent could overwrite newer truth or violate safety, block as stale and require refresh/new proposal where appropriate.
20. **Latest-state decisions remain possible where explicitly designed:** if the workflow permits a decision against current truth despite change (for example bill-void review after payment became Paid), show the latest state/consequences and decide against that current state rather than automatically declaring the request stale.
21. **No silent conflict resolution:** stale quantity/state is never silently clipped, merged, partially applied or overwritten to make an old request fit. User sees the changed state and must review again.
22. **Patient creation safety:** final registration rechecks duplicate candidates; unknown create result is checked before retry so accidental duplicate Patient identity is not created. Resolves REM-023.
23. **Identity/demographic audit:** Possible Duplicate provenance retains candidate Patient IDs/actor/time; demographic correction retains request/direct-edit attribution and prior/proposed/resulting values subject to role visibility. Changed baseline blocks overwrite. Resolves REM-024 and REM-025.
24. **Visit/payment/waiver safety:** Visit creation, Paid recording, combined Paid+queue recovery, waiver, and payment correction follow duplicate/unknown-outcome/baseline rules already accepted in G4. One actionable request per defined baseline/type remains enforced. Resolves REM-033.
25. **Queue race rule:** Call, Start Consultation, Reassign, Unresponded/reposition, financial ineligibility, cancellation decision and Visit completion all revalidate current Visit state/Doctor/queue membership. The first valid committed transition wins; later incompatible action refreshes instead of applying. History of the earlier event remains. Resolves REM-040.
26. **Clinical draft/amendment conflict:** stale draft save cannot overwrite a newer saved draft/effective cancellation. Preserve local unsaved input long enough for user review/recovery rather than silently discarding/merging it. Amendment uses current revision baseline and appends a new revision; generic audit metadata does not leak clinical content. Resolves REM-044.
27. **Prescription version safety:** duplicate finalization cannot create multiple current Finalized versions; replacement atomically supersedes the then-current version and becomes current; stale replacement/cancellation blocks apply. Version lineage remains auditable. Resolves REM-049.
28. **Dispense safety:** final dispense rechecks Visit, current prescription version, fulfilment lineage/remaining allowance, approved substitution state, active pharmacy unit and valid stock. Multi-unit races cannot overfill. Dispensing + stock deduction is one effective atomic operation; unknown outcome checks both before retry. Resolves REM-054.
29. **Billing/payment/Visit-completion safety:** bill creation checks unbilled committed dispensing; Mark Paid does not duplicate payment; correction baseline cannot overwrite newer payment; bill-void decision uses current payment state; Visit completion rechecks all participating units/actionable fulfilment; post-Visit bill administration remains record-level. Resolves REM-061.
30. **Inventory safety:** movement history is append-only; pending adjustment/transfer reserves or changes nothing; approval/direct adjustment revalidates current stock; no operation may create negative stock; transfer is one linked two-sided event; unknown outcome checks movement/reference before retry. Resolves REM-066.
31. **Owner work safety:** request state and target baseline are revalidated; stale/resolved work cannot act twice; password reset is Resolved by credential set; direct Owner action stays distinct; current Owner authority is checked. Resolves REM-069.
32. **Account/config safety:** Owner grant remains TOTP-gated, zero-active-Owner protection remains enforced, role/account changes invalidate stale protected authority, and account/config unknown outcomes are checked before retry. Audit excludes secret values. Resolves REM-071.
33. **Reports are read-only derived views:** refresh/filter/drill-down cannot mutate source records. Current effective values must remain reproducible from preserved source/history; current role scope governs report access; clinic-local date/time bucketing is applied consistently. Resolves REM-073.
34. **Audit navigation safety:** linking from audit/history to a source record is allowed only when the current viewer is authorized for that source; otherwise show safe metadata without turning audit into a privilege bypass.
35. **High-impact confirmation remains explicit:** final destructive/control actions still require their existing explicit labeled confirmation. G14 does not introduce indiscriminate extra double-confirm dialogs.
36. **Technical boundary:** database locks, optimistic-version tokens, idempotency keys, event-store technology, session propagation transport, distributed transactions and retention storage are solution architecture details. The PRD fixes user-visible safety outcomes, not implementation mechanisms.

### Mandatory G14 reminder disposition plan

All 20 G14 reminders are satisfied by the decisions above and may move to RESOLVED only after Documents 05–08 implement and validate the contract:

- REM-007, REM-008, REM-009
- REM-014, REM-015, REM-016, REM-017
- REM-023, REM-024, REM-025
- REM-033, REM-040, REM-044, REM-049
- REM-054, REM-061, REM-066, REM-069, REM-071, REM-073

### Current action

Apply the G14 cross-product audit/history, authority revalidation, stale-state, unknown-outcome, concurrency and atomicity contract across Documents 05–08; then commit and validate before resolving reminders.

## 2026-09-25 — G14 GROUP COMMIT + COMMIT VALIDATION

### Main group commit

- `8613e6123d71a198f0b8f7d880f5fe9bf12533bd`
- Changed Documents 05–08.
- PRD advanced to v0.16.

### Validation result

**PASS.**

- P requirements remain P-001 through P-116 with no duplicate IDs.
- Acceptance scenarios extend through AC-103 with no duplicate IDs.
- UX acceptance scenarios extend through UXA-169 with no duplicate IDs.
- Screen contracts remain 49 with no duplicate screen IDs.
- PRD/acceptance/interaction align at v0.16; Document 07 is v0.15 with Parent PRD v0.16.
- Interaction Section 44 appears exactly once.
- Audit attribution, source-permission boundary, secret exclusion, current-authority revalidation, duplicate/unknown-outcome recovery, atomic-vs-sequenced effects, baseline-specific stale handling, cross-domain concurrency and read-only reporting safety are explicit.

## 2026-09-25 — G14 BACKWARD COMPATIBILITY COMPLETE

**PASS — no reconciliation commit required.**

- **G1:** effective role/workspace audit, independent tab contexts and stale-authority revocation are preserved; G14 does not force tabs to share workspace state.
- **G2:** Owner TOTP gating, disabled-account behavior, non-Owner reset lifecycle and one-time recovery-code/secret boundaries are preserved.
- **G3:** final Patient duplicate recheck, unknown-create recovery, Possible Duplicate provenance and stale demographic correction remain intact.
- **G4:** Paid+queue partial success preserves Paid; waiver/correction baselines remain stale-safe; no refund semantics are unchanged.
- **G5:** queue transitions use current Visit/Doctor/queue truth; first valid transition wins and later incompatible action refreshes without erasing queue history.
- **G6:** stale draft cannot overwrite newer/effectively-cancelled clinical state; amendments remain revision-based and clinical detail remains role-protected.
- **G7:** one current Finalized prescription, atomic replacement/supersession, cancellation blocking and preserved version lineage remain intact.
- **G8:** dispensing revalidates current version/allowance/substitution/unit/stock; multi-unit overfill remains blocked; dispense + stock remains one atomic effective operation.
- **G9:** bill/payment/correction/void/completion semantics remain type-specific; critically, payment change before bill-void decision is latest-state review, not automatic staleness.
- **G10:** movement immutability, no negative stock, no Pending reservation, direct Owner adjustment and linked atomic transfer remain intact.
- **G11:** lifecycle-specific Owner actions remain distinct; resolved/stale work cannot act twice; direct Owner actions are not self-approval artifacts.
- **G12:** Admin/Owner boundaries, TOTP-gated Owner grant, zero-active-Owner protection, live revocation and prospective configuration remain intact.
- **G13:** reporting remains read-only/permission-scoped and reproducible from preserved source history; G14 does not change report formulas.

### Mandatory reminder dispositions

The committed G14 contract satisfies all 20 inherited G14 reminders:

- REM-007, REM-008, REM-009
- REM-014, REM-015, REM-016, REM-017
- REM-023, REM-024, REM-025
- REM-033, REM-040, REM-044, REM-049
- REM-054, REM-061, REM-066, REM-069, REM-071, REM-073

### Next exact action

Resolve those reminders in Document 10, scan G15 for downstream impact, record only real G15 reminder(s), then run G14 final closure gates.

## 2026-09-25 — G14 FORWARD IMPACT ANALYSIS COMPLETE

### Resolved inherited reminders

All 20 G14 reminders are RESOLVED in Document 10:

- REM-007, REM-008, REM-009
- REM-014, REM-015, REM-016, REM-017
- REM-023, REM-024, REM-025
- REM-033, REM-040, REM-044, REM-049
- REM-054, REM-061, REM-066, REM-069, REM-071, REM-073

### Targeted reminder created

- REM-074 -> G15: printing/reprinting is read-only rendering over preserved source facts/snapshots and must not create duplicate business-state effects on retry.

### Reminder-register commit

`5bd70089713fe82b5fc3cb37a3dbad960472deb7`

Open reminder count after G14: **5**, all targeting G15.

## 2026-09-25 — G14 FINAL CLOSURE

### Final gate results

- **Gate A — Current-group validation:** PASS
- **Gate B — Backward compatibility with G1–G13:** PASS
- **Gate C — Forward impact/reminders:** COMPLETE
- **Gate D — Ledger/checkpoint state:** CURRENT

### Main Group 14 commit

`8613e6123d71a198f0b8f7d880f5fe9bf12533bd`

### Final verdict

- Locked BRD alignment: PASS
- Product completeness: HIGH
- Implementation readiness at PRD level: HIGH
- New clinic/business input required: NONE
- All 20 inherited G14 reminders resolved.
- REM-074 created for G15.
- No unresolved backward conflict remains.
- Audit/history, authority revalidation, stale-state protection, retry/unknown-outcome recovery and cross-domain concurrency are now one explicit global contract.

### Transition

Proceed directly to G15.

# G15 — Printing & Physical Outputs

## Current checkpoint

**Stage:** PREPARING

### Primary requirements

- P-113 — Prescription A4 output
- P-114 — Unavailable medicine legend
- P-115 — Pharmacy A4 output
- P-116 — No thermal dependency

### Mandatory prior-group reminders

- REM-050 — prescription print/reprint must use stored finalization-time availability snapshot; current Finalized is default; historical copies must be unmistakably historical.
- REM-055 — pharmacy output must show actual supplied quantity, unsupplied remainder, approved substitute where relevant, and pharmacy-unit context without altering prescription truth.
- REM-062 — pharmacy bill/output must use frozen bill snapshot/unit attribution, payment context, active-vs-void historical status, and never recalculate from current prices/state/stock.
- REM-072 — output-template changes are prospective presentation configuration only; historical business facts/snapshots remain preserved on reprint.
- REM-074 — print/reprint is read-only rendering and retrying output must not create prescription/bill/dispense/payment/stock/approval business effects.

### Required source review

- locked BRD prescription printing/unavailable-marker/A4/physical-output requirements;
- workflow/state model prescription and pharmacy-output behavior;
- decision register/traceability for print/reprint and thermal-printer boundary;
- current P-113–P-116;
- prescription/pharmacy screens and print interactions;
- G7/G8/G9/G12/G14 accepted snapshot, fulfilment, bill, configuration and read-only-output contracts;
- all five reminders targeting G15.

### Current action

Perform full G15 source review before autonomous product reasoning.

### Blockers

None.

### Blockers

None.

## 2026-09-25 — G15 SOURCE REVIEW + DECISIONS RESOLVED

No new clinic/business input is required. G15 implements the locked A4 prescription/output behavior using the immutable prescription, dispensing and bill snapshots already defined by G7–G9, the prospective output-configuration rule from G12, and the read-only rendering rule from G14.

1. **A4 is the V1 physical-output baseline:** prescription and optional pharmacy bill/dispensing output use standard A4-compatible rendering. Thermal receipt printing remains outside V1 and no workflow depends on a thermal device.
2. **Printer integration is not business logic:** browser/system print/PDF generation mechanics, supported printer drivers and PDF library are technical design. The product must provide printable A4 content without coupling workflow completion to printer success.
3. **Prescription source:** a prescription print is always tied to a specific Finalized prescription version. Normal Doctor print/reprint defaults to the latest current Finalized version.
4. **Finalization snapshot:** every prescription version renders its stored finalization-time medicine data and clinic-wide availability snapshot. Current stock is never substituted into that historical version. Resolves REM-050.
5. **Unavailable marker:** only items whose stored finalization-time clinic-wide state was Out of Stock or Not Stocked receive **. The legend explains they were unavailable from the clinic pharmacy at prescription finalization and should be obtained externally.
6. **Partial fulfilment later does not rewrite prescription:** later partial supply, stock changes, substitution or another unit's availability never changes the original prescription's ** marker/content. Pharmacy output carries later fulfilment truth.
7. **Historical prescription copy:** a Superseded prescription or a prescription printed from a Cancelled/Voided/closed historical Visit context must carry a prominent historical-status banner and must not look like the current clinic-dispensing source. The label describes workflow status; it does not invent a medical/legal statement that the prescription is invalid outside the CRM.
8. **Completed Visit copy:** a prescription reprinted after Visit completion remains printable for record/patient use but is visibly a copy from a closed clinic workflow and is not presented as active clinic-pharmacy dispensing work.
9. **No in-place correction through print:** print/reprint never edits prescription data, creates a replacement, changes Visit state, changes availability, or alters dispensing allowance.
10. **Prescription output content:** render clinic identity/header as configured, Patient/Visit identity required to identify the record, Doctor identity, prescription version/finalization context, prescribed medicine/instructions/quantity data from that version, ** markers and legend. Exact typography/branding/signature layout is configurable/design work unless already required elsewhere.
11. **Generated-at versus source time:** if a generated/reprint timestamp is shown, distinguish it from prescription finalization time so a reprint does not appear newly prescribed.
12. **Pharmacy A4 is unit-specific by default:** a pharmacy bill/dispensing output belongs to the pharmacy unit whose bill/dispensing records it represents. It never silently mixes Pharmacy A and B into one unit bill.
13. **Frozen bill facts:** pharmacy bill print/reprint uses frozen bill lines, supplied quantities, configured price/tax basis, total, unit, Visit and source-dispensing references. Current medicine price, current stock or a replacement prescription cannot recalculate the historical bill. Resolves REM-062.
14. **Current bill/payment status overlay:** when printing an existing bill, show its current business status (for example active or Cancelled/Voided) and current effective recorded payment context where applicable, while bill lines/total remain frozen. This is current status presentation, not bill recalculation.
15. **Voided bill historical copy:** Cancelled/Voided bill output is prominently historical/voided and cannot look like a new payable active bill. If a Paid record remains because V1 void creates no refund, show the recorded payment context separately rather than implying refund.
16. **Actual fulfilment truth:** pharmacy dispensing summary uses committed actual supplied quantities, not merely prescribed quantities. Unsupplied remainder is shown separately and contributes no supplied/billed quantity. Resolves REM-055.
17. **Approved substitution presentation:** when an approved substitute was actually supplied, the supplied line identifies the actual substitute medicine and may reference the original prescribed item as substitution lineage; do not print the original as though it was the medicine supplied.
18. **Pharmacy-unit attribution:** actual supplied lines/bill output retain the dispensing pharmacy unit. Any future/configured consolidated summary must preserve per-unit attribution rather than erasing it.
19. **In-progress vs closed fulfilment summary:** if a dispensing summary is generated before clinic-pharmacy fulfilment is closed, label it as current/in-progress and show the generation time because remaining supply may change. A closed/completed summary uses committed recorded fulfilment; reprint never invents later supply.
20. **Template/configuration prospectivity:** A4 layout/header/footer/configuration changes affect future renders. Historical source business facts/snapshots do not change. Resolves REM-072.
21. **V1 template-history decision:** reprint may use the **current configured visual A4 template** with the preserved historical source facts/snapshots. V1 does not require retaining every old template version or byte-identical original PDF. If a later compliance decision requires exact rendered-document retention, that is a separate compliance/technical requirement.
22. **Read-only rendering:** generating, previewing, printing, reprinting, downloading/refreshing printable output or retrying after printer/browser uncertainty creates no new prescription version, bill, dispense, payment, stock movement, approval, Visit transition or other business-state effect. Resolves REM-074.
23. **Print failure is not workflow failure:** printer/PDF/render failure shows output-specific retry/error while preserving source workflow state. User may retry rendering because it is read-only.
24. **Print authorization follows source authority:** Doctor prescription output remains under Doctor prescription access; Pharmacist pharmacy bill/dispensing output remains under pharmacy/billing authority. Output routes do not broaden role permissions.
25. **Historical output access follows current permission:** a bookmarked/old printable route rechecks current authority and source visibility before rendering.
26. **No hidden state mutation from print status:** marking a bill Paid, voiding a bill, completing a Visit, dispensing, or replacing a prescription always occurs in the owning workflow, never as a consequence of clicking Print.
27. **A4 consultation receipt remains optional configuration:** the locked BRD says standalone consultation receipt is not core V1. G15 does not create a new mandatory receipt workflow.
28. **No QR/barcode requirement:** future physical-file QR/barcode remains outside V1 and is not added to outputs.
29. **Five inherited reminders are fully addressed by these decisions:** REM-050, REM-055, REM-062, REM-072 and REM-074.

### Current action

Apply G15 prescription/pharmacy A4, historical labeling, snapshot/configuration, permission and read-only-rendering contracts across Documents 05–08; then commit and validate.

## 2026-09-25 — G15 GROUP COMMIT + COMMIT VALIDATION

### Main group commit

- `359cf7f734494b4c6446479142986125efc768f2`
- Changed Documents 05–08.
- PRD advanced to v0.17.

### Validation result

**PASS.**

- P requirements remain P-001 through P-116 with no duplicate IDs.
- Acceptance scenarios extend through AC-111 with no duplicate IDs.
- UX acceptance scenarios extend through UXA-178 with no duplicate IDs.
- Screen contracts remain 49 with no duplicate IDs.
- PRD/acceptance/interaction align at v0.17; Document 07 is v0.16 with Parent PRD v0.17.
- Interaction Section 45 appears exactly once.
- Prescription snapshot/reprint, historical labeling, actual fulfilment/substitution, frozen bill facts, template prospectivity, read-only retry and no-thermal dependency are explicit.

## 2026-09-25 — G15 BACKWARD COMPATIBILITY COMPLETE

**PASS — no reconciliation commit required.**

- **G1:** printable routes obey current source/workspace authority; output does not create new cross-workspace context.
- **G2:** printing exposes no authentication secret and does not change account/authentication state.
- **G3:** Patient/Visit identity on output uses existing stable records and printing cannot create/merge Patient identity.
- **G4:** consultation payment/waiver/queue state is unchanged; optional consultation A4 acknowledgement remains configuration, not a new receipt workflow.
- **G5:** queue/Visit state is never changed by print/preview/reprint.
- **G6:** prescription output does not expand non-Doctor access to unrestricted clinical notes or mutate completed clinical records.
- **G7:** selected Finalized version and stored finalization-time availability snapshot remain authoritative; Superseded/closed-context copies are historical and reprint creates no version.
- **G8:** pharmacy output shows actual committed fulfilment, unsupplied remainder, approved substitute supplied and pharmacy-unit attribution without rewriting prescription truth.
- **G9:** bill lines/total/unit/source remain frozen; current payment/void status is an overlay; Voided output is historical and no print action causes refund/rebill/stock restoration.
- **G10:** print/reprint never changes stock; pharmacy-unit provenance remains visible.
- **G11:** print does not approve/reject/reset/directly apply Owner-controlled work.
- **G12:** current A4 template may change presentation prospectively while historical business facts/snapshots remain preserved; no old-template archive is invented.
- **G13:** output does not mutate source/report state or recalculate historical financial/inventory facts.
- **G14:** preview/print/reprint is read-only; current authority is rechecked and unknown printer/browser outcome retries rendering rather than business action.

### Mandatory reminder dispositions

The committed G15 contract satisfies all five inherited G15 reminders:

- REM-050
- REM-055
- REM-062
- REM-072
- REM-074

### Next exact action

Resolve the five G15 reminders in Document 10, run G15 Gates A–D, close G15, then perform final cross-document/global PRD validation before declaring the long-lived refinement complete.

## 2026-09-25 — G15 REMINDER CLOSURE COMPLETE

### Resolved inherited reminders

- REM-050
- REM-055
- REM-062
- REM-072
- REM-074

### Reminder-register commit

`cc759b52ea59f3e536f6fd71c2e72d3b648cd9e5`

Open reminder count after G15: **0**.

No future refinement group remains; no new reminder was created.

## 2026-09-25 — G15 FINAL CLOSURE

### Final gate results

- **Gate A — Current-group validation:** PASS
- **Gate B — Backward compatibility with G1–G14:** PASS
- **Gate C — Forward impact/reminders:** COMPLETE — no future group remains
- **Gate D — Ledger/checkpoint state:** CURRENT

### Main Group 15 commit

`359cf7f734494b4c6446479142986125efc768f2`

### Final verdict

- Locked BRD alignment: PASS
- Product completeness: HIGH
- Implementation readiness at PRD level: HIGH
- New clinic/business input required: NONE
- All five inherited G15 reminders resolved.
- No future reminders remain OPEN.
- No unresolved backward conflict remains.
- A4 prescription/pharmacy output is now explicitly snapshot-safe, historical-status aware, permission-safe and read-only.

### Transition

All 15 PRD refinement groups are complete. Run final global cross-document validation before changing the long-lived refinement PR from its draft/completion state or merging it.









