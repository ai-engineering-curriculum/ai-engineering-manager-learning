# Exercise 04: Offline and Online Eval Cadence Design

**Estimated effort:** 4 hours · **Output:** one cadence table + per-metric threshold specification + four drift-triage runbooks + one pager-vs-channel policy, for one feature

## Objective

Produce the four operational artefacts an AI Engineering Manager needs to run the eval cadence from chapter 06: a one-page **cadence table** covering all five stages, a per-metric **threshold specification** separating deploy-gating thresholds from pager-firing thresholds, four one-page **drift-triage runbooks** (one each for generation / judge / retrieval / tool-use), and the **pager-vs-channel policy** that says which thresholds wake on-call vs. which only notify. These artefacts should be enough that on-call engineers, release engineers, and the eval engineer can run the cadence without you in the room.

## Prerequisites

- Chapter 06 (`Offline and Online Eval Cadence`) — the main source.
- Chapters 03 and 04 — deploy-gate thresholds and registry identifiers are referenced throughout.
- The feature from exercise 01 (or an equivalent well-understood feature).
- A rough sense of the feature's traffic volume (helps size canary / monitoring sampling).

## Problem statement

Produce four artefacts for one specific feature.

### Artefact 1 — The cadence table (45 minutes)

A one-page table with these columns, one row per stage (pre-merge, canary, shadow if applicable, A/B if applicable, continuous monitoring):

| Stage | Mechanism | Sample / scope | Judging | Threshold type | Fires on |

For each stage, fill in:

- **Mechanism** — how it runs (CI job name, canary-routing mechanism, shadow pipeline, experimentation system, online monitoring tool).
- **Sample / scope** — what data it reads (the golden suite, the canary slice, the shadow-paired samples, the A/B treatment group, the stratified production sample).
- **Judging** — which judges run (reference the judge registry from exercise 03 by name).
- **Threshold type** — deploy-gating, canary-promotion-gating, pager-firing, channel-only, A/B-primary-outcome.
- **Fires on** — the response path (PR check, release-promotion vote, pager rotation, Slack channel, A/B decision meeting).

Shadow and A/B may not apply for every feature. If you drop a stage, state why (in a row with "N/A — because …") rather than silently omitting it.

### Artefact 2 — Per-metric threshold specification (75 minutes)

For each metric in your suite (behavioural task-success, judge-based quality dimensions, retrieval metrics, tool-use correctness, cost, latency, safety-category rates), write a block containing:

- **Definition** — judge version, dataset version, aggregation method.
- **Deploy-gating threshold** (from exercise 02).
- **Canary-promotion threshold** (if applicable to this metric at canary) — usually equal to or slightly tighter than deploy-gating.
- **Continuous monitoring threshold(s)** — short-window ceiling (for safety / hard floors), day-over-day delta, week-over-week trend, as applicable. State which fire pages and which fire channel-only (links to Artefact 4).
- **Absolute floor** — safety and compliance, non-overridable, specific number.

For continuous-monitoring thresholds, state the **window** (rolling-hour, rolling-day, 7-day vs. 14-day) and the **sample size** (how many judged traces are needed before a page-worthy alert fires — avoids waking on-call on 3-sample noise).

### Artefact 3 — Four drift-triage runbooks (60 minutes)

One page each for the four drift shapes from chapter 06: generation-quality, judge, retrieval, tool-use. Each runbook has:

- **How you recognise this drift** — the signal pattern on the dashboard. Which metrics move, which do not.
- **First diagnosis step** — the single action the on-call engineer takes first (e.g., "pin the model version and re-run on last week's trace slice").
- **Second and third diagnosis steps** — what to do next.
- **Likely causes** — a short bulleted list of the common causes for this drift.
- **Escalation path** — who to page or ping if the first-line can't diagnose (eval engineer for judge drift, RAG engineer for retrieval, agentic engineer for tool-use).
- **Rollback path** — the specific action that reverses this drift's likely cause (roll back the prompt, roll back the judge version, pin the model version, re-index the corpus at prior parameters, roll back the tool-schema change).

Keep each runbook on one page. These are pager-attachment documents, not reference manuals.

### Artefact 4 — Pager-vs-channel policy (30 minutes)

A one-page document stating which monitoring alerts wake on-call and which only post to a Slack (or equivalent) channel. Structure:

- **Non-negotiable pages** — safety absolute-floor breaches (prompt-injection resist rate below the floor, sensitive-data leakage incidents, policy-violation rate spike).
- **Quality pages** — day-over-day quality drops above a specified magnitude with a specified minimum sample count; hard latency / cost ceiling breaches.
- **Channel-only alerts** — latency / cost soft-ceiling drift, moderate quality drift, A/B secondary-metric drift, long-horizon trend lines.
- **The silence-is-silent rule** — if a monitor has fired more than N times in M days without producing an actionable incident, it either tightens or moves to channel-only (prevents alert fatigue).

For each class of alert, name the rotation that owns it (per `mod-107`) and the acknowledgement SLO (how fast on-call must acknowledge).

## Requirements

- Cadence table is one page (landscape is fine), covers all applicable stages, and drops with justification any stage that does not apply to your feature.
- Threshold specification distinguishes deploy-gating from pager-firing explicitly; identical thresholds for both are not allowed for continuous-monitoring metrics. (It is fine for safety absolute floors to be identical — those are the exception.)
- Continuous-monitoring thresholds name a window and a sample-size minimum. "Alert when score drops below 0.8" with no window and no sample-size is not a threshold; it is a noise generator.
- Four drift-triage runbooks exist, each on one page, each pointing at a rollback action.
- Pager policy names at least one non-negotiable page and at least one channel-only alert. A policy with only pages produces fatigue; a policy with only channels produces silent regressions.
- Every artefact references the registries from exercise 03 (judge IDs, dataset IDs). No orphan threshold metrics.

## Starter guidance

- Start with Artefact 2 (thresholds). Per-metric work drives the structure of Artefact 1 (the table) and Artefact 4 (pages vs. channels).
- When sizing continuous-monitoring sample sizes, err generous — most teams page too early on too few samples and fatigue the on-call. A good starting heuristic: at least 100 judged samples in the window before a page-worthy alert fires on judge-based metrics. Safety signals can fire on smaller N (even 5 jailbreak successes is worth a page).
- The judge-drift runbook is the most-skipped and the most-saving. Many teams lose two days to the wrong diagnosis the first time a judge drifts; the one-page runbook pays for itself within the first incident.
- Pager-vs-channel: when in doubt, pick channel-only *first* and promote to page only if a channel-only alert has demonstrated it was needed. Easier to promote than to earn forgiveness after a 3 a.m. page on a noise signal.
- Shadow stage is often skipped. For most features, canary is enough. Shadow is right when the surface is high-risk and a canary would expose users to the risk — model swaps, safety-policy changes, tool-authorisation expansions. If shadow doesn't apply, say so and keep the row with "N/A — because …".
- If your team does not yet run A/B, the A/B row can be `N/A — not yet instrumented; see mod-109 for the A/B capability plan`. Honesty about missing capability is better than aspirational rows that never run.

## Acceptance criteria

- Cadence table is one page, covers the five stages (or justifies missing stages), and names the mechanism / scope / judging / threshold-type / response for each.
- Threshold specification covers every metric in the suite, with deploy / canary / continuous thresholds explicitly separated, and with sample-size minimums on continuous.
- Four drift-triage runbooks exist, each one page, each with recognition / diagnosis / escalation / rollback.
- Pager-vs-channel policy names at least one non-negotiable page and at least one channel-only alert, and names the rotation and ack SLO.
- A new on-call engineer can be handed the four artefacts on their first shift and run the cadence credibly.
- At the next quarterly review (chapter 06 cadence), the artefacts will have been updated against at least one real production signal — budget for that review date as part of this exercise.

## Stretch goals

- **Alert-threshold dry-run.** Replay last quarter's production traces (if your observability store retains them) against your proposed continuous-monitoring thresholds. Count how many pages would have fired, how many channel-only alerts, and how many of them would have been actionable. Tune the thresholds against the count.
- **Shadow pilot.** For a change your team is about to ship, run the shadow stage end-to-end — mirror the production traffic to the new version, judge a paired sample, produce a one-page shadow-report. This is the stage most teams skip until an incident forces them to adopt it; running it once makes adoption easier.
- **Pager-policy defence.** Present the pager-vs-channel policy to the broader on-call rotation (not just your team — the people actually carrying the pager). Collect their pushback. Every page they say "I would have wanted this to be channel-only" is a tuning opportunity. Produce a 10-line retrospective on what they changed your mind about.
