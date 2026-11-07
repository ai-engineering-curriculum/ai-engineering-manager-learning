# Exercise 03: Capacity Paired with Provider Release Cadence

**Estimated effort:** 2 hours · **Output:** a one-page capacity-and-cadence table for one quarter, a current-state vendor-cadence read, a re-plan ritual agenda, and one worked delta memo

## Objective

Produce the capacity-plus-cadence artefact from chapter 03 for **one specific team and one specific quarter** (current or upcoming). The output pairs the team's capacity blocks week-by-week with the provider events you can actually see coming from the vendors your team depends on, defines the thirty-minute re-plan ritual, and ends with a worked delta memo based on one plausible vendor event in the quarter.

## Prerequisites

- Chapter 03 (`Capacity planning paired with provider release cadence`).
- Chapter 01 at least skimmed — the capability-posture language is reused here.
- A team context with real or realistic provider dependencies. Three usable patterns:
  - Your current team's actual provider set (strongest). If you can share the vendor names, do; if not, anonymise consistently as `[Provider A]`, `[Model X]`, etc.
  - A prior team's provider set (fine).
  - A composite team whose provider set you define in the charter, grounded in public vendor documentation you cite.
- Access — read-only — to the current deprecation and release pages of the providers you depend on. The exercise is substantially worse if you make up the vendor events rather than read them from primary docs.

## Problem statement

Produce the following five artefacts, all for the same team and quarter.

### Artefact 1 — Team and provider inventory (20 minutes)

One paragraph: team name, size, quarter being planned. Then a short table listing every provider the team depends on in production or in active prototyping. Columns:

- **Provider**
- **Model / API in use** (the specific family name, not "GPT-4-class")
- **Posture** (production / prototype / evaluated-but-not-adopted)
- **Known deprecation date** (from the vendor doc; `none announced` if nothing is on the page)
- **Known or expected release window** (what the vendor has publicly announced is coming, with source URL — or `none announced` if nothing is on the page)

Cite the source URL for every non-`none announced` row. If a row depends on a non-public roadmap conversation with the vendor, mark it `non-public` and do not include specifics that would violate confidentiality.

### Artefact 2 — Capacity-and-cadence table (40 minutes)

The chapter 03 one-page table, filled in for the quarter. Columns:

- **Week**
- **Capacity block** (e.g. "kickoff + re-forecast", "delivery", "eval sprint on `[Model X]`", "migration sprint for `[Model Y]` deprecation", "mid-quarter cost review", "end-of-quarter calibration")
- **Expected provider event** (if any — links to the vendor doc)
- **Owner**

Constraints:

- Thirteen weeks (or whatever your quarter's length is). Every week has a line, even if the capacity block is "steady-state delivery."
- Every provider event is cited from a real vendor doc (deprecation page, release blog, framework update). If you cannot find a specific event, say so — "no provider events expected this quarter based on current vendor docs" — and move on. Do not fabricate.
- At least one week includes the re-plan ritual (either triggered by an announced event or as a standing cadence event).
- At least one week includes a cost / finops review that overlaps with the vendor pricing-bulletin cadence.
- At least one week is reserved for reading the current frontier-lab release-safety framework update (RSP / Preparedness / FSF), with the governance peer as the co-owner.

### Artefact 3 — Re-plan ritual agenda (20 minutes)

Write the thirty-minute agenda for one re-plan ritual in this quarter. Structure:

- **Participants** — named by role: chair (you), architect or principal MLE, product partner if relevant, governance peer if the triggering event is a framework update, the eval-owner if the triggering event is a model release.
- **Inputs** — the vendor doc (linked), the current committed-rows list (from exercise 01 if you did it; from chapter 01's structure if not), the eval-suite catalogue.
- **Agenda, five steps** — reading the announcement; checking announced-upgrade-dependency rows for band change; checking current-model-floor rows for migration; checking speculative rows for promotion; producing the delta memo.
- **Outputs** — the delta memo, any capacity adjustments, any band-column updates on the committed-rows table.

### Artefact 4 — Worked delta memo (20 minutes)

Pick one plausible vendor event in this quarter (either a real announced release or deprecation you discovered in Artefact 1, or a worked hypothetical you clearly label as such). Produce the one-page delta memo from chapter 03. Sections:

- **Event** — one sentence on what the vendor announced, with source link.
- **Rows affected** — the three to five committed rows whose bands or posture changed, with the new numbers (not the old numbers).
- **Opportunity captured** — any speculative rows promoted to current-model-floor delivery candidates.
- **Capacity delta** — how the capacity-and-cadence table in Artefact 2 changes this quarter.
- **Ask** — what, if anything, the memo asks leadership to decide (usually: approve a migration line that displaces a committed row, or approve a scope change).

Keep the memo to one page in markdown. The language is for a non-engineering leadership reader; the technical detail lives in the links.

### Artefact 5 — "What this role does NOT own" line (10 minutes)

Write a three-to-five-item list — one sentence per item — of what reading the provider cadence does **not** authorise you to own this quarter. Draw from chapter 03's "what this role does not own" section. Frame each item as what the other role owns and what you contribute if asked.

This artefact exists to prevent scope creep in practice. Managers who do not write it end up accidentally authoring AI governance policy or negotiating with vendors; both of those belong elsewhere.

## Requirements

- Every artefact is for the same team and quarter as named in Artefact 1.
- Every provider event in Artefacts 2 and 4 is cited from a real vendor doc (primary source). If no event is cited, the row says `none announced` with the date the vendor doc was checked.
- The capacity-and-cadence table has thirteen (or quarter-length) rows, no missing weeks.
- The re-plan ritual agenda fits on half a page.
- The delta memo fits on one page and includes an ask section — a memo with no ask is a status update, not a planning artefact.
- The "what this role does NOT own" list is three to five items and has no overlap with items you actually own this quarter.
- Where you are forced to use a hypothetical event in Artefact 4, say so at the top of the memo. Do not present a hypothetical as a real announcement.

## Starter guidance

- Begin with Artefact 1 (the provider inventory). You cannot plan around events you have not enumerated. Open each vendor's deprecation page and release blog and fill in rows as you read.
- For the capacity table, do not try to fill in a provider event in every week. Most weeks are steady-state delivery. The point of the table is naming the events that *do* exist and leaving the rest honest.
- For the re-plan ritual agenda, treat it as a routine thirty-minute meeting, not a crisis response. A ritual that only runs when there is a crisis gets skipped during the quarter it is most needed.
- For the delta memo, write for a leadership reader. The tone is "here is what changed in the plan and why; here is the one decision I need from you." Do not include links in the body text that require specialist context to interpret.
- For the "what you do not own" list, read chapter 03's "what this role does not own" section first and borrow the items that apply. The three most common errors to prevent: authoring internal policy that mirrors a vendor safety framework, taking the lead on renegotiating enterprise contracts after a pricing reset, and representing your organisation back to the vendor on their framework. Each of these has a nameable owner in your organisation — name them.

## Acceptance criteria

- Provider inventory lists every production and prototyping model, with cited deprecation and release dates (or `none announced` with doc-check date).
- Capacity-and-cadence table has every week of the quarter filled, with at least one re-plan ritual event, one cost-review event, and one framework-update read.
- Re-plan ritual agenda is thirty minutes, with named participants and a five-step structure that produces a delta memo.
- Worked delta memo is one page, with event + rows affected + capacity delta + leadership ask.
- "What this role does NOT own" list is three to five items naming the actual other-role owner.
- A peer AI Engineering Manager can read your capacity-and-cadence table and tell you which week is the highest-risk week for the quarter, and the capacity table's answer matches.

## Stretch goals

- **Provider-cadence reading habit.** Produce a short one-page checklist of the vendor pages your team should read each week (deprecations, release notes, pricing bulletins, framework updates). Assign the reading duty to a rotating seat on the team, and write the five-minute weekly ritual that produces a "no new events" or "here are N new events" note. Publish the checklist to your team handbook.
- **Pricing-reset simulation.** Pick one of your team's cost-near-ceiling features (from exercise 02's cost ceiling, or the GA cost budget) and work through what a 20% pricing increase from the primary provider does to the monthly ceiling, the abort rule, and the mitigation. Write the re-plan memo that would land on leadership the day after such an announcement.
- **Multi-provider fan-out.** Expand the provider inventory to include every provider the team has evaluated or considered — not only those currently in production. The exercise reveals how many implicit bets the team has been making on providers whose cadence they are not actively watching.
