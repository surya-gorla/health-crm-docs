# Hospital CRM — Agent Registry

## Purpose

This is the compact lineage register for the project's continuity-owning agents.

It is intentionally **not** an Agent Graveyard and it is not a duplicate of the full handoff/history.

The registry answers:

- which numbered continuity agent owned the project at a given time;
- whether that agent is Active or Retired;
- the agent's broad verified scope;
- where its retirement handoff/provenance lives;
- which future agent inherited continuity.

Detailed product truth remains in the BRD/PRD. Detailed engineering truth belongs in engineering contracts/ADRs/Change Packages. Git remains the file/commit history.

---

## Numbering rule

Only the **continuity-owning higher agent** receives the sequential project identity:

`Agent 1 -> Agent 2 -> Agent 3 -> Agent 4 -> ...`

A specialist or lower-cost agent used only for:

- test execution;
- screenshots;
- raw logs;
- browser/network/console evidence;
- artifact/ZIP collection;
- prescribed scenario execution;

does **not** consume the next Agent number.

Those runs are identified through Test Evidence IDs such as `TE-001`, `TE-002`, etc.

The continuity agent owns interpretation, reconciliation, product/engineering decisions within delegated authority, durable project state, readiness/merge reasoning, and handoff continuity.

A Test/Evidence Runner produces evidence and a provisional verdict; it does not independently change product truth.

---

## Current lineage

| Agent | Status | Active period | Verified/current scope | Retirement / continuity source |
| --- | --- | --- | --- | --- |
| Agent 1 | RETIRED | Project start -> retirement before Agent 2 | Original clinic discovery; BRD creation/refinement/lock; PRD creation and early refinement methodology/work | `docs/AGENT-CONTINUITY-HANDOFF.md` Appendix A/B. Exact per-change attribution is used only where source evidence supports it. |
| Agent 2 | RETIRED | Continuation -> 2026-09-26 | Restored live state; reconciled G13; completed G14/G15; final/global validation; second independent audit; acceptance reconciliation; PR #2 merge; documentation organization/continuity work | `docs/AGENT-CONTINUITY-HANDOFF.md` Appendix C and repository history. |
| Agent 3 | ACTIVE | 2026-09-26 -> current | Engineering-transition governance: Guidelines v1.1; PRD controlled-amendment model; agent/test-evidence lineage; product-baseline lifecycle closure; IB-001 implementation-baseline freeze | Retirement handoff will be created only when Agent 3 retires. |

Do not retroactively invent finer Agent 1/2 attribution where the surviving source does not prove it.

---

## Attribution rule from Agent 3 onward

Meaningful durable objects should identify the responsible continuity agent where useful.

Examples:

- PRD change record: `Reconciled by: Agent 3`
- ADR: `Decision owner: Agent 3`
- Change Package: `Continuity owner: Agent 3`
- Test Evidence: `Reviewed by: Agent 3`

Do not litter every sentence or line with agent names. Git already preserves file-level history.

Attribution belongs on decision/change objects where it improves provenance.

---

## Test/Evidence Runner boundary

A Test/Evidence Runner may:

- run prescribed test scenarios;
- collect screenshots/video/logs/network/console evidence;
- package raw artifacts;
- record reproduction steps;
- report PASS / FAIL / BLOCKED / PARTIAL;
- identify observations and suspected blockers.

A Test/Evidence Runner may not independently:

- redefine BRD/PRD behavior;
- approve a PRD amendment;
- change architecture because a test failed;
- decide that a validator/test failure is a product defect;
- close a Change Package on behalf of the continuity agent.

Its verdict is evidence, not project authority.

The continuity agent must classify the result against governing sources.

---

## Retirement rule

Before a numbered continuity agent retires:

1. restore/verify live repository state;
2. ensure active Change Packages/ADRs/PRD changes have current status;
3. ensure Guidelines contain any genuinely learned process changes;
4. record unresolved blockers and exact next action;
5. create that agent's retirement handoff;
6. update this registry to RETIRED;
7. identify the next numbered continuity agent when one actually takes over.

Do **not** update Guidelines merely because an agent retires. Guidelines change only when the operating method changes.

Do **not** create an empty future-agent handoff in advance.

---

## Evidence honesty

Where historical attribution is reconstructed, label it as reconstructed.

Where exact repository/handoff evidence proves attribution, it may be stated directly.

Never convert a broad historical inference into false per-change certainty.
