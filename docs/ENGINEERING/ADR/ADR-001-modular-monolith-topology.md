# ADR-001 — Modular Monolith and Same-Origin Application Topology

| Field | Value |
| --- | --- |
| Status | ACCEPTED — effective when present on `main` |
| Date | 2026-09-26 |
| Decision owner | Agent 3 |
| Product baseline | IB-001 |
| Superseded by | None |

## Context / problem

V1 is one clinic, online-only, browser-first and transaction-heavy. Several workflows require atomic behavior across domain concepts, including dispensing + stock deduction and linked transfers. Multiple roles and pharmacy units are supported, but there is no business requirement for independently deployed services.

## Constraints / product sources

- BRD OD-023/024: single-clinic web topology; hosting deferred.
- BRD/P-constraints: no offline mode; browser is primary client.
- P-107–P-112 safety behavior: stale/concurrent/unknown outcomes must be handled safely.
- inventory/prescription/payment flows require strong transactional consistency.

## Alternatives considered

1. Microservices from V1.
2. Separate independently deployed frontend/API with cross-origin auth.
3. Same-origin modular monolith.

## Decision

Use a **same-origin modular monolith**.

- React SPA and NestJS API are one logical application origin in production.
- Backend domain modules remain in one deployable API process.
- PostgreSQL is shared by the modular monolith.
- frontend and backend may remain separate workspace packages while production traffic stays same-origin.
- no message broker, Redis or distributed service mesh in the bootstrap.

## Consequences / tradeoffs

Benefits:

- simple transactional boundaries;
- simpler secure cookie/session/CSRF model;
- fewer partial/distributed failure modes;
- easier operations for a single-clinic pilot;
- faster cross-domain consistency checks.

Costs:

- module discipline must be enforced inside one codebase;
- scale is primarily vertical/process-level before service decomposition;
- a later service split requires explicit contracts and migration.

## Affected future work

Repository bootstrap, auth/session, API, DB transactions, deployment, testing, every Change Package.

## Supersession

Split a module into a network service only after measured scale/reliability/organizational evidence shows a real need and a superseding ADR defines data ownership and failure semantics.
