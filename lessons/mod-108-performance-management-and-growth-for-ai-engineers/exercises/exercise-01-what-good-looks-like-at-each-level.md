# Exercise 01: What Good Looks Like At Each Level and Shape

**Estimated effort:** 2 hours · **Output:** a published, versioned, behaviour-worded level rubric for the AI-engineering IC family on your team (or a realistic composite team), covering L20 → L48 across the six primary IC shapes.

## Objective

Produce the first manager-owned artefact of this module: the AI-engineering-specific level rubric specified in chapter 02. The rubric is the document your team's performance reviews (chapter 03), growth plans (chapter 04), and calibration meetings (chapter 06) will read against for the next twelve months.

A peer AI Engineering Manager, reading your rubric, should be able to pick any IC on your team at random, read the rubric, and place them at a level in a shape within five minutes — without asking you to translate any secret-language shorthand.

## Prerequisites

- Chapter 01 of this module for the four manager-owned artefacts and the four deferrals.
- Chapter 02 — the full rubric structure (three dimensions, five artefact classes, four level anchors, shape specialisations).
- Mod-102 chapter 01 (the AI-engineering role matrix — seven IC shapes) and chapter 04 (the calibration mechanics on the hiring side).
- The three public leveling exemplars: Dropbox Engineering Career Framework, levels.fyi Google explainer, GitLab Engineering Handbook.
- Your team's (or a realistic composite team's) current state: what shapes exist, which ICs hold which titles today, which IC tracks exist upstream.

## Problem statement

A published rubric, in markdown, filed where your team's engineering handbook lives (team wiki, Notion, GitLab-style handbook). The document has four parts.

### Part A — the rubric header (10 minutes)

A header block that makes the rubric legible and maintainable:

- Rubric version (semantic version or date).
- Change-log (even if this is version 1.0.0, write "created 2026-11-07; next scheduled re-read 2027-04 on the next frontier-model-generation shift").
- The three public exemplars cited by link (Dropbox, levels.fyi Google, GitLab).
- A one-paragraph preface explaining the rubric's purpose, its dimensional structure, and its artefact classes.

### Part B — the four level anchors (60 minutes)

Four sections, one per level anchor: L20, L30, L40, L48. For each anchor, specify:

1. **The dimensional read** — scope, autonomy, influence at this level (3–5 bullets each).
2. **The artefact-class read** — what the five artefact classes (traces reviewed, eval suites, prompts/judges/fixtures registered, incidents led, mentorship/cross-shape pairing) look like at this level (one paragraph per class).
3. **The honest-cadence note** — any level-specific anti-atrophy or assistant-leverage note (chapter 05).

The L30 section is the longest, because L30 is where five shapes anchor. L20 and L48 are shorter.

### Part C — the shape specialisations (40 minutes)

For each of the six primary IC shapes — `applied-ai-engineer`, `agentic-ai-engineer`, `ai-eval-engineer`, `rag-engineer`, `fine-tuning-engineer`, `senior-agentic-ai-engineer` — produce a short specialisation section (5–10 sentences) covering:

1. **The unmistakable artefact** at this shape at its primary level (per chapter 02 core concept 2). What is the one thing an IC of this shape produces that an IC of another shape does not?
2. **The shape-specific read on each of the five artefact classes.** What does "traces reviewed" look like for an eval engineer vs. a RAG engineer? Specify.
3. **The common confusions.** Which adjacent shape is this shape most often confused with? What is the telling tell?

If your team does not currently employ all six shapes, write the specialisation anyway for the shapes you expect to hire into over the next twelve months (mod-102 chapter 01).

Also produce one section covering the two staff-plus IC shapes (`agentic-systems-architect`, `principal-ml-engineer`), at the L48 altitude, with lighter detail since the staff-plus IC tracks are the depth anchors.

### Part D — the publication discipline (10 minutes)

A short closing section naming:

- Where the rubric lives (URL / path on your handbook).
- How it is linked from JDs (mod-102 chapter 02), from promotion packets, from 1:1 growth-plan docs.
- The cadence on which it is re-read (default annual + triggered by frontier-model-generation shift; chapter 06 is specific).
- The named owner.

## Requirements

- Rubric has a visible version number and a change log.
- Rubric cites the three public exemplars by link.
- L20, L30, L40, L48 all have full sections covering scope / autonomy / influence and the five artefact classes.
- Each of the six primary IC shapes has a specialisation section with the unmistakable artefact, the shape-specific artefact-class read, and the common-confusions note.
- The staff-plus altitude (L48) has a section with both canonical shapes named.
- The rubric is **behaviour-worded**, not years-of-experience-worded. If you catch yourself writing "5+ years of AI experience," rewrite the line as a behaviour.
- The rubric has an honest-cadence note at L30 and L40 (where the frontier-assistant halo risk is highest).
- Publication discipline section names where the rubric lives, who owns it, and when it is re-read.
- Length: 2,500–4,000 words. Longer rubrics drift toward bureaucracy; shorter ones elide the specialisations.

## Starter guidance

- Write the L30 anchor first. L30 is the level where five of the six shapes live and where the specialisation work is most valuable. If the L30 anchor is strong, L20 and L40 flow from it; if the L30 anchor is thin, every specialisation reads thin.
- Specialise on the artefact-class *content*, not on title or scope hierarchy. "Owns the measurement contract for one or more product surfaces" is behaviour-worded; "Senior Eval Engineer" is a title.
- For the unmistakable-artefact bullet per shape, test the sentence against the other shapes. If an applied-AI engineer could also produce the artefact you wrote for `ai-eval-engineer`, the specialisation is not sharp enough — rewrite.
- Resist the temptation to write L48 as a long aspirational essay. The staff-plus IC tracks (`agentic-systems-architect-learning`, `principal-ml-engineer-learning`) are the depth anchors; your rubric places the role rather than teaches it.
- The honest-cadence notes are new and often the parts of the rubric that get skipped. Write them anyway. Three sentences per level; name the specific risk at this level (L20: craft never grew in the first place; L30: throughput-inflation halo; L40: scope-reach without owned judgment); the risk is the thing the review will diagnose against.
- Do not write the rubric in isolation. The staff-plus IC partner (`agentic-systems-architect` or `principal-ml-engineer`) should read your L40 and L48 anchors before you publish; the peer manager (`ai-infra-team-lead`) should read the cross-shape specialisations where the infra sub-ladder touches.
- A rubric that would be unchanged by a frontier-model-generation shift is probably too abstract. The L30 applied anchor should talk about work on a surface that is model-sensitive; chapter 06 is specific about the re-read cadence.
- Publish in writing, not in slides. Slides lose the version history and the linkability chapter 02 requires. Markdown in a team handbook or a GitLab-style wiki is the canonical form.

## Acceptance criteria

- Rubric is a single markdown document in your team's engineering handbook (link it in your submission).
- Rubric carries a version, a change log, and a named owner.
- The three public exemplars are cited by link.
- L20, L30, L40, L48 sections are complete with scope / autonomy / influence and the five artefact-class reads.
- Six IC shape specialisations are complete with unmistakable artefact, shape-specific artefact-class read, and common-confusions note.
- L48 section names both canonical staff-plus shapes.
- Rubric is behaviour-worded throughout; no years-of-experience language.
- Honest-cadence notes present at L30 and L40.
- Publication-discipline section specifies where the rubric lives, who owns it, and when it is re-read.
- 2,500–4,000 words.
- A peer manager can read the rubric and place any IC on your team at a level and shape in under five minutes.

## Stretch goals

- **Current-team reading.** Take every IC on your team and produce a one-line placement against your new rubric (shape + level + artefact-class grid). Compare against their current title and level. Note any surprises — those are the drift-corrections your first calibration meeting after publishing the rubric will consume.
- **JD cross-reference.** Write or update one open-role JD (mod-102 chapter 02) against your new rubric. The rubric's artefact-class language should surface in the JD's expectations section.
- **Rubric-delta memo.** If your team already had a leveling rubric before this exercise, produce a short delta memo covering what changed, what stayed, and what the delta implies for anyone currently at a level whose anchor shifted. Chapter 06 is specific about the calibration conversation this produces.
- **Staff-plus third shape.** If your team's staff-plus candidates do not fit the architect or principal MLE rubric cleanly, draft a third staff-plus shape (e.g., principal eval engineer, principal retrieval architect) and defend the addition. Mod-102 chapter 04 names the pattern.
- **Cross-ladder read.** Map your rubric row-by-row against the Google L3→L7 shape and the Dropbox IC2→IC5 progression. The map should be plausible; where it is not, your specialisations may have drifted from the public anchor and should be reconciled.
