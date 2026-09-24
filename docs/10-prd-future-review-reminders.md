# Hospital CRM — PRD Future Review Reminders

## Purpose

This document stores forward-looking issues raised by already-reviewed groups that must be evaluated when their target groups are reviewed.

A reminder is not an already-decided future requirement. It is a mandatory review input. The target group must evaluate it together with its own BRD/PRD evidence and may resolve, refine, supersede, or convert it into a cross-group reconciliation decision.

---

## Reminder Status

- **OPEN** — target group has not yet evaluated the reminder.
- **IN REVIEW** — target group is actively evaluating it.
- **RESOLVED** — target group evaluated and disposition is recorded.
- **SUPERSEDED** — a later accepted rule replaced the reminder before target review.
- **NOT APPLICABLE** — target review established that no product change/check is required.

---

## Open Reminder Register

| ID | Raised By | Target Group | Reminder | Why It Matters | Status |
| --- | --- | --- | --- | --- | --- |
| REM-007 | G1 | G14 | Audit/history must preserve effective role/workspace for material multi-role actions, including same-human actions under different roles. | P-096 requires cross-product audit support. | OPEN |
| REM-008 | G1 | G14 | State-safety/concurrency review must include permission changes occurring while a protected workspace/tab is already open. | G1 requires safe handling of stale authority; G14 owns cross-product stale-state/safety behavior. | OPEN |
| REM-009 | G1 | G14 | Multi-tab/session behavior must not blur authority context across tabs, and state changes in one context must not silently make another tab authoritative for a different role. | G1 explicitly permits independent permitted workspace contexts per tab/window. | OPEN |
| REM-014 | G2 | G14 | Audit/history for authentication and recovery events may record safe metadata but must never expose passwords, reset credentials, TOTP secrets/codes, or recovery-code values. | G2 introduces explicit secret-handling boundaries that G14 must preserve in cross-product audit UX. | OPEN |
| REM-015 | G2 | G14 | Extend retry/stale-state review to authentication recovery: repeated Forgot Password must not create duplicate actionable requests, and a reset already resolved by another Owner session cannot be applied again as Pending. | P-111/P-112 currently emphasize other business records; G2 adds an auth-recovery state that needs the same safety discipline. | OPEN |
| REM-016 | G2 | G14 | Evaluate account disablement/role changes while multiple sessions or tabs are open. Preserve the product rule that stale authority/protected use stops when the state change is detected, while leaving exact propagation/session transport to technical design. | G1/G2 now define product behavior but not implementation mechanics. | OPEN |
| REM-017 | G2 | G14 | Include recovery-code lifecycle in state-safety review: used codes cannot authenticate again and regeneration invalidates the prior set. Do not expose code values in audit/state history. | Recovery codes are security-sensitive one-time state governed by G2. | OPEN |
| REM-022 | G3 | G13 | Returning-patient/unique-patient reporting must respect the permanent Patient IDs that actually exist. Possible Duplicate profiles remain separate identities in V1; reporting must not silently similarity-deduplicate them unless a later approved business definition explicitly says so. | V1 has no duplicate merge, so reporting logic can otherwise silently contradict the identity model. | OPEN |
| REM-023 | G3 | G14 | Extend P-111 retry/idempotency review to Patient creation: an unknown registration outcome must be resolved by checking effective state before another create attempt, including concurrent candidate changes discovered at final submit. | Patient identity duplication is high-impact and G3 now defines product-level retry behavior. | OPEN |
| REM-024 | G3 | G14 | Audit/history must cover Possible Duplicate provenance and demographic corrections without exposing unauthorized clinical content: candidate Patient IDs/actor/time for the marker; old/new/requester/Doctor decision/time for corrections; Doctor direct edits remain attributable. | G3 introduces auditable identity-risk and demographic-change metadata that G14 must present consistently. | OPEN |
| REM-025 | G3 | G14 | Stale-state protection must explicitly cover demographic correction: if the captured current value changed before Doctor decision, the old proposal cannot overwrite newer truth; exact concurrency/version mechanism stays technical. | G3 establishes the stale business behavior; G14 owns the cross-product stale-state contract. | OPEN |
| REM-032 | G4 | G13 | Consultation revenue/financial reporting should use the current effective recorded financial state after approved corrections, count Paid consultation amounts, exclude Waived, and avoid double-counting superseded/original payment records retained for audit. | G4 preserves original financial history while defining one corrected effective record; G13 owns reporting definitions/presentation. | OPEN |
| REM-033 | G4 | G14 | Cross-product retry/stale safety must cover Visit creation, Paid recording, combined Mark Paid + queue partial success, one Pending waiver/correction per relevant baseline, stale waiver after Paid, and stale payment-correction baseline. Exact idempotency/concurrency mechanisms remain technical. | G4 defines product-level safety outcomes that G14 must generalize. | OPEN |
| REM-039 | G5 | G13 | Average-wait reporting must respect queue history: ordinary reassignment/Unresponded/move-to-end preserve the queue-entry journey; actual removal for financial ineligibility plus later re-entry creates another queue-entry event. Define the deterministic reporting event without deleting earlier history. | OD-029 defines wait as queue entry -> With Doctor; G5 now allows multiple historical queue-entry events in one Visit. | OPEN |
| REM-040 | G5 | G14 | Cross-product stale/concurrency review must include races among Call, Start Consultation, Reassign, Unresponded, move-to-end, financial correction, cancellation request/decision, and Visit completion. One Pending cancellation per Visit and stale decisions/actions must be enforced. | G5 depends heavily on current state/assignment and concurrent actors. | OPEN |
| REM-043 | G6 | G13 | Patients-seen/day and related consultation counts should anchor to the original Consultation Completed event. Later clinical amendments must not create a second completed consultation or change the original completion event; evaluate how later Visit cancellation affects reporting without rewriting historical completion. | G6 separates completion from later amendment/cancellation history; G13 owns reporting. | OPEN |
| REM-044 | G6 | G14 | Generalize stale/audit safety for clinical drafts and amendments: concurrent draft saves must not overwrite newer content, stale amendment baseline must refresh, effective cancellation must reject stale clinical writes, and clinical revision audit must preserve current vs prior revisions without exposing content to unauthorized audit roles. | G6 defines product-level clinical concurrency/revision behavior; G14 owns cross-product safety/audit. | OPEN |
| REM-048 | G7 | G13 | Most-prescribed/prescription reporting must not double-count superseded replacement versions as independent new clinical intent. Define reporting around current/effective prescription lineage while preserving raw version history for audit. | G7 introduces version lineage with multiple finalized versions for one Visit. | OPEN |
| REM-049 | G7 | G14 | Cross-product stale/idempotency review must cover prescription draft/finalization/replacement: duplicate finalization must not create multiple current versions, stale replacement cannot supersede newer current version, effective cancellation blocks stale prescription writes, and current vs Superseded history remains explicit/auditable. | G7 defines version-state/concurrency behavior; G14 owns global safety/audit. | OPEN |
| REM-050 | G7 | G15 | Printing must use the selected finalized version's stored finalization-time availability snapshot. Current Finalized is the default print/reprint target; any historical Superseded/Cancelled-context copy must be unmistakably historical and must never look like the current dispensable prescription. | G7 fixes snapshot/reprint semantics but G15 owns physical-output presentation. | OPEN |
| REM-053 | G8 | G13 | Medicine-sales reporting must use actual supplied quantities/value, including the actual approved substitute medicine supplied where applicable, and exclude unsupplied remainder. Unit-level activity may consolidate without erasing pharmacy-unit attribution. | G8 distinguishes prescribed intent from actual fulfilment; G13 owns reporting. | OPEN |
| REM-054 | G8 | G14 | Cross-product safety must cover dispense idempotency/unknown outcome, item-lineage remaining allowance across replacement, multi-unit races, prescription replacement/cancellation while dispense is open, and stale substitution requests after version/quantity changes. | G8 depends on concurrent state across prescription, Visit, stock, unit, and substitution decisions. | OPEN |
| REM-055 | G8 | G15 | Pharmacy A4 dispensing/billing summary should faithfully show actual supplied quantity, unsupplied remainder, approved substitute supplied where relevant, and pharmacy-unit context without altering the original prescription version. | G8 defines fulfilment truth; G15 owns physical output. | OPEN |

| REM-060 | G9 | G13 | Pharmacy financial reporting must define deterministic treatment of current/effective payment state after corrections, multi-unit bills, and Paid bills that are later Voided without refund. Avoid double-counting historical/superseded payment records and preserve pharmacy-unit attribution; do not conflate active charge state with actual recorded external payment history. | G9 preserves non-destructive bill/payment history and permits Paid+Voided history, creating a reporting-definition dependency. | OPEN |
| REM-061 | G9 | G14 | Cross-product stale/idempotency review must include bill creation from unbilled dispensing, double-bill prevention, Mark Paid, payment-correction baseline races, bill-void decision concurrent with payment, Visit completion across multiple pharmacy units, post-Visit bill administration, and unknown-outcome retry safety. | G9 introduces concurrent state across dispensing, bill, payment, Visit completion, request/decision, and multiple units. | OPEN |
| REM-062 | G9 | G15 | Pharmacy A4 bill/dispensing output must use the frozen bill snapshot and unit attribution, show actual supplied quantities/payment context where applicable, distinguish active versus Cancelled/Voided historical copies, and never recalculate historical bills from current prices, prescription state, or stock. Evaluate together with REM-055 for unsupplied remainder/substitute presentation. | G9 fixes immutable bill snapshot semantics while G15 owns physical-output presentation. | OPEN |
| REM-065 | G10 | G13 | Inventory reporting must derive current stock from unit ledgers, distinguish valid available from expired/unavailable recorded quantity, preserve unit/batch attribution, treat transfers as linked internal movements rather than sales/additions, and avoid counting corrective movements as new dispensing. | G10 formalizes movement categories and valid-availability semantics that G13 must aggregate without distorting operational truth. | OPEN |
| REM-066 | G10 | G14 | Cross-product safety/audit must cover concurrent dispense/adjustment/transfer movements, stale stock baselines, no-reservation Pending requests, linked two-sided transfer atomicity, no-negative-stock guarantees, movement immutability, Owner direct adjustments, and unknown-outcome retry/idempotency. | G10 makes inventory correctness depend on current ledger state across several concurrent actors and unit ledgers. | OPEN |
| REM-068 | G11 | G13 | Waiver/cancellation/bill-void/payment-correction/inventory/transfer/password-reset reporting must distinguish Pending, effective Approved/Resolved/direct Owner actions, Rejected, and Stale/Non-actionable outcomes. Reports must not count rejected/stale requests as effective business changes or double-count direct Owner actions as both request and approval. | G11 defines type-specific request outcomes and direct actions; G13 owns reporting/aggregation. | OPEN |
| REM-069 | G11 | G14 | Global audit/state-safety review must preserve requester and Owner effective-authority attribution, prevent duplicate/stale Owner actions across sessions, protect unknown-outcome retry, keep direct Owner actions distinct from requests, and exclude credentials/unrestricted clinical content from Owner approval/audit metadata. | G11 unifies exception work while relying on current-state revalidation and authority-aware audit across many workflows. | OPEN |
| REM-070 | G12 | G13 | Reporting must preserve historical staff/medicine/pharmacy-unit attribution after disable/archive/role/config changes. Current labels may be shown for usability, but historical business facts and unit/account identity must not disappear or be reassigned; report logic must distinguish archived/current entities. | G12 makes staff, medicines and pharmacy units non-destructive lifecycle entities while G13 owns report presentation/aggregation. | OPEN |
| REM-071 | G12 | G14 | Cross-product security/state review must cover Owner TOTP-gated role grants, zero-active-Owner protection, live role revocation/account disablement, safe retries of account/config changes, and configuration audit with no credential/TOTP/recovery secrets. | G12 adds state transitions whose safety depends on current authority/session/account/config truth. | OPEN |
| REM-072 | G12 | G15 | A4/output configuration changes are prospective presentation configuration only. Reprint of historical prescription/bill/other output must use preserved historical business facts/snapshots even if the current template/layout changes; define whether reprint uses current visual template with historical facts or preserved historical rendering without altering source facts. | G12 separates configurable output layout from immutable historical records; G15 owns print/reprint behavior. | OPEN |
---

## Resolved Reminder History

| REM-036 | G5 | G8 | Resolved by G8: every dispense revalidates active Sent-to-Pharmacy state; effective cancellation blocks future dispense while committed dispensing/stock remains. | RESOLVED |
| REM-042 | G6 | G8 | Resolved by P-066/PHA-02: Pharmacy visibility is limited to current/previous prescriptions, allergies, and dispensing context; unrestricted diagnosis/notes/history remain blocked. | RESOLVED |
| REM-045 | G7 | G8 | Resolved by G8 fulfilment-lineage model: Pharmacy requires Sent to Pharmacy + latest current Finalized prescription, Superseded versions are non-dispensable, and prior dispensing counts against corrected current allowance. | RESOLVED |


| REM-035 | G5 | G7 | Resolved by G7: effective cancellation blocks new active draft/finalize/replacement/pharmacy-readiness progression while all Draft/Finalized/Superseded prescription history remains preserved. | RESOLVED |
| REM-041 | G6 | G7 | Resolved by G7 readiness model and reconciliation commit `e7020544866df081bd98e469890c61ea3f95c1c7`: consultation completion and current Finalized prescription are independent prerequisites; second prerequisite advances to Sent to Pharmacy. | RESOLVED |


| REM-021 | G3 | G6 | Resolved by G6 Doctor demographic-review/direct-correction contracts and Patient-ID-scoped longitudinal history: stale value/reviewer is blocked and Possible Duplicate candidate histories are not auto-combined. | RESOLVED |
| REM-034 | G5 | G6 | Resolved by G6 consultation/cancellation concurrency: authoring starts only at With Doctor, Pending cancellation does not freeze work, and effective cancellation blocks stale future writes while preserving saved clinical content. | RESOLVED |


| ID | Raised By | Target Group | Disposition | Status |
| --- | --- | --- | --- | --- |
| REM-006 | G1 | G12 | Resolved by G12 live role-revocation behavior: already-open workspace loses revoked authority on protected action/navigation/refresh. | RESOLVED |
| REM-012 | G2 | G12 | Resolved by G12 Owner-role lifecycle: Admin cannot grant Owner and newly granted Owner capability remains unavailable until required TOTP readiness. | RESOLVED |
| REM-013 | G2 | G12 | Resolved by G12 separate account/role/credential state: reset never re-enables; re-enable needs fresh sign-in; role removal affects only that authority. | RESOLVED |
| REM-031 | G4 | G12 | Resolved by G12 prospective consultation-fee configuration: existing Visit applied amount remains frozen. | RESOLVED |
| REM-059 | G9 | G12 | Resolved by G12 prospective pharmacy price/tax configuration: existing bill snapshot is never recalculated. | RESOLVED |
| REM-064 | G10 | G12 | Resolved by G12 prospective conversion/threshold/catalogue configuration and preserved base-unit movement history; Admin configuration cannot bypass Owner operational stock control. | RESOLVED |
| REM-067 | G11 | G12 | Resolved by G12 role/account lifecycle: request-time authority history is preserved, revoked authority stops, and pending non-Owner reset becomes ineligible after Owner grant. | RESOLVED |
| REM-005 | G1 | G11 | Resolved by G11/P-096: same human may act under distinct legitimate roles, with requester and Owner decision preserved as separate authority-attributed events. | RESOLVED |
| REM-010 | G2 | G11 | Resolved by G11 password-reset lifecycle: Set Temporary Credential is the action and successful reset is Resolved, not generic Approved/Rejected. | RESOLVED |
| REM-011 | G2 | G11 | Resolved by G11: only current eligible Pending reset is actionable; stale/resolved reset cannot act again and unknown result is refreshed before retry. | RESOLVED |
| REM-029 | G4 | G11 | Resolved by G11 waiver rules: one Pending Unpaid request, Paid/Waived staleness, rejection leaves Unpaid, and Direct Owner Waiver stays outside self-approval. | RESOLVED |
| REM-030 | G4 | G11 | Resolved by G11 payment-correction detail: captured/current/proposed records are distinct, changed baseline blocks application, and correction is not refund. | RESOLVED |
| REM-038 | G5 | G11 | Resolved by G11 Visit-cancellation rules: one Pending request, workflow may continue, Completed-before-decision is stale, history remains, and same-human Doctor/Owner attribution is preserved. | RESOLVED |
| REM-057 | G9 | G11 | Resolved by G11 bill-void review: latest payment state is revalidated, Pending bill stays active/payable, Paid approval warns no refund/no stock restoration, and post-Visit bill administration remains possible. | RESOLVED |
| REM-058 | G9 | G11 | Resolved by G11 pharmacy payment-correction rules: baseline-safe/stale-safe, bill lines/total immutable, type-specific current/proposed/history context retained. | RESOLVED |
| REM-063 | G10 | G11 | Resolved by G11 inventory/transfer review: captured/current stock shown, stale/invalid application blocked, linked transfer semantics preserved, and Direct Owner Adjustment is not self-approval. | RESOLVED |
| REM-047 | G7 | G10 | Resolved by G10 movement model: prescription finalization/replacement never retroactively alters stock; prior deduction remains attached to the actual dispense/original version. | RESOLVED |
| REM-052 | G8 | G10 | Resolved by G10 batch/base-unit ledger and stale-stock rules: dispense remains atomic with stock deduction, expired/insufficient stock cannot satisfy supply, Pending changes reserve nothing, and concurrent stock is revalidated. | RESOLVED |
| REM-056 | G9 | G10 | Resolved by G10 physical-movement boundary: billing/payment/Visit completion/bill void do not mutate stock; legitimate correction is a separate Owner-authorized inventory movement. | RESOLVED |
| REM-028 | G4 | G9 | Resolved by G9 external-payment/payment-correction contract: explicit Paid confirmation, UPI/Cash/Card/Other, Other description, optional reference, no partial/refund, baseline-safe Owner correction, and unknown-outcome checking; consultation waiver was not imported into pharmacy billing. | RESOLVED |
| REM-037 | G5 | G9 | Resolved by G9 Visit-cancellation boundary: cancellation blocks new dispensing/new bill creation while preserving existing bill/payment history and allowing record-level payment/correction/void administration without reopening the Visit. | RESOLVED |
| REM-046 | G7 | G9 | Resolved by G9 source-dispensing bill lineage: prescription replacement does not erase or re-bill previously supplied/billed quantities; only new committed supply becomes newly billable. | RESOLVED |
| REM-051 | G8 | G9 | Resolved by G9 Visit-level pharmacy completion: all committed dispensing across units must be billed, intended fulfilment must be finished, unsupplied remainder may close with no back-order, and payment state does not gate completion. | RESOLVED |
| REM-001 | G1 | G2 | Resolved by D-G2-01 / P-009: every account containing Owner authority must satisfy Owner second factor before **any** normal workspace; choosing Doctor/non-Owner workspace cannot bypass it. | RESOLVED |
| REM-002 | G1 | G2 | Resolved across D-G2-07 plus G1 reconciliation: whole-account disablement ends protected account use when detected and returns to sign-in; role-only revocation removes affected authority/workspace. Exact propagation remains technical. | RESOLVED |
| REM-003 | G1 | G2 | Resolved by D-G2-06 / P-013: reset credential enters only the forced-change gate; successful replacement then resumes G1 normal single-/multi-workspace routing. | RESOLVED |
| REM-004 | G1 | G2 | Resolved by D-G2-01/D-G2-08 and reconciliation commit `5fe2bcc7151092307aa1d527d3b42863ec7e6144`: normal authenticated workspace switching does not repeat login/TOTP, while newly granted Owner authority remains second-factor gated before use. | RESOLVED |
| REM-018 | G3 | G4 | Resolved by P-026/P-031 and G4 decisions: Visit creation always links the selected Patient ID; Possible Duplicate/missing physical file are non-blocking; queue entry remains separate. | RESOLVED |
| REM-019 | G3 | G4 | Resolved by P-027 and REC-05/REC-08: active Visit may be Unassigned; Doctor is assigned before queue entry or Visit-linked demographic-correction submission; no fake Visit is created for no-active-Visit correction. | RESOLVED |
| REM-020 | G3 | G5 | Resolved by P-042 / queue interaction: reassignment transfers current demographic-correction reviewer authority to the new Doctor; stale prior-Doctor decision is blocked. | RESOLVED |
| REM-026 | G4 | G5 | Resolved by P-040/DOC-01/REC-07: Assigned Visits Awaiting Financial Eligibility remain outside queue membership/count/order. | RESOLVED |
| REM-027 | G4 | G5 | Resolved by G5 financial-correction rule plus G4 reconciliation commit `8423beaa1cf8f5796a5aae57b7ee708a4ae14d5f`: Waiting/Called may leave current queue non-destructively; With Doctor or later is not unwound. | RESOLVED |

---

## Target G10 — Resolved

G10 evaluated and resolved all inherited reminders targeting Inventory, Stock Accountability & Pharmacy Transfers:

- REM-047
- REM-052
- REM-056

Their dispositions are recorded in Resolved Reminder History.

## Target G11 — Resolved

G11 evaluated and resolved all inherited reminders targeting Owner Approval Center & Exception Control:

- REM-005
- REM-010
- REM-011
- REM-029
- REM-030
- REM-038
- REM-057
- REM-058
- REM-063

Their dispositions are recorded in Resolved Reminder History.

## Target G14 — Mandatory Reminders

When G14 begins, explicitly evaluate:

- REM-007
- REM-008
- REM-009
- REM-014
- REM-015
- REM-016
- REM-017


## Target G4 — Resolved

REM-018 and REM-019 were resolved during the G4 review and are recorded in Resolved Reminder History.

## Target G5 — Resolved

REM-020 was resolved during G5. Additional G4 reminders REM-026/027 were also resolved and are recorded in Resolved Reminder History.

## Target G6 — Resolved

REM-021 was resolved during the G6 review.

## Target G12 — Resolved

G12 resolved REM-006, REM-012, REM-013, REM-031, REM-059, REM-064 and REM-067.

## Target G13 — Mandatory Reminders

When G13 begins, explicitly evaluate:

- REM-022

## Additional Target G14 Reminders from G3

When G14 begins, also explicitly evaluate:

- REM-023
- REM-024
- REM-025


## Target G13 — Additional Mandatory Reminder from G4

When G13 begins, also explicitly evaluate:

- REM-032

## Target G14 — Additional Mandatory Reminder from G4

When G14 begins, also explicitly evaluate:

- REM-033


## Target G6 — Additional Reminder Resolved

REM-034 was resolved during the G6 review.

## Target G7 — Reminder Resolved

REM-035 was resolved during G7.

## Target G8 — Reminder Resolved

REM-036 was resolved during G8.

## Target G13 — Additional Mandatory Reminder from G5

When G13 begins, also evaluate:
- REM-039

## Target G14 — Additional Mandatory Reminder from G5

When G14 begins, also evaluate:
- REM-040


## Target G7 — Additional Reminder Resolved

REM-041 was resolved during G7.

## Target G8 — Additional Reminder Resolved

REM-042 was resolved during G8.

## Target G13 — Additional Mandatory Reminder from G6
When G13 begins, also evaluate:
- REM-043

## Target G14 — Additional Mandatory Reminder from G6
When G14 begins, also evaluate:
- REM-044


## Target G8 — G7 Reminder Resolved

REM-045 was resolved during G8.

## Target G9 — Resolved

G9 evaluated and resolved all inherited reminders targeting Pharmacy Billing, Payment & Bill Cancellation:

- REM-028
- REM-037
- REM-046
- REM-051

Their dispositions are recorded in Resolved Reminder History.

## Target G13 — Additional Mandatory Reminder from G7
When G13 begins, also evaluate:
- REM-048

## Target G14 — Additional Mandatory Reminder from G7
When G14 begins, also evaluate:
- REM-049

## Target G15 — Mandatory Reminder from G7
When G15 begins, evaluate:
- REM-050


## Target G13 — Additional Mandatory Reminder from G8
When G13 begins, also evaluate:
- REM-053

## Target G14 — Additional Mandatory Reminder from G8
When G14 begins, also evaluate:
- REM-054

## Target G15 — Additional Mandatory Reminder from G8
When G15 begins, also evaluate:
- REM-055


## Target G13 — Additional Mandatory Reminder from G9
When G13 begins, also evaluate:
- REM-060

## Target G14 — Additional Mandatory Reminder from G9
When G14 begins, also evaluate:
- REM-061

## Target G15 — Additional Mandatory Reminder from G9
When G15 begins, also evaluate:
- REM-062


## Target G13 — Additional Mandatory Reminder from G10
When G13 begins, also evaluate:
- REM-065

## Target G14 — Additional Mandatory Reminder from G10
When G14 begins, also evaluate:
- REM-066


## Target G13 — Additional Mandatory Reminder from G11
When G13 begins, also evaluate:
- REM-068

## Target G14 — Additional Mandatory Reminder from G11
When G14 begins, also evaluate:
- REM-069


## Target G13 — Additional Mandatory Reminder from G12
When G13 begins, also evaluate:
- REM-070

## Target G14 — Additional Mandatory Reminder from G12
When G14 begins, also evaluate:
- REM-071

## Target G15 — Additional Mandatory Reminder from G12
When G15 begins, also evaluate:
- REM-072
