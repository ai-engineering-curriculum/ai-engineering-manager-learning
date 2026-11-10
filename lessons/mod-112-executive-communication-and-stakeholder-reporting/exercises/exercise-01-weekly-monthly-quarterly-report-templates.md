# Exercise 01: Weekly, Monthly, and Quarterly Report Templates

**Estimated effort:** 3 hours · **Output:** three complete reporting artefacts (one weekly, one monthly, one quarterly) plus the four single-source-of-truth pages they read from.

## Objective

Author the full three-cadence reporting surface for a realistic AI engineering team — a weekly roll-up, a monthly deep-dive, and a quarterly exec-ready summary — reading from a common set of single-source-of-truth handbook pages. By the end of the exercise, a peer manager who has not seen your team before should be able to read the three artefacts in order and build an accurate situational picture at increasing compression ratios.

The three artefacts must be internally consistent: the weekly's misses appear in the monthly's miss ledger; the monthly's trajectory appears in the quarterly's narrative; the quarterly's confidence bands trace back to the monthly's numbers. Chapter 02 defines the templates; this exercise fills them.

## Prerequisites

- Chapter 01 (`The manager's altitude for executive communication and stakeholder reporting`) and chapter 02 (`Weekly roll-ups, monthly deep-dives, and quarterly exec summaries`).
- Mod-104 chapter 06 (offline and online eval cadence) — the eval-trajectory substrate.
- Mod-107 chapter 05 (blameless postmortems) — the incident substrate.
- Mod-110 chapter 02 (team cost model) and chapter 06 (FinOps lever set) — the budget substrate.
- Optional: a real team context (your current team, or a public engineering blog you can ground the composite against). The templates work on a composite but are stronger when the numbers trace to real workloads.

## Problem statement

Produce a complete three-cadence reporting surface for one AI engineering team over a quarter. The team can be your current team or a plausible composite (e.g., a mid-size SaaS vendor's internal-support-reply agent team of 6 engineers, two production features, one open fine-tune initiative). The deliverable has three parts and a consistency cross-check.

### Part A — Single source of truth (45 minutes)

Produce the four handbook pages chapter 02 names, dated as of the quarter-end you will report on. Each page is a markdown file in your team's handbook format (or an equivalent substrate).

1. **Eval-score trajectory page.** The headline eval scores for every production feature over the last 12 weeks, with the top three slices broken out. Current release-gate thresholds listed. Current drift notes (generation drift, judge drift, retrieval drift, tool-use drift per mod-104). Three unresolved fixture candidates named.
2. **Release-and-incident ledger page.** Every release in the quarter (minimum three), with a one-line outcome; every P0–P3 incident (minimum one) with a postmortem link (stub is acceptable for the exercise, as long as the fields the weekly and monthly compose from are populated).
3. **Budget-and-usage page.** The six AI-engineering cost buckets (mod-110 chapter 02), month-over-month over the quarter; the current FinOps-lever reading across all five levers (mod-110 chapter 06); one cost-per-feature attribution number.
4. **Model-portfolio and vendor page.** The current model portfolio; the active vendor contracts with renewal dates; the open deprecation notices; the quarter's model-watch note highlights (mod-109 chapter 03 ritual).

### Part B — The three cadence artefacts (90 minutes)

1. **The weekly roll-up (20 minutes).** The final week of the quarter. ~500 words, the five sections chapter 02 names (headline, release-gate posture, eval trajectory, incidents and misses, next week's watch-list). Addressed to the manager's director, the product counterpart, the infra peer, and the governance peer.
2. **The monthly deep-dive (30 minutes).** The final month of the quarter. ~2 pages, the six sections chapter 02 names. The eval-trajectory section includes a sparkline or small chart (markdown table, ASCII, or embedded image reference is fine). The incident read and the budget read are both populated.
3. **The quarterly exec-ready summary (40 minutes).** The quarter just ended. ~1 page or 3 slides, the five sections chapter 02 names. The register is translated to executive altitude — not eval jargon. Every number carries a noise band, trajectory, or confidence band.

### Part C — Consistency cross-check (15 minutes)

Write a one-page reviewer note — as if you were a peer manager auditing the three artefacts — that answers:

- Does every miss in the monthly appear in the weekly that first surfaced it? If not, which?
- Does the quarterly's narrative match the trajectory the monthly's numbers imply? Specifically, if the quarterly says "quality improved modestly over the quarter," does the monthly's month-over-month eval trajectory actually show modest improvement?
- Does the quarterly's confidence-band projection for next quarter trace to specific assumptions readable in the monthly's trend read?
- Is the register translated appropriately — weekly in partner-register (eval delta, drift, release-gate floor), quarterly in executive-register (feature quality, slice recovery, resilient to vendor change)?
- Is the "no-miss" discipline followed — does the weekly say "none" explicitly when a section has no items rather than omitting the section?

## Requirements

- All four single-source-of-truth pages are produced, dated, and internally consistent.
- The weekly has exactly five sections in the order chapter 02 names them; sections with no content say "none" rather than being omitted.
- The monthly has exactly six sections; the eval-trajectory section shows at least four data points over time; the miss ledger is populated or explicitly empty.
- The quarterly has exactly five sections; translated to executive register; every number carries a noise band or confidence read; next-quarter bets have three confidence bands.
- Every claim in the monthly links to a handbook page or a release-ledger entry; the weekly's links follow suit.
- The cross-check reviewer note is written and names at least one honest inconsistency or improvement from the first draft.

## Starter guidance

- Produce the single-source-of-truth pages first. The weekly reads off them; the monthly reads off them; the quarterly reads off them. Writing the cadence artefacts first will produce numbers that don't quite trace back, and you will spend the Part C cross-check reconciling fictions.
- Use real numbers (if the team is yours) or plausible numbers (if composite). An eval trajectory that moves 0.3 points week-over-week inside a ±1-point noise band is more realistic than one that moves 5 points every week. Realistic numbers teach the compression discipline; implausible numbers teach how to inflate.
- At least one release should ship with a miss; at least one incident should hit the quarter; at least one vendor-regression should appear on the model-portfolio page. The three cadences are specifically designed to narrate these honestly; a draft without them produces a happy-path template that will fail the first week it has to carry a miss.
- Resist the temptation to combine sections in the monthly. The six are intentionally stable; combining them to "save space" teaches your readers that the shape is negotiable, and the first time you want to use the stability (e.g. to publish a miss) they will interpret the deviation as noise.
- For the quarterly's translation to executive register: write the engineering-register version first in a scratch doc, then rewrite each section in the executive-register. Read both side by side before finalising. A quarterly that sounds like your weekly is a quarterly that is leaving signal on the table at the executive altitude.
- The cross-check is where you catch yourself. The first draft of any reporting surface contains at least one inconsistency the second read will surface; budget the 15 minutes.

## Acceptance criteria

- Four single-source-of-truth pages exist, dated, and are the substrate the three cadence artefacts read from.
- The weekly artefact exists, 500 words (±150), five sections, Friday-style sendable.
- The monthly artefact exists, ~2 pages, six sections, first-Monday-of-the-month sendable.
- The quarterly artefact exists, ~1 page or 3 slides, five sections, executive-register.
- Every number in every artefact carries a noise band, trajectory, or confidence band. No point estimates without context.
- At least one miss is narrated in all three cadences, at the appropriate compression ratio for each.
- The cross-check reviewer note is written and names at least one inconsistency fixed in the second draft.
- A peer manager can read the three artefacts in sequence and summarise the team's quarter honestly from memory afterwards.

## Stretch goals

- **Cross-cadence version control.** Add a short "version header" to each artefact — date, author, which of the four source-of-truth pages it reads from — so future readers can trace back from a cached quarterly to the handbook pages that supplied the numbers.
- **The "no-miss quarter" variant.** Produce a second quarterly for a hypothetical "no-miss quarter" — no releases held, no incidents, no vendor regressions. The exercise is to narrate honestly without a miss to publish; the discipline is to avoid drifting into marketing register. The "watch-list" section becomes the discipline-protecting lever; the next-quarter confidence bands retain their asymmetry.
- **The adversarial reviewer.** Hand the three artefacts (and the single-source-of-truth pages) to a peer manager and ask them to red-team the compression. Specifically: where did your compression hide a decision the reader should have made, where did it over-claim, where does the executive register slide into marketing. Rewrite from their findings.
- **The external-partner variant.** Produce a second monthly tailored to a specific cross-functional partner (product VP, head of AI governance, infra VP). The sections are the same but the emphasis shifts; the exercise is to see where partner-specific emphasis reads differently from the baseline.
- **Twelve-month cadence calendar.** Produce the full-year calendar of weekly / monthly / quarterly sends, with the specific deadlines each hits (release-gate reads, budget variance reviews, exec steering committees). The calendar is the operating substrate the three artefacts drop into; without it, the manager re-decides timing each cadence and misses.
