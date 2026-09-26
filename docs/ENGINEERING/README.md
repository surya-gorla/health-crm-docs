# Hospital CRM — Engineering Documentation

## Purpose

This directory is the canonical home for cross-project engineering truth that derives from the locked BRD and current accepted PRD baseline.

Engineering documentation must not silently redefine product behavior.

The initial product source baseline is:

**[IB-001 — V1 Implementation Baseline](00-implementation-baseline-IB-001.md)**

Immutable product-source commit:

`fa9bbff607b257752f855021211a2d2d183d6dd1`

Executable implementation repository:

`surya-gorla/health-crm`

---

## Current engineering stage

**IB-001 FROZEN**

**Minimum Engineering Bootstrap — ESTABLISHED on `main`**

**Implementation repository foundation + AI repository instructions — ESTABLISHED on `surya-gorla/health-crm/main`**

Current Change Package:

**[CP-001 — Account Entry, Session & Workspace Authority Foundation](change-packages/CP-001-account-entry-session-workspace-authority.md)**

Effective-state rule:

- on an unmerged review branch, a new/updated Change Package is proposed engineering truth;
- after the CP record is merged to docs `main`, implementation work may begin on the implementation repository under that package;
- implementation code/contracts remain executable/engineering truth only when committed to the appropriate canonical repository.

No detailed domain/data/API/UI contract should be treated as established merely because it has been discussed in chat.

Only committed canonical engineering records become shared engineering truth.

---

## Bootstrap decision records

- [ADR-001 — Modular Monolith and Same-Origin Application Topology](ADR/ADR-001-modular-monolith-topology.md)
- [ADR-002 — V1 Runtime, Framework and Database Stack](ADR/ADR-002-platform-stack.md)
- [ADR-003 — Server-Side Sessions and Backend-Authoritative Access Context](ADR/ADR-003-auth-session-authority.md)
- [ADR-004 — PostgreSQL Transactions, Idempotency and Append-Only Audit](ADR/ADR-004-data-transactions-audit.md)
- [ADR-005 — Deployment, Backup and Disaster-Recovery Baseline](ADR/ADR-005-deployment-backup-recovery.md)
- [ADR-006 — Verification, CI and Operational Logging Foundation](ADR/ADR-006-verification-ci-logging.md)

## Active Change Packages

- [CP-001 — Account Entry, Session & Workspace Authority Foundation](change-packages/CP-001-account-entry-session-workspace-authority.md) — implementation package live on docs `main`; Task A contract establishment is the current work.

## Active canonical contracts

- [CP-001 Auth / Session / Workspace Authority Contract](contracts/CP-001-auth-session-workspace-authority-contract.md) — PROPOSED on this review branch; effective only after merge to docs `main`.

## Planned canonical areas

Create these only as real Change Package work requires them:

- domain/state contracts;
- data contracts;
- API/command contracts;
- authentication/authorization contracts;
- UI engineering/design system;
- testing evidence/contracts;
- Change Packages.

Avoid speculative documentation that has no immediate consumer.

---

## Authority order

1. locked BRD;
2. current accepted PRD baseline + EFFECTIVE PRD changes;
3. this engineering layer;
4. executable implementation.

If engineering truth conflicts with product truth, engineering does not win automatically.

Follow `Guidelines.md` and Document 11 change control.
