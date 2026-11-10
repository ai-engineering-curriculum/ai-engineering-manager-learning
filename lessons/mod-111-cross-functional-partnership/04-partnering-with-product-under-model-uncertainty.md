# Partnering with Product Under Model Uncertainty

The classical PRD assumes a feature either exists or does not, with a known ship date and a user-visible behaviour the engineering team can implement, test, and ship. The artefact rests on a quiet assumption that the system is deterministic — a given input produces a given output, behaviour does not drift between a Tuesday evaluation and a Thursday launch. For a CRUD endpoint, a billing flow, a mobile layout change, that assumption is correct. For an AI feature it is not.

An AI feature has a probabilistic quality curve, a vendor-dependent capability floor, and an eval surface (mod-104) that may say "no, not yet" after the PRD said "yes." The quality curve is a distribution over inputs that shifts when the vendor ships a new checkpoint, when the retrieval index refreshes, when the judge rubric tightens, when an adversarial case is added. The capability floor is set by what the provider can reliably do this quarter, not what the PRD wished for last quarter. The PM-manager partnership has to translate between the PRD's deterministic grammar and the AI roadmap's probabilistic grammar without either side flattening the other's truth — without the PM pretending the quality curve is a point, and without the manager pretending the ship date is unknowable.

## Motivation — four failure shapes when the PM-manager interface has no translation layer

A team ships AI features with no explicit PM-interface translation layer. The PRD lands, the engineering team builds against it, the launch happens — sometimes. Four failures repeat.

- **The demo-driven PRD.** Written against a prototype that produced a compelling output on a cherry-picked prompt in a product-review meeting. The acceptance criteria quietly encode the demo trajectory: the target user types *this* question, the system returns *that* answer. The eval suite shows the behaviour holds on roughly the demo input and degrades sharply outside it. The team ships to the demo and the first week of production exposes the gap. The failure: acceptance criteria written against a single trace and never translated into the distribution the eval suite could measure.
- **The vendor-unlock that arrived early.** A frontier provider ships a capability mid-quarter that the PRD assumed was two quarters away — longer context, better tool-use, a cheaper tier, a new modality. PRD, launch comms, go-to-market plan, and support readiness are all still writing against the old assumption. The team either compresses the launch into two weeks of unreviewed work (and ships without governance sign-off) or leaves the unlock on the shelf because the PM organisation has no mechanism to absorb a positive surprise.
- **The capability that never landed.** The inverse. The PRD was written against an announced upgrade (mod-103's "announced-upgrade dependency" posture) that slipped past the planned launch date. Leadership has already told partners the ship date. The pressure to "ship something" lands on engineering, and the manager is asked to accept a release gate the eval suite is still rejecting. The PRD never published what evidence would collapse or widen the launch band, so when the band moved there was no vocabulary to explain the move to leadership.
- **The acceptance criteria the eval suite cannot measure.** The PM writes success criteria in product-language — "feel helpful," "grounded in the user's documents," "recover gracefully." The engineering team, under launch pressure, maps these to proxies the eval suite *can* measure — a judge rubric for "helpful," a citation-coverage metric for "grounded," a retry count for "recovers gracefully." The proxies are reasonable; they are also not what the PM asked for, and nobody wrote down the mapping. Two quarters in, the PM is reading green dashboards while user interviews are reporting the original problem.

All four look like disagreements about scope, timeline, or quality. They are not; they are translation-layer failures. The PM and manager are operating in two grammars with no shared artefact to pin them against.

## Core concept 1 — the two roadmap shapes and the translation layer

The PM roadmap is deterministic by assumption. A feature is either on the roadmap or it is not; a feature has a committed launch date or it is unscheduled; a feature either exists at launch or the launch slips. This is not a weakness — it is the correct shape for a function whose downstream customers (sales, marketing, support, finance) need a dated commitment to plan against.

The manager's roadmap (mod-103) is confidence-banded. Each row carries a target date, a P10, a P90, a one-sentence band basis, and a capability posture (current-model floor, announced-upgrade dependency, or speculative). This too is the correct shape — for a surface whose quality curve depends on a vendor's release cadence and an eval suite's referee call.

The two shapes are not reconcilable by collapsing one into the other. The PM cannot publish a P10/P90 band to a sales partner; the manager cannot publish a point estimate to the engineering team without lying about what the team knows. The reconciliation is a three-part translation layer, co-authored per feature.

- **The success-criteria document.** The PM and manager co-author a document that pins each PRD acceptance criterion to one or more numerical metrics on the eval suite (mod-104's golden datasets and judge outputs). "The assistant should feel helpful" becomes "judge-rubric-helpfulness ≥ Y on golden-set-vN, no regression on the adversarial-tail subset." The document names what is measured, on what dataset, and what counts as a pass. If a criterion cannot be pinned, the pair either designs a new eval fixture or rewrites the criterion. Unpinned criteria do not make it into the PRD.
- **The capability-contingent plan.** The pair writes down what happens under both branches of the quality coin. If model quality on eval X is ≥ Y by the launch band, the feature ships at the scope in the PRD. If not, the fallback is named explicitly — not "we'll figure it out" but a specific retrenchment: a simpler surface, a narrower user cohort, a human-in-the-loop fallback, a deferral to the next model release. The fallback carries its own launch comms draft.
- **The published band and the collapsing evidence.** The feature has a planned launch date band (P10 / P50 / P90) visible to PM stakeholders in a form they can plan against — typically the P50 as the "planned date" and the P90 as the "committed-no-later-than" date sales and marketing anchor on. Published alongside is the one-sentence statement of **what evidence would collapse the band**: "the judge-stability run on the new rubric clears variance of X by Date Y" or "the vendor's announced upgrade lands on its Date Z window." When the evidence arrives, the band collapses; when it does not, the band widens. The PM organisation learns to read on the evidence rather than on the date.

<!-- needs-research: confirm whether any public industry source publishes a worked template of this three-artefact translation layer, versus the pattern being composed here from mod-103 and mod-104 internal canon. -->

A PM-manager interface without these three artefacts collapses into one of the four motivation failures.

## Core concept 2 — disagree-and-commit under model uncertainty

The PM and manager will disagree on release-gate calls. This is not a pathology; it is the correct behaviour when two functions with different grammars read the same evidence. The PM is accountable for the commercial outcome; the manager is accountable for the engineering craft and the eval-surface honesty that keeps the team's long-run trust intact.

The disagree-and-commit discipline — articulated in the 2016 Amazon shareholder letter<!-- needs-research: cite the exact passage in the Jeff Bezos 2016 Amazon shareholder letter on disagree-and-commit --> — is the operating rule that makes the disagreement productive rather than destructive. Four mechanics.

- **Disagree honestly in writing.** Each side writes a short memo — a paragraph, not a document — naming the position, the evidence supporting it, and the risk the opposing position carries. "I think we should ship; the eval numbers are inside the launch band; the residual risk is in the adversarial-tail subset." "I think we should hold; the judge-stability run has not been re-executed since the rubric change; the eval numbers may be measuring a moved goalpost." The memos go on file next to the release decision.
- **The decision-maker is pre-named.** The operating agreement between Product and Engineering names in advance who decides what. On commercial launch scope and timing, the product organisation decides and the manager is a reviewer. On release-gate blocks driven by **safety floors** (mod-104 chapter 03's non-negotiable floors; mod-106's R15 launch-readiness checklist), the manager — or the governance partner the manager escalates to — decides and the PM is a reviewer. The pre-naming means the disagreement is about substance, not about who is allowed to decide. If a surface has no pre-named decision-maker, agreeing on one is the first disagreement to resolve.
- **Commit publicly.** Once decided, both sides commit publicly. The decision is announced in the channels the roadmap lives in; the dissent is in the written record but is not re-litigated in Slack or at every subsequent review. The pre-named decision-maker's call is final for the launch window in question.
- **Measure the disagreement.** This is the AI-specific extension. The dissenter's evidence does not disappear once the call is made; it becomes a post-launch eval fixture, a monitoring signal, or a red-team case from the mod-104 data flywheel. If the manager dissented on "the adversarial tail is undermeasured," the adversarial-tail subset gets a named production monitor for the first N weeks post-launch, reviewed at the T+2 week retro. If the PM dissented on "we are leaving user value on the table," the delayed-launch counterfactual is tracked. "Disagree, commit, and **measure** the disagreement" is the rule.

A disagreement handled this way builds trust; a disagreement handled as a slow-boil email thread or a surprise VP escalation destroys it.

## Core concept 3 — three recurring disagreements and how to shape them

Three disagreements recur so reliably across AI-engineering teams that pre-naming their shape is a cross-functional literacy, not a pathology.

### "The model will improve by launch; let's commit the PRD anyway."

PM optimism meets engineering uncertainty. The PM has read the vendor's roadmap; the manager has seen announced upgrades slip before; the PRD is being written against a capability the current model does not yet reliably demonstrate. The PM wants a committed date for sales; the manager wants to say "probably, if the upgrade lands," which is not a committable thing.

Shape: use the confidence-banded ETA from mod-103 and publish the band externally rather than internally collapsing it. The PM takes the P90 to sales as the committed-no-later-than; the P50 goes in the planning deck; the band basis is on file for the sales partner who asks. When the announced upgrade lands on its window, the band collapses; when it slips, the band widens and the sales conversation updates with the slip rather than with a surprise. A single number *looks* committable and is dishonest; a band *looks* fuzzy and is honest. The partnership, done well, teaches the stakeholder side to read bands.

### "The eval numbers are fine; why can't we ship?"

PM frustration with safety floors. The judge-rubric score is above target, the PM has a launch date anchored in a go-to-market plan, and the manager is still holding the release. The frustration at this shape is usually because the pair has not distinguished two categories of metric before the launch conversation.

Shape: distinguish **quality metrics** from **absolute floors**, and name which category each metric falls in on the success-criteria document *before* the launch is on the calendar. Quality metrics — helpfulness, grounding, latency, cost-per-call — are negotiable; the PM owns the trade-off and shipping at a lower helpfulness score to hit a date is a product call. Absolute floors — the mod-104 chapter 03 non-negotiables (jailbreak-resistance, harmful-content refusal, PII-leak rate, grounding on content where the governance surface is explicit) — are not. They are the manager's or governance's call, and no commercial pressure collapses them. When the pair pre-names each metric's category, the launch conversation is a trade-off discussion rather than an authority fight.

### "The model unlocked X earlier than we planned; let's compress the launch."

The inverse problem, harder because it looks like free money. A frontier provider ships a capability mid-quarter that the PRD had assumed was two quarters away. The PM wants to compress the launch into the current quarter; the manager is being asked to shorten the governance evidence packet (chapter 02's territory) to match a vendor's cadence.

Shape: a capability-unlock does not reset the evidence-packet timeline. Governance, legal, privacy, and security reviews run on human-process timelines that are not compressible on a vendor's schedule. The manager publishes in advance a **minimum partner-review timeline** — a floor — naming how many business days each partner review requires from the moment a feature is reviewable. When a capability unlocks earlier, the PM can start the clock earlier; the clock itself does not shorten. This reframes the conversation from "can we compress the review" (which partners cannot do without cutting corners) to "can we start the review on an earlier branch" (often possible if engineering readiness is honestly ahead). The mod-103 re-plan ritual the week after a major provider release is where the unlock gets translated into an updated PRD and an updated partner-review clock.

## Core concept 4 — the artefacts the PM and manager co-own

The translation layer is not a mindset; it is three artefacts that exist on file, dated, and read on a cadence. They mirror mod-103's prototype → MVP → GA promotion gate extended to the PM interface.

- **The feature charter.** Jointly authored by the PM and the manager at the top of each feature. It carries: the problem statement in user-language; the target user and use-case; the success criteria pinned to eval metrics (per core concept 1); the non-goals (what the feature deliberately does not do, so scope creep is a visible negotiation rather than a silent drift); the risk classification from mod-106's risk taxonomy; and the capability posture from mod-103. A PRD without a charter has not survived the translation layer. A charter without a PRD is a plan without a user story. Both exist, dated, side by side.
- **The launch-readiness checklist (R15-adjacent).** The manager-owned surface from mod-106 that gates a release: engineering gate (eval pass, red-team pass, incident-review closed), governance sign-off, legal fit-check, security review, privacy sign-off, support readiness (are support agents trained on the feature's failure modes), and launch comms (is the demo honest about the eval numbers — chapter 05 of mod-112's territory). The PM is a reviewer and co-signer on the commercial items; the manager owns the gate itself. The checklist is public to both sides; neither is surprised by an item on it.
- **The post-launch review at T+2 weeks.** A short, scheduled review two weeks after launch, jointly owned. Agenda: adoption (did the user cohort the PRD named actually use the feature); production quality (how do the eval metrics look on real traffic sampled rather than golden-dataset-fixed); incident count (mod-106 category A/B/C); user feedback (support tickets, interviews); the dissenter's metric from any disagree-and-commit decision; and the next iteration's charter candidate. Output: either "promote to steady-state operation" or a named iteration on the roadmap. A feature without a T+2 review is the zombie shape from mod-110 chapter 05, waiting to form.

The charter is the prototype's entry; the checklist is the MVP → GA promotion gate; the retro is the GA → steady-state review. The PM and manager co-own all three.

## Core concept 5 — handling the "demo vs. eval gap" with Product and leadership

The sharpest cross-functional honesty failure in AI-engineering teams is the demo-vs-eval gap. A compelling demo shows the feature at its best on a prompt the engineer chose; the eval suite shows the feature on a distribution the engineer did not choose, and the eval numbers are consistently lower than the demo would suggest. Leadership, reading the two side by side, often implicitly weights the demo more heavily — it is vivid where the eval is a dashboard — and the launch decision drifts toward the demo.

The manager's push-back is specific.

First, refuse demo-only acceptance. The number that counts is the eval number on the current golden dataset, not the trajectory the demo walked. The manager says this out loud — in the launch-readiness review, the executive brief, the roadmap read — and names the eval number as the referee.

Second, insist on eval-backed numbers in launch comms. Marketing claims should be backed by the eval report, not the demo trajectory. "Our system answers X-type questions correctly N% of the time on a representative test set" is defensible; "our system answers X-type questions correctly" (implicitly always, because the demo did) is not. The PM authors launch comms; the manager is the technical accuracy reviewer and drafts specific redlines rather than vague "this feels overstated" feedback.

Third — the hardest beat — when leadership pressure to "just ship the demo" arises, the manager refuses and the PM covers the external narrative. Division of labour: the manager carries the "the eval suite is still rejecting this" position; the PM carries the "we need to extend the ship date to the next eval pass" narrative with stakeholders who were planning against the earlier date. Neither side does the other's work. A manager writing the stakeholder-facing extension narrative without the PM is overstepping and underdefended; a PM arguing the engineering position without the manager is under-equipped. The chapter-02 governance evidence packet and the mod-112 demo-discipline material are what the manager carries into the pressure conversation.

A pair that holds the line is read as the function that can be trusted to ship AI features that work. A pair that collapses ships the demo once and loses the trust for a year.

## Core concept 6 — the operating cadence with Product

The translation layer and the artefacts land only if there is a cadence that reads on them. The PM-manager operating cadence is a standing meeting — weekly or bi-weekly depending on the team's pace — with three agenda beats keyed to the lifecycle of the features on the shared roadmap.

- **Pre-release.** What's on the roadmap in the next one-to-two sprints; what charters are drafted; what evidence will be ready when (eval runs scheduled, red-team cases in progress, partner-review clocks starting). Output: a shared forecast of the next launch-readiness review.
- **In-flight.** For features being built, eval deltas since the last meeting; red-team findings not yet resolved; any shift in capability posture (an announced upgrade, a benchmark regression); any change in band basis. Output: updated P10/P50/P90 on the shared roadmap and an updated capability-contingent plan where inputs have moved.
- **Post-release.** For features launched within the last four weeks, production quality (eval sampling on real traffic), support signal, user feedback, and the dissenter's metric from any disagree-and-commit decision. Output: either steady-state confirmation or a next-iteration charter candidate.

A fourth beat, quarterly, is the **portfolio surface**: wind-down decisions, scope-change decisions, and the capability bets the next quarter is counting on and counting against (mod-103's annual-roadmap capability-bet page read through the PM interface). This is where features in prototype for more than one cycle get an explicit productise-or-wind-down decision.

This cadence is the integration point between mod-103's roadmap rituals, mod-109's operating cadence, and the PM organisation's own planning cadence. The manager designs the interface beats that let the two cadences compose. A team whose PM standing meeting shares no artefact with the engineering roadmap ritual will see the two roadmaps drift within a quarter. A team whose cadences compose explicitly will find the translation layer has become invisible because it is working.

## What good looks like

A PM-manager interface under model uncertainty is doing its job when:

- Every feature on the shared roadmap has a feature charter on file with eval-pinned success criteria, a named capability posture, and a named risk classification; PM and manager can both recite the charter from memory in a stakeholder review.
- The published launch date is a band (P10 / P50 / P90) with a one-sentence basis; stakeholders read on the P90 for commitments and the P50 for planning, and the band collapses when the evidence lands rather than drifting silently.
- The disagree-and-commit discipline has produced at least one measured post-launch dissent in the last two quarters — one side's objection became a named production monitor or an added eval fixture, and the retro reports on the outcome honestly.
- Quality metrics and absolute floors are categorised on the success-criteria document before the launch conversation; launch decisions argue trade-offs within quality metrics, not authority over the floors.
- A mid-quarter capability unlock triggers the re-plan ritual within a week; the PRD and the partner-review clock are updated in the same meeting; any compressed launch is either on a defended earlier branch or explicitly declined with a reason.
- The T+2 week post-launch review is a standing meeting with a named output every time; no feature ships without one, and the dissenter's metric is on the agenda.
- The demo-vs-eval gap is read out loud in launch-readiness reviews; launch comms cite eval numbers and dataset names, not demo trajectories.

## What bad looks like

- The PRD specifies one ship date; the engineering roadmap specifies another; the two are reconciled in private conversations and nobody in the organisation sees the band.
- Acceptance criteria are written in product-language with no mapping to eval metrics; engineering quietly maps each criterion to a proxy; the PM is reading a dashboard that does not measure the thing the user cares about.
- A release disagreement goes to VP escalation without written memos; the escalation resolves in whichever direction the VP was already leaning; no dissenter's evidence becomes a measurable post-launch signal.
- A frontier capability lands mid-quarter and the team either compresses the launch into two weeks of uncovered governance work or leaves the capability on the shelf because no re-plan ritual exists.
- Quality metrics and absolute floors are not pre-named; every launch is a fresh argument about who is allowed to say no.
- The launch is sold to leadership on the demo; launch comms repeat the demo trajectory as typical behaviour; the first week of production traffic exposes the gap and the retro is defensive.
- There is no standing PM-manager cadence; the two sides meet only when a feature is in crisis; the two roadmaps drift and the translation layer is reconstructed by hand each time.

## Summary

- The PM roadmap is deterministic by assumption; the manager's roadmap is confidence-banded (mod-103). The two are reconciled by a translation layer, not by collapsing one into the other.
- The translation layer is three co-authored artefacts per feature: a success-criteria document that pins PRD acceptance criteria to eval metrics (mod-104); a capability-contingent plan that names the fallback explicitly; and a published launch band with the evidence that would collapse it.
- Disagreements are expected. The discipline is written memos, a pre-named decision-maker (product on commercial scope; manager or governance on absolute safety floors), public commit after the decision, and — AI-specific — turning the dissenter's evidence into a measurable post-launch signal from the mod-104 flywheel.
- Three recurring disagreements have known shapes: "model will improve by launch" (publish bands, resist flattening); "eval numbers are fine, why can't we ship" (pre-name quality-vs-floor categories on the charter); and "capability unlocked early, let's compress" (publish a minimum partner-review timeline as a floor).
- The PM-manager partnership co-owns the feature charter, the launch-readiness checklist (R15-adjacent), and the T+2 post-launch review — mirroring mod-103's prototype → MVP → GA shape through the PM interface.
- The standing operating cadence with pre-release / in-flight / post-release beats (and a quarterly portfolio beat) is the integration point between mod-103, mod-109, and the PM organisation's own planning cadence.

The next chapter extends this translation discipline downstream to the executive-communication and demo-honesty surface (mod-112), where the PM-manager pair faces leadership together.
