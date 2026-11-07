# Exercise 04: Run a Mock Calibration

**Estimated effort:** 3 hours · **Output:** one per-candidate debrief + one rolling-calibration note + one rubric correction

## Objective

Walk through all three cadences of calibration — per-candidate debrief, rolling pipeline review, and rubric correction — on a set of mock loop outcomes. By the end of this exercise you should have the muscle memory to (a) drive a debrief from vibe to artefact, (b) spot drift across a batch of recent offers, and (c) convert the drift into a published rubric update — all without introducing a secret internal leveling language.

## Prerequisites

- Chapter 04 (`Calibration across the AI engineering ladder`) of this module.
- Exercise 03 (interview loop and rubric pack). Calibration is grading against *your* rubric, not a generic one.
- Optional but recommended: two or three willing peers (fellow AI Engineering Managers, senior ICs, or hiring partners) to play interviewer roles in part A.

## Problem statement

Produce three artefacts in sequence, each feeding the next.

### Part A — Per-candidate debrief, from vibe to artefact (90 minutes)

Pick three fictional (or lightly anonymised real) candidate loop outcomes against the role you specified in exercise 02. For each, you will be given — or will construct — a set of **round summaries**: what each interviewer saw and claimed. Your job is to run the debrief.

Construct the three candidates using the following rough shapes so each produces a different calibration conversation:

- **Candidate A — "strong, at level."** Four of five AI-specific rounds come back as "at level" with specific artefact evidence; one round ("below") is from an interviewer who graded against an unpublished expectation. The calibration work is to catch and strip the unpublished expectation, keep the artefact evidence, and decide at level with confidence.
- **Candidate B — "the superstar."** Prominent public profile. §5 system design came back glowing. §3 eval design and §4 non-deterministic debugging came back light — the interviewers softened their rubrics because "obviously they know AI." The calibration work is to apply the chapter 05 superstar rule and the three sanity-check questions.
- **Candidate C — "the shape mismatch."** Loop was run as applied-AI engineer at L30, but the candidate's artefact portfolio across the rounds reads cleanly as eval engineer at L30. The calibration work is to place shape *and* level honestly, which may mean a different offer than the one the recruiter pre-wrote.

For each of the three candidates, produce a one-page debrief document with:

- The round summaries (as given or constructed).
- The leveling grid — five artefact classes × below/at/above for the target level.
- The shape placement — the row in your chapter 01 matrix, with justification.
- The debrief move you ran — specifically, the moment where you asked *"what evidence is that?"* and what changed in the room.
- The final call — offer or no offer, at what level, at what shape, with what ramp.

Mark in the margin every time you had to **strip secret-language shorthand** from the room ("L3.5-ish," "senior-but-not-staff," "classic L4"). Chapter 04's rule is that the published rubric is the only language the team uses.

### Part B — Rolling-calibration note (60 minutes)

Pretend the three debriefs from Part A are the three most recent of the last ten loops this quarter. Pull the ten loops (invent the rest with enough variety to make the exercise honest) and run the rolling-calibration meeting as a written artefact. Produce:

- A table of the ten offers: shape, level, source (from chapter 05), artefact-class gaps the debrief flagged.
- A one-paragraph read of what the pipeline looks like in aggregate — are the four sources balanced; is any shape over-indexed; is any level drifting.
- A **named drift** from the four patterns in chapter 04 (shape drift, level inflation, round over-weighting, secret-language drift). You must find at least one; a quarter with zero drift is a quarter you have not calibrated.
- A **correction** — what you will publish in the next day: an updated rubric line, a role-matrix clarification, a recruiter-brief adjustment, or a loop change (an under-weighted round getting more weight; a round that is grading inconsistently getting a rubric pass).

### Part C — Published rubric correction (30 minutes)

Write the **actual rubric update** that comes out of Part B. This is the artefact your team and future candidates will read.

- A short preamble: what we observed, what we are changing, why.
- The specific lines in the rubric that change, shown as `before` / `after`.
- A dated version header on the rubric so the audit trail is visible.
- A note on how the loop will enforce the update in the next round (new interviewer training, a rubric re-read in the next debrief, a published candidate-brief update).

This closes the loop: calibration found a drift, the team published a correction, future loops and candidates will see it. No internal shorthand survives.

## Requirements

- Three per-candidate debriefs in Part A; each has a leveling grid, a shape placement, a named "what evidence is that?" moment, and a final call.
- The superstar debrief explicitly applies the three sanity-check questions from chapter 05 and names the operating rule.
- The shape-mismatch debrief changes the originally-assumed shape and defends the change with artefacts.
- The rolling-calibration note names at least one drift from the four chapter 04 patterns and proposes a specific correction.
- The rubric correction in Part C is *written* — not a plan to write one later. A dated, visible update.

## Starter guidance

- The debrief move you are practising is almost mechanical: hear a level claim, ask "what evidence is that?" and refuse to move on until an artefact is on the table. Do it five times in one debrief and the room learns the pattern.
- The superstar candidate is the hardest one to debrief honestly. Pressure to close them runs hot. The chapter 05 sanity-check questions are your backstop — if the candidate cannot answer "will you carry an on-call rotation at this altitude" cleanly, no amount of §5 brilliance makes the offer safe.
- The shape-mismatch candidate is the second-hardest, because it means admitting the loop ran against the wrong rubric. The recovery is honest — "we loaded this as applied, but the artefact portfolio is eval; here is an eval-shape offer or a decline, not a mis-placed applied offer."
- Keep the published rubric correction small. A one-line change that *actually lands* beats a 40-line rewrite that nobody reads. Iterate.

## Acceptance criteria

- Three debriefs, each one page, each with the leveling grid, shape placement, "what evidence is that?" moment, and final call.
- A rolling-calibration note with ten-loop table, aggregate read, one named drift, and one proposed correction.
- A published rubric correction with preamble, before/after, dated version, and enforcement note.
- No internal shorthand survives into the written artefacts. The published rubric is the only language used.
- A peer AI Engineering Manager can read your pack and run the same debrief conversation in their own loop.

## Stretch goals

- Run the debriefs **out loud** with real colleagues playing the interviewer roles. The written exercise catches most of the muscle; the live exercise catches the rest.
- Produce a **four-quarter rolling-calibration log** showing how one specific drift evolved — e.g., "L30 eval bar drifted up over Q2, caught in Q3 rolling calibration, corrected in Q3 published rubric, two candidates re-offered at level on appeal in Q4."
- Add a **candidate-appeal mechanism** to the rubric correction: when a correction changes a level bar, define how a recently-made offer gets re-reviewed if the candidate or the manager flags it. The absence of an appeal path is where secret-language drift returns.
- Extend the exercise up to staff-plus: run a mock calibration for an L48 `agentic-systems-architect` offer. The artefact posture at L48 is cross-team; the calibration conversation changes. Chapter 04's anchor table is the starting point.
