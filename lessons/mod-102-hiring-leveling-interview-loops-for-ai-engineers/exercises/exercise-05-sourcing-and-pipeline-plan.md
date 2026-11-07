# Exercise 05: Sourcing and Pipeline Plan

**Estimated effort:** 2 hours · **Output:** one four-source pipeline plan + one recruiter brief + one superstar-trap guardrail

## Objective

Produce the three artefacts that keep the top of the funnel healthy for a new AI engineering team: a **four-source pipeline plan** (adjacent SWE, ML-adjacent, research-to-production, agent/LLM community) with a quarterly target per source; a **recruiter brief** that enforces artefact-based screening at the top of the funnel; and a **superstar-trap guardrail** — the written operating rule and sanity-check questions that prevent the most expensive AI-engineering hiring mistake.

## Prerequisites

- Chapter 05 (`Sourcing, pipeline, and the superstar trap`) of this module.
- Exercises 01 (role matrix), 02 (JD), and 03 (loop). The pipeline sources candidates against a specific shape, level, and loop; the plan does not float.
- A realistic hiring plan — real or constructed — for the next one or two quarters with at least four open roles.

## Problem statement

Produce three artefacts for the four-role hiring plan.

### Part A — Four-source pipeline plan (60 minutes)

A one-page markdown plan with the following structure:

**Section 1 — Roles and target source mix.** A table:

| Role (shape · level) | Primary source | Secondary source | Explicitly not-this-source |
|---|---|---|---|
| applied-ai-engineer L30 | adjacent SWE | ML-adjacent conversion | pure-research |
| … | … | … | … |

For each role, the primary and secondary source must be different from chapter 05's four. The not-this-source column names a source you deliberately *do not* target — and the one-line reason (e.g., "pure research for this applied role will miscast; route to a different opening when seen").

**Section 2 — Per-source sourcing moves.** Four short subsections (one per chapter 05 source), each naming:

- Two or three specific targeting moves (specific communities, specific company types, specific internal channels).
- The signal in a résumé that would make you actively reach out.
- The signal in a résumé from this source that is a **false positive** (e.g., for ML-adjacent: "primary scope is still training classical models on labelled data; no LLM shipping — route to the ML Engineering ladder instead").

**Section 3 — Quarterly targets.** For the next quarter, the volume target per source. Not candidates; **loops started**. Example shape:

| Source | Loops started in Q | Share of total |
|---|---|---|
| adjacent SWE | 8 | 40% |
| ML-adjacent | 4 | 20% |
| research-to-production | 3 | 15% |
| agent/LLM community | 5 | 25% |
| **total** | **20** | **100%** |

No source exceeds roughly half of total loops started; no source is at zero. If your plan has one of those failure modes, write a one-line note on why and a plan for next quarter.

**Section 4 — First-week pipeline check.** What you look at on Friday of week 1 to catch a source that is already imbalanced.

### Part B — Recruiter brief (45 minutes)

A half-page brief for the recruiter handling the openings. It must include:

- **The role matrix.** Attach or reference (link) exercise 01's matrix. The recruiter cannot screen without it.
- **Artefact signals per role.** For each open role, the top three résumé signals the recruiter looks for (not the JD bullets — the shorter, pattern-matching version a recruiter can skim). Specific to your team context.
- **Anti-filter rules.** Explicitly:
  - "Do not filter on `AI Engineer` job title."
  - "Do not filter on years of experience."
  - "Do not filter on degree unless the role explicitly requires one (it does not)."
- **Source-specific calibration.** One line per source on how résumés from that source read: adjacent SWE (artefacts are production shipping, less vocabulary); ML-adjacent (artefacts are pipelines and offline evals, conversion-to-LLM signal matters); research-to-production (artefacts are publications and systems, conversion-to-production signal matters); agent/LLM community (artefacts are contributions and public writing, production-muscle signal matters).
- **The rejected-at-screen review cadence.** Chapter 05 recommends a weekly scan of the rejected pile. Name when it happens and who looks.

### Part C — Superstar-trap guardrail (15 minutes)

Write the one-paragraph **operating rule** your team will adopt on hiring at staff-plus and senior altitudes, grounded in chapter 05. It must contain, in writing:

- The rule: every AI engineering hire, at every level including staff-plus, works the eval and incident surface as part of the job.
- The three sanity-check questions:
  1. Has this candidate ever led an incident to postmortem, not just designed a system that had one?
  2. Has this candidate ever written or defended a judge?
  3. Will this candidate carry an on-call rotation, scoped to their altitude?
- Where the guardrail lives: in the JD's "what you'll own" section at L40+, in the loop's rubric for the senior-shaped rounds (§4, §5), and as the final question in the hiring-manager + closing round.
- The one-line escalation: if a staff-plus candidate declines any of the three in the offer conversation, the role is not a fit, regardless of brand.

Keep this to one paragraph. It is load-bearing precisely because it is short enough to recite.

## Requirements

- The pipeline plan covers all four sources from chapter 05, with none at zero and none above roughly 50% of loops started.
- Each source has at least two specific targeting moves and a named false-positive signal.
- The recruiter brief explicitly deletes `AI Engineer` title filtering, years-of-experience filtering, and degree filtering.
- The superstar guardrail is a paragraph that fits on a Slack message and includes all three sanity-check questions verbatim.
- All three artefacts reference concrete roles from your exercise 01 matrix and your exercise 02 JDs. Nothing floats in the abstract.

## Starter guidance

- The hardest row in the Part A table is usually the research-to-production row. If you cannot name a real source for research-to-production conversion candidates in your context, write that down; chapter 05 is explicit that this source is small and high-variance. Honest "we don't yet have a strong source here, this quarter we build one" is better than fictional targeting.
- The false-positive signals in Part A Section 2 matter more than the targeting moves. Most pipeline plans succeed at generating candidates and fail at screening them honestly against artefacts. Being able to say "this résumé looks right for the source but actually routes to a different role" is the discipline.
- The recruiter brief is only useful if it is **read**. Write it as something a recruiter can skim in two minutes and carry into every intake call this week.
- The superstar-trap guardrail is the one artefact in this exercise that you may, under pressure, feel tempted to soften. Don't. Its value is that it is specific and binding. "We consider operational muscle important" is not a guardrail; the three questions are.

## Acceptance criteria

- Pipeline plan has roles × sources table, four per-source sections, quarterly targets, and the first-week check.
- Recruiter brief has role matrix link, three artefact signals per role, explicit anti-filter rules, four source-specific calibration lines, and a rejected-at-screen cadence.
- Superstar-trap guardrail is one paragraph, includes the operating rule and the three sanity-check questions, and names where the guardrail lives in the JD / loop / closing round.
- A peer AI Engineering Manager can read the three artefacts and execute the hiring plan the same way you would.
- Your version does not promise any source volume that you cannot credibly deliver in the quarter.

## Stretch goals

- **Run the first-week check for real** (if you are in a live hiring quarter). The exercise of actually pulling the pipeline on Friday of week 1 reveals which source is already lagging, and the fix in week 2 is cheap. Waiting until week 4 is three times more expensive.
- **Write a rejected-at-screen audit** after the first week: look at 20 rejected résumés and ask whether each rejection was against the artefact bar or the title bar. Chapter 05 names this as a weekly habit; this is the first instance of it.
- **Produce a mobility-plus-referrals funnel** as a cross-cut: for each source, name the specific channels (internal mobility requests, specific referral asks). Chapter 05 is explicit that referrals are a distribution channel, not a fifth source — write the discipline into your plan so it does not drift.
- **Author the offer-conversation script for staff-plus hires** that explicitly asks the three sanity-check questions. The questions on paper are easier than the questions in a live offer call; drafting the script is practice.
