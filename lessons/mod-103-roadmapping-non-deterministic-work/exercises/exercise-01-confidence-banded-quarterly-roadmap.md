# Exercise 01: Confidence-Banded Quarterly Roadmap

**Estimated effort:** 3 hours · **Output:** a one-page quarterly roadmap for one specific team with banded dates, capability postures, capacity lines, and pre-booked re-plan events

## Objective

Produce the quarterly roadmap artefact from chapter 01 for **one specific team** — real or carefully chosen. The output is the committed-rows table with P10 / P50 / P90 bands, named band bases, capability postures, the capacity-lines panel, the capability-bets page for the annual view, and a pre-booked re-plan calendar. By the end of the exercise, a reader should be able to walk through your roadmap with you and ask "what is this row betting on" for every row, and get an answer that cites a specific model, dataset, or dependency.

## Prerequisites

- Chapter 01 (`Confidence-banded quarterly and annual roadmaps`).
- Skim of chapters 03 and 05 — the capability-posture concept depends on chapter 03's vendor-cadence read; the capacity lines panel depends on chapter 05's bucket definitions.
- A team context. Three usable patterns, strongest first:
  - A team you manage or will manage inside the next 90 days.
  - A team at a prior employer you can describe honestly, with permission to abstract where appropriate.
  - A composite team you build from public references, with a short written charter you author yourself. Cite anything you draw from.
- An honest read on the team's prior four quarters' delivery actuals (velocity, incident-hours, deprecation events, model-release events). If you have none, acknowledge it and borrow a reference class from a comparable team or published source.

## Problem statement

Produce the following six artefacts, all for the same team.

### Artefact 1 — Team charter (10 minutes)

One paragraph. Team name (or anonymised equivalent), size, scope (what AI products or agents it ships), and the one sentence that answers "why does this team exist." If anonymised, note it at the top. This is the shared ground for everything else.

### Artefact 2 — Quarterly committed-rows table (60 minutes)

The roadmap's main artefact. A markdown table with one row per committed deliverable. Columns:

- **Row** — short name of the deliverable.
- **Target (P50)** — the single most-likely date.
- **P10** — "if everything goes well" date.
- **P90** — "if the known risks materialise" date.
- **Band basis** — one sentence naming the two to three concrete drivers of the width between P10 and P90.
- **Capability posture** — one of `current-model floor`, `announced-upgrade dependency on [specific model/release + announced window]`, or `speculative` (with a note that speculative rows belong in discovery, not here).
- **Owner** — the named human on the team.

Constraints:

- Between four and eight committed rows. Fewer is a sign the team is under-planned; more is a sign the delivery capacity is overstuffed.
- No speculative rows in this table. If a speculative row lands here, it goes to Artefact 4 (capacity lines) in the discovery block instead.
- Every row with an `announced-upgrade dependency` names the **specific release** by product name and the vendor's announced window — not "a new model coming out."
- Every band basis names at least one risk that would move the P90 — never just "eval variance" alone.

### Artefact 3 — Capability-bets page (20 minutes)

The annual-roadmap one-pager from chapter 01. Two short lists:

- **Counting on** — three to five capability shifts the annual plan is assuming happen (e.g. a specific model's context window expanding; a specific provider shipping an announced tool-use capability). Each is one sentence plus the vendor and announced window.
- **Counting on not happening** — three to five regressions or shifts the annual plan is assuming do **not** happen (e.g. a model this team depends on being deprecated with less than ninety days notice; a sudden pricing reset that pushes a GA feature past its cost ceiling).

### Artefact 4 — Capacity lines panel (20 minutes)

A short table — one line per bucket — with the percentage this quarter's plan reserves and the one-sentence defence. The buckets are chapter 05's: delivery, discovery, incident / regression, and (if applicable) research-to-production handoff ramp.

Constraints:

- Percentages sum to 100.
- `delivery` is strictly less than 100%. A plan that says "100% delivery" is incorrect by construction.
- `incident` is strictly greater than 0%. Even a pre-first-GA team spends some time on regressions, eval maintenance, and vendor-migration work.
- Each bucket's defence cites a specific artefact or prior-quarter actual (not "best practice"). Discovery cites candidate-row history or a specific exploration. Incident cites burn-rate, prior-quarter incident-hours, or an upcoming deprecation.

### Artefact 5 — Re-plan calendar (20 minutes)

A short list, one line per pre-booked event. For each: the date (or trigger), what it is, and what it produces. Include:

- At least one event per major provider release expected in the quarter, named with the provider and the announced window.
- One standing cadence event (e.g. the end-of-month re-forecast).
- At least one trigger-based event (e.g. "on any P50 overrun, the band is re-pulled within three working days").
- The end-of-quarter calibration review that will produce your team's own reference class for next quarter.

### Artefact 6 — Band calibration note (30 minutes)

A one-paragraph critique of your own bands, written honestly. Identify:

- Which row has the **widest** uncertainty, and why you did not widen it further.
- Which row has the **narrowest** uncertainty, and the evidence you have that justifies it (reference class, prior-quarter actuals, dependency map).
- Which row is **most likely to bust its P90**, and the leading indicator you will watch.
- One row (if any) where the band is honest but leadership is unlikely to accept it — and how you plan to defend it in the roadmap review.

## Requirements

- Every artefact is produced for the same team context named in Artefact 1. No switching teams mid-exercise.
- Every row in Artefact 2 has all seven columns filled. Missing columns do not pass the exercise.
- Every `announced-upgrade dependency` row names the specific release. If you do not know a specific one, demote to `current-model floor` or move to discovery.
- Artefact 3's lists are specific, not hedged. "Counting on a new model shipping" does not count; "counting on `[vendor] [product]` tool-use-latency not regressing beyond `X` ms on `[your eval]`" does.
- Artefact 4's percentages sum to 100 and each defence cites a real artefact or number.
- Artefact 5 includes at least one provider-release event. If no provider releases are expected in the quarter, say so explicitly in the re-plan calendar and cite the vendor doc you checked.
- Where you use real numbers, do not fabricate. Use `[placeholder: measure before quoting]` where you genuinely do not have the value. Use `<!-- needs-research: ... -->` comments where a factual claim would require external evidence you do not have.

## Starter guidance

- Begin by listing every row the team is already carrying (not from a blank page). The exercise is to make the uncertainty honest, not to invent a plan from scratch.
- The P10/P90 width is almost always wider than the team's gut estimate. If your bands look narrow, you are probably not naming all three drivers of the band basis — add a line about vendor dependency, add a line about eval tail risk, add a line about scope discovery.
- For capability-posture reads, the vendor's own deprecation or release page is the source of truth. Link it in the band basis if the row depends on it; the link is part of the artefact.
- For the capacity-lines defence, prior-quarter actuals beat heuristics. If you have the team's incident-hours for the prior quarter, use the number — do not round up for "best practice."
- If leadership at your organisation has never seen a banded roadmap before, Artefact 6 (band calibration note) is where you think through the first conversation. The anti-pattern is publishing P10 and letting leadership anchor on it; the defence is publishing P50 as the prominent date and talking about what moves the band.

## Acceptance criteria

- One-page committed-rows table with every row carrying target, P10, P90, band basis, capability posture, and owner.
- One capability-bets page with "counting on" and "counting on not happening," each with at least three concrete entries.
- One capacity-lines panel summing to 100%, with per-bucket defences that cite real artefacts or numbers.
- One re-plan calendar with at least one provider-release event, one standing event, and one trigger-based event.
- One band calibration note that honestly critiques the roadmap's own uncertainty.
- A peer AI Engineering Manager can read the roadmap and, for any row, ask "what is this betting on" and get a specific answer from the band basis or capability posture.

## Stretch goals

- **Second-quarter projection.** Produce a shortened Artefact 2 for the *next* quarter — themes with confidence instead of dated rows. Writing both back-to-back forces the discipline that only this quarter's rows are dated.
- **Stakeholder-view slide.** Build a one-slide version of the roadmap for a non-engineering leadership audience: five rows max, P50 dates only, with the capability-posture column surfaced as a colour (green / amber / red) and the capacity-lines panel on the right. Keep the band basis in a speaker-notes field. Compare the two views — the engineering view and the stakeholder view — and confirm they do not disagree.
- **Retrospective calibration.** If your team has run four or more quarters already, pull the actuals from the last two quarters' commitments into a small table: for each prior commitment, the published P50, the published P90, and the actual landing date. Compute the hit rate against P90. This becomes your team's reference class for the P90 column in the next quarter's planning.
