# CP-001 — Executable Platform Foundation

## Change Package control

| Field | Value |
| --- | --- |
| Package | CP-001 |
| Name | Executable Platform Foundation |
| Type | Infrastructure / integration foundation |
| Continuity owner | Agent 3 |
| Product baseline | IB-001 |
| Effective PRD changes | None |
| Docs engineering baseline | `0204986fda63552bd08501f3fbd185e4d9f0e1c0` |
| Implementation baseline | `05c754da7df01653f6502503c52a93812c04966e` |
| Current lifecycle state | TASK PLAN READY |
| Product feature completion claimed | NONE |
| Current blockers | None before implementation; executable validation still required |

---

# 1. Objective

Turn the merged implementation-repository skeleton into a **runnable, testable platform foundation** that conforms to IB-001 and ADR-001 through ADR-006.

CP-001 exists to prove that the selected cross-cutting engineering choices work together before a clinic workflow is implemented.

CP-001 does **not** implement or claim completion of:

- authentication/login;
- Owner TOTP;
- workspace selection;
- patient identity/registration;
- visits/payment recording;
- queue;
- consultation/clinical record;
- prescription;
- pharmacy/dispensing;
- billing/payment workflow;
- inventory/transfers;
- approvals;
- staff administration/configuration;
- reporting;
- business audit history;
- product A4 output.

Those capabilities require later Change Packages with their own product traceability.

---

# 2. Why CP-001 is the first Change Package

The implementation repository currently has:

- the selected repository/workspace skeleton;
- persistent Agent instructions;
- Node/pnpm/TypeScript pins;
- no executable React application;
- no executable NestJS application;
- no dependency lockfile;
- no PostgreSQL/Prisma runtime integration;
- no CI;
- no executable verification harness.

Every later user/product capability depends on a stable build/runtime/test/database foundation.

Making authentication or a clinic workflow CP-001 would mix platform scaffolding with product behavior and make failures harder to classify.

Therefore the smallest coherent first parent package is the executable platform foundation.

---

# 3. Governing sources reviewed

## 3.1 Process / baseline

- `Guidelines.md`
  - Section 13 — Change Package
  - Section 15 — five closure gates
  - Section 16 — continuous automated verification
  - Section 19 — Git/change discipline
- `docs/ENGINEERING/00-implementation-baseline-IB-001.md`
- `docs/ENGINEERING/01-minimum-engineering-bootstrap.md`
- implementation repository root/API/Web `AGENTS.md`

## 3.2 Business/product technical dependencies

### BRD OD-024 — Client / Device / Hosting Model

Relevant constraints:

- web application;
- internet-dependent V1;
- no offline mode;
- desktop/laptop browser primary client;
- mainstream browsers;
- normal A4 printing;
- no thermal-printer dependency;
- hosting provider remains technical architecture.

### BRD OD-025 — Backup and Recovery

Relevant only as an architecture dependency already resolved at bootstrap level.

CP-001 does not deploy production backup/restore infrastructure; it must not make implementation choices that contradict ADR-005.

### PRD Section 29.2 — Technical dependencies

Relevant dependencies:

- database/API architecture;
- session implementation;
- hosting/cloud;
- backup/recovery;
- performance engineering;
- encryption/key management.

CP-001 implements only the platform portions required to run/test the application. It does not claim the later authentication/security product capability or final production hosting/compliance completion.

## 3.3 Engineering decisions

- ADR-001 — Modular Monolith and Same-Origin Application Topology
- ADR-002 — V1 Runtime, Framework and Database Stack
- ADR-003 — Server-Side Sessions and Backend-Authoritative Access Context
  - compatibility constraint only; CP-001 does not implement login/session business behavior
- ADR-004 — PostgreSQL Transactions, Idempotency and Append-Only Audit
  - database/transaction foundation only; no business transaction is introduced
- ADR-005 — Deployment, Backup and Disaster-Recovery Baseline
  - portability/configuration compatibility only
- ADR-006 — Verification, CI and Operational Logging Foundation

---

# 4. Product traceability classification

CP-001 is an **infrastructure package**.

No `P-###`, `AC-###`, `RA-###`, `AU-###`, `UXA-###` or screen contract is marked implemented by this package.

Reason:

- the package establishes executable infrastructure;
- it does not deliver a clinic user capability;
- falsely claiming product acceptance coverage here would weaken traceability.

Gate A is satisfied by demonstrating that:

1. the package is derived from the explicit technical dependencies and accepted ADRs;
2. no hidden clinic policy is introduced;
3. no product requirement is falsely marked complete.

---

# 5. Impact areas

CP-001 may change only platform-foundation areas such as:

- root package/dependency/lockfile configuration;
- `apps/api` platform bootstrap;
- `apps/web` platform bootstrap;
- `packages/contracts` platform-level shared contracts only;
- TypeScript/build/lint/format configuration;
- PostgreSQL/Prisma development and test setup;
- environment/config schema;
- health/readiness endpoints;
- OpenAPI generation;
- correlation/structured operational logging;
- same-origin dev/build integration;
- Vitest/Playwright/integration test harness;
- GitHub Actions CI;
- local developer run commands;
- platform documentation.

It must not add clinic-domain tables, workflows, statuses or permissions.

---

# 6. Existing contracts affected

CP-001 consumes, but should not need to change:

- IB-001;
- Guidelines v1.1;
- ADR-001 through ADR-006;
- implementation-repository root/API/Web `AGENTS.md`.

If implementation proves one of these cross-cutting decisions defective, CP-001 enters RECONCILIATION and the affected canonical engineering source is updated explicitly before completion.

No silent workaround is allowed.

---

# 7. Contract delta — Platform Foundation Contract

When CP-001 is complete, the implementation repository shall provide the following shared engineering capabilities.

## PF-001 — Reproducible dependency graph

- pnpm workspace dependencies are declared;
- `pnpm-lock.yaml` is committed;
- CI can install using `pnpm install --frozen-lockfile`;
- Node/pnpm pins match ADR-002.

## PF-002 — Executable API foundation

A NestJS 12 API process:

- boots under the selected ESM/TypeScript configuration;
- exposes versioned API namespace;
- exposes liveness and readiness endpoints;
- readiness can verify PostgreSQL connectivity;
- exposes/generated OpenAPI for the platform endpoints;
- emits structured operational logs with a correlation/request ID;
- contains no clinic-domain endpoints yet.

## PF-003 — Executable web foundation

A React 19.3 + Vite 8.1 web application:

- boots in development;
- builds for production;
- loads a neutral platform/bootstrap page;
- can reach the API through same-origin-compatible local development proxying;
- does not present any clinic workflow as implemented.

## PF-004 — PostgreSQL / Prisma 7 foundation

- local/test PostgreSQL 18 can be started reproducibly;
- Prisma 7 uses PostgreSQL with the required driver adapter;
- Prisma configuration/schema validate;
- Prisma Client can be generated;
- API readiness/integration tests prove database connectivity;
- migration directory/tooling is prepared without inventing clinic-domain models;
- no SQLite substitute is used for DB integration behavior.

## PF-005 — Configuration / secret boundary

- local/example configuration documents only non-secret placeholders;
- required environment values are validated on application startup where applicable;
- no real secret is committed;
- production/provider selection is not hard-coded into source.

## PF-006 — Verification foundation

Repository checks include:

- formatting/lint;
- TypeScript typecheck;
- unit/smoke tests;
- API integration against real PostgreSQL;
- build;
- Playwright browser smoke test;
- OpenAPI generation/contract smoke;
- CI workflow executing the relevant checks.

## PF-007 — Same-origin deployability

The repository has one documented deploy/build path that can serve the web application and API under one logical origin without introducing cross-origin authentication as a requirement.

Concrete cloud provider remains deferred.

---

# 8. Required implementation tasks

## T1 — Dependency and workspace realization

- install exact compatible dependencies for merged ADR versions;
- create and commit `pnpm-lock.yaml`;
- add deterministic root scripts;
- keep dependencies minimal to CP-001 needs.

## T2 — NestJS API bootstrap

- create Nest application entry/module;
- versioned API prefix;
- health/liveness and readiness capability;
- OpenAPI bootstrap;
- correlation/request-ID middleware/interceptor;
- structured safe operational logging;
- config loading/validation;
- no product-domain controller/module.

## T3 — React/Vite web bootstrap

- create Vite/React entry;
- neutral platform bootstrap view;
- same-origin-compatible dev API proxy;
- production build configuration;
- no product workspace/navigation/screens.

## T4 — PostgreSQL/Prisma foundation

- local PostgreSQL 18 developer/test service definition;
- Prisma 7 configuration;
- PostgreSQL driver adapter integration;
- schema/generator with no clinic-domain models;
- DB connectivity service suitable for readiness/integration testing;
- migration/generate/validate scripts;
- no destructive production reset command.

## T5 — Verification harness

- Vitest setup;
- API unit/smoke tests;
- real-PostgreSQL integration test;
- web smoke/unit test where useful;
- Playwright browser smoke;
- OpenAPI smoke/validation;
- build/typecheck/lint/format scripts.

## T6 — CI

GitHub Actions PR workflow:

- correct Node/pnpm pins;
- frozen lockfile install;
- PostgreSQL 18 service;
- Prisma validation/generation;
- typecheck;
- lint/format;
- tests/integration;
- builds;
- browser/OpenAPI smoke checks as configured.

## T7 — Developer/run documentation

Document:

- prerequisites;
- environment setup;
- local PostgreSQL;
- install;
- API/web dev commands;
- test commands;
- CI-equivalent validation;
- safe reset guidance for local/test only;
- confirmation that no clinic feature is implemented by CP-001.

---

# 9. Test / acceptance plan

## 9.1 Static/package checks

- package manifests parse;
- lockfile exists;
- frozen-lockfile install succeeds;
- TypeScript typecheck succeeds;
- lint succeeds;
- formatting check succeeds.

## 9.2 Database checks

Against a real PostgreSQL 18 instance:

- Prisma config/schema validation succeeds;
- client generation succeeds;
- API can connect;
- readiness reports ready when DB is reachable;
- readiness reports non-ready when DB dependency is unavailable;
- no clinic-domain table/model is introduced by CP-001.

## 9.3 API checks

- API boots;
- liveness returns successful machine-readable result;
- readiness returns successful result when dependencies are ready;
- API prefix/versioning is present;
- OpenAPI document can be generated;
- requests have correlation IDs;
- logs do not include request/response bodies by default.

## 9.4 Web checks

- dev/build succeeds;
- neutral bootstrap page renders;
- Playwright loads the page;
- browser can reach API through expected dev/same-origin route;
- no clinic workflow/control is represented as complete.

## 9.5 CI checks

- clean PR CI run passes from committed source;
- checks use committed lockfile;
- PostgreSQL-backed integration is included;
- no test relies on SQLite for DB semantics.

---

# 10. Adversarial / failure checks

Although CP-001 contains no product workflow, validate platform failure behavior relevant to future packages:

- API does not claim readiness if PostgreSQL is unavailable;
- application startup fails clearly when required configuration is invalid/missing;
- no secret/example credential is committed;
- log output does not dump request/response bodies by default;
- web failure to reach API is surfaced as platform/bootstrap failure, not fabricated product data;
- local/test reset tooling cannot be mistaken for a production migration command;
- no globally shared browser workspace state is introduced in the neutral web scaffold.

---

# 11. Required layers / repositories

## Documentation repository

- CP-001 record;
- engineering reconciliation only if implementation exposes an ADR/bootstrap defect;
- package validation/forward-impact checkpoint.

## Implementation repository

- DB/platform;
- backend;
- frontend;
- infra/local development;
- CI;
- tests;
- developer documentation.

No lower Test/Evidence Runner is required before implementation begins. Gate C / independent review may use CI plus a later Test Evidence run where it materially improves evidence.

---

# 12. ADR / engineering-decision status

Existing ADRs are sufficient to begin implementation.

No new ADR is currently required.

Raise a new/superseding ADR only if implementation forces a cross-cutting, costly-to-reverse choice not already settled.

---

# 13. Current blockers

None before implementation.

Known deferred dependencies that are **not CP-001 blockers**:

- final cloud/provider/region;
- external healthcare/privacy/compliance validation;
- legal/audit/data-retention policy;
- catastrophic Owner recovery procedure;
- performance SLA;
- product design system;
- clinic configuration values.

---

# 14. Reconciliation log

None at TASK PLAN READY.

Any implementation discovery that changes a canonical contract must be recorded here and in the canonical source it affects.

---

# 15. Commits / PRs

## Documentation

- CP planning branch: `engineering/cp-001-platform-foundation`
- planning PR: not yet created at initial record creation

## Implementation

- implementation baseline main: `05c754da7df01653f6502503c52a93812c04966e`
- implementation branch/PR: created only after this planning record is validated/merged

---

# 16. Validation results

## Source review

**PASS**

The package boundary is supported by the Guidelines, IB-001, BRD technical dependencies, PRD technical dependencies and ADR-001–ADR-006.

## Product-boundary review

**PASS**

No clinic behavior is being introduced or claimed complete.

## Task-plan review

Pending exact-head review/merge of this CP planning record.

---

# 17. Forward impacts / dependencies

Once CP-001 is COMPLETE:

- later packages may rely on executable web/API/PostgreSQL/CI/testing foundations;
- the next Change Package may begin product capability work only after dependency analysis;
- authentication/session/workspace product capability is a likely early dependency, but its package number/scope is not fixed by CP-001;
- CP-001 does not pre-authorize any product feature.

---

# 18. Closure gates

Current status before implementation:

- Gate A — Product traceability: **READY / planning-level PASS**
- Gate B — Engineering compatibility: **READY / planning-level PASS**
- Gate C — Implementation verification: **NOT STARTED**
- Gate D — Product acceptance: **N/A for user-feature completion; platform acceptance NOT STARTED**
- Gate E — Continuity: **IN PROGRESS**

CP-001 must not be marked COMPLETE until all relevant final gates pass.

---

# 19. Final checkpoint

Not yet available.

Current lifecycle state:

**TASK PLAN READY**

Next exact action:

1. validate/merge this CP-001 planning record;
2. create implementation branch `cp/001-platform-foundation` from live implementation `main`;
3. implement T1–T7;
4. advance lifecycle to IMPLEMENTING;
5. collect executable/CI evidence before any completion claim.
