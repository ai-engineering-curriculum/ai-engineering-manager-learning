# Exercise 02: JD for One Concrete Opening

**Estimated effort:** 2 hours · **Output:** one single-page JD built from artefacts

## Objective

Produce a **real, postable JD** for one concrete open AI-engineering role, using the five-artefact-class template from chapter 02. By the end of this exercise you should have a JD that (a) a candidate can read in two minutes and know whether to apply, (b) your interview loop (exercise 03) can measure against, and (c) your recruiter can screen on without re-inserting the years-of-experience and PhD clauses you deleted.

## Prerequisites

- Chapter 02 (`Writing JDs from artefacts, not years of experience`) of this module.
- Exercise 01 (role matrix and placement rule). You cannot write a JD for a role you have not yet placed on the matrix.
- Access to a real or realistic team context — your own, your employer's, or a well-documented public one. Fabricated JDs without a team context grade poorly on this exercise.

## Problem statement

Pick one shape-and-level pairing from exercise 01's matrix. Produce a single-page markdown JD using the five-section template from chapter 02:

1. **Who you are** — one paragraph, plain English, primary artefact named.
2. **What you'll own in year one** — exactly five bullets, one per artefact class: traces reviewed, eval suites, prompts/judges registered, incidents led, mentorship scope.
3. **What a strong first 90 days looks like** — three bullets, specific.
4. **Who you work with** — one paragraph pointing at the handoff contracts.
5. **What we do NOT expect** — three bullets. Years, pedigree, and "nice-to-have" pile that you explicitly delete.

### Part A — JD draft (60 minutes)

Write the JD. Target length: one page in markdown that prints onto roughly one A4 / Letter page. Longer than that and the JD is doing the IC track's job, not yours.

Constraints that must be visible in the draft:

- **No years-of-experience clause.** Anywhere.
- **No "PhD or equivalent" clause.** Unless you can defend why and the role is research-adjacent (which is unusual for this track).
- **No fluff adjectives.** "Rockstar," "10x," "passionate about AI," and equivalents are banned.
- **Every artefact bullet names a nameable artefact** — a trace, a registered prompt, a judge-case corpus, an eval-fixture PR, an incident postmortem, a kill-switch document.
- **The "who you work with" paragraph** references at least one handoff from mod-101 chapter 03 (down to the IC ladder you manage, sideways to a governance or infra peer, or up to a staff-plus IC).

### Part B — Rubric crosswalk (30 minutes)

Produce a two-column table that maps every bullet in the JD to the interview round (chapter 03) that will measure it. Every artefact bullet should map to at least one round. If any bullet maps to zero rounds, either delete the bullet or add a round to the loop (exercise 03).

| JD bullet (abbreviated) | Round(s) that measure it |
|---|---|
| "Add two permanent eval fixtures from bugs" | §3 eval-design, §4 non-det debugging |
| "Register prompts with trace + eval delta on PR" | §1 prompt+trace+judge review |
| … | … |

This crosswalk is the single most important forcing function in the exercise: a JD whose bullets no round measures is a JD that offers go out against unwritten rubrics.

### Part C — Recruiter screening brief (30 minutes)

Write a short (half-page) screening brief for the recruiter handling this role. The brief must include:

- **Three artefact-level signals** the recruiter should look for in the résumé (specific things, not "AI experience").
- **Two signals that should NOT filter out the candidate** — e.g., "no 'AI Engineer' job title; evaluate the artefact bullets in the résumé regardless."
- **One disqualifier** — a specific thing in the résumé that genuinely routes to a different role (e.g., "primary experience is model serving / GPU capacity; route to `ai-infra-team-lead`").
- The three sanity-check questions for staff-plus candidates from chapter 05 (led an incident, written a judge, will carry on-call), if the role is at L40+.

## Requirements

- The JD is a **single page** in markdown. Longer than that is a sign the JD is working at the wrong altitude.
- Every one of the five sections is filled; no section is skipped.
- The rubric crosswalk covers **every artefact bullet** in the JD. No orphan bullets.
- The recruiter brief names the sources from chapter 05 — adjacent SWE, ML-adjacent, research-to-production, agent/LLM community — and says which this role weights most heavily.
- The JD's "what we do NOT expect" section explicitly deletes years-of-experience, PhD-required, and fluff phrasing. Not implicitly — in writing.

## Starter guidance

- The worked examples in chapter 02 (applied-AI engineer at L30, eval engineer at L30) are **patterns**, not templates to fill in. Write for *your* team's artefacts, codebase, product, and partners.
- The hardest bullet is usually the **mentorship-scope** bullet. L20s have none; L30s pair with L20s informally; L40s mentor formally across shapes. Specificity matters — "pairs weekly with two L20 engineers on prompt-registry hygiene" beats "mentors junior engineers."
- The "what a strong first 90 days looks like" section is where the JD becomes real to the candidate. Each bullet should be something the first-quarter check-in actually asks about, so the hire and the manager are measuring against the same artefact.
- If you are writing for a staff-plus role, read chapter 05's superstar-trap section before you draft. The JD is one of the three places (JD, loop, offer conversation) where the operating rule — every hire works the eval and incident surface — gets written down.

## Acceptance criteria

- The JD fits on one page in markdown; a reader can tell in two minutes what they would own.
- Every artefact bullet in the JD has at least one round in the crosswalk that measures it.
- The recruiter brief names three artefact signals, two anti-filter signals, one disqualifier, and (for L40+) the three sanity-check questions.
- A peer AI Engineering Manager can read the JD and name the shape and level without being told.
- No years-of-experience clause, no PhD clause, no fluff adjectives.

## Stretch goals

- Write a **second JD for an adjacent shape at the same level** (e.g., applied-AI engineer L30 and RAG engineer L30). The exercise of writing two JDs with the same template forces the artefact bullets to actually differentiate.
- Write a **third JD for the same shape one level up** (e.g., agentic-AI engineer L30 and senior-agentic-AI engineer L40). The differentiation lives entirely in the artefact bullets — scope of traces, eval-commissioning depth, registry conventions ownership, incident command, mentorship formality.
- Produce the JD in your team's **house markdown style**: make it postable without reformatting. If your team does not have a house style, this is a signal to start one; the GitLab-handbook-style discipline (chapter 02, chapter 04) makes future JDs cheap to produce.
