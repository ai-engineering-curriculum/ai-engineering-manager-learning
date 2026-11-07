# Exercise 03: 90-Day Entry Plan for One Team

**Estimated effort:** 2 hours · **Output:** a filled 90-day plan (five artefacts) for one specific team

## Objective

Produce the 90-day entry plan from chapter 04 for **one specific team** — real or carefully-chosen reference. The output is the set of artefacts the chapter prescribes: a stakeholder-tour plan for Phase 1, the four inventories for Phase 2, and the three commitments for Phase 3, all specific enough that someone could use them to critique your plan rather than nod politely.

## Prerequisites

- Chapter 04 (`The 90-day entry plan`).
- Chapter 01 and chapter 03 — the ladder diagram and deferral contract shape the stakeholder tour.
- The four inventories draw on material you will study in depth later: model + vendor (mod-105), eval + incident (mod-104 and mod-107), prompt + judge registry (mod-104). For this exercise a one-page read per inventory is enough.
- Pick a team context before you start. The three usable patterns:
  - A team you have joined or will join in the next 90 days. (Strongest.)
  - A team at a prior employer you can describe honestly. (Fine.)
  - A composite team built from public references, with a written one-paragraph team charter you author yourself. (Fallback — cite anything you draw from.)

## Problem statement

Produce the following five artefacts.

### Artefact 1 — Team charter (10 minutes)

One paragraph. Team name, size, scope (what AI products or agents it ships), the one sentence that answers "why does this team exist." If your team context is real and you cannot share specifics, anonymise and note so at the top. This is the shared ground for everything else.

### Artefact 2 — Phase 1 stakeholder tour plan (20 minutes)

A named list of every 30-minute conversation you will run in weeks 1–2. For each entry:

- Name (or role, if anonymised).
- Role / level.
- One sentence on what you expect to learn.
- The three questions from chapter 04 you will ask every direct report (included once at the top, not repeated per entry).

Include your direct reports (every one of them), your manager, your staff-plus IC partner if one exists, at least one sibling-manager peer (`ai-infra-team-lead` or sibling), at least one governance peer, at least one product partner, and the CAIO or equivalent executive if one exists in your context.

### Artefact 3 — Phase 2 inventories (60 minutes)

Produce four one-page inventories. Each one answers the question in its chapter 04 description. Use real or realistic numbers — a schematic with `[placeholder]` cells is acceptable where you genuinely cannot know the value yet, but every row with a placeholder should also name the action you take to fill it.

- **Inventory 1 — Team shape.** Role mix (counts per sub-ladder), open roles and time-to-fill, attrition-risk read, span-of-control number, gap statement (one row per gap).
- **Inventory 2 — Model and vendor inventory.** Models in production with provider, version, task, cost. Contract posture per provider. Known deprecations. Fallback posture per model. Gap statement (highest-severity row per model).
- **Inventory 3 — Eval and incident inventory.** Registered eval suites with coverage and gating level. Golden datasets with owner and refresh cadence. Judges in use. Incidents from the last two quarters, categorised. For each, was a permanent eval fixture added. Gap statement (three evals that should exist and do not).
- **Inventory 4 — Prompt and judge registry audit.** Where every production prompt lives. Versioning posture. Judge registry posture. PR hygiene. Gap statement (one sentence on where the registry needs to be in 90 days).

### Artefact 4 — Phase 3 commitments (20 minutes)

Exactly three commitments.

- One **operating-cadence change** (meeting, ritual, review) — named, with a reason drawn from your inventories.
- One **release-gate change** — named, with the specific policy text, enforced from a specific date.
- One **roadmap ask** — named, defended with evidence from your inventories.

Each commitment has three lines: the change, the evidence, the start date. Nothing more.

### Artefact 5 — Day-90 one-pager (10 minutes)

Write the one-page document chapter 04 prescribes as the day-90 artefact. It has five sections:

1. What I heard in Phase 1.
2. What I found in Phase 2.
3. What I committed to in Phase 3.
4. What I'm watching in days 91–180.
5. What I need from this leadership group to be successful.

Keep it to one printed page.

## Requirements

- Every artefact is produced for the same team context named in Artefact 1. No switching teams mid-exercise.
- The stakeholder tour includes every direct report — not "my direct reports" as a group entry.
- Each inventory names at least one real gap. If an inventory lists zero gaps, the exercise is not complete — you are not looking hard enough.
- The three commitments are drawn from the inventories. A commitment that cannot cite an inventory row to justify it does not count.
- The day-90 one-pager fits on one printed page.
- Where you use real numbers, do not fabricate. Use `[placeholder: measure before quoting]` when you genuinely do not have the value; use `<!-- needs-research: ... -->` comments when a factual claim would require external evidence you do not have.

## Starter guidance

- The schematic model + vendor inventory in chapter 04 is a template, not a model answer. Replace every placeholder with real or realistic content for your team.
- If your team context does not include a specific role (e.g. no in-house CAIO), say so in Artefact 1 and route the stakeholder-tour conversation to the nearest equivalent.
- For commitments, the common failure is picking three that are either all cadence-shaped (safe, low-signal) or all gate-shaped (risky, unfunded). The template forces one of each category deliberately.

## Acceptance criteria

- Five artefacts produced, all for the same team.
- Four inventories, one page each, each with at least one real gap statement.
- Three commitments, each citing a specific inventory row.
- A day-90 one-pager that fits on one printed page.
- You can point at any inventory row and say in one sentence why it matters for your three commitments.

## Stretch goals

- Produce a **risk-weighted timeline** for days 46–90: which commitment is most likely to slip, what the leading indicator is, what the fallback plan looks like.
- Role-play the day-90 one-pager as a 10-minute readout to your (real or chosen) manager. Record what questions you expect, and whether your plan answers them.
- For one of the inventories, write the **day-180 version** — what the one-page document will look like after a full quarter of execution. Comparing day-30 and day-180 versions is a useful calibration exercise for how much a quarter actually moves things.
