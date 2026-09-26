# Hospital CRM — Minimum Engineering Bootstrap

## Document control

| Field | Value |
| --- | --- |
| Status | ACCEPTED WHEN MERGED TO `main` |
| Product baseline | IB-001 |
| Continuity owner | Agent 3 |
| Scope | Cross-cutting engineering foundations only |
| Implementation repository | `surya-gorla/health-crm` |
| Supersession model | ADRs are superseded explicitly; never silently rewritten |

---

# 1. Purpose

This document establishes only the engineering choices with enough blast radius that multiple implementation agents must share them before production code begins.

It does **not** predefine every endpoint, table, DTO, UI component, error code, report query, or domain field.

If this file is viewed on an unmerged branch, it is proposed engineering truth. It becomes current engineering truth only after merge to the default branch.

---

# 2. Governing product constraints

The bootstrap is derived from IB-001 and preserves these product facts:

- V1 is a web application.
- V1 is internet-dependent; offline mode is not required.
- desktop/laptop browser is the primary client; tablet responsiveness may be supported.
- V1 is initially one clinic, while supporting multiple Doctors, Reception staff and pharmacy units without redesigning the permission model.
- payment execution stays outside the CRM; the CRM records payment state.
- individual staff accounts are required.
- any account containing Owner authority requires password + Google Authenticator-compatible TOTP.
- separate browser tabs may legitimately operate in different permitted workspace contexts.
- protected actions must recheck current authority; loaded UI is not authorization.
- clinical, prescription, financial, inventory, configuration and audit history must not be silently rewritten.
- dispensing + stock deduction and linked pharmacy transfers require atomic effective behavior.
- stale and unknown-outcome behavior must fail safely.
- A4 browser/system printing is required; thermal printing is not.
- no third-party integration is required for V1.
- compliance/retention obligations still require external validation.

---

# 3. Bootstrap decisions

## 3.1 Repository and application topology

Use two repositories:

- product/governance/engineering authority: `surya-gorla/health-crm-docs`
- executable application: `surya-gorla/health-crm`

The application repository is a TypeScript monorepo using pnpm workspaces.

Initial structural boundary:

```text
health-crm/
├── apps/
│   ├── web/
│   └── api/
├── packages/
│   └── contracts/
├── AGENTS.md
├── package.json
└── pnpm-workspace.yaml
```

Do not introduce Turborepo/Nx initially. pnpm workspaces are sufficient until measured build/coordination needs justify another orchestration layer.

Production topology is a **same-origin modular monolith**:

```text
Browser
   |
 HTTPS
   |
Single application origin
   |
   +-- React SPA
   |
   +-- NestJS REST API
           |
           +-- PostgreSQL
```

No microservices, message broker, Redis, search cluster or separate analytics warehouse is part of the bootstrap.

Rationale: the V1 workload is one clinic with strongly transactional cross-domain workflows. A modular monolith preserves transaction boundaries and minimizes distributed-state failure modes.

See ADR-001.

---

## 3.2 Runtime and platform stack

Bootstrap stack:

- Node.js **24 LTS**; initial implementation pin: **24.21.0**
- pnpm **12**; initial implementation pin: **12.7.0**
- TypeScript — strict mode; exact compatible version pinned in the implementation lockfile
- frontend: **React 19.3**
- frontend build/dev: **Vite 8.1**
- backend: **NestJS 12**, ESM project
- database: **PostgreSQL 18**; baseline current minor observed during bootstrap: **18.6**
- ORM/migrations: **Prisma ORM 7 stable line**
- do not adopt Prisma ORM 8 release-candidate builds for V1 bootstrap

Reason for Prisma 7: the current Prisma 8 line is still release-candidate software and its release-status documentation lists missing capabilities, including transaction-isolation support, that matter to this product's concurrency-sensitive workflows.

Patch versions are pinned by the implementation repository lockfile. Major-version upgrades require compatibility review and, when cross-cutting, an ADR supersession/update.

See ADR-002.

---

## 3.3 API and contract style

Use JSON REST under a versioned API prefix such as `/api/v1`.

Use explicit business-command endpoints for material state transitions rather than generic CRUD mutation when the product defines a named business action.

Examples of the style, not final endpoint definitions:

- mark paid
- start consultation
- complete consultation
- finalize prescription
- replace prescription
- dispense
- void bill
- approve/reject controlled request
- transfer stock

OpenAPI is the machine-readable API contract.

Backend response/error contracts must include stable machine-readable error codes. The frontend must not branch on free-text error messages.

Exact endpoints and DTOs are created only by the Change Package that needs them.

---

## 3.4 Authentication, session and authority foundation

Browser authentication uses an **opaque server-side session**, not a browser-stored bearer JWT as the primary session mechanism.

Foundation rules:

- session token is held in an `HttpOnly`, `Secure`, same-site cookie;
- server-side session state is persisted in PostgreSQL for V1;
- password hashing uses Argon2id;
- Owner TOTP follows the existing product requirement;
- TOTP secrets are encrypted at rest with a versioned application encryption key from the deployment secret store;
- recovery-code values are shown only when product behavior permits and are stored as one-way verifiers, not recoverable plaintext;
- state-changing requests use same-origin enforcement plus CSRF protection;
- disabling an account or revoking authority must invalidate/prohibit continued protected use.

Workspace context is **per browser tab**, not one global workspace value inside the server session.

The web client retains its tab workspace context in tab-scoped state (for example `sessionStorage`) and sends the explicit workspace/authority context with protected requests.

The API validates that requested context against current account status, role assignment, resource state and target workspace on every protected action.

Frontend hiding/disablement remains UX only; backend policy enforcement is authoritative.

No Redis session store is introduced unless observed load/operational evidence later justifies it.

See ADR-003.

---

## 3.5 Data, concurrency, idempotency and audit foundation

PostgreSQL is the authoritative transactional store.

Foundation rules:

- internal record identifiers use PostgreSQL-native UUIDv7 where a globally unique internal key is required;
- clinic-facing identifiers such as Patient ID / Visit ID remain separate human-facing business identifiers;
- database constraints are part of correctness, not optional documentation;
- material atomic effects execute inside database transactions;
- concurrency-sensitive quantity/financial operations may use row-level locks;
- baseline-sensitive editable records use an explicit current-version/revision check where stale overwrite must be rejected;
- high-impact commands receive a stable client operation/idempotency identifier and persist enough operation state to recover safely after an unknown outcome;
- database uniqueness/invariant constraints back application checks where feasible;
- do not use distributed locks in V1 unless later evidence requires them.

Prisma 7 is the normal data-access/migration layer, but reviewed raw SQL migrations/queries are allowed where PostgreSQL-native constraints, locking or correctness require them.

Production migrations must never depend on destructive reset workflows.

Audit is **append-only evidence**, not full event sourcing.

Material audit rows should be committed in the same transaction as their owning business effect where atomic attribution is required.

General application logs and business audit are separate systems.

Never place passwords, reset credentials, TOTP secrets/codes, recovery-code values or unrestricted clinical content into ordinary logs/audit metadata.

See ADR-004.

---

## 3.6 Environment, secrets and deployment model

Use separate environments:

- local development
- automated test
- staging
- production

Each environment has isolated databases and secrets.

Production architecture:

- one HTTPS application origin;
- containerized application service capable of serving the built SPA and API under the same origin;
- managed PostgreSQL;
- provider-managed TLS;
- encrypted database storage/backups;
- deployment secrets supplied through the platform secret store, never committed.

The concrete cloud/hosting provider and production region are intentionally **not** selected in this bootstrap because applicable patient-data/privacy/data-residency requirements still require external validation.

The chosen architecture must remain portable across a managed container/service + managed PostgreSQL provider.

---

## 3.7 Backup and recovery engineering targets

The BRD requires technical backup/recovery targets. The initial V1 engineering targets are:

- continuous/near-continuous managed PostgreSQL point-in-time recovery capability;
- engineering RPO target: **15 minutes or better**;
- automated encrypted daily snapshot;
- PITR window: **at least 7 days**;
- daily backup retention engineering default: **30 days**, subject to external compliance/policy validation before production;
- engineering RTO target: **4 hours or better** for the pilot;
- restore responsibility: designated technical system operator/engineering maintainer, not the CRM Owner application role;
- perform a successful restore drill before pilot release;
- maintain a written restore/disaster-recovery runbook.

No active-active multi-region architecture is required for V1.

These are engineering availability/recovery targets, not legal records-retention rules.

A future compliance decision may tighten them through the appropriate change path.

See ADR-005.

---

## 3.8 Printing and reporting foundation

V1 A4 output uses browser print/print-CSS or equivalent browser/system printing from authorized immutable/current source facts.

Do not store byte-identical PDFs by default because the current product explicitly permits historical facts to render through the current configured A4 template.

If later compliance requires exact rendered-document retention, treat that as new technical/compliance scope.

Reporting uses PostgreSQL/read-model queries in V1.

Do not introduce a separate data warehouse, Elasticsearch/OpenSearch or BI pipeline until measured/reporting needs justify it.

Exact chart library/export format remains deferred.

---

## 3.9 Logging, health and error handling

Use structured JSON application logs with:

- timestamp;
- severity;
- request/correlation ID;
- service/module;
- machine-readable error code where relevant;
- safe operational context only.

Do not log request/response bodies by default.

Do not log secrets or unrestricted clinical content.

Expose application health/readiness checks suitable for deployment monitoring.

Keep business audit separate from operational logs.

Vendor-specific observability/telemetry backends remain deploy-time choices.

---

## 3.10 Testing and CI foundation

Use:

- **Vitest** for TypeScript unit tests and service-level integration tests;
- **Playwright** for browser end-to-end tests;
- real PostgreSQL for database/integration tests; do not substitute SQLite for transaction/concurrency behavior;
- OpenAPI/contract validation for API compatibility.

GitHub Actions is the CI baseline.

Once implementation exists, PR CI must at minimum run:

1. dependency install from the committed lockfile;
2. formatting/lint check;
3. TypeScript typecheck;
4. unit tests;
5. PostgreSQL integration/migration checks;
6. build;
7. relevant E2E/contract checks as the corresponding capability appears.

Use ESLint + Prettier as the initial lint/format baseline unless an ADR later replaces them.

See ADR-006.

---

# 4. Initial module boundaries

The backend remains one deployable modular monolith.

Expected module boundaries, without predefining their internal schemas/endpoints:

- authentication/access;
- patient identity;
- visits and consultation payment;
- queue;
- consultation/clinical record;
- prescription;
- pharmacy/dispensing;
- pharmacy billing/payment;
- inventory/transfers;
- approvals/exceptions;
- administration/configuration;
- reporting;
- audit;
- printing/output.

Cross-module calls stay in-process behind explicit service/application boundaries.

Do not create network service boundaries merely to mirror this list.

---

# 5. Explicit non-decisions / deferred items

This bootstrap intentionally does **not** decide:

- every database table/field/index;
- full domain entity model;
- exact API endpoint list/DTOs;
- frontend component library/design system;
- every routing/state-management library;
- exact hosting vendor/region;
- legal/privacy/healthcare compliance regime;
- legal data/audit-retention duration;
- final performance SLA/latency targets;
- exact analytics/chart/export implementation;
- exact printer/PDF library;
- catastrophic Owner identity-verification procedure beyond the existing no-bypass security boundary;
- historical import/migration, because no source dataset exists;
- third-party integrations.

Those decisions belong to external validation or the Change Package that actually needs them.

---

# 6. Bootstrap acceptance gates

The bootstrap is ready only when:

- IB-001 remains the product baseline;
- no BRD/PRD business behavior changed;
- all six bootstrap ADRs are present;
- every ADR states context, constraints, alternatives, decision, tradeoffs, affected future work and supersession rule;
- implementation repository instructions can be derived from these decisions without duplicating the full PRD;
- the code repository can be initialized without an agent inventing stack/topology/auth/database/testing foundations.

---

# 7. Next exact action after bootstrap merge

After this bootstrap is validated and merged:

1. initialize `surya-gorla/health-crm` with the selected monorepo/toolchain foundation;
2. add root `AGENTS.md` derived from Guidelines + bootstrap;
3. add path-specific instructions only where real divergence exists;
4. select and record Change Package `CP-001` based on dependency analysis;
5. implement CP-001 through the Change Package lifecycle and five closure gates.
