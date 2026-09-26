# Hospital CRM — Living Project Guidelines

## Document control

| Field | Value |
| --- | --- |
| Document | Living Project Guidelines |
| Status | ACTIVE — LIVING |
| Established | 2026-09-26 |
| Scope | Product continuity, BRD/PRD discipline, engineering, implementation, validation, release continuity |
| Product authority | No — the locked BRD and validated PRD remain product authority |
| Process authority | Yes — this is the current operating baseline for humans and AI agents until explicitly revised |
| Historical source | docs/AGENT-CONTINUITY-HANDOFF.md plus the validated BRD/PRD process |
| Revision model | Evidence-driven, explicit, incremental; old guidance is not silently rewritten |

---

# 0. Purpose

This file is the current living operating constitution for the Hospital CRM project.

It exists because the project is expected to use AI agents heavily and because chat memory, agent memory, and individual implementation sessions are not reliable sources of continuity.

The project therefore keeps its working method in the repository.

This file preserves the useful rules developed during the BRD and PRD phases, records how those rules changed when the project moved toward engineering and implementation, and defines how the guidelines themselves may continue to change.

There is deliberately no claim that these guidelines are ultimate or permanently correct.

The rule is:

> Pick the strongest current working model, use it consistently, observe where it succeeds or fails, and revise it explicitly when evidence shows a better rule is needed.

A guideline should not be changed merely because a new agent prefers a different style. It should be changed because project evidence, implementation reality, repeated friction, a discovered safety gap, or a better validated process justifies the change.

Guideline evolution must be visible. Do not silently rewrite history.

---

# 1. Authority model

Different kinds of truth have different authorities.

## 1.1 Product truth

For business and product behavior, use this order:

1. Locked BRD.
2. Validated PRD and its accepted companion specifications.
3. Accepted product decisions and traceability derived from those sources.
4. Engineering contracts that implement the product.
5. Code/tests that implement those contracts.

If implementation or an engineering contract conflicts with the locked BRD/validated PRD, implementation does not win automatically. Treat the mismatch as a defect or initiate explicit product change control.

This Guidelines file must never silently override product truth.

## 1.2 Repository-state truth

For questions such as current branch, merged state, current file content, current code, active PR, or latest commit:

1. Live repository state wins.
2. Current committed documents come next.
3. Historical handoffs/ledgers/chats provide provenance but may be stale.

Preserved Agent Two principle:

> Start from live truth.

Preserved live-state principle:

> Audit the live repository state, not merely the retirement manual or last chat state.

## 1.3 Continuity/provenance

The large continuity handoff remains valuable for:

- historical reasoning;
- exact prior user directives;
- Agent One and Agent Two behavior;
- why earlier product decisions were made;
- previous validation chronology.

It is provenance and behavioral history, not authority to contradict the current BRD/PRD or current live repository state.

---

# 2. Working relationship with the user

The user is a product partner, not a client who needs repeated reassurance or approval for every ordinary decision.

The established working behavior is:

- be direct;
- be autonomous once sufficient evidence exists;
- preserve continuity;
- reason deeply before acting;
- be skeptical of weak assumptions;
- detect contradictions early;
- think through obscure edge cases;
- recommend/decide rather than option-dump;
- distinguish business-policy questions from product/technical questions;
- be willing to revise an earlier derived decision when later evidence proves it incomplete;
- preserve the structure and history of the project;
- keep user updates meaningful rather than noisy.

Avoid:

- asking the user to repeat information already available;
- repeated clarification when a safe answer can be derived;
- generic product-management advice;
- shallow happy-path reasoning;
- asking approval for ordinary derived decisions;
- silently changing accepted behavior;
- leaving engineers/agents to invent missing product policy;
- stopping after every minor operation;
- treating one file edit as completion;
- losing context between sessions;
- creating process ceremony that has no safety or continuity value.

Preserved Agent One relationship rule:

> Treat the user as a demanding product partner, not a client who needs constant reassurance.

Preferred tone:

> Calm, highly competent, precise, collaborative, slightly obsessive about consistency, but not verbose merely for appearance.

---

# 3. Decision authority

For a product decision, use this order:

1. locked BRD;
2. accepted PRD behavior;
3. established user reasoning preferences;
4. established product principles;
5. compatibility constraints from completed work;
6. inherited reminders/dependencies where applicable;
7. careful independent reasoning.

Then make the decision.

Ask the user only when one of the following is genuinely true:

- a new clinic/business-policy decision is missing;
- two governing product sources genuinely conflict and no safe reconciliation can be derived;
- an external legal/compliance/medical authority is required;
- the user explicitly reserved the decision for the clinic/themselves.

Do not ask merely to approve:

- loading/empty/error behavior;
- stale-state blocking;
- confirmation behavior;
- safe retry handling;
- history presentation;
- non-destructive correction behavior;
- technical architecture choices that do not change clinic policy;
- implementation structure;
- ordinary UX/product details derivable from accepted sources.

If a decision is technical but high-impact, document it in the engineering decision record rather than pushing it to the user by default.

---

# 4. Universal reasoning method

The project retains the reasoning method developed during PRD refinement.

For every meaningful decision:

1. Read the governing source.
2. Identify the real ambiguity, risk, or missing detail.
3. Classify it: business policy, product design, engineering contract, implementation detail, compliance/external dependency, or future scope.
4. Reuse previous accepted behavior instead of solving the same concept independently again.
5. Ask what breaks if the detail remains unspecified.
6. Choose the smallest clean rule that makes behavior safe and deterministic.
7. Test it adversarially.
8. Propagate it through every affected layer.
9. Validate the actual committed result.
10. Check backward compatibility.
11. Check downstream/forward impact.
12. Persist the resulting truth and the next action.

Preserved Agent One question:

> How can this decision break another workflow?

For meaningful behavior, consider at least:

- happy path;
- stale state;
- concurrent actors;
- role boundaries;
- multi-role accounts;
- multiple doctors/pharmacy units where relevant;
- cancellation/void/correction;
- retry;
- timeout/unknown outcome;
- partial failure;
- historical records;
- current versus historical truth;
- authority changes while a page is open;
- auditability;
- physical-world mismatch;
- downstream work.

A decision is not complete merely because the current screen or current function works. It must survive the product around it.

---

# 5. Product-design principles inherited from BRD/PRD work

These remain default invariants unless governing product sources explicitly require otherwise.

## 5.1 Explicit over magical

Major state changes should be explicit.

Examples already established in the product include explicit Mark Paid, Start Consultation, Complete Consultation, Finalize Prescription, Owner decision, queue entry, replacement, and confirmation for high-impact actions.

Do not infer a major business transition from an unrelated field selection or convenient UI event.

## 5.2 Non-destructive over destructive

Historical business truth should remain visible.

Prefer:

- correction;
- revision;
- amendment;
- superseding;
- cancellation/void with history;
- archive/disable rather than deletion;
- immutable movement/event history.

Historical truth must not disappear because current truth changed.

## 5.3 Current effective state must be distinguishable from history

Never make a historical record appear currently actionable.

Keep current state and prior state clearly separable across payments, prescriptions, clinical revisions, assignments, approvals, staff, inventory, and other stateful workflows.

## 5.4 Pending is not applied

A request being Pending does not normally mutate the underlying business record.

The actual business state changes only when the correct authority performs the effective action.

## 5.5 Direct authority is not fake approval

Do not force an authorized Owner to create a request and then approve their own request merely to reuse an approval UI.

Direct authority actions may be auditable without pretending to be self-approval.

## 5.6 Current authority beats loaded-page authority

An open tab does not preserve permission forever.

Protected final actions must re-evaluate current account/role/workspace/target authority.

UI hiding alone is not authorization enforcement.

## 5.7 Stale state fails safely

Stale intent must not silently overwrite current truth.

Determine which baseline facts the action depended on.

Block old intent when it could:

- overwrite newer truth;
- exceed current quantity/stock;
- act on a superseded version;
- use revoked authority;
- duplicate an already-completed effect.

Refresh current truth, explain the conflict, and preserve unsaved input where reasonably possible.

## 5.8 Unknown outcome is not the same as known failure

- confirmed no-effect failure may be retried;
- unknown outcome must re-read current truth/effects first;
- if the intended effect already exists, recover/show it instead of duplicating it.

Do not intentionally create duplicates for high-impact operations.

## 5.9 Sequenced effects and atomic effects are different

If one step in a legitimate sequence succeeds and a later step fails, preserve the valid earlier effect and resume appropriately.

If product truth requires one effective atomic operation, partial visible final state is unacceptable.

Examples already established:

- dispense + matching stock deduction;
- linked pharmacy transfer source decrement + destination increment;
- prescription replacement old Finalized to Superseded + new version to Finalized.

## 5.10 Audit is evidence, not a privilege bypass

Audit/history should make material actions attributable:

- who;
- what;
- effective role/workspace;
- target record;
- when;
- why where applicable;
- prior/captured state where applicable;
- resulting state where applicable.

Audit must not expose secrets or unauthorized protected content merely because it is an audit screen.

## 5.11 Smallest safe rule

Do not optimize for the number of rules or for ceremonial completeness.

Prefer the smallest set of explicit rules that makes behavior deterministic, safe, auditable, and implementation-ready.

Do not overengineer V1.

---

# 6. Repository and continuity discipline

Before changing the repository:

1. Read the root README.
2. Read this Guidelines file.
3. Read docs/README.md for current documentation navigation/authority.
4. Read the relevant BRD/PRD sources.
5. Read the continuity handoff only as deeply as needed for historical context/provenance.
6. Restore live GitHub state before asserting what is current.

Always:

- prefer repository truth over chat memory;
- never ask the user to repeat a decision already recorded;
- preserve exact evidence labels when distinguishing verbatim history from reconstruction;
- never claim exact/verbatim evidence when only a reconstruction is available;
- keep logical changes together;
- avoid unrelated edits in the same change;
- validate the committed result, not only local intent;
- inspect actual source before fixing a validator-reported issue;
- distinguish product defects from validator defects;
- distinguish stale bookkeeping from substantive defects;
- reconcile real defects transparently;
- update persistent state when work crosses sessions;
- leave the next action discoverable.

Do not create a new branch or PR for every trivial operation. Use branch/PR boundaries where they provide meaningful isolation, review, rollback, or merge safety.

Do not merge merely because files appear complete.

Where supported, use exact-head merge guards for important merges.

After merge, validate the merged/live state.

---

# 7. BRD/PRD-era workflow retained as precedent

The implementation phase does not continue G1–G15 refinement, but the method that made that work successful remains precedent.

## 7.1 PRD group lifecycle used previously

The established lifecycle was:

1. NOT STARTED
2. PREPARING
3. SOURCE REVIEW
4. GROUP REASONING
5. DECISIONS RESOLVED
6. CHANGES COMMITTED
7. COMMIT VALIDATED
8. BACKWARD COMPATIBILITY CHECK
9. RECONCILIATION if required
10. FORWARD IMPACT ANALYSIS
11. FUTURE REMINDERS RECORDED
12. FINAL GROUP VALIDATION
13. COMPLETE

## 7.2 Previous four closure gates

A PRD group was complete only when:

- Gate A — current-group validation passed;
- Gate B — backward compatibility passed;
- Gate C — forward impact/reminders were resolved/recorded;
- Gate D — ledger/checkpoint state matched reality.

This principle remains: completion is multi-dimensional, not merely “the edit exists.”

## 7.3 Previous group-review standard

A deep product review considered:

1. what the group is;
2. why/problem;
3. users;
4. exact requirement IDs;
5. complete journey;
6. screens;
7. fields/displayed information;
8. actions/interactions;
9. states;
10. permissions;
11. edge cases;
12. audit/history;
13. acceptance tests;
14. BRD lineage;
15. gaps an engineer/designer would otherwise guess;
16. verdict on completeness and implementation readiness.

## 7.4 Requirements grouping discipline

The validated G1–G15 partition remains part of PRD provenance. Do not casually renumber/reassign completed PRD requirements or redo completed groups unless real evidence shows a defect or approved product change requires it.

## 7.5 What must not be repeated from the old phase

Do not:

- casually reopen the locked BRD;
- change business policy through UX or technical language;
- redo completed G1–G15 merely because a new agent wants a different organization;
- renumber established requirements without an exceptional migration reason;
- treat one screen as the whole feature;
- stop after changing only one PRD layer when behavior affects multiple layers;
- skip acceptance coverage;
- overwrite historical product truth;
- silently broaden role permissions;
- invent refunds/returns/priority/merge/offline/general-retail behavior;
- convert technical mechanisms into new business rules.

---

# 8. What changed for the engineering/implementation phase

The project is no longer primarily refining product definition. The validated product must now be translated into executable software.

The earlier principles are retained, but the operating unit and closure model change.

| Area | BRD/PRD phase | Engineering/implementation phase | Why |
| --- | --- | --- | --- |
| Primary work unit | Semantic PRD group G1–G15 | Change Package | Engineering work may span DB, backend, frontend, infra, integration, or shared foundations |
| Main truth being refined | Product behavior | Engineering truth + executable implementation | Product behavior is already validated |
| Contract strategy | Product contracts largely completed before implementation | Engineering contracts grow incrementally as needed | Real implementation reveals technical facts that are wasteful or impossible to predict fully upfront |
| Backend/frontend | Not applicable as separate implementation layers | May be sequential subtasks of one parent capability | A locally correct backend does not prove the user-facing capability is complete |
| Verification | Document/traceability validation | Continuous automated + integration + acceptance validation | Verification cannot be a final phase after development |
| Compatibility | Backward check against previous groups | Backward/cumulative check against prior packages and current engineering contracts | Every package inherits existing implementation truth |
| Forward impact | Reminder register for later groups | Forward-impact record/backlog/contract implications | Later packages need discoverable obligations |
| Completion gates | Four PRD gates | Five Change Package gates | Implementation adds executable verification and continuity concerns |
| Persistent state | PRD ledger/reminders | Change Package records, engineering contracts, ADRs, tests, repository state | Future AI agents must continue from the repository rather than chats |

What did not change:

- source discipline;
- autonomous derivation where safe;
- smallest-safe-rule thinking;
- adversarial failure analysis;
- non-destructive history;
- explicit authority;
- backward compatibility;
- downstream impact;
- committed-state validation;
- continuity;
- transparency when reconciling earlier decisions.

---

# 9. Current engineering operating model

The governing high-level flow is:

LOCKED BUSINESS TRUTH
→ VALIDATED PRODUCT TRUTH
→ PRODUCT BASELINE CLOSURE
→ MINIMUM ENGINEERING BOOTSTRAP
→ REPEATING CHANGE PACKAGE LOOP
→ CUMULATIVE RELEASE VALIDATION
→ PILOT / RELEASE
→ NEW CHANGE PACKAGES / V1.x

This replaces the earlier overly linear model that implied all architecture, data, APIs, UI contracts, and implementation planning should be fully completed before development.

Architecture and contracts still matter. The change is that only high-blast-radius foundation decisions are established upfront. Detailed engineering truth is then extended as real Change Packages reach it.

---

# 10. Three layers of truth during implementation

## 10.1 Product Truth

Includes:

- locked BRD;
- validated PRD;
- acceptance criteria;
- screen contracts;
- interaction/product behavior contracts.

Answers:

> What must the product do?

Engineering cannot silently change this layer.

## 10.2 Engineering Truth

Includes, as they are established:

- system architecture;
- domain model;
- technical state model;
- data model/contracts;
- API/command contracts;
- authorization implementation contracts;
- audit/concurrency/idempotency strategy;
- UI engineering/design-system contracts;
- integration contracts;
- ADRs;
- test strategy and engineering conventions.

Answers:

> How have we decided to implement the required product behavior?

This layer is living and controlled.

## 10.3 Executable Truth

Includes:

- code;
- database migrations;
- automated tests;
- infrastructure/configuration;
- built application behavior.

Answers:

> What actually exists right now?

Code is important evidence but does not silently redefine product requirements.

When these layers disagree, identify the defect/class of change rather than normalizing the mismatch.

---

# 11. Product baseline closure

Before production implementation begins, freeze a precise product baseline.

This is the immediate next product-process step.

The closure must:

- reconcile stale document-control metadata that still says DRAFT/refinement-in-progress where the ledger/live repository shows completed validated refinement;
- ensure README/index status wording matches current truth;
- preserve BRD/PRD substance unless a real defect is found;
- record exact source versions and exact Git commit;
- create a clear implementation-baseline marker/tag or equivalent immutable reference;
- confirm the validated counts/coverage remain intact.

Do not perform another ceremonial G1–G15 re-review unless new evidence reveals a real defect.

The purpose is not to reopen product design. It is to remove stale process metadata and establish an unambiguous starting point for engineering.

---

# 12. Minimum engineering bootstrap

Do not create a giant speculative technical specification before coding.

Establish only cross-cutting decisions with large blast radius that multiple agents must share before they can safely work independently.

Expected bootstrap topics include:

- repository/project structure;
- frontend/backend technology choices;
- application topology;
- database technology;
- migration mechanism;
- API style;
- authentication/session architecture;
- authorization enforcement boundaries;
- audit foundation;
- transaction/idempotency/concurrency principles;
- configuration/secrets/environment strategy;
- test frameworks;
- lint/format/static analysis;
- CI validation;
- logging/error handling foundation;
- development/run commands;
- AI-agent repository instructions.

Do not predefine every endpoint, table, DTO, UI component, error code, or technical field before there is a real Change Package that needs it.

---

# 13. Change Package

A Change Package is the parent unit of engineering work.

It is the smallest coherent piece of work that can move from current truth to validated repository truth while preserving product traceability and system compatibility.

A Change Package may represent:

- a user capability;
- a backend capability;
- a frontend capability;
- a full vertical slice;
- database/infrastructure foundation;
- shared authentication/authorization;
- audit infrastructure;
- shared UI/design-system work;
- a migration;
- an integration;
- a technical correction/refactor with meaningful cross-system impact.

## 13.1 Parent completion versus layer completion

Backend, frontend, database, infrastructure, and design-system tasks may be sequential subtasks.

Completing a backend subtask does not automatically mean the parent user capability is complete.

If a Change Package represents a user/product capability, it remains open until all required layers integrate and the relevant product acceptance behavior passes.

This directly prevents AI agents from declaring success on locally correct pieces that do not form the intended product.

## 13.2 Change Package minimum record

Each package should record, where applicable:

- package ID/name;
- objective;
- relevant BRD/PRD/P/AC/screen/interaction sources;
- current lifecycle state;
- impact areas;
- existing contracts affected;
- contract delta;
- required DB/backend/frontend/infra/shared tasks;
- test/acceptance plan;
- ADRs/engineering decisions;
- current blockers;
- reconciliation discovered during implementation;
- commits/PRs;
- validation results;
- forward impacts/dependencies;
- final checkpoint.

## 13.3 Lifecycle

Use this default lifecycle:

1. NOT STARTED
2. PREPARING
3. SOURCE REVIEW
4. IMPACT MAPPED
5. CONTRACT DELTA READY
6. TASK PLAN READY
7. IMPLEMENTING
8. LAYER VALIDATION
9. INTEGRATING
10. ACCEPTANCE VALIDATION
11. RECONCILIATION if required
12. CUMULATIVE REGRESSION
13. FORWARD IMPACT RECORDED
14. INDEPENDENT REVIEW
15. COMPLETE

The exact labels may evolve if experience shows a simpler/better model, but do not remove a safety function merely to shorten the list.

---

# 14. Engineering contract evolution

Engineering knowledge is deliberately allowed to grow during implementation.

The rule is:

> New concept → append. Existing concept changed → reconcile/supersede. Never create parallel conflicting current truths.

Examples:

- adding a new Prescription contract when pharmacy implementation begins is normal expansion;
- discovering that an existing Patient Search response needs another field requires updating the canonical Patient Search contract, not creating a contradictory frontend-only definition.

There must be one canonical current truth for each shared concept.

History may explain how that truth changed.

## 14.1 What belongs in a shared engineering contract

Promote an implementation discovery into shared engineering truth when it affects:

- another developer/agent;
- another application layer;
- persisted data;
- API/integration behavior;
- authorization/security;
- concurrency/idempotency;
- user-visible behavior;
- deployment/runtime;
- testing/acceptance;
- future packages.

Do not document every local helper name or trivial refactor as architecture.

## 14.2 Three classes of discovered change

### Class A — Local implementation detail

Examples:

- helper/function organization;
- internal module split;
- local naming/refactor that changes no shared contract.

Handle locally, test, and continue.

### Class B — Engineering contract change

Examples:

- API needs another field;
- database needs stable lineage/version metadata;
- command needs an idempotency key;
- service boundary changes;
- authorization check belongs at another boundary.

Update canonical engineering truth, assess impact, update tests, and continue. Normally no user decision is needed if product behavior is unchanged.

### Class C — Product behavior change

Examples:

- changing who has authority;
- allowing a pharmacist to edit a Doctor prescription;
- adding refunds/returns/priority/merge behavior;
- changing payment semantics;
- changing a locked workflow because implementation would be easier.

Stop the technical shortcut.

This requires product change control against BRD/PRD and, when it is a genuine new clinic/business-policy choice, user authority.

---

# 15. Five Change Package closure gates

A package is COMPLETE only when all relevant gates pass.

## Gate A — Product traceability

- relevant product sources identified;
- implementation does not invent hidden business policy;
- behavior remains traceable to P/AC/screen/interaction requirements where applicable.

## Gate B — Engineering compatibility

- architecture/domain/data/API/auth/UI contracts remain coherent;
- no conflicting current engineering truth;
- previously completed packages remain compatible;
- necessary contract/ADR updates are committed.

## Gate C — Implementation verification

Relevant executable checks pass, such as:

- unit tests;
- integration tests;
- contract tests;
- migration tests;
- build/type/static checks;
- lint/format rules;
- security checks;
- CI.

## Gate D — Product acceptance

The integrated behavior satisfies relevant acceptance requirements, including meaningful adversarial cases such as:

- happy path;
- stale state;
- concurrent action;
- retry/unknown outcome;
- unauthorized/revoked authority;
- history preservation;
- correction/cancellation;
- partial failure.

## Gate E — Continuity

A completely new AI agent should be able to continue without relying on the previous chat.

Before closure:

- canonical engineering docs/contracts are current;
- ADRs are current;
- package record is current;
- tests are current;
- forward impact is recorded;
- branch/PR/merge state is clear;
- next work is discoverable.

Only then mark the parent package COMPLETE.

---

# 16. Automated verification is continuous

Automated verification is not a final phase after development.

The expected pattern is:

contract/source understanding
→ implement a bounded change
→ run local checks
→ integrate
→ run integration/contract checks
→ acceptance validation
→ cumulative regression
→ merge
→ validate merged/live state.

Tests and automated checks act as executable protection for established product and engineering invariants.

Do not accept “it compiles” or “the AI says done” as completion evidence.

---

# 17. AI-agent-specific rules

AI increases speed but also increases interpretation drift.

Therefore:

1. Repository truth beats agent/chat memory.
2. Every task must start from relevant current sources, not only a copied prompt.
3. A task should identify relevant P/AC/screen/interaction IDs when they exist.
4. Agents must not invent business policy.
5. Agents must not create a new shared contract when a canonical one already exists.
6. If a shared contract must change, update it explicitly and assess consumers.
7. High-impact operations must be considered for authorization, stale state, concurrency, retry, audit, and partial failure.
8. Implementation should include tests derived from acceptance behavior.
9. Important work should receive independent/fresh review rather than only self-review by the implementation agent.
10. Review the source requirements, not merely the implementing agent's explanation.
11. Do not trust a handoff's claimed status without checking live repository state.
12. Do not hide uncertainty or reconstruct exact evidence as if verbatim.
13. Leave durable state in the repository before ending a long-lived task.
14. Keep persistent agent instructions concise enough to be followed; point to canonical sources rather than duplicating the full PRD into every instruction file.

When the implementation repository structure exists, use root and path-specific agent instruction files where useful, but keep this Guidelines file as the cross-project operating baseline.

---

# 18. Engineering decision records

Record an ADR or equivalent durable decision when a technical choice is:

- cross-cutting;
- costly to reverse;
- likely to affect several future packages;
- important to security/data/integration/reliability;
- non-obvious enough that a future agent may reasonably choose differently.

A decision record should capture:

- context/problem;
- constraints/product sources;
- meaningful alternatives considered;
- chosen decision;
- consequences/tradeoffs;
- affected contracts/packages;
- status;
- superseding decision if later replaced.

Do not silently rewrite an old decision record to make history look cleaner.

If a newer decision replaces it, mark/supersede the old decision and link the new one.

Do not create ADRs for trivial local implementation choices.

---

# 19. Git and change-management behavior

Prefer logical, reviewable changes.

Do not:

- mix unrelated cleanup into a feature package;
- create a PR for every tiny edit;
- merge while known reconciliation work is still open;
- call a package complete before its gates pass;
- use code changes to silently redefine product behavior.

For substantial/risky changes:

- isolate work appropriately;
- keep commit purpose clear;
- validate the exact committed state;
- use independent review;
- use merge guards where available;
- validate the merged result.

If validation discovers a real defect after something was marked ready, reopen the work honestly rather than defending the prior status.

---

# 20. Cumulative release validation

Change Package validation does not replace final system validation.

Before clinic pilot/release, perform a cumulative release gate covering at least the relevant:

- complete end-to-end workflow;
- role/authority/security behavior;
- stale/concurrency/idempotency behavior;
- database migration and recovery/backup behavior;
- audit/history integrity;
- cross-package integration;
- performance/reliability expectations;
- required compliance/external validations;
- pilot/UAT readiness.

The exact release checklist may grow as engineering becomes concrete.

---

# 21. Guideline revision protocol

These guidelines are intentionally living.

No guideline is treated as sacred merely because it was written first.

However, consistency requires one current baseline at a time.

## 21.1 Valid triggers for guideline change

Consider revision when:

- the same rule repeatedly creates friction without meaningful safety value;
- agents repeatedly misunderstand or bypass a rule because it is ambiguous;
- two guidelines conflict;
- implementation reality exposes a missing stage/control;
- a safety/continuity gap is discovered;
- documentation overhead becomes disproportionate;
- a validator/process produces repeated false positives;
- a supposedly independent work unit cannot actually be completed independently;
- source/code/contract drift appears;
- a better proven working pattern emerges;
- the user explicitly changes the desired operating model.

Do not change the rules merely for novelty.

## 21.2 How to revise

For every meaningful guideline revision:

1. State what is not working.
2. Gather concrete evidence/examples.
3. Identify whether the problem is the rule, its wording, its application, or missing tooling.
4. Make the smallest useful change.
5. Check whether the change weakens an important safety/continuity property.
6. Update this canonical Guidelines file.
7. Record what changed and why in the revision history.
8. Update affected templates/agent instructions/process documents.
9. Use the new rule consistently.
10. Re-evaluate it after real use.

Do not maintain multiple competing current guideline files.

## 21.3 Product boundary

Changing these guidelines may change how work is performed.

It must not silently change what the clinic/product is required to do.

Any true product behavior change follows the BRD/PRD change-control boundary.

---

# 22. Current next actions

At the time this living guideline baseline was created, the next sequence is:

1. Product Baseline Closure
   - reconcile stale PRD document-control/status metadata against the already completed/validated refinement;
   - reconcile README/index wording where required;
   - verify no substantive product content is accidentally changed.

2. Freeze the implementation baseline
   - record exact BRD/PRD versions;
   - record exact Git commit/tag or equivalent immutable reference;
   - record validation state.

3. Establish Minimum Engineering Bootstrap
   - decide only the cross-cutting technical foundations needed before implementation;
   - create canonical locations for architecture, domain/data/API/auth/UI/testing/ADR/change-package truth.

4. Establish AI implementation instructions
   - derive concise root/path-specific agent instructions from this file once the implementation repository structure and technologies are known.

5. Select Change Package 001
   - perform source review;
   - map impact;
   - establish only the contracts it actually requires;
   - implement and validate using the Change Package lifecycle/gates.

Do not jump directly from PRD to uncontrolled screen/code generation.

---

# 23. Revision history

## v1.0 — 2026-09-26 — Living guideline baseline created

Inherited and consolidated the proven Agent One/Agent Two BRD/PRD-era operating rules, including:

- live-state/source-of-truth discipline;
- user decision-authority boundary;
- smallest-safe-rule reasoning;
- adversarial stale/concurrency/retry/audit thinking;
- explicit/non-destructive product principles;
- cross-document propagation;
- committed-state validation;
- backward compatibility;
- forward impact;
- persistent continuity;
- independent revalidation when confidence is insufficient;
- transparent reconciliation of defects;
- no unnecessary user approval for derivable decisions.

Engineering-phase changes introduced after independent workflow revalidation:

- replaced the overly linear “finish all architecture/contracts, then develop” model;
- introduced Product Baseline Closure;
- introduced Minimum Engineering Bootstrap;
- introduced the three-layer Product Truth / Engineering Truth / Executable Truth model;
- replaced PRD-group work units with controlled Change Packages for implementation;
- made backend/frontend/DB/infra tasks possible as sub-work while preventing premature parent-capability completion;
- changed engineering contracts to controlled living contracts;
- established “new concept append / existing concept reconcile / never conflicting current truths”;
- introduced Class A/B/C implementation discoveries;
- replaced the previous four PRD closure gates with five engineering Change Package gates;
- moved automated verification from a final phase into the continuous work loop;
- added ADR/engineering decision-history rules;
- added explicit AI-agent consistency rules;
- added cumulative release validation;
- added this guideline-revision protocol so the process itself can improve without losing continuity.

Future revisions must add a new revision-history entry explaining what changed, why, and what evidence motivated the change.

---

# 24. Preserved short-form operating reminders

When in doubt:

> Start from live truth.

> Do not ask the user to repeat recorded decisions.

> Separate business policy from derivable product/engineering decisions.

> Ask: “How can this decision break another workflow?”

> Prefer the smallest clean, explicit, auditable rule.

> Preserve history.

> Current authority beats loaded-page authority.

> Pending is not applied.

> Unknown outcome requires checking current truth before retry.

> New concept: append. Existing concept: reconcile. Never create conflicting current truths.

> Validate the committed/live result.

> A subtask being done does not mean the parent capability is done.

> Leave the repository so the next fresh agent can continue without depending on chat memory.

> If a guideline is not working, do not work around it silently. Improve the guideline explicitly.

> Reason carefully, decide, document, validate, and continue.
