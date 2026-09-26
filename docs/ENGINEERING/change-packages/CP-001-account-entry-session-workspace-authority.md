# CP-001 — Account Entry, Session & Workspace Authority Foundation

## Document control

| Field | Value |
| --- | --- |
| Change Package | CP-001 |
| Status | TASK PLAN READY — implementation not started |
| Product baseline | IB-001 |
| Effective PRD changes | None |
| Continuity owner | Agent 3 |
| Documentation repository | `surya-gorla/health-crm-docs` |
| Implementation repository | `surya-gorla/health-crm` |
| Created | 2026-09-26 |
| Current implementation branch | Not yet created |
| Parent implementation baseline | `fa9bbff607b257752f855021211a2d2d183d6dd1` |

---

# 1. Objective

Establish the secure account-entry, authenticated-session, credential-recovery and workspace-authority foundation that every later protected Hospital CRM workflow depends on.

The package must make the following product truths executable before later patient/visit/clinical/pharmacy modules begin:

- each staff member uses one individual account;
- Owner-containing accounts cannot enter any normal workspace before the Owner second-factor gate is satisfied;
- non-Owner accounts do not receive mandatory 2FA in V1;
- staff forgotten-password recovery is Owner-controlled rather than self-service;
- Owner-reset credentials cannot enter normal work until forced replacement succeeds;
- disabled accounts cannot enter or continue protected work;
- a multi-role human uses one account but acts through an explicit effective workspace/authority context;
- separate tabs may use different permitted workspaces;
- current backend authority, not loaded UI state, controls protected actions;
- authentication/recovery secrets never become normal audit/history content.

This package deliberately establishes the authority/session substrate before patient, visit, queue, clinical, prescription, pharmacy, inventory, reporting or administration feature packages consume it.

---

# 2. Why CP-001 is first

Every later Change Package needs a stable answer to:

- who is signed in;
- whether the account is currently enabled;
- which roles are currently assigned;
- whether Owner authentication gates are satisfied;
- which workspace/authority the current tab is operating under;
- whether that authority is still current at protected-action time;
- how the human identity and effective authority are attributed in audit history.

Implementing business modules before these guarantees exist would force later rework and would allow different agents to invent incompatible access models.

The package therefore follows the already-accepted engineering bootstrap:

- ADR-001 — same-origin modular monolith;
- ADR-002 — TypeScript / React / NestJS / PostgreSQL / Prisma 7 stack;
- ADR-003 — opaque server-side sessions + backend-authoritative access context;
- ADR-004 — PostgreSQL transactions/idempotency/append-only audit;
- ADR-006 — real-PostgreSQL integration tests + Playwright + CI.

No new architecture choice is introduced by CP-001 unless implementation evidence proves a bootstrap decision incomplete.

---

# 3. Governing product sources

## 3.1 Locked BRD

Primary locked sources:

- FR-064 — supported role set and multi-role individual accounts;
- FR-069 — group-based access with individual accounts;
- FR-084 — individual clinic-context login/password;
- FR-085 — mandatory TOTP for every account containing Owner authority;
- FR-097–FR-100 — Owner workspace separation, Owner/Doctor separation, non-Owner 2FA rule;
- FR-101 — staff Forgot Password creates Owner reset request;
- FR-102 — Owner sets temporary/new credential without reading existing password;
- FR-103 — forced replacement after Owner reset; auditable reset;
- FR-104 — one-time Owner recovery codes;
- BR-034–BR-036 — role separation and fixed-clinic individual login;
- BR-039 — one individual account may hold multiple roles without collapsing authorities;
- BR-043 — Owner TOTP mandatory; non-Owner no 2FA;
- BR-044 — Owner-controlled staff password recovery;
- BR-045 — reset credential must be replaced after next valid login;
- BR-046 — recovery-code rules and no in-app catastrophic Owner bypass;
- BR-054 — Owner/Admin authority does not grant unrestricted Doctor clinical access;
- BR-057 — Administrator cannot mutate Owner authority;
- OD-021 — authentication;
- OD-022 — role/group permissions;
- OD-023 — single-clinic topology with role/unit scaling.

CP-001 does not change any of these locked business rules.

## 3.2 Accepted PRD requirements

Primary requirements owned by this package:

- P-001 — Workspace identity
- P-002 — Role-safe navigation
- P-003 — Multi-role switching
- P-004 — Workspace-scoped context preservation
- P-005 — Permission-aware shell
- P-008 — Individual account login
- P-009 — Owner TOTP
- P-010 — Non-Owner login
- P-011 — Staff Forgot Password
- P-012 — Owner reset
- P-013 — Forced credential replacement
- P-014 — Owner TOTP enrollment and recovery codes
- P-015 — Disabled account
- P-096 — Effective authority attribution for multi-role users

Cross-product constraints consumed by the package:

- P-107–P-110 audit/history principles where authentication/account events produce safe audit metadata;
- P-111/P-112 safety rules where current authority, stale state or unknown outcomes apply.

The package does not claim ownership of unrelated downstream business workflows merely because they later reuse these cross-product rules.

---

# 4. Screens and interaction contracts

## 4.1 In-scope product screens

- SH-01 — Login
- SH-02 — Owner TOTP Verification
- SH-03 — Staff Forgot Password Request
- SH-04 — Forced Password Change
- SH-05 — Owner TOTP Enrollment & Recovery Codes
- SH-06 — Workspace Selector / Switcher
- OWN-09 — Staff Password Reset, only the minimal Owner reset capability required by P-012

The general application shell is in scope only to the extent required to:

- show signed-in human identity;
- show active workspace/authority;
- provide safe logout;
- provide an always-reachable switcher for multi-workspace users;
- deny unauthorized routes/actions;
- distinguish no-authority from temporarily unavailable state.

No downstream patient/queue/clinical/pharmacy content is introduced by the shell in CP-001.

## 4.2 Interaction contracts consumed

Primary:

- IX Section 26 — Permission and State-Unavailability Pattern
- IX Section 29 — Multi-Role and Workspace Interaction Contract
- IX Section 35 — Authentication and Credential-Recovery Interaction Contract
- IX Section 44.1–44.8 — audit identity/visibility/secret exclusion/current authority/tab context
- IX Section 44.9–44.10 — duplicate submission and unknown-outcome safety where high-impact auth/recovery actions require it
- IX Section 44.23–44.24 — Owner reset/account-role/config safety where applicable

---

# 5. Acceptance ownership

## 5.1 Acceptance scenarios CP-001 must fully prove

Authentication/user-account acceptance:

- AU-001 — Owner 2FA mandatory for whole account login
- AU-002 — Staff no 2FA
- AU-003 — Disabled account
- AU-004 — Initial Owner TOTP enrollment gate
- AU-005 — Owner authority added during an active non-Owner session
- AU-006 — Forgot Password does not enumerate accounts
- AU-007 — Repeated Forgot Password does not duplicate current request
- AU-008 — Owner reset resolves request without changing account status
- AU-009 — Forced password change blocks normal work
- AU-010 — Recovery code is a one-time second factor
- AU-011 — Authentication secrets are not audit content
- AU-012 — Normal workspace switching does not repeat authentication
- AU-013 — Individual fixed-clinic account login

Workspace/interaction acceptance:

- UXA-019 — Single-role entry bypasses workspace selector
- UXA-020 — Multi-role switch control remains reachable
- UXA-021 — Permission absence versus state unavailability
- UXA-022 — Workspace switch does not leak active record context
- UXA-023 — Unsaved work protected during workspace switch
- UXA-024 — Revoked role cannot remain usable through an open workspace
- UXA-025 — Multiple tabs retain independent authority context
- UXA-026 — Cross-workspace attention does not leak protected detail
- UXA-027 — Same human can act through two legitimate authorities with separate attribution, at the authority/audit substrate level

Release-blocking authentication cases relevant to this package include the negative scenarios that prohibit:

- Owner TOTP bypass;
- Owner-containing account entering another workspace before 2FA;
- self-service staff password reset;
- account enumeration through Forgot Password;
- duplicate actionable reset requests;
- reset credential entering normal work before replacement;
- password reset implicitly enabling a disabled account;
- reused/invalidated recovery-code authentication;
- authentication secret leakage to audit/history;
- disabled active session continuing protected use;
- revoked authority remaining usable through an already-open page/tab.

## 5.2 Acceptance that receives foundation support now but requires later cross-domain revalidation

These scenarios depend on later business workflows and must **not** be marked globally complete merely because CP-001 creates the authority/audit mechanism:

- AC-050 — same human acts under distinct legitimate authorities in a real request/approval workflow;
- AC-084 — audit preserves effective authority for same-human multi-role actions in material domain events;
- UXA-114 — Owner approval context does not leak Doctor-only clinical content;
- UXA-118 — same-human requester/approver attribution on real downstream request detail.

CP-001 must make those behaviors technically possible and test the generic authority/audit substrate. Their full domain-specific acceptance remains a closure obligation of the later Change Packages that first implement those workflows.

---

# 6. Explicit non-scope

CP-001 does **not** implement:

- patient search/registration/profile;
- Visit creation/payment;
- Doctor queue;
- consultation/clinical record;
- prescription;
- pharmacy/dispensing/billing;
- inventory;
- generic Owner Approval Center workflows other than the minimal staff password reset operation required by P-012;
- general staff account/role administration from P-097–P-101;
- reports;
- printing;
- production hosting/provider selection;
- catastrophic Owner recovery;
- general authenticator device migration/replacement beyond accepted TOTP enrollment/recovery-code behavior;
- final password-strength/rate-limit/lockout values unless a technical security baseline is needed and can be established without changing product policy.

A development/test fixture or bootstrap account mechanism is allowed to make the package testable. It is not a user-facing staff-management feature and must not be presented as the final production provisioning workflow.

---

# 7. Impact analysis

## 7.1 Domain/state impact

CP-001 needs canonical engineering definitions for:

- Staff Account;
- Role Assignment;
- Account enabled/disabled state;
- authentication gate state;
- server-side Session;
- per-session Owner second-factor readiness;
- TOTP enrollment state;
- Recovery Code verifier/use state;
- Staff Password Reset Request;
- temporary/reset credential state;
- per-tab Workspace Context;
- safe authentication/account audit events.

These definitions must preserve human identity separately from effective authority/workspace.

## 7.2 Data impact

Expected persisted concerns include:

- account identity/login and credential verifier;
- account status;
- role assignments;
- server-side sessions;
- TOTP secret ciphertext + key version metadata;
- recovery-code one-way verifiers and use/invalidation state;
- reset request lifecycle;
- reset/temporary credential verifier and forced-change state;
- append-only safe audit metadata;
- timestamps/version/revision data needed for revocation/current-state checks.

Exact schema/table names are implementation decisions and must be established as canonical engineering truth during CP-001 rather than invented independently by frontend/backend agents.

## 7.3 API/command impact

The package is expected to establish explicit contracts for capabilities such as:

- sign in;
- current session/authentication state;
- Owner TOTP verify;
- initial Owner TOTP enrollment;
- recovery-code verify;
- recovery-code regeneration;
- Forgot Password request;
- Owner list/open/reset eligible staff-reset request;
- forced password replacement;
- logout;
- current allowed workspace/role context;
- current authority/permission refresh.

Exact routes and DTOs are not fixed by this package record. They must be defined once in the canonical API/OpenAPI contract before implementation consumers diverge.

## 7.4 Authorization impact

Backend policy must be authoritative.

Protected actions/navigation must re-evaluate at least:

- account enabled state;
- currently assigned role;
- Owner second-factor readiness when Owner capability is involved;
- requested effective workspace;
- target/resource authority where later workflows require it.

Workspace context is tab-scoped client state and must not be treated as a globally trusted server-session workspace.

## 7.5 UI impact

The web application must establish:

- authentication gate routing;
- workspace-entry routing;
- visible identity + active workspace shell;
- tab-local workspace context;
- safe role-revocation handling;
- unsaved-work protection on workspace change;
- access-denied versus state-unavailable presentation.

## 7.6 Audit/logging impact

Authentication/account audit must preserve safe metadata while excluding:

- passwords;
- old/new/reset credential values;
- TOTP secret;
- TOTP codes;
- recovery-code values.

Operational logs and business/security audit remain separate.

---

# 8. Contract delta required before/during implementation

CP-001 is authorized to establish only the contracts needed for the scope above.

Required canonical engineering contracts:

1. authentication/session state contract;
2. account/role/workspace authority contract;
3. authentication/account persistence contract;
4. authentication REST/OpenAPI contract;
5. auth/account audit-event contract;
6. shared auth error-code contract;
7. frontend auth/workspace state contract;
8. testing/evidence contract for auth/session revocation, multi-tab context and secret exclusion.

If implementation reveals a product ambiguity, do not silently decide in code. Classify it under Document 11 and create a PRD-CHG only if product truth genuinely needs amendment.

---

# 9. Implementation task plan

The default implementation sequence is:

## Task A — Canonical contracts and data foundation

- define account/role/session/auth-gate/reset/audit engineering contracts;
- establish Prisma/PostgreSQL schema and reviewed constraints;
- establish migration/test database foundation;
- define stable error codes and OpenAPI shapes.

## Task B — Backend authentication/session core

- credential verification;
- opaque session creation/rotation/destruction;
- current-session endpoint;
- enabled/disabled enforcement;
- current role/authority lookup;
- CSRF/same-origin protections;
- safe audit events;
- no secret logging.

## Task C — Owner TOTP and recovery codes

- initial enrollment;
- secret encryption-at-rest integration;
- verification;
- recovery-code one-time use;
- regeneration invalidating prior set;
- Owner gate enforcement.

## Task D — Staff password recovery

- non-enumerating Forgot Password;
- one current actionable Pending request;
- Owner reset action;
- forced password replacement;
- stale/resolved/ineligible request protection.

## Task E — Workspace authority and shell

- single-role direct entry;
- multi-workspace selection/switching;
- tab-scoped workspace context;
- visible active authority;
- unauthorized route protection;
- role revocation/disablement behavior;
- unsaved-work switch guard.

## Task F — Integrated acceptance and regression

- real PostgreSQL integration tests;
- concurrency/stale/duplicate-reset tests;
- multi-tab Playwright scenarios;
- Owner/non-Owner login paths;
- TOTP/recovery-code paths;
- disabled/revoked authority;
- secret-exclusion checks;
- OpenAPI/contract validation;
- cumulative build/type/lint/test gates.

Backend/frontend may be implemented sequentially, but CP-001 remains open until the integrated package passes the relevant product acceptance.

---

# 10. Known dependencies and risks

## 10.1 Initial account provisioning

The accepted product defines account behavior, not a final production first-Owner provisioning UX.

CP-001 may use controlled development/test fixtures or a reviewed technical bootstrap mechanism to make the package executable.

Production first-Owner/staff provisioning must not be invented as clinic policy and remains a deployment/staff-administration dependency unless governing sources establish it.

## 10.2 Current-authority refresh

The product requires revoked/disabled authority to stop on the next protected action/navigation or permission refresh; it does not require a specific real-time transport.

CP-001 should prefer the smallest safe mechanism consistent with ADR-003 rather than introduce WebSockets/event infrastructure without evidence.

## 10.3 Password/rate-limit specifics

Password-strength values, brute-force/rate-limit/lockout values and catastrophic Owner recovery are explicit technical/security dependencies rather than settled clinic business rules.

If a minimum safe implementation baseline is necessary, document it as engineering/security truth and do not present it as a user-approved business rule.

---

# 11. Change Package closure gates

## Gate A — Product traceability

Must prove:

- P-001–P-005, P-008–P-015 and P-096 implementation traceability;
- relevant AU/UXA scenarios mapped to tests;
- no new hidden business policy;
- downstream-only scenarios remain explicitly deferred rather than falsely closed.

## Gate B — Engineering compatibility

Must prove compatibility with:

- IB-001;
- ADR-001–ADR-006;
- same-origin modular-monolith boundary;
- PostgreSQL/session/authority decisions;
- existing AGENTS.md rules;
- no conflicting parallel auth/API/data contracts.

## Gate C — Implementation verification

At minimum:

- build/type/lint;
- unit tests;
- real-PostgreSQL integration/migration tests;
- OpenAPI validation;
- relevant Playwright E2E;
- secret/logging assertions;
- session revocation/current-authority tests.

## Gate D — Product acceptance

Must pass the fully-owned AU/UXA authentication/workspace scenarios and relevant negative/release blockers.

Domain-specific same-human dual-role acceptance remains forward-linked to the later packages that introduce those domain actions.

## Gate E — Continuity

Before COMPLETE:

- canonical auth/data/API/UI contracts are current;
- CP-001 record is current;
- tests/evidence are current;
- implementation branch/PR state is recorded;
- forward impacts are recorded;
- next package/dependency is discoverable.

---

# 12. Current checkpoint

**Stage:** TASK PLAN READY

Completed:

- live docs and implementation repositories inspected;
- IB-001 and ADR-001–ADR-006 confirmed on docs `main`;
- implementation repository foundation and root/path-specific AGENTS instructions confirmed on implementation `main`;
- locked BRD sources reviewed;
- accepted P/acceptance/screen/interaction sources reviewed;
- package boundary selected;
- impact areas mapped;
- required contract delta identified;
- implementation task sequence defined.

Not yet started:

- implementation-repository CP-001 branch;
- canonical CP-001 engineering contracts;
- schema/API/UI implementation;
- executable tests.

## Next exact action

After this CP-001 record is validated and merged into the documentation repository:

1. reconcile stale implementation-repository bootstrap-status wording;
2. create the CP-001 implementation branch from live `health-crm/main`;
3. establish the canonical CP-001 auth/session/authority contracts before product code diverges;
4. begin Task A.

No product feature implementation should begin before this package record is live on docs `main`.
