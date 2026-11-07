# Capacity Planning Paired with Provider Release Cadence

Your roadmap sits on top of a model stack you do not own. The vendor deprecates models; the vendor ships capability upgrades; the vendor changes prices; the vendor publishes new release-safety language and tightens or loosens what their models will do in production. If the capacity plan does not treat the external stack as a moving part, every provider announcement becomes an unplanned fire drill. This chapter makes the external cadence a first-class input to capacity planning.

A note on what this role owns: you are a **consumer** of provider release-safety frameworks (Anthropic RSP, OpenAI Preparedness, Google DeepMind FSF). You do not author them, and you are not the compliance owner. You read them as planning inputs — early-warning on behavioural changes, capability-tier shifts, and the cadence of the next release wave. The owning depth belongs to governance peers; the planning use belongs to you.

## Motivation

- **Frozen-stack plan.** The roadmap is written as if the provider stack will not move for a year. Six weeks in, a model is deprecated with three months of notice and the plan has no room to migrate. The team either misses commitments or ships a worse user experience on a less-capable fallback.
- **Reactive-only plan.** The team never reads vendor release notes until the day of an announcement. Every release becomes a Slack thread that consumes a week. The team never captures the upside — a new capability that could promote a speculative item to current-model floor sits unused because no one noticed.

The fix is pairing. Each quarter's capacity plan is written against the provider cadence; each major provider release triggers a named ritual that produces a short memo.

## Core concept 1 — the provider cadence has four shapes

Your job is to recognise which shape is landing so you can route it to the right capacity response.

### 1. Model release (capability unlock)

A new model is announced and ships. Capability typically improves across an uneven surface — some tasks noticeably better, some unchanged, some regressed. Your work: a one-off evaluation sprint against your registered suites to find the delta, then a decision on whether to migrate.

**Capacity response:** a bounded block (a few engineer-days, not weeks) scheduled in the week following the announced ship date. Owner: an engineer with the eval harness in hand, not the manager.

### 2. Model deprecation (forced migration)

An existing model is announced end-of-life, with a date typically weeks to months out. Vendors publish deprecation pages; teams that read them survive, teams that do not get surprised. Public exemplars of this surface (not an exhaustive list, and these change regularly — check the current docs):

- OpenAI maintains a public deprecations page listing model shutdown dates and recommended successors.
- Anthropic documents model deprecations and recommended migration paths.
- Google documents model lifecycle for Gemini / Vertex AI.
- Azure OpenAI documents its own model retirement policy for the Azure-hosted variants.

**Capacity response:** add a migration line to the quarterly capacity plan the moment the deprecation lands in the vendor doc. The migration has: destination model, eval re-run plan, prompt / judge revalidation, cost delta, SLO implication, target completion date well before the vendor's deadline. If you are using a model your production workload cannot be migrated off without a full eval cycle, that model is a structural risk on the capacity plan every quarter.

### 3. Pricing reset

The vendor changes per-token pricing, context pricing, cached-prompt pricing, batch pricing, or the structure of enterprise tiers. Some of these are quiet improvements; some are increases disguised as reshaped bundles.

**Capacity response:** a short re-run of the cost projection for any feature currently near its cost ceiling (MVP cost ceiling from chapter 02 or GA budget line). Push the result into the quarterly cost review (a cross-mod artefact with `mod-110-budget-finops-and-build-vs-buy`). A pricing reset rarely demands roadmap changes by itself; a pricing reset that moves one of your features out of its cost ceiling does.

### 4. Release-safety framework update

The provider publishes an updated version of its release-safety framework (Anthropic RSP, OpenAI Preparedness, Google DeepMind Frontier Safety Framework). Read these for **what they signal about future releases** — which capabilities are on the vendor's internal watchlist, how the vendor expects to gate the next tier of models, and what mitigations they commit to. You are reading them as a planning input, not as a policy you enforce.

What to extract for your plan:

- Named capability thresholds the vendor is tracking. These hint at which future capabilities might be gated, delayed, or released with extra mitigation — your "announced-upgrade dependency" rows (chapter 01) should name these.
- Deployment-tier language. If the vendor commits to pre-deployment evals for certain capabilities, expect public releases of those capabilities to lag the research announcement — build the lag into your roadmap bands.
- Policy text that would constrain how you are allowed to use the model. These are almost always surfaced in the acceptable-use policy and in the enterprise contract; the framework update is the signal that enforcement posture may shift.

**Capacity response:** at most a review conversation (an hour, not a week) with the governance peer whose remit this is. The governance peer owns the policy response. You own the capacity-planning response: adjust your "announced-upgrade dependency" rows if the signal changes their timing.

## Core concept 2 — the quarterly capacity-plus-cadence table

The deliverable is a one-page table joining your capacity plan to the provider cadence you can see coming.

| Week | Capacity block | Expected provider event | Owner |
|------|----------------|-------------------------|-------|
| W1 | kickoff + quarter re-forecast | — | manager |
| W2–W3 | delivery (committed rows) | — | engineering |
| W4 | **re-plan ritual** | announced provider model release window | manager + architect |
| W5–W8 | delivery (committed rows) + eval sprint on new release | migration deadline for deprecated model `[X]` lands in vendor docs | eng lead for migration |
| W9 | mid-quarter cost review | pricing bulletin cadence from `[providers]` | manager + finops peer |
| W10–W12 | delivery + RSP/Preparedness/FSF read | framework update window | manager + governance peer |
| W13 | end-of-quarter calibration review | — | manager |

The numbers are illustrative. The pattern — named events paired with specific capacity blocks — is what the manager produces each quarter. The pattern fails if you write it once and never update; it succeeds when you refresh it on the first of each quarter by reading the current vendor release notes and deprecation pages.

## Core concept 3 — the re-plan ritual

Already introduced in chapter 01. Expanded here because the provider cadence is its most common trigger.

### Inputs

- The provider release note or deprecation announcement.
- The current committed-rows list with its bands and capability posture.
- The eval-suite catalogue from the eval team (or the `mod-104` equivalent artefact if the team is small enough that there is no separate eval team).

### The ritual (thirty minutes, standing)

1. Read the announcement end-to-end. Not the summary — the actual doc.
2. For each committed row with "announced-upgrade dependency," check whether the announcement either delivers the dependency (promote) or slips it (widen the band).
3. For each row with "current-model floor," check whether the model in question is deprecated (migration line added) or materially cheaper (cost ceiling re-run).
4. For each row in "speculative," check whether the new capability makes any of them current-model-floor candidates (promote from discovery to delivery, consume the discovery capacity block).
5. Produce the delta memo.

### The delta memo

One page. Audience: your direct leadership. Contents:

- The event: one sentence on what the provider announced.
- Rows affected: the three to five rows whose bands or posture changed, with the new numbers.
- Opportunity captured: any speculative row promoted to delivery.
- Capacity delta: how the capacity table changes this quarter.
- Ask: what, if anything, leadership needs to decide (usually: approve a migration line that will displace a committed row, or approve a scope change).

A re-plan ritual that does not produce the memo does not count. The memo is what makes the roadmap honest to leadership.

## Core concept 4 — what this role does **not** own

Draw the line explicitly, because the governance surface is new in most organisations and work drifts into whoever is paying attention.

- **Authoring internal AI policy that mirrors vendor safety frameworks.** Not you. Governance peer.
- **Legal response to vendor acceptable-use changes.** Not you. Legal.
- **Enterprise-contract renegotiation on pricing reset.** Co-owned at best. Procurement or legal leads; you contribute the usage / cost projections.
- **External engagement with the vendor on their framework (RSP / Preparedness / FSF).** Not you. The governance or CAIO function.

You read the frameworks as planning inputs, bring the planning implications to the re-plan ritual, and loop the governance peer in as soon as policy posture is in play. Reading these as a planning input is a weekly habit, not a vendor-management responsibility.

## Capacity blocks: how the external cadence sizes them

The percentages are chapter 05's full topic. The external cadence raises or lowers them in two ways:

- **Deprecation density.** A quarter with two or three announced migrations consumes more incident / regression capacity than a quarter with none. The capacity line expands at the expense of discovery, not delivery (because delivery already carries bands that will absorb some slip).
- **Release upside.** A quarter where a major model release is expected mid-window may justify expanding discovery capacity for the back half of the quarter to absorb promotions from speculative rows.

Both of these are **named adjustments** in the capacity plan, not post-hoc apologies. The plan is honest about them at kickoff.

## A worked quarter

Same hypothetical as chapters 01 and 02. Q4 opens; here is the manager's capacity-and-cadence read.

- `[Primary production model]` — on vendor's release-cadence; a successor is announced for `[window inside Q4]`. Action: one engineer-day eval sprint scheduled W5. Memo to leadership on W6.
- `[Secondary production model]` — deprecation announced for `[date in Q1 of next year]`. Migration line added to Q4. Target migration completion: W11, with a two-week safety buffer before the vendor's deadline. Owner named.
- `[Tool-use subcapability for the agent surface]` — announced in `[provider release blog]`, availability in `[window late Q4]`. One committed row has an announced-upgrade dependency on this; band basis mentions the dependency explicitly.
- Pricing: no reset announced by any of the primary providers. No cost re-run scheduled. If a reset lands mid-quarter, the W9 cost-review cadence absorbs it.
- Frameworks: `[provider]` has flagged a framework update is possible in `[window]`. Reading-and-review conversation with governance peer booked in the capacity plan. No ownership change.

## What good looks like

- A one-page capacity-and-cadence table published at the start of the quarter and updated at each re-plan ritual.
- Every committed row has an explicit capability posture, and the "announced-upgrade dependency" rows name the specific announcement and date they are depending on.
- A deprecation lands; a migration line is on the capacity plan within 48 hours; the delta memo goes out within a week.
- The manager can describe, in one sentence per provider, what the next release looks like, when it is expected, and which roadmap rows are affected.

## What bad looks like

- The manager hears about a deprecation from an engineer who hit an API error.
- A new model lands; the team either migrates immediately in a panic or ignores it entirely. No eval sprint. No memo.
- The capacity plan is reused quarter-over-quarter with the same percentages regardless of the external cadence that quarter.
- The governance peer learns about a provider framework update when the manager forwards them a Slack link after the news is three days old.

## Summary

- Treat provider releases, deprecations, pricing resets, and framework updates as four distinct event shapes, each with a specific capacity response.
- Publish a capacity-and-cadence table each quarter. Name the events you can see coming and the capacity block that absorbs each one.
- Run the re-plan ritual on every major provider announcement; produce a one-page delta memo.
- Read provider release-safety frameworks (RSP / Preparedness / FSF) as planning inputs, not as policy you own. Policy ownership sits with governance peers; planning use is yours.
- Size capacity blocks against the external cadence — deprecation-heavy quarters expand incident/regression capacity; release-upside quarters may expand discovery.

The next chapter picks up the other external interface a manager routinely owns: the handoff from research into production.
