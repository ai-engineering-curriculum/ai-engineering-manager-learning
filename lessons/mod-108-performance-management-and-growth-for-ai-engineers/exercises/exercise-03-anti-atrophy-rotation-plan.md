# Exercise 03: Anti-Atrophy Rotation Plan

**Estimated effort:** 2 hours · **Output:** (a) a team-level anti-atrophy posture memo; (b) three individual anti-atrophy plans — one each for an L20, an L30, and an L40 IC — with the three loops right-sized to level; and (c) a six-month cadence calendar showing when each loop runs.

## Objective

Produce the chapter-05 artefacts an AI Engineering Manager owns: a published team-level posture that makes the anti-atrophy discipline visible to the whole team, three individual plans that show the loops right-sized across the ladder, and a cadence calendar that protects the time on the team's operating rhythm.

A peer AI Engineering Manager reading your artefacts should be able to say what the team's stance is on frontier-assistant use, what each of the three ICs is doing in each loop this cycle, and when the next deliberate-practice slot, reading loop, and rotation loop run for each IC.

## Prerequisites

- Chapter 03 — the 1:1 spine where the anti-atrophy check lands.
- Chapter 05 — the three loops (deliberate practice, reading, rotation), their right-sizing by level, the manager's four responsibilities, the four warning signs.
- The three publicly-cited productivity studies on AI assistants in the module resources (Peng et al. 2023; Microsoft Research 2024; METR 2025 experienced-developer study). You do not need to re-derive their findings; the posture memo references them.
- The team's current composition (or a realistic composite) at the three levels: one L20, one L30, one L40, each in a specific shape.
- The current model portfolio (mod-105) the team's work depends on.

## Problem statement

Three artefacts. The team-level memo goes in your engineering handbook; the individual plans go in the ICs' growth-plan docs (or alongside them); the cadence calendar can live in the handbook or in the team's shared calendar system.

### Artefact A — the team-level anti-atrophy posture memo (40 minutes; 400–600 words)

A short memo, in your engineering handbook, that establishes the team's posture on frontier-assistant use and anti-atrophy. Sections:

1. **Purpose.** Two paragraphs on why the team has an explicit anti-atrophy posture. Frame it as the chapter-05 motivation — this is a discipline that counters a drift the tool naturally introduces to craft; it is not a ban and not an accusation.
2. **The three loops as team practice.** Short descriptions of the deliberate-practice loop, the reading loop, and the rotation loop. One paragraph per loop. Explain that every IC carries each loop on their growth plan, right-sized to level.
3. **The team's reading-loop targets.** A specific list of cookbooks, provider release notes, and release-note aggregators the team collectively tracks. The reading-loop IC artefact is a 2–3 sentence note per fortnight shared in the team's shared doc.
4. **The team's rotation cadence.** How the team sequences rotation loops — when a provider ships a new model, when a framework ships a breaking change, when a new embedding model lands. The named owner per rotation.
5. **The manager's own loop.** A short paragraph (modelling the pattern per chapter 05 core concept 3, move 4) naming what the manager's own anti-atrophy artefact looks like this cycle.
6. **Measurement and honest-cadence note.** Explain that the posture does not measure assistant use directly (bans produce workarounds; measurement via logs is often legally and culturally fraught); it measures the three loops. Reference the chapter-03 1:1 spine as the diagnostic.

### Artefact B — three individual anti-atrophy plans (50 minutes; one per IC)

For each IC — L20, L30, L40 — a short plan (150–250 words) with:

1. **The IC's shape and current grid.** From exercise 01's rubric.
2. **Deliberate-practice loop.** Per chapter 05 table — L20 weekly, L30 fortnightly, L40 monthly. For each loop, specify the problem shape, the timebox, and the produced artefact (the IC's own notes).
3. **Reading loop.** Protected calendar time; the specific sub-list of provider release notes / cookbooks the IC is responsible for this cycle; the fortnightly note commitment.
4. **Rotation loop.** The specific rotation this cycle — what is being migrated or compared, against what, with what produced artefact (memo, side-by-side delta, migration notes). L20 may observe without owning a rotation; L30 owns a small-scope rotation; L40 owns a sub-system rotation.
5. **1:1 spine integration.** Which week of the chapter-03 spine the anti-atrophy slot lands on for this IC.
6. **Risk notes.** If this IC has shown any of the four warning signs from chapter 05 (repeated "let me pull it up," deprecated-model memory, vanishing unassisted design docs, frozen portfolio), name it honestly — the plan is shaped in response.

### Artefact C — the six-month cadence calendar (30 minutes)

A table or calendar, in your team's cadence tool (Notion, GitLab handbook, shared calendar), covering the next six months. Rows for each of the three ICs; columns for each week. In each cell, mark which loop is running: DP (deliberate practice), RL (reading loop), ROT (rotation). The point is to see the rhythm — where the time lands, who is doing what each week, where conflicts with cycle-end work or planned PTO exist. Mod-109 owns the operating-cadence deeper design; this calendar is the chapter-05 slice.

## Requirements

- Team-level memo is 400–600 words, published in the handbook, with the six sections.
- Memo names the three loops as team practice, lists the reading-loop targets concretely, names the rotation cadence, shows the manager's own loop.
- Memo includes a measurement / honest-cadence note that does not rely on invasive assistant-use logging.
- Three individual plans produced, one each for L20, L30, L40.
- Each individual plan right-sizes the three loops to the IC's level per the chapter-05 table.
- Each individual plan specifies a concrete problem shape for the deliberate-practice loop, a concrete reading-loop target list, and a concrete rotation artefact.
- Each individual plan carries risk notes that honestly reflect any warning-sign signal on the IC today.
- Six-month cadence calendar covers all three ICs with weekly granularity.
- Loops do not stack unrealistically; the manager has protected the time by writing it on the calendar, not by hoping it fits.

## Starter guidance

- The team memo's hardest sentence is the opening paragraph that frames the discipline as counter-drift, not anti-assistant. If the memo reads as a ban or an accusation, the team will read it as either hostile to the tool or indifferent to their intelligence. Rewrite the opening until it reads as *pro-craft*.
- The three loops are not interchangeable. Do not consolidate deliberate practice + rotation into one slot "because it's efficient." The loops produce different muscles; the consolidation is the pattern teams try first and abandon.
- For the L20 plan, over-weight deliberate practice. L20 is the level where the craft is being built in the first place; assistant-driven L20 ships produce L20 who have not grown. Four weekly sessions, 30–60 minutes each, is the right target.
- For the L40 plan, over-weight rotation and reading. L40 engineers make decisions that affect scope; they owe the team currency with the stack they are deciding against. One monthly deliberate-practice session, often producing a teaching artefact, is right.
- The reading-loop target list should match the IC's shape. An eval engineer's reading loop targets OpenAI Evals release notes, Inspect, promptfoo, Ragas, DeepEval, and one methodology paper per cycle. An applied engineer's list is different.
- Resist listing every blog or newsletter. The reading loop is a *protected hour fortnightly*, not a browse. Pick 3–5 primary sources the IC is actually responsible for engaging with and summarising; the rest is noise.
- The rotation loop produces a working artefact, not a memo-on-what-I-read. The rotation IC makes something — a side-by-side eval delta, a migration spike, a new-embedding comparison. Chapter 05 is specific about why.
- On the calendar, make sure the loops do not conflict with the team's cycle-end rituals (demo day, retrospectives), on-call rotations, or holidays. The protection matters; chapter 05 core concept 3, move 1, is specific.
- Do not plan the manager's own loop as an afterthought. If the memo's "manager's own loop" paragraph is weaker than any individual plan, the team reads the signal immediately.
- Share the memo in draft with one or two senior ICs on the team for a reality check before publishing. If a senior IC reads the memo as condescending, the message is not landing; revise the frame.

## Acceptance criteria

- Team-level memo, three individual plans, and six-month calendar all produced.
- Team memo is 400–600 words with the six sections and lives in the handbook.
- Memo frames the discipline as counter-drift and pro-craft, not anti-assistant.
- Three individual plans right-size the three loops to L20 / L30 / L40 per the chapter-05 table.
- Each plan has concrete problem shapes, reading targets, and rotation artefacts named.
- Each plan carries risk notes honestly.
- Six-month calendar protects weekly deliberate-practice slots, fortnightly reading hours, and quarterly rotation blocks on actual team-calendar dates without conflict.
- Manager's own loop is named in the team memo and is not weaker than any IC's plan.
- The artefacts collectively would survive a peer-manager review that asks "is this a ban, is this an accusation, or is this a counter-drift discipline?" — answer is clearly the third.

## Stretch goals

- **Full team coverage.** Produce individual plans for every IC on your team, not only three. The full set is the material for next cycle's growth-plan read-out (chapter 04 section 4).
- **Published reading-loop digest.** Pick one fortnight and compile the team's reading-loop notes into a 300-word team-visible digest. The digest is a lightweight ritual that normalises the reading loop; three cycles of digests make the loop self-sustaining.
- **Rotation artefact library.** Collect the rotation artefacts from the first two cycles into a short library. The library becomes an onboarding resource for new hires (a new L20 can read "here's what rotation memos look like on our team" rather than guessing).
- **Dry-run a deliberate-practice session.** Pick one of the problem shapes from your L30 plan; run the session yourself, assistant off, timeboxed. Produce the notes the IC would. The exercise is in the writing of the notes — if the notes are thin, the session was too, and the IC's session will be too.
- **Measurement without surveillance.** Design two team-level health metrics for the anti-atrophy posture that do not rely on tracking individual assistant use. Examples: the number of rotation memos published per cycle, the completeness of the reading-loop digest, the dates on the last deliberate-practice slot across the team. The metrics live on your mod-109 operating-cadence surface.
- **Team retrospective design.** Draft a short retrospective agenda for a mid-year check-in on the anti-atrophy posture. What went well, what drifted, what to re-sequence. Include questions ICs should be asked and the honest-cadence frame (chapter 03) that keeps the retro from becoming a performance-review.
