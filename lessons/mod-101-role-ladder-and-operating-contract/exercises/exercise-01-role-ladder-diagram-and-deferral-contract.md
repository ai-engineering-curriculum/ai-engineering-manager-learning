# Exercise 01: Role Ladder Diagram and Deferral Contract

**Estimated effort:** 2 hours · **Output:** one ladder diagram + one deferral-contract page

## Objective

Produce the two artefacts an AI Engineering Manager (level 45) should carry into their first week: a **ladder diagram** that positions your role against the six neighbours covered in chapter 01, and a **deferral contract** that names what crosses each handoff boundary (down, sideways, up, out) from chapter 03. By the end of this exercise you should be able to point at the diagram and the contract and say, in one sentence per role, "this is what they own, this is what I own, this is what crosses the line."

## Prerequisites

- Chapter 01 (`Where this role sits`) and chapter 03 (`Handoff contracts`) of this module.
- The `CURRICULUM.md` and `JOB_REQUIREMENTS.md` ownership rule at the repo root.
- Optional: a real team context (your own, your employer's, or a well-documented public one) to ground the artefact.

## Problem statement

You are joining (or being promoted into) an AI Engineering Manager role at level 45 and have a two-hour block to produce the entry-week artefact described above. The artefact has two parts and a required cross-check.

### Part A — Ladder diagram (45 minutes)

Produce a diagram (whiteboard photo, Excalidraw, Mermaid, draw.io, or ASCII) that shows:

- Your role at the centre, labelled `ai-engineering-manager · L45 · AI Engineering family`.
- The six neighbouring roles covered in chapter 01, at the correct levels and in the correct families:
  - `ai-augmented-team-manager` (L40, AI Workforce family) — sibling below.
  - `ai-enablement-lead` (L45, AI Workforce family) — sibling at level.
  - `ai-infra-team-lead` (L45, AI Infra family) — peer manager.
  - `agentic-systems-architect` (L48, AI Engineering family) — staff-plus IC above.
  - `principal-ml-engineer` (L48, ML Engineering family) — staff-plus IC above (peer of the architect).
  - `chief-ai-officer` (L70, AI Governance family) — executive.
- The IC sub-ladders you manage, with level numbers: applied-ai-engineer (L30), agentic-ai-engineer (L30), senior-agentic-ai-engineer (L40), ai-eval-engineer (L30), rag-engineer (L30), fine-tuning-engineer (L30), llm-application-developer (L20).
- The governance peers whose evidence you consume: ai-risk-engineer (L25), ai-evaluation-engineer (L35).
- An explicit "out of scope" cluster for: legal counsel, procurement / finance, IT identity, security operations / DFIR, external research.

### Part B — Deferral contract (60 minutes)

Produce a one-to-two-page markdown document, structured as four sections — **Down**, **Sideways**, **Up**, **Out** — matching the direction headings in chapter 03. For every role in the diagram, write exactly two lines:

- `You hand them:` what artefact, decision, or evidence crosses the boundary from you to them.
- `They hand you:` what artefact, decision, or evidence crosses the boundary from them to you.

For the **Out** section, each entry has one line only: the specific reason that role's work is not yours, and where the handoff routes.

### Part C — Cross-check (15 minutes)

Pick three *ambiguous* scenarios from the list below (or invent your own) and, for each, write one sentence pointing at the diagram + contract and naming who owns the first move. The scenarios are chosen because they land in the inbox of several roles at once and the wrong absorption is a common first-quarter failure.

1. A product manager asks: "Can your team own the AI usage policy for the sales org?"
2. Legal flags a new sub-processor clause in a provider DPA. "Who reviews this?"
3. The CFO wants a quarterly ROI model for AI investment across the engineering org. "Who builds this?"
4. An engineer on your team ships a prompt change that regresses the eval. The on-call page fires. "Who runs point?"
5. A frontier lab announces a capability-shift model release in four weeks. "Who reads it, and who decides if we re-plan?"
6. The CAIO asks for a board-deck slide on your team's safety posture.
7. SecOps reports a possible data-exfil via tool-use on your agent.
8. A staff engineer proposes rewriting the retrieval layer in a new framework.

## Requirements

- The diagram includes **level numbers** and **family labels** for every role. Levels without numbers do not count.
- The deferral contract names a **concrete, nameable artefact** for every "hand them / hand you" line — "collaboration" is not an artefact. "Red-team findings report" is.
- The out-of-scope section names **where the work routes**, not just "not us."
- The cross-check section resolves each scenario by pointing at a *specific* line in the deferral contract — no floating prose.

## Starter guidance

- Chapter 01 gives you the six neighbours at a glance. Chapter 03 gives you the full enumeration with "hand them / hand you" phrasing for the sideways / up / out entries — adapt and refine; don't just copy.
- When you are stuck on "what crosses the boundary" for a role, read the `JOB_REQUIREMENTS.md` ownership rule and the role summary in the matching curriculum repo; the one-line answer is almost always there.
- For the cross-check, resist the urge to say "we'll figure it out." A level-45 EM who cannot answer "who makes the first move" in under a minute leaks ownership.

## Acceptance criteria

- The ladder diagram exists as a viewable artefact (image, diagram source, or ASCII block) and includes all 15+ roles listed above with correct levels and families.
- The deferral contract has exactly four direction-section headings and a filled "You hand them / They hand you" row for every role except out-of-scope entries.
- Three cross-check scenarios are resolved, each by pointing at a specific contract line.
- You can explain the diagram in under two minutes out loud.

## Stretch goals

- Add a **version header** to the deferral contract (date, author, team context). You will produce v2 later in the module 111 cross-functional work; having v1 dated helps.
- Add a second diagram: the same ladder re-drawn as if you were a level-70 CAIO looking down at the organisation. The exercise of seeing your role from above changes what you prioritise in your 1:1 with that role.
- Produce a **risk list** of the three handoffs most likely to break in your actual (or chosen) team context, with a one-line mitigation each.
