# Exercise 02: Prototype → MVP → GA Promotion Criteria

**Estimated effort:** 3 hours · **Output:** two filled-in gate checklists (prototype → MVP, MVP → GA) for one real feature, plus the pre-written promotion-vote agenda and decision memo template

## Objective

For **one specific AI feature** your team ships or is planning to ship, produce the written gate criteria from chapter 02 — not as a generic template, but filled in with this feature's eval suites, baselines, cost numbers, safety posture, SLO targets, on-call path, and rollback mechanism. The exercise ends with a dry-run of the first promotion vote (prototype → MVP), using the template you produced.

## Prerequisites

- Chapter 02 (`The prototype → MVP → GA promotion gate`).
- Skim of chapter 05 for the discovery / delivery / incident mix (your feature consumes capacity from a specific bucket at each posture).
- One feature in mind, with enough detail that you can fill in the evidence sections. Three usable patterns:
  - A feature your team currently has in prototype or MVP posture.
  - A feature at a prior employer you can describe honestly, with the numbers abstracted where needed.
  - A hypothetical feature for a composite team, with the feature charter written by you and the numbers `[placeholder: measure before quoting]` where realism is impossible.

## Problem statement

Produce the following four artefacts.

### Artefact 1 — Feature charter (15 minutes)

One paragraph. Feature name, audience (who sees it), what task it supports, and the one-sentence answer to "what problem does this feature solve." If anonymised, note it at the top. One further sentence naming the current posture (prototype, MVP, or GA target) and the next promotion vote that is scheduled.

### Artefact 2 — Prototype → MVP gate (60 minutes)

The five-piece gate from chapter 02, filled in for this feature. One section per piece. Each section is one or two paragraphs, with concrete numbers and named artefacts, not generic language.

1. **Eval delta vs. baseline.** Name the eval suite (with link to registration or source). Name the baseline (production system being replaced, no-retrieval variant, published benchmark, or similar). State the margin threshold — the specific number the vote will test against. State the reproducibility expectation (how the eval is re-run, from what SHA, with what config).
2. **User-research signal.** Name the user cohort and the session count the vote will accept. State the "wrong answers hurt how much" one-sentence read. State the task-fit read. If the user-research work is not scheduled, name the date by which it will be.
3. **Cost ceiling.** Name the per-request cost target at the modal request size, in dollars **and** as a ratio to the baseline. Name the monthly ceiling at projected MVP traffic. Write the abort rule — the specific condition that pauses the MVP for review.
4. **Safety floor.** Name the safety eval suites that will run (jailbreak, prompt-injection if tool-using, content-category sensitivity). Name the guardrail layer (input moderation, output moderation, policy-aware system prompt, or whichever is appropriate to this audience). Write the one-sentence abuse-and-harm hypothesis for this audience and the monitoring hook that would surface the first ten incidents of it.
5. **Owner-of-record and rollback mechanism.** Name the single human owner on the team. State the rollback mechanism (feature flag, config revert, prior model pin, whichever) and the worst-case rollback time at MVP traffic. State the audit-log posture — where traces land, retention, who has access.

A passing section is specific enough that a reviewer can ask "how will you know if this piece fails" and the answer cites a metric, a monitor, or a named observable. A generic section — "we will run safety evals" — fails the exercise.

### Artefact 3 — MVP → GA gate (60 minutes)

The eight-piece gate from chapter 02, filled in for this feature. Items 1–5 build on Artefact 2's answers; items 6–8 are GA-specific. One section per piece. Where an item is not yet in place for this feature, name the dated commitment that will put it in place before the GA vote.

1. **Eval delta holds under production traffic.** Name the production-traffic-derived eval set (how traces are sampled, how the judging slice is built, who refreshes it). Name the acceptance threshold on this set.
2. **User signal is a sustained outcome, not a demo.** Name the outcome metric (satisfaction, task completion, downstream action), the window, and the acceptance threshold. Name the cherry-pick-prevention mechanism (e.g., full-window measurement rather than a selected week).
3. **Cost at GA scale.** Model the cost at projected first-six-months-of-GA traffic. Compare to the GA cost budget (from `mod-110-budget-finops-and-build-vs-buy`, if available, or from your organisation's equivalent). If out of band, name the mitigation (smaller audience, routing strategy, cheaper judge, tier downgrade).
4. **Safety monitored in production, not only tested pre-release.** Name the safety-eval cadence (weekly, on-release, or whichever). Name the production monitor for the named abuse-and-harm hypothesis and the review fraction. Name the release-triad sign-off (per `mod-106` vocabulary).
5. **Release gate enforced in CI.** Name the CI gate configuration — which eval thresholds, cost thresholds, and safety thresholds the gate enforces. Confirm that a failing gate blocks the PR without a human override. If a human override exists, document who can issue it and under what written policy.
6. **Incident playbook exists and has been rehearsed.** Name the three incident types this feature is most likely to produce (regression on eval X, cost spike, policy violation at scale). For each, link or draft the runbook. State the date the team last rehearsed the playbook in a tabletop. If no tabletop has run, name the date one will.
7. **On-call readiness.** Name the on-call rotation this feature joins, the pager configuration, the primary / secondary tier structure, and any shared coverage with the `ai-infra-team-lead` peer. Name the handoff document.
8. **SLO and error-budget policy.** State the SLIs (latency, availability, cost-per-request, quality signal) with target values and measurement windows. Write the error-budget policy — specifically, what happens when the budget is exhausted (typically a feature-freeze on this surface). Name the SLO-review cadence.

### Artefact 4 — Promotion-vote agenda and decision memo template (30 minutes)

Write both artefacts.

- **Agenda** — a thirty-minute agenda for the prototype → MVP vote on this feature. Name the chair (you), the technical veto participant (architect or principal MLE), the governance participant, the product participant. Allocate minutes per gate section. Reserve a slot for the pass / fail call per section and a final go / no-go.
- **Decision memo template** — a short markdown skeleton the chair fills in during (or immediately after) the vote. Sections: feature + posture + vote type + date, summary of each gate section's call (one line each), overall decision, remediation list if any, re-vote date if any, owner-of-record. Keep it to one page.

Then, if your context permits, run a **dry-run vote** with a willing colleague or two (one playing the technical veto, one playing product or governance). Fill in the decision memo based on the dry-run. If you cannot run a live dry-run, write a plausible decision memo for your feature as it stands today and note "self-simulated" at the top.

## Requirements

- All four artefacts are produced for the same feature named in Artefact 1. No switching features mid-exercise.
- Every gate section has concrete numbers or specific named artefacts, not generic placeholders. Where a value is genuinely unknown, use `[placeholder: measure before quoting]` and name the action that will produce the value.
- Every gate section includes at least one **failure observable** — the specific thing the team would see if this piece failed in production, and the escalation that would follow.
- The decision memo template fits on one page in markdown.
- The dry-run vote or self-simulated memo names a real pass / fail call per section — no section is "deferred" without a specific reason and a re-vote date.
- Where safety or policy guidance applies, cite the specific guidance (e.g., OWASP LLM Top 10 category, NIST GenAI Profile risk category) rather than hand-waving.

## Starter guidance

- Begin with Artefact 3 (the MVP → GA gate) only if your feature is already in MVP. Otherwise start with Artefact 2 (prototype → MVP), because the GA gate reuses many of the MVP commitments and this ordering prevents double work.
- The "abort rule" for cost is the piece most teams skip and most leadership teams later ask about. Write it explicitly — a specific dollar or percentage threshold, plus the specific signal, plus the specific action — even if your organisation does not currently enforce it.
- The hardest section is usually the SLO with error-budget policy. Treat it as an engineering exercise, not a theatre exercise: the SLO is the mechanism that keeps the feature healthy once you move on to the next thing. Reuse the SLI vocabulary from Google's SRE Book or Workbook.
- If the feature is not safety-sensitive (e.g., an internal tool with no external users), the safety sections still apply, but the acceptance thresholds are lighter. Do not delete the sections; mark them "scope: internal; reduced posture per policy X."
- For the dry-run vote, pick a colleague who will actually disagree with you. A dry-run in which every section passes on the first read is a signal the exercise is pattern-matching rather than probing.

## Acceptance criteria

- Feature charter present, one paragraph.
- Prototype → MVP gate: five sections, each with concrete numbers, named artefacts, and a named failure observable.
- MVP → GA gate: eight sections, each with concrete numbers, named artefacts, and a dated commitment if the item is not yet in place.
- Promotion-vote agenda: thirty minutes, named participants, time-boxed per section.
- Decision memo template: one page, all sections filled by the dry-run or self-simulated vote.
- Every "I don't know" is marked `[placeholder: measure before quoting]` with the named action that will produce the value.
- A peer AI Engineering Manager can read your gate criteria and run the next vote on this feature without you in the room.

## Stretch goals

- **Second feature crosswalk.** Fill in the same gate for a second feature with a *different* shape (e.g. a doc-Q&A agent and a coding-assist feature). The two gates should differ in interesting ways — safety floor for the coding assist is about code-exfiltration and malicious prompt execution; safety floor for the doc-Q&A is about content categories and injection via indexed documents. Compare side-by-side.
- **Promotion-vote retrospective.** If your team has already run a prior MVP or GA vote, write a short retrospective: what the gate missed, what the vote caught, what the first week in production revealed. Use the retrospective to tighten the gate for the next feature.
- **Published gate.** Produce a version of the two gates suitable for your team's internal handbook — generic enough to apply to future features, tight enough to be directly fillable. The one-page template is the artefact other managers in your organisation will borrow.
