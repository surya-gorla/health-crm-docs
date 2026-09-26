# ADR-006 — Verification, CI and Operational Logging Foundation

| Field | Value |
| --- | --- |
| Status | ACCEPTED — effective when present on `main` |
| Date | 2026-09-26 |
| Decision owner | Agent 3 |
| Product baseline | IB-001 |
| Superseded by | None |

## Context / problem

AI agents will implement the product incrementally. Repository instructions alone are not sufficient to prevent drift; executable checks must protect established product and engineering invariants.

## Constraints / product sources

- every Change Package has implementation and acceptance gates;
- concurrency/idempotency behavior cannot be meaningfully validated with an unrelated in-memory database;
- logs must not expose authentication secrets or unrestricted clinical content;
- code must remain reviewable by future agents.

## Alternatives considered

1. manual testing only;
2. unit tests with mocked/in-memory persistence only;
3. layered automated tests using the real PostgreSQL engine plus browser E2E.

## Decision

Use:

- Vitest for TypeScript unit/service integration testing;
- Playwright for browser E2E;
- real PostgreSQL for DB/migration/concurrency integration tests;
- OpenAPI contract validation;
- GitHub Actions for CI;
- strict TypeScript;
- ESLint + Prettier initially;
- structured JSON operational logging with correlation IDs;
- deployment health/readiness endpoints.

PR CI grows with implementation and must eventually include:

- frozen dependency install;
- format/lint;
- typecheck;
- unit tests;
- PostgreSQL integration/migration tests;
- build;
- relevant contract/E2E checks.

Do not log request/response bodies by default.

Business audit and operational logging remain distinct.

## Consequences / tradeoffs

Benefits:

- catches cross-agent drift mechanically;
- tests database semantics the product actually depends on;
- gives deterministic evidence for Change Package closure.

Costs:

- integration/E2E CI is slower than unit-only checks;
- test data/environment management must be maintained;
- logging redaction rules require discipline.

## Affected future work

Repository initialization, every Change Package, testing agent evidence, deployment readiness.

## Supersession

Testing/lint/observability tools may be replaced by a superseding ADR when the replacement retains equivalent or stronger gates and evidence quality.
