# Exercise 02: Team Management Canon Crosswalk to AI Engineering

**Estimated effort:** 2 hours · **Output:** one crosswalk table + three worked examples

## Objective

Produce a **crosswalk** that reads general engineering-management canon (Fournier, Grove, Larson, Lopp, Google re:Work, Project Oxygen) through the lens of AI-engineering-specific scope. The output is the table structure introduced in chapter 02 — canonical practice on the left, AI-specific attachment on the right — filled in with enough specificity that someone reading it over your shoulder could point at any row and recognise the real work. Then you ground it with three worked examples drawn from a real team context.

## Prerequisites

- Chapter 02 (`The operating contract: canon + AI-specific scope`).
- Working familiarity with at least one of the canonical references: *The Manager's Path*, *High Output Management*, *An Elegant Puzzle*, *Managing Humans* / Rands in Repose, Google re:Work, Project Oxygen. If you have not read any, pick one and read a chapter before continuing — the crosswalk only works when you know what you are attaching AI scope *to*.
- (Optional but recommended) A specific team context. Your own, your employer's, or a well-documented public team.

## Problem statement

### Part A — Produce the crosswalk table (60 minutes)

Build a table with at minimum the following rows — add more if your team context calls for them.

| Canonical practice | Canonical reference | AI-specific attachment | Owning module |
|---|---|---|---|
| 1:1 cadence and career growth | | | mod-108 |
| Feedback close to the event | | | mod-108 |
| Hiring loop mechanics | | | mod-102 |
| Interview rubric calibration | | | mod-102 |
| OKR / planning | | | mod-103 |
| Roadmap and replanning | | | mod-103 |
| Code review | | | mod-104 |
| Release process | | | mod-104, mod-106 |
| Vendor / tooling decisions | | | mod-105, mod-110 |
| Incident response | | | mod-107 |
| Performance calibration | | | mod-108 |
| Team shape and span | | | mod-109 |
| Budget / FinOps | | | mod-110 |
| Cross-functional interface | | | mod-111 |
| Executive communication | | | mod-112 |

Fill every cell:

- **Canonical reference:** a specific book chapter or re:Work guide. *"Grove, Chapter 2 — Managerial Leverage"* counts; "Grove" alone does not.
- **AI-specific attachment:** the concrete thing the AI context adds on top of the canonical practice, named with enough specificity that it maps to an artefact or decision. *"Prompt + trace + judge review as a code-review requirement"* counts; "AI-aware code review" does not.

### Part B — Three worked examples (45 minutes)

Pick three rows from your table and, for each, write a 150-to-250-word worked example that shows:

- What the canonical practice looks like, done well, in a non-AI team.
- What the AI-specific attachment adds, in a specific scenario.
- The one concrete artefact or moment where you can tell the attachment has actually been made (not just talked about).

Drawing from a real team is strongly preferred. If you do not have one, you may use a well-documented public case (an engineering blog post, a published postmortem, a conference talk), but cite it.

### Part C — Gap list (15 minutes)

List three places in your crosswalk where you suspect the AI-specific attachment is underdeveloped in your real or chosen team context — the attachment *should* exist but either does not yet or is only loosely practised. These are not commitments (you have not spent 90 days there yet); they are observations that would shape module 101's final chapter (`The 90-day entry plan`).

## Requirements

- Every row of the crosswalk has all four columns filled.
- The AI-specific-attachment column names a **concrete artefact, decision, or ritual** — not a mood ("AI-aware," "thoughtful about AI").
- Each worked example names a **specific artefact or moment** that is the evidence the attachment has actually been made.
- The gap list has three entries, each one sentence, each with the row it maps to.

## Starter guidance

- Chapter 02's existing table is a floor, not a ceiling — you are expected to go deeper than it on each row.
- When in doubt about the AI-specific attachment, ask: *"What does the canonical practice fail to notice?"* If a backend EM doing the canonical practice well would still miss something important in an AI context, that something is the attachment.
- For the "Owning module" column, let the module numbers in the table do the work — you do not need to re-summarise the module. The point is to see at a glance which modules of this track pick up which attachment.

## Acceptance criteria

- The crosswalk table has at least 15 rows, all four columns filled.
- Three worked examples (150–250 words each), each pointing at a specific artefact or moment.
- Three gap-list entries, each mapped to a specific row.
- You can read any row aloud in under 10 seconds and the attachment is immediately parseable.

## Stretch goals

- Add a fifth column: *canonical-practice failure mode in an AI team.* For each row, name the specific way the canonical practice falls over when the attachment is missing — e.g. "release process: calendar-based ship date passes while eval delta is still negative."
- Pick one row and write the **before / after** version of the team's operating cadence that would land the attachment. One short paragraph each.
- Interview (informally) one real engineering manager who runs an AI-engineering team and ask: which row of the crosswalk did you underestimate in your first six months? Note the answer at the bottom of the document.
