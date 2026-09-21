# 04 — User stories: amendments and new stories

**Session:** Field Sales Management System design, 21 Sep 2026
**Derived from:** decisions settled in session, recorded in `01-tablet-day.md` (T7.1, T7.6–T7.12, T2.5) and `02-head-office.md` (H1.6, H1.7, H2.6–H2.8)
**Scope:** only decisions confirmed in conversation. Drafting calls in 00–03 that haven't been confirmed (e.g. T2.1 Today above Overdue, H1.1 Type column) are not turned into stories here.
**Numbering:** the source area documents are under `plan_docs/stories`. New cross-area stories retain their `US-NEW-###` placeholders until they are folded back into those sources; amendments reference the source story IDs.

---

## Summary

| ID | Type | Title | Surface |
|---|---|---|---|
| BR-NEW-001 | Business rule | Automatic order disposition | Head office |
| BR-NEW-002 | Business rule | Rep discount allowance and promotion interaction | Tablet, head office |
| BR-NEW-003 | Business rule | Free of charge on discontinuing stock only | Tablet |
| BR-NEW-004 | Business rule | Structured Visit Due Reason Type | Rep website, manager website, tablet |
| US-NEW-001 | New story | Quantity popover when adding a Low item | Tablet (T-06, T-07) |
| US-NEW-002 | New story | Low tab as a catch-up list | Tablet (T-07) |
| US-NEW-003 | New story | Rep prompted when a line couldn't be supplied | Tablet (T-02, T-08) |
| US-NEW-004 | New story | Out-of-pattern quantity marker on Review | Tablet (T-07) |
| US-NEW-005 | New story | Orders processed without head office acceptance | Head office (H-01, H-02) |
| US-NEW-006 | New story | Manage Visit Due Reason Types | Manager/admin website |
| Pricing US-008 *(confirm)* | Amendment | Price override applied within allowance | Tablet |
| Pricing US-009 *(confirm)* | Amendment | Free of charge restricted and applied | Tablet |
| Promotions US-007 *(confirm)* | Amendment | Spend threshold qualifies before rep discounts | Tablet, customer |
| Visit Planning US-003 | Clarification | Planner horizon, views, responsive panel and Due Reason disclosure | Rep website (R-01) |
| Visit Planning US-004, US-005 | Clarification | Drag uses default duration; click scheduled visit to amend | Rep website (R-01) |
| Visit Planning US-009 | Clarification | Review-and-select bulk digest actions | Rep website (R-02) |
| Targets & Performance US-004 | Amendment | Targeted Locations ordered by greatest target shortfall | Rep website (R-03) |
| Stock Allocation US-003, US-005 | Clarification | Manager-selected stock pools when re-proposing | Head office (H-10) |
| Area 1 US-014 | Amendment | Sent item shows removed lines and applied prices | Tablet (T-08) |
| Head Office US-001, US-002 | Superseded in part | Worklist no longer holds orders | Head office |
| Head Office US-003 | Superseded | Order detail becomes a record | Head office |

---

## Business rules

### BR-NEW-001 — Automatic order disposition

Orders are never accepted by a person. Each order flag has one disposition:

| Disposition | Flags | Behaviour |
|---|---|---|
| Route | Stock Shortfall, Oversold | Order accepted; short quantity becomes outstanding and goes to allocation (H-10). |
| Auto-resolve | Unavailable Line | Order accepted; the unavailable line is removed with its reason recorded; the rep is prompted (US-NEW-003). |
| Annotate | Large, Watched Product, Rep-flagged, New Location, Prospect Conversion | Recorded on the order. No human step. |
| Divert | *(none)* | Price Override and Free of Charge no longer divert, because BR-NEW-002 and BR-NEW-003 enforce limits at capture. |

### BR-NEW-002 — Rep discount allowance and promotion interaction

A rep may lower a line's price by up to an allowance, expressed as a percentage and configured by a manager through a commercial policy profile. Assigning a product to an eligible profile makes that policy available on the tablet; products outside a policy cannot be discounted by the rep. An override inside the allowance is applied, not requested.

1. **Base price** is the best non-promotional price: tier, list or quantity break. Quantity breaks are bulk pricing, not an offer.
2. **No promotion on the line** — the rep can go down to the allowance off the base price.
3. **Promotion no larger than the allowance** — the rep's discount stacks on the promotion price.
4. **Promotion larger than the allowance** (measured as head office set it, against tier or list) — the rep can't stack on it, but can still go down to the allowance off the base price if that is lower than the promotion. Only if the promotion is lower than that too is the line blocked.
5. **Buy X get Y, bundle, mix and match** — lines in these promotions never take a rep discount, whatever the promotion's size.
6. **Spend threshold** — whether the order qualifies is checked on resolved prices, before rep discounts. *Assumed:* the threshold discount then applies to the order total as it finally stands (RC-NEW-006).

**Rationale:** rule 4's fallback keeps the system monotonic — running a promotion never lowers the best price a rep can reach. Rule 5 is the one accepted exception, traded for a rule a rep can state in one sentence.

**Note for wherever the allowance is set:** because of rule 3, the effective maximum discount is nearly twice the allowance (a 9% promotion plus a 10% rep discount is about 18% off).

**Data:** the applicable policy rules and product membership must be in the tablet snapshot (brief §11). "Profile" is a working model, not yet a settled entity: it may become explicit business rules and must not be conflated with the catalogue's existing one-per-product Product Profile without resolving that model first.

### BR-NEW-003 — Free of charge on discontinuing stock only

1. A free-of-charge line can only be a product in the Discontinuing state.
2. A manager assigns eligible products to a commercial policy profile and sets an FOC quantity allowance per rep per calendar month.
3. Products outside an eligible policy cannot be added free of charge. Inside the rep's remaining monthly allowance, the line is applied; no head office decision.
4. An FOC item is an ordinary order line whose resolved price is €0.00. Stock, availability, allocation, despatch and removal behave exactly as for any other product line.
5. The quantity counts against the rep's monthly allowance while the order line exists. Reducing or removing the line releases that quantity. If processing removes it through the Unavailable Line path (BR-NEW-001), the returned allowance reaches the tablet at Sync.

**Data:** the applicable policy, product membership, rep's month-to-date FOC use and remaining allowance must be in the tablet snapshot. The tablet derives its offline balance from the snapshot plus its local order lines.

### BR-NEW-004 — Structured Visit Due Reason Type

1. A Visit Due may continue to have no Due Reason.
2. When a manager supplies a Due Reason, they choose a structured Reason Type and may enter the free-text explanation carried by the existing model.
3. The manager/admin chooses the Reason Type's icon and colour from a controlled system set. The type supplies stable icon, colour, filter and reporting semantics; the explanation supplies the visit-specific context.
4. Colour is never the only indicator: the UI pairs it with the type icon and an accessible type label.
5. An authorised manager or administrator maintains the Reason Type vocabulary.
6. The Reason Type and explanation travel with the Visit Due to the rep website and tablet snapshot.

**Presentation:** colour is never offered as a free-form value and never acts as the only signal.

---

## New stories

### US-NEW-001: As a Field Salesperson, I want to enter a quantity when I add a Low item to the order so that the line reflects what the customer just agreed, not a number the system guessed.

> **Context:** extends the Low-marking behaviour in Area 1 US-006 to US-011 *(confirm which)* and order entry in Area 1 US-012. Suggested quantities were considered and dropped: the data model holds a low-stock *threshold*, not a target level, and a pre-filled number anchors the rep.

**Acceptance Criteria:**

**AC-NEW-001-1:**
- **Given** I have ticked Low on a stock-check line for an orderable product
- **When** I tap Add to order
- **Then** a quantity popover opens showing the product name, the count I recorded, the resolved price with its source, and an empty quantity field

**AC-NEW-001-2:**
- **Given** the quantity popover is open
- **When** I enter a quantity that brings a quantity break into reach
- **Then** the break prompt appears beneath the price and updates as I type

**AC-NEW-001-3:**
- **Given** I have entered a valid quantity
- **When** I tap Add to order
- **Then** the line is added to the order at that quantity and the item shows as ADDED with its quantity on the Low tab

**AC-NEW-001-4:**
- **Given** the quantity popover is open
- **When** I tap Cancel
- **Then** nothing is added and the item remains not added

**AC-NEW-001-5:**
- **Given** the product is measure-based
- **When** I enter a quantity below the minimum or off the step
- **Then** the existing measure validation message is shown and Add to order is unavailable until corrected

**AC-NEW-001-6:**
- **Given** an item on the Low tab is not added or REMOVED
- **When** I tap Add
- **Then** the same quantity popover opens

**AC-NEW-001-7:**
- **Given** any route into the quantity popover
- **When** it opens
- **Then** the quantity field is empty — it is never pre-filled

**Recommended Acceptance Tests:**

- `Should_OpenQuantityPopover_When_AddToOrderTappedOnLowItem`
  → Verifies Add to order on a Low line opens the popover rather than adding immediately.
- `Should_OpenWithEmptyQuantity_When_PopoverOpens`
  → Verifies no suggested quantity is ever pre-filled.
- `Should_UpdateBreakPromptLive_When_QuantityApproachesBreak`
  → Verifies the break prompt reacts to the typed quantity.
- `Should_AddLineAndMarkAdded_When_QuantityConfirmed`
  → Verifies the line is added and the Low tab state becomes ADDED.
- `Should_LeaveItemNotAdded_When_PopoverCancelled`
  → Verifies cancel has no side effects.
- `Should_UseSamePopover_When_AddTappedOnLowTab`
  → Verifies both entry points share one component.

---

### US-NEW-002: As a Field Salesperson, I want the Low tab to show what happened to every item I marked Low so that I can see at a glance whether anything the customer is short of hasn't made it onto the order.

> **Context:** extends order entry, Area 1 US-012 / US-013. Most Low items are added at tick-time (US-NEW-001); the Low tab is a catch-up list for anything skipped. Presented as a tab beside Order pad and Search all.

**Acceptance Criteria:**

**AC-NEW-002-1:**
- **Given** the order follows a stock check with items marked Low
- **When** I open the Low tab
- **Then** every Low item is listed with exactly one state: not added; ADDED with its quantity; REMOVED; REPLACED naming the replacement and its quantity; or CAN'T ADD with the availability reason

**AC-NEW-002-2:**
- **Given** the Low tab is shown in the tab strip
- **When** any items are not added or CAN'T ADD
- **Then** the tab label shows a count of those items only; ADDED, REMOVED and REPLACED are not counted

**AC-NEW-002-3:**
- **Given** a Low item was added to the order
- **When** I remove its line from the order
- **Then** the item shows REMOVED with an Add action

**AC-NEW-002-4:**
- **Given** I marked Low on an unavailable product and added a replacement
- **When** I open the Low tab
- **Then** the item shows REPLACED with the replacement product and quantity

**AC-NEW-002-5:**
- **Given** a Low item is unorderable and no replacement was added
- **When** I open the Low tab
- **Then** it shows CAN'T ADD with the reason and a Find replacement action, and no quantity control

**AC-NEW-002-6:**
- **Given** there are several not added items
- **When** I view the Low tab
- **Then** there is no action that adds them all at once

**AC-NEW-002-7:**
- **Given** a Low item's product is on the order
- **When** I untick Low on the stock-check line
- **Then** the order line is unchanged

**Recommended Acceptance Tests:**

- `Should_ShowExactlyOneState_When_LowTabListsItem`
  → Verifies every item resolves to one of the five states.
- `Should_CountOnlyNotAddedAndCantAdd_When_LowTabLabelRendered`
  → Verifies the tab count excludes handled items.
- `Should_ShowRemoved_When_AddedLineRemovedFromOrder`
  → Verifies a deliberate removal is distinguished from never-added.
- `Should_ShowReplacementDetail_When_ReplacementAdded`
  → Verifies REPLACED names what went in instead.
- `Should_HideQuantityControl_When_ItemCantBeAdded`
  → Verifies unorderable items offer Find replacement only.
- `Should_NotOfferAddAll_When_MultipleItemsNotAdded`
  → Verifies there is no bulk add.
- `Should_KeepOrderLine_When_LowUnticked`
  → Verifies unticking Low doesn't remove an order line.

---

### US-NEW-003: As a Field Salesperson, I want to be told on Home when a line was removed from one of my orders so that I can tell the customer before their delivery arrives short.

> **Context:** follows from BR-NEW-001 auto-resolve. For most shops the rep is the only channel to the customer, and the order total has changed since the rep quoted it. Touches T-02 (Area 1 US-003) and T-08 (Area 1 US-014).

**Acceptance Criteria:**

**AC-NEW-003-1:**
- **Given** a line on one of my orders was removed because the product was unavailable
- **When** the tablet next syncs
- **Then** Home shows a "not supplied" count in the exception strip alongside the unsent, cycle-decision and conflict counts

**AC-NEW-003-2:**
- **Given** the "not supplied" count is above zero
- **When** I tap it
- **Then** I see each affected order with its location, the removed line or lines, and the reason

**AC-NEW-003-3:**
- **Given** I open an affected order
- **When** T-08 renders
- **Then** the removed line reads "Not supplied — [reason]. Removed from the order. Let the customer know."

**AC-NEW-003-4:**
- **Given** no lines have been removed from my orders
- **When** Home renders
- **Then** no "not supplied" count is shown

**Recommended Acceptance Tests:**

- `Should_ShowNotSuppliedCount_When_LineAutoRemovedAfterSync`
  → Verifies the prompt appears after the sync that brings the removal down.
- `Should_ListAffectedOrdersWithReason_When_CountTapped`
  → Verifies the count leads to the orders and reasons.
- `Should_PromptToTellCustomer_When_SentItemHasRemovedLine`
  → Verifies T-08 wording on the removed line.
- `Should_HideCount_When_NoLinesRemoved`
  → Verifies the strip doesn't show a zero counter.

> **Open:** what clears the count (RC-NEW-001).

---

### US-NEW-004: As a Field Salesperson, I want lines with an unusually high quantity for this shop marked when I review an order so that I catch a mistyped quantity before it's sent.

> **Context:** extends Area 1 US-012 / US-013. With no human acceptance at head office (BR-NEW-001), Review is the last point a slip (48 typed as 480) can be caught. Large stays an annotate flag.

**Acceptance Criteria:**

**AC-NEW-004-1:**
- **Given** a line's quantity is well above what this location ordered of that product on its last 3 accepted orders
- **When** I view Review order
- **Then** the line shows "Usually about [N] here"

**AC-NEW-004-2:**
- **Given** the location has no accepted order containing the product
- **When** I view Review order
- **Then** no marker is shown on that line

**AC-NEW-004-3:**
- **Given** a line's quantity is well below the location's usual
- **When** I view Review order
- **Then** no marker is shown

**AC-NEW-004-4:**
- **Given** one or more lines are marked
- **When** I tap Mark Ready to Send
- **Then** the order is marked Ready to Send without any confirmation or acknowledgement step

**AC-NEW-004-5:**
- **Given** the tablet has no connection
- **When** I view Review order
- **Then** markers are still calculated, from the order history already in the snapshot

**Recommended Acceptance Tests:**

- `Should_MarkLine_When_QuantityWellAboveLastThreeOrders`
  → Verifies the marker and its "usually about" figure.
- `Should_NotMarkLine_When_NoOrderHistoryForProductAtLocation`
  → Verifies first-time products are never marked.
- `Should_NotMarkLine_When_QuantityBelowUsual`
  → Verifies high-only behaviour.
- `Should_NotBlockReadyToSend_When_LinesMarked`
  → Verifies the marker is a non-blocking fallback.
- `Should_CalculateMarkerOffline_When_NoConnection`
  → Verifies it uses snapshot data.

> **Note:** the threshold for "well above" is an implementation detail, not a design decision.

---

### US-NEW-005: As a Head Office user, I want orders that are within policy to go through without my acceptance so that my worklist only holds things that genuinely need a decision.

> **Context:** replaces the order-decision parts of Head Office US-001 to US-003. Implements BR-NEW-001.

**Acceptance Criteria:**

**AC-NEW-005-1:**
- **Given** a synced order has no flags, or only annotate flags
- **When** it is received
- **Then** it is accepted and released to the warehouse without a human step, and its flags are recorded on the order

**AC-NEW-005-2:**
- **Given** a synced order has a line flagged Stock Shortfall or Oversold
- **When** it is received
- **Then** the order is accepted, what's available is released, and the short quantity becomes outstanding and appears in allocation (H-10)

**AC-NEW-005-3:**
- **Given** a synced order has a line that is now Unavailable
- **When** it is received
- **Then** the order is accepted, that line is removed with its reason recorded, and the capturing rep is prompted (US-NEW-003)

**AC-NEW-005-4:**
- **Given** I open the worklist
- **When** it renders
- **Then** it contains only range proposals, duplicate matches and customer account requests, each labelled with its type

**AC-NEW-005-5:**
- **Given** there are no range proposals, duplicate matches or account requests
- **When** I open the worklist
- **Then** it reads "Nothing needs a decision." with no action offered

**AC-NEW-005-6:**
- **Given** I open an order's detail
- **When** H-02 renders
- **Then** it shows the lines as captured, any line removed automatically with its reason, applied rep prices and free-of-charge lines with their working, the annotations as plain sentences, and "Total as captured" and "Total accepted" labelled separately

**AC-NEW-005-7:**
- **Given** I am viewing an order's detail
- **When** I look for decision controls
- **Then** there are no Accept, Partial release or per-line override and free-goods decisions

**Recommended Acceptance Tests:**

- `Should_AcceptAndRelease_When_OrderHasOnlyAnnotateFlags`
- `Should_RouteShortQuantityToAllocation_When_LineShort`
- `Should_RemoveLineAndPromptRep_When_LineUnavailable`
- `Should_ExcludeOrders_When_WorklistRendered`
- `Should_ShowGoodNewsEmptyState_When_NothingNeedsDecision`
- `Should_ShowBothTotals_When_LineRemovedAutomatically`
- `Should_OfferNoDecisionControls_When_OrderDetailViewed`

---

### US-NEW-006: As an authorised manager or administrator, I want to maintain Visit Due Reason Types so that planners use consistent reasons that can be recognised, filtered and reported.

> **Context:** extends Visit Planning's free-text Due Reason with a managed structured type (BR-NEW-004). The manager/admin chooses its icon and colour from a controlled system set.

**Acceptance Criteria:**

**AC-NEW-006-1:**
- **Given** I am authorised to maintain Reason Types
- **When** I create a type with a unique name
- **Then** it becomes available when a manager supplies a Due Reason

**AC-NEW-006-2:**
- **Given** a Reason Type is referenced by existing Visit Dues
- **When** I rename it
- **Then** those visits show the new name while retaining their original reason explanation

**AC-NEW-006-3:**
- **Given** a Reason Type is referenced by existing or historical Visit Dues
- **When** I retire it
- **Then** it is archived rather than deleted, remains visible on those visits and is unavailable for new reasons

**AC-NEW-006-4:**
- **Given** another active Reason Type already has the same name
- **When** I try to save a duplicate
- **Then** the save is rejected with a message naming the existing type

**AC-NEW-006-5:**
- **Given** I create or edit a Reason Type
- **When** I choose its presentation
- **Then** I select an icon and colour from the controlled system set
- **And** the UI previews them together with the type name

**Recommended Acceptance Tests:**

- `Should_AddReasonType_When_NameIsUnique`
  → Makes the type available for new Visit Dues.
- `Should_PreserveExistingReasons_When_TypeRenamed`
  → Historical references remain intact.
- `Should_ArchiveInsteadOfDelete_When_TypeIsReferenced`
  → Existing and historical visits retain their type.
- `Should_RejectReasonType_When_ActiveNameAlreadyExists`
  → Prevents indistinguishable options.
- `Should_SaveReasonType_When_IconAndColourChosenFromControlledSet`
  → Keeps planner indicators configurable but visually constrained.

---

## Amendments to existing stories

### Stock Allocation US-003 and US-005 — re-propose from selected stock pools

> **Context:** an Incoming delivery may be too far away to be useful even though it is technically allocatable. Stock changes preserve the current draft until the manager explicitly chooses to re-propose.

**Additional Acceptance Criteria:**

**AC-SA003-A:**
- **Given** an allocation draft uses On Hand and one or more Incoming deliveries
- **When** I choose `Re-propose...`
- **Then** I see each stock pool with its quantity and availability/expected date
- **And** pools currently included in the draft start selected

**AC-SA003-B:**
- **Given** an Incoming delivery is too far away to use for current orders
- **When** I exclude it and apply Re-propose
- **Then** the complete-what-you-can proposal uses only the selected stock pools

**AC-SA003-C:**
- **Given** I am reviewing the pool selection
- **When** the page describes the effect
- **Then** it states that Re-propose replaces current draft allocations across all waiting orders

**AC-SA003-D:**
- **Given** I cancel the pool-selection step
- **When** I return to H-10
- **Then** the existing draft remains unchanged

---

### Targets & Performance US-004 — target-led Location ordering

> **Context:** R-03 is a target-attainment view. Ordering by highest contribution foregrounds Locations already performing well rather than those needing attention.

**Replacement Acceptance Criterion for Scenario 3:**

**AC-TP004-A:**
- **Given** two or more Locations have targets in the selected period
- **When** I open the per-Location target breakdown
- **Then** targeted Locations are ordered by lowest percentage attainment first

**AC-TP004-B:**
- **Given** one or more Locations have no target in the selected period
- **When** the Location breakdown uses the default `All` filter
- **Then** those Locations appear in a separate `No target set` section beneath targeted Locations
- **And** each shows its actual contribution and Accepted Order count without a percentage

**AC-TP004-C:**
- **Given** the Location breakdown is open
- **When** I choose `With targets` or `No target`
- **Then** only Locations matching that target-status filter are shown
- **And** choosing `All` restores both sections

**AC-TP004-D:**
- **Given** I have a target for the selected period
- **When** headline progress renders
- **Then** its progress bar includes a labelled straight-line pace marker
- **And** exact percentage attainment and time remaining are shown in text

**AC-TP004-E:**
- **Given** a target may be naturally end-weighted
- **When** actual progress is behind the straight-line marker
- **Then** the rep view does not infer `off track`, apply warning colour, or change ordering from that difference

**AC-TP004-F:**
- **Given** a manager creates or edits a target
- **When** they define its period and amount
- **Then** no pacing model is requested or inferred
- **And** reps and managers use their commercial context to interpret the neutral reference marker

**Superseded:** US-004 scenario 3's `highest contribution first` ordering for the target breakdown.

---

### Visit Planning US-003 — four-week panel and selectable scheduler views

> **Context:** resolves the source story's mismatch between its four-week example and two-week empty state, and replaces the fixed-week assumption in the first R-01 wireframe.

**Additional / Replacement Acceptance Criteria:**

**AC-VP003-A:**
- **Given** I open the planner
- **When** the scheduler loads
- **Then** I can select Day, Week or Month view and navigate the corresponding timeframe

**AC-VP003-B:**
- **Given** an open Visit Due is Overdue or due within the next four weeks and has no Scheduled Day
- **When** I view the unscheduled panel
- **Then** it appears in the Town list and map where coordinates exist

**AC-VP003-C:**
- **Given** an unscheduled Visit Due is due later than four weeks from today and is not Overdue
- **When** I view the unscheduled panel
- **Then** it does not appear until it enters the four-week horizon

**AC-VP003-D:**
- **Given** there are no Overdue or unscheduled Visit Dues due within the next four weeks
- **When** I view the panel
- **Then** it shows "Nothing unscheduled due in the next 4 weeks"

**AC-VP003-E:**
- **Given** I change the scheduler between Day, Week and Month
- **When** the calendar timeframe changes
- **Then** the unscheduled panel continues to use the same four-week horizon

**AC-VP003-F:**
- **Given** I have never opened the planner before
- **When** it loads
- **Then** the scheduler opens in Week view

**AC-VP003-G:**
- **Given** I previously selected Day, Week or Month view
- **When** I return to the planner
- **Then** it opens in my last-selected view

**AC-VP003-H:**
- **Given** I select Month view
- **When** the month renders
- **Then** each day shows its number of scheduled visits and does not render individual visit cards
- **And** it shows town names only when the day cell has room

**AC-VP003-I:**
- **Given** a day in Month view contains one or more scheduled visits
- **When** I select that day
- **Then** the scheduler switches to Day view with that date active
- **And** the day's individual visits are shown there

**AC-VP003-J:**
- **Given** I am using Day, Week or Month view and there is enough horizontal room
- **When** the planner renders
- **Then** the unscheduled panel remains visible beside the calendar

**AC-VP003-K:**
- **Given** available width is too limited for both usable calendar cells and the open panel
- **When** the planner renders
- **Then** the panel collapses to an `Unscheduled (n)` control
- **And** I can reopen it without losing my selection or changing its four-week horizon

**AC-VP003-L:**
- **Given** a scheduled visit has a Due Reason
- **When** I view it in Week view
- **Then** its card shows the Reason Type's icon and colour without the free-text explanation
- **And** the icon has an accessible type label

**AC-VP003-M:**
- **Given** a scheduled visit has a Due Reason
- **When** I view it in Day view
- **Then** the reason text is shown when space permits
- **And** opening the visit always shows the Reason Type and full explanation

> **Dependency:** BR-NEW-004 defines the structured Reason Type. Its vocabulary and ownership are still open.

**Superseded:** Visit Planning US-003 scenario 5's “next 2 weeks” empty-state wording.

---

### Visit Planning US-004 and US-005 — duration behavior when scheduling

> **Context:** distinguishes the immediate drag accelerator from the explicit `Schedule on...` route while preserving the source stories' default-duration and warning rules.

**Additional Acceptance Criteria:**

**AC-VP004-A:**
- **Given** an unscheduled Visit Due has a resolved default duration
- **When** I drag it onto an available day
- **Then** it is scheduled immediately with that duration and the day's load updates

**AC-VP004-B:**
- **Given** a scheduled visit uses its default duration
- **When** I select the visit and amend its duration
- **Then** the visit and day load update immediately

**AC-VP004-C:**
- **Given** a normal drop takes the day over its Working Day
- **When** the visit is scheduled
- **Then** the drop succeeds and the day shows `Over by` with the amount

**AC-VP004-D:**
- **Given** the dropped date is after the Visit Due's due date
- **When** I drop the visit
- **Then** the existing US-004 confirmation is shown before scheduling, including the Due Reason when present

**AC-VP004-E:**
- **Given** the destination is blocked by Planned Absence
- **When** I try to drop the visit
- **Then** the drop is refused as specified by US-004

---

### Visit Planning US-009 — review-and-select bulk decisions

> **Context:** replaces immediate `Apply to all` behavior. The rep chooses an outcome first, reviews every candidate across both digest sections, and may exclude individual visits before applying it.

**Additional / Replacement Acceptance Criteria:**

**AC-VP009-A:**
- **Given** the digest contains visits in `Still open from earlier weeks` and `This week`
- **When** I choose `Review Keep Overdue...` or `Review Mark Missed...`
- **Then** a dedicated review page lists all candidate visits under those same section headings
- **And** every candidate starts selected

**AC-VP009-B:**
- **Given** the bulk review is open
- **When** I use Select all, Clear all, or change an individual checkbox
- **Then** the selected count and the set to be changed update immediately

**AC-VP009-C:**
- **Given** I excluded one or more visits
- **When** I apply the chosen outcome
- **Then** only selected visits receive it
- **And** excluded visits remain undecided in the digest

**AC-VP009-D:**
- **Given** no visits are selected
- **When** the review renders
- **Then** the apply action is unavailable and the review explains that at least one visit must be selected

**AC-VP009-E:**
- **Given** I apply the chosen outcome successfully
- **When** processing completes
- **Then** I return to the Cycle End digest
- **And** excluded or otherwise undecided visits remain visible there

**AC-VP009-F:**
- **Given** I leave the review without applying
- **When** I return to the digest
- **Then** no visit decisions have changed

**AC-VP009-G:**
- **Given** I am reviewing `Mark Missed` for selected visits
- **When** I enter an optional shared reason
- **Then** that reason is applied to every selected visit that has no individual override

**AC-VP009-H:**
- **Given** a selected visit has a different cause
- **When** I override or clear its reason
- **Then** that visit keeps its individual value while the other selected visits retain the shared reason

**AC-VP009-I:**
- **Given** I change the shared reason after adding an individual override
- **When** the review updates
- **Then** the override is preserved and only non-overridden selected visits receive the new shared value

**AC-VP009-J:**
- **Given** I apply `Mark Missed` to selected Visit Dues
- **When** processing succeeds
- **Then** each selected Visit Due is closed and recorded as Missed with its applicable optional reason
- **And** no replacement visit, notification or automatic follow-up workflow is created
- **And** the next fixed-cycle Visit Due is unchanged

**AC-VP009-K:**
- **Given** a missed visit should be attempted again
- **When** the rep or a reviewing manager decides to reschedule it
- **Then** they create a one-off Visit Due through the normal planning flow

**AC-VP009-L:**
- **Given** I want to make different decisions for individual visits
- **When** I use a row's `Keep Overdue / Missed` controls on the digest
- **Then** I can save those decisions without entering either bulk-review page

**AC-VP009-M:**
- **Given** the digest first loads
- **When** I have not made a decision for a visit
- **Then** neither row outcome is preselected

**AC-VP009-N:**
- **Given** I select `Missed` on one digest row
- **When** the row expands
- **Then** an optional reason field appears directly beneath that visit

**AC-VP009-O:**
- **Given** I entered a row-level Missed reason
- **When** I switch that row to `Keep Overdue`
- **Then** the reason field is hidden and its value is not saved

**Superseded:** Visit Planning US-009 scenario 3's immediate `Apply to all` behavior.

---

### Pricing US-008 *(confirm)* — price override applied within allowance

> **Context:** overrides change from *requested* (head office decides) to *applied* within the rep's allowance (BR-NEW-002). The override sheet has four states: stacks, falls back, blocked by promotion, blocked by multi-buy.

**Additional Acceptance Criteria:**

**AC-PR008-A:**
- **Given** I open the override sheet on a line
- **When** it renders
- **Then** it shows the resolved price with its source and the lowest price I can offer, before I type anything

**AC-PR008-B:**
- **Given** I enter a price no lower than the lowest I can offer, with a reason
- **When** I tap Apply
- **Then** the price is applied to the line immediately and no head office decision is created

**AC-PR008-C:**
- **Given** I enter a price below the lowest I can offer
- **When** I try to apply it
- **Then** Apply is unavailable

**AC-PR008-D:**
- **Given** the line has a promotion no larger than my allowance
- **When** the sheet renders
- **Then** the lowest price is my allowance off the promotion price, and the sheet says so

**AC-PR008-E:**
- **Given** the line has a promotion larger than my allowance, and my allowance off the tier or break price is lower than the promotion price
- **When** the sheet renders
- **Then** the lowest price is my allowance off that base, and the sheet names the base ("10% off the bulk price")

**AC-PR008-F:**
- **Given** the line has a promotion larger than my allowance, and the promotion price is lower than anything my allowance can reach
- **When** the sheet renders
- **Then** it explains that no further discount can be added, with no price field and a Close action

**AC-PR008-G:**
- **Given** the line is part of a buy X get Y, bundle or mix-and-match promotion
- **When** the sheet renders
- **Then** it names the promotion and explains a rep discount can't be added, with no price field

**AC-PR008-H:**
- **Given** a rep price has been applied to a line
- **When** I open the price provenance sheet
- **Then** the applied price is shown as a calculation: the base or promotion price, my discount, and my reason

**Superseded:** any criterion in which head office accepts or declines an override ("accept at €9.00" / "accept at resolved €11.20"), and any status on T-08 describing an override's outcome.

**Recommended Acceptance Tests:**

- `Should_ShowLowestPriceBeforeInput_When_OverrideSheetOpens`
- `Should_ApplyWithoutHeadOfficeDecision_When_PriceWithinAllowance`
- `Should_StackOnPromotion_When_PromotionWithinAllowance`
- `Should_FallBackToBasePrice_When_PromotionExceedsAllowanceButBaseAllowanceLower`
- `Should_ExplainBlock_When_PromotionBelowAnyReachablePrice`
- `Should_BlockRepDiscount_When_LineInMultiBuy`
- `Should_ShowCalculation_When_ProvenanceOpenedOnDiscountedLine`

---

### Pricing US-009 *(confirm)* — free of charge restricted and applied

> **Context:** BR-NEW-003.

**Additional Acceptance Criteria:**

**AC-PR009-A:**
- **Given** I choose Add free-of-charge line
- **When** the picker opens
- **Then** it lists only products in the Discontinuing state, and states that only discontinuing products can be given free

**AC-PR009-B:**
- **Given** the picker is open
- **When** it renders
- **Then** it shows the order's free-of-charge cap and how many units are already used

**AC-PR009-C:**
- **Given** adding the quantity would exceed the cap
- **When** I try to add it
- **Then** it can't be added

**AC-PR009-D:**
- **Given** the free-of-charge line is within the cap
- **When** I add it
- **Then** it is applied to the order and no head office decision is created

**Superseded:** free-of-charge lines on any product with a free-text reason; head office "with or without the free line" decisions.

**Recommended Acceptance Tests:**

- `Should_ListOnlyDiscontinuingProducts_When_FocPickerOpens`
- `Should_ShowCapAndUsage_When_FocPickerOpens`
- `Should_PreventAdd_When_FocCapWouldBeExceeded`
- `Should_ApplyWithoutHeadOfficeDecision_When_FocWithinCap`

---

### Promotions US-007 *(confirm)* — spend threshold qualifies before rep discounts

**Additional Acceptance Criteria:**

**AC-PM007-A:**
- **Given** an order qualifies for a spend-threshold promotion on resolved prices
- **When** rep discounts bring the order total below the threshold
- **Then** the order still qualifies for the threshold discount

**Recommended Acceptance Tests:**

- `Should_KeepThresholdDiscount_When_RepDiscountDropsTotalBelowThreshold`

---

### Area 1 US-014 — sent item view

**Additional Acceptance Criteria:**

**AC-A1014-A:**
- **Given** a line on the order was removed as unavailable
- **When** I open the sent item
- **Then** the line reads "Not supplied — [reason]. Removed from the order. Let the customer know."

**AC-A1014-B:**
- **Given** the order has a rep price or a free-of-charge line
- **When** I open the sent item
- **Then** they are shown as facts of the order ("SPF30 at your price €9.25"), not as the outcome of a request

**Superseded:** "outcomes of any override or free-goods request".

---

### Head Office US-001, US-002 — worklist *(superseded in part)*

Superseded: the Flagged Orders and Routine Orders sections, and the "Accept all routine" action. Orders no longer appear on the worklist (US-NEW-005).
Retained: range proposals, duplicate matches and account requests, each labelled with its type.

### Head Office US-003 — order detail decisions *(superseded)*

H-02 becomes a record rather than a decision screen (US-NEW-005, AC-6 and AC-7). Partial Release is replaced by routing short quantities to allocation. Whether Hold and Reject survive is open (RC-NEW-003).

### Head Office US-006 — held orders *(affected)*

Depends on RC-NEW-003. If Hold is removed, this story's trigger needs redefining or the story retires.

---

## Edge cases

- **EC-NEW-001 — promotion versus bulk price.** SPF30 at 24 units: promotion €9.99 (11% off tier) exceeds a 10% allowance, but 10% off the €10.08 break is €9.07. The rep can offer €9.07 (BR-NEW-002 rule 4).
- **EC-NEW-002 — weak multi-buy.** A buy 10 get 1 free (about 9%) blocks the rep's discount although a price promotion of the same size wouldn't. Accepted (BR-NEW-002 rule 5).
- **EC-NEW-003 — free-of-charge stock runs out.** The line is removed as unavailable and the rep is prompted (BR-NEW-003 rule 3).
- **EC-NEW-004 — first order of a product at a location.** No out-of-pattern marker, since there's nothing to compare against (US-NEW-004).
- **EC-NEW-005 — Low unticked after adding.** The order line stays (US-NEW-002).

---

## Open clarifications

- **RC-NEW-001** — What clears the "not supplied" count: opening the order, an explicit "Told them", or the next call at that location?
- ~~**RC-NEW-002** — Are the rep discount allowance and the free-of-charge cap one figure for everyone, set per rep, or per category?~~ **Resolved at UX level:** a manager groups eligible products into a commercial policy profile with rules such as 10% rep discount and X FOC units per rep per month.
- **RC-NEW-003** — Do Hold and Reject survive on H-02 with no flag driving them?
- **RC-NEW-004** — Do orders appear on H-01 at all, e.g. as a read-only feed, or only via search and the customer record?
- **RC-NEW-005** — Does the quantity popover dismiss on add, or advance to the next not added Low item?
- **RC-NEW-006** — Does the spend-threshold discount apply to the final order total (assumed) or the pre-discount total?
- **RC-NEW-007** — What does the Low tab show on an order that doesn't follow a stock check (a phone order, say)? Not discussed.
- ~~**RC-NEW-008** — If a Product is outside a commercial policy profile, is the corresponding action unavailable?~~ **Resolved:** yes; product membership explicitly enables the action.
- **RC-NEW-009** — Does the commercial policy profile replace or extend the catalogue's existing one-per-product Product Profile, or become a separate rule/membership model? The UX name is provisional until the business rules are firmer.
- ~~**RC-NEW-010** — When an offline rep adds an FOC line, does it consume the monthly allowance immediately, and how is that reservation reconciled?~~ **Resolved:** FOC is an ordinary €0.00 order line; its quantity counts while the line exists and normal line removal releases it. The tablet combines the synced balance with local lines.
- ~~**RC-NEW-011** — Does the manager/admin choose the icon and colour for each Visit Due Reason Type, or does the system assign them?~~ **Resolved:** the manager/admin chooses both from a controlled system set.
- ~~**RC-NEW-012** — What happens after a rep marks a Visit Due Missed?~~ **Resolved:** close and record only. No automatic replacement or workflow; a rep or manager may manually create a one-off Visit Due.
