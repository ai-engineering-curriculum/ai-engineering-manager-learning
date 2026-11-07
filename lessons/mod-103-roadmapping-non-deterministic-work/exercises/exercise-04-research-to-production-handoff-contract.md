# Exercise 04: Research-to-Production Handoff Contract

**Estimated effort:** 3 hours · **Output:** a filled-in R16 acceptance gate for one specific research artefact, a decommissioning path for one specific stalled track, and the paired acceptance memo and 90-day retrospective agenda

## Objective

Design the research-to-production handoff from chapter 04 for **one specific research artefact** that your team will plausibly accept into production, and the decommissioning path for **one specific research track** that is a plausible candidate to be killed or parked. The exercise produces a filled-in acceptance gate across all four dimensions (reproducibility, eval scope, documentation, on-call readiness), the owner-of-record transition memo, the decommissioning memo, and the 90-day retrospective agenda you would run with the research side after acceptance.

## Prerequisites

- Chapter 04 (`Designing the research-to-production handoff (R16)`).
- Chapter 02 — the acceptance vote mirrors the promotion vote; reuse the chairing structure.
- Two contexts, strongest first:
  - An actual research artefact being handed to your team (or that you were handed in a prior role), plus an actual stalled track. Full fidelity.
  - A plausible hypothetical artefact and a plausible hypothetical track, grounded in your team's charter. Use `[placeholder: measure before quoting]` where realism is impossible, and mark the artefact and track as hypothetical at the top of each.
- Enough research-side context (even for a hypothetical) to answer: who built the artefact, what the research team's benchmark looked like, what the research team's current support posture is.

## Problem statement

Produce the following five artefacts.

### Artefact 1 — Artefact and track summaries (20 minutes)

Two short paragraphs.

- **Research artefact** to be accepted. One paragraph: what it is (a model, a fine-tune, a judge, a retrieval approach, a tool-use pattern, a safety classifier), who built it, the headline result from the research team, and why your engineering team is considering accepting it now. Name the posture (research in-house, in-house with external collaboration, or external frontier-lab artefact).
- **Research track** to be decommissioned. One paragraph: what the track was chartered to do, what budget was spent, what signal has or has not emerged, and why it is a plausible candidate for decommissioning this quarter. Name whether the recommendation is lean-toward "extend", "park", or "kill" — and your current read, not the final decision.

### Artefact 2 — Acceptance gate for the research artefact (60 minutes)

The four-dimension gate from chapter 04, filled in for the artefact in Artefact 1. One section per dimension. Each section is one or two paragraphs with concrete named artefacts.

1. **Reproducibility.** State the one-line reproduction command (or the plan to produce it, with named owner and date). Name the hardware the engineering team will reproduce on. State the tolerance — how close reproduced numbers must be to research headline numbers to clear the gate. Name the dependency-pinning posture (frozen requirements, container, nix, whichever). If the artefact is a fine-tune, name the checkpoint hosting location and the training-recipe doc.
2. **Eval scope.** Name the research team's primary benchmark and the headline result. Name the engineering team's registered eval suite the artefact will additionally be evaluated against, and the acceptance threshold. Name the production-distribution eval set (or the commitment to produce one before acceptance, with a date). State the known weak spots the research team has disclosed; if none are disclosed, flag that as a gate concern.
3. **Documentation.** Reference the model card or system card (Mitchell et al. 2019 — see resources). List the required sections: intended use, training / evaluation data provenance, known limitations, safety considerations, dependencies, suggested usage and anti-patterns. Confirm each section is present; if any is missing, name the dated commitment to produce it.
4. **On-call readiness.** Name the first-90-days support agreement: the named individual on the research side committed to secondary-tier pager coverage, the response-time expectation, the escalation path. State the decommissioning path for when the artefact needs to be pulled from production. Name the day-90 retrospective date.

Write the acceptance vote agenda — thirty minutes, chaired by you, with named participants (architect / principal MLE, research representative, governance peer if safety-relevant, product partner if user-facing). Allocate minutes per dimension; reserve a slot for the pass / fail call per dimension and the overall decision.

### Artefact 3 — Owner-of-record transition memo (30 minutes)

A one-page markdown memo documenting the transition from research ownership to engineering ownership, assuming the acceptance vote passes. Sections:

- **Artefact** — name and version at acceptance.
- **Before acceptance** — one paragraph on how the artefact was owned pre-vote.
- **At acceptance** — the named engineering team owner-of-record (single human, not a team), the source-of-truth repo change (transfer or fork), the version pin, the audit-log posture, and the artefact registry entry.
- **After acceptance** — the research team's ongoing role: PR contributions into the engineering repo under the engineering team's review process, stewardship of new variants, co-ownership of the 90-day retrospective.
- **Support agreement** — a short restatement of the on-call commitment from Artefact 2.
- **Decommissioning path** — the condition under which the artefact is rolled back to the prior production state and the vote is re-opened.

Keep the memo to one page. The audience is the research team, your team, and your leadership — each should be able to read it and know what changes.

### Artefact 4 — Decommissioning memo for the stalled track (30 minutes)

A one-paragraph decommissioning memo for the stalled track in Artefact 1. Sections:

- **Track** — name.
- **Charter** — one sentence on what the track was chartered to do.
- **Budget spent** — capacity, calendar, or both.
- **Signal produced** — what the track learned (positive or negative) that would be useful to carry forward.
- **Decision** — extend, park, or kill. If extend, name the new budget and the dated review. If park, name where the artefacts and findings are archived and who owns the archive. If kill, name whether the work is permanently closed or available to be re-chartered under new conditions (and what those conditions are).
- **Co-decision** — the engineering manager (you) and the research lead (named). If the CAIO or equivalent was required to arbitrate, name the decision and date.

Then, write the one-paragraph note that will go to the research team. The note is short, clear, and respectful. It cites the signal produced and the decision; it does not litigate the quarters that led here.

### Artefact 5 — 90-day retrospective agenda (20 minutes)

Write a thirty-to-sixty-minute agenda for the 90-day retrospective that will run after acceptance. Participants named: you, the artefact's owner-of-record, the research representative who committed to on-call, the architect or principal MLE, and optionally the product partner if the artefact is on a user-facing feature.

Agenda sections, time-allocated:

- **Production behaviour read** — the eval numbers in production vs. the acceptance-gate expectation.
- **Support experience read** — incidents during the first 90 days, response-time against the agreement, pager volume.
- **Weak-spot reconciliation** — the weak spots the research team disclosed at acceptance vs. the ones that actually bit in production.
- **Owner-of-record continuation** — does the single-owner-of-record continue, change, or spread? Does the research-side on-call extend, taper, or end?
- **Research-track implications** — what the research team should learn from the production experience for the next artefact.

## Requirements

- All five artefacts are produced for the same artefact / track context named in Artefact 1. No switching mid-exercise.
- Every gate section cites concrete artefacts or thresholds, not generic language.
- The model-card reference uses the Mitchell et al. (2019) framing; the resources file cites the arXiv preprint.
- The owner-of-record transition memo names one human, not a team. A memo that names a team fails the acceptance criteria.
- The decommissioning memo has a decision (extend / park / kill) with a dated follow-up where applicable. A memo without a decision is a status update, not a decommissioning artefact.
- The 90-day retrospective agenda includes the weak-spot reconciliation section — the single most important outcome of a retrospective and the one most often skipped.
- Where safety considerations apply, cite the specific safety-eval suite or guardrail layer rather than hand-waving.

## Starter guidance

- The hardest piece of the acceptance gate is almost always the on-call readiness item. Most research teams have not committed to pager coverage before; the first conversation is rarely comfortable. Draft the agreement in writing before the conversation and bring it to the acceptance vote rather than negotiating in the room.
- For external frontier-lab artefacts, the "reproducibility" item is not about running the model locally — it is about reproducing the evaluation numbers through the vendor's API from a specific SHA of your harness. Write the gate that way.
- For the decommissioning memo, the hardest tone to get right is respect for the work the research team produced without softening the decision. Writing "the track produced no signal that justified further investment" without hedging is often the right call; writing the same with four paragraphs of apology is usually not.
- The 90-day retrospective is where the acceptance gate pays back. If the retrospective reveals weak spots the gate missed, tighten the gate for the next artefact. If it reveals the gate was too tight, loosen a dimension — the goal is a gate the research side respects, not a gate that only lets artefacts through once a year.
- Where you must mark an artefact or track as hypothetical, do so visibly at the top of the file. Realism matters; faux realism is worse than honest hypotheticals.

## Acceptance criteria

- Two one-paragraph summaries (artefact + track) with posture and current lean.
- Acceptance gate: four dimensions, each with concrete named artefacts and thresholds, plus a thirty-minute vote agenda with named participants.
- Owner-of-record transition memo: one page, naming one human as owner-of-record, with all transition sections filled.
- Decommissioning memo: one paragraph with a decision (extend / park / kill) and a dated follow-up where applicable, plus the short note to the research team.
- 90-day retrospective agenda: time-allocated, five sections, with the weak-spot reconciliation section present.
- Any hypothetical artefacts or tracks are clearly labelled as such at the top of the file.
- A peer AI Engineering Manager can read the acceptance gate and run the vote on this artefact without you in the room; a peer can read the decommissioning memo and understand what is being closed and why.

## Stretch goals

- **Research-side partner review.** If the research artefact is real, share the acceptance gate with the research-side counterpart before the vote. Record the pushback and the adjustments. The first version of an acceptance gate almost always over-indexes on one dimension (usually reproducibility) and under-indexes on another (usually documentation). Research-side feedback catches it.
- **Published R16 contract.** Produce the generic version of the acceptance gate — the artefact your team's handbook will reference for every future research-to-production handoff. Keep it to two pages. The generic version will be borrowed by other managers in your organisation.
- **Dormancy audit.** List every research track in your organisation's orbit that has not produced a handoff in the last two quarters. For each, write the one-sentence read: extend, park, kill. Share with the research lead or CAIO. Most organisations have more stalled research than they realise; the audit is the start of the conversation.
