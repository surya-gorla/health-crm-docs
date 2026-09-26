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

The Minimum Engineering Bootstrap is documented in:

**[01 — Minimum Engineering Bootstrap](01-minimum-engineering-bootstrap.md)**

Effective-state rule:

- on an unmerged review branch, the bootstrap/ADRs are proposed engineering truth;
- when these records are present on `main`, the bootstrap is established and the next stage is implementation-repository initialization + AI repository instructions before CP-001.

No detailed domain/data/API/UI contract or Change Package should be treated as established merely because it has been discussed in chat.

Only committed canonical engineering records become shared engineering truth.

---

## Bootstrap decision records

- [ADR-001 — Modular Monolith and Same-Origin Application Topology](ADR/ADR-001-modular-monolith-topology.md)
- [ADR-002 — V1 Runtime, Framework and Database Stack](ADR/ADR-002-platform-stack.md)
- [ADR-003 — Server-Side Sessions and Backend-Authoritative Access Context](ADR/ADR-003-auth-session-authority.md)
- [ADR-004 — PostgreSQL Transactions, Idempotency and Append-Only Audit](ADR/ADR-004-data-transactions-audit.md)
- [ADR-005 — Deployment, Backup and Disaster-Recovery Baseline](ADR/ADR-005-deployment-backup-recovery.md)
- [ADR-006 — Verification, CI and Operational Logging Foundation](ADR/ADR-006-verification-ci-logging.md)

## Active Change Package

**[CP-001 — Executable Platform Foundation](CHANGE-PACKAGES/CP-001-executable-platform-foundation.md)**

Current planning state: **TASK PLAN READY**.

CP-001 is infrastructure-only. It does not claim implementation of any clinic product requirement or screen.

---

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
