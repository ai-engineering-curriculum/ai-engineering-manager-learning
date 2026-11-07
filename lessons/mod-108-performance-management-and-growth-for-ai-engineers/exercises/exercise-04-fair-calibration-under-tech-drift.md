# Exercise 04: Fair Calibration Under Tech Drift

**Estimated effort:** 2 hours · **Output:** (a) a drift-fair review packet for one IC whose cycle was affected by technology drift, (b) a mock calibration-meeting facilitation guide applying the five chapter-06 adaptations and catching the four drift patterns, and (c) a dated correction log the meeting outputs land in.

## Objective

Produce the chapter-06 artefacts a manager owns at review time when the ground has moved underneath the cycle. The review packet is what the IC and the calibration meeting read; the facilitation guide is what you (as the meeting chair or participant) run against; the correction log is the published audit trail that keeps the rubric honest.

A peer AI Engineering Manager, reading your packet, should be able to say why each cycle artefact is weighted the way it is — specifically where drift required a re-anchor and how the review graded the engineer against the plan read at planning time. Reading your facilitation guide, they should be able to run the mock calibration meeting themselves and arrive at a defensible rating.

## Prerequisites

- Chapter 03 — the review-packet template with the eight sections, especially section 2 (artefact-class evidence) and section 4 (drift notes).
- Chapter 06 — the counterfactual-at-planning-time principle, the five calibration adaptations, the four drift patterns and their corrections, the no-counterfactual-grading rule.
- The published level rubric from exercise 01.
- An IC whose cycle was affected by technology drift. Use a real case if you have one; otherwise, construct a realistic composite from chapter 06's shipped-against-replaced-model, framework-regression, or rubric-shift examples.
- The latest frontier model portfolio read (mod-105) for the context the plan was made against.

## Problem statement

Three artefacts, in markdown, filed where your performance-review documents live.

### Artefact A — the drift-fair review packet (60 minutes)

A complete review packet for one IC, following the chapter-03 eight-section template. The specific adaptations for this exercise are in sections 2 and 4.

Sections:

1. **Context and shape** — one paragraph. Shape and level against exercise 01's rubric; artefact-class grid from the last review; any shape transitions in the cycle.
2. **Artefact-class evidence** — for each of the five artefact classes (traces reviewed, eval suites, prompts/judges/fixtures registered, incidents led, mentorship/cross-shape pairing), three to five specific examples from the cycle, dated, with links where possible. Artefacts the IC produced in response to drift (migration memos, shelving notes, re-anchor artefacts) are counted in the relevant class.
3. **Growth-plan read** — what the growth plan from last cycle said; what moved; what did not; the honest read on the anti-atrophy loop and the rotation targets.
4. **Drift notes** — the key section for this exercise. A short dated narrative of the drift that landed in the cycle, with specific drift notes from the 1:1 shared doc. For each drift event: what changed, when, what the re-anchor was, what got shelved, what got added. Explicitly includes the **shipped-against-replaced-model note** (chapter 06 adaptation 2) if applicable.
5. **External signal** — any external artefacts from the cycle; the three test questions (chapter 07) applied to each.
6. **Peer-feedback synthesis** — three to five lines synthesising peer feedback.
7. **Rating and rationale** — the level placement at cycle-end, the rating, and a two-sentence rationale that references the rubric and the artefact-class grid. The rating must survive the no-counterfactual-grading rule; if it does not, the rating is wrong.
8. **Next-cycle growth plan** — the co-authored plan for next cycle, with named commitments and dated check-in points. Carries forward any anti-atrophy and credential-honesty commitments.

### Artefact B — the mock calibration-meeting facilitation guide (30 minutes)

A short document (300–500 words) that a facilitator would use to run a calibration meeting on this packet (and any other packets you add). The guide covers:

1. **The opening read.** The specific rubric anchors the meeting will re-read aloud (chapter 06 calibration ritual step 1). Identify which levels are under discussion and copy-paste the anchors from your exercise 01 rubric.
2. **The three drift questions.** The specific questions the facilitator asks per packet (chapter 06 adaptation 1): vendor-driven drift, framework / tooling drift, rubric shift.
3. **The four drift-pattern checks.** For each packet, the facilitator reads the four drift patterns from chapter 06 and names any that apply (rubric-anchor drift, assistant-throughput inflation, shape-drift, secret-language drift).
4. **The dispute-resolution script.** A short script the facilitator uses when the room disagrees on a rating. Modelled on the mod-102 chapter 04 disputed-debrief walkthrough. The row-by-row grid argument; the artefact citation requirement; the re-read of the rubric aloud; the specific line "if we had known..." that triggers the no-counterfactual-grading rule.
5. **The output discipline.** What the meeting writes down (the dated correction log — Artefact C) and what it does not (secret-language shorthand, oral-only decisions).

### Artefact C — the dated correction log (30 minutes)

A section of your published rubric (exercise 01) or a separate audit-trail document. Contains:

1. **At least one correction** the mock calibration meeting produces for this exercise. Could be a tightened L30 anchor on a row the rubric drifted on, a published change to the growth-plan template, a note on how the external-signal weight was read across packets, or a specific shape-drift placement adjustment.
2. **The date** of the correction.
3. **The reason** (short, two sentences): what the meeting saw, what pattern it caught, what the correction is.
4. **The audience** the correction is published to (the team handbook, the engineers at an all-hands, the hiring page, the candidates in the current pipeline).

## Requirements

- Review packet carries all eight sections from chapter 03.
- Section 2 (artefact-class evidence) names at least three specific dated examples per class.
- Section 4 (drift notes) includes dated drift events, named re-anchors, and (if applicable) the shipped-against-replaced-model note in the specific chapter-06 phrasing.
- Section 7 (rating and rationale) names the level placement, the rating, and the two-sentence rationale against the rubric and the grid.
- Rating survives the no-counterfactual-grading rule; the rationale does not include the phrase "if we had known..." or any equivalent counterfactual.
- Facilitation guide carries the five sections above with specific content, not placeholders.
- Facilitation guide is usable by a peer manager running the meeting cold.
- Correction log has at least one dated correction with reason and audience.
- Correction log entry is published to a named audience (a specific page in the handbook, or an all-hands slot, or similar).
- All three artefacts refer to the same rubric (exercise 01); the artefacts form a coherent set.

## Starter guidance

- Pick an IC whose cycle was genuinely affected by drift. If you construct a composite, make it specific — a specific model-version bump, a specific framework version, a specific deprecation date. Generic drift ("the AI landscape was changing") is not drift; drift is dated.
- Write section 4 (drift notes) before section 2 (artefact-class evidence). The drift notes determine which cycle artefacts count — the shelving memo, the migration note, the re-anchor artefact are evidence that belongs in section 2.
- Write section 7 (rating and rationale) last, after drafting all the other sections. The rating reads off the sections, not the other way around. Managers who draft the rating first then write evidence to support it are counterfactual-grading in the other direction.
- For the shipped-against-replaced-model note, use the exact chapter-06 phrasing — "IC committed to [deliverable] in planning memo [date] against [then-current] model version. Provider shipped [new-version] in [week], which [closed / changed / invalidated] the opportunity. IC and manager agreed to [shelve / re-scope / re-anchor] on [date]..." — and fill in specifics.
- For the facilitation guide, the dispute-resolution script matters most. A calibration meeting where everyone agrees needs no facilitation; the meetings worth preparing for are the ones where the room is split. Write the script against the hardest disagreement you can imagine for your packet.
- For the correction log, the correction has to be real, not performative. If your mock calibration meeting legitimately caught something — a rubric row that drifted, an external-signal weight that read differently across managers, a shape-drift placement — correct it in writing. If the meeting caught nothing, write a one-line note that says so and name the next check-in that will catch something.
- Resist the temptation to pre-decide the rating. The exercise is to show the discipline that reaches a defensible rating, not to prove a rating you already picked. The packet that reads as "arguing for the rating" is the packet that will not survive a real calibration room.
- The no-counterfactual-grading rule is the hardest to internalise and the easiest to drift against. Read your draft section 7 twice looking specifically for "if we had known" phrasing or its equivalents. Rewrite ruthlessly.
- Share the packet in draft with one peer manager before finalising. The peer's fresh read is the simulation of calibration; what they ask is what the real meeting will ask.
- The correction log is published, not whispered. If the correction cannot be published (because it is personnel-specific or legally sensitive), it is not a *rubric* correction — it is a review-specific decision. The rubric corrections are the ones that read publicly.

## Acceptance criteria

- Review packet produced, all eight sections complete.
- Section 2 has at least three dated examples per artefact class.
- Section 4 has dated drift events, named re-anchors, and (if applicable) the shipped-against-replaced-model note in chapter-06 phrasing.
- Section 7's rating and rationale is defended against the no-counterfactual-grading rule; no counterfactual phrasing in the rationale.
- Facilitation guide covers the five sections and is usable by a peer manager without additional context.
- Facilitation guide includes a dispute-resolution script of at least 150 words.
- Correction log has at least one dated correction with reason and named audience.
- Corrections are rubric-level, not personnel-level; the log is publishable.
- All three artefacts refer to the same rubric and read as a coherent set.

## Stretch goals

- **Full cycle's packets.** Produce drift-fair review packets for every IC on your team whose cycle was affected by drift. Run a real calibration meeting against them. The five adaptations and four drift patterns are the facilitation content.
- **Rolling-review log.** Pair the correction log with two prior cycles' hypothetical corrections, building a dated multi-cycle audit trail. Mod-102 chapter 04's rolling-pipeline-calibration pattern is the parallel.
- **Rubric re-read.** If your facilitation meeting catches a rubric-anchor drift (chapter 06 drift pattern 1), produce the specific rubric update in your exercise 01 document. Update the version number, add the change-log entry.
- **Counterfactual-grading training deck.** Produce a short (6–8 slide) training deck for peer managers or hiring managers on the no-counterfactual-grading rule. Include three concrete examples of the pattern and the specific manager move that catches it. Mod-112 (executive communication) is where the deck might eventually land.
- **Public rubric diff.** If your rubric is or could be public, produce the diff the correction log implies and share it with candidates currently in-pipeline. The GitLab-handbook publication discipline (chapter 02 and chapter 06) is the pattern.
- **AI-assistant-use calibration note.** If your packet's IC's cycle involved substantial assistant use, produce a short note on how the review packet handled the chapter-03 honest-cadence split — was the assistant-throughput inflation drift (chapter 06 drift pattern 2) evident, what did you look for, what did you find, what did the review conversation include. The note is a reusable template for other packets.
