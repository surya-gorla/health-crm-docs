# Hospital CRM — Interaction and Form Behavior Specification

## Document Control

| Field | Value |
| --- | --- |
| Document | Interaction and Form Behavior Specification |
| Version | 0.12 |
| Status | DRAFT — PRD companion |
| Date | 2026-09-20 |
| Parent | PRD v0.12 |
| Screen source | Document 07 |
| Business source | BRD v1.0 LOCKED |
| Classification | DERIVED PRODUCT DESIGN unless explicitly marked INHERITED |

---

# 1. Purpose

This document defines reusable interaction behavior across the V1 product so that screens do not independently invent conflicting UX.

It covers:

- forms;
- search;
- validation;
- patient identity;
- payment capture;
- approvals;
- queue interactions;
- medicine selection;
- dispensing;
- inventory quantities;
- tables/lists;
- status presentation;
- notifications;
- loading/empty/error states;
- stale data;
- confirmation and reason capture;
- unsaved changes.

This document does not specify visual branding, CSS, framework, database, or API implementation.

---

# 2. Interaction Principles

## IX-01 — Explicit over surprising

High-impact actions must happen only after a clear user action.

Examples:

- selecting UPI does not mark Paid;
- selecting a substitute does not approve substitution;
- opening a cancellation dialog does not cancel;
- entering an adjustment quantity does not alter stock.

## IX-02 — Common path is short

Frequent operational actions should avoid unnecessary fields, dialogs, and page transitions.

## IX-03 — Exceptions carry context

Every exception request must show the affected record and why the exception matters.

## IX-04 — Preserve user confidence

The product must clearly communicate whether an action is:

- not yet saved;
- saved;
- pending approval;
- approved;
- rejected;
- failed.

## IX-05 — Do not use color alone

State meaning should be represented by text/iconography/layout as well as visual emphasis.

---

# 3. Global Form Contract

## 3.1 Labels

Every persistent input has a visible label or equivalent accessible label.

Placeholder text is not the only label.

## 3.2 Required fields

Required fields are marked consistently.

Do not mark optional fields as required merely to simplify implementation.

## 3.3 Validation timing

Use:

- immediate validation for structurally impossible values when helpful;
- submit-time validation for required fields;
- server/business validation after submit where effective state must be checked.

Do not erase entered values after a validation error.

## 3.4 Error placement

Field-specific errors appear near the field.

Cross-form/business errors appear in a prominent form-level region.

## 3.5 Submit behavior

While a final submit is in progress:

- prevent accidental duplicate submission;
- keep the user informed;
- do not show success until the effective result is confirmed.

## 3.6 Unsaved changes

If a user attempts to leave a materially edited form before successful save/finalization, warn before discarding the unsaved input.

This does not require a persistent autosave feature.

## 3.7 Read-only history

Historical versions use read-only presentation.

Do not present editable controls that suggest a historical record can be changed in place.

---

# 4. Text, Identifier, and Date Inputs

## 4.1 Patient ID / Visit ID

IDs are displayed in a copy-friendly form.

They are not editable user fields after generation.

## 4.2 Names

Do not transform patient/staff names into uppercase or other formatting that could alter intended display unless explicitly configured.

## 4.3 Phone

Phone input should tolerate normal human formatting while preserving the actual value needed by the system.

Phone is never treated as globally unique patient proof.

## 4.4 Email

Email is required for patient registration because the BRD says so, but it must not be used as unique identity unless separately decided later.

## 4.5 DOB

DOB is the entered value.

Age is derived for display.

Age is not separately editable.

## 4.6 Reason fields

Where a specific reason is required, use a multi-line free-text control rather than a vague yes/no confirmation.

A generic value such as blank or whitespace is not acceptable.

The product should not invent a mandatory coded-reason taxonomy unless separately designed.

---

# 5. Patient Search Behavior

## 5.1 Search modes

Support search by:

- Patient ID;
- phone;
- name.

The UI may use one smart search field or separate controls as long as all three are supported.

## 5.2 Exact Patient ID

A valid exact Patient ID is visually prioritized because it is the unique patient identifier.

Exact match does not silently change active patient context. Reception performs an explicit **Select Patient** action before subsequent work uses that identity.

## 5.3 Phone results

A phone search may return multiple patients.

Result design must make shared-phone cases understandable:

- each patient remains a distinct candidate row;
- equal phone values do not collapse identities;
- phone match alone is never presented as confirmation.

## 5.4 Name similarity

Name similarity may surface candidates.

The UI must describe them as candidates, not “the patient,” until Reception/patient confirms identity.

## 5.5 No auto-merge

No interaction, score, or duplicate warning can auto-merge patient records in V1.

## 5.6 Candidate identity presentation

Candidate rows expose enough identity for confirmation without turning search into clinical-history access.

Minimum useful result context:

- patient name;
- Patient ID;
- phone;
- DOB / derived age context;
- compact address cue where useful;
- Possible Duplicate status.

Limited prior-Visit identity-confirmation context may be opened deliberately where permitted. Do not expose unrestricted diagnosis, clinical notes, or prescriptions to Reception through search.

## 5.7 New patient action

When search is ambiguous, **Register New Patient** remains deliberate and visually separate from selecting a candidate.

Starting registration does not itself certify that no duplicate exists; final registration still performs the current candidate check defined in Section 6.

---

# 6. Patient Registration Form Behavior

## 6.1 Required group

Required:

- full name;
- phone;
- DOB;
- gender;
- address;
- email.

Required fields reject blank/unusable values. DOB cannot be a future/impossible date.

## 6.2 Optional group

Optional fields should be visually secondary:

- emergency contact;
- blood group;
- known allergies;
- guardian/parent;
- Government ID.

Do not promote optional fields to required merely for implementation convenience.

## 6.3 Pre-creation edit boundary

Before effective Patient creation, Reception may freely correct unsaved registration values.

After creation, established demographics follow the Doctor-controlled correction contract rather than direct Reception overwrite.

Patient ID is system-generated only after successful creation and is never an editable registration/correction input.

## 6.4 Final duplicate-candidate check

Before the create action becomes effective, evaluate current candidate matches from the entered identity data.

If no candidate requires review:
- creation may proceed.

If candidates are surfaced:
- pause effective creation;
- show the candidate set;
- allow explicit **Select Existing Patient**; or
- allow explicit **Create New as Possible Duplicate** when the patient cannot confidently confirm an existing record.

A candidate appearing only at final submit because another record was recently created is handled the same way. Do not silently create the new Patient.

Exact similarity-scoring thresholds remain technical/solution design.

## 6.5 Possible Duplicate behavior

If Reception deliberately continues after unresolved candidate review:

- apply the Possible Duplicate marker automatically;
- keep the profile fully usable;
- retain safe provenance: candidate Patient IDs surfaced at creation, creating actor, and time;
- do not auto-merge, auto-link, or silently clear the marker.

The warning informs identity risk; it does not block legitimate creation when the patient cannot confidently identify an existing profile.

## 6.6 Pending and unknown create outcome

While Patient creation is submitting:

- prevent duplicate final submit;
- retain entered context;
- do not show success or a Patient ID until effective creation is confirmed.

If the outcome is unknown:
- do not blindly send another create request;
- refresh/check effective patient state first;
- recover the successful Patient creation if it already occurred.

Exact idempotency implementation remains technical.

## 6.7 Physical-file association

After successful creation, display Patient ID in copy-friendly form for association with the paper file.

A missing physical file:
- does not block digital Patient use;
- does not block Visit creation/payment/queue flow;
- does not justify another Patient ID.

Physical-file locating/replacement stays outside the CRM workflow.

## 6.8 Reception patient-history boundary

Reception may see only identity/operational prior-Visit summaries needed for patient matching and reception work.

Do not use Patient Profile or search expansion to expose unrestricted:
- diagnosis;
- clinical notes;
- prescriptions;
- Doctor longitudinal clinical history.

---

# 7. Status Vocabulary Contract

Use consistent visible labels.

## 7.1 Consultation payment

- Paid
- Unpaid
- Waived

## 7.2 Visit/queue

- Waiting
- Called
- Unresponded
- With Doctor
- Consultation Completed
- Sent to Pharmacy
- Completed
- Cancelled/Voided

## 7.3 Prescription

- Draft/unfinalized where needed by implementation
- Finalized
- Superseded

## 7.4 Approval

- Pending
- Approved
- Rejected

## 7.5 Pharmacy availability

- In Stock
- Out of Stock
- Not Stocked

Do not create synonymous labels on different screens such as “Cancelled” in one place and “Deleted” in another.

---

# 8. Payment Capture Pattern

## 8.1 Default methods

Direct choices:

- UPI
- Cash
- Card
- Other

Bank Transfer is entered via Other if used.

Payment method is informational/reconciliation data for an external payment; choosing a method does not initiate payment.

## 8.2 Other

When Other is selected:

- show a required short “Payment method” description field;
- do not allow final Paid confirmation until it is completed.

## 8.3 Reference

Payment reference/transaction number is optional.

Do not force it for UPI or Card.

## 8.4 Consultation initial state and applied amount

A newly created Visit starts with effective consultation outcome **Unpaid**.

The Visit stores the consultation amount applied at Visit creation. Later clinic-fee configuration changes do not silently rewrite that Visit.

## 8.5 Final Paid action

Use an explicit action such as **Mark Paid**.

Selecting UPI, Cash, Card, or Other must never mark a payment Paid by itself.

Before confirmation show:

- Visit;
- full effective amount;
- selected method;
- Other description if applicable;
- optional reference.

Disable duplicate final confirmation while submitting.

A Visit already Paid/Waived does not expose normal Mark Paid again.

## 8.6 Consultation queue eligibility

Queue entry is a separate operational consequence.

Require both:
- Paid or Waived;
- Doctor assigned.

Combined **Mark Paid & Add to Queue** is acceptable only when Doctor is already assigned.

If payment succeeds but queue insertion fails:
- preserve Paid;
- show **Paid — Not Queued**;
- allow safe queue retry after refresh.

If payment outcome is unknown, do not queue until Paid is confirmed.

## 8.7 Unpaid

Unpaid is the valid initial/remaining state.

It may coexist with:
- Doctor assigned or unassigned;
- Pending waiver.

It is never queue-eligible.

## 8.8 Waived

Waived is a financial outcome separate from Paid.

It is queue-eligible but still requires Doctor assignment before actual queue entry.

Do not silently convert Waived to Paid or Paid to Waived through normal payment UI.

## 8.9 No partial payment UI

Do not expose amount-paid/amount-due split controls for consultation or pharmacy V1.

## 8.10 No refund UI

Do not expose refund action in V1.

A Paid -> Unpaid payment correction means the earlier record was wrong; it is not a refund.

## 8.11 Payment correction

Recorded payment correction uses request/Owner decision, not direct overwrite.

Every request shows:

- payment identity (Visit consultation payment or pharmacy bill payment);
- captured current effective payment baseline;
- proposed corrected payment record;
- mandatory specific reason.

For consultation, supported fields may include effective Paid/Unpaid state, method, Other description, reference, and Visit-specific recorded amount.

For pharmacy, supported fields may include effective Paid/Unpaid state, method, Other description, reference, or other payment-record metadata. Pharmacy payment correction **never** changes the established bill lines or bill total.

If proposed state is Paid, full-payment method rules apply.

Before Owner applies a correction, revalidate the captured baseline. A changed baseline makes the old request stale/non-applicable until refreshed review.

Do not use payment correction to silently create/revoke Waived, create refund behavior, or repair a wrong pharmacy bill amount. Paid -> Unpaid means the earlier payment record was wrong.

Only one simultaneously actionable request against the same current payment baseline should exist.

## 8.12 Unknown outcome / retry

For a state-changing financial/Visit action with unknown outcome, do not blindly resubmit.

This applies at minimum to:

- Visit creation;
- consultation Paid recording;
- pharmacy bill creation;
- pharmacy Mark Paid;
- Visit pharmacy-completion transition;
- bill-void request/decision;
- payment-correction request/decision.

Instead:

- retrieve current effective Visit/bill/payment/request state;
- recover the already-created state if present;
- prevent duplicate effective records/actions;
- only allow a new state-changing attempt once the prior outcome is known safe.

Exact idempotency/transaction implementation remains technical design.
---

# 9. Reason-and-Approval Pattern

This pattern is shared by:

- waiver;
- Visit cancellation;
- pharmacy bill void;
- payment correction;
- inventory adjustment;
- stock transfer;
- clinical amendment;
- prescription replacement.

Not all use the same approver, but the interaction contract is consistent.

## 9.1 Request step

Show:

- affected record;
- current state;
- proposed action/change;
- mandatory reason;
- Submit Request.

## 9.2 Pending state

After submit:

- show Pending;
- identify approver role;
- do not imply effective state changed.

## 9.3 Decision step

Approver sees:

- requester;
- reason;
- current state;
- proposed result;
- important impact warning;
- Approve;
- Reject.

## 9.4 Decision confirmation

High-impact approvals should require an explicit final click.

Avoid double confirmations unless the impact is unusually high and the extra step materially prevents error.

## 9.5 Rejection

Rejected requests remain in history.

If the business record remains active, say so clearly.

---

# 10. Owner Approval Center Behavior

## 10.1 Pending-first default

Approval Center should default to Pending requests.

History filters expose Approved/Rejected.

## 10.2 Sorting

Pending work should be sortable/filterable by time and type.

Do not silently prioritize based on business urgency that has not been defined.

## 10.3 Resolved request

A resolved request is read-only.

If another browser/session already resolved the request, the current user must see stale-state feedback rather than a second effective approval.

## 10.4 Type-specific impact

**Bill void:** show latest payment state and explicitly say inventory will not be restored; if Paid, explicitly say approval does not create a refund.  
**Inventory adjustment:** show category/reason, captured baseline and current stock, entered/base quantity, proposed delta/result; stale baseline must block application.  
**Transfer:** show source/destination, medicine/batch, captured/current transferable quantity, normalized quantity, and linked transfer consequence.  
**Payment correction:** show captured baseline plus original/proposed payment fields; re-check baseline before applying. Pharmacy correction must not expose bill lines/total as editable correction fields.  
**Visit cancellation:** show current Visit state and that history remains.  
**Waiver:** show fee/current financial outcome/queue eligibility effect; approval is valid only while the current outcome remains Unpaid. If the Visit became Paid, the pending waiver is stale/non-actionable.

---

# 11. Queue Interaction Pattern

## 11.1 Queue organization

Doctor-specific queues remain visually separated.

If tabs/filters are used, selected Doctor remains visible.

Do not include assigned-but-financially-ineligible Visits in queue count/order. Show those in the separate pre-queue financial context defined by G4.

## 11.2 Default ordering and position

Queue position is current persisted ordering, not editable free-form data.

- new eligible entry -> queue end;
- reassignment -> destination queue end;
- Unresponded -> five positions down/end;
- patient leaves -> same-Doctor queue end.

Do not offer arbitrary drag/drop or priority insertion.

Ordinary repositioning preserves queue-entry history.

## 11.3 Call and Start Consultation

**Call Patient**
- Doctor only;
- current Waiting only;
- sets Called;
- records actor/time;
- Reception sees call.

**Start Consultation**
- Doctor only;
- current Called only;
- Visit must still be assigned to that Doctor;
- sets With Doctor.

Viewing a row does not imply either state change.

## 11.4 Unresponded

Reception marks Unresponded only from current Called.

After success:

- record Unresponded event;
- move five positions down or end;
- current state becomes Waiting;
- new position is visible.

If Called state changed first, action is stale and refresh is required.

## 11.5 Reassign

Reassign is Reception-only from Waiting or Called.

Require:
- current source Doctor;
- destination Doctor different from source;
- explicit confirmation.

Success:
- remove source membership;
- set new Doctor;
- append to destination end as Waiting;
- preserve source queue/call/reassignment history.

If a Visit-linked demographic correction is Pending, reviewer authority follows the new Doctor.

Do not reassign With Doctor or later.

## 11.6 Patient leaves before consultation

From Waiting or Called, Reception may move a financially eligible Paid/Waived Visit to the same queue end.

- current state Waiting;
- prior Call remains historical;
- financial state unchanged;
- not cancellation/refund.

## 11.7 Financial correction while queued

If current effective financial outcome becomes Unpaid before With Doctor:

- Waiting/Called membership leaves active queue;
- preserve existing queue history;
- show Not Queued / Unpaid outside queue;
- renewed eligibility requires explicit new queue entry at end.

If correction occurs at With Doctor or later, do not rewind clinical state.

## 11.8 Urgent case

Do not create priority star, urgency score, priority request, emergency reorder button, arbitrary drag/drop priority, or hidden priority score.

Urgent communication remains outside software.

---

# 12. Consultation Form Pattern

## 12.1 Authoring gate

Editable current consultation exists only while:

- Visit state is With Doctor;
- current Doctor is the assigned Doctor;
- Visit is not Cancelled/Voided.

Opening queue/history does not start consultation.

## 12.2 Required vs optional

Required for **Complete Consultation**:

- chief complaint / patient problem;
- assessment / diagnosis.

Optional:

- symptoms/history;
- examination;
- notes;
- advice;
- follow-up.

Save Draft may succeed before required completion fields are all filled.

## 12.3 Save Draft

Save Draft is explicit and does not change Visit state.

Before save:
- validate current With Doctor state;
- validate current Doctor assignment;
- validate the loaded draft baseline is current.

If newer saved content/state exists, block silent overwrite and refresh/review.

V1 does not require every intermediate draft save to become a permanent amendment/revision.

## 12.4 Clinical history

Historical content opens read-only.

Longitudinal timeline is scoped to one permanent Patient ID.

Possible Duplicate marker does not merge candidate Patient histories.

## 12.5 Completion

Complete Consultation is explicit.

Before completion:
- required clinical fields are present;
- current state/assignment are valid;
- current content is persisted.

Success:
- current effective clinical record becomes completed;
- record Consultation Completed;
- ordinary clinical fields become read-only.

Completion does not implicitly finalize prescription. After recording Consultation Completed, apply G7 readiness: a pre-existing current Finalized prescription allows immediate progression to Sent to Pharmacy; otherwise remain Consultation Completed awaiting prescription finalization.

## 12.6 Amendment

Completed consultation cannot re-enter ordinary edit mode.

Create Amendment:
- starts from latest effective revision;
- requires reason;
- creates a new effective revision;
- preserves old revisions.

If latest revision changed after form load, stale amendment cannot apply.

Amendment may correct a completed record even after Visit later becomes Completed or Cancelled/Voided; it does not reopen workflow.

## 12.7 Effective cancellation while editing

Pending cancellation does not freeze valid consultation work.

If cancellation becomes effective:
- block subsequent draft save/completion/active authoring;
- preserve already-saved content;
- do not silently commit stale local input.

---

# 13. Medicine Search and Prescription Lifecycle Pattern

## 13.1 Search result identity

Medicine result should present:

- display name;
- strength;
- dosage form;
- manufacturer where helpful;
- generic/molecule where available;
- current availability.

## 13.2 Availability while authoring

Availability is informational during prescribing.

Out of Stock/Not Stocked does not disable Add to Prescription or finalization.

Immediately before finalization/replacement finalization, refresh current availability.

The finalized version stores clinic-wide availability-at-finalization per item for printing/history.

## 13.3 Multi-pharmacy view

If multiple units exist:

- show clinic total/overall availability;
- expose unit-level availability without giving Doctor inventory-edit controls.

Per-unit availability remains operationally current; it does not rewrite a prior finalized version's clinic-wide marker.

## 13.4 Prescription draft row

Each unfinalized row supports:

- dose amount;
- frequency;
- duration;
- optional timing/food/instruction;
- quantity.

Draft may be edited during With Doctor or Consultation Completed while no current finalized prescription exists.

## 13.5 Quantity

If deterministic, show system-calculated quantity and do not require duplicate manual entry.

If calculation is not deterministic, Doctor quantity is required.

Finalization is unavailable until every row has resolved quantity and required fields.

## 13.6 Finalization

Finalize Prescription is explicit and freezes the version.

Require:
- active allowed Visit state;
- at least one complete item;
- resolved quantities;
- current draft baseline;
- refreshed availability snapshot.

If Visit is With Doctor, finalization does not end consultation.

If Visit is Consultation Completed, finalization satisfies pharmacy readiness and Visit becomes Sent to Pharmacy.

If prescription was already finalized while With Doctor, later clinical completion satisfies the second readiness prerequisite and current Visit becomes Sent to Pharmacy.

## 13.7 Immutable/current/Superseded

Finalized current version is read-only.

Any correction uses replacement.

Replacement atomically moves:
- old current Finalized -> Superseded;
- replacement -> current Finalized.

Pharmacy/default print targets current Finalized, not Superseded.

## 13.8 Replacement with prior dispensing

Show prior dispensing context before replacement finalization.

Replacement never erases:
- dispensed quantity;
- stock deduction;
- bill/payment history.

If active version changed before submit, replacement is stale.

## 13.9 Visit cancellation/closure

Pending cancellation does not automatically block otherwise-valid prescription work.

Effective Cancelled/Voided or Completed blocks new active draft/finalize/replacement.

Existing versions remain history.

---

# 14. Printed Prescription Pattern

## 14.1 Finalization-time ** marker

Only medicines whose **clinic-wide availability snapshot at that prescription version's finalization** is Out of Stock or Not Stocked receive **.

Current stock is not substituted for the stored finalization snapshot when rendering/reprinting that version.

## 14.2 Partial dispensing later

A later partial quantity does not alter the original finalized/printed prescription.

The pharmacy output communicates actual supplied/unsupplied quantity.

## 14.3 Reprint

Reprint:
- does not create a new prescription version;
- uses the same finalized prescription data and availability snapshot;
- defaults to the current Finalized prescription rather than an older Superseded version.

Historical-copy labeling for Superseded/cancelled-context output is finalized in G15.

---

# 15. Dispensing Interaction Pattern

## 15.1 Entry gate

Dispensing requires:

- Visit = Sent to Pharmacy;
- latest current Finalized prescription;
- active permitted pharmacy unit.

A Finalized prescription while Visit is With Doctor is not dispensable.

Superseded/previous prescriptions are historical only.

## 15.2 Prescription-item lineage

Current item shows fulfilment across the prescription version lineage.

For an item lineage:

**remaining allowable = max(0, current active prescribed quantity - cumulative committed dispensed quantity across versions/units for that lineage).**

If corrected quantity is now below already-dispensed quantity:
- remaining = 0;
- show historical overage;
- do not reverse prior stock/dispensing.

A materially changed/new medicine item receives a new fulfilment lineage; removed lineages cannot dispense further.

## 15.3 Quantity model

Per active item show:

- current prescribed total;
- cumulative already dispensed;
- remaining allowable;
- current active-unit valid availability;
- quantity to dispense;
- resulting unsupplied remainder.

## 15.4 Upper bound

Entered quantity must be >0 and cannot exceed:

- current remaining allowable; or
- current valid/non-expired active-unit stock.

Recompute both before final confirmation.

## 15.5 Atomic dispense

Before commit revalidate:

- Visit state;
- current prescription version;
- item lineage/remaining allowance;
- pharmacy unit;
- current valid stock.

Success records dispensing and stock deduction together.

If result is unknown, refresh/check before retry.

## 15.6 Expired stock

Expired stock is never selectable or dispensable.

Owner inventory authority cannot approve an expired-stock dispense.

## 15.7 Partial fulfilment

If less is supplied:

- record supplied quantity;
- show unsupplied remainder;
- bill supplied quantity only;
- do not rewrite prescription;
- do not reserve the remainder.

## 15.8 Substitution

Pending substitute is non-dispensable.

Request identifies current prescription item, proposed substitute, proposed quantity, and reason.

Doctor approval authorizes only the approved proposal.

Approved substitute supply:
- records approval reference;
- consumes the original item fulfilment allowance;
- cannot combine with original supply to exceed allowed remaining fulfilment.

If units/strength/form are not safely comparable, require Doctor-confirmed quantity rather than Pharmacist inference.

## 15.9 Multi-pharmacy

Every dispense shows/records active pharmacy unit.

Stock deduction is unit-specific.

Remaining prescription allowance is clinic-wide across units.

Unit switch with unsaved dispense quantities requires explicit discard/review.

## 15.10 Replacement/cancellation concurrency

If prescription becomes Superseded or Visit becomes Cancelled/Voided before commit:
- block the old dispense action;
- preserve committed dispensing;
- refresh to current state.

---

# 16. Pharmacy Billing Pattern

## 16.1 Billing source and pharmacy-unit ownership

Bill only **committed actual dispensing**, never prescribed-but-unsupplied quantity.

The billing surface must keep the active pharmacy unit visible.

Eligible bill source rows are supplied-but-not-yet-billed dispensing records for that Visit and unit.

- one committed dispensing quantity may belong to only one active/non-voided bill lineage;
- if multiple units supplied one Visit, each unit may create its own bill;
- prescription replacement does not make previously billed dispensing billable again.

Bill creation is explicit; opening billing does not automatically create a bill.

## 16.2 Bill creation snapshot

Before **Create Bill**, show medicine, actual supplied quantity, price basis, configured tax/amount fields where applicable, total, unit, and source dispensing references.

On successful creation:

- bill starts Unpaid;
- bill lines/quantities/price basis/total/unit/source references are frozen;
- later price/configuration or prescription changes do not silently recalculate the bill.

Established bill lines/total are not edited in place. A wrong bill uses void; a wrong payment record uses payment correction.

## 16.3 External payment

Use UPI/Cash/Card/Other.

- Other requires description;
- reference is optional;
- method selection alone does not change payment state;
- explicit **Mark Paid** follows verified external full-payment success;
- Paid represents the full bill total;
- no partial-payment control;
- no refund control.

An existing bill may be marked Paid even after its Visit is Completed. That financial update does not reopen the Visit.

## 16.4 Pharmacy payment correction

Request payment correction rather than directly rewriting an established payment record.

Show:

- bill/payment identity;
- current effective payment baseline;
- proposed payment fields;
- mandatory reason.

Owner decision revalidates the baseline. If it changed, stale proposal cannot apply.

Correction may change payment state/method/Other description/reference/payment metadata, but never bill lines/total. Paid -> Unpaid is record correction, not refund.

## 16.5 Finish clinic-pharmacy fulfilment

Dispensing one medicine or paying one bill does not complete the Visit.

Expose an explicit **Finish Clinic Pharmacy Fulfilment** action while Visit = Sent to Pharmacy.

Before commit, revalidate across the whole Visit:

- no committed dispensing remains unbilled;
- all intended clinic dispensing is finished;
- unsupplied remainder is explicitly left outside with no reservation/back-order;
- all participating pharmacy units' committed dispensing is accounted for in their unit bills;
- no actionable substitution request still represents intended clinic fulfilment.

Payment state does not gate completion.

On success: Sent to Pharmacy -> Completed.

Unsupplied remainder is compatible with Completed because V1 has no back-order.

## 16.6 Multi-unit completion

One unit's bill/payment cannot complete the Visit while another unit still has committed unbilled dispensing.

Show enough Visit-level fulfilment context to explain why completion is enabled or blocked without merging unit-level financial attribution.

## 16.7 Bill void request

Void is a request, not direct destructive action.

The request requires a specific reason and only one actionable Pending request per bill.

While Pending:

- bill remains active;
- payment may still be recorded;
- payment correction may still occur through its separate workflow;
- stock is unchanged.

The request dialog must state:

- bill will leave active billing if approved;
- prior bill/payment/request/decision history remains;
- stock will not automatically be restored;
- Paid bill approval does not create a refund.

## 16.8 Owner void decision and concurrency

Before decision, revalidate bill, request status, and **latest payment state**.

If bill became Paid while Pending, Owner may still approve with explicit no-refund consequence.

Approval -> Cancelled/Voided. Rejection -> active bill unchanged.

A voided bill remains historical and its source dispensing is not silently regenerated into a new bill.

## 16.9 Visit cancellation/completion boundary

Effective Visit cancellation or Visit Completed blocks:

- new dispensing;
- new bill creation.

Existing bills remain available according to authority for:

- Mark Paid after external success;
- payment-correction request/decision;
- bill-void request/decision.

Those actions do not reopen the Visit or permit more dispensing.

## 16.10 Retry and stale-state safety

For bill creation, Mark Paid, Visit completion, void request/decision, and payment correction:

- disable duplicate final submission while pending;
- revalidate current state before applying;
- if result is unknown, refresh/check effective state before retry;
- stale/resolved requests cannot apply again.

Exact locking/transaction mechanics remain technical.


---

# 17. Inventory Quantity, Movement and Transfer Pattern

## 17.1 Base-unit truth

Every medicine has a configured base stock/dispensing unit.

Higher package units may be used for human entry/display where configured, but quantity-changing inventory logic uses normalized base units.

## 17.2 Package conversion visibility

When staff enters package quantity for an adjustment/transfer:

- show entered quantity/unit;
- show conversion factor/result;
- show normalized base-unit quantity;
- preserve the conversion/result used for the eventual movement.

Do not silently reinterpret a historical movement after package configuration changes.

## 17.3 Movement-first stock model

Operational stock quantity is the result of attributable movements.

Do not offer Pharmacist a generic “edit current stock” field.

Movement categories include, as applicable:

- automatic prescription dispense;
- approved stock addition;
- approved damage/loss/expiry reduction;
- approved correction;
- Owner direct adjustment;
- transfer source;
- transfer destination.

Historical movement is read-only. Later correction is another controlled movement.

## 17.4 Batch and valid availability

Where batch/lot applies, movement identifies the concrete batch.

Valid available quantity excludes expired/unavailable batch stock.

A quantity-changing action must not silently use another batch, expired stock, or insufficient batch quantity simply to make totals fit.

## 17.5 Adjustment request preview

Before Pharmacist submits quantity change, show:

- pharmacy unit;
- medicine/batch;
- current captured baseline;
- entered quantity/unit;
- normalized base-unit effect;
- category;
- projected result;
- mandatory reason.

For count correction, show entered counted/resulting quantity **and** derived delta.

For a price-only request, show current -> proposed price and no stock delta.

## 17.6 Pending adjustment

Pending request:

- changes no stock/price;
- creates no reservation;
- keeps captured baseline for Owner review;
- remains historical even after resolution.

Normal legitimate inventory movements may continue while Pending.

## 17.7 Adjustment approval/stale state

Owner review shows captured baseline beside current unit/batch state.

Before approval:

- revalidate current stock/value;
- block if old proposal would no longer produce the reviewed result or would create invalid/negative stock;
- do not partially apply a stale request;
- require refreshed/new proposal when needed.

Approval creates the movement/value change once. Rejection changes nothing.

## 17.8 Owner direct adjustment

Owner direct adjustment is an Owner-authority action, not a self-approval request.

Require the same category/reason/current state/projected effect, plus explicit confirmation and full audit.

## 17.9 Expired stock

When batch reaches expiry:

- mark it expired/unavailable immediately;
- exclude it from valid dispense availability;
- keep recorded expired quantity visible;
- do not automatically erase quantity from ledger;
- disposition/removal uses controlled Owner-authorized adjustment.

Owner authority cannot override expired-stock dispensing prohibition.

## 17.10 Low-stock and near-expiry

Evaluate configured thresholds against current valid inventory/batch context.

Show unit context and retain drill-down from Owner consolidated view.

Exact threshold values are configuration.

## 17.11 Transfer request

Transfer always keeps source and destination visible.

Before submit show:

- source/destination;
- medicine/batch;
- current valid transferable source quantity;
- entered package/base quantity;
- normalized base quantity;
- mandatory reason.

Source and destination must differ.

Pending transfer changes/reserves nothing.

## 17.12 Transfer decision

Owner review revalidates source transferable quantity.

If current source quantity cannot satisfy the full request:

- mark/block stale action;
- do not partially transfer;
- require refreshed/replacement request.

Approval is one effective linked event:

- source -Q;
- destination +Q;
- same transfer ID/reference;
- same physical batch/expiry/manufacturer identity;
- both unit ledgers updated together.

Reject/stale failure changes neither ledger.

## 17.13 Negative-stock prevention

No dispensing, approved adjustment, direct adjustment, or transfer may produce negative stock.

If stock changed since form load, refresh/review rather than silently clipping the requested quantity.

## 17.14 Inventory retry/unknown outcome

For adjustment request/direct adjustment/approval and transfer request/approval:

- disable duplicate final action while pending;
- if outcome is unknown, retrieve request + ledger state first;
- recover already-applied movement if present;
- do not knowingly apply the same movement twice.

Exact transaction/locking implementation remains technical.

---

# 18. Inventory Table Pattern

Recommended columns vary by screen, but inventory tables should support:

- medicine;
- pharmacy unit;
- valid available stock;
- recorded expired/unavailable quantity where useful;
- base/package representation;
- batch/lot;
- nearest expiry;
- low-stock state;
- near-expiry state;
- Out of Stock state.

Movement/history table should support:

- time;
- movement category;
- unit;
- medicine/batch;
- entered quantity/unit;
- normalized delta;
- before -> after;
- source/request/transfer reference;
- actor/effective authority;
- reason where applicable.

Filters:

- low stock;
- out of stock;
- near expiry;
- expired;
- medicine search;
- pharmacy unit where permitted.

Owner consolidated views must allow drill-down to the unit/batch ledger.

Do not visually mix current valid availability with expired/unavailable quantity as if both were dispensable.

---

# 19. Table and List Behavior

## 19.1 Row click vs action buttons

A row may open detail.

High-impact actions should use explicit labeled controls/menu items, not an ambiguous row click.

## 19.2 Empty state

An empty table should explain whether:

- there is genuinely no data;
- filters removed all results;
- the user lacks scope.

## 19.3 Filtering

Applied filters remain visible.

Provide a clear reset/clear mechanism.

## 19.4 Pagination/virtualization

Implementation may choose pagination, virtualization, or equivalent.

The product requirement is that large lists remain usable without changing business behavior.

## 19.5 Sorting

Do not invent business priority through default sorting unless the ordering is inherently defined, such as queue position.

---

# 20. Status Badge / State Presentation

Statuses should be compact and consistent but must include readable text.

Examples:

- Paid
- Unpaid
- Waived
- Pending
- Approved
- Rejected
- Superseded
- Cancelled/Voided
- Out of Stock

Do not rely on red/green alone.

---

# 21. Notification and Attention Pattern

V1 notifications are in-product.

At minimum, attention items include:

- Doctor Call visible to Reception;
- Owner approval requests;
- Doctor substitution requests;
- low-stock alerts;
- near-expiry alerts.

## 21.1 Notification content

Expose:

- type;
- short affected context;
- time;
- status/read state as implemented.

## 21.2 Notification action

Clicking a notification opens the corresponding authorized record/workflow.

A notification does not grant extra permission.

---

# 22. Loading Pattern

## 22.1 Initial page loading

Show clear loading state when primary content is unavailable.

## 22.2 Action loading

For state-changing actions:

- disable duplicate final submit while pending;
- retain context;
- show progress.

## 22.3 Do not fake empty

Do not display “No patients,” “No approvals,” or “No stock” before the query has actually completed.

---

# 23. Empty-State Pattern

An empty state should answer:

1. what is empty;
2. whether this is normal;
3. what the permitted next action is.

Examples:

**No pending approvals** -> “No requests are waiting for your decision.”  
**Doctor queue empty** -> “No patients are currently waiting in this queue.”  
**No patient results** -> offer deliberate New Patient action.

---

# 24. Error Pattern

## 24.1 Recoverable failure

Show:

- what failed in user terms;
- Retry when safe;
- preserve form input where possible.

## 24.2 State-changing failure

Do not show success until the effective state is confirmed.

## 24.3 Unknown outcome

If the client cannot determine whether a state-changing request succeeded, avoid automatically repeating it in a way that could duplicate the action.

Prompt the user to refresh/check current state.

---

# 25. Stale Data / Concurrency Pattern

When an action depends on a current state and that state has changed since loading:

- do not silently apply against stale data;
- explain that the record changed;
- refresh the relevant current values;
- ask the user to review again.

Examples:

- approval already resolved;
- queue Visit reassigned;
- stock changed before dispensing;
- prescription superseded;
- bill already voided.

---

# 26. Permission and State-Unavailability Pattern

## 26.1 User lacks authority

If a user lacks authority for a module/action:

- omit it from normal navigation/action choices;
- do not render protected content and merely disable a control;
- deny direct/bookmarked/history access;
- show safe access-denied messaging where the user reaches a protected route;
- provide a path back to a permitted workspace.

## 26.2 User has authority but current state blocks the action

If the user normally has authority but the current record/state makes an action temporarily invalid:

- the control may remain visible;
- it may be disabled;
- the interface should explain the relevant condition where useful.

Example: a Doctor may see **Complete Consultation** disabled because required clinical fields are missing; a Pharmacist who has no Owner authority should not see **Approve Inventory Adjustment** as a normal action.

## 26.3 Permission revocation during an open session

An already-open page does not preserve revoked authority. On the next protected navigation/action or permission refresh, access is re-evaluated; protected content/action is denied and the user is returned to a permitted workspace when necessary.

The exact permission-refresh/session implementation belongs to technical design.

---

# 27. Confirmation Pattern

Use confirmation for high-impact final actions.

The confirmation should identify the consequence, not merely say “Are you sure?”

Examples:

- **Finalize Prescription** — cannot be edited in place afterward.
- **Approve Bill Void** — bill leaves active billing; stock is not restored.
- **Approve Inventory Loss** — available stock will decrease by X.
- **Disable Staff Account** — user will no longer be able to log in.

---

# 28. Audit Timeline Pattern

Where history is exposed, event entries should be human-readable.

Typical event:

- timestamp;
- actor;
- action;
- target;
- reason when applicable;
- prior -> resulting state where applicable.

Clinical event metadata must respect the clinical-content access boundary.

---

# 29. Multi-Role and Workspace Interaction Contract

The contract applies to every valid multi-role combination, not only Owner + Doctor.

## 29.1 Entry and switching

- a single-workspace user enters that workspace directly after all applicable authentication/credential gates succeed;
- a multi-workspace user chooses among assigned workspaces whose applicable authentication gates are satisfied for the current session and retains an always-reachable workspace-switch control;
- one human uses one account across all assigned roles;
- ordinary switching among already-authentication-qualified workspaces does not require a second username/password login or repeat Owner TOTP solely because of the switch;
- if Owner authority is newly granted to a session that has not satisfied the Owner second-factor requirement, Owner-capable access remains gated by P-009 / Section 35 before it becomes normally switchable.

## 29.2 Authority context

- the active workspace/authority remains visible;
- clinical actions are performed in Doctor authority where Doctor permission is required;
- Owner financial/inventory/approval actions are performed in Owner authority;
- the same separation applies to Reception, Pharmacist, and Administrator combinations;
- do not infer authority from whichever screen happened to be open previously.

## 29.3 Workspace-scoped work context

- switching workspaces does not silently carry the active patient, Visit, Doctor queue, pharmacy unit, or protected record into the target workspace;
- an explicit cross-workspace transition may pass a record reference only after the target workspace/authority is entered;
- material unsaved changes trigger a warning before a switch can discard them.

## 29.4 Multiple browser tabs/windows

A multi-role user may keep different permitted workspaces open in different tabs/windows. Each tab/window retains its own visible workspace/authority context. Changing one must not silently alter another.

## 29.5 Role and account changes while signed in

If a role is revoked while its workspace is open, stale authority must not remain usable. The next protected action/navigation or permission refresh re-evaluates access and safely returns the user to a permitted workspace when required.

If Owner authority is newly granted, the new Owner capability remains unavailable until the Owner second-factor requirement is satisfied for that session.

If the entire account is disabled, P-015 / Section 35 applies: protected account use ends when disablement is detected and the user returns to the sign-in boundary.

## 29.6 Cross-workspace attention

The shell may show an attention/count indicator for another permitted workspace, but opening protected detail/action requires entering the correct workspace/authority context first.

## 29.7 Audit attribution and same-human dual-role actions

Material action history preserves both the human identity and the effective role/workspace used. Conceptually the audit semantics include:

- actor/account;
- effective role or workspace;
- action;
- affected target;
- timestamp;
- reason/decision context where applicable.

If the same human legitimately performs different sides of a workflow through different assigned roles, each action remains separately attributable. For example, the same Owner + Doctor may request a Visit cancellation in Doctor authority and later approve it in Owner authority if the locked business rules permit both actions.

---

# 30. Multi-Pharmacy Interaction Contract

When multiple pharmacy units exist:

- current pharmacy unit is visible;
- inventory tables show unit context;
- dispensing belongs to one unit;
- bill belongs to the dispensing unit;
- transfer explicitly shows source/destination;
- Owner can switch between consolidated and unit view;
- Doctor sees availability but cannot mutate it.

---

# 31. Responsive Behavior

V1 is desktop/laptop primary with tablet-size responsiveness.

Product behavior should preserve:

- visible active workspace;
- patient/Visit context;
- critical actions;
- tables/lists usable without hiding authority context.

A separate mobile application is not required.

Exact breakpoint values are design implementation.

---

# 32. Keyboard and Focus Behavior

For high-frequency desktop workflows:

- standard tab order should follow visual/form order;
- Enter should not accidentally trigger a destructive/high-impact action from an unrelated field;
- validation should move/focus attention to the first meaningful error where practical;
- modal/dialog focus should stay within the active dialog until closed.

Exact accessibility conformance target is a separate design/technical policy decision unless later approved.

---

# 33. Interaction Acceptance Gate

This interaction specification fails review if:

- selecting a payment method alone can mark Paid;
- selecting an approval row can approve/reject without explicit action;
- staff can overwrite a payment rather than request correction;
- Pharmacist can edit stock directly around the approval workflow;
- Doctor can edit a finalized prescription in place;
- expired stock is available in normal dispense selection;
- a shared phone result looks like unique identity proof;
- loading can be mistaken for an empty state;
- a stale approval can be applied twice;
- a multi-pharmacy action hides its unit context;
- a disabled/unauthorized control still exposes protected content;
- unsaved material form edits can be discarded without warning;
- history looks editable/current when it is not;
- a user without authority sees a forbidden action as a normal navigation/action option;
- workspace switching silently carries protected patient/Visit context into another authority context;
- material unsaved work is discarded by workspace switching without warning;
- revoked authority remains usable because the workspace was already open;
- switching workspace in one browser tab silently changes another tab's authority context;
- cross-workspace notifications expose protected detail/action outside the correct workspace;
- an Owner-containing account reaches any normal workspace before required Owner 2FA completes;
- a non-Owner reset credential reaches normal product content before forced password replacement succeeds;
- Forgot Password reveals whether an arbitrary identifier exists, is disabled, or contains Owner authority;
- repeated Forgot Password submissions create duplicate simultaneously actionable Owner reset requests;
- a password reset silently changes account enabled/disabled state;
- a used or invalidated recovery code can be reused;
- normal audit/history exposes passwords, reset credentials, TOTP secrets/codes, or recovery-code values;
- a detected disabled account continues normal protected use through an already-open session;
- a patient is auto-selected or auto-merged solely from phone/name/similarity confidence;
- final Patient creation proceeds despite surfaced duplicate candidates without explicit identity decision;
- Possible Duplicate status blocks otherwise valid Patient/Visit use;
- Reception patient search/profile exposes unrestricted clinical history;
- Reception directly overwrites established demographics after Patient creation;
- a stale demographic-correction request overwrites a newer value;
- missing physical file blocks digital patient workflow or causes a new Patient ID;
- an unknown-outcome Patient creation is blindly retried and can create a duplicate identity;
- Patient ID is editable through a demographic-correction path;
- Visit creation creates/replaces Patient identity;
- a Visit is forced to have a Doctor before it can exist even though active Unassigned Visits are permitted;
- a Visit enters queue without both Doctor assignment and Paid/Waived eligibility;
- confirmed Paid is erased because a subsequent queue step failed;
- Doctor waiver request is only reachable from queue even though Unpaid Visits cannot enter queue;
- duplicate Pending waiver requests are created for one Visit;
- a stale waiver is approved after the Visit became Paid;
- Owner direct waiver creates a redundant self-approval step;
- a payment correction applies against a changed baseline;
- Paid-to-Unpaid correction is presented as refund or erases prior operational history;
- fee configuration silently rewrites an existing Visit amount;
- unknown Visit/payment outcome is blindly retried and duplicates records;
- assigned-but-Unpaid pre-queue Visit is counted/ordered as actual queue member;
- consultation starts from Waiting without explicit Called -> With Doctor;
- With Doctor/later Visit is reassigned or inserted with arbitrary priority;
- prior Doctor can decide Visit-linked demographic correction after reassignment;
- Unresponded history disappears when current state returns to Waiting;
- Waiting/Called Visit remains actively queued after effective state becomes Unpaid;
- queue history is deleted when current membership is removed for financial correction;
- Pending cancellation freezes workflow despite locked active behavior;
- cancellation is approved after Visit reached Completed;
- duplicate Pending cancellation requests are created;
- cancellation deletes prior clinical/pharmacy/financial history or implies refund;
- stale queue/cancellation action applies after state/assignment changed;
- clinical authoring is available before explicit With Doctor state;
- Save Draft completes consultation implicitly;
- stale Doctor draft save overwrites newer clinical content/state;
- consultation completes without required complaint/problem and assessment/diagnosis;
- completed consultation returns to ordinary editable mode;
- Possible Duplicate candidate histories are automatically merged;
- stale demographic-correction approval overwrites newer value/reviewer routing;
- Doctor direct demographic correction lacks old/new/actor/time history;
- completed consultation is destructively edited rather than amended;
- amendment reopens queue/payment/cancellation workflow;
- unfinished cancelled draft is treated as a completed consultation;
- active clinical writes continue after effective cancellation;
- non-Doctor authority exposes unrestricted clinical content;
- unfinalized prescription draft is treated as pharmacy-ready;
- consultation completion auto-finalizes prescription;
- prescription finalization auto-completes consultation;
- finalized prescription while With Doctor sends Visit to Pharmacy prematurely;
- prescription finalizes with incomplete required row data or unresolved quantity;
- reprint recalculates ** markers from current stock;
- Finalized prescription is edited in place;
- stale/non-current prescription version is replaced without refresh;
- prior dispensing/stock/billing is erased by replacement;
- Superseded prescription remains Pharmacy's silent default;
- active prescription work continues after effective Visit cancellation or Completed state;
- clinical amendment silently mutates prescription;
- implementation invents a no-prescription Consultation Completed -> Completed shortcut;
- Pharmacy dispenses a Finalized prescription before Visit reaches Sent to Pharmacy;
- historical/Superseded prescription becomes active dispensing source;
- Pharmacist sees unrestricted diagnosis/notes/Doctor longitudinal history;
- prescription replacement resets prior dispensed quantity;
- original plus substitute supply exceeds the permitted fulfilment allowance;
- stale multi-unit dispense exceeds current remaining allowance;
- dispensing record and stock deduction diverge;
- expired stock is dispensed;
- partial fulfilment creates a back-order/reservation;
- stale substitution survives replacement/cancellation/changed remaining quantity;
- dispensing continues after effective Visit cancellation;
- unsaved dispense quantities silently carry to another pharmacy unit;
- dispensing alone marks Visit Completed.


---

# 34. Role-Action Boundary Matrix

This section makes the authorization contract explicit at the interaction layer.

| Action | Reception | Doctor | Pharmacist | Administrator | Owner |
| --- | --- | --- | --- | --- | --- |
| Register/search patient | Yes | View as clinically needed | Lookup for dispensing | No routine workflow | Oversight only |
| Directly edit established demographics | No | Yes, audited | No | No | Only if also Doctor |
| Request demographic correction | Yes | N/A | No | No | Only through permitted role |
| Diagnose / clinical notes | No | Yes | No | No | Only if also Doctor |
| Finalize/replace prescription | No | Yes | No | No | Only if also Doctor |
| Approve medicine substitution | No | Yes | No | No | Only if also Doctor |
| Record consultation payment | Yes | No routine flow | No | No | Oversight / permitted correction approval |
| Request consultation waiver | Yes | Yes | No | No | May directly waive |
| Approve consultation waiver | No | No unless also Owner | No | No | Yes |
| Request Visit cancellation | No | Yes | No | No | Only if also Doctor |
| Approve Visit cancellation | No | No unless also Owner | No | No | Yes |
| Dispense medicine | No | No | Yes | No | No routine workflow |
| Record pharmacy payment | No | No | Yes | No | Oversight |
| Request pharmacy bill void | No | No | Yes | No | No routine request |
| Approve pharmacy bill void | No | No | No | No | Yes |
| Submit inventory adjustment | No | No | Yes | Configuration only | May control/approve |
| Approve inventory adjustment | No | No | No | No | Yes |
| Request stock transfer | No | No | Yes | No | May control/approve |
| Approve stock transfer | No | No | No | No | Yes |
| Create/disable non-Owner staff | No | No | No | Yes | Yes |
| Grant/revoke Owner role | No | No | No | **Never** | Yes |
| Disable Owner account | No | No | No | **Never** | Owner-authorized lifecycle only |
| View unrestricted full clinical content | No | Yes when clinically authorized | No | No | Only if also Doctor |

Rules:

1. **Administrator cannot grant or revoke Owner authority and cannot disable an Owner account.**
2. Owner authority does not substitute for Doctor authority.
3. Doctor authority does not substitute for Owner authority.
4. Hiding a control in the UI is not sufficient authorization; direct navigation/action must also be denied.
5. A multi-role user gains the union of explicitly assigned roles, while the active authority context remains visible and auditable.
6. Permission absence and temporary state unavailability are different: unauthorized actions are hidden/blocked; authorized-but-currently-invalid actions may be disabled with explanation.
7. A role revoked during an open session cannot remain usable merely because its page was already open.
8. Separate browser tabs/windows may use different permitted workspaces, but each must keep its authority context explicit.
9. If the same human performs separate workflow actions under different legitimate roles, audit history preserves the same human identity and the distinct authority used for each action.

---

# 35. Authentication and Credential-Recovery Interaction Contract

## 35.1 Authentication sequencing

The product treats authentication as a sequence of required gates rather than assuming password success always means normal workspace entry.

1. Validate username/login + password in the fixed clinic context.
2. If the account is disabled, block normal product entry.
3. If a non-Owner account used an Owner-reset credential, require SH-04 Forced Password Change.
4. If the account contains Owner authority and TOTP is not enrolled, require SH-05 initial enrollment.
5. If the account contains Owner authority and TOTP is enrolled, require SH-02 TOTP/recovery-code verification.
6. Only after all applicable gates succeed does the product apply G1 single-/multi-workspace entry.

An Owner-containing account cannot choose a Doctor or other workspace to bypass the Owner second-factor gate.

## 35.2 Normal workspace switching after authentication

Once the current session has satisfied all authentication requirements for its assigned roles, switching among already-permitted workspaces does not require another username/password login or repeated TOTP solely because of the switch.

If Owner authority is added to a session that did not previously contain it, this rule does not make the new Owner capability immediately usable. Owner second-factor satisfaction is required before Owner-capable access is exposed.

## 35.3 Forgot Password

The unauthenticated Forgot Password response is non-enumerating.

- no self-service password change;
- no indication that an arbitrary identifier exists;
- no indication that the account is disabled;
- no indication that the account contains Owner authority;
- only an eligible enabled non-Owner account creates/retains the actionable Pending Owner reset request;
- repeat submissions while Pending do not create duplicate simultaneously actionable requests.

Owner accounts do not enter the non-Owner staff-reset workflow.

## 35.4 Staff reset-request lifecycle

V1 uses a minimal lifecycle:

**Pending -> Resolved by successful Owner reset**

Owner action sets/replaces a temporary credential. It is not modeled as a generic Approve/Reject decision.

A resolved/stale request cannot be applied again as Pending.

Resetting the credential does not:

- modify roles;
- enable a disabled account;
- delete history.

## 35.5 Forced password replacement

A valid reset credential allows entry only into the mandatory replacement gate.

Until replacement succeeds:

- normal navigation is unavailable;
- workspace selector/switcher is unavailable;
- protected workflow content is unavailable.

After successful replacement:

- the temporary credential no longer authenticates;
- G1 normal workspace-entry behavior resumes.

## 35.6 Owner TOTP enrollment and recovery codes

Initial enrollment:

- occurs only after password authentication;
- requires verification with a generated TOTP;
- shows recovery codes only after successful factor verification;
- completes before normal workspace entry.

Recovery code:

- is an alternative second factor, not an alternative to the password;
- is single-use;
- is rejected when used/invalidated;
- is replaced as a set when recovery codes are regenerated.

Recovery-code regeneration is explicit and warns that the prior set becomes invalid.

## 35.7 Disabled account during an active session

Disabled account state overrides otherwise valid credentials/session state.

When disablement is detected during an already-open session:

- protected actions/navigation stop;
- the user returns to the sign-in boundary;
- re-enabling later requires a fresh normal sign-in rather than reviving the terminated session.

Exact propagation/session-revocation mechanics remain technical.

## 35.8 Authentication secret handling

Normal UI history/audit may show safe metadata for required events, such as:

- reset requester;
- target account;
- Owner actor;
- action;
- outcome/state;
- timestamp.

It must not show:

- current/old/new passwords;
- temporary/reset credential values;
- TOTP secret;
- TOTP codes;
- recovery-code values.

## 35.9 Explicitly deferred security controls

The PRD does not define:

- inactivity/session timeout;
- password-strength policy values;
- brute-force/rate-limit/lockout implementation;
- cryptographic credential/secret storage;
- exact cross-device live-session invalidation mechanism;
- catastrophic Owner recovery mechanism;
- general authenticator device replacement/migration beyond the specified enrollment/recovery-code behavior.

---

# 36. Patient Identity and Demographic-Correction Interaction Contract

## 36.1 Identity selection boundary

Search result confidence never silently becomes active patient context.

Even an exact Patient ID match requires an explicit **Select Patient** action before the workspace acts on that patient.

## 36.2 Possible Duplicate provenance

Possible Duplicate is an informational identity-risk marker, not a workflow lock.

When created from unresolved candidate review, preserve safe provenance sufficient for later understanding:

- candidate Patient IDs shown;
- creating actor;
- creation time.

Do not expose extra clinical detail merely because candidate provenance exists.

## 36.3 Established-demographic boundary

Reception may edit registration values until Patient creation succeeds.

After creation:
- Reception proposes correction;
- Doctor authority decides/applies correction;
- Patient ID is immutable.

## 36.4 Demographic correction state

A Reception-originated correction has:

- current value captured at request time;
- proposed value;
- Doctor routing/reviewer;
- status.

Normal states:
- Pending;
- Approved;
- Rejected;
- Stale when current value changed before decision.

Pending does not mutate the patient.

## 36.5 Doctor routing

- active Visit with Doctor -> route to current assigned Doctor;
- active Visit without Doctor -> require Doctor selection/assignment before submit;
- no active Visit -> choose authorized Doctor reviewer without creating a fake Visit.

If active Visit assignment changes before decision, the pending request follows the current assigned Doctor.

Visit completion does not silently discard an already-submitted patient-level correction request.

## 36.6 Stale correction safety

Before applying approval, compare the request's captured current value with the patient's current effective value.

If they differ:
- do not overwrite the newer value;
- mark/present the request as stale;
- refresh current data;
- require Doctor review again.

Exact version/concurrency implementation remains technical.

## 36.7 Doctor direct correction

Doctor may directly correct demographics where authorized.

Direct correction:
- acts on current patient state;
- preserves old/new value;
- records actor/time;
- never changes Patient ID.

## 36.8 Patient registration unknown outcome

Patient creation is identity-sensitive.

If the create outcome is unknown:
- do not automatically repeat it;
- first determine whether the Patient was already created;
- only allow a new create when current state proves the prior create did not succeed.

This requirement must later be reconciled with cross-product retry/idempotency rules in G14.

---

# 37. Visit, Consultation Payment, and Waiver Interaction Contract

## 37.1 Visit creation

Visit creation is a distinct state-changing action from queue entry.

On successful create:

- retain selected permanent Patient ID;
- generate Visit ID;
- capture Visit-specific consultation amount;
- set financial outcome to Unpaid;
- Doctor may remain Unassigned.

Possible Duplicate status or missing physical file never blocks this action.

## 37.2 Doctor assignment

Doctor may be assigned at Visit creation or later.

Doctor assignment is required before:
- queue entry;
- submitting a Visit-linked demographic correction when the active Visit was previously unassigned.

Do not create a Visit merely to obtain a Doctor reviewer for a patient-level correction when no active Visit exists.

## 37.3 Financial/queue state combinations

Valid pre-queue combinations include:

- Unpaid + Unassigned;
- Unpaid + Doctor assigned;
- Paid + Unassigned;
- Paid + Doctor assigned but not yet queued;
- Waived + Unassigned;
- Waived + Doctor assigned but not yet queued.

Only Paid/Waived + Doctor assigned is queue-entry eligible.

## 37.4 Waiver request

Waiver request exists only from Unpaid.

- require specific reason;
- one actionable Pending request per Visit;
- repeated submit while Pending does not duplicate;
- Pending does not change underlying Unpaid state;
- rejection leaves Unpaid;
- later new request after rejection is allowed while still Unpaid.

If payment becomes Paid while Pending:
- approval must be blocked;
- request is presented as stale/non-actionable;
- request history remains.

## 37.5 Doctor waiver access before queue

A Doctor assigned to an Unpaid Visit may request waiver from a separate assigned-pre-queue financial context.

This context:
- is not ordered queue;
- does not count as queue entry;
- does not expose consultation authoring solely due to assignment;
- exists to make the locked Doctor waiver-request authority reachable.

## 37.6 Owner direct waiver

Direct Waiver is not a request.

Require:
- current Unpaid Visit;
- amount;
- specific reason;
- explicit Owner-authority confirmation.

Success -> Waived immediately.

Do not create a second approval step.

## 37.7 Payment correction baseline

Every correction request is anchored to the current effective financial record that existed when submitted.

If that baseline changes before Owner decision:
- do not apply old proposal;
- show stale-state feedback;
- refresh current values.

Only one simultaneously actionable request against the same baseline should exist.

## 37.8 Non-destructive financial correction

Financial correction never erases historical Visit/queue/clinical events.

Before queue entry, corrected state changes future eligibility.

For current operational membership, apply the G5 stage rule:

- Waiting/Called corrected to Unpaid -> current queue membership may be removed while prior queue history remains;
- With Doctor or later -> do not unwind clinical workflow.

If a removed Visit later regains eligibility, explicit re-entry creates a new current queue-entry event instead of restoring the former queue position.

## 37.9 Combined action partial failure

A combined payment + queue UI does not justify hiding partial effective success.

If Paid is confirmed but queue entry fails:
- show Paid;
- show Not Queued;
- do not record Paid again;
- retry only queue entry after validating current state.

If payment is not confirmed:
- do not queue.

## 37.10 Fee configuration boundary

The applied Visit amount is a Visit-level record.

Changing clinic fee configuration does not rewrite existing Visit amounts.

A Visit-specific corrected amount, when legitimately needed, uses the controlled financial-correction path and does not alter global configuration.

---

# 38. Queue State, Reassignment, and Visit Cancellation Contract

## 38.1 Queue-state transition rules

Normal queue transition path:

**Waiting -> Called -> With Doctor**

Unresponded is a transient event from Called that repositions and returns to Waiting.

State-changing controls are only visible/enabled where the transition is currently valid.

## 38.2 Queue-entry history

Preserve queue-entry/reassignment/reposition/call events.

Reassignment, Unresponded, and move-to-end do not pretend the Visit newly arrived.

Actual removal from queue followed by later re-entry creates a new current queue-entry event while prior entries remain history.

## 38.3 Financial loss of eligibility before consultation

When a queued Waiting/Called Visit becomes effectively Unpaid:

- remove active queue membership;
- do not delete past queue history;
- show the Visit in pre-queue financial resolution;
- renewed eligibility requires explicit re-entry at end.

If With Doctor or later, do not rewind clinical progression.

## 38.4 Cancellation request

Cancellation request is Doctor-authority action with mandatory reason.

- one Pending request per Visit;
- Pending does not freeze workflow;
- current Visit state continues independently;
- request remains visible to authorized users.

If state reaches Completed before Owner decision, the request is stale/non-actionable.

## 38.5 Cancellation decision

Owner revalidates request and current Visit state.

Approve only while current state is one of the locked cancellable states.

Approval:
- Cancelled/Voided;
- leaves active workflow;
- preserves every existing historical record;
- does not create refund.

Reject:
- leaves current state unchanged;
- preserves request/decision history.

## 38.6 Concurrent workflow changes

Queue/cancellation actions depend on current state/assignment.

If another user/session:
- reassigns Visit;
- starts consultation;
- marks Unresponded;
- moves Visit;
- completes Visit;
- resolves cancellation;

then stale action from old view must not apply.

## 38.7 Cancellation after downstream work exists

Cancellation is non-destructive even after clinical/pharmacy artifacts exist.

Later group reviews must ensure:
- Doctor workspace stops future active authoring when cancellation becomes effective;
- Pharmacy does not continue future active Visit fulfilment after cancellation;
- already-created clinical/prescription/dispensing/billing history remains viewable to authorized roles.

---

# 39. Clinical Record, Demographic Review, and Amendment Contract

## 39.1 Current consultation record

The current editable clinical draft is Visit-specific and Doctor-specific.

A successful draft save records the current content against the same Patient ID + Visit ID and does not create a new Visit or completed revision.

## 39.2 Concurrent draft safety

State-changing clinical save/complete depends on current Visit state, assignment, and current saved baseline.

A stale Doctor session must refresh rather than overwrite:
- newer saved draft;
- new Doctor assignment;
- effective cancellation;
- completed consultation.

## 39.3 Demographic-correction tasks

Doctor workspace exposes requests assigned to that Doctor.

Decision view shows:
- Patient ID;
- Visit ID when applicable;
- field;
- captured current value;
- proposed value;
- requester/time.

Approve/reject revalidates current field value and reviewer authority.

Patient-level request without active Visit remains patient-level; do not create a Visit.

## 39.4 Direct Doctor demographic correction

Direct correction is separate from approving a Reception request.

Show current -> proposed value.

On success record old/new/Doctor/time.

Patient ID is never editable.

Stale loaded value blocks overwrite.

## 39.5 Clinical revision model

Only a completed consultation enters the amendment revision model.

Revision chain:
- original completed record;
- amendment 1;
- amendment 2;
- etc.

Exactly one revision is the current effective clinical record; all prior revisions remain historical/read-only.

## 39.6 Amendment after Visit closure

Amending a completed clinical record after Completed or Cancelled/Voided:
- changes only the effective clinical record revision;
- does not reopen Visit;
- does not alter queue/payment/cancellation/pharmacy state.

Saved draft from a consultation cancelled before completion remains historical but is not treated as a completed record eligible for this amendment flow.

## 39.7 Cancellation concurrency

If cancellation becomes effective before clinical save/complete:
- current Visit state wins;
- stale clinical write is rejected;
- already-saved content remains.

If completion becomes effective first:
- completed clinical record remains;
- subsequent valid cancellation preserves it.

## 39.8 Clinical-content access

Full clinical content requires Doctor authority and authorized patient/Visit context.

Non-Doctor operational/audit views may show safe event metadata without exposing full clinical content.

---

# 40. Prescription Version, Pharmacy-Readiness, and Cancellation Contract

## 40.1 Independent clinical and prescription readiness

Two independent prerequisites exist for pharmacy handoff:

1. consultation is completed;
2. a current finalized prescription exists.

When both are true, current Visit state becomes Sent to Pharmacy.

The event that satisfies the second prerequisite triggers the readiness transition.

This does not imply that Complete Consultation finalizes prescription or that Finalize Prescription completes consultation.

## 40.2 Prescription draft states

Unfinalized draft:
- Visit-specific;
- non-dispensable;
- editable in With Doctor or Consultation Completed;
- not equivalent to Finalized.

Finalized:
- immutable ordinary view;
- current dispensing source only when Visit is pharmacy-ready.

Superseded:
- historical;
- never default current dispensing source.

## 40.3 Finalization snapshot

Before finalization:
- refresh availability;
- validate complete rows/quantity/current Visit/current draft.

On finalization store:
- prescription version identity;
- Doctor/time;
- full item data;
- clinic-wide availability snapshot used for ** markers.

## 40.4 Finalized before consultation completion

A Doctor may finalize while Visit is With Doctor.

The finalized prescription exists and may be reviewed/printed by Doctor, but the Visit remains With Doctor and Pharmacy must not treat it as pharmacy-ready until consultation is completed.

## 40.5 Consultation completed before finalization

If consultation completes first, Visit remains Consultation Completed until a current finalized prescription exists.

Doctor workspace exposes that Visit as an awaiting-prescription task rather than putting it back in the Doctor queue.

## 40.6 Replacement lineage

Replacement uses the current active Finalized version as its baseline.

On success:
- old version -> Superseded;
- new version -> Finalized/current;
- mandatory reason/Doctor/time/link retained.

A stale baseline cannot be replaced silently.

## 40.7 Prior dispensing

Replacement does not reverse physical or recorded prior dispensing.

All prior dispensing remains attributable.

Pharmacy later computes subsequent allowable dispensing from the active corrected prescription plus preserved prior dispensing history.

## 40.8 Cancellation and Completed boundary

Effective cancellation blocks new active prescription work but preserves existing prescription history.

Completed Visit does not gain a new active prescription/replacement workflow in V1.

Pending cancellation alone does not freeze valid prescription work.

## 40.9 No-prescription bypass

The locked V1 workflow does not define a direct no-prescription bypass from Consultation Completed to Completed.

Do not invent such a state transition in implementation without approved business change control.

## 40.10 Clinical amendment isolation

A clinical amendment changes clinical revision only.

It never silently edits/finalizes/replaces prescription.

Prescription correction during an active Visit uses the explicit replacement workflow.

---

# 41. Pharmacy Retrieval, Fulfilment Lineage, and Dispense-Safety Contract

## 41.1 Pharmacy-ready truth

Pharmacy dispensing truth is the conjunction of:

- current Visit state = Sent to Pharmacy;
- current prescription version = Finalized/current;
- active permitted pharmacy unit.

Any stale copy of those facts is non-authoritative.

## 41.2 Historical prescription visibility

Pharmacy may inspect previous/Superseded prescriptions and known allergies for permitted fulfilment context.

Historical prescription visibility never grants:
- unrestricted diagnosis/notes;
- dispensing from Superseded version;
- editing/finalizing prescription.

## 41.3 Fulfilment lineage across replacement

A carried-forward prescription item keeps its fulfilment lineage so prior dispensing counts against corrected/current allowed quantity.

New materially different medicine identity begins a new lineage.

Removed lineages remain historical and non-dispensable.

## 41.4 Reduced corrected quantity below prior dispense

Replacement is not reversed merely because prior dispensing exceeds the newly corrected quantity.

Instead:
- historical dispensing remains;
- remaining allowable becomes 0;
- no further supply for that lineage;
- warning/audit exposes the mismatch.

## 41.5 Multi-session/unit concurrency

Before each dispense, derive remaining allowance from current committed history across all units.

A quantity visible earlier is not a reservation.

If another unit/session supplied first, reject stale excess and refresh.

## 41.6 Dispense transaction outcome

Successful dispense creates:
- attributable dispensing record;
- unit-specific stock deduction;
- billable supplied quantity for later G9 processing.

Those outcomes correspond to the same effective dispense operation.

Do not create a stock deduction without its dispensing record or vice versa.

## 41.7 Substitute fulfilment

Doctor-approved substitution authorizes only the approved proposal.

Substitute supply is tied back to:
- original prescription item;
- substitution decision;
- current prescription version;
- actual pharmacy unit;
- actual supplied quantity.

It consumes fulfilment allowance rather than creating an unrelated extra allowance.

## 41.8 Visit cancellation

Effective cancellation:
- blocks future dispense;
- leaves all prior dispense/stock/billing history intact;
- does not auto-restock.

## 41.9 Visit completion ownership

Dispensing alone does not mark Visit Completed.

G9 determines billing/payment and final pharmacy workflow completion.
