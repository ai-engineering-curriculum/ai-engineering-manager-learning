# Exercise 03: Interview Loop and Rubric Design

**Estimated effort:** 3 hours · **Output:** one loop schedule + one interviewer-facing rubric pack + one candidate-facing round brief

## Objective

Produce the three artefacts an AI Engineering Manager needs to run a consistent interview loop for the role from exercise 02: a **loop schedule** (which rounds run, in what order, by whom), an **interviewer rubric pack** (one written rubric per AI-specific round, with L20 → L48 anchors), and a **candidate-facing round brief** that tells applicants what each round is testing without giving away the prompts. By the end of this exercise you should be able to hand the pack to a new interviewer and have them run a round the same way the existing team does.

## Prerequisites

- Chapter 03 (`Interview loops for AI engineering roles`) of this module.
- Exercise 01 (role matrix) and exercise 02 (JD). The loop measures the artefacts the JD names.
- Access to realistic materials for at least one AI-specific round — ideally a redacted production trace, an eval report, or a system-design brief from your team.

## Problem statement

Design the loop for the role you wrote the JD for in exercise 02. Produce three artefacts.

### Part A — Loop schedule (45 minutes)

A one-page markdown document listing the rounds, in order, with:

- Round number and name.
- Which round from chapter 03 it is (general SWE, §1, §2, §3, §4, §5, behavioural, or a bespoke round you add with a one-line justification).
- Duration.
- Interviewer role on the team (e.g., "senior-agentic engineer," "eval engineer," "hiring manager").
- The two or three JD bullets it measures (cross-ref the crosswalk from exercise 02).

Constraints:

- **General SWE rounds are present.** At minimum, a phone-screen coding round and one onsite coding round. The loop does not delete them to make room for AI-specific rounds.
- **AI-specific rounds are sized to the shape and level** from exercise 02 (use the §6 table in chapter 03 as the starting point).
- **No round is "the AI round."** Every AI-specific round has a specific name, specific rubric, and specific interviewer.
- **The behavioural / closing round is present** and names the three sanity-check questions from chapter 05 if the role is at L40+.

### Part B — Interviewer rubric pack (90 minutes)

For every AI-specific round in your loop, write a one-page rubric. Each rubric has:

- **Round purpose** — one sentence.
- **Setup** — what the interviewer brings (trace, brief, dataset, problem statement).
- **Candidate task** — three to five numbered steps the interviewer walks the candidate through.
- **Rubric by level** — L20 / L30 / L40 (and L48 if applicable) behaviours. Each level is two to three bullet lines of *behaviours*, not credentials.
- **Anti-patterns for the interviewer** — three specific grading mistakes to avoid in this round.
- **What a strong signal sounds like** — one or two verbatim-style quotes from a passing candidate (synthetic is fine; the point is to concretise the rubric).

The rubric for §3 (eval design) and §4 (non-deterministic debugging) is required at minimum. If your role is agent-shaped, §5 (system design) is required. If your role is L30+, §1 (prompt + trace + judge) and §2 (model selection) are required.

### Part C — Candidate-facing round brief (45 minutes)

A short markdown document — half a page per round — that you (or the recruiter) send to candidates ahead of the loop. For each AI-specific round:

- What the round is testing (one sentence, no rubric reveal).
- What they should bring or prepare (if anything; §1, §3, §4, §5 may ask the candidate to think about a recent system they've shipped).
- What they should *not* worry about (e.g., tool-name trivia, brand-specific vocabulary).
- Roughly how long the round is and who they are speaking with.

This document is the GitLab-handbook move adapted to interviewing. It is **not** the interviewer rubric; it describes the shape of the signal, not the behavioural anchors.

## Requirements

- The loop schedule lists at least one general-SWE round. Loops that delete general-SWE to make room for AI rounds fail the exercise.
- Every AI-specific round in the schedule has a corresponding rubric in part B. No round grades against an unwritten rubric.
- Every rubric has L20 → L40 (or whatever subset applies to the target level) anchors. "Pass / fail" rubrics fail the exercise.
- The anti-patterns section is written for **this team's** real grading drifts — not generic. If you cannot name three team-specific anti-patterns, pull three from chapter 03's lists and own them.
- The candidate-facing brief does not reveal any round's prompt. Transparency is about shape, not content.

## Starter guidance

- Chapter 03 gives you five AI-specific rounds and a round-to-shape mapping table (§6). Start there. Add rounds only when the shape genuinely requires one not in the table (rare).
- The hardest rubric to write is often §4 (non-deterministic debugging) because the scenario has multiple plausible answers. The rubric lives in **reasoning**, **sequencing**, and **separation of mitigation from root cause** — not in "did they get the right answer."
- If a round in your loop does not have an interviewer trained on it, that is a signal: either train an interviewer now (shadow → co-run → run alone), or drop the round. Rounds run by untrained interviewers grade worst.
- Keep the candidate-facing brief kind and specific. "You will be asked to review a trace from a real feature and propose a fix" is kind. "You will be judged on your AI judgment" is cruel and unhelpful.

## Acceptance criteria

- Loop schedule has general SWE + AI-specific rounds sized to the role; each round has a named interviewer role and the JD bullets it measures.
- Rubric pack has one written rubric per AI-specific round, with level anchors and anti-patterns.
- Candidate brief exists and never reveals a round's prompt.
- A peer AI Engineering Manager can read the pack and run the loop against your team's role the same way you would.
- The pack is version-dated. Chapter 04 will ask you to iterate on it after the first rolling calibration.

## Stretch goals

- **Run a dry-run of one round** with a willing senior engineer playing the candidate. Record the debrief. The first round you run cold almost always reveals a rubric gap; capture it.
- **Write the calibration-conversion** from your rubrics: how a debrief conversation moves from "they did X" to "that places them at L30 against bullet Y." This is the forward link to exercise 04.
- **Add one deliberately out-of-distribution round** — e.g., a 15-minute conversation about a model-provider DPA clause. Grade whether the candidate engages or defers; the signal is often clearer than the rounds that look like coding interviews.
- **Produce a round rotation plan** so interviewers do not grade the same round forever. Interviewers who have run §4 twenty times develop personal shortcuts that drift from the published rubric; rotation fights that drift.
