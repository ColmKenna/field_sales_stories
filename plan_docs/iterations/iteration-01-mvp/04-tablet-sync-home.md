# E4 — Tablet sync and Home

**Iteration** — 1, MVP: orders from the field reach the warehouse
**Outcome** — A rep signs in, syncs the day's data, finds any assigned shop offline, and every captured item reaches the server exactly once or waits on the tablet with a stated cause. Restricted products never reach a tablet without permission.
**Stories**

| # | Story | Priority | Delivered here |
|---|---|---|---|
| 1 | Rep at a Location US-001 — Sync my tablet | Must | S1–S5 (S4's example reason is superseded by BR-NEW-006; the partial-rejection behaviour is kept for technical faults, MI-52) |
| 2 | Rep at a Location US-004 — Find and open any assigned Location | Must | S1–S4 (S2's Visit Due with Due Reason arrives with E13) |
| 3 | Rep at a Location US-017 — Resolve items in Needs Attention | Must | S1–S7 |
| 4 | Rep at a Location US-019 — Hide Restricted Products from reps without permission | Must | S1–S4 |
| 5 | Rep at a Location US-003 — See today as a record and what's at risk | Must | S2, S5 and the unsent count (S1, S3, S4, S6 → E13; S7a → E14; S7 → E15; S8–S16 → E8) |

**Exit criterion** — On a tablet with signal, a rep signs in and syncs: finished calls and orders upload first, each exactly once and judged as captured, then the day's data downloads in full. Offline, the rep searches 500 assigned shops in under a second, opens one, and sees Home as the record of the day with the unsent count. Failures say why. Restricted groups the rep lacks never reach the device.

**Capability-class stamp** — Frontier + extended reasoning for the sync slivers (T-4.1.1, T-4.1.2); Frontier workhorse for the other tight-loop and assisted tasks and for scenario drafting. Concrete models: see [../00-index.md](../00-index.md).

**Ownership source** — classified in this plan from the stories alone; every tier is `(inferred)`.

**Spec sources** — [rep-at-a-location-tablet.md](../../stories/rep-at-a-location-tablet.md), [01-tablet-day.md](../../uxdocs/01-tablet-day.md) (T-01, T-02, T-04, T-05), [00-conventions-and-shared-elements.md](../../uxdocs/00-conventions-and-shared-elements.md) (§3 "As of sync", §4 Unsent work), [04-user-stories-amendments.md](../../uxdocs/04-user-stories-amendments.md) (BR-NEW-006, BR-NEW-007).

---

### T-4.1.1-S — Test scenarios for tablet sign-in and snapshot download

**Owner** — Human-Led
**Gates** — T-4.1.1
**Produces** — a scenario list: names plus one-line intent, no code

Questions to answer while writing it:
- What must the first snapshot contain for Iteration 1 — assigned Locations (town, address, main contact), non-restricted products with categories, today's and future base prices, units? List every field; later tasks add to it.
- What happens if the download is interrupted halfway: the old snapshot stays whole and in use, never a mix?
- What proves "a snapshot download never alters Unsent items"?
- What is the size and time budget on the target device (MI-02), and at what size does a scenario fail?
- How long does a tablet sign-in last, and what happens when it expires offline?

---

### T-4.1.1 — Sign in on the tablet and download the day's data offline

**Parent story**

> As a Field Salesperson, I want one Sync that sends my finished work and then refreshes my data so that head office gets my Calls and Orders and I work from current Locations and products.
>
> Acceptance criteria:
> - With signal, Sync downloads the Morning Snapshot and Home shows "Last synced 07:42" (S1, download part)
> - A snapshot download never alters Unsent items (edge case)
> - No signal shows "No connection — your work is saved on this tablet" (edge case)

**Slice** — A rep signs in on the tablet, taps Sync and gets their assigned shops and the catalogue offline, replaced in one piece, with Home showing when they last synced.
**Spec source** — Rep at a Location US-001 S1 (download), edge cases and non-functional notes; design decision "Deliberate Sync that always downloads"; uxdocs 01 T-01, T-02 (T1.3: no automatic sync)
**Depends on** — T-3.1.1, T-1.2.1
**Pattern to follow** — novel — see design notes (first tablet slice)
**Ownership** — Impl: Human Tight-Loop | Test: Human-Led | Complexity: C | Confidence: L
  (inferred) — carries the offline storage and snapshot spike (MI-02) and the tablet stack choice (MI-01); Blast Radius High; Edge-Case Discovery High (atomic swap, interrupted download, unsent work untouched).

**Provisional commit message**

```
feat(sync): download the rep's day onto the tablet offline

- The tablet works all day from a morning snapshot, so the download
  replaces the whole snapshot at once or not at all
- Unsent work is stored apart from the snapshot, so a download can never
  overwrite something the rep captured
```

**Capability class** — Frontier + extended reasoning · effort: Anthropic high budget / OpenAI xhigh / Google high
Offline storage and snapshot design are the largest technical unknown in the plan.

**Work package**

Increments:
1. Spike (MI-02): build a synthetic snapshot at the full projected size — catalogue with breadcrumbs, attributes, prices, tiers and breaks, promotions, suggested-list history, thresholds, replacements, leads, a master's branches — and measure download, storage and search time on the target tablets. Record the result and the storage choice.
2. Tablet app shell on the chosen stack (MI-01) with rep sign-in and a session that survives offline use.
3. Snapshot contract v1: assigned Locations (town, address, main contact), non-restricted products with category breadcrumbs, base prices (today and future-dated), units and steps. Versioned from the start.
4. Server snapshot builder per rep, using T-3.1.1's resolver for assigned Locations.
5. Download that stages the new snapshot and swaps it in only when complete; the previous snapshot stays in use until then.
6. Unsent work (calls, orders, schedule changes) stored separately from the snapshot.
7. Home shows "Last synced 07:42"; with no signal Sync shows "No connection — your work is saved on this tablet".

Decision points:
- Full snapshot every sync, or deltas? The stories say "always downloads"; the spike decides whether full is affordable.
- One snapshot per rep built on demand, or pre-built each morning?
- How does the contract version, so an older tablet app can still sync after a server release?
- Who owns the contract? Every later tablet task changes it; name one owner (see the index's parallel lanes).

Delegable slivers:
- **Synthetic snapshot generator** — Write a generator that produces a snapshot at a given scale (products, locations, history depth) in the agreed contract format for the spike. Do not touch production code.
- **Staged-swap tests** — Given the agreed scenarios from T-4.1.1-S, write tests proving an interrupted download leaves the previous snapshot whole and unsent work untouched. Do not change the swap logic; report failures.
- **Last-synced and no-connection display** — Show "Last synced 07:42" on Home and "No connection — your work is saved on this tablet" when Sync has no signal, reading the stored sync time. Change nothing else.

---

### T-4.2.1-S — Test scenarios for offline location search

**Owner** — Scenario Review
**Gates** — T-4.2.1
**Produces** — a scenario list: names plus one-line intent, no code

**Agent prompt**

```
Draft the test scenario list for offline search across the rep's assigned
Locations (task T-4.2.1). Read plan_docs/stories/rep-at-a-location-tablet.md
US-004 S1, S3 and the non-functional note (results within 1 second for 500
Locations), and plan_docs/uxdocs/01-tablet-day.md T-04 (T4.1). Output one
line per scenario as Should_Outcome_When_Condition, then "→" and a one-line
intent. Cover S1 and S3, then derivable edges: apostrophes ("Murphy's"),
accents, partial words, a Closed Location, 500 Locations on the slowest
supported tablet. Mark undecided cases as "Needs a decision". Write no test
code; change no files.
```

---

### T-4.2.1 — Search assigned shops offline

**Parent story**

> As a Field Salesperson, I want to search all my assigned Locations offline and open one so that I can record an unplanned Call or take a phone Order.
>
> Acceptance criteria:
> - With 312 assigned Locations and no signal, typing "Murphy" shows Murphy's Pharmacy (Rathdrum) and Murphy's Stores (Aughrim) (S1)
> - Typing "Quinn" with no match shows "No assigned locations match 'Quinn'" with a note that only assigned Locations are searchable (S3)
> - Results update within 1 second for 500 Locations (non-functional)

**Slice** — Offline, a rep types part of a shop's name and sees matching assigned shops with their town within a second, or a message that only assigned shops are searchable.
**Spec source** — Rep at a Location US-004 S1, S3, non-functional note; uxdocs 01 T-04 (T4.1)
**Depends on** — T-4.1.1
**Pattern to follow** — T-4.1.1 (snapshot access)
**Ownership** — Impl: Agent-Assisted | Test: Scenario Review | Complexity: M | Confidence: M
  (inferred) — device performance target (Test Safety Net Medium); settled design; checkpoint on the index against 500 fixtures.

**Provisional commit message**

```
feat(tablet): search assigned locations offline

- Unplanned calls and phone orders start from search, so it must work with
  no signal and answer within a second across a full book
```

**Capability class** — Frontier workhorse · effort: Anthropic standard budget / OpenAI high / Google high
Performance work on the device needs a measured pause.

**Agent prompt**

```
Role: You are building offline Location search (T-04) for the Field Sales
Management System's rep tablet app.

Context:
- Slice: offline, a rep types part of a shop's name and sees matching
  assigned shops with their town within a second, or a message that only
  assigned shops are searchable.
- Specs: plan_docs/stories/rep-at-a-location-tablet.md US-004 S1, S3 and
  non-functional notes; plan_docs/uxdocs/01-tablet-day.md T-04 (DECISION
  T4.1: the "assigned only" explanation appears on the empty result, not
  above the field).
- Stack: {{STACK}}. Code locations: {{PLACEHOLDER}} — snapshot store from
  T-4.1.1.
- Pattern to follow: read Locations from T-4.1.1's snapshot store.

Acceptance criteria:
1. With 312 assigned Locations and no signal, typing "Murphy" shows
   "Murphy's Pharmacy (Rathdrum)" and "Murphy's Stores (Aughrim)"; the name
   is dominant, the Town secondary.
2. Typing "Quinn" with no match shows "No assigned locations match 'Quinn'"
   with a note that only assigned Locations are searchable.
3. Results update within 1 second for 500 Locations on the slowest
   supported tablet.
4. Rows are large one-handed touch targets and open the Location (T-4.2.2).

Constraints:
- Use the project's existing conventions and test framework.
- Search reads only the snapshot; no network calls.
- No tests of framework internals or trivial members.
- Irreversible or schema-altering operations are out of scope (a local
  search index is allowed).

Gates: stay read-only until I reply "Approved" to your plan; do not modify
existing files until I reply "Apply".
Checkpoint: after the search index is built and timed against 500 fixture
Locations on a target device, stop and show the timings. Resume only on
"Continue T-4.2.1".

Steps: 1. local index over snapshot Locations; 2. search screen per T-04;
3. empty result per T4.1; 4. tests from agreed scenarios.

Test expectations: implement the scenarios agreed in T-4.2.1-S; do not design
your own.

Definition of done: offline, a rep types part of a shop's name and sees
matching assigned shops with their town within a second, or a message that
only assigned shops are searchable.

Self-verification (report each line pass or fail with evidence):
- [ ] Criteria 1–4 demonstrated
- [ ] Timing measured on a device, not an emulator
- [ ] Provisional commit message reconciled with the actual diff

Out of scope: the Location screen (T-4.2.2), product search (T-5.1.1).
```

**Checkpoint**

Produces before pausing — the local search index and its measured timings on a target device with 500 Locations.
Human reviews — Is the second-per-search target met with headroom for the snapshot to grow?
Resume trigger — `Continue T-4.2.1`

---

### T-4.2.2-S — Test scenarios for the tablet Location screen

**Owner** — Scenario Review
**Gates** — T-4.2.2
**Produces** — a scenario list: names plus one-line intent, no code

**Agent prompt**

```
Draft the test scenario list for the tablet Location screen (task T-4.2.2).
Read plan_docs/stories/rep-at-a-location-tablet.md US-004 S2, S4 and
plan_docs/uxdocs/01-tablet-day.md T-05 (T5.1, T5.2). Output one line per
scenario as Should_Outcome_When_Condition, then "→" and a one-line intent.
Cover S2 and S4, then derivable edges: a Location with no contacts, no recent
calls or orders, an unsent order at a Location reassigned since the
snapshot. Visit Dues are out of scope here. Mark undecided cases as "Needs a
decision". Write no test code; change no files.
```

---

### T-4.2.2 — Open a shop's Location screen on the tablet

**Parent story**

> As a Field Salesperson, I want to search all my assigned Locations offline and open one so that I can record an unplanned Call or take a phone Order.
>
> Acceptance criteria:
> - Opening Doyle's Shop shows address, Main Contact, recent Calls and Orders, and Record Call and New Order actions (S2; any Visit Due with Due Reason arrives with E13)
> - Doyle's Shop reassigned away in the new snapshot, with an Unsent Order, shows "No longer assigned to you — your existing order will still be sent" and offers neither New Order nor Record Call (S4)

**Slice** — A rep opens an assigned shop and sees its address, main contact, recent calls and orders, with Record Call and New Order side by side — or, if it was reassigned since, a notice that their unsent order will still be sent and no new actions.
**Spec source** — Rep at a Location US-004 S2, S4; uxdocs 01 T-05 (T5.1, T5.2)
**Depends on** — T-4.2.1
**Pattern to follow** — T-4.2.1 (tablet screen)
**Ownership** — Impl: Agent-Assisted | Test: Scenario Review | Complexity: M | Confidence: M
  (inferred) — first tablet detail screen, which later tablet screens copy; settled design.

**Provisional commit message**

```
feat(tablet): open a location with its contact, history and actions

- The Location screen is the hub for everything at one shop, so calling
  and ordering are offered as equals
- A shop reassigned since capture keeps its unsent order (valid when
  captured) but offers nothing new
```

**Capability class** — Frontier workhorse · effort: Anthropic standard budget / OpenAI high / Google high
Sets the tablet detail-screen pattern.

**Agent prompt**

```
Role: You are building the tablet Location screen (T-05) for the Field Sales
Management System.

Context:
- Slice: a rep opens an assigned shop and sees its address, main contact,
  recent calls and orders, with Record Call and New Order side by side; a
  shop reassigned since shows that the unsent order will still be sent and
  offers no new actions.
- Specs: plan_docs/stories/rep-at-a-location-tablet.md US-004 S2, S4 and
  glossary (Valid when captured); plan_docs/uxdocs/01-tablet-day.md T-05
  (T5.1 two equal primary actions side by side; T5.2 the inactive main
  contact sits in the identity block).
- Stack: {{STACK}}. Code locations: {{PLACEHOLDER}} — snapshot store
  (T-4.1.1), search (T-4.2.1).
- Pattern to follow: T-4.2.1's screen structure.

Acceptance criteria:
1. Opening Doyle's Shop shows its address, Main Contact, recent Calls and
   Orders, and Record Call and New Order as equal actions side by side.
2. If Doyle's Shop was reassigned away in the new snapshot and the rep has
   an Unsent Order for it, the screen shows "No longer assigned to you — your
   existing order will still be sent" and offers neither New Order nor Record
   Call.
3. A Location with no recent calls or orders shows none without error.
4. Recent Calls and Orders open their read-only views once those exist
   (T-7.5.1); until then they are listed only.

Constraints:
- Use the project's existing conventions and test framework.
- The snapshot must keep reassigned Locations that have unsent work (add
  that rule to the snapshot builder only if T-4.1.1 hasn't, and say so).
- No tests of framework internals or trivial members.
- Irreversible or schema-altering operations are out of scope.

Gates: stay read-only until I reply "Approved" to your plan; do not modify
existing files until I reply "Apply".
Checkpoint: after the screen renders for an assigned and a reassigned shop
against T-05, stop and show both. Resume only on "Continue T-4.2.2".

Steps: 1. Location read model from the snapshot; 2. T-05 layout; 3. the
reassigned state; 4. tests from agreed scenarios.

Test expectations: implement the scenarios agreed in T-4.2.2-S; do not design
your own.

Definition of done: a rep opens an assigned shop and sees its address, main
contact, recent calls and orders, with Record Call and New Order side by
side — or, if it was reassigned since, a notice that their unsent order will
still be sent and no new actions.

Self-verification (report each line pass or fail with evidence):
- [ ] Criteria 1–4 demonstrated
- [ ] Layout matches T-05
- [ ] Provisional commit message reconciled with the actual diff

Out of scope: Visit Due with Due Reason (E13), GPS capture and temporary
closure (E17), main contact status wording (T-2.5.2), recording a call
(T-5.3.1), building an order (T-5.1.1).
```

**Checkpoint**

Produces before pausing — the Location screen rendered for an assigned shop and a shop reassigned since the snapshot.
Human reviews — Does it match T-05, with the two actions equal and the reassigned state unmistakable?
Resume trigger — `Continue T-4.2.2`

---

### T-4.1.2-S — Test scenarios for uploading captured work exactly once

**Owner** — Human-Led
**Gates** — T-4.1.2
**Produces** — a scenario list: names plus one-line intent, no code

Questions to answer while writing it:
- Which items upload: saved Calls and Ready to Send Orders only; never In Progress (S1)? Schedule changes arrive in E13.
- How is a retry after a lost confirmation recognised as the same item, so nothing is ever duplicated (edge case)?
- What does "judged as captured" check on the server — which rules, against which snapshot version (BR-NEW-006, S5)?
- What is the smallest set of technical faults the server may reject for (incomplete upload, duplicate submission, corrupt payload)?
- Does the order in which items upload matter (a Call and the Order linked to it)?

---

### T-4.1.2 — Upload finished work first, each item exactly once

**Parent story**

> As a Field Salesperson, I want one Sync that sends my finished work and then refreshes my data so that head office gets my Calls and Orders and I work from current Locations and products.
>
> Acceptance criteria:
> - With 2 saved Calls, 1 Ready to Send and 1 In Progress Order, Sync uploads the 2 Calls and the Ready to Send Order, each marked Sent as the server confirms it; the In Progress Order stays unchanged; then the snapshot downloads (S1)
> - An order for a product that became unavailable after the snapshot is accepted and marked Sent; head office handles the line (S5; BR-NEW-006)
> - Partial uploads never duplicate items (edge case)

**Slice** — Sync uploads each saved call and ready order, marks each Sent only when the server confirms it, never uploads the same item twice, and then downloads the new snapshot.
**Spec source** — Rep at a Location US-001 S1 (upload), S5, edge cases; glossary (Sync, Valid when captured); uxdocs 04 BR-NEW-006
**Depends on** — T-4.1.1, T-5.1.1
**Pattern to follow** — T-4.1.1 (sync session)
**Ownership** — Impl: Human Tight-Loop | Test: Human-Led | Complexity: C | Confidence: L
  (inferred) — data integrity: losing or duplicating an order is the costliest failure in the system; idempotency design depends on the stack (MI-01).

**Provisional commit message**

```
feat(sync): upload captured work first, exactly once

- Upload always runs before download so new data can never overwrite
  unsent work
- Each item carries an identity and its snapshot version, so a retry after
  a lost confirmation is recognised, and the server judges it as captured
```

**Capability class** — Frontier + extended reasoning · effort: Anthropic high budget / OpenAI xhigh / Google high
Exactly-once delivery across an unreliable connection.

**Work package**

Increments:
1. Item identity generated on the tablet at capture; server-side idempotent receive keyed on it.
2. Each Call and Order carries the snapshot version it was captured against.
3. Server receive: validate structure only (technical faults), never current business rules (BR-NEW-006); store as received; confirm per item.
4. Tablet: upload Ready to Send Orders and saved Calls one at a time; mark Sent only on confirmation; leave In Progress untouched.
5. After uploads, run T-4.1.1's download regardless of upload outcome.
6. Received orders enter the order lifecycle as Pending (hand-off point for T-7.1.1).

Decision points:
- What identifies an item across retries — a tablet-generated identifier, or identifier plus content hash?
- If the server confirms but the confirmation is lost, the next sync re-sends: the server must reply "already received" and the tablet mark it Sent. Confirm this is the whole protocol.
- Does a Call upload before its linked Order, and what if only one gets through?
- Which technical faults exist at all (MI-52)?

Delegable slivers:
- **Idempotent receive tests** — Given the agreed scenarios from T-4.1.2-S, write server tests proving a re-sent item is stored once and confirmed again. Do not change the receive logic; report failures.
- **Upload progress display** — Show "Sending 2 of 3" during upload from the existing upload queue's counts. Do not change the upload logic.

---

### T-4.1.3-S — Test scenarios for sync failure causes

**Owner** — Human-Led
**Gates** — T-4.1.3
**Produces** — a scenario list: names plus one-line intent, no code

Questions to answer while writing it:
- Signal drops after 1 of 3 items is confirmed: exactly what state is each item in, and what does the rep read (S2)?
- Sign-in expired: nothing uploads and the message is "Sign in again to send 3 items" with Sign in (S3) — does the download still run?
- What does the rep see when upload succeeds and download fails?
- How are these faults reproduced in tests (a fault-injection harness)?

---

### T-4.1.3 — Say why a sync didn't complete

**Parent story**

> As a Field Salesperson, I want one Sync that sends my finished work and then refreshes my data so that head office gets my Calls and Orders and I work from current Locations and products.
>
> Acceptance criteria:
> - Signal dropping after the server confirms 1 of 3 items leaves that item Sent and the other 2 unchanged, with "2 items not sent — poor connection. They'll send next Sync."; the download is attempted when signal allows (S2)
> - With sign-in expired, nothing uploads and "Sign in again to send 3 items" appears with a Sign in action; all work stays on the tablet (S3)
> - Progress shows "Sending 2 of 3" (non-functional)

**Slice** — When a sync can't complete, the rep is told why in a sentence — poor connection, no connection or sign-in expired — with what was sent, what wasn't, and what to do.
**Spec source** — Rep at a Location US-001 S2, S3, non-functional notes; uxdocs 01 T-01
**Depends on** — T-4.1.2
**Pattern to follow** — T-4.1.2 (upload queue)
**Ownership** — Impl: Human Tight-Loop | Test: Human-Led | Complexity: M | Confidence: L
  (inferred) — fault paths of the sync protocol; Test Safety Net High until a fault-injection harness exists.

**Provisional commit message**

```
feat(sync): report sync failures by cause

- "Sync failed" gives the rep nothing to act on, so each failure names its
  cause and the one action it needs
```

**Capability class** — Frontier workhorse · effort: Anthropic standard budget / OpenAI high / Google high
Failure-path behaviour on a protocol a human owns.

**Work package**

Increments:
1. Fault-injection harness for the sync client (drop after N confirmations, expired session, server unavailable).
2. Partial upload: confirmed items Sent, others unchanged, message "2 items not sent — poor connection. They'll send next Sync."; download attempted.
3. Expired sign-in: nothing uploads, "Sign in again to send 3 items" with Sign in; work stays.
4. No connection: "No connection — your work is saved on this tablet".
5. Progress "Sending 2 of 3".

Decision points:
- After a failed upload, does the download still run when signal allows (S2 says yes), and is that visible to the rep?
- Does "Sign in" resume the same sync automatically afterwards?

Delegable slivers:
- **Fault-injection harness** — Build a test harness that wraps the sync transport and can drop the connection after N confirmations, return an expired-session response, or refuse connection. Test-only code; do not change production sync.
- **Failure messages** — Map each classified sync failure to the exact sentences in US-001 S2, S3 and the no-connection edge case on T-01. Do not change classification or retry logic.

---

### T-4.3.1-S — Test scenarios for Needs Attention

**Owner** — Scenario Review
**Gates** — T-4.3.1
**Produces** — a scenario list: names plus one-line intent, no code

**Agent prompt**

```
Draft the test scenario list for the Unsent Items screen and Needs Attention
(task T-4.3.1). Read plan_docs/stories/rep-at-a-location-tablet.md US-017
S1–S7 (note S1's reason is superseded by S5–S7 and BR-NEW-006 in
plan_docs/uxdocs/04-user-stories-amendments.md) and
plan_docs/uxdocs/01-tablet-day.md T-01 (T1.1, T1.2). Output one line per
scenario as Should_Outcome_When_Condition, then "→" and a one-line intent.
Cover S2–S7, then derivable edges: several items in Needs Attention, a
rejected Order that is then edited, deleting a rejected Order. Mark undecided
cases as "Needs a decision". Write no test code; change no files.
```

---

### T-4.3.1 — Show unsent items with Needs Attention first

**Parent story**

> As a Field Salesperson, I want items head office's system rejected to be shown with the reason so that I can fix or discard them and nothing quietly disappears.
>
> Acceptance criteria:
> - A rejected item appears at the top under Needs Attention with its reason (S1, with reasons per S5–S7)
> - An Order in Needs Attention for "Duplicate submission" can be reviewed and marked ready again, uploading at next Sync (S2)
> - A Call in Needs Attention can be deleted after confirming; Calls are deletable only here (S3)
> - The Unsent count includes it and reminders say "1 needs attention" (S4)
> - An Order captured while Quinn's was assigned uploads and is accepted after reassignment; nothing appears in Needs Attention (S5)
> - An incomplete upload shows "Couldn't be sent - the upload was incomplete" with Open and Delete (S6)
> - A reason the rep can't act on shows "This couldn't be sent - contact the office", keeping the raw reason for support (S7)

**Slice** — The Unsent Items screen shows counts by state with Needs Attention first, each rejected item with a sentence the rep can act on, and lets the rep fix and re-mark or delete it.
**Spec source** — Rep at a Location US-017 S1–S7; uxdocs 01 T-01 (T1.1, T1.2); uxdocs 04 BR-NEW-006
**Depends on** — T-4.1.2
**Pattern to follow** — T-4.2.2 (tablet screen)
**Ownership** — Impl: Agent-Assisted | Test: Scenario Review | Complexity: M | Confidence: M
  (inferred) — depends on the server's rejection contract from T-4.1.2 (MI-52); settled design.

**Provisional commit message**

```
feat(tablet): show unsent work with needs-attention items first

- Nothing captured may quietly disappear, so a rejected item stays visible
  with a sentence the rep can act on
- Only technical faults reject; a rule change after capture never does
```

**Capability class** — Frontier workhorse · effort: Anthropic standard budget / OpenAI high / Google high
Depends on a protocol contract; review pause on the reason mapping.

**Agent prompt**

```
Role: You are building the Unsent Items screen with Needs Attention (T-01)
for the Field Sales Management System's tablet app.

Context:
- Slice: the Unsent Items screen shows counts by state with Needs Attention
  first, each rejected item with a sentence the rep can act on, and lets the
  rep fix and re-mark or delete it.
- Specs: plan_docs/stories/rep-at-a-location-tablet.md US-017 S1–S7 and its
  UX amendment; plan_docs/uxdocs/04-user-stories-amendments.md BR-NEW-006;
  plan_docs/uxdocs/01-tablet-day.md T-01 (T1.1 Needs Attention first and the
  only section with per-item buttons; T1.2 the reason is a sentence, not a
  code).
- Stack: {{STACK}}. Code locations: {{PLACEHOLDER}} — upload queue and
  server rejection contract from T-4.1.2.
- Pattern to follow: T-4.2.2's screen structure.

Acceptance criteria:
1. Counts by state: In Progress, Ready to Send, Needs Attention.
2. Needs Attention is listed first; each item shows its reason as a
   sentence.
3. An Order rejected for "Duplicate submission" can be opened, reviewed and
   marked ready again; it uploads at next Sync.
4. A Call in Needs Attention can be deleted after confirming; Calls cannot
   be deleted anywhere else.
5. The Unsent count includes Needs Attention items; reminder text reads
   "1 needs attention".
6. An incomplete upload shows "Couldn't be sent - the upload was incomplete"
   with Open and Delete.
7. A reason the rep can't act on shows "This couldn't be sent - contact the
   office"; the raw reason is kept for support.
8. An Order captured before its Location was reassigned uploads and is
   accepted; nothing appears in Needs Attention.

Constraints:
- Use the project's existing conventions and test framework.
- No business-rule rejections exist; if the server contract returns one,
  stop and report.
- No tests of framework internals or trivial members.
- Irreversible or schema-altering operations are out of scope.

Gates: stay read-only until I reply "Approved" to your plan; do not modify
existing files until I reply "Apply".
Checkpoint: after the mapping from server rejection codes to rep sentences is
written, stop and show it. Resume only on "Continue T-4.3.1".

Steps: 1. rejection-to-sentence mapping; 2. T-01 screen with counts and
sections; 3. fix/re-mark and delete actions; 4. tests from agreed scenarios.

Test expectations: implement the scenarios agreed in T-4.3.1-S; do not design
your own.

Definition of done: the Unsent Items screen shows counts by state with Needs
Attention first, each rejected item with a sentence the rep can act on, and
lets the rep fix and re-mark or delete it.

Self-verification (report each line pass or fail with evidence):
- [ ] Criteria 1–8 demonstrated
- [ ] Provisional commit message reconciled with the actual diff

Out of scope: sync reminders (E13), editing orders (T-5.2.1), correcting
calls (T-5.6.1).
```

**Checkpoint**

Produces before pausing — the mapping from each server rejection code to the sentence the rep reads.
Human reviews — Is every possible rejection a technical fault with an action the rep can take, and is there no business-rule rejection left?
Resume trigger — `Continue T-4.3.1`

---

### T-4.4.1-S — Test scenarios for hiding restricted products

**Owner** — Human-Led
**Gates** — T-4.4.1
**Produces** — a scenario list: names plus one-line intent, no code

Questions to answer while writing it:
- A rep holding some groups and not others: which products appear, per group (edge case)?
- Where must a restricted product be absent — Order entry search, Order Pad, Stock Check, suggested list, competitor-note picker, replacements — with no "unavailable" trace (S1; T7.5)?
- Permission removed after an In Progress line was added: the line reads "No longer available to you", can't be re-added, and the order still sends (S4). Which of those does each surface show?
- A product moves into a restricted group after it was on a suggested list: what does the next snapshot contain?
- An archived group (MI-45): hidden or shown?

---

### T-4.4.1 — Keep restricted products off tablets without permission

**Parent story**

> As a Field Salesperson without permission for a Restriction Group, I want its products absent from my tablet entirely so that high-value or legally restricted items are never exposed or ordered by mistake.
>
> Acceptance criteria:
> - Without the "Pharmacy-only medicines" permission, "Controlled Pain Relief 30s" does not appear in Order entry, Stock Check or a Competitor Note picker, and no "unavailable" entry is shown (S1)
> - The product is not downloaded to the tablet at Sync (S2)
> - With the permission it appears and behaves like any other product (S3)
> - An In Progress line added before removal shows "No longer available to you" after Sync, can't be re-added, and the Order sends under valid-when-captured (S4)

**Slice** — Products in a restriction group the rep lacks are never downloaded or shown anywhere on the tablet, and a line captured before a permission was removed still sends, marked "No longer available to you".
**Spec source** — Rep at a Location US-019 S1–S4; Coverage Management US-009 S2; Product Management US-004 S4; uxdocs 01 T-07 (T7.5)
**Depends on** — T-3.4.1, T-4.1.1, T-5.1.1
**Pattern to follow** — T-4.1.1 (snapshot builder)
**Ownership** — Impl: Human Tight-Loop | Test: Human-Led | Complexity: M | Confidence: M
  (inferred) — authorization: a leak of a legally restricted product to an unqualified rep is a compliance incident; the archived-group rule is open (MI-45).

**Provisional commit message**

```
feat(sync): keep restricted products off unpermitted tablets

- Legally restricted products must not sit on a device that merely hides
  them, so they are filtered when the snapshot is built, per rep
- A line captured before a permission was removed still sends, marked, as
  every other rule change after capture does
```

**Capability class** — Frontier workhorse · effort: Anthropic standard budget / OpenAI high / Google high
Authorization held by a human; narrow slivers delegated.

**Work package**

Increments:
1. Snapshot builder filters products by the rep's current permissions (T-3.4.1), including suggested lists, replacements and pickers built from the snapshot.
2. Server-side check that no snapshot payload contains a product from a group the rep lacks (a guard test on the builder output).
3. Tablet: a line on an In Progress order whose product is no longer in the snapshot reads "No longer available to you", cannot be re-added, and still uploads.
4. Apply the archived-group rule once decided (MI-45).

Decision points:
- Archived group: filter as restricted, or treat as unrestricted (MI-45)?
- What does the tablet show for a stored line whose product no longer exists in the snapshot at all — name and price from the line itself?
- Is there any head-office or manager path (e.g. a website view) that must apply the same filter for reps?

Delegable slivers:
- **Snapshot guard test** — Write a test that builds snapshots for a rep with and without each restriction group and asserts no product from a missing group appears anywhere in the payload. Test-only; do not change the builder.
- **Removed-permission line display** — On the tablet order line, show "No longer available to you" when the product is absent from the snapshot and hide the add/quantity controls for it, per US-019 S4. Do not change upload logic.

---

### T-4.5.1 — Show Home as the record of the day

**Parent story**

> As a Field Salesperson, I want Home to show today's visits, what I've done, Overdue and Due soon items so that I know where I'm going, what I've captured and what I might miss.
>
> Acceptance criteria:
> - A Call and a Ready to Send Order saved at Murphy's at 09:40 show Murphy's Pharmacy marked Done with its Order and state beneath; a phone Order from Nolan's at 11:15 appears under Today as unplanned with its time and state (S2)
> - With no Scheduled Visits, Home shows "No visits scheduled today" and still shows search and any unplanned activity (S5)
> - The Unsent count sits in the exception strip (T2.2)

**Slice** — Home lists what the rep has done today — calls and orders at planned or unplanned shops with each order's state — shows the unsent count, offers search, and says "No visits scheduled today" when nothing is planned.
**Spec source** — Rep at a Location US-003 S2, S5; uxdocs 01 T-02 (T2.2, T2.3)
**Depends on** — T-4.1.1, T-4.2.2, T-5.3.1
**Pattern to follow** — T-4.2.2 (tablet screen)
**Ownership** — Impl: Agent-Assisted | Test: Agent-Autonomous | Complexity: M | Confidence: M
  (inferred) — Home is a shared surface that E8, E13, E14, E15 and E25 extend, so the build pauses on its structure; the tests are mechanical.

**Provisional commit message**

```
feat(tablet): show the day's record on Home

- Home answers "what have I done and what's still unsent", which reps need
  from day one even before visit planning exists
- Built as an extensible strip and sections so later visit, not-supplied
  and customer-request features slot in without redesign
```

**Capability class** — Frontier workhorse · effort: Anthropic standard budget / OpenAI high / Google high
A shared screen other epics extend.

**Agent prompt**

```
Role: You are building the tablet Home screen (T-02) as the record of the day
for the Field Sales Management System.

Context:
- Slice: Home lists what the rep has done today — calls and orders at
  planned or unplanned shops with each order's state — shows the unsent
  count, offers search, and says "No visits scheduled today" when nothing is
  planned.
- Specs: plan_docs/stories/rep-at-a-location-tablet.md US-003 S2, S5 and the
  non-functional note; plan_docs/uxdocs/01-tablet-day.md T-02 (T2.2 the
  exception counts are one strip; T2.3 done visits stay in place).
- Stack: {{STACK}}. Code locations: {{PLACEHOLDER}} — snapshot (T-4.1.1),
  Location screen (T-4.2.2), calls (T-5.3.1), orders (T-5.1.1).
- Pattern to follow: T-4.2.2's screen structure.

Acceptance criteria:
1. After saving a Call and a Ready to Send Order at Murphy's Pharmacy at
   09:40, Home shows Murphy's Pharmacy marked Done with its Order and state
   beneath.
2. A phone Order from Nolan's Pharmacy (not scheduled) at 11:15 appears
   under Today as unplanned, at 11:15, with its Order state.
3. With no Scheduled Visits, Home shows "No visits scheduled today" and
   still shows search and unplanned activity.
4. The Unsent count appears in a single exception strip, which later
   features add counters to.
5. Rows are large one-handed touch targets with the Location name dominant.

Constraints:
- Use the project's existing conventions and test framework.
- Build the exception strip and the Today list so later counters (not
  supplied, cycle decisions, conflicts) and sections (Overdue, Due soon,
  Customer requests) can be added without restructuring.
- No tests of framework internals or trivial members.
- Irreversible or schema-altering operations are out of scope.

Gates: stay read-only until I reply "Approved" to your plan; do not modify
existing files until I reply "Apply".
Checkpoint: after Home renders the S2 day against T-02 and the extension
points for strip counters and sections are in place, stop and show them.
Resume only on "Continue T-4.5.1".

Steps: 1. today's record from local calls and orders; 2. T-02 layout;
3. exception strip with the Unsent count; 4. empty state; 5. tests.

Test expectations: select scenarios yourself from the criteria.

Definition of done: Home lists what the rep has done today — calls and
orders at planned or unplanned shops with each order's state — shows the
unsent count, offers search, and says "No visits scheduled today" when
nothing is planned.

Self-verification (report each line pass or fail with evidence):
- [ ] Criteria 1–5 demonstrated
- [ ] A dummy extra counter and section can be added without changing
      existing code paths
- [ ] Provisional commit message reconciled with the actual diff

Out of scope: scheduled visits, Overdue, Due soon, deadlines and website
counts (E13), cover and left visits (E14, E15), not supplied (T-8.5.1),
customer requests (E25), visit sequence (E13).
```

**Checkpoint**

Produces before pausing — Home rendered for the S2 day, and the extension points for strip counters and sections.
Human reviews — Can the visit, not-supplied and customer-request features slot into this Home without a redesign?
Resume trigger — `Continue T-4.5.1`

---

## Epic self-verification

| Check | Result | Evidence |
|---|---|---|
| Every story in this epic appears and none is dropped | Pass | A1-001 (T-4.1.1–T-4.1.3), A1-004 (T-4.2.1, T-4.2.2), A1-017 (T-4.3.1), A1-019 (T-4.4.1), A1-003 part (T-4.5.1; other scenarios in E8, E13–E15) |
| Every task satisfies the three slice criteria | Pass | 8 of 8 |
| Every task carries a tier with a rationale citing dimensions | Pass | 8 of 8 |
| Unscoreable dimensions scored High, marked (inferred), reported | Pass | T-4.1.1–T-4.1.3 (MI-01, MI-02, MI-52), T-4.4.1 (MI-45) |
| Tasks modifying existing behaviour order characterisation first | Pass | None in this epic modify earlier behaviour |
| No Agent-Autonomous body where Blast, Taste or Test Safety Net is High | Pass | No Agent-Autonomous task in this epic |
| No agent prompt designs tests where Oracle Ambiguity or Edge-Case Discovery is High | Pass | T-4.1.1, T-4.1.2, T-4.1.3, T-4.4.1 are Human-Led |
| Every Agent-Assisted body has a checkpoint with an exact resume trigger | Pass | T-4.2.1, T-4.2.2, T-4.3.1, T-4.5.1 |
| Every Human Tight-Loop body has increments, decision points and slivers | Pass | T-4.1.1, T-4.1.2, T-4.1.3, T-4.4.1 |
| Every scenario task precedes the task it gates | Pass | 7 `-S` sections |
| Every agent prompt is self-contained | Pass | Each names specs, criteria, constraints, gates, out of scope |
| No concrete model name in any task section | Pass | Classes only |
| Every placeholder and inferred marker is reported | Pass | `{{STACK}}`, `{{PLACEHOLDER}}`, MI-01, 02, 45, 52 |
