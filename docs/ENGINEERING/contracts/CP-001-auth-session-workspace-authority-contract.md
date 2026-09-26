# CP-001 Contract — Authentication, Session & Workspace Authority

## Document control

| Field | Value |
| --- | --- |
| Contract | CP-001 Authentication / Session / Workspace Authority |
| Status | PROPOSED — effective when merged to docs `main` |
| Change Package | CP-001 |
| Product baseline | IB-001 |
| Effective PRD changes | None |
| Continuity owner | Agent 3 |
| Implementation branch | `surya-gorla/health-crm:cp-001/account-entry-session-workspace-authority` |
| Created | 2026-09-26 |

---

# 1. Purpose

This contract is the canonical engineering interpretation of the account-entry/session/workspace-authority behavior required by CP-001.

It exists so backend, frontend, database and tests do not independently invent incompatible meanings for:

- authentication state;
- Owner second-factor readiness;
- forced password replacement;
- disabled/revoked authority;
- per-tab workspace context;
- staff reset requests;
- recovery codes;
- current-session state;
- stable auth/account errors;
- audit attribution.

This contract does **not** change product truth.

If implementation proves a product ambiguity rather than an engineering gap, use Document 11 PRD change control instead of silently changing this contract.

---

# 2. Canonical role and workspace vocabulary

## 2.1 Account roles

V1 role names are:

- Owner
- Administrator
- Receptionist
- Doctor
- Pharmacist

One account may hold more than one role.

Roles are permissions on one human identity; they are not separate accounts.

## 2.2 Effective workspace/authority context

A material protected action is evaluated under an explicit effective workspace/authority.

For CP-001 the canonical workspace role values are:

- `owner`
- `administrator`
- `reception`
- `doctor`
- `pharmacist`

The active workspace role is **not persisted as one global server-session workspace**.

The browser tab owns its current workspace selection.

A protected request supplies the tab's intended workspace role to the API.

The API revalidates that requested workspace against current account state, role assignments and Owner second-factor readiness.

Future packages may extend a workspace with additional resource scope, such as pharmacy-unit context, without changing the rule that the tab context is explicit and backend-validated.

---

# 3. Authentication state model

## 3.1 Session authentication phases

The server-side session exposes exactly one effective authentication phase:

1. `UNAUTHENTICATED`
2. `PASSWORD_VERIFIED_REPLACEMENT_REQUIRED`
3. `PASSWORD_VERIFIED_OWNER_TOTP_ENROLLMENT_REQUIRED`
4. `PASSWORD_VERIFIED_OWNER_SECOND_FACTOR_REQUIRED`
5. `AUTHENTICATED`

These are engineering states, not new user-facing business statuses.

## 3.2 Entry transitions

### Normal non-Owner credential

`UNAUTHENTICATED -> AUTHENTICATED`

only when:

- username/login + password are valid;
- account is enabled;
- the credential is not a temporary/reset credential requiring replacement;
- the account does not currently contain Owner authority.

### Owner-containing account, TOTP already enrolled

`UNAUTHENTICATED -> PASSWORD_VERIFIED_OWNER_SECOND_FACTOR_REQUIRED -> AUTHENTICATED`

Normal workspace entry is impossible until second factor succeeds.

### Owner-containing account, TOTP not enrolled

`UNAUTHENTICATED -> PASSWORD_VERIFIED_OWNER_TOTP_ENROLLMENT_REQUIRED -> AUTHENTICATED`

The transition to `AUTHENTICATED` occurs only after:

- TOTP enrollment secret is created;
- a generated TOTP successfully proves enrollment;
- recovery codes are created and shown for that enrollment event;
- the product-required acknowledgement step is satisfied.

### Valid temporary/reset credential

`UNAUTHENTICATED -> PASSWORD_VERIFIED_REPLACEMENT_REQUIRED -> AUTHENTICATED`

Normal workspace entry is impossible until replacement succeeds.

If the account contains Owner authority at the time replacement completes, the session must still pass the applicable Owner second-factor/enrollment gate before `AUTHENTICATED`.

The implementation must not use forced replacement to bypass Owner TOTP.

## 3.3 Disabled account

Disabled account state overrides valid credentials and active session state.

- disabled account cannot transition into a normal authenticated workspace;
- a previously authenticated session becomes unusable for protected work as soon as disablement is detected on session refresh/protected request;
- re-enabling the account does not resurrect a terminated/invalidated old session.

---

# 4. Server-side session contract

## 4.1 Session token

Browser authentication uses an opaque random session token.

The raw token:

- exists only in a Secure, HttpOnly, same-site cookie;
- is never exposed through normal JSON payloads;
- is stored server-side only as a verifier/hash or other non-recoverable representation where feasible;
- is rotated when authentication privilege meaningfully increases.

Privilege-increasing transitions include at least:

- password verified -> authenticated after Owner second factor;
- reset credential -> authenticated after replacement;
- unauthenticated -> authenticated.

## 4.2 Session record minimum state

A persisted session must be able to determine:

- session stable ID;
- account stable ID;
- authentication phase;
- created time;
- last validated/used time needed by technical session policy;
- invalidated/revoked time where applicable;
- whether Owner second-factor requirements for this authenticated session are satisfied;
- CSRF verifier/token state;
- safe correlation/audit reference where useful.

It must **not** persist one global active workspace that would silently affect all tabs.

## 4.3 Current-authority check

Every protected request revalidates current:

- session validity;
- account enabled state;
- requested workspace role;
- current role assignment;
- Owner second-factor readiness when Owner authority is requested;
- target-specific authority when a later package requires it.

Loaded frontend state is never sufficient proof of authority.

## 4.4 Session invalidation

At minimum, the backend can invalidate:

- one session;
- all sessions for an account when required by a security/account action.

Exact cross-device push propagation is not required.

The required product behavior is satisfied when stale authority is blocked on the next protected request/navigation/auth-state refresh.

---

# 5. Browser workspace-context contract

## 5.1 Storage

Current workspace selection is tab-scoped.

Permitted baseline mechanism:

- `sessionStorage` or equivalent tab-local state.

Do not use `localStorage` or a global account/session value in a way that changes sibling tabs.

## 5.2 Request propagation

Protected API requests carry the intended effective workspace role using:

`X-Workspace-Role`

Allowed values are the canonical workspace role values from Section 2.2.

The API:

- rejects an absent workspace header when a protected endpoint requires an effective workspace;
- rejects a workspace not currently assigned/permitted;
- rejects Owner workspace use when current Owner second-factor readiness is not satisfied;
- records effective workspace role in audit metadata for material actions.

A future package may introduce additional scope headers/fields such as pharmacy-unit context. That extension must not replace current role validation.

## 5.3 Workspace switching

Switching workspace:

- changes only the current tab's workspace context;
- does not itself create/reissue a login;
- does not repeat Owner TOTP merely because the workspace changes after the session has already satisfied all applicable gates;
- must clear or explicitly re-open protected record context rather than silently carrying Patient/Visit/queue/pharmacy context;
- must trigger unsaved-work protection before discarding material local work.

---

# 6. CSRF and same-origin contract

State-changing browser requests require both:

- same-origin enforcement;
- a session-bound CSRF token.

Engineering baseline:

- the current-session endpoint may expose a non-secret CSRF token;
- the web client sends it in `X-CSRF-Token` for protected state-changing requests;
- the API validates it against the current server-side session;
- CSRF tokens are not authentication substitutes and do not authorize a workspace/role.

Authentication endpoints that establish a session still enforce origin checks and any endpoint-appropriate CSRF/bootstrap mechanism selected by the API implementation.

---

# 7. Password and credential contract

## 7.1 Password verifier

Passwords and temporary/reset credentials use Argon2id verifiers.

Never persist or log recoverable plaintext passwords.

## 7.2 Temporary/reset credential

An Owner-created reset credential:

- replaces the current sign-in credential;
- is marked as requiring forced replacement;
- does not change account enabled state;
- does not change role assignments;
- resolves the linked reset request on successful reset action;
- cannot be retrieved/read later from storage.

After successful replacement:

- the temporary credential verifier is replaced by the new password verifier;
- the forced-replacement flag is cleared;
- the old/reset credential no longer authenticates.

---

# 8. Owner TOTP contract

## 8.1 Secret storage

The TOTP secret is:

- Google Authenticator-compatible;
- encrypted at rest;
- stored with encryption-key version metadata;
- never written to ordinary logs/audit history.

## 8.2 Initial enrollment

Enrollment requires:

1. valid password-authenticated pending session;
2. generated TOTP secret;
3. proof using a valid generated TOTP;
4. recovery-code creation;
5. one-time display of recovery codes;
6. required acknowledgement;
7. authenticated-session transition only after completion.

An incomplete enrollment does not allow normal workspace entry.

## 8.3 Second-factor verification

For an already enrolled Owner-containing account:

- valid current TOTP satisfies the second factor;
- one valid unused recovery code may satisfy the second factor;
- invalid/expired/already-used/invalidated code does not.

## 8.4 Recovery codes

Recovery codes:

- are generated as a set;
- are shown only for enrollment/regeneration;
- are stored as one-way verifiers;
- are individually single-use;
- previous unused set is invalidated when a new set is generated;
- values are excluded from normal audit/history and operational logs.

---

# 9. Staff password-reset request contract

## 9.1 Forgot Password submission

Public command accepts the staff login identifier.

The external response is intentionally generic.

The response must not reveal whether the identifier:

- exists;
- is disabled;
- contains Owner authority;
- is otherwise ineligible.

For one eligible enabled non-Owner account:

- create a Pending reset request if no current actionable Pending request exists;
- otherwise retain/use the existing actionable Pending request;
- do not create multiple simultaneously actionable Pending requests.

Owner accounts do not enter this workflow.

## 9.2 Reset-request state

Persisted lifecycle:

- `PENDING`
- `RESOLVED`

Current actionability is additionally derived from current target state.

A request can be non-actionable even if historical status remains Pending, for example if the target becomes ineligible before Owner action.

Do not invent a user-visible approval/rejection lifecycle for password reset.

## 9.3 Owner reset action

The Owner action is:

`Set Temporary Credential`

Before commit, revalidate:

- acting session;
- Owner authority + Owner second-factor readiness;
- request still Pending/actionable;
- target account still eligible for this non-Owner reset flow;
- target current state.

On success, one transaction must:

- replace the target credential verifier with temporary/reset verifier;
- mark forced replacement required;
- mark/reset-request status Resolved;
- append safe audit evidence;
- preserve account enabled state and roles.

Unknown outcome must be recovered from current request/credential-operation state before another effective attempt.

---

# 10. Canonical API surface for CP-001

All routes are under `/api/v1`.

Exact DTO field names may evolve only through this same canonical contract before frontend/backend divergence.

## 10.1 Public/account-entry routes

### `POST /auth/login`

Input:

- `login`
- `password`

Returns one safe next-gate result:

- `AUTHENTICATED`
- `PASSWORD_REPLACEMENT_REQUIRED`
- `OWNER_TOTP_ENROLLMENT_REQUIRED`
- `OWNER_SECOND_FACTOR_REQUIRED`

Invalid credentials return generic auth failure.

Disabled/unavailable account returns safe unavailable behavior after credential validation without exposing unrelated account existence.

### `POST /auth/forgot-password`

Input:

- `login`

Always returns the same generic accepted response shape.

No eligibility/account-existence detail is returned.

### `POST /auth/password/replace-reset-credential`

Allowed only in `PASSWORD_VERIFIED_REPLACEMENT_REQUIRED`.

Input:

- `newPassword`
- `confirmPassword`

Successful result routes into whichever remaining authentication gate applies.

## 10.2 Owner second-factor routes

### `POST /auth/owner-second-factor/verify`

Allowed only in `PASSWORD_VERIFIED_OWNER_SECOND_FACTOR_REQUIRED`.

Input:

- `method: "totp" | "recovery_code"`
- `code`

Successful verification transitions the session to `AUTHENTICATED`.

### `POST /auth/owner-totp/enrollment/start`

Allowed only in `PASSWORD_VERIFIED_OWNER_TOTP_ENROLLMENT_REQUIRED`.

Returns the one-time enrollment representation needed by SH-05.

### `POST /auth/owner-totp/enrollment/verify`

Input:

- generated TOTP code.

On success:

- confirms enrollment;
- creates recovery-code set;
- returns that set for one-time display;
- does not yet expose it through later session reads.

### `POST /auth/owner-recovery-codes/regenerate`

Requires fully authenticated Owner authority.

Explicitly invalidates the prior recovery-code set and returns the new one-time set.

## 10.3 Authenticated session routes

### `GET /auth/session`

Returns safe current-session state including:

- authentication phase;
- signed-in human/account display identity where authenticated/pending-auth gate permits;
- currently assigned role/workspace options safe to expose;
- Owner second-factor readiness relevant to workspace entry;
- CSRF token when applicable;
- current selected workspace is **not** returned as a global server-session value.

### `POST /auth/logout`

Invalidates current session and clears browser session cookie.

## 10.4 Owner staff-reset routes

Require:

- authenticated session;
- `X-Workspace-Role: owner`;
- current Owner role;
- current Owner second-factor readiness.

### `GET /owner/password-reset-requests`

Returns safe actionable/history metadata for staff reset requests.

Never returns password/reset credential values.

### `GET /owner/password-reset-requests/:requestId`

Returns safe request/target metadata needed to perform the reset action.

### `POST /owner/password-reset-requests/:requestId/set-temporary-credential`

Input:

- `temporaryPassword`
- `confirmTemporaryPassword`
- stable operation/idempotency identifier.

Per Section 9.3, success resolves request and requires target forced replacement.

---

# 11. Stable error-code contract

Frontend behavior must branch on stable codes, not free-text messages.

Initial CP-001 codes:

- `AUTH_INVALID_CREDENTIALS`
- `AUTH_ACCOUNT_UNAVAILABLE`
- `AUTH_SESSION_REQUIRED`
- `AUTH_SESSION_INVALID`
- `AUTH_PASSWORD_REPLACEMENT_REQUIRED`
- `AUTH_OWNER_TOTP_ENROLLMENT_REQUIRED`
- `AUTH_OWNER_SECOND_FACTOR_REQUIRED`
- `AUTH_SECOND_FACTOR_INVALID`
- `AUTH_RECOVERY_CODE_INVALID`
- `AUTH_WORKSPACE_REQUIRED`
- `AUTH_WORKSPACE_FORBIDDEN`
- `AUTH_OWNER_SECOND_FACTOR_NOT_READY`
- `AUTH_CSRF_INVALID`
- `AUTH_CURRENT_AUTHORITY_CHANGED`
- `RESET_REQUEST_NOT_ACTIONABLE`
- `RESET_REQUEST_ALREADY_RESOLVED`
- `RESET_TARGET_INELIGIBLE`
- `OPERATION_OUTCOME_UNKNOWN`

Error payload baseline:

- `code`
- safe human-readable `message`
- `correlationId`
- optional safe structured details when the product contract requires the changed/current value to be shown.

Never include secret credential/TOTP/recovery values in error details.

---

# 12. Audit-event contract

Authentication/account events may record safe metadata such as:

- stable actor/account ID;
- target account ID where applicable;
- effective workspace role when a material action occurs under a workspace;
- event type;
- outcome;
- timestamp;
- reset request ID or stable operation ID where applicable;
- correlation ID.

Initial event families include:

- login succeeded/failed with safe anti-enumeration handling;
- Owner TOTP enrollment completed;
- Owner second factor succeeded/failed;
- recovery code used;
- recovery codes regenerated;
- Forgot Password request accepted/created/deduplicated internally;
- Owner set temporary credential;
- forced password replacement completed;
- logout/session invalidation;
- account/role authority rejection detected on protected use.

Never record:

- password;
- temporary/reset credential;
- TOTP secret;
- TOTP code;
- recovery-code value.

Audit metadata must not convert generic public authentication failure into an account-enumeration leak.

---

# 13. Frontend state contract

Frontend must model:

- current authentication phase from the API;
- signed-in identity;
- available permitted workspaces;
- tab-local selected workspace;
- unsaved-work state;
- authority/session refresh result;
- stable API error code.

Routing rules:

- unauthenticated -> SH-01;
- password replacement required -> SH-04 only;
- Owner enrollment required -> SH-05 only;
- Owner second factor required -> SH-02 only;
- fully authenticated single workspace -> enter it directly;
- fully authenticated multiple workspaces -> SH-06;
- account/session invalidated -> sign-in boundary;
- role revoked but other workspaces remain -> route to permitted workspace selection/safe shell.

Frontend must not infer normal authenticated access from a successful password response when a later gate is still required.

---

# 14. Persistence invariants

Database/migration implementation must guarantee, where feasible:

- unique individual login identifier according to chosen normalized-login rule;
- one current account row per human account identity;
- explicit account enabled/disabled state;
- many role assignments per account;
- session rows attributable to account;
- no recoverable plaintext password/reset credential;
- TOTP secret ciphertext + key version, never plaintext field;
- recovery-code verifier rows with used/invalidated state;
- no more than one **actionable** current Pending staff reset request per eligible target account;
- reset request history retained after resolution;
- audit rows append-only through normal application flow;
- stable internal IDs distinct from human-facing identities.

Exact table/index names are implementation details but the invariants are not.

---

# 15. Concurrency and retry rules

## 15.1 Forgot Password deduplication

Concurrent/repeated eligible submissions must converge on one actionable Pending request.

Use database uniqueness/transaction semantics, not request timing assumptions.

## 15.2 Owner reset

Two Owner actions against one Pending request cannot both become effective.

The first valid committed reset resolves the request.

Later attempts re-read current truth and return non-actionable/resolved state.

## 15.3 Recovery code use

One recovery code can succeed once.

Concurrent use must be serialized/conditioned so a second use cannot authenticate.

## 15.4 Recovery-code regeneration

Regeneration and code use must not allow an old set to remain valid after the regeneration transaction takes effect.

## 15.5 Current authority

Role/account changes that occur after a page loaded are checked on the next protected request.

No protected endpoint authorizes solely from stale roles cached in session/client state.

---

# 16. Test contract

CP-001 must include executable tests covering at least:

## Authentication

- normal enabled non-Owner login;
- invalid credential generic response;
- disabled account blocked;
- Owner password cannot enter normal workspace without second factor;
- Owner enrollment path;
- Owner existing-TOTP path;
- valid/invalid TOTP;
- valid recovery code succeeds once;
- reused recovery code fails;
- regeneration invalidates previous recovery set.

## Password reset

- public Forgot Password response identical for eligible/ineligible/unknown/Owner/disabled identifiers;
- repeated eligible submissions produce one actionable Pending request;
- Owner reset resolves the request;
- second reset attempt cannot apply the same request again;
- reset does not enable account or modify roles;
- reset credential reaches forced replacement only;
- forced replacement invalidates temporary credential;
- disabled account blocks replacement continuation.

## Workspace/session authority

- single-role direct entry;
- multi-role workspace selector;
- workspace context remains tab-local;
- sibling tab workspace does not change;
- Owner workspace rejected if Owner readiness is absent;
- role revocation blocks next protected use;
- account disablement blocks next protected use;
- logout invalidates session;
- unauthorized direct route/API access denied.

## Secret/audit safety

Automated assertions must show ordinary API/log/audit responses never expose:

- password/reset credential values;
- TOTP secret/code;
- recovery-code values after the one-time display surface.

## Database/concurrency

Use real PostgreSQL tests for:

- reset-request deduplication;
- recovery-code single-use;
- concurrent Owner reset;
- session/authority revalidation behavior where transaction ordering matters.

---

# 17. Forward-extension points

Later packages may append, without invalidating this contract:

- target-resource authority checks;
- pharmacy-unit scope;
- patient/Visit context;
- domain-specific audit event payloads;
- cross-workspace attention counts;
- staff-role management actions;
- account lifecycle UI.

They must not weaken:

- explicit effective workspace role;
- backend current-authority validation;
- Owner gate semantics;
- per-tab workspace independence;
- secret exclusion;
- non-destructive reset/audit history.

---

# 18. Current checkpoint

**Stage:** PROPOSED CONTRACT

Completed:

- CP-001 is live on docs `main`;
- CP-001 implementation branch exists;
- auth/session/workspace contract has been derived from locked BRD, accepted PRD, ADR-001–ADR-006 and implementation AGENTS rules.

Next after validation/merge:

1. update CP-001 record to point to this established contract and live implementation branch;
2. update implementation-repo stale bootstrap-status metadata;
3. begin Task A schema/OpenAPI/shared contract implementation on the CP-001 implementation branch;
4. do not begin SH-01–SH-06 UI or auth service implementation until the schema/shared machine-readable contract aligns with this canonical contract.
