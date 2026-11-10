# Weekly Roll-Ups, Monthly Deep-Dives, and Quarterly Exec Summaries

The three cadences are the operating surface of this module. Everything else — the investment ask, the regression narrative, the demo discipline, the career plan — reads through them. A team that does not run the three cadences well is a team whose leadership is reading the AI-engineering function in a vacuum, filled in by whichever Slack thread the VP saw last.

This chapter builds the three artefacts: the weekly, the monthly, the quarterly. For each: audience, compression ratio, fixed sections, failure modes, a worked example against a realistic feature. The common engine is a **single source of truth** — a team handbook page the three cadences read from at different compression ratios — so the manager does not author three independent documents a week and inevitably contradict themselves across cadences.

## Core concept 1 — the single source of truth the three cadences feed from

Before writing any of the three cadences, the manager establishes the single surface the team reads on every day. The surface is not glamorous and does not go to leadership. It is the engineering substrate that lets the three cadences be written honestly without a Thursday scramble.

Call it the team handbook. Four pages, each owned, each updated on a known cadence:

1. **Eval-score trajectory page.** The headline eval scores for every production feature, month-over-month and week-over-week, with the slice-level breakouts the trace-review (mod-104) uses. The current release-gate thresholds are on this page; the drift notes from the trace-review are on this page; the unresolved fixture candidates are on this page. Updated by the `ai-eval-engineer` or the senior IC covering evals; the manager reads it every Monday.
2. **Release-and-incident ledger page.** Every release that cleared the gate; every release that was held; every P0–P3 incident with its postmortem link; every regulator-facing Article 73 narrative. Ordered by date. Updated by the release-triad chair and the incident commander; the manager reads it every Monday.
3. **Budget-and-usage page.** The six AI-engineering cost buckets from mod-110 chapter 02, with month-over-month trajectory; the current month's FinOps-lever reading (caching rate, batch utilisation, prompt-size trend); the cost-per-feature attribution. Updated by the Finance partner cadence; the manager reads it the first Monday of the month.
4. **Model-portfolio and vendor page.** The current model portfolio (mod-105), the active vendor contracts and renewal dates, the open deprecation notices, the week's model-watch notes (mod-109 chapter 03). Updated by the senior IC covering the portfolio; the manager reads it every Monday.

Four pages. Every cadence below reads *off* these four pages at different compression ratios. A manager who writes the weekly without reading them is making it up; a manager who maintains them without compressing them is drowning in substrate the leadership audience will never open. The art is in the compression.

## Core concept 2 — the weekly team roll-up

**Audience.** The manager's direct line (engineering director or VP), the horizontal partners (product counterpart, infra peer manager, governance peer), and any senior IC on the team who is not in the manager's 1:1 loop.

**Compression ratio.** ~500 words, five sections, five minutes to read. Not a status report; a *situational-awareness* document that lets the director make this-week decisions without a sync.

**Cadence.** Friday morning (or Thursday in orgs with Friday-off cultures). Fixed day, fixed sender, fixed subject line. Partners build a Monday-morning habit of reading it; irregular delivery kills the habit.

**Five fixed sections.** The shape does not vary week-to-week. Stability of shape is what lets the reader scan at speed.

1. **Headline.** One sentence: what changed this week that matters to the reader. If nothing changed, say so — "no material change on the engineering surface this week; release-gate posture held; caching rate held at 42%." A manager who cannot write "nothing material changed" when nothing did is producing noise.
2. **Release-gate posture.** One or two sentences on the release-gate status of the current release candidates. Did a release ship? Did a release hold? Did a release miss on a specific slice? A link to the release-ledger entry. The reader should know, in one scan, whether the team is ready-to-ship, mid-ship, or holding.
3. **Eval trajectory.** Three numbers: the headline suite score this week, the delta vs. last week, and the delta vs. the four-week moving average. One sentence of context on any drift seen (generation drift, judge drift, retrieval drift, tool-use drift — mod-104). A link to the eval-trajectory page. If the delta is inside noise, say so — "delta +0.3 points, inside the ±1-point noise band."
4. **Incidents and misses.** Any P0–P3 incident opened or closed this week; any release that missed its eval delta; any vendor regression observed. One sentence each, plus a link. If none, say "none" — this is the honesty-by-construction lever. A manager who skips this section in weeks when there were no misses teaches the reader to assume the section is only ever populated when the misses are forced.
5. **Next week's watch-list.** What the manager is watching in the coming week — a vendor release the team is planning around, a judge-model upgrade that might force a threshold recalibration, a planned release window, a cross-functional sign-off on the calendar. Three bullets maximum. The reader uses this to anticipate asks before they land in Slack.

A concrete worked example (the running feature is an internal support-reply agent):

```
Subject: AI eng weekly — 2026-11-07 — csrep-agent, v1.4 train

Headline: Shipped csrep-agent v1.4 Wed; headline eval held at 78.2 against a 76.0 gate; 
one slice miss on "billing-dispute" (-3.1 vs. train), published below.

Release-gate posture: v1.4 released 2026-11-05 after release-triad sign-off (ledger: 
handbook/release-ledger#2026-11-05). v1.5 candidate opens Monday; eval re-baseline on
the new provider snapshot kicks off same day.

Eval trajectory: headline suite 78.2 (Δ +0.4 vs. last week, Δ -0.1 vs. 4-wk mavg). 
Billing-dispute slice 68.4 (Δ -3.1 vs. train, below release-gate floor of 70 on that 
slice; triaged — see Incidents). Judge drift note: judge-v3 upgraded this week; 
re-ran 500-item calibration set, Cohen's κ against human raters held at 0.72, inside 
the ±0.05 stability band.

Incidents and misses: 
  - Release miss, billing-dispute slice -3.1. Reason: retrieval drift against a new 
    billing-template corpus the support team rolled out 10-28 without eval-fixture 
    sync. Fixture queued (eval-registry#412); re-train opens Monday; expected to 
    recover inside the v1.5 window.
  - No P0–P3 incidents this week.

Next week's watch-list:
  - Anthropic model-snapshot release expected mid-week (per handbook/model-portfolio); 
    model-watch meeting Tuesday will set the re-baseline decision.
  - DPIA for the new finance-template indexing feature sits with the DPO; expected 
    signed by Friday (mod-111 release-approval workflow).
  - Procurement renewal cadence opens for the primary provider contract in Nov 20 
    window; ask-list drafted, going to the FinOps + Finance sync Wed.
```

Four features of this weekly worth naming. First: the honesty-by-construction. The "release miss" line appears in the first weekly after the miss is known, not six weeks later when a partner forces it. The reason is named; the queued fix is named; the expected recovery window is named. Second: the links. Every claim links to a handbook page the partner can open if they need depth. The weekly is a *router*, not an archive. Third: the stability of shape. If the partner reads one of these a month for a year, they build a mental model of "where does the eval delta live, where does the miss live, where does the vendor-watch live." The fourth section is always the fourth section. Fourth: the compression. A weekly that takes more than ten minutes to read is not a weekly; it is a monthly that happened on the wrong cadence.

## Core concept 3 — the monthly deep-dive

**Audience.** The manager's director, the second-line (VP or SVP), and the cross-functional leaders who read AI engineering as a shared surface — product VP, infra VP, head of AI governance, head of AI enablement.

**Compression ratio.** ~2 pages, 15 minutes to read. The monthly is the first cadence where *trend reading* happens — not just what moved this week but what is moving month-over-month, and what that trajectory suggests for the next three months.

**Cadence.** First Monday of the month. The month that just closed is the month being reported on.

**Six fixed sections.**

1. **Month in one paragraph.** Three to five sentences that would stand alone as the month's narrative if the reader got nothing else. The headline eval trajectory, the headline release outcome, the headline miss. If this paragraph is boring, that is correct — AI engineering delivered at altitude is routine.
2. **Eval-trajectory read.** Month-over-month movement on the headline suite and the top three slices. A chart (sparkline or small line chart) with the previous six months' trajectory, not just a single-month delta. Drift notes: did the team re-baseline on a new model or judge this month? Did a slice move outside its noise band? The reader needs to see trajectory, not point estimates, to know whether the function is improving or holding.
3. **Release summary and miss ledger.** Every release that cleared the gate this month, with a one-line result; every release that was held, with the reason; every release that shipped-with-a-miss, with the slice and the reason. The miss ledger is the paired output of the hits summary — never one without the other.
4. **Budget-and-usage read.** The month's spend across the six mod-110 buckets, month-over-month trajectory, the current-month FinOps-lever reading, variance vs. plan if applicable. One sentence on the lever being pulled this month (caching, prompt-size, batch, fallback-routing, context-window). This section is the handoff to Finance's variance explanation; the Finance partner should be able to lift the numbers verbatim.
5. **Incident read.** Every P0–P3 incident this month, the postmortem link, the one-sentence systemic lesson that fed back into the eval harness. If there were no incidents, say so — but also say *what the on-call rotation saw*, because "no incidents" without any near-miss signal is suspicious.
6. **Next month.** The planned release windows; the planned vendor-portfolio moves (migrating to a new default model snapshot, opening a new vendor evaluation); the open decisions the director can help with. This is the manager's lever for upward alignment — putting a decision in the section forces it to the right altitude.

A sketch of the monthly's shape (section 1 only, to show the compression):

```
Month in one paragraph. In November 2026 csrep-agent shipped two releases (v1.4 and 
v1.5) against a stable headline eval of 78.1 ± 0.6 (prev six-month trajectory: slow 
improvement, +2.3 from May). One slice miss on billing-dispute in v1.4 (-3.1), traced 
to a retrieval-corpus drift and recovered in v1.5 (back to 71.4 against the 70.0 floor).
No P0 or P1 incidents; one P3 (prompt-escape in a developer-facing agent, 
contained in <20 minutes, postmortem linked). Model-provider spend held against plan 
(+2.4% within the ±5% band), on a 38% caching rate (up from 31% last month, driven 
by the prompt-template refactor shipped in v1.3). Next month opens the finance-template 
index feature, with DPIA sign-off expected week 1 and release target week 3.
```

A month in one paragraph. The rest of the monthly expands each clause with the specific numbers, the links, and the trend charts. A partner who reads only that paragraph has an accurate top-of-mind read on the AI function; a partner who opens the full monthly has the detail they need to make decisions.

## Core concept 4 — the quarterly exec-ready summary

**Audience.** The executive altitude — VP, SVP, CAIO, possibly CTO or CEO — and, through them, the board or the external narrative. In some orgs the quarterly is read verbatim at the AI steering committee; in others it is briefed up by the manager's director.

**Compression ratio.** ~1 page of prose or 3 slides. Five minutes to read. The reader is not reading to understand AI engineering; they are reading to allocate capital, approve the next quarter's plan, and set the external narrative. Write to what they need to decide.

**Cadence.** First week of the quarter close. The quarter that just closed is the quarter being reported on; the quarter opening is the quarter being planned.

**Five fixed sections, each translated to executive register.**

1. **Quarter in one slide (or one paragraph).** The quarter's shipped outcomes, the honest miss ledger in one sentence, the model-uncertainty posture at quarter-end, the budget posture. Translated to executive register — not "the suite held at 78.2 against the gate" but "the feature held quality against our release bar; one slice regressed temporarily and was recovered the next release."
2. **The quarter's bets, outcome of each.** For each major initiative the quarter started with: did it land, did it miss, is it mid-flight? For misses, the *reason* in one sentence. For mid-flight, the revised landing window and the specific reason for the move. Executives tolerate a miss that is explained; they do not tolerate a miss that is surfaced to them for the first time at quarter-end.
3. **Honest regression read.** One paragraph. The misses that were worth naming at exec altitude — a release that missed its eval delta, a user-facing incident, a vendor regression that hit the product. The *reasons* named; the *lessons fed back into the eval harness* named. This is chapter 04's depth; here, it is compressed to the single paragraph the executive reads.
4. **Model-uncertainty posture.** One short paragraph, translated out of eval jargon. "Our internal support feature passes our quality bar on 78% of tickets end-to-end; the remaining 22% are escalated to human review by design, and we measure both the pass-through rate and the human-override rate weekly" is the shape. The executive needs to know *how good is the feature, under what conditions, and where does human review enter*. Not the eval-suite number out of context.
5. **Next quarter's plan with confidence bands.** Three bets for next quarter, each with a low / mid / high confidence read on the outcome and the named condition each band assumes. The quarterly plan is a plan under non-determinism; the confidence band is the shape it takes. Chapter 03 teaches the citation discipline that makes the bands defensible.

The translation discipline worth being explicit about. The weekly's register (eval delta, judge drift, red-team pass, release-gate floor) is correct for the audience of partners who share the vocabulary. The quarterly's register is not. "The feature cleared the release gate" is not "the eval suite passed the 76-point threshold." "The miss was traced to a retrieval-corpus drift and recovered the next release" is not "the slice regressed -3.1 against the train number and we re-indexed the billing-template corpus against the eval-fixture registry." A manager who publishes the quarterly in weekly-register is leaking signal; a manager who publishes the weekly in quarterly-register is drowning partners in abstraction. Translate, do not lazy-copy.

## Core concept 5 — three failure shapes across all three cadences

Three mistakes that recur regardless of cadence. All three are honesty-by-construction failures.

- **The silent absence of the misses section.** In weeks with no misses, the section does not appear. In weeks with misses, it does. The reader quickly learns the section is optional and begins to assume misses are being hidden when the section is missing, even in weeks when nothing missed. The fix: the section is always present, always labelled, and populated with "none" when none. Mod-104's trace-review discipline and mod-107's postmortem discipline both encode this move; the reporting surface inherits it.
- **The single-number delta.** The weekly shows "eval delta +0.4" without a noise band; the monthly shows "month-over-month +1.2 points" without the previous six months' trajectory; the quarterly shows "feature quality improved" without a confidence band. Single numbers without context are not reporting; they are marketing. The fix: every number carries its noise band, trajectory, or confidence band. Mod-104's confidence-interval discipline and mod-110's three-band ROI discipline both encode this move; the reporting surface inherits it.
- **The register slip.** The weekly reads at exec-register; the quarterly reads at engineering-register. Partners of the weekly cannot act on it because the specifics are missing; executives of the quarterly skim past it because it is unreadable without the glossary. The fix: the two translations are different writing exercises. The single source of truth makes the compression tractable, but the translation itself is work the manager cannot skip.

## What good looks like

A team has a manager-owned reporting cadence when:

- The four single-source-of-truth pages are current; the manager reads them every Monday; the partners can open them without asking.
- The weekly is sent every Friday on a fixed subject line; the five sections are stable; misses appear in the week they occur.
- The monthly is sent the first Monday of the month; the six sections are stable; trajectory is read over six months, not a single point.
- The quarterly is sent the first week of the quarter close; the five sections are stable; the register is translated to executive altitude.
- Every number in every cadence carries a noise band, a trajectory, or a confidence band. No point estimates.
- Every miss is published at the cadence it emerges — weekly first, monthly summary, quarterly paragraph — with the reason.

## What bad looks like

- The handbook pages exist but are weeks stale; the Friday weekly is written from Slack search.
- The weekly's day of the week drifts; partners stop reading because the habit never formed.
- The monthly is a longer version of the weekly; no trend charts; no trajectory reading.
- The quarterly is a hits-only deck; the miss ledger is an appendix nobody opens.
- Single-number deltas appear in every cadence; the executive assumes the numbers are more precise than they are, and the next quarter the manager is accused of over-promising when the actual outcome lands in the noise band.
- Misses are published only when a partner forces them; credibility erodes.

## Summary

- The reporting surface is three cadences — weekly, monthly, quarterly — for three audiences at three compression ratios. All three read off a shared single source of truth (eval, release-and-incident, budget, model-portfolio pages).
- The weekly is 500 words, situational awareness, five fixed sections, Friday. The monthly is 2 pages, trend reading, six fixed sections, first Monday of the month. The quarterly is 1 page or 3 slides, executive altitude, five fixed sections, first week of quarter close.
- Honesty is a construction. The misses section is always present; every number carries a noise band or trajectory; the register is translated for each audience.
- The next chapter composes these cadences into the investment-ask narrative — model-spend, labelling, headcount, tooling — defended honestly against the empirical productivity literature.
