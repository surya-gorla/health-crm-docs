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
| Current PRD version | v0.9 DRAFT |
| Current group | None — G7 complete; starting G8 next |
| Current stage | G7 COMPLETE / TRANSITIONING |
| Completed groups | G1, G2, G3, G4, G5, G6, G7 |
| In-progress groups | None |
| Not started | G8–G15 |
| Open cross-group conflicts | 0 |
| Open future reminders | 37 — see Document 10 |
| Latest completed group main commit | `eec1ae4e4b951456798eb008c710e0d305a1aa50` |

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
