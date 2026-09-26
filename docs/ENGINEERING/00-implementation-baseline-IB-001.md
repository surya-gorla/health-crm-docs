# Hospital CRM — Implementation Baseline IB-001

## Document control

| Field | Value |
| --- | --- |
| Baseline ID | IB-001 |
| Status | FROZEN — V1 implementation source baseline |
| Frozen on | 2026-09-26 |
| Continuity owner | Agent 3 |
| Product authority source | Locked BRD + current accepted PRD baseline |
| Immutable source reference | `fa9bbff607b257752f855021211a2d2d183d6dd1` |
| Source repository | `surya-gorla/health-crm-docs` |
| Target implementation repository | `surya-gorla/health-crm` |
| Post-baseline PRD changes at freeze | None — no EFFECTIVE `PRD-CHG` exists |
| Governing process | `Guidelines.md` v1.1 |

---

## Effective-state rule

The `FROZEN` status is current project truth only when this baseline record is present on the live default branch.

If this file is viewed on an unmerged review branch, it represents the proposed freeze record; the live project remains at the previously merged governance/product-baseline state until merge.

The immutable product-source reference itself remains the already-merged commit `fa9bbff607b257752f855021211a2d2d183d6dd1`.

---

# 1. Purpose

IB-001 is the exact product-definition snapshot from which V1 engineering bootstrap and implementation begin.

This file does **not** create new product behavior.

It records the already-accepted product truth, the exact immutable Git source used to freeze it, and the validation state associated with that source.

The immutable baseline is the exact Git commit:

`fa9bbff607b257752f855021211a2d2d183d6dd1`

That commit is sufficient as an immutable implementation-baseline marker under Guidelines Section 11.2. A branch name is not used as the immutable reference because branches may move.

Future engineering work may occur on later commits. Those later commits must continue to interpret product truth from:

1. IB-001;
2. plus any later **EFFECTIVE** `PRD-CHG-###` record in Document 11.

---

# 2. Locked business baseline

The V1 business baseline is:

- **BRD v1.0 — LOCKED**
- locked on 2026-09-20.

Canonical business documents at IB-001:

1. `docs/BRD/01-business-requirements-document.md`
   - Version: **1.0**
   - Status: **V1 Business Requirements — LOCKED**
2. `docs/BRD/02-workflows-and-state-model.md`
   - Status: **V1 Business Workflow — LOCKED**
3. `docs/BRD/03-open-decisions-and-edge-cases.md`
   - Status: **V1 Decision Register — LOCKED**
4. `docs/BRD/04-requirements-traceability.md`
   - Status: **V1 Traceability — LOCKED**
   - current locked baseline: **v1.0**

No engineering decision may silently change this business baseline.

A genuine business-policy change must use BRD/business change control and the required user/clinic authority.

---

# 3. Accepted product baseline

Canonical accepted product documents at IB-001:

| Document | Version | Baseline state |
| --- | ---: | --- |
| 05 — Product Requirements Document | 0.17 | ACCEPTED — V1 product baseline under controlled amendment |
| 06 — PRD Acceptance and Traceability | 0.18 | ACCEPTED — V1 acceptance/traceability baseline under controlled amendment |
| 07 — Information Architecture and Screen Specification | 0.16 | ACCEPTED — V1 screen/IA baseline under controlled amendment |
| 08 — Interaction and Form Behavior Specification | 0.17 | ACCEPTED — V1 interaction baseline under controlled amendment |
| 09 — PRD Refinement State Ledger | — | CLOSED HISTORICAL RECORD |
| 10 — PRD Future Review Reminders | — | CLOSED HISTORICAL REGISTER |
| 11 — PRD Change Control Ledger | — | ACTIVE |

At freeze time:

- G1–G15 refinement: **COMPLETE**
- open cross-group conflicts: **0**
- open refinement reminders: **0**
- active/effective post-baseline PRD amendments: **0**

The current accepted PRD remains authoritative until an **EFFECTIVE** PRD change supersedes part of it.

---

# 4. Validated product-definition counts

Validation associated with IB-001 confirms:

- Product requirements: **116**
  - `P-001` through `P-116`
  - continuous and unique
- End-to-end acceptance scenarios: **112**
  - `AC-001` through `AC-112`
- Role/authority acceptance scenarios: **6**
  - `RA-001` through `RA-006`
- Authentication/user-account acceptance scenarios: **13**
  - `AU-001` through `AU-013`
- UX/interaction acceptance scenarios: **183**
  - `UXA-001` through `UXA-183`
- Explicit acceptance coverage:
  - every `P-001` through `P-116` is represented
- Screen contracts: **49 unique**
- Interaction specification:
  - Sections **1 through 45** continuous
- Refinement reminders:
  - `REM-001` through `REM-074` preserved
  - **0 OPEN**

The post-merge validation of PR #4 also confirmed:

- no BRD file changed during the governance/product-lifecycle closure;
- Documents 05–08 retained their substantive product/acceptance/screen/interaction content;
- Documents 09–10 retained their historical refinement/reminder content;
- no accidental `PRD-CHG` was created.

Validation status for IB-001: **PASS**.

---

# 5. Governance state at freeze

The governing process state at IB-001 is:

- `Guidelines.md` — **v1.1**
- active continuity agent — **Agent 3**
- Agent 1 — **RETIRED**
- Agent 2 — **RETIRED**
- Agent 3 — **ACTIVE**
- Test/Evidence Runners — evidence producers only; they do not consume numbered continuity-agent identities
- PRD substantive change path — `docs/PRD/11-prd-change-control-ledger.md`

Documents 09–10 are historical provenance and must not be reused as active development ledgers.

---

# 6. Implementation repository boundary

The executable V1 application is to be built in:

`surya-gorla/health-crm`

Repository state observed at baseline freeze:

- repository exists;
- default branch: `main`;
- repository size reported as 0;
- no existing implementation was used to infer product behavior.

The documentation repository remains the canonical source for business/product/governance truth.

The implementation repository will contain executable truth and implementation-local engineering instructions/contracts as they are established.

Shared/cross-project engineering decisions that must remain durable across repositories should remain traceable back to the docs repository.

---

# 7. How future work references IB-001

Every engineering bootstrap decision, ADR, Change Package, implementation task, or Test Evidence Package should be able to identify the product baseline it was derived from.

Use:

`Product baseline: IB-001`

If a later PRD amendment becomes EFFECTIVE, future work should state both:

- `Product baseline: IB-001`
- `Effective PRD changes: PRD-CHG-###, ...`

Do not rewrite IB-001 to make later truth appear original.

IB-001 remains historical truth for the implementation start point.

---

# 8. Baseline change/replacement rule

IB-001 is frozen.

Later product changes do not mutate this file's historical meaning.

If a materially new implementation baseline is ever required:

1. preserve IB-001;
2. reconcile all effective PRD changes;
3. create a new baseline ID such as `IB-002`;
4. record the new immutable source commit;
5. validate it independently;
6. record why a new baseline was necessary.

Do not silently repoint IB-001 at a different commit.

---

# 9. Next exact action

With IB-001 frozen, the next phase is:

**Minimum Engineering Bootstrap**

Per Guidelines Section 12, establish only the cross-cutting technical decisions with large blast radius that must be shared before implementation agents can safely work independently.

Do not start uncontrolled screen/code generation before that bootstrap is established.
