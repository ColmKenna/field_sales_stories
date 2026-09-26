# Handover — Field Sales Management System UX session

**Prepared:** 21 Sep 2026
**For:** the next agent picking up this design work
**Read this first, then the files in §4.**
**Last updated:** 26 Sep 2026 — customer screens in progress. **Resume at §10 "Customer screens — in progress".**

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

**23 Sep 2026:** everything settled in `04` has now been folded into `../stories/` (see the note at the top of `04` for where each placeholder story went). From now on, a newly settled decision goes into both `04` and the matching source story.

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
| `01-tablet-day.md` | T-01 Sync, T-02 Home, T-03 Agenda, T-04 Search, T-05 Location, T-06 Call & stock check, T-07 Order entry, T-08 Sent item | **T-01–T-08 baseline settled (24 Sep 2026).** BR-NEW-008 adds a new cross-area question: T7.14 with several Agreed Ranges. C1.3–C1.7 add a collapsed Customer requests section to T-02 Home; requests and approvals move in the background — a narrow exception to T1.3. C8.1 adds "Set up online ordering" from the contact on T-05 |
| `02-head-office.md` | H-01 Worklist, H-02 Order detail, H-03 Despatch, H-10 Allocation, H-16 Category archive | **H-01, H-02, H-10 and H-16 settled.** H-03 remains drafting calls |
| `03-rep-planner.md` | R-01 Planner, R-02 Cycle end digest, R-03 Rep performance | **R-01, R-02 and R-03 settled** |
| `04-user-stories-amendments.md` | BR-NEW-001–008, US-NEW-001–007, amendments to Pricing, Promotions, Area 1, Visit Planning (US-003–US-015), Coverage US-001–US-003, Targets, Stock Allocation, Self-service; superseded Head Office stories; EC-NEW-001–008; RC-NEW-001–013 | Reflects every settled decision up to the end of the session |
| `05-manager.md` | M-01 to M-15, plus M-16 Add a one-off Visit Due (not in the brief; numbered after M-15 so it doesn't clash with the brief's M-06 Territory assignment) and M-17 Online ordering approvals (not in the brief; added 26 Sep 2026 from C-08) | **M-01–M-16 baseline settled; M-07 gains "Set up online ordering…" (C8.4); M-17 first-pass settled (M17.1; layout confirmed 26 Sep 2026)** (M16.6 superseded; M11.2 layout deferred to usage feedback). BR-NEW-008 adds a cross-area question for M11.1's `Show full Agreed Range` when several ranges exist. The three logged assumptions were confirmed on 23 Sep 2026: a Region moves with its last Counties (CV001-E2); carve-out Move visits are inherited (CV003-H); the unassigned count is scoped by nearest held geography, not company-wide (M15.2, CV007-D–G). No original manager drafting calls remain (M6.6, M7.5, M7.6, M13.3, M14.2 and M15.3 settled 23 Sep 2026). |

| `06-customer.md` | Jobs to be done; C-02 My locations; C-03 Catalogue & search (browse and search frames); C-04 Order entry (review and placed-order frames); C-05 Multi-branch order (desktop and phone frames); C-01 Invitation & first sign-in (expired-invitation frames); C-08 Create customer user (contact frame; tablet and M-07 entry points); C-06 Order history (first-pass frame); C-07 Order detail & repeat (first-pass frame and repeat warning); C-09 Usual products — the landing page, not in the brief, numbered after C-08 | **C-09 settled (24 Sep 2026)**; C9.8 mark wording confirmed 26 Sep 2026. **C-02 settled (25 Sep 2026). C-05 first-pass settled (26 Sep 2026):** C5.1–C5.16. **C-03 first-pass settled (26 Sep 2026):** C3.1–C3.3. **C-04 first-pass settled (26 Sep 2026):** C4.1–C4.2. **C-06 first-pass settled (26 Sep 2026):** C6.1–C6.2. **C-07 first-pass settled (26 Sep 2026):** C7.1–C7.4. **C-01 first-pass settled (26 Sep 2026):** C1.1–C1.7. **C-08 first-pass settled (26 Sep 2026):** C8.1–C8.4 |

**Not drawn at all (32 screens):** T-09 to T-14, R-04, and H-04 to H-09, H-11 to H-15, H-17 to H-30. The head office screens were deliberately deferred, because brief §10 calls them conventional once the patterns settle.

**Tally (26 Sep 2026):** 74 screens (the brief's 71 plus M-16, M-17 and C-09). 41 settled; 1 drafted (H-03); 32 not drawn. C-07 (C7.1–C7.4), C-01 (C1.1–C1.7) and C-08 (C8.1–C8.4) are first-pass settled, so every customer screen is drawn. M-17 is first-pass settled (M17.1). The 26 Sep drafting calls were all confirmed the same day.

---

## 5. Two kinds of decision — this matters most

Every wireframe carries **DECISION blocks**. They are not all equal.

- **Settled** — Colm confirmed it in conversation. Authoritative. Listed in §6.
- **Drafting calls** — Claude made the call where the brief was silent, with rationale, **and Colm has not confirmed it**. That's the majority of DECISION blocks in 00, 01 (outside T-07 and T2.5), 02 (H-03, H-10, H-16) and all of 03.

Do not turn a drafting call into a user story, or treat it as agreed, until Colm confirms it.

**Settled IDs:** T1.1–T1.3, T2.1–T2.7, T3.1–T3.2, T4.1, T5.1–T5.3, T6.1–T6.5, T7.1–T7.14, T8.1–T8.3, H1.6, H1.7, H2.3, H2.6, H2.7, H2.8, H10.1–H10.7, H16.1–H16.3, R1.1–R1.12, R2.1–R2.7, R3.1–R3.5, M1.1–M1.2, M2.1–M2.4, M3.1–M3.4, M4.1–M4.4, M5.1–M5.5, M6.1–M6.6, M7.1–M7.6, M8.1–M8.2, M9.1–M9.4, M10.1–M10.2, M11.1, M11.3, M12.1–M12.2, M13.1–M13.3, M14.1–M14.2, M15.1–M15.3, M16.1–M16.5, M16.7, M17.1, C1.1–C1.7, C2.1–C2.2, C3.1–C3.3, C4.1–C4.2, C5.1–C5.16, C6.1–C6.2, C7.1–C7.4, C8.1–C8.4, C9.1–C9.9, and the H-01 disposition table.

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
- **Clearing (settled 24 Sep 2026, T2.6/T8.3):** reps phone the shop the same day, so the signal is **declared** — "Told them", marked **per order** (records every removed line on it), on both the Not supplied list and T-08. A told order stays greyed in the list with "Told 14:20" and **Undo** until the rep leaves the list. **Backstop:** any Call logged at the Location clears every affected order there. A **later removal** on a told order re-raises it with only the new line flagged (EC-NEW-007).
- Concepts taught: completion signal (viewed / declared / inferred); backstop; unit of an item; user control and freedom; a declaration covers what was known.
- **Rejected:** clearing on opening the order; next call only; delivery as backstop; no backstop; per-line or per-Location marking; snackbar-only undo; told order staying cleared after a new removal.

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
- One-off visits are usually **short notice**. The manager chooses the rep based on the reason (expertise) or on who will be nearby, and **phones the rep** to confirm. The rep **resyncs mid-day** after that call.
- A Location can have **many Location Profiles**, and any of them may carry visit and stock defaults. Each default resolves separately to the most demanding value and is shown with its source (BR-NEW-005). This supersedes "one Location Profile per Location" in the source stories.
- The common coverage change from a shop is a **specialist assignment by profile within an area**, not a Primary Rep change. To be settled on M-10.
- **"As of sync" only when stale** (24 Sep 2026, BR-NEW-007, convention §3 rule 4): reps sync every morning, so the chip shows only when the last sync was before today, naming the date. Supersedes brief §2.1's always-on chip.
- **Work is judged as captured** (24 Sep 2026, BR-NEW-006): an uploaded order or call is validated against the rules in its snapshot, never today's. A reassignment, archive or policy change after capture doesn't reject it; Needs Attention holds only technical faults. Supply facts (stock, availability) are still handled by BR-NEW-001.
- When a one-off visit lands near another rep's scheduled visit, the **manager and reps sort it out between themselves** afterwards, and the manager then updates the plan. The system informs; it doesn't decide.

---

## 8. Where the session stopped

The allowance and FOC granularity question is answered at UX level: a manager groups eligible products into a commercial policy profile carrying rules such as 10% rep discount and X FOC units per rep per month. Products outside a policy cannot use those actions. "Profile" is provisional and may become a more explicit business-rule model.

FOC accounting is settled by treating it as an ordinary order line at €0.00: the line counts while it exists and normal removal releases it. R-01–R-03, H-10, H-16 and M-01–M-05 are settled. Campaign detail always opens on `Overall`; `By rep` is a secondary diagnostic view. Overall progress and outcome counts remain visible above either detail table, and every outcome count filters the visit list without changing totals. `Extend remaining...` opens a review with all open visits selected, supports row exclusions, previews the new window, and never changes completed or cancelled visits. M-05 conflicts sort by earliest affected visit in `Open`; entry from a rep row carries a removable rep filter. Cleared conflicts retain both versions and clearing audit details in a separate `Resolved` view ordered by most recently cleared. M-16 Add a one-off Visit Due is settled (22 Sep 2026): Location-first; Primary Rep default with a visit-only override (plain picker, assigned reps first); no tablet delivery status; a non-blocking notice when another visit is scheduled at the Location within a week of the window; and a one-off's Call never completes a recurring visit, even when the same rep holds both (an exception to One Call clears all). When one visit is enough, the manager moves the existing visit forward in the planner instead of adding a one-off. A per-visit choice on the form (M16.6) was rejected. Folded into `04` as the Visit Planning US-015 amendment, EC-NEW-006 and RC-NEW-013 (resolved). The one-off screen was renumbered M-16 because the brief's M-06 is Territory assignment. M-06 Territory assignment is then settled: rep-anchored page; bulk transfer of the assignments themselves (filter, select all, exclude, pick the rep), with M-09 bulk reassign kept for temporary moves; County rows expand to Towns; partial selections become carve-outs and a County moves with its last Towns; the receiving rep's page can pull in place or take over another rep's book. Folded into `04` as a Coverage US-001 amendment (AC-CV001-A–H). M-07 Location coverage is then settled: it is the manager's Location page (coverage first, plus Add one-off visit and History, with no visits section); an unassigned shop leads with assigning its Town and shows how many other shops there are unassigned; a covered shop leads with "Change just this shop". Colm also established that a Location can have many profiles, with defaults resolved field by field to the most demanding value (BR-NEW-005), and that specialist-by-profile-within-an-area is the common coverage change (input for M-10). Folded into `04` as the Coverage US-002 amendment and BR-NEW-005. M-08 Impact preview & handover is then settled: a transfer asks "Is Colm leaving?". If yes, there is no Leave and no per-visit handover, and all open visits go to the new owner unscheduled as visits needed. Inherited visits keep their windows and are marked "inherited from Colm": they count operationally but not in the new rep's performance, until each visit closes. Folded into `04` as the Coverage US-003 amendment. M-09 is then settled: short leave never changes ownership (handled by the M-02 absence flow with Cover or Extend, and the schedule restarts on return); bulk reassign is for long absence only, with open visits moving as inherited visits needed; reversal uses the per-visit Move/Leave handover (the rep may return part-time), restores the derived owner, and flags Locations whose territory now resolves to someone else. Folded into `04` as the Coverage US-004/US-005 amendment. M-10 is then settled: specialist scopes combine any number of conditions (Customer, Profile, Brand, Area) with AND by default, with an Advanced AND/OR mode, and are shown as a sentence with a match count; campaign routing preselects the specialist matching the campaign's link, the manager must choose when that doesn't decide, and the visit falls back to the Primary Rep when no specialist matches. Folded into `04` as the Coverage US-006 amendment. M-11 is then settled: rows open on the chain's recently ordered products with empty cells (search adds more; a toggle shows the full Agreed Range); changing `All` fills only cells not changed by hand, with `Clear adjustments` to reset them; the grid layout (flat vs nested rows) is deliberately deferred until there is usage feedback. Folded into `04` as the Master & Branch US-006 amendment. M-12 is then settled: each rep's figure is pre-filled with the previous period's target (a deliberate exception to propose-don't-impose; blank stays blank); save confirms carried figures and the confirmation names how many were carried unchanged. Folded into `04` as the Targets & Performance US-001/US-002 amendment. M-13 is then settled: "furthest behind" defaults to percentage points behind straight-line pace, re-sortable by € behind pace; for reps with both value and units targets, value decides the order (units shown on the row). Folded into `04` as the Targets & Performance US-005 clarification. M-14 is then settled: two views over one permission record, per rep (onboarding) and per group (training cohorts, several reps with one shared reason, recorded separately). Folded into `04` as the Coverage US-009 amendment. M-15 is then settled: a standing "N Locations unassigned" count sits in the M-01 header whenever the count is above zero, because unassigned shops generate no visits and would otherwise be invisible. Rows follow M7.2. Folded into `04` as the Coverage US-007 amendment. **All manager screens are now settled.** On 23 Sep 2026 the three logged assumptions were closed: M6.4 applies at Region level; M8.2's inherited marker covers carve-out Move; and M15.2 scopes the unassigned count to each manager's area, derived from nearest held geography (same County, then Region, else Head Office), after Colm rejected company-wide on diffusion-of-responsibility grounds. The manager drafting calls were then confirmed: M6.6 action placement by proximity, M7.5 History as its own view, M7.6 layout, M13.3 units-only reps grouped separately under the € sort, M14.2 permission-removal feedforward, and M15.3 M-15 opening on the count that was clicked. **The manager area has no open calls.** On 24 Sep 2026 tablet open question 2 was settled: "not supplied" clears when the rep taps "Told them" on the order (list or T-08, with Undo), with the next Call at the Location as backstop; a later removal re-raises the order (T2.6, T8.3). Q4 was then settled: on Phone, T-06's stock check becomes "Stock mentioned" (T6.5). Q3 was then settled: an order with no Call carries the gaps from the Location's most recent Call, minus products ordered since, with no age limit (T7.13). **Tablet open questions 2–4 are all settled.** Every remaining tablet-day drafting call was then confirmed or amended, so **T-01–T-08 are all settled** (24 Sep 2026). Colm's corrections produced two new business rules: BR-NEW-006 (work is judged as captured; Needs Attention holds only technical faults) and BR-NEW-007 ("as of sync" only when the last sync wasn't today). The quantity popover was then settled as always dismissing (RC-NEW-005). **The tablet-day area has no open questions.** Later on 24 Sep 2026 the customer screens began (`06-customer.md`). Colm corrected the persona: customers buy the same products but rarely the same order, so the landing page is built around frequent products, not Repeat. **C-09 Usual products** (new screen) is settled: the signed-in person's own orders at the shop, products ordered twice in six months (C9.1); any other order for the Location (rep, colleague, chain head office) marks the product, non-blocking (C9.2), as "Ordered … · N on the way", then "Despatched … · expected soon" for a company-set period after despatch (C9.3); a new user sees "Often ordered for [shop]" (Location-wide, same rule) until their 3rd order, then it goes (C9.4, EC-NEW-008); the page is a launchpad with the order on C-04 (C9.5); Add opens an empty quantity popover and stays (C9.6); orders reached by a link only (C9.7). C2.1: a multi-shop user chooses the shop on every visit. Folded into `04` as US-NEW-007 and into `self-service.md` US-003 S6, US-004 S7–10, US-005 S6.

---

## 9. Open questions

### Pick up first

1. ~~**Allowance and FOC cap granularity and accounting.**~~ **Resolved at UX level — eligible products join a provisional commercial policy profile; FOC is X units per rep per month and behaves as ordinary €0.00 order lines.** Still open only at domain-modelling level: policy entity shape (RC-NEW-009).
2. ~~**What clears "not supplied" on Home**~~ **Resolved 24 Sep 2026** — "Told them" per order with Undo; next Call at the Location as backstop (§6.3, RC-NEW-001).
3. ~~**Low tab on an order with no stock check**~~ **Resolved 24 Sep 2026** — carries the gaps (not added, CAN'T ADD) from the Location's most recent Call, minus products ordered since, dated, no age limit (T7.13, RC-NEW-007).
4. ~~**T-06 when channel = Phone**~~ **Resolved 24 Sep 2026** — becomes "Stock mentioned": same suggested rows with a filter box, Low / Out / Add to order, optional "Add count", Out stored as Low + count 0, untouched rows not saved as Not checked (T6.5).

### Head office

5. Do Hold and Reject survive on H-02 with no flag driving them? Affects Head Office US-006. (RC-NEW-003)
6. Do orders appear on H-01 at all, e.g. a read-only feed? (RC-NEW-004)
7. H-10 — draft allocation when an incoming delivery changes.
8. H-16 — does "Leave them in the archived branch" need a second confirmation?

### Smaller

9. ~~Quantity popover: dismiss on add, or advance?~~ **Resolved 24 Sep 2026 — always dismisses** (RC-NEW-005).
10. Spend-threshold discount base — final total (assumed) or pre-discount? (RC-NEW-006)
11. ~~T-02 Overdue below Today~~ **confirmed 24 Sep 2026 (T2.1)**; ~~T-07 footer showing order value in front of customer~~ **resolved 24 Sep 2026 — shown**; ~~T-07 agreed range merged or separate~~ **resolved 24 Sep 2026 — section at top, each product once (T7.14)**; ~~T-05 two equal primary actions~~ **confirmed 24 Sep 2026 (T5.1)**; ~~T-01 no auto-sync~~ **confirmed 24 Sep 2026 (T1.3; reminder per US-002 S2)** — the rest are drafting calls awaiting confirmation.
12. R-01: ~~absence days blocked or droppable~~ **resolved — blocked**; ~~unscheduled horizon~~ **resolved — Overdue plus next four weeks**; ~~one-week window vs 3-week cycles~~ **resolved — selectable Day / Week / Month views**. R-02: apply-to-all scope.
13. 00 conventions: stepper placement for left-handed one-handed use; impact preview dialog vs page threshold; run-out wording on one line or two.

### Carried from brief §11

Tablet snapshot size (now also must carry: applicable commercial policy rules and membership, rep month-to-date FOC usage/remaining allowance, last-3-orders per product per location, each Location's most recent Call's unresolved Low items, products ordered at the Location since), soft-reminder hour, prospect pricing, R-04 corrections screen, editing a live promotion, customer app scope, Location Profile vs Location Type.

---

## 10. Suggested next steps

In the brief's §10 order, with what's done:

1. ~~T-07 order entry and the order line~~ — Low list, pricing and FOC line behaviour settled. Remaining: provisional policy entity shape, plus open questions 3 and 9.
2. **T-02 Home and T-01 Sync** — drafted; confirm drafting calls; settle open question 2.
3. **T-06 Call & stock check** — drafted; settle open question 4.
4. ~~H-01 Worklist and H-02 Order detail~~ — settled. Remaining: open questions 5, 6.
5. ~~**R-01 Planner**~~ — settled in `03`. Includes responsive Day / Week / Month scheduling, fixed four-week unscheduled horizon plus Overdue, immediate drag with editable duration, and structured manager/admin-maintained Due Reason Types with controlled icon/colour choices.
6. ~~H-10 Allocation and H-16 Category archive~~ — settled. Remaining: open questions 7, 8.
7. ~~M- screens~~ — all settled, with no drafting calls or assumptions left (23 Sep 2026). C- screens — not drawn.

### Next round (after 24 Sep 2026)

The tablet day (T-01–T-08) is fully settled, with no open questions. Pick up with **one** of:

1. **Customer screens C-01 to C-08.** Not drawn. Start with jobs-to-be-done for the Customer User: they are online, so `self-service.md` rules out "as of sync" wording, and BR-NEW-006/007 don't apply. Read `../stories/self-service.md` and brief §7 (C screens) and §8 (flows) first.
2. **Head office.** Confirm H-03 Despatch's drafting calls, then §9 Q5–8: Hold/Reject on H-02 (RC-NEW-003), orders on H-01 (RC-NEW-004), H-10 draft allocation when a delivery changes, H-16 second confirmation. After that, the 25 undrawn H- screens, which brief §10 calls conventional.

Ask Colm which one he wants before starting. Keep the same method: one concept, a worked example, then one question; batch simple drafting-call confirmations by screen; fold each decision into `01`–`05`, `04` and the matching source story as it settles.

Carry-forward to watch: BR-NEW-006 (work is judged as captured) may bear on H-02 Hold/Reject (Q5). A held or rejected order must not be rejected for a rule that changed after capture.

### Next session (set by Colm, 23 Sep 2026) — completed 24 Sep 2026

**Tablet open questions 2–4**, in this order:

1. ~~**What clears "not supplied" on Home**~~ **Settled 24 Sep 2026** (T2.6, T8.3; folded into `01`, `04` US-NEW-003 AC-5–10 and EC-NEW-007, and `rep-at-a-location-tablet.md` US-003 Scenarios 12–16).
2. ~~**The Low tab on an order with no stock check**~~ **Settled 24 Sep 2026** (T7.13; `04` US-NEW-002 AC-8–12; source US-012 Scenarios 19–22) — for example a phone order (§9 Q3, RC-NEW-007).
3. ~~**T-06 when channel = Phone**~~ **Settled 24 Sep 2026** (T6.5; folded into `01`, `04` Area 1 US-006/US-007 AC-A1006-A–F, and `rep-at-a-location-tablet.md` US-006 Scenarios 8–12).

~~Then confirm the remaining drafting calls on T-02 and T-01 (§9 Q11).~~ **Done 24 Sep 2026** — every T-01–T-08 drafting call confirmed or amended (T1.1 corrected into BR-NEW-006 judged-as-captured; T5.3 GPS action only while unconfirmed; T6.3 wording "Below low-stock level (6)"; T7.14 agreed range section; T8.1 into BR-NEW-007 stale-only sync chip).

~~Quantity popover (§9 Q9)~~ **settled 24 Sep 2026.**

Next: the customer screens (C-01 to C-08), or the remaining head office screens. Leave the provisional commercial-policy entity shape for the business-rule modelling pass unless it blocks the UX.

### Customer screens — in progress (started 24 Sep 2026)

C-09 Usual products settled (C9.1–C9.7, C9.9); C2.1 settled 24 Sep 2026. All folded into `06-customer.md`, `04` (US-NEW-007, EC-NEW-008) and `../stories/self-service.md`.

**25 Sep 2026 — C2.2 settled:** a Hickey's Head Office buyer mostly signs in to place a chain order. C-02 puts `[ Order for several branches ]` first, opening C-05 directly; the head office location and eligible branches sit below for single-location orders, with no preselection on return. Folded into `06-customer.md`, `04` (AC-NEW-007-12), and `../stories/self-service.md` (US-003 S7). **Next: design C-05**, starting with what products the chain buyer expects to find when the grid opens. M-11 is the rep/manager precedent: products from the chain's recent accepted orders, with empty quantities; its layout remains deferred to usage feedback. C9.8 mark wording remains a drafting call.

**25 Sep 2026 — C5.1 settled:** Colm expects a predefined set for the chain grid: the confirmed Agreed Range held on the Master Location. It may have subsets, for example “2026 Christmas gift packs”. Customer C-05 uses this set rather than M-11's recently ordered products; quantities start empty and permitted products outside the range remain reachable. Folded into `06-customer.md`, `04` (AC-SS006-A), and `../stories/self-service.md` (US-006 S6). **Next question:** how the buyer uses subsets within a chain order, including what is shown first. Do not assume the subset is a separate order or a new catalogue Range entity.

**25 Sep 2026 — C5.2 settled:** Colm confirmed that entered product rows and their quantities remain visible in the grid when the buyer switches from a subset such as Christmas gift packs to everyday products. Folded into `06-customer.md`, `04` (AC-SS006-B), and `../stories/self-service.md` (US-006 S7). **Next:** ask what products appear when C-05 first opens. The grouping of retained rows and phone presentation remain open.

**25 Sep 2026 — C5.3 settled at UX level:** a dropdown lists named ranges, with one designated default automatically selected and shown when C-05 opens. Entered rows remain visible on changing the selection. Folded into `06-customer.md`, `04` (AC-SS006-C), and `../stories/self-service.md` (US-006 S8). **Clarify next:** Master & Branch Ordering currently assumes one Agreed Range per chain; does Colm mean multiple separate Agreed Ranges, or named subsets within one? Do not change that domain rule until he answers. Who designates the default, row grouping and phone presentation remain open.

**26 Sep 2026 — C5.4 settled:** a chain can have multiple separate Agreed Ranges, not just named subsets within one. This supersedes the one-range-per-chain assumption in `../stories/master-branch-ordering.md`. Folded into `06-customer.md`, `04` (BR-NEW-008, AC-SS006-D), `../stories/self-service.md` (US-006 S9), and `../stories/master-branch-ordering.md` (US-001 S5, assumption struck). Earlier examples with one range remain valid examples, not a cardinality rule. **Next:** who designates the customer's default range. Also open: how the rep branch pad, master view, Range Review/proposals, rep/manager grid and tablet snapshot handle several ranges. Do not infer union or default-only behaviour from the old singular examples.

**26 Sep 2026 — C5.5 settled:** the customer designates the default Agreed Range for the customer multi-branch grid. Folded into `06-customer.md`, `04` (BR-NEW-008 rule 4, AC-SS006-E), `../stories/self-service.md` (US-006 S10) and the Master & Branch source amendment. **Next:** is the default shared across the chain's buyers or personal to each buyer? Which customer users may change it remains open; do not infer an account permission model yet.

**26 Sep 2026 — C5.6 settled:** only the buyer who sets the default sees it. Every customer buyer with access to the chain grid may set their own personal default; another buyer's starting range stays unchanged. Folded into `06-customer.md`, `04` (BR-NEW-008 rule 4, AC-SS006-F), `../stories/self-service.md` (US-006 S11) and Master & Branch's amendment. **Next:** what opens before a buyer has set a default? Keep row grouping, phone layout and BR-NEW-008 rep/proposal impacts open.

**26 Sep 2026 — C5.7 settled for the current design:** before the buyer sets a personal default, show the first range in the dropdown automatically. This selection does not itself save a personal preference. Colm may revisit the fallback later; the dropdown's sort order is deferred rather than another UX decision now. Folded into `06-customer.md`, `04` (BR-NEW-008 rule 2, AC-SS006-G), `../stories/self-service.md` (US-006 S12) and the Master & Branch amendment. **Next:** where entered rows stay visible as the buyer changes ranges. Phone layout and cross-surface impacts remain open.

**26 Sep 2026 — C5.8 settled:** entered product rows and their quantities sit at the top of the grid, above the selected range's available products. Folded into `06-customer.md`, `04` (BR-NEW-008 rule 2, AC-SS006-H) and `../stories/self-service.md` (US-006 S13). **Next:** design C-05's phone presentation; also resolve products appearing in several Agreed Ranges. Cross-surface BR-NEW-008 impacts remain open.

**26 Sep 2026 — C5.9 settled:** chain buyers mainly place multi-branch orders on a laptop or larger tablet using the grid. Phone supports the same job with the rep tablet's product-at-a-time pattern: one quantity for selected branches, adjust exceptions, running summary, same branch-order review. Folded into `06-customer.md`, `04` (AC-SS006-I) and `../stories/self-service.md` (US-006 S14). **Next:** resolve what happens when a product belongs to multiple Agreed Ranges; then draw the C-05 desktop and phone frames. The phone's detailed composition is still a drafting task, not a confirmed decision.

**26 Sep 2026 — C5.10 settled:** the same product may be a confirmed member of multiple Agreed Ranges for one chain. Folded into `06-customer.md`, `04` (BR-NEW-008 rule 1, AC-SS006-J), `../stories/self-service.md` (US-006 S15), and `../stories/master-branch-ordering.md` (US-001 S6). **Next:** when an entered product also belongs to the currently selected range, decide whether it appears only in the entered group at the top or again among that range's available products. Then draw C-05. The rep screens must reconcile overlap with T7.14's one-product-one-place rule.

**26 Sep 2026 — C5.11 settled:** an entered product appears only in “Entered in this order” at the top; it is omitted from the selected range's remaining-product rows even if it belongs to that range. Its existing branch quantities are edited from the one top row. Folded into `06-customer.md`, `04` (AC-SS006-K), and `../stories/self-service.md` (US-006 S16). Desktop and phone structure frames were then drafted in `06-customer.md`; these frames are **drafting calls awaiting confirmation**, not new settled decisions. Ask Colm to review the frames one concept at a time. BR-NEW-008 impacts on rep screens and proposals remain open.

**26 Sep 2026 — C5.12 settled:** the customer chain order starts with all eligible branches selected, following the rep multi-branch precedent. Closed branches are excluded; Temporarily Closed branches remain selected and dated; the buyer may deselect branches before entry. Folded into `06-customer.md`, `04` (AC-SS006-L) and `../stories/self-service.md` (US-006 S17); the draft frames now show branch selection. **Next:** what happens if the buyer removes a branch after entering quantities for it? Then confirm the frame composition. C5.7's fallback and dropdown sort order remain deferred.

**26 Sep 2026 — C5.13 settled:** a branch with any quantity in the current in-progress chain order cannot be deselected. The branch stays selected, quantities remain, and a message says there is an active order against it and to clear its quantities first. A zero-quantity branch may be deselected. Folded into `06-customer.md`, `04` (AC-SS006-M), and `../stories/self-service.md` (US-006 S18). **Next:** whether the message should link directly to that branch's entered quantities or merely explain the block; then confirm C-05's draft frames.

**26 Sep 2026 — C5.14 settled:** on blocked removal, open directly on that branch's entered quantities with the reason visible, so the buyer can edit or clear them. The branch remains selected while any quantity remains. Folded into `06-customer.md`, `04` (AC-SS006-N) and `../stories/self-service.md` (US-006 S19). **Next:** after the last quantity is cleared, should the branch be removed automatically or should the buyer return to selection and remove it deliberately? Then confirm the C-05 draft frames.

**26 Sep 2026 — C5.15 settled:** after clearing the last quantity, the branch remains selected until the buyer returns to Branches and deliberately deselects it. Folded into `06-customer.md`, `04` (AC-SS006-O), and `../stories/self-service.md` (US-006 S20). The phone's product-entry state was then sketched as a drafting call. **Next:** confirm the C-05 desktop and phone composition drafts one concept at a time. BR-NEW-008 rep/proposal impacts remain open.

**26 Sep 2026 — C5.16 settled:** Colm accepted the desktop/larger-tablet C-05 hierarchy for the initial round, noting that details will likely change during implementation. Entered lines at top, selected range below, branch quantity columns and running summary into Review. The phone's exact composition remains a draft for implementation feedback, not a blocker to the first-pass screen. C-05 is first-pass settled; tally updated. **Moved to C-03 Catalogue & search.** The source already settles curated assigned Ranges plus unranged products, deliberate Search all products, restricted products hidden and availability wording. Next question: when a curated search has no result, how should the user reach Search all products with their query preserved?

**26 Sep 2026 — C3.1 settled:** Colm replaced the source's two-step search with one search. Curated matches appear first, and other products the Customer may buy appear underneath in the same results. No widening action or repeated query. Category browsing remains curated as a first-pass inference from the request, pending the next discussion. Folded into `06-customer.md`, `04` (AC-SS004-A/B), and `../stories/self-service.md` (US-004 glossary, flow, scenarios). A search frame is drafted, moving C-03 from undrawn to in progress. **Next:** confirm how browsing without a query should work.

**26 Sep 2026 — C3.2 settled:** without a query, browse the curated catalogue first and offer “View more products” to browse other products the Customer may buy. Restricted products remain hidden. Folded into `06-customer.md`, `04` (AC-SS004-C), and `../stories/self-service.md` (US-004). A browse frame is drafted. **Next:** determine whether “View more products” opens a wider browse state in place or a separate page.

**26 Sep 2026 — C3.3 settled:** Colm chose a simple same-page switch with a way back. Drafted as a two-option “Your catalogue” / “All products” control above browse categories. “Your catalogue” is the initial selection; category context is retained on the return where possible. Search remains grouped across both scopes regardless of browse selection. Folded into `06-customer.md`, `04` (AC-SS004-D), `../stories/self-service.md` (US-004), and the screen inventory brief. Availability row examples were then drawn from existing US-004 wording; exact presentation remains a drafting call. **Next:** review C-03 as a first-pass whole, then move to C-04.

**26 Sep 2026 — C-03 first-pass settled:** search groups curated matches before other permitted matches; category browsing starts curated and widens on the same page using “Your catalogue” / “All products”; the return path is the same control. Existing US-004 availability messages appear on catalogue/search rows. The screen's exact styling remains for implementation feedback. C-04 has been started with a job and source summary. **Next:** resolve the edit/cancel promise after submission now that orders are accepted automatically on receipt.

**26 Sep 2026 — C4.1 settled:** the customer confirms the order by placing it. They may correct it beforehand, but afterward it is read-only in self-service, including any transient Pending state. For a part cancellation or amendment they contact the company; the customer does not make the change. Company-side mechanics remain a separate unresolved operational design question, especially because the head-office source currently forbids editing accepted order lines. Folded into `06-customer.md`, `04` (AC-SS005-A, AC-SS007-A, AC-SS008-A), `../stories/self-service.md` (US-005, US-007, US-008), `../stories/head-office-order-processing.md` (order-state note), and the screen inventory. C-04 review frame drafted. **Next:** decide how the contact route should appear on the placed order detail.

**26 Sep 2026 — C4.2 settled:** show both company phone number and email address alongside the order reference on the placed-order detail. The same contact route applies to branch orders created through C-05. Folded into `06-customer.md`, `04` (AC-SS007-B), and `../stories/self-service.md` (US-006/US-007). C-04 review and placed-order frames are first-pass ready; exact styling may change during implementation. C-04 counted settled and C-06 started. **Next:** decide whether C-06 groups branch orders from one chain submission or shows each as a separate chronological row.

**26 Sep 2026 — C6.1 settled:** a chain submission is one expandable entry in C-06 order history; inside it, each branch retains its own order reference, status and value and opens its read-only detail. Single-location orders remain ordinary rows. Folded into `06-customer.md`, `04` (AC-SS008-B), `../stories/self-service.md` (US-008 S5), and the screen inventory. A first-pass history frame is drafted. **Next:** decide how a Location filter affects a chain group when only one branch matches.

**26 Sep 2026 — C6.2 settled:** a single-branch Location filter shows a chain-created branch order as an ordinary chronological row, without the expandable chain wrapper. The wrapper remains for the all-Locations view. Folded into `06-customer.md`, `04` (AC-SS008-C), `../stories/self-service.md` (US-008 S6), and the screen inventory. C-06 counted first-pass settled; C-07 started with its source rules. US-009 scenario 5 was aligned with automatic acceptance and C4.1. **Next:** decide whether Repeat on the chain group can repeat an entire submission, or only its individual branch orders.

**26 Sep 2026 — C7.1 settled:** Repeat is available on an individual branch order, not on the chain submission group. It opens a new, unsubmitted single-location order for that branch, using the source's all-lines or selected-lines rules. Whole-chain Repeat is deferred as a possible future change. Folded into `06-customer.md`, `04` (AC-SS009-A), `../stories/self-service.md` (US-009 S1/S6), and the screen inventory. C-07 detail frame drafted. **Next:** can a customer repeat a visible order placed by a colleague or rep, or only their own?

**26 Sep 2026 — C7.2 settled:** Repeat is offered on every order visible in C-06 for the customer's Locations, whether the customer, a colleague or the rep placed it; the detail screen is identical for all of them. Because prices resolve fresh (US-009 S4), a rep's discount on the original is not copied. Folded into `06-customer.md`, `04` (AC-SS009-B), `../stories/self-service.md` (Repeat definition, US-009 S7), and the screen inventory. **Next:** what Repeat does when the Location already has an unplaced order with lines — nothing in US-005 or US-009 covers it.

**26 Sep 2026 — C7.3 settled:** Repeat adds the chosen lines to the Location's one unplaced order (C9.5), keeping what is already there; it starts a new order only when none is in progress. Replace and an Add/Replace prompt were rejected. Folded into `06-customer.md`, `04` (AC-SS009-C), `../stories/self-service.md` (Repeat definition, US-009 S8), and the screen inventory. **Next:** when a repeated product is already in the current order (12 Hand Cream there, 12 on the repeated order), what quantity results?

**26 Sep 2026 — C7.4 settled:** a repeated product already in the current order stays on one line and takes the repeated order's quantity. If any quantity would change, one warning (never one per product) lists each change before anything is applied; Continue applies the whole repeat, Cancel leaves the order untouched. Continue/Cancel is Claude's reading of "one warning" — a drafting detail. Folded into `06-customer.md` (with a warning frame), `04` (AC-SS009-D/E), `../stories/self-service.md` (US-009 S9), and the screen inventory. C-07 counted first-pass settled. C-01 started with job and source summary. **Next:** US-002 S2 gives an expired invitation two recovery paths — "contact your sales representative" and "request a new one". Decide which, and whether a self-requested invitation needs the manager approval that US-001 S1 requires for rep-created accounts.

**26 Sep 2026 — C1.1 settled:** an expired invitation offers "Request a new invitation". The request goes to the rep or manager and a new invitation is sent only after one of them approves it; a repeat visit shows the pending request rather than creating a duplicate. Automatic resend and a contact-only dead end were rejected. Folded into `06-customer.md` (expired and request-sent frames), `04` (AC-SS002-A/B, new index row), `../stories/self-service.md` (US-002 S2, S4), and the screen inventory. **Next:** who receives the request — the Location's rep, the manager, or either — and where they act on it.

**26 Sep 2026 — C1.2 settled:** the rep responsible for the contact's Location approves a re-invitation request; no manager step, because the account was approved at creation. With no assigned rep, it goes to the manager who sees the Location as Unassigned (M15.2) — confirmed by Colm the same day. Folded into `06-customer.md`, `04` (AC-SS002-C), `../stories/self-service.md` (US-002 S5), and the screen inventory. **Next:** where the rep sees and acts on the request. T-02 Needs Attention holds only technical faults (T1.1, 24 Sep note), so it has no obvious home yet.

**26 Sep 2026 — C1.3 settled; no-rep fallback confirmed:** the rep sees and approves re-invitation requests on T-02 Home ("Send new invitation"). T-05 and R-01 were rejected. Because tablet sync is deliberate, the request arrives at the rep's next sync and the invitation leaves at the sync after approval. Colm also confirmed C1.2's fallback: no assigned rep → the manager who sees the Location as Unassigned. Folded into `06-customer.md`, `01-tablet-day.md` (T-02 addition note), `04` (AC-SS002-D), `../stories/self-service.md` (US-002 S5–S6), and the screen inventory (C-01, T-02). **Next:** placement on Home — a fifth counter in the exception strip, which T2.5 says should force a rethink of the strip, or its own section.

**26 Sep 2026 — C1.4–C1.6 settled:** (C1.4) requests get their own collapsed "Customer requests" section on Home, shown only when one is waiting; the strip stays at four counters. Its position as the first collapsed section is a drafting call. (C1.5) Colm: "notifications like this should sync when the rep is in range" — customer requests are delivered in the background whenever the tablet has signal. **This is a narrow exception to settled T1.3** (manual sync both ways); orders, calls and all other work remain manual, and the strip counts still change only on a manual sync. (C1.6) A request also emails the rep, or the manager for an unassigned Location. Folded into `06-customer.md`, `01-tablet-day.md` (T-02 section frame; T1.3 exception note), `04` (AC-SS002-D/E), `../stories/self-service.md` (US-002 S6), and the screen inventory. **Next:** does "Send new invitation" also go out in the background, or wait for the rep's manual sync?

**26 Sep 2026 — C1.7 settled; C-01 first-pass settled; C-08 started:** "Send new invitation" also leaves in the background as soon as there is signal, so the whole customer-request loop runs outside manual sync (T1.3 exception now both directions, customer requests only). Drafting call: a sent request stays greyed with "Sent 14:20" until the rep leaves Home; offline it shows "Will send when in range". Folded into `06-customer.md`, `01-tablet-day.md`, `04` (AC-SS002-D), `../stories/self-service.md` (US-002 S6), and the screen inventory. C-01 counted first-pass settled; invitation lifetime and password policy stay as source questions. C-08 started with a source summary; no drawn rep or manager screen yet creates or approves customer accounts. **Next:** where does the rep create a customer account — the tablet (from the contact on T-05) or the rep website?

**26 Sep 2026 — C8.1 settled:** the rep sets up online ordering on the tablet, from the contact reached through T-05, usually with the contact present; the request uses the C1.5/C1.7 background channel. Drafting call: tapping the contact opens their details with "Set up online ordering" (or "Online ordering: Active"). Contact frame drafted in `06-customer.md`; T-05 addition note in `01-tablet-day.md`; `04` (AC-SS001-A/B, new index row); `../stories/self-service.md` (US-001 S7); screen inventory (C-08, T-05). Also this session: gendered pronouns for Mary Walsh removed from new UI strings and ACs (names used instead). **Next:** where the manager approves a rep-created account — no drawn manager screen handles it.

**26 Sep 2026 — C8.2 / M17.1 settled; M-17 added:** a new manager page, **M-17 Online ordering approvals** (not in the brief, numbered after M-16), holds rep-created accounts and re-invitations for unassigned Locations. The manager is emailed with a link to it; approval happens on the page, where Decline requires the reason the rep sees. M-01's exceptions column and email-only approval were rejected. Drafting calls: two groups, Approve acts at once, inline required decline reason. Folded into `05-manager.md` (new section and frame), `06-customer.md`, `04` (AC-SS001-C/D, index row), `../stories/self-service.md` (US-001 S8), and the screen inventory (new M-17 entry). **Next:** where the rep sees a decline reason (T-02 Customer requests section, the contact on T-05, or both); then where a manager creates an account directly (US-001 S2).

**26 Sep 2026 — C8.3 settled:** a manager's decline reaches the rep who set up the account in T-02's Customer requests section, over the background channel ("Mary Walsh: online ordering declined: Contact has left the business"). The contact on T-05 alone was rejected. Drafting calls: one **OK** action removes the notice; the section keeps its name although it now holds outcomes as well as requests. Folded into `06-customer.md` (notice frame), `01-tablet-day.md` (T-02 note), `04` (AC-SS001-E), `../stories/self-service.md` (US-001 S9), and the screen inventory. **Next:** where a manager creates an account directly (US-001 S2, no approval).

**26 Sep 2026 — C8.4 settled; C-08 first-pass settled; every customer screen drawn:** a manager sets up online ordering from M-07's footer row ("Set up online ordering…", beside Add one-off visit), picks the contact, sees the scope line and the invitation goes out on save with no approval. This extends M7.6's footer (noted as an addition, not a rewrite). A "Set up an account" button on M-17 was rejected. Folded into `06-customer.md`, `05-manager.md` (M-07 addition; M-17 open item resolved), `04` (AC-SS001-F, index row), `../stories/self-service.md` (US-001 S10), and the screen inventory (C-08, M-07).

**Drafting calls from 26 Sep — all confirmed by Colm 26 Sep 2026, as drafted:** C9.8 mark wording; C7.4 warning offers Continue/Cancel before applying; C1.4 Customer requests is the first collapsed section on Home; C1.7 sent request greys with "Sent 14:20", offline "Will send when in range"; C8.1 tapping the contact on T-05 opens their details; C8.3 single OK on a decline notice and the section keeping its name; M-17 grouping, Approve acting at once and inline decline reason.

**Next:** BR-NEW-008 cross-area follow-up — how several Agreed Ranges appear on T7.14 (rep order pad) and M11.1 (`Show full Agreed Range`).

~~Also awaiting Colm: **C9.8** mark wording.~~ **Confirmed 26 Sep 2026.**

Remaining open questions in `06-customer.md`:

1. ~~**C-03 Catalogue** — does the C9.2 "ordered by someone else" mark also show on catalogue and search rows?~~ **Resolved — C-09 only (C9.9).**
2. ~~**C-02** — the chain head office buyer and entry to C-05.~~ **Resolved 25 Sep 2026 (C2.2):** chain order leads.
3. **C-05** — first-pass settled; phone details, C5.7 fallback and dropdown sort order can be revisited during implementation. BR-NEW-008 cross-surface impacts remain open. **C-03/C-04/C-06** — first-pass settled; exact styling later. Company-side changes to accepted orders need separate operational design. **C-07** — whole-chain Repeat deferred (C7.1); any visible order can be repeated, whoever placed it (C7.2); Repeat adds to the Location's current unplaced order (C7.3); a product already there takes the repeated quantity after one warning (C7.4); first-pass settled. **C-01** — expired invitation offers "Request a new invitation", approved by the Location's rep in its own T-02 Home section, requests and approvals move in the background, with an email (C1.1–C1.7); first-pass settled. **C-08** — rep creates on the tablet from the contact on T-05, sent in the background (C8.1); manager approves on new M-17, prompted by email (C8.2); declines reach the rep in T-02's Customer requests section (C8.3); a manager creates one directly from M-07 (C8.4). First-pass settled.
4. ~~**C-04** — editing while Pending now orders are accepted on receipt (Self-service RC 7).~~ **Resolved 26 Sep 2026 (C4.1–C4.2):** placement is confirmation, no customer edit/cancel afterward, phone and email shown with order reference.
5. ~~**C9.8** — confirm mark wording.~~ **Confirmed 26 Sep 2026:** "your rep", colleague's name, chain head office by name.

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
