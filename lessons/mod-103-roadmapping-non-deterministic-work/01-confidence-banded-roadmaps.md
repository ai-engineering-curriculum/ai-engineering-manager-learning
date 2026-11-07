# Confidence-Banded Quarterly and Annual Roadmaps

Traditional software roadmaps publish dates. This one publishes dates **and the uncertainty around them**, because the underlying system — a large model plus retrieval plus tools plus a judge — is not a thing you can estimate the way you estimate a CRUD endpoint. Capability can change mid-quarter when a new model lands. Eval scores regress for reasons that need investigation before you can estimate them. A single evaluation round can turn an "8 points behind GA threshold" prototype into a GA candidate, or bounce an MVP back into prototype. A roadmap that pretends otherwise is a promise the team cannot keep.

This chapter is about reading the uncertainty honestly and publishing it in a form your leadership team can plan against.

## Motivation

Two failure modes drive the design of this chapter.

- **Point-estimate roadmap.** The manager publishes "launch on October 14" with no band. Everyone agrees it will probably slip and then nobody plans the slip. When the model-vendor deprecates the model the prototype is built on, or a safety eval regresses at the release gate, the roadmap has no mechanism to absorb the surprise. Trust compounds down.
- **Everything-is-exploratory roadmap.** The manager refuses to commit because "this is AI, nobody can predict." Leadership loses the ability to sequence dependent commitments (marketing, sales, partner contracts, finance plans). The team becomes invisible in planning and underfunded as a result.

The right posture sits between: date *and* band *and* the handful of events that would collapse or widen the band.

## Core concept 1 — the confidence band

Every roadmap row carries three fields instead of one.

- **Target date.** The single most-likely date — the P50 in common probabilistic language.
- **Band.** Two more dates: the P10 ("if everything goes well") and the P90 ("if the known risks materialise"). The spread between P10 and P90 is the **uncertainty width**.
- **Band basis.** One sentence that names what the band is covering: eval variance, model-release timing, vendor-contract risk, dependency on another team, scope discovery. If you cannot name the basis, you are not confident enough to publish the band yet.

A row that reads `MVP: Oct 14 (P10 Oct 7 · P90 Nov 11) — basis: safety-eval tail risk + unclear judge stability` is planning-grade. A row that reads `MVP: Oct 14` is not.

### Why three dates, not a range

A date range alone ("Oct 7 – Nov 11") gets flattened upward into "mid-October" or "early November" depending on who is reading. Explicit P10 / P50 / P90 keeps the shape of the distribution visible. Leadership that cares about the downside plans against P90. Leadership that is coordinating a marketing beat anchors on P50. The band basis tells them why the two dates are that far apart and what evidence would bring them closer.

Tune the P10/P90 convention to your organisation (some firms prefer P20/P80), but **pick one and publish it**. The worst outcome is one team's "optimistic case" being another team's "committed date."

### Where the band comes from

The band is not vibes. It is built from three inputs, and you should be able to show your work.

- **Historical reference class.** How wide did the band around similar past work turn out to be? For a team's first prototype, you have no reference class; you borrow one from a comparable team or published source and say so explicitly. By the fourth or fifth prototype you have your own.
- **Named risks for this item.** Enumerate the three to five concrete things that could cause slip (specific eval not yet designed, specific vendor contract not yet signed, specific model expected to deprecate inside the window). Each risk has a leading indicator and a check-in date.
- **Capability-shift assumption.** State explicitly whether the plan assumes (a) the current model holds, (b) a specific announced upgrade lands on its announced window, or (c) a capability unlock is possible but not required. This is where mid-quarter model shifts show up in the band; see chapter 03 for the full capacity-plus-cadence treatment.

If you cannot write the reference class or the named risks, your band is a guess dressed as a number. Prefer "not yet estimated" with a date by which you will estimate.

## Core concept 2 — the plan assumes mid-quarter capability shifts

For a traditional engineering team, "the foundation stays constant for the quarter" is a safe assumption. For an AI-engineering team it is false. Frontier model providers ship capability upgrades on roughly quarterly cadences; costs change; context windows expand; tool-use reliability moves; deprecations are announced with weeks to months of notice. Your quarterly roadmap must treat this as expected weather, not emergency.

The mechanism: for each roadmap item, name the **capability dependency** in one of three postures.

- **Current-model floor.** The item ships against the capability the current production model already demonstrates on your evals. A new model is not required. If a new model lands, upside is possible but not necessary.
- **Announced-upgrade dependency.** The item requires a capability the current model does not reliably hit on your evals but an announced upgrade (model release, preview, or API feature with a public ship date) is expected to clear. Name the specific upgrade and its announced window. If the window slips, this item's band widens.
- **Speculative capability.** The item requires a capability no shipped model reliably provides and no announced release has been promised to provide. Prefer **not** to place these on the committed roadmap — park them in the discovery capacity line (chapter 05). If leadership insists, mark the band so wide the implication is obvious (e.g. P90 slips out of the quarter).

Each roadmap row names its posture explicitly. The sum of "announced-upgrade dependency" rows is your **vendor-release exposure** — the fraction of the roadmap that is betting on an external ship date you do not control.

### The re-plan trigger

A capability shift mid-quarter should not blow up the roadmap. It should trigger a **named re-plan event** whose outputs are: updated bands, promoted rows (items a new capability moves from "speculative" to "current-model floor"), demoted rows (items a deprecation makes impossible without migration work), and a one-page delta memo to leadership.

Teams that do this routinely treat the re-plan as a thirty-minute standing ritual the week after a major provider release. Teams that do not end up relitigating the entire quarter in an unscheduled meeting.

## Core concept 3 — the quarterly and annual roadmap shape

### The quarterly roadmap (committed)

- **Commitments** — items the team is actively building this quarter. Each carries target date, P10/P90, band basis, capability posture, and an owner. The number of commitments is bounded by the delivery-capacity line (chapter 05); do not list more than that line can carry.
- **Capacity-reserved lines** — explicit capacity blocks for discovery, incident / regression, and research-to-production handoff. These do not have target dates; they have a **percentage of team capacity** with a defence (also chapter 05). A quarterly roadmap without these lines is implicitly committing 100% of capacity to delivery, which guarantees you cannot absorb incidents.
- **Re-plan events** — pre-booked dates (typically the week after each major announced provider release) where bands are reviewed and the memo to leadership is produced.

### The annual roadmap (directional, not committed)

- **Q1 commitments** at the detail above.
- **Q2–Q4** as **themes with confidence**, not items with dates. Each theme has a one-sentence intent, the top two to four risks that would make it a bad bet in six months, and an explicit confidence level ("high" / "medium" / "low"). Items only move from theme to dated commitment when the quarter is in planning.
- **Capability bets** — one page naming the model-capability shifts the annual plan is *counting on*, and the shifts it is *counting on not happening* (e.g. "we are counting on `gpt-X` context window reaching N; we are counting on `claude-Y` tool-use latency not regressing"). This page is the external-dependency section of the plan.

Leadership can budget against an annual roadmap shaped this way. Leadership cannot budget against twelve months of dated commitments the team knows it will not hit.

## A worked example

Hypothetical team: five engineers, one AI product (a doc-Q&A agent for an internal customer-support org). Reference class: three prior prototype → MVP → GA cycles over eighteen months. Current quarter is Q4.

### Quarterly commitments table (sketch)

| Row | Target | P10 | P90 | Band basis | Capability posture | Owner |
|-----|--------|-----|-----|------------|--------------------|-------|
| Agent-Retriever v2 MVP (new chunking + judge) | Nov 4 | Oct 24 | Dec 2 | eval stability on long-doc traces; two new judges need stability runs | current-model floor | A. Rivera |
| Tool-use expansion (ticketing tool) | Nov 25 | Nov 11 | Dec 23 | tool reliability on announced `[model]` upgrade; dep on infra routing | announced-upgrade dependency on `[model vX, announced window early Nov]` | J. Park |
| Multi-agent planner (speculative) | — | — | — | capability not demonstrated in any shipped model | speculative — in discovery capacity, not on committed line | A. Rivera (discovery lead) |
| Latency SLO work (regression surface) | rolling | — | — | sits in incident/regression capacity | current-model floor | whole team, 15% capacity |

### Capacity lines

- Delivery (committed rows): **60%**
- Discovery / exploratory: **20%**
- Incident, regression, SRE-shaped work: **15%**
- Research-to-production handoff ramp: **5%**

(Defence of these percentages is chapter 05.)

### Re-plan events

- Week after each provider release note on `[model vX]`.
- Last Friday of month 2 (standing).
- On any P50-overrun event (e.g. the day Agent-Retriever v2 passes its P50 without MVP sign-off, the band is re-pulled).

### The one-page annual view

- **Q1 theme — judge quality.** Confidence: high. Risk: new judge framework adoption cost.
- **Q2 theme — agent reliability on third-party tools.** Confidence: medium. Risk: tool-use reliability in third-party APIs.
- **Q3 theme — cost reduction via routing.** Confidence: medium. Depends on model routing infra from infra peer.
- **Q4 theme — new-surface expansion.** Confidence: low. Depends on multi-agent planning capability maturing in the market.

Themes become dated commitments only when their quarter opens for planning.

## What good looks like

A confidence-banded roadmap is doing its job when:

- Leadership can read the quarterly plan and the annual plan in five minutes each.
- The engineering team can point at any band and recite the basis without the manager in the room.
- A mid-quarter provider release triggers the re-plan ritual, the memo lands within a week, and no one is surprised that bands moved.
- A P50 overrun pulls the band immediately — the team does not wait for the P90 to admit it.
- At the end of the quarter, the calibration review (how many commitments landed inside the band, how many P90s were busted, how wide the published bands were vs. the actual outcomes) produces a reference class for next quarter.

## What bad looks like

- "Here is the plan" — one column of dates, no band, no capability posture.
- Bands published but never updated after kickoff.
- Capacity lines sum to 100% delivery; there is no discovery line, no incident line.
- A model release ships mid-quarter; the team debates it ad-hoc in Slack; no memo goes to leadership; dates shift silently.
- End-of-quarter review only reports binary hit/miss on target dates and ignores the band — the team never builds a reference class.

## Summary

- Publish three dates per commitment, not one — P10, P50, P90 — plus the one-sentence basis for the band.
- Name each row's capability posture (current-model floor, announced-upgrade dependency, or speculative).
- Reserve explicit capacity lines for discovery and incident work on the quarterly plan; do not commit 100% to delivery.
- Treat mid-quarter capability shifts as expected weather — pre-book re-plan rituals and produce a short memo each time.
- The annual roadmap is themes with confidence, not twelve months of dated commitments.

The next chapter picks up the lifecycle inside a single row: what evidence promotes a prototype to an MVP, and what promotes an MVP to GA.
