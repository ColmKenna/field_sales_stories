# Handover — Field Sales Management System UX session

**Prepared:** 21 Sep 2026
**For:** the next agent picking up this design work
**Read this first, then the files in §4.**

---

## 1. What to hand over

| Item | Where | Notes |
|---|---|---|
| This file | `handover.md` | Start here |
| The original brief | `../stories/screen-inventory-ux-brief.md` | "Field Sales Management System — Screen Inventory & UI Brief", 19 Sep 2026. Every screen ID (T-, R-, M-, H-, C-) and story reference comes from it |
| Wireframe documents | `00`–`03` | Living documents, updated in place |
| User stories | `04-user-stories-amendments.md` | Stories derived from settled decisions only |
| Agent prompt | the "Socratic UX & Design Agent" prompt used in this session | Keep using it — the method in §3 depends on it |

The fourteen source user-story documents are now available in `../stories/`. Use them as the source of truth when resolving or folding in the amendments from `04-user-stories-amendments.md`.

---

## 2. The project in brief

A wholesaler sells into shops. **Field Salespersons** visit or phone shops, count stock and take orders on a tablet that is **offline all day** and syncs deliberately. **Managers** plan coverage and targets. **Head office** processes orders to an external warehouse, maintains the catalogue and controls pricing. **Customers** can order online.

Three things shape every screen (brief §1): the tablet works from a morning snapshot; the rep captures and head office confirms; performance is measured per location.

**Important:** the second of those has changed in this session — see §6.2. Orders no longer need head office confirmation.

---

## 3. How the session runs

### The user

Colm — senior software developer in Ireland, strong engineering instincts, **new to UX as a discipline**. Works at specification level; outputs feed implementation by others.

### What works

- **Socratic, one question per turn.** Teach the concept by name in 2–4 sentences → concrete example → one question that applies it. Alternatives only after 2–3 attempts at the same point stall.
- **Tight trade-off framing with worked numbers.** Colm answers forks tersely — "b", "yes", "high only", "still reach their allowance". Present 2–3 named options with the consequence of each, then ask.
- **Take domain corrections at face value.** Colm knows the business; when he corrects a scenario (§7), accept it, update the artefact, move on. Don't defend a hypothetical.
- **Fold settled decisions into the artefacts immediately** — before moving on. Update DECISION blocks, mark superseded ones, resolve open questions with ~~strikethrough~~ **Resolved.** If a thread is mid-decision, wait until it settles so the file isn't edited twice.
- **Log open questions explicitly** rather than leaving them implicit.
- **"Next"** means move to the next area.

### What to avoid

- CSS, colour, typography, spacing, component libraries, framework markup.
- Summarising on every turn.
- Pre-filling or proposing solutions before the concept has been taught and the question asked.
- Re-proposing alternatives Colm has already rejected (listed in §6).

---

## 4. File set and status

| File | Covers | Status |
|---|---|---|
| `00-conventions-and-shared-elements.md` | Brief §2 global patterns drawn: order line (5 densities), price provenance sheet, availability labels, "as of sync", unsent work, impact preview, filter→review→apply, archive, gap lists, empty states | Mostly drafting calls; provenance sheet updated for rep-discount calculation |
| `01-tablet-day.md` | T-01 Sync, T-02 Home, T-03 Agenda, T-04 Search, T-05 Location, T-06 Call & stock check, T-07 Order entry, T-08 Sent item | **T-07 heavily worked and settled.** Rest mostly drafting calls |
| `02-head-office.md` | H-01 Worklist, H-02 Order detail, H-03 Despatch, H-10 Allocation, H-16 Category archive | **H-01, H-02, H-10 and H-16 settled.** H-03 remains drafting calls |
| `03-rep-planner.md` | R-01 Planner, R-02 Cycle end digest, R-03 Rep performance | **R-01, R-02 and R-03 settled** |
| `04-user-stories-amendments.md` | BR-NEW-001–003, US-NEW-001–005, amendments to Pricing US-008/009, Promotions US-007, Area 1 US-014, superseded Head Office US-001–003/006, EC-NEW-001–005, RC-NEW-001–007 | Reflects every settled decision up to the end of the session |
| `05-manager.md` | M-01 Visit Planning overview onward | **M-01–M-05 settled; M-06 active.** One-off Visit Due entry points are next |

**Not drawn at all:** M-01 to M-15, C-01 to C-08, and H-04 to H-09, H-11 to H-15, H-17 to H-30. Deliberately deferred — brief §10 calls them conventional once the patterns settle.

---

## 5. Two kinds of decision — this matters most

Every wireframe carries **DECISION blocks**. They are not all equal.

- **Settled** — Colm confirmed it in conversation. Authoritative. Listed in §6.
- **Drafting calls** — Claude made the call where the brief was silent, with rationale, **and Colm has not confirmed it**. That's the majority of DECISION blocks in 00, 01 (outside T-07 and T2.5), 02 (H-03, H-10, H-16) and all of 03.

Do not turn a drafting call into a user story, or treat it as agreed, until Colm confirms it.

**Settled IDs:** T7.1, T7.6, T7.7, T7.8, T7.9, T7.10, T7.11, T7.12, T2.5 (except its clearing rule), H1.6, H1.7, H2.3, H2.6, H2.7, H2.8, H10.1–H10.7, H16.1–H16.3, R1.1–R1.12, R2.1–R2.7, R3.1–R3.5, M1.1–M1.2, M2.1–M2.4, and the H-01 disposition table.

---

## 6. Settled decisions

### 6.1 Order entry and the Low list (T-06, T-07)

| Decision | Why | Rejected — don't re-propose |
|---|---|---|
| Low list is a **tab** beside Order pad and Search all (T7.1) | Count keeps it visible at no vertical cost; stable screen model | Pinned strip; bottom sheet |
| Marking Low offers **Add to order**; Add opens a **quantity popover** — from T-06 at tick-time and from the Low tab | Rep confirms quantity with the customer verbally anyway, so the popover costs no extra time | Auto-add with a suggested quantity; auto-add at 0 or 1 |
| Quantity field **opens empty** (T7.7) | No "usual level" exists in the data model (H-28 holds a threshold, which is a floor); pre-filled numbers anchor | Top-up suggestion (usual − counted) |
| Low tab is a **catch-up list** with five states: not added, ADDED, REMOVED, REPLACED, CAN'T ADD. Count shows not added + CAN'T ADD only | Removed and replaced are decisions, not gaps | Two-state added/not added |
| **No "Add all"** (T7.6) | Each line needs a confirmed quantity | Bulk add |
| Unticking Low **doesn't remove** an added line (T7.8) | Low records the shelf; the line records the agreement | Line follows the tick |

### 6.2 Head office order processing (H-01, H-02)

**No order needs a person.** Every flag has a disposition:

| Disposition | Flags | Behaviour |
|---|---|---|
| Route | Stock Shortfall, Oversold | Accepted; short quantity goes to H-10 allocation |
| Auto-resolve | Unavailable Line | Accepted; line removed with reason; rep prompted |
| Annotate | Large, Watched Product, Rep-flagged, New Location, Prospect Conversion | Recorded; no human step |
| Divert | *(none)* | Override and FOC left once limits were enforced at capture |

- H-01 now holds only **range proposals, duplicate matches and account requests**. "Accept all routine" is gone.
- H-02 is a **read-only record**: captured lines, what was changed automatically and why, "Total as captured" and "Total accepted".
- Concepts taught: alarm fatigue; gatekeeper vs guardrail; exception routing.
- **Rejected:** keeping any flag as Divert; Large as Divert.

### 6.3 Rep notification (T-02, T-08)

- When a line is auto-removed, the rep is **actively prompted**: a fourth counter on Home's exception strip, "not supplied" (T2.5).
- T-08 shows the removed line with "Let the customer know."
- **Rejected:** passive visibility only.
- **Open:** what clears the counter (RC-NEW-001).

### 6.4 Rep discount allowance (T7.9, T7.12, BR-NEW-002)

Replaces head office override approval. Colm's rules, in order:

1. The allowance is a **percentage off the resolved price**. Overrides inside it are **applied**, not requested (button says Apply).
2. **Base price** = best non-promotional price: tier, list or **quantity break**. Breaks are bulk buying, not an offer.
3. **Promotion no larger than the allowance** → rep's discount **stacks** on the promotion price.
4. **Promotion larger than the allowance** → can't stack, but the rep **can still reach the allowance off the base price** if that is lower than the promotion. Only if the promotion is lower still is the line blocked. (Monotonicity: a promotion must never leave the rep worse off.)
5. **Buy X get Y, bundle, mix and match** → **never stackable**, whatever the size. Accepted exception to monotonicity.
6. **Spend threshold** → qualification checked **before** rep discounts. *Assumed* (not confirmed): threshold discount applies to the final total.
7. A blocked override sheet **opens and explains why** (T7.12) — the rep needs a sentence for the customer.
8. **Working policy model:** a manager groups eligible products into a commercial policy profile such as "Allow 10% rep discount and 12 FOC units per rep per month". Product membership enables the action. The profile/entity name is provisional and may become explicit business rules; do not conflate it with the existing one-per-product catalogue Product Profile without resolving that model.

**Consequence noted:** effective maximum discount is nearly twice the allowance under rule 3.

**Rejected:** absolute minimum price per product; promotions consuming the allowance (option A, total capped); multi-buys measured by effective discount; spend threshold measured after rep discounts; threshold switching off rep discounts.

### 6.5 Free of charge (T7.10, BR-NEW-003)

- Only on **Discontinuing** products; **quantity allowance per rep per calendar month**; applied without approval.
- FOC eligibility and the monthly allowance are configured through the working commercial policy profile by a manager; product membership opts in.
- An FOC item is an **ordinary order line at €0.00**. Stock, availability, allocation, despatch and removal behave exactly as for any other line.
- Its quantity counts against the monthly allowance while the line exists. Reducing or removing it releases the quantity; server-side removal is reflected at the next Sync.

### 6.6 Catching slips (T7.11)

- Large stays Annotate. Slips are caught on **T-07 Review**, where the rep checks the order before saving.
- Review **marks unusually high quantities** ("Usually about 48 here") using the **last 3 accepted orders** at the location — data already in the snapshot for T-06's suggested list.
- **High only.** No history → no marker. **Non-blocking fallback**: never blocks Mark Ready to Send, no acknowledgement. Threshold is implementation detail.
- Concepts taught: slips vs mistakes (Norman); self-review weakness; signal detection.
- **Rejected:** low-quantity markers; a confirmation dialog.

---

## 7. Domain facts from Colm (not in the brief)

- Prices resolve from the **morning snapshot**; there is no price drift between capture and processing. A product that goes unavailable and returns at a new price has already had its line cancelled.
- **Free of charge exists only to clear discontinuing stock.**
- Reps **confirm quantities verbally** with the customer when adding.
- **Quantity breaks are bulk buying** — part of the base price, not a promotion.
- Reps should have a **cap on how far they can discount**, enforced on the tablet.

---

## 8. Where the session stopped

The allowance and FOC granularity question is answered at UX level: a manager groups eligible products into a commercial policy profile carrying rules such as 10% rep discount and X FOC units per rep per month. Products outside a policy cannot use those actions. "Profile" is provisional and may become a more explicit business-rule model.

FOC accounting is settled by treating it as an ordinary order line at €0.00: the line counts while it exists and normal removal releases it. R-01–R-03, H-10, H-16 and M-01–M-05 are settled. Campaign detail always opens on `Overall`; `By rep` is a secondary diagnostic view. Overall progress and outcome counts remain visible above either detail table, and every outcome count filters the visit list without changing totals. `Extend remaining...` opens a review with all open visits selected, supports row exclusions, previews the new window, and never changes completed or cancelled visits. M-05 conflicts sort by earliest affected visit in `Open`; entry from a rep row carries a removable rep filter. Cleared conflicts retain both versions and clearing audit details in a separate `Resolved` view ordered by most recently cleared. M-06 Add a one-off Visit Due is active; its entry points are the next decision.

---

## 9. Open questions

### Pick up first

1. ~~**Allowance and FOC cap granularity and accounting.**~~ **Resolved at UX level — eligible products join a provisional commercial policy profile; FOC is X units per rep per month and behaves as ordinary €0.00 order lines.** Still open only at domain-modelling level: policy entity shape (RC-NEW-009).
2. **What clears "not supplied" on Home** — opening the order, explicit "Told them", or next call at the location? (RC-NEW-001)
3. **Low tab on an order with no stock check** — e.g. a phone order. (RC-NEW-007)
4. **T-06 when channel = Phone** — does the stock check stay, collapse or disappear?

### Head office

5. Do Hold and Reject survive on H-02 with no flag driving them? Affects Head Office US-006. (RC-NEW-003)
6. Do orders appear on H-01 at all, e.g. a read-only feed? (RC-NEW-004)
7. H-10 — draft allocation when an incoming delivery changes.
8. H-16 — does "Leave them in the archived branch" need a second confirmation?

### Smaller

9. Quantity popover: dismiss on add, or advance to the next not added item? (RC-NEW-005)
10. Spend-threshold discount base — final total (assumed) or pre-discount? (RC-NEW-006)
11. T-02 Overdue below Today; T-07 footer showing order value in front of customer; T-07 agreed range merged or separate; T-05 two equal primary actions; T-01 no auto-sync — all drafting calls awaiting confirmation.
12. R-01: ~~absence days blocked or droppable~~ **resolved — blocked**; ~~unscheduled horizon~~ **resolved — Overdue plus next four weeks**; ~~one-week window vs 3-week cycles~~ **resolved — selectable Day / Week / Month views**. R-02: apply-to-all scope.
13. 00 conventions: stepper placement for left-handed one-handed use; impact preview dialog vs page threshold; run-out wording on one line or two.

### Carried from brief §11

Tablet snapshot size (now also must carry: applicable commercial policy rules and membership, rep month-to-date FOC usage/remaining allowance, last-3-orders per product per location), soft-reminder hour, prospect pricing, R-04 corrections screen, editing a live promotion, customer app scope, Location Profile vs Location Type.

---

## 10. Suggested next steps

In the brief's §10 order, with what's done:

1. ~~T-07 order entry and the order line~~ — Low list, pricing and FOC line behaviour settled. Remaining: provisional policy entity shape, plus open questions 3 and 9.
2. **T-02 Home and T-01 Sync** — drafted; confirm drafting calls; settle open question 2.
3. **T-06 Call & stock check** — drafted; settle open question 4.
4. ~~H-01 Worklist and H-02 Order detail~~ — settled. Remaining: open questions 5, 6.
5. ~~**R-01 Planner**~~ — settled in `03`. Includes responsive Day / Week / Month scheduling, fixed four-week unscheduled horizon plus Overdue, immediate drag with editable duration, and structured manager/admin-maintained Due Reason Types with controlled icon/colour choices.
6. **H-10 Allocation and H-16 Category archive** — drafted; not discussed.
7. M- and C- screens — not drawn.

A natural session plan: settle M-06 Add a one-off Visit Due, then continue through the remaining manager panels. Leave the provisional commercial-policy entity shape for the business-rule modelling pass unless it blocks the UX.

---

## 11. Conventions

### Wireframes

- ASCII frames; tablet ~64 characters wide, laptop ~88–96. Drawn to show **hierarchy**, not pixels.
- Notation: `[ Button ]` · `[x]` checkbox · `( )` radio · `>` navigates · `v` expands · `(!)` orderable with caveat · `(x)` not orderable · `(i)` opens detail.
- Mermaid for flows.
- DECISION IDs are per screen: `T7.9` = T-07, ninth decision. Convention-level ones in `00` are `1.1`, `5.2` etc.

### Consistent example data — reuse it

| Product | Figures |
|---|---|
| SPF30 Sun Lotion 200ml | Tier €11.20 · break at 24+ €10.08 · Autumn promotion €9.99 (11% off) · discontinuing, replaced by SPF30 v2 |
| Hand Cream 75ml | Tier €4.00 · Spring offer €3.80 (5%) · rep price €3.42 |
| Sudocrem 125g | €4.85 · buy 3 get 1 free |
| Commercial policy example | 10% rep discount · 12 FOC units per rep per month |
| Locations | Quinn's Centra (Rathdrum), Hickey's Pharmacy / Head Office, Carey's Pharmacy (Arklow), Byrne's (Aughrim), Doyle's (Tinahely) |
| People | Colm and Aoife (reps), Mary Walsh (inactive main contact) |

### User stories

Match the format already in `04` and in Colm's earlier specs:

- `US-NEW-###: As a [role], I want [x] so that [y].` with a `> **Context:**` note
- `**AC-NEW-###-N:**` in Given / When / Then
- `**Recommended Acceptance Tests:**` as `Should_Outcome_When_Condition` with a `→` explanation line
- `BR-NEW-###` business rules, `EC-NEW-###` edge cases, `RC-NEW-###` open clarifications
- Amendments: additional criteria plus a **Superseded:** line naming what to strike
- Mark story IDs inferred from the brief as *(confirm)*

### Principles that recur

- **Propose, don't impose** (brief §2.6): never pre-fill a *decision*. Pre-filling is fine for **arithmetic or transcription** — H-10's proposed split, H-03's despatch quantities.
- **Guardrails over gatekeepers**: prefer limits enforced at capture to human review afterwards.
- **Monotonicity**: adding a discount must never make the best reachable price worse (one accepted exception: multi-buys).
- Colm's standing principles: jobs-to-be-done first, YAGNI for UX, progressive disclosure, primary job above the fold, action-oriented naming, no decoration without meaning.

### Editing the files

Changes were made in place with exact string replacement (asserting a single match) and frames generated programmatically with `ljust` so borders align. Keep that approach — the files are large and hand-aligned ASCII drifts easily.
