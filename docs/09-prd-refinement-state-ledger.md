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
| Current PRD version | v0.5 DRAFT |
| Current group | G3 — Patient Search, Identity, Registration & Patient Profile |
| Current stage | FORWARD IMPACT ANALYSIS — G3 to G4–G15 |
| Completed groups | G1, G2 |
| In-progress groups | G3 |
| Not started | G4–G15 |
| Open cross-group conflicts | 0 |
| Open future reminders | 13 — see Document 10 |
| Latest checkpoint commit | `5b437164ed4492451bbd1e4b26c4a1cc38e4bbc2` |

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
| G3 | Patient Search, Identity, Registration & Patient Profile | FORWARD IMPACT ANALYSIS | `b7db51edcd73f650a8e3f27bc7e98d21f4e3a438` | PASS vs G1–G2 | IN PROGRESS | Current group |
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
