# ADR-005 — Deployment, Backup and Disaster-Recovery Baseline

| Field | Value |
| --- | --- |
| Status | ACCEPTED WHEN MERGED TO `main` |
| Date | 2026-09-26 |
| Decision owner | Agent 3 |
| Product baseline | IB-001 |
| Superseded by | None |

## Context / problem

The BRD explicitly defers hosting and requires technical architecture to define backup frequency, recovery target, acceptable data loss, downtime tolerance, restore responsibility and disaster-recovery process.

Compliance/data-retention/data-residency rules are not yet externally validated.

## Constraints / product sources

- online-only V1;
- single-clinic pilot;
- no required third-party integration;
- hosting/provider deferred;
- backup/recovery targets must be defined technically;
- compliance/retention must not be invented as confirmed policy.

## Alternatives considered

1. self-hosted database/application on one clinic machine;
2. active-active multi-region deployment;
3. managed application service + managed PostgreSQL, provider chosen after compliance/data-residency review.

## Decision

Use a portable managed-service model:

- one HTTPS application origin;
- containerized application service;
- managed PostgreSQL;
- provider-managed TLS and encrypted storage/backups;
- separate local/test/staging/production environments and secrets.

Initial engineering recovery targets:

- point-in-time recovery capability;
- RPO: 15 minutes or better;
- encrypted daily snapshot;
- PITR window: at least 7 days;
- default daily-backup retention: 30 days, subject to compliance validation;
- RTO: 4 hours or better for pilot;
- designated technical system operator/engineering maintainer owns restore execution;
- successful restore drill before pilot;
- written DR/restore runbook.

Do not select a production provider/region until privacy/healthcare/data-residency requirements are externally validated.

No active-active multi-region topology is required for V1.

## Consequences / tradeoffs

Benefits:

- practical recovery without overbuilding the pilot;
- portability while compliance/provider choice is open;
- managed DB reduces operational risk.

Costs:

- final deployment decision is still blocked on external compliance/provider validation;
- recovery targets incur managed backup/PITR cost;
- restore drills become an operational responsibility.

## Affected future work

Deployment, secrets/key management, production readiness, release validation, incident handling.

## Supersession

Compliance/provider constraints or observed availability needs may supersede these engineering defaults through an explicit ADR/change.
