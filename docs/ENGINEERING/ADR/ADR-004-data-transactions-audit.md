# ADR-004 — PostgreSQL Transactions, Idempotency and Append-Only Audit

| Field | Value |
| --- | --- |
| Status | ACCEPTED WHEN MERGED TO `main` |
| Date | 2026-09-26 |
| Decision owner | Agent 3 |
| Product baseline | IB-001 |
| Superseded by | None |

## Context / problem

The product contains money/status changes, clinical revision history, prescription versioning, cumulative dispensing limits, stock ledgers and linked transfers. It explicitly distinguishes known failures from unknown outcomes and forbids stale overwrites.

## Constraints / product sources

- dispense + stock deduction is one effective operation;
- linked transfer source/destination movement is atomic;
- prescription replacement is an atomic lineage transition;
- stale intent must not overwrite current truth;
- unknown outcome must be checked before retry;
- historical truth must remain attributable;
- audit must not expose authentication secrets/unrestricted clinical content.

## Alternatives considered

1. application checks only, minimal DB constraints;
2. event-sourced architecture;
3. relational transactions + targeted locks/versioning/idempotency + append-only audit.

## Decision

Use PostgreSQL as transactional authority.

- internal technical IDs use UUIDv7 where appropriate;
- business/human IDs remain separate;
- enforce invariants with DB constraints where feasible;
- use transactions for atomic business effects;
- use row locks for quantity/financial operations when current rows must serialize;
- use version/revision baselines for stale-sensitive edits;
- high-impact commands carry stable operation/idempotency identifiers;
- persist enough operation result/state to recover unknown outcomes safely;
- use Prisma 7 normally, with reviewed raw SQL when required for native constraints/locking;
- material audit rows are append-only and transactionally coupled to the business effect where required;
- business audit is not full event sourcing;
- operational logs remain separate.

## Consequences / tradeoffs

Benefits:

- database-enforced safety for the highest-risk workflows;
- safe retry/recovery model;
- historical attribution without converting the entire system to event sourcing.

Costs:

- some workflows require careful SQL/transaction design;
- generic CRUD generators cannot own high-impact mutations;
- integration testing must run against PostgreSQL.

## Affected future work

Patient/visit corrections, consultation, prescription, dispensing, billing, inventory, approvals, staff/configuration, audit, testing.

## Supersession

A different persistence/concurrency model requires a superseding ADR demonstrating equal or stronger atomicity, stale-state, history and retry guarantees.
