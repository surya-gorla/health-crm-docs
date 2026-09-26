# ADR-003 — Server-Side Sessions and Backend-Authoritative Access Context

| Field | Value |
| --- | --- |
| Status | ACCEPTED — effective when present on `main` |
| Date | 2026-09-26 |
| Decision owner | Agent 3 |
| Product baseline | IB-001 |
| Superseded by | None |

## Context / problem

The product requires local individual accounts, mandatory Owner TOTP, role revocation during open sessions, and legitimate different workspace contexts in separate tabs.

A global client JWT/workspace claim would make revocation and per-tab authority behavior harder to keep correct.

## Constraints / product sources

- one individual account per staff member;
- Owner-containing accounts require password + TOTP;
- newly granted Owner authority remains gated by Owner TOTP;
- account disable/role revoke must affect open sessions;
- separate tabs may use different permitted workspaces;
- workspace switching must not silently carry protected record context;
- backend authority beats loaded UI authority.

## Alternatives considered

1. browser-local bearer JWT with long-lived role claims;
2. external identity provider;
3. opaque server-side session + current database authority checks.

## Decision

Use an opaque server-side browser session persisted in PostgreSQL.

- session token in Secure/HttpOnly/same-site cookie;
- state-changing requests protected against CSRF;
- passwords hashed with Argon2id;
- Owner TOTP secrets encrypted at rest with a versioned deployment key;
- recovery codes stored only as one-way verifiers;
- backend checks current account/role/workspace/resource authority for protected actions;
- per-tab workspace context is client tab state and is sent explicitly with requests;
- no single global workspace is persisted into the account session;
- frontend visibility never substitutes for backend authorization;
- no Redis session store initially.

## Consequences / tradeoffs

Benefits:

- immediate/reliable authority revocation path;
- natural same-origin browser security;
- per-tab workspace behavior matches the PRD;
- simpler secret exposure boundary than browser bearer tokens.

Costs:

- database session lookup/write lifecycle must be implemented and cleaned up;
- CSRF protection is required;
- horizontal scale relies on shared PostgreSQL session state until another store is justified.

## Affected future work

Authentication, workspace shell, staff administration, every protected API, security tests, operational recovery.

## Supersession

Any future JWT/SSO/session-store change requires a superseding ADR proving equivalent current-authority, Owner-TOTP and per-tab-workspace guarantees.
