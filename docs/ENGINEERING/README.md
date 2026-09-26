# Hospital CRM — Engineering Documentation

## Purpose

This directory is the canonical home for cross-project engineering truth that derives from the locked BRD and current accepted PRD baseline.

Engineering documentation must not silently redefine product behavior.

The initial product source baseline is:

**[IB-001 — V1 Implementation Baseline](00-implementation-baseline-IB-001.md)**

Immutable product-source commit:

`fa9bbff607b257752f855021211a2d2d183d6dd1`

---

## Current engineering stage

**IMPLEMENTATION BASELINE FROZEN → MINIMUM ENGINEERING BOOTSTRAP NEXT**

No detailed architecture, domain/data/API/auth/UI contract, or Change Package should be treated as established merely because it has been discussed in chat.

Only committed canonical engineering records become shared engineering truth.

---

## Planned canonical areas

Create these only as real bootstrap/Change Package work requires them:

- architecture;
- domain/state contracts;
- data contracts;
- API/command contracts;
- authentication/authorization;
- audit/concurrency/idempotency;
- UI engineering/design system;
- testing;
- ADRs;
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
