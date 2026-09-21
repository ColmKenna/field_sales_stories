# 05 — Manager website (M-01 … M-15)

**Device assumptions:** laptop, online, used by a Sales Manager planning and reviewing a team.
**Consequence:** exception-first overview screens may be dense, but every summary must drill into the visits, Locations or assignments behind it.

---

## M-01 · Visit Planning overview

**Job:** find which rep needs planning attention, with region as a secondary way to investigate where coverage is breaking down.

```
+------------------------------------------------------------------------------------------------+
| Visit planning                         [ By rep ] [ By region ]          Current period  v      |
+------------------------------------------------------------------------------------------------+
| REP / COVERAGE             | DUE     | OVERDUE | MISSED | UNSCHED <14D | OTHER EXCEPTIONS     |
+------------------------------------------------------------------------------------------------+
| Colm                       | 18 / 24 |       3 |      1 |            4 | 1 over day           |
| Wicklow · 42 Locations     |         |         |        |              | 2 handover pending  > |
+------------------------------------------------------------------------------------------------+
| Aoife                      | 21 / 23 |       0 |      0 |            2 | 1 conflict           |
| Rathdrum, Avoca · 31 Locs  |         |         |        |              | Covering another team|
+------------------------------------------------------------------------------------------------+
| Ciara                      | 20 / 20 |       0 |      0 |            0 | 0 absence decisions  |
| Wexford North · 28 Locs    |         |         |        |              | 0 conflicts         > |
+------------------------------------------------------------------------------------------------+
```

**By region** retains the source-story hierarchy: Region → Town → Visit Dues, with responsible rep on each visit and Unassigned surfaced beside the affected Town.

> **DECISION M1.1 — By rep is the first-use default; the last-selected view is remembered. Settled.**
> A manager normally plans for a rep, so the primary question is "who needs attention?" rather than "which geography?". `By region` remains a peer view for geographic failures and Unassigned Locations. After first use, returning to the manager's last-selected view preserves their working context.

> **DECISION M1.2 — each rep row carries a compact coverage reminder. Settled direction.**
> The manager probably knows each patch, so this is orientation rather than a full assignment inventory. The summary uses the broadest meaningful effective area first, then a carved-out or overflow count, and always ends with the effective Location count: `Wicklow County · 3 towns carved out · 119 Locations` or, where no coherent territory exists, `Rathdrum, Avoca + 4 areas · 31 Locations`. Detailed assignments remain in M-06/M-07.

**Source-confirmed behavior:**
- Every rep remains visible when all exception counts are zero.
- Cross-team cover is informational, with no Approve/Decline action.
- Handover Pending links to Coverage Management.
- By region surfaces Unassigned Locations and links to assignment.
- Opening an Overdue visit shows its window, Due Reason, last Call and Scheduled Day, and allows Mark Missed with an optional reason.

---

## Open questions from this file

1. ~~**M-01 coverage summary** — how are representative area labels chosen?~~ **Resolved — broadest meaningful area first, then carve-out/overflow count, then effective Location count.**

---

## M-02 · Absence entry & decisions

**Job:** block a rep's unavailable time immediately, then decide what happens to affected visits without making those decisions a prerequisite for recording the absence.

**Stage 1 — record the absence:**

```
+---------------------------------------------------------------------------------------+
| < Visit planning                         Record planned absence                       |
+---------------------------------------------------------------------------------------+
| Rep                 [ Colm                                             v ]            |
| From                [ 5 Oct 2026 ]     To                 [ 9 Oct 2026 ]             |
| Full / partial day  [ Full days                                         v ]            |
| Reason              [ Annual leave                                      ]            |
|                                                                                       |
| The calendar will be blocked immediately when this absence is saved.                  |
|                                                   [ Cancel ]   [ Save absence ]       |
+---------------------------------------------------------------------------------------+
```

**Stage 2 — affected visits after save:**

```
+---------------------------------------------------------------------------------------+
| Absence saved · Colm · 5-9 Oct                         14 visits need a decision       |
+---------------------------------------------------------------------------------------+
| The days are blocked now. Undecided visits keep their current window and show         |
| "Absence - decision pending" to Colm.                                                  |
|                                                                                       |
|                                  [ Return to overview ]   [ Decide affected visits ]  |
+---------------------------------------------------------------------------------------+
```

`Decide affected visits` opens the decision list required by Visit Planning US-007: deadline-marked rows, Extend / Keep / Cover per visit, Apply to all with exceptions, and covering-rep capacity context.

> **DECISION M2.1 — absence save and visit decisions are separate commit points. Settled.**
> Saving the absence blocks the rep's calendar immediately and returns already scheduled visits on those days to Unscheduled as specified by US-006. The manager may continue directly to the affected-visit decisions or return to the overview. Leaving does not roll back the absence: affected visits remain unchanged and visibly `Absence — decision pending` until decided.

> **DECISION M2.2 — no affected visits skips the decision stage. Source-confirmed.**
> The saved result says `No visits affected` and returns to the absence/overview context; it does not offer a meaningless decision step.

> **DECISION M2.3 — bulk Cover uses a select-all review with exclusions. Settled.**
> Choosing `Apply to all: Cover` opens the affected visits with all eligible rows selected. The manager chooses one covering rep as the batch default, can clear all or unselect individual visits, and may handle excluded rows separately with Extend, Keep or another covering rep. Before apply, the review shows the selected count and the covering rep's capacity warning for the affected period. Cross-team cover remains informational to the other manager, not an approval.

> **DECISION M2.4 — overlapping entries offer `Extend existing absence`; adjacent entries stay separate. Settled.**
> A genuine date/time overlap shows the existing absence and previews the combined range before offering `Extend existing absence`. The wording replaces the ambiguous `Merge`. Two absences that merely touch or sit next to each other are saved as separate entries without an extension prompt, even when their reasons look similar; the boundary may be intentional. Extending keeps the existing absence as the record being edited, with its reason still editable before save.

**Open:**
- *(none for M-02)*

---

## M-03 · Campaign creation

**Job:** create one coherent campaign across many Locations without hiding who will receive the resulting visits.

```
Filter                    Review                    Details                   Confirm
  (1)----------------------(2)------------------------(3)------------------------(4)

Customer / Profile /      All matches selected      Name and Due Window       "Create 38 visits
Region / optional Brand   Exceptions removable      Reason Type + text         across 6 reps?
                                                     Duration                   10 to specialists"
                                                     Outcome List
```

> **DECISION M3.1 — Campaign creation is a four-step wizard. Settled.**
> `Filter → Review → Details → Confirm` reflects real dependencies: the candidate set must exist before it can be reviewed; campaign details apply to the reviewed set; routing counts can only be confirmed after both are known. Back preserves every entry and selection so correction never means starting over.

> **DECISION M3.2 — each step states its result before advancing. Source-aligned.**
> Filter shows the match count; Review shows selected/excluded/unassigned counts; Details validates that at least one Outcome completes the visit; Confirm names total visits, reps and specialist routing before anything is created. A zero-match filter cannot advance.

> **DECISION M3.3 — choosing a campaign rep resolves unassigned ownership, but never replaces an existing Primary Rep. Settled.**
> Review allows the manager to choose the rep who will receive each selected campaign visit. If the Location has no Primary Rep, that choice also creates a permanent direct Primary Rep assignment as part of campaign confirmation, records it in Assignment History, and supplies future recurring visits, tablet access and attribution. If the Location already has a Primary Rep, choosing another rep affects this campaign visit only; ownership remains unchanged. Confirm separates these consequences: `38 campaign visits across 6 reps · 3 Locations will also receive a Primary Rep` and lists those three before creation.

> **DECISION M3.4 — unassigned selections support a bulk Primary Rep default with row overrides. Settled.**
> Review offers one Primary Rep choice that applies to every selected unassigned Location. The manager can override any row before advancing. The summary always reflects the effective row values, not merely the bulk default, and Confirm lists every permanent assignment separately from campaign-only routing.

**Open:**
- *(none for M-03)*
