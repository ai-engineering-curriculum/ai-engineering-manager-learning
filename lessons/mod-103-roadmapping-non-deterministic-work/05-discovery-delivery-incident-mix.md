# Operating the Discovery / Delivery / Incident Capacity Mix

Every prior chapter in this module pushed work onto one of three capacity buckets — delivery (committed-row execution), discovery (exploratory prototyping), and incident / regression (keeping production honest). This chapter makes the mix itself a first-class artefact. The manager's job is to pick the percentages, defend them to leadership, and refuse the two directions leadership will try to push them.

Research-to-production handoff (chapter 04) is usually a fourth, smaller bucket; some teams fold it into discovery, some keep it separate. The pattern is the same either way: name each bucket, defend its size, and let the mix absorb the uncertainty the delivery line cannot.

## Motivation

- **All-delivery mix.** Leadership asks "what are the five commitments next quarter"; the manager fills 100% of the team with those five rows. The first incident consumes one commitment entirely. The second incident puts the team in the hole. By quarter-end the team has delivered three of five commitments and resents every incident for stealing from the plan — but the plan is what left no room.
- **All-discovery mix.** A new manager overcorrects by reserving so much capacity for "learning and exploration" that nothing ships. Leadership stops trusting the roadmap. Discovery capacity is allocated but produces no handoffs because the manager has not designed the flow from discovery into the delivery line.
- **Invisible-incident mix.** Incident capacity is "whatever is left over." Engineers absorb nights and weekends. The team retains the appearance of delivering 100% to leadership but attrition rises and nobody connects the two.

The fix is named, published percentages — and a defence of each one.

## The buckets

### 1. Delivery (committed-row execution)

The capacity that goes to the roadmap rows published in chapter 01. If a row has a target date, a band, and a capability posture, it sits here. The output is user-visible shipped work.

### 2. Discovery (exploratory prototyping)

Capacity reserved for prototype-grade work (prototype in the sense of chapter 02 — demo-grade, no production traffic, no SLO). The output is a set of **candidate rows** for the next quarter's delivery line — items that have cleared the prototype bar and are ready for the MVP vote.

Discovery is *not* "random time to tinker." Discovery capacity lands on a named set of prototypes per quarter, each with a success criterion ("by end of quarter, this prototype has either cleared the prototype → MVP gate or is explicitly killed"). A prototype that is neither promoted nor killed at quarter-end means the discovery bucket is running without a close-out ritual and the manager should fix the ritual, not add more capacity.

### 3. Incident and regression (keeping production honest)

Capacity for everything that keeps the already-shipped surface working: incidents, regressions, eval maintenance, vendor-migration work (chapter 03), on-call toil, scheduled safety re-evaluations, runbook upkeep. This is **separate from delivery**. If it lives inside delivery, delivery lies about what it is costing the team.

The size of this bucket is a function of the surface you operate — how many features are at GA, how mature their SLOs are, how volatile the vendor stack is this quarter.

### 4. Research-to-production handoff ramp (optional)

Capacity reserved for the engineering side of the R16 contract (chapter 04): reproducing research artefacts, writing integration code, running acceptance evals, staffing the ninety-day retrospective. If you have an active research-to-production pipeline, this is its own bucket. If you do not — the pipeline is dormant this quarter — fold it into discovery.

## Setting the percentages

There is no universal split. There are two useful posture reads you take each quarter.

### Posture read 1 — production surface maturity

- **Pre-first-GA team.** No GA features yet. Incident / regression capacity is small because there is no production surface. Delivery and discovery dominate. A plausible starting mix: delivery 70%, discovery 25%, incident 5%.
- **One or two GA features, early.** The first production features are shipping. Incidents happen more than expected. Teams routinely under-reserve this bucket in the first GA quarter. A plausible mix: delivery 60%, discovery 20%, incident 15%, research-handoff 5%.
- **Multiple GA features, mature.** Several surfaces running, with SLOs and error-budget policies. Incident capacity is now a real line and deprecation work is routine. A plausible mix: delivery 50%, discovery 20%, incident 25%, research-handoff 5%.
- **Portfolio under stress.** An incident surge, a deprecation storm, or a sequence of safety findings pushes incident capacity up. The right answer is usually a temporary rebalance for one quarter, not a permanent mix change. A plausible mix: delivery 40%, discovery 10%, incident 45%, research-handoff 5%. Flag it as temporary and name the event that triggered it; name the next-quarter target mix.

The numbers above are starting references, not prescriptions. The reference class that matters most is **your team's own prior four quarters**: if incidents have historically consumed 30% of the team's time but your plan reserves 10%, your plan will be wrong about the first ten things that go into incident space.

### Posture read 2 — stage of the organisation's AI journey

Independent of your team's maturity, the organisation has its own posture — is leadership pushing exploration ("find the next AI product") or execution ("ship the one we committed to")? Discovery capacity is sized against that pressure. In an exploration-pushing organisation, 25–30% discovery is defensible. In an execution-pushing organisation, 10–15% is the floor — below that you lose the ability to generate future committed rows.

Both postures need an incident bucket. Nothing changes that.

## Defending the mix

Leadership — in two directions, routinely — will try to shrink buckets that look like "not shipping."

### Pressure 1 — "shrink discovery, we have commitments to hit"

The answer: discovery is the mechanism that **generates next quarter's commitments.** A roadmap is a factory; delivery is the shipping dock; discovery is the line that feeds the dock. If you cut discovery, the dock runs for one quarter and empties. In six months the team has nothing to commit.

The defensible artefact: a written list of candidate rows produced by discovery in the previous quarter, and the number of this quarter's committed rows that came from last quarter's discovery. "Of this quarter's six committed rows, four came from prototypes that ran in discovery capacity last quarter" is the strongest defence.

### Pressure 2 — "shrink incident capacity, the team is over-engineering"

The answer: incident / regression capacity is the mechanism that keeps shipped features working. If you cut it, the SLO (chapter 02) breaks, the error budget exhausts, and the team enters a freeze — at which point you lose delivery capacity to a feature-freeze policy you have committed to. The cost of under-reserving is paid in delivery either way.

The defensible artefact: the error-budget burn chart for each GA feature over the prior two quarters, the incident catalogue, and an honest answer to "what fraction of incident-capacity hours went to new-feature-related issues vs. existing-feature regressions." Leadership almost always believes the team is over-engineering because they cannot see the volume of regression work; show them.

### Pressure 3 — "add this one more committed row"

The subtler pressure, usually from a sympathetic executive who believes the team can "absorb" one more. The answer: show the full capacity table (chapter 03), ask which row the new row displaces, and let leadership make the trade. The common failure is accepting the new row without naming a displaced row, and shipping neither.

## The anti-patterns

- **Capacity percentages that never change quarter-over-quarter.** The provider stack moves, the production surface grows, incidents cluster — the mix should move too. A static mix means the plan is being published without being reviewed.
- **Discovery with no close-out.** Prototypes drift for quarters because there is no "promote or kill" ritual at end-of-quarter. Discovery capacity consumed; no candidate rows produced. The ritual fixes this; adding more capacity does not.
- **Incident capacity that gets "absorbed back" at month 2.** A team runs the first month of the quarter without incidents, and the manager quietly moves the unused capacity into delivery. The next month's incident surge then eats delivery anyway. Don't do this — unused incident capacity can convert to discovery (future candidate rows) or to engineering-health work (eval maintenance, runbook improvement, on-call quality), both of which pay back later.
- **The research-handoff bucket run like discovery.** If handoff is on the plan, it is dated work with acceptance gates (chapter 04), not open-ended exploration.

## How the mix shows up to leadership

One slide per quarter. Three or four stacked bars.

- Bar 1: the mix for this quarter (the one being proposed).
- Bar 2: the mix the team actually ran last quarter (actuals from retrospectives — hours or story points or whatever the team measures).
- Bar 3 (optional): the mix in the typical quarter from the prior year (reference class).
- Bar 4 (optional): the mix leadership is implicitly asking for by the sum of their asks (useful when there is pressure to accept extra commitments).

The conversation leadership has when they see the slide is the conversation you want them to have: "we're proposing 20% discovery; we ran 18% last quarter; the asks on the table add up to a 5% discovery quarter — which asks come off?"

A manager who can lead this conversation defends the roadmap. A manager who does not publish the mix ends up being told what the mix is by whoever is loudest.

## A worked read

Same hypothetical team as the rest of the module. One or two GA features, early. Next quarter's proposed mix:

- **Delivery — 60%.** Six committed rows with bands. One (`[row X]`) carries an announced-upgrade dependency; one (`[row Y]`) is a migration triggered by the chapter 03 deprecation calendar. The sum of row estimates at the P50 fits within the delivery capacity, leaving the band for the P90 slip inside the capacity.
- **Discovery — 20%.** Four prototypes, each with a named success criterion. Two came from last quarter's user-research signal; two came from provider release-note reading (chapter 03 upside).
- **Incident / regression — 15%.** Based on actuals from the prior two quarters, averaged and rounded up for a new GA surface added mid-quarter.
- **Research-to-production handoff — 5%.** Integration ramp for the judge artefact from chapter 04's worked example.

The defence to leadership: "If you want a seventh committed row in delivery, it displaces either a row already committed or four hours a week of incident coverage. Choose which. The discovery line is where next quarter's rows come from; cutting it means next quarter is lighter than this one."

## What good looks like

- Four named buckets, with percentages the manager can defend in one minute each.
- A one-slide quarterly view that shows proposed, actual-last-quarter, and (where available) reference-class mix.
- A named discovery close-out ritual at quarter-end that produces candidate rows.
- An honest incident actuals line, published each retrospective.
- Leadership conversations about "add a commitment" always reference the mix and name the displacement.

## What bad looks like

- A plan that implies 100% delivery with no lines for anything else.
- Discovery that never produces a candidate row and is quietly "absorbed" each quarter.
- An incident surge that no one explained because no one was tracking incident actuals.
- A quarter that ends with the team mentally exhausted and leadership happy with the number of items shipped — a reliable early signal of attrition risk.

## Summary

- Name four capacity buckets explicitly: delivery, discovery, incident / regression, and (when applicable) research-to-production handoff ramp.
- Set percentages against two reads: production-surface maturity and organisation posture. Prior-quarter actuals are your strongest reference class.
- Defend each bucket in a specific way: discovery with candidate-row history, incident with error-budget burn and incident catalogue, delivery with the band + capability posture of its committed rows.
- The quarterly capacity slide shows proposed vs. actual vs. reference class. Lead the conversation it starts.
- Unused incident capacity converts to discovery or engineering-health work, not to delivery.

This closes the module. The next steps are:

- **mod-104** picks up the eval-gated SDLC that enforces the release gates introduced in chapter 02.
- **mod-105** picks up the model-and-vendor portfolio work that the chapter 03 capacity-plus-cadence table depends on.
- **mod-106** picks up the full safety-triad vocabulary that chapter 02's safety floor only sketches.
- **mod-107** picks up the AI-specific incident surface that this chapter's incident-capacity bucket exists to absorb.
- **mod-109** picks up the operating-cadence rituals (planning, retrospective, re-plan) this module introduced as beats on the capacity-and-cadence table.
- **mod-112** picks up the executive-communication artefacts (delta memo, capacity slide, band-calibration review) this module prescribes but does not fully teach.
