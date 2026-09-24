# Hospital CRM — Interaction and Form Behavior Specification

## Document Control

| Field | Value |
| --- | --- |
| Document | Interaction and Form Behavior Specification |
| Version | 0.7 |
| Status | DRAFT — PRD companion |
| Date | 2026-09-20 |
| Parent | PRD v0.7 |
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

Show:

- payment/Visit identity;
- captured current baseline;
- proposed corrected record;
- mandatory reason.

Supported corrected fields may include effective Paid/Unpaid state, method, Other description, reference, and Visit-specific recorded amount.

If proposed state is Paid, full-payment method rules apply.

Before Owner applies a correction, revalidate the captured baseline. A changed baseline makes the old request stale/non-applicable until refreshed review.

Do not use payment correction to silently create/revoke Waived.

## 8.12 Unknown outcome / retry

For Visit creation or Paid recording with unknown outcome:

- do not blindly resubmit;
- retrieve current Visit/payment state;
- recover the already-created state if present;
- only allow a new state-changing attempt once prior outcome is known safe.

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

**Bill void:** explicitly say inventory will not be restored.  
**Inventory adjustment:** show stock before/proposed after.  
**Transfer:** show source and destination.  
**Payment correction:** show captured baseline plus original/proposed financial fields; re-check baseline before applying.  
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

## 12.1 Required vs optional

Required:

- chief complaint / patient problem;
- assessment / diagnosis.

Optional:

- symptoms/history;
- examination;
- notes;
- advice;
- follow-up.

## 12.2 Clinical history

Historical content opens read-only.

## 12.3 Completion

Consultation completion is an explicit action.

If required content is missing, completion is blocked with field-level errors.

## 12.4 Amendment

Completed consultation cannot re-enter ordinary edit mode.

Use amendment flow with reason.

---

# 13. Medicine Search and Prescription Row Pattern

## 13.1 Search result identity

Medicine result should present:

- display name;
- strength;
- dosage form;
- manufacturer where helpful;
- generic/molecule where available;
- availability.

## 13.2 Availability

Availability is informational during prescribing.

Out of Stock/Not Stocked does not disable Add to Prescription.

## 13.3 Multi-pharmacy view

If multiple units exist:

- show clinic total/overall availability;
- expose unit-level availability without giving Doctor inventory-edit controls.

## 13.4 Prescription row

Each unfinalized row supports:

- dose amount;
- frequency;
- duration;
- optional timing/food/instruction;
- quantity.

## 13.5 Quantity

If auto-calculated, show the result as system-calculated and allow only behavior consistent with the locked quantity rule.

If the system cannot calculate deterministically, require Doctor quantity input.

## 13.6 Finalization

Finalization freezes the prescription version.

Any later correction uses replacement.

---

# 14. Printed Prescription Pattern

## 14.1 ** marker

Only medicines known as Out of Stock/Not Stocked at prescription finalization receive **.

## 14.2 Partial dispensing later

A later partial quantity does not alter the original printed prescription.

The pharmacy output communicates actual supplied/unsupplied quantity.

## 14.3 Reprint

Reprint does not create a new prescription version.

---

# 15. Dispensing Interaction Pattern

## 15.1 Quantity model

Per item show:

- prescribed total;
- already dispensed;
- remaining allowable;
- current unit availability;
- quantity to dispense.

## 15.2 Upper bound

The entered quantity cannot exceed:

- remaining allowable prescription quantity; or
- available valid stock.

## 15.3 Expired stock

Expired stock is not selectable as dispensable stock.

## 15.4 Partial fulfilment

If less is supplied:

- show supplied quantity;
- show unsupplied remainder;
- bill supplied quantity only.

## 15.5 Substitution

A requested substitute is not treated as dispensable until Doctor approval.

## 15.6 Multi-pharmacy

Every dispense shows/records pharmacy unit.

Cumulative prescription limit spans units.

---

# 16. Pharmacy Billing Pattern

## 16.1 Bill source

The bill is generated from actual supplied items, not all prescribed items.

## 16.2 Price transparency

Each line shows the price basis needed to understand the charge.

## 16.3 Payment

Use the same UPI/Cash/Card/Other pattern.

## 16.4 Void

Void is a request, not direct destructive action.

The request dialog must state:

- bill will leave active billing if approved;
- prior bill/payment history remains;
- stock will not automatically be restored.

---

# 17. Inventory Quantity and Unit Pattern

## 17.1 Base unit

Inventory calculations use configured base unit.

## 17.2 Package display

Higher packaging may be used for human entry/display where configuration exists.

## 17.3 Conversion visibility

When an entered package quantity converts to base units, show the resulting base quantity before a material adjustment/transfer request is submitted.

## 17.4 Manual change

Pharmacist sees current stock and proposed delta/result before submitting request.

## 17.5 Negative stock prevention

Do not allow a request to present an impossible resulting source quantity without explicit validation feedback.

The exact concurrency implementation is technical.

---

# 18. Inventory Table Pattern

Recommended columns vary by screen, but the inventory table should support:

- medicine;
- pharmacy unit;
- available stock;
- base/package representation;
- nearest expiry;
- low-stock/out-of-stock state;
- action appropriate to role.

Filters:

- low stock;
- out of stock;
- near expiry;
- expired;
- medicine search.

Owner consolidated views must allow drill-down to the unit-level ledger.

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
- stale queue/cancellation action applies after state/assignment changed.


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
