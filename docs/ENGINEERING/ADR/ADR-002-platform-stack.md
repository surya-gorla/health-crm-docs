# ADR-002 — V1 Runtime, Framework and Database Stack

| Field | Value |
| --- | --- |
| Status | ACCEPTED — effective when present on `main` |
| Date | 2026-09-26 |
| Decision owner | Agent 3 |
| Product baseline | IB-001 |
| Superseded by | None |

## Context / problem

The implementation needs one stable TypeScript stack that AI agents can share without repeatedly selecting frameworks or incompatible versions.

## Constraints / product sources

- desktop/laptop web browser primary client;
- online-only V1;
- substantial forms/tables/workflow state;
- transaction-sensitive relational data;
- no mobile app requirement;
- technical architecture may be chosen without altering business behavior.

## Alternatives considered

1. Next.js full-stack.
2. React/Vite + lightweight Node HTTP framework.
3. React/Vite + opinionated NestJS API.
4. non-TypeScript split stack.
5. Prisma 8 release candidate versus supported Prisma 7.

## Decision

Use:

- Node.js 24 LTS; initial pin 24.21.0;
- pnpm 12; initial pin 12.7.0;
- strict TypeScript;
- React 19.3;
- Vite 8.1;
- NestJS 12, ESM;
- PostgreSQL 18;
- Prisma ORM 7 stable line + Prisma Migrate.

Prisma raw SQL is allowed where database-native correctness requires it.

Do not start V1 on Prisma 8 release-candidate builds.

## External version snapshot verified 2026-09-26

Official project sources checked during bootstrap reported:

- Node.js 24 as an LTS release line; Node.js 26 is Current rather than LTS.
- React latest stable documentation at 19.3.
- Vite 8.1 as the current stable Vite 8 release line.
- NestJS current migration documentation for version 12.
- PostgreSQL current supported major 18 with current minor 18.6.
- pnpm 12 as the current release line, with 12.7 released 2026-09-25.
- Prisma ORM 8 as release-candidate software; Prisma ORM 7 remains supported and stable.

This snapshot explains the initial bootstrap selection. It is not a rule that versions can never move.

## Rationale

NestJS provides explicit module/guard/validation conventions that reduce agent-to-agent architectural drift.

React/Vite fits an internal SPA without SSR/SEO requirements.

PostgreSQL is the natural transactional system for longitudinal clinical, inventory, payment and audit records.

Prisma 7 is the supported stable line during bootstrap; Prisma 8 remains release-candidate software and currently documents missing capabilities relevant to this product's concurrency needs.

## Consequences / tradeoffs

Benefits:

- end-to-end TypeScript;
- mature web/backend ecosystem;
- strongly typed relational access/migrations;
- one lockfile/toolchain;
- predictable AI-agent conventions.

Costs:

- Prisma cannot be treated as the only source of database truth; some constraints/locks need SQL;
- future major upgrades require deliberate compatibility review;
- a monorepo still requires module ownership discipline.

## Affected future work

Implementation repository initialization, CI, all web/API/data Change Packages.

## Supersession

Major stack replacement requires a superseding ADR and migration plan. Patch/minor upgrades remain ordinary dependency maintenance when compatible and validated.
