# Exercise 02: AI-Specific Ritual Set Design

**Estimated effort:** 2 hours · **Output:** a published ritual-set document for one team covering the four AI-specific rituals, the ritual-cost ledger, the kill-criteria, and the first-quarter audit plan.

## Objective

Produce the second manager-owned artefact of this module: the written, published ritual set specified in chapter 03 — the four AI-specific rituals (model-watch, prompt + eval + trace review, incident replay, vendor-provider release review) sequenced against your team's existing general-management ritual backbone, with a ritual-cost ledger, kill-criteria, and a first-quarter audit plan.

A peer manager reading your ritual document should be able to tell what every recurring meeting on your team's calendar is for, who owns it, what it produces, and under what conditions it would be retired — without asking you.

## Prerequisites

- Chapter 03 of this module for the four AI-specific rituals, the ritual-cost ledger, the kill-criteria, and the quarterly audit discipline.
- The output of exercise 01 (your team-shape decision) — ritual-set design reads against the shape.
- The general-management ritual canon your team already runs (standup, sprint planning or Kanban, retrospective, demo, 1:1 cadence, review cycle, on-call handoff).
- Mod-104 chapter contents (eval registry) for the artefact the prompt + eval + trace review ritual reads on.
- Mod-105 chapter contents (vendor / model portfolio) for the inputs the model-watch and vendor-release review run on.
- Mod-107 chapter contents (incident response and blameless postmortems) for the substrate the incident replay reads on.
- Mod-108 chapter 03 (the 1:1 and review cadence) and chapter 05 (the anti-atrophy reading loop) for the cadence beats this exercise sequences.

## Problem statement

A published ritual-set document, in markdown, in your team's engineering handbook. The document has five parts.

### Part A — the general-management ritual backbone (10 minutes)

A section inventorying the rituals your team *already runs* at the general-management altitude. One line each: name, cadence, duration, owner, output, last-reviewed date (if known). If a ritual on the list has no last-reviewed date, mark it for the first-quarter audit.

### Part B — the four AI-specific rituals (60 minutes)

A section per ritual (four sections), each covering the same six fields chapter 03 names.

- **Purpose.** One or two sentences. What this ritual reads on. Why the general-management backbone does not already cover it.
- **Cadence.** Specific (weekly, biweekly, monthly) and timed against the surface the ritual reads on. Justify the cadence against the surface's change rate.
- **Owner.** Named role (and name, if you are writing for your specific team). If rotation is on, the rotation cadence and the first three rotation holders.
- **Input.** What has to be prepared before the meeting. Who prepares it.
- **Output.** The artefact the ritual produces, including where it is published, who reads it, and how it links to downstream rituals (the trace review's fixture additions land in the registry; the incident replay's judge updates land in the registry; the model-watch's migration decisions land in the vendor-release review's agenda; etc.).
- **Kill-criterion.** The condition under which this ritual would be downgraded in cadence, re-chartered, merged with another ritual, or retired. Specific, measurable, not aspirational.

The four rituals to cover: weekly model-watch; weekly prompt + eval + trace review; (bi)weekly incident replay; monthly vendor-provider release review. If your team's shape or context argues against one of them (a team that genuinely cannot run a model-watch because it defers the portfolio decision to a central function, for example), write the deferral explicitly: who is covering the surface instead, what artefact the team receives from them, and what the team does in lieu.

### Part C — the sequencing against the week (15 minutes)

A calendar or table showing where the AI-specific rituals sit against the general backbone through a representative week. The sequencing notes chapter 03 names:

- Model-watch early in the week, before sprint planning or grooming.
- Prompt + eval + trace review after the model-watch, before the week's shipping peak.
- Incident replay biweekly in healthy periods (adjust as your incident surface demands).
- Vendor-release review monthly, with the staff-plus IC partner present.

Produce the sequencing as a day-by-day grid (Mon–Fri, morning/afternoon). Highlight the focused-work blocks — the time your team is *not* in a meeting. A ritual set that leaves no focused-work time is a design failure and the sequencing should surface it.

### Part D — the ritual-cost ledger (20 minutes)

A table with the columns chapter 03 core concept 4 names: ritual, cadence, duration, headcount, weekly person-hours, last-decision-or-artefact-produced date.

- Fill every row. The weekly person-hours cell is headcount × duration-in-hours × frequency-per-week (monthly cadence = frequency 0.25, biweekly = 0.5).
- The last-decision-or-artefact column is the earning column. If a ritual's cell says "none in the last eight weeks," the ritual is a candidate for the first-quarter audit's kill queue.
- Produce a total line. What percentage of theoretical capacity (40 × headcount) does the ritual set consume? 10–15% is normal; above 20% warrants a fresh audit.

### Part E — the first-quarter audit plan (15 minutes)

A short section describing how the first quarterly ritual audit (90 days from publication) will run.

- **Date scheduled.** A specific calendar date within 100 days of today.
- **Attendees.** Typically the manager, one or two senior ICs, and optionally the staff-plus IC partner.
- **Agenda.** 10 minutes reading the ledger; 30 minutes reviewing the AI-specific rituals against their kill-criteria; 30 minutes reviewing the general-management backbone rituals; 20 minutes writing the changelog (what is retired, downgraded, re-chartered, merged, or added).
- **Output.** A dated changelog published in the handbook. If the audit retires at least one ritual that is not earning its place, name a reasonable target (not a mandate; a healthy calendar may genuinely have zero rot, though it is uncommon).

## Requirements

- The document is a single markdown file in your team's engineering handbook; the exercise submission links to it.
- Every ritual (general-management backbone and AI-specific) is listed with name, cadence, duration, owner, output, and (for the AI-specific) kill-criterion.
- Any AI-specific ritual your team is *not* running has an explicit written deferral naming who covers the surface instead.
- The sequencing is shown day-by-day; focused-work blocks are visible.
- The ritual-cost ledger is complete and totaled; the percentage-of-capacity is computed.
- The first-quarter audit is scheduled on a specific date on the manager's calendar.
- The document is behaviour-worded: a kill-criterion like "if the ritual has produced no registry fixture in six weeks, downgrade to biweekly" is behaviour-worded; "if the ritual feels stale, maybe change it" is not.
- Length: 1,500–3,000 words (not including the ledger table). Longer is a drift signal.

## Starter guidance

- Write Part B first. The four AI-specific rituals are the content load of the document; Parts A, C, D, and E all read against them.
- For each AI-specific ritual, resist the urge to write a generic version. The right output of the model-watch depends on your vendor mix (a team dominated by Anthropic has different watch inputs than one dominated by OpenAI or by multiple providers); the right output of the prompt + eval + trace review depends on your registry's shape (mod-104); the right incident-replay cadence depends on your incident rate; the right vendor-release-review attendance depends on who is in your shape decision (exercise 01).
- The kill-criterion is the hardest field to write and the most important. If you cannot write a specific, measurable condition, the ritual is still vague to you. Rewrite the purpose and the output until the kill-criterion comes into focus; if it never does, the ritual itself may not be well-defined.
- For Part C, bring your calendar app into the exercise. Sequence against actual time constraints — not a Platonic week. The output grid is a design the team can run next Monday.
- For Part D, estimate the last-decision-or-artefact column honestly. If you do not know when a ritual last produced an outlived-the-meeting artefact, that is a finding. Write "unknown — to be determined in Q1 audit."
- For Part E, the scheduled date is the forcing function. A ritual-set document with no audit date is a document that will be ignored in a quarter. Put the date on your calendar before you publish.
- Ask the staff-plus IC partner and one senior IC to read the document before publishing. The sequencing (Part C) is the most common source of practical friction; the senior IC will catch where the calendar collides with their focused-work needs.
- Compare the ledger against your previous role or a peer team's. A ledger that reads substantially under 10% or substantially over 20% of capacity is worth investigating; the common range is 10–15%.

## Acceptance criteria

- The document is in your team's engineering handbook, linked from your submission.
- Every ritual (general backbone and AI-specific) is listed with the chapter-03 six fields where applicable.
- Sequencing is shown with focused-work blocks visible.
- The ritual-cost ledger is complete, totaled, and converted to a percentage of theoretical capacity.
- The first-quarter audit is scheduled on a specific date on the manager's calendar.
- All kill-criteria are behaviour-worded and measurable.
- A peer manager can read the document and tell what every recurring meeting on your calendar is for within five minutes.
- Length is 1,500–3,000 words (excluding the ledger).

## Stretch goals

- **The rituals-that-must-die list.** Apply chapter 03's three default-kill patterns (the weekly status read-out, the no-deliverable research meeting, the empty office hours) to your team's current calendar. Identify at least one ritual that should die; write the retirement note (why, effective when, what replaces it, how the team was notified).
- **The async alternatives inventory.** For each AI-specific ritual, identify at least one input that could be gathered asynchronously (written model-watch notes shared in a channel before the meeting; trace samples pre-annotated; vendor release-review readings circulated the Friday before). The written-first discipline shortens meetings and widens participation.
- **The on-call-friendly adjustment.** How does your ritual set change when the team is in the middle of a sustained incident class? Write a "crisis-mode" variant: which rituals are protected, which are suspended, which are merged. The variant is the artefact the manager reaches for when the incident surface heats up, not something to improvise on day three of an incident.
- **The horizontal-team contract alignment.** If your team is horizontal (chapter 02 alignment), the ritual set probably needs explicit rituals with dependent teams (office hours, roadmap input cycles, cross-team quarterly reviews). Add a Part F covering the dependent-team-facing rituals and their kill-criteria.
- **The cross-pod sync (for a split or dual-pod shape).** If your shape decision names two pods under one manager or two sibling managers, what is the shared ritual between them? Write that ritual's six fields. The shared ritual is where the two pods' measurement discipline, incident surface, and vendor-release reading stay aligned.
