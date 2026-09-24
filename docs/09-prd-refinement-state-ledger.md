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
| Current PRD version | v0.6 DRAFT |
| Current group | None — G4 complete; starting G5 next |
| Current stage | G4 COMPLETE / TRANSITIONING |
| Completed groups | G1, G2, G3, G4 |
| In-progress groups | None |
| Not started | G5–G15 |
| Open cross-group conflicts | 0 |
| Open future reminders | 27 — see Document 10 |
| Latest group closure commit | `a54e3d23dea2d1765d3ae467e26c00d3d78d3c74` |

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
