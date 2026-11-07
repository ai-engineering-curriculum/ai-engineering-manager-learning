# Exercise 01: Role Matrix Across Applied, Agentic, Eval, RAG, and Fine-Tune

**Estimated effort:** 3 hours · **Output:** one role matrix table + one role-to-shape placement rule

## Objective

Produce the two artefacts an AI Engineering Manager needs before writing a single JD: a **role matrix** that enumerates the AI-engineering IC shapes with their primary artefacts and level anchors, and a **placement rule** that takes a concrete open-headcount request and routes it to a shape. By the end of this exercise you should be able to look at a vague "we need an AI engineer" request from a product partner and respond with a shape, a level, and the artefacts the role will own — in under five minutes.

## Prerequisites

- Chapter 01 (`The AI engineering role matrix`) of this module.
- The role-matrix table in chapter 04 (`Calibration across the AI engineering ladder`) for the L20 → L48 anchors.
- The paired IC-track pages (linked from chapter 01) for depth on each shape. You are *placing* shapes, not re-teaching them.

## Problem statement

You are standing up a new AI engineering team (or inheriting one with unclear role definitions). Produce the two artefacts below. Each builds on the chapter but does not copy it: your version is specialised to a real or realistic team context.

### Part A — Role matrix table (90 minutes)

Produce a markdown table with one row per IC shape and the following columns. The seven shapes listed in chapter 01 are the minimum; add others (e.g. `prompt-engineer`, `ai-platform-engineer`) if your team context genuinely needs them, with a one-line justification for each addition.

- **Shape** (e.g. `applied-ai-engineer`)
- **Level** (one of L20, L30, L40; note which ladder for staff-plus)
- **Primary artefact** — one sentence, the thing nobody else on the team produces in quite this way
- **Representative traces reviewed** — shape and volume
- **Representative eval posture** — authored, extended, or commissioned
- **Representative incident posture** — participated, led, commanded, methodology-shaped
- **Confused with** — the nearest-shape miscast, named
- **IC curriculum link** — the authoritative source on craft depth

Keep every cell to one line. Longer cells are a sign the matrix is doing the IC track's job.

### Part B — Role-to-shape placement rule (60 minutes)

Write a one-page markdown document with two sections.

**Section 1 — Placement decision tree.** A set of questions that, applied to a product partner's request, routes to a shape in the matrix. Candidate questions to include:

- "What does the feature's daily failure mode look like — a wrong answer, a wrong tool call, a missing retrieval, a slow response, a judge drift?"
- "Does the team have an eval suite that covers this capability, or is the eval work itself the gap?"
- "Does the feature require multi-step planning or a single model call?"
- "Is the measurement contract already written and defended, or does it need to be authored first?"
- "Does the role need to **own** the retrieval corpus contract or just consume one?"
- "Is a fine-tune the plausible answer, or is this vendor-API territory?"

**Section 2 — Three worked placements.** Three concrete, realistic open-headcount requests, each resolved against the decision tree. For each, show:

- The request as it arrived (one sentence from a product or engineering partner).
- The one or two questions from the tree that were load-bearing.
- The shape and level you place it at.
- The ML/infra-adjacent role (chapter 01) you deliberately *did not* place it at, and why.

### Part C — Cross-check (30 minutes)

Pick two ambiguous shape-boundary scenarios from the list below (or invent your own) and, for each, write two or three sentences placing them at a shape using your matrix and tree. These are deliberately in the grey zone.

1. A product partner wants to hire "a prompt engineer who can also handle evaluation."
2. A candidate's résumé says "ML Engineer" and their last three roles have been building RAG systems for enterprise SaaS.
3. The CAIO wants to add "a staff AI engineer who can own the agent platform." The team of six has no staff-plus IC yet and is shipping one agent.
4. A product manager says "we just need someone who can plug in models and ship."
5. A hiring request lands for "an AI engineer for our fine-tuning workstream" but the team has never fine-tuned a model, only used vendor APIs.
6. A sibling team is hiring "an AI Engineering Manager" but every bullet in the JD is about running GPU clusters.

## Requirements

- The matrix row count matches the shape count — no shape is collapsed; no shape is invented without a one-line justification.
- Every row's **primary artefact** is a nameable thing (a trace, a registered prompt, a judge-case corpus, a fine-tune checkpoint). "Collaboration" is not an artefact.
- Every row's **confused with** cell names a specific adjacent shape or an ML/infra-adjacent role — not "generic AI engineer."
- The placement tree's questions are **answerable** against a one-paragraph request. A question that needs a full discovery doc is too heavy.
- Each worked placement names the ML/infra-adjacent role you did *not* pick and why.

## Starter guidance

- Resist the urge to invent shapes. Seven is already a lot for a small team to hold in its head. If your team context needs an eighth, write the justification clearly; next quarter you may fold it back into one of the seven.
- The hardest rows to write are the two most-confused pairs: `applied-ai-engineer` vs. `agentic-ai-engineer` at L30, and `ai-eval-engineer` vs. `rag-engineer` vs. `applied-ai-engineer` on measurement-adjacent roles. Spend extra time on the primary-artefact cell for each — specificity here does the leveling work.
- The placement tree is written for **you and your recruiters**, not for candidates. It can be pointed and opinionated. Candidate-facing language lands in the JD (exercise 02).
- Keep the IC curriculum link visible. The handoff rule across the curriculum is that craft depth belongs to the IC tracks; your matrix *places* shapes, not teaches them.

## Acceptance criteria

- The role matrix table covers at least the seven shapes from chapter 01, with every column filled on every row.
- The placement tree has at least five load-bearing questions and resolves cleanly for each of the three worked placements.
- The two cross-check scenarios are resolved by pointing at specific cells in the matrix and specific questions in the tree — no floating prose.
- You can walk a product partner through the matrix and get to a shape in under five minutes.

## Stretch goals

- Add a **"what we do not hire for" row** to the matrix: a one-sentence statement of the roles that look adjacent but route to other ladders (`principal-ml-engineer`, classical MLE, AI infra family, AI governance peers). This is the first line of defence against the hiring-request miscast.
- Add a **version header** to the matrix (date, team context). You will update it in a year during the annual ladder calibration (chapter 04); having v1 dated helps.
- Produce the matrix in two altitudes — one for your team (narrow), one for the whole AI engineering function (wider). The exercise of translating between them reveals where your team's shape mix is unusual.
