# Exercise 03: Backlog, Bug, and Experiment Triage Cadence

**Estimated effort:** 2 hours · **Output:** a written triage-cadence document for one team covering the regression-by-cause taxonomy, the triage meeting's shape, the experiment-work sizing contract, and the capacity line.

## Objective

Produce the third manager-owned artefact of this module: the written, published triage cadence specified in chapter 04 — the regression-by-cause taxonomy visible on every open bug, the triage meeting's shape, the experiment-work sizing contract, and the team-level experiment capacity line.

A peer manager reading your triage document should be able to walk up to any bug in your queue and tell its cause tag, its first-line owner, and whether it is blocking an experiment, without asking you.

## Prerequisites

- Chapter 04 of this module for the regression-by-cause taxonomy, the triage meeting's shape, the experiment-work sizing contract, and the three failure shapes classical triage produces on this surface.
- The outputs of exercises 01 (shape decision) and 02 (ritual set) — the triage cadence reads against the shape and the ritual set.
- Mod-103 (roadmapping non-deterministic work) for the confidence-banded planning the experiment sizing contract reads against.
- Mod-104 (eval-driven SDLC) for the registry the bug-to-fixture and incident-to-fixture loops land in.
- Mod-107 (AI incident response) for the incident taxonomy the replay ritual uses.

## Problem statement

A published triage-cadence document, in markdown, in your team's engineering handbook. The document has five parts.

### Part A — the regression-by-cause taxonomy (30 minutes)

A section defining the taxonomy as it applies to your team. For each of the five causes (prompt, judge, retrieval, tool, model-vendor):

- **The definition on your surface.** What specifically does a "prompt regression" look like on your team's product? For a RAG-heavy team, the retrieval-vs-prompt distinction is sharper; for an agentic team, the tool-vs-prompt distinction is critical; for a team without fine-tune, the vendor-regression surface is different.
- **The telling signals** chapter 04 names for the cause, adapted to your team's observable artefacts (your trace format, your judge library, your retrieval logs, your tool contracts, your vendor pins).
- **The first-line owner** per cause, named as a role on your shape (from exercise 01). If your team is a horizontal eval capability team, the first-line owners read differently than for a vertical pod.
- **The common anti-pattern** on this cause specific to your team's recent history. If your team's bug corpus has a repeated pattern of mis-routed bugs on one cause, name it and the fix.
- **Tagging mechanics.** Where the tag lives (Jira custom field, Linear label, GitHub issue label — your team's tooling). How "untagged" is surfaced as a status.

### Part B — the triage meeting's shape (25 minutes)

- **Cadence.** Weekly, biweekly, or as-needed. Justify against your bug and regression rate.
- **Attendees.** Named (role, and if appropriate name). The chapter-04 default is the engineering manager, one senior IC per shape in the mix, the on-call engineer, the PM for severity-and-user-impact context. Adjust for your team.
- **Agenda.** The six-beat agenda chapter 04 names (open from queue, classify, experiment review, backlog re-rank, experiment proposals, close with output). Produce a time-boxed version against your cadence choice; a 45-minute meeting looks different from a 60-minute one.
- **Output.** The dated triage log's location and shape. Who reads it (the on-call engineer who missed the meeting is the primary reader; name secondary readers — the PM, the manager's next-line-manager, the staff-plus IC partner).
- **Interaction with other rituals.** How the triage meeting connects to the chapter-03 rituals: which bugs become fixture candidates for the prompt + eval + trace review, which incidents become candidates for the next replay.

### Part C — the experiment-work sizing contract (25 minutes)

The template a new experiment has to fill before approval. The five fields chapter 04 names.

1. **The question the experiment answers.** With an example from your team's recent work (a prompt A/B, a retrieval bake-off, a judge calibration).
2. **The decision the answer enables.** With the example's decision explicit.
3. **The sizing — engineer-days and calendar-days.** With a hard ceiling. Define what counts as an engineer-day (fully dedicated day, half-day, PR-time-only).
4. **The acceptance criteria.** With the example's criteria: sample size, metric, significance test or decision threshold.
5. **The exit conditions.** With the example's exit conditions: early-stop and ceiling.

Include a worked example (filled template) and a template-only version (blank). The worked example is what the next IC proposing an experiment pattern-matches against.

### Part D — the team-level experiment capacity line (15 minutes)

- **The fraction of sprint engineer-days allocated to experiment work.** 10–20% on a product-shipping vertical pod; 30–40% on a specialist research-heavy team. Name the fraction on your team and the sprint length it is being computed against.
- **The enforcement mechanism.** How the triage meeting defends the line. What happens when a proposed experiment would push the team over the line (defer the new experiment, kill a running one that has not converged, or formally re-size with the staff-plus IC and the PM).
- **The re-size cadence.** How often the capacity line is re-opened. Quarterly is a reasonable default; anchor against your shape-decision re-read cadence (exercise 01).

### Part E — the compounding loops (15 minutes)

A short section describing how the three compounding loops chapter 04 names will run on your team.

- **Bug-to-fixture.** Which open bugs in the last quarter produced a registered fixture? What is the target rate (one fixture per N reproducible bugs)?
- **Incident-to-fixture.** Which incidents in the last quarter produced a registered judge update or fixture? If any incident failed to produce an artefact, the explicit documentation of the deferral is named.
- **Experiment-to-decision.** Which approved experiments in the last quarter reached a documented decision? If any ended without a decision, the diagnosis is on the sizing contract (which field was missing or wrong).

If your team is newly formed and does not have a quarter of history, write the target rates and the mechanics for next quarter, and set a date to re-read against actuals.

## Requirements

- The document is a single markdown file in your team's engineering handbook; the submission links to it.
- The regression-by-cause taxonomy has the five causes named with definition on your surface, telling signals, first-line owner, common anti-pattern, and tagging mechanics.
- The triage meeting's shape has cadence, attendees, agenda, output, and interaction with other rituals.
- The experiment-work sizing contract has the five fields with a worked example and a blank template.
- The experiment capacity line has the fraction, the enforcement mechanism, and the re-size cadence.
- The compounding loops section names target rates and the mechanics for each loop.
- The document is behaviour-worded on the taxonomy: definitions read as "if the trace shows X, Y, and Z, the cause is tagged A" rather than aspirational language.
- Length: 1,500–3,000 words (plus the template).

## Starter guidance

- Write Part A (the taxonomy) first, and write it against your team's actual recent bug corpus. Pull the last 15–30 bugs from your tracker and tag each one retroactively. The retroactive tagging surfaces:
  - which causes dominate (shape of the triage meeting's attention budget);
  - which causes have ambiguous first-line owners (gap in exercise 01's shape decision);
  - which bugs were mis-routed under the old classical triage (motivation 1 of chapter 04).
- For Part B, the attendee list is the most common source of friction. A meeting with more than six attendees runs long and produces less decision; a meeting with fewer than three tends to lack the cross-shape perspective. If your team's shape forces more than six attendees on a single triage, split the meeting by subsystem.
- For Part C, resist the urge to make the sizing contract a long template. The five fields are intentionally minimal; a six-field template has one field too many; a four-field template is missing the exit condition, which is the field experiments most reliably skip.
- For Part D, the capacity-line fraction is the most contentious field to publish because it exposes a trade-off that previously lived informally. Publish it anyway. A visible fraction defended in triage meetings is cheaper to operate than an invisible fraction fought over in planning.
- For Part E, if you do not have historical data, set the target rates conservatively (one fixture per 3–5 reproducible bugs is a reasonable starting target; 100% of incidents producing an artefact of some kind — either a fixture or a documented deferral — is the discipline).
- Ask the eval engineer (or the senior IC covering measurement) to read the taxonomy. They are the cause-identification specialist; their correction on the definition of a "judge regression" versus a "prompt regression" is often substantive.
- Ask the on-call engineer and the PM to read the triage-meeting agenda. They attend; their read on the agenda's practical shape and on the output's usefulness is the acceptance test.

## Acceptance criteria

- The document is in your team's engineering handbook, linked from your submission.
- The regression-by-cause taxonomy has all five causes defined on your surface with signals, first-line owner, anti-pattern, and tagging mechanics.
- The triage meeting's shape is complete with cadence, attendees, agenda, output, and ritual interaction.
- The experiment-work sizing contract has a worked example and a blank template.
- The experiment capacity line has a specific fraction, enforcement, and re-size cadence.
- The compounding-loops section names target rates and mechanics.
- A peer manager can walk up to any bug in your queue and tell its cause tag and first-line owner within one minute.
- Length is 1,500–3,000 words plus the template.

## Stretch goals

- **The retroactive tagging exercise.** Take the last 30 bugs in your team's tracker. Tag each with the cause taxonomy. Produce a one-paragraph summary: distribution of causes, number of previously mis-routed bugs, patterns of recurring cause-type. The retroactive tagging is often the most revealing single exercise in this module.
- **The experiment-colonisation audit.** Look back at the last two quarters' experiments. For each, read against the sizing contract retroactively. How many had a written question? A written decision? A written sizing? Acceptance criteria? Exit conditions? The audit produces the number of experiments that would have been reshaped or deferred under the contract — and often reveals the hidden capacity cost.
- **The masked-regression recovery.** If your team has had a regression in the last year that was initially mis-tagged, write a short retrospective: what the true cause was, what cause was initially pursued, how long the mis-routing cost, what signal would have caught the correct cause earlier. The retrospective is a candidate agenda item for the next incident replay.
- **The horizontal-team service contract overlay.** If your team is horizontal and serves multiple vertical pods, the bug and experiment intake has to cross team boundaries. Write how the cause taxonomy and the triage meeting interact with inbound bugs from client pods: what classification happens on their side, what happens on yours, when a bug is escalated back for resolution versus owned on your side.
- **The experiment-to-mod-103-plan connection.** If your team uses mod-103's confidence-banded planning, write how the experiment-to-decision loop (your experiment outputs) feeds the confidence bands (your planning inputs). The connection is where this module's triage cadence lands in next quarter's roadmap.
