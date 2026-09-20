# Hospital CRM — Interaction and Form Behavior Specification

## Document Control

| Field | Value |
| --- | --- |
| Document | Interaction and Form Behavior Specification |
| Version | 0.2 |
| Status | DRAFT — PRD companion |
| Date | 2026-09-20 |
| Parent | PRD v0.2 |
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

A valid exact Patient ID should be visually prioritized because it is the unique patient identifier.

## 5.3 Phone results

A phone search may return multiple patients.

Result design must make shared-phone cases understandable.

## 5.4 Name similarity

Name similarity may surface candidates.

The UI must describe them as candidates, not “the patient,” until Reception/patient confirms identity.

## 5.5 No auto-merge

No interaction, score, or duplicate warning can auto-merge patient records in V1.

## 5.6 New patient action

When search is ambiguous, “Register New Patient” remains deliberate and visually separate from selecting a candidate.

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

## 6.2 Optional group

Optional fields should be visually secondary:

- emergency contact;
- blood group;
- known allergies;
- guardian/parent;
- Government ID.

## 6.3 Possible Duplicate warning

If candidate records were surfaced before new registration, keep a visible warning/context so Reception understands why the new profile may receive Possible Duplicate.

The warning must not block the user when the patient cannot confidently identify an existing profile.

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

## 8.2 Other

When Other is selected:

- show a required “Payment method” description field;
- do not allow final payment confirmation until it is completed.

## 8.3 Reference

Payment reference/transaction number is optional.

Do not force it for UPI or Card.

## 8.4 Final action

Use an explicit action such as **Mark Paid** / **Mark Paid & Continue**.

Selecting UPI, Cash, Card, or Other must never mark a payment Paid by itself. Payment becomes Paid only after the user performs the explicit final payment-confirmation action.

## 8.5 Unpaid

Unpaid remains a legitimate recorded state.

For consultation, Unpaid remains queue-blocked unless Owner-approved Waived.

## 8.6 No partial payment UI

Do not expose amount-paid/amount-due split controls for consultation or pharmacy V1.

## 8.7 No refund UI

Do not expose refund action in V1.

## 8.8 Payment correction

Recorded payment correction uses request/approval, not direct overwrite.

The correction request must show:

- original state;
- proposed state;
- reason.

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
**Payment correction:** show original/proposed financial state.  
**Visit cancellation:** show current Visit state and that history remains.  
**Waiver:** show fee/payment status and queue eligibility effect.

---

# 11. Queue Interaction Pattern

## 11.1 Queue organization

Doctor-specific queues remain visually separated.

If tabs/filters are used, the selected Doctor must stay visible.

## 11.2 Position

Queue position is a display of current ordering, not an editable free-form number.

## 11.3 Call

Doctor Call Patient is explicit.

Reception sees the call state/notification.

## 11.4 Unresponded

Reception marks Unresponded only from Called.

After the rule executes:

- event remains in history;
- Visit returns to Waiting;
- new position is visible.

## 11.5 Reassign

Reassign requires selecting destination Doctor.

Show current Doctor and destination before confirmation.

## 11.6 Urgent case

Do not create a priority star, emergency reorder button, or hidden priority score in V1.

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

# 26. Permission-Denied Pattern

If a user reaches a forbidden function:

- do not render sensitive content and merely disable the button;
- show safe access-denied messaging;
- provide path back to permitted workspace.

Direct navigation must not bypass role rules.

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

# 29. Multi-Role Interaction Contract

A user with Owner + Doctor:

- logs in once;
- completes Owner-required TOTP;
- can switch Owner/Doctor workspace;
- performs clinical actions in Doctor context;
- performs financial/inventory approvals in Owner context.

Do not create two separate user identities.

Do not silently infer authority from whichever screen was last open.

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
- history looks editable/current when it is not.


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
