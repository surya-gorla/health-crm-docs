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
| REM-050 | G7 | G15 | Printing must use the selected finalized version's stored finalization-time availability snapshot. Current Finalized is the default print/reprint target; any historical Superseded/Cancelled-context copy must be unmistakably historical and must never look like the current dispensable prescription. | G7 fixes snapshot/reprint semantics but G15 owns physical-output presentation. | OPEN |
| REM-055 | G8 | G15 | Pharmacy A4 dispensing/billing summary should faithfully show actual supplied quantity, unsupplied remainder, approved substitute supplied where relevant, and pharmacy-unit context without altering the original prescription version. | G8 defines fulfilment truth; G15 owns physical output. | OPEN |

| REM-062 | G9 | G15 | Pharmacy A4 bill/dispensing output must use the frozen bill snapshot and unit attribution, show actual supplied quantities/payment context where applicable, distinguish active versus Cancelled/Voided historical copies, and never recalculate historical bills from current prices, prescription state, or stock. Evaluate together with REM-055 for unsupplied remainder/substitute presentation. | G9 fixes immutable bill snapshot semantics while G15 owns physical-output presentation. | OPEN |
| REM-072 | G12 | G15 | A4/output configuration changes are prospective presentation configuration only. Reprint of historical prescription/bill/other output must use preserved historical business facts/snapshots even if the current template/layout changes; define whether reprint uses current visual template with historical facts or preserved historical rendering without altering source facts. | G12 separates configurable output layout from immutable historical records; G15 owns print/reprint behavior. | OPEN |
| REM-074 | G14 | G15 | Printing/reprinting is a read-only rendering operation over preserved source facts/snapshots: it must not create a new prescription version, bill, dispense, payment, stock movement, approval or other business-state effect merely because output is generated/retried. Unknown printer/browser outcome may retry rendering, while historical/current-copy labeling and source-snapshot rules remain explicit. | G14 separates read-only derived views from state-changing actions; G15 owns physical output/reprint behavior. | OPEN |
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
| REM-007 | G1 | G14 | Resolved by G14 audit attribution: same-human actions retain separate effective role/workspace events. | RESOLVED |
| REM-008 | G1 | G14 | Resolved by G14 current-authority rule: an already-open protected page cannot preserve revoked authority. | RESOLVED |
| REM-009 | G1 | G14 | Resolved by G14 independent-tab rule: each tab keeps explicit permitted workspace context and revalidates current authority independently. | RESOLVED |
| REM-014 | G2 | G14 | Resolved by G14 secret-exclusion rule: credentials/TOTP/recovery-code values never enter normal audit/history. | RESOLVED |
| REM-015 | G2 | G14 | Resolved by G14 duplicate/unknown-outcome contract: reset/request already resolved elsewhere cannot execute again. | RESOLVED |
| REM-016 | G2 | G14 | Resolved by G14 current-authority rule across open tabs/sessions after account/role change. | RESOLVED |
| REM-017 | G2 | G14 | Resolved by G14 recovery-code contract: use is one-time, regeneration invalidates prior set, values remain secret. | RESOLVED |
| REM-023 | G3 | G14 | Resolved by G14 Patient-create safety: final duplicate/current-state recheck and unknown-outcome recovery precede another create attempt. | RESOLVED |
| REM-024 | G3 | G14 | Resolved by G14 identity/demographic audit metadata with source-data permission boundaries. | RESOLVED |
| REM-025 | G3 | G14 | Resolved by G14 baseline-specific stale rule: changed demographic baseline cannot be overwritten by old proposal. | RESOLVED |
| REM-033 | G4 | G14 | Resolved by G14 retry/stale contract covering Visit/payment/waiver/correction plus sequenced Paid+queue partial-effect recovery. | RESOLVED |
| REM-040 | G5 | G14 | Resolved by G14 first-valid-transition/current-Visit-state rule for concurrent queue/cancellation/completion actions. | RESOLVED |
| REM-044 | G6 | G14 | Resolved by G14 clinical draft/amendment conflict rule, local-input preservation and clinical-content audit boundary. | RESOLVED |
| REM-049 | G7 | G14 | Resolved by G14 prescription version safety: duplicate finalization blocked, replacement uses current baseline and lineage remains auditable. | RESOLVED |
| REM-054 | G8 | G14 | Resolved by G14 dispense revalidation, multi-unit allowance protection and atomic dispense+stock unknown-outcome recovery. | RESOLVED |
| REM-061 | G9 | G14 | Resolved by G14 billing/payment/void/completion retry and current-state rules, including latest-state bill-void review. | RESOLVED |
| REM-066 | G10 | G14 | Resolved by G14 inventory movement immutability, no-negative-stock, stale-baseline, linked-transfer and unknown-outcome rules. | RESOLVED |
| REM-069 | G11 | G14 | Resolved by G14 Owner-work duplicate/stale protection, authority attribution, direct-action distinction and secret/clinical boundaries. | RESOLVED |
| REM-071 | G12 | G14 | Resolved by G14 current account/role authority, TOTP/zero-Owner safeguards, account/config retry and secret-free audit. | RESOLVED |
| REM-073 | G13 | G14 | Resolved by G14 read-only/report-scope/time-basis rule: reporting cannot mutate source and remains reproducible from preserved history. | RESOLVED |
| REM-022 | G3 | G13 | Resolved by G13 returning-patient definition: reporting uses actual permanent Patient IDs and Possible Duplicate profiles remain separate identities until a future merge exists. | RESOLVED |
| REM-048 | G7 | G13 | Resolved by G13 most-prescribed definition: standard aggregation uses the current final prescription version per Visit and does not double-count Superseded versions; lineage remains historical detail. | RESOLVED |
| REM-053 | G8 | G13 | Resolved by G13 reconciliation: medicine sales use actual committed supplied quantity/value, approved substitution is attributed to the actual substitute medicine supplied, unsupplied remainder contributes zero sale, and pharmacy-unit provenance is preserved. | RESOLVED |
| REM-032 | G4 | G13 | Resolved by G13 consultation-revenue definition: current effective Paid state only, Waived excluded, preserved prior payment history not double-counted. | RESOLVED |
| REM-039 | G5 | G13 | Resolved by G13 successful waiting-journey definition: ordinary queue movements preserve entry time; actual removal/re-entry creates new segment used if it leads to first With Doctor. | RESOLVED |
| REM-043 | G6 | G13 | Resolved by G13 patients-seen definition: one count at first Consultation Completed; amendments/later cancellation do not duplicate or erase that historical event. | RESOLVED |
| REM-060 | G9 | G13 | Resolved by G13 pharmacy-revenue definition: effective Paid/frozen bill amount, multi-unit attribution, Paid+Voided no-refund retained as recorded money received with void context. | RESOLVED |
| REM-065 | G10 | G13 | Resolved by G13 inventory/sales reporting: unit movement ledgers, valid-vs-expired distinction, transfers net zero and are not sales, corrective movements keep categories. | RESOLVED |
| REM-068 | G11 | G13 | Resolved by G13 exception reporting: effective outcomes separated from Pending/Rejected/Stale workflow activity and direct Owner actions are not double-counted. | RESOLVED |
| REM-070 | G12 | G13 | Resolved by G13 archived reporting dimensions: disabled/archived staff/medicine/unit identities remain historically attributable and filterable. | RESOLVED |
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

## Target G13 — Resolved

G13 resolved all reminders targeting Reporting & Management Visibility:

- REM-022
- REM-032
- REM-039
- REM-043
- REM-048
- REM-053
- REM-060
- REM-065
- REM-068
- REM-070

REM-022 and REM-048 were already satisfied by the G13 product definition but were omitted from the initial reminder-register closure. REM-053 required one explicit reconciliation clarification so approved-substitute sales are attributed to the actual medicine supplied while preserving pharmacy-unit provenance.

## Target G4 — Resolved

REM-018 and REM-019 were resolved during the G4 review and are recorded in Resolved Reminder History.

## Target G5 — Resolved

REM-020 was resolved during G5. Additional G4 reminders REM-026/027 were also resolved and are recorded in Resolved Reminder History.

## Target G6 — Resolved

REM-021 was resolved during the G6 review.

## Target G12 — Resolved

G12 resolved REM-006, REM-012, REM-013, REM-031, REM-059, REM-064 and REM-067.

## Target G6 — Additional Reminder Resolved

REM-034 was resolved during the G6 review.

## Target G7 — Reminder Resolved

REM-035 was resolved during G7.

## Target G8 — Reminder Resolved

REM-036 was resolved during G8.

## Target G7 — Additional Reminder Resolved

REM-041 was resolved during G7.

## Target G8 — Additional Reminder Resolved

REM-042 was resolved during G8.

## Target G8 — G7 Reminder Resolved

REM-045 was resolved during G8.

## Target G9 — Resolved

G9 evaluated and resolved all inherited reminders targeting Pharmacy Billing, Payment & Bill Cancellation:

- REM-028
- REM-037
- REM-046
- REM-051

Their dispositions are recorded in Resolved Reminder History.

## Target G14 — Resolved

G14 evaluated and resolved all 20 inherited reminders targeting Cross-Product State, Audit, History & Safety:

- REM-007
- REM-008
- REM-009
- REM-014
- REM-015
- REM-016
- REM-017
- REM-023
- REM-024
- REM-025
- REM-033
- REM-040
- REM-044
- REM-049
- REM-054
- REM-061
- REM-066
- REM-069
- REM-071
- REM-073

Their dispositions are recorded in Resolved Reminder History.

## Target G15 — Mandatory Reminder from G7
When G15 begins, evaluate:
- REM-050


## Target G15 — Additional Mandatory Reminder from G8
When G15 begins, also evaluate:
- REM-055


## Target G15 — Additional Mandatory Reminder from G9
When G15 begins, also evaluate:
- REM-062


## Target G15 — Additional Mandatory Reminder from G12
When G15 begins, also evaluate:
- REM-072




## Target G15 — Additional Mandatory Reminder from G14
When G15 begins, also evaluate:
- REM-074
