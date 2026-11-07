# Fair Calibration Under Rapid Technology Drift

An engineer plans a quarter against the model landscape in October; by December, the model provider has shipped a new version that closes the gap the engineer's work was meant to address, and the deliverable is shelved. Another engineer plans a quarter against a specific framework; the framework ships a breaking change in month two, and the engineer spends the rest of the quarter on the migration rather than the planned feature. A third engineer plans a quarter against a stable surface; nothing drifts; they ship against the original plan on time. If the three are reviewed against their original plans without an explicit drift-fair practice, the first two are graded worse than the third — not because they did worse work, but because the ground moved under them. Over cycles, this systematically punishes the engineers doing the hardest work (the ones working on the fastest-moving surfaces) and systematically selects for engineers who avoid surfaces that can be obsoleted by a vendor release. In an AI engineering function that specific anti-selection is corrosive.

This chapter is the manager-owned calibration practice that keeps the review fair when the technology underneath is moving. It borrows the calibration ritual from mod-102 chapter 04 (where it runs on the hiring loop) and specialises it for performance review on the AI engineering surface.

## Motivation

The fair-calibration problem has three concrete shapes the AI engineering manager sees most.

- **The shipped-against-replaced-model case.** An engineer commits in cycle planning to deliver a fine-tune that will shave 40% off serving cost. The provider ships a cheaper, close-to-as-good base model mid-cycle. The planned fine-tune is no longer justifiable. The engineer and the manager agree to shelve it. The engineer's cycle is now underweighted on "shipped artefacts" because the artefact the plan committed to was shelved for a reason both agreed to. Without an explicit drift-fair review, the engineer's review reads "did not ship the fine-tune"; with it, the review reads "shelved the fine-tune in week 7 because the provider's release closed the cost gap; produced the shelving memo; re-anchored on retrieval work for the remainder of the cycle."
- **The framework-regression case.** The team's agent framework ships a breaking change in month two. The engineer who owns the agent runtime spends six weeks migrating. The migration is uncredited scope in the original plan but is now the engineer's primary deliverable. Without drift-fair review, the engineer appears to have produced less; with it, the review names the migration explicitly as a drift-induced pivot and reads the migration memo as the primary cycle artefact.
- **The quiet-rubric-inflation case.** Eighteen months ago, L30 applied-AI meant "shipped an LLM feature end-to-end with meaningful trace work." The frontier model generation that year made trace work on common patterns half as hard. If the rubric is not re-read, promotions silently slow: the same work that was L30 eighteen months ago is now closer to L20-plus, and ICs who would have been promoted are not, without an explanation. Mod-102 chapter 04 names this on the hiring side; here it is the performance side of the same bug.

A drift-fair review does three things at once: it credits engineers for the work they actually did, it keeps the rubric honest about the moving baseline, and it does not reward engineers who avoided drift-exposed work by staying on stable surfaces.

## The counterfactual-at-planning-time principle

The central rule the manager and the IC share is simple enough to write in a sentence:

> **Grade the engineer against the plan they agreed to at planning time, read against the information available then, not against a counterfactual you have now.**

The principle is the base; the practice is the operationalisation.

### What the practice looks like

Three specific mechanics turn the principle into a review habit.

1. **Dated planning memo.** Every cycle plan starts with a dated memo that names the assumptions the plan rests on: the current model portfolio, the current framework versions, the current vendor roadmap, the current provider-release cadence. Not an exhaustive essay; one paragraph. The memo is what the review packet (chapter 03, section 4 "Drift notes") reads against at cycle-end.
2. **In-cycle drift note.** When drift lands mid-cycle (a provider release, a framework regression, a deprecation notice), the manager and the IC write a drift note in the shared 1:1 doc: *what changed, when, what the re-anchor was, what got shelved*. Dated. The drift note is the audit trail at review time.
3. **Cycle-end drift synthesis.** The review packet section 4 synthesises the drift notes into a short narrative: "this cycle saw X vendor-driven drift; the IC re-anchored from Y to Z in week W; the artefacts the plan committed to that survived the drift are these; the artefacts shelved by drift are these; the artefacts added by drift are these." The synthesis is read at calibration.

### What the practice is not

The principle is **not** a get-out-of-shipping-free card. Three specific things it does not do:

- **It does not credit shipping-nothing.** An engineer whose whole cycle was "drift" and who did not produce the shelving memos, the drift notes, the re-anchor memos, and the rotation-loop artefacts (chapter 05) still did not ship. The drift note is evidence the engineer *responded to drift well*; absence of any deliverable is still absence.
- **It does not excuse avoidable drift.** An engineer who committed to a plan that ignored a vendor roadmap item already announced is accountable for the planning. The planning memo is read with the information available *then* — including the information the engineer should have been reading at the time (chapter 05 reading loop).
- **It does not cover every drift as equivalent.** A provider version-bump that moves pass rates two points is drift the IC adapts to; an architectural shift that invalidates the plan requires a re-plan, which is a manager-plus-IC decision with a written re-plan memo. The two are graded differently; the practice is about naming them.

## Specific calibration adaptations for AI engineering

The base calibration practice (per-cycle debrief, rolling pipeline review, annual ladder re-read) is what every mature review function runs. The AI-specific adaptations below are what this chapter adds.

### Adaptation 1 — the three cycle drifts named explicitly

At the calibration meeting, every IC's review is read against three drift questions, in order, before the rating is set:

1. **Was there vendor-driven drift on this IC's surface in the cycle?** If yes, the drift notes are read. The review language is specific about the re-anchor.
2. **Was there framework / tooling drift on this IC's surface?** If yes, the migration or tooling-rebuild work is named as cycle artefact.
3. **Was there a rubric shift between the cycle plan and the cycle end?** If yes (rare but not unheard of — a model-generation shift mid-cycle can land here), the calibration meeting flags the rubric read and consults the drift-corrections log.

The three-question read is written into the calibration-meeting template. The point is that drift is read deliberately, not passed over because the meeting is pressed for time.

### Adaptation 2 — the shipped-against-replaced-model note

A specific sentence the review packet includes when the case arises:

> "IC committed to [deliverable] in planning memo [date] against [then-current] model version. Provider shipped [new-version] in [week], which [closed / changed / invalidated] the opportunity. IC and manager agreed to [shelve / re-scope / re-anchor] on [date]; the re-anchored work is [description]. The planning-time call was defensible against the information available then."

The note sits in section 4 of the review packet. It is a single paragraph. Its purpose is to make the drift-fair read visible at calibration without re-litigating the decision; calibration either accepts the note or escalates with a specific disagreement (which nearly never happens when the note is specific).

### Adaptation 3 — the model-generation-shift rubric re-read

When a frontier-model generation shift happens in or shortly before a review cycle (GPT-5-class to GPT-6-class, Claude 5-class to Claude 6-class, Gemini 3-class to Gemini 4-class, or similar), the manager triggers a rubric re-read before the cycle's calibration meeting. The questions:

- Does the chapter-02 anchor for L30 in each shape still describe work that would have been at-level eighteen months ago, or has the baseline moved? If the baseline moved, the anchor either tightens (the new L30 does more than the old L30) or specialises (the new L30 does different work than the old L30).
- Are there specific artefact-class rows where the baseline moved? Trace reading on common patterns is the usual first-to-shift row; generic feature-shipping throughput is the second.
- Who on the team is currently rated against the old anchor? Is their rating still justified under the new anchor?

The output of the re-read is a dated change log entry on the published rubric (chapter 02's publication discipline); the specific adjustments go in writing; the calibration meeting reads against the new anchors with the old audit trail visible.

### Adaptation 4 — the rolling review of recent reviews

Mod-102 chapter 04 names this pattern on the hiring side ("rolling pipeline calibration"). The performance-side equivalent runs every two to four cycles: the AI-engineering management group reads the last N completed review packets together and asks three questions:

- Did we grade drift fairly in each? Where we did, is the pattern reusable? Where we didn't, what was the specific miss?
- Did we hold the rubric to the same bar across packets? If one IC's L30 read differently from another IC's L30, which one was off?
- Did the external-signal weight (chapter 07) read consistently across packets, or did one manager weight a conference talk heavily and another rate the same talk down to zero?

The output is a dated correction to either the rubric or the drift-fair practice. Per mod-102 chapter 04's pattern: the correction is published, not whispered.

### Adaptation 5 — the no-counterfactual-grading rule at calibration

The specific prohibition the manager raises if calibration slips into it. A sentence that sometimes appears in calibration meetings:

> "If we'd known about the provider release at planning time, we would have scoped X differently; grading them at 'meets' instead of 'exceeds' reflects what we know now about Y."

This is counterfactual grading and is the specific pattern the drift-fair discipline prevents. The correction in the moment: *we grade against the plan read at planning time; if we think the plan would have been different with hindsight, that is useful planning feedback for next cycle, not a reason to downgrade this cycle's review*. The manager who holds this line protects the team's willingness to take on drift-exposed work.

## The four drift patterns to catch, with corrections

Four specific drift patterns the calibration practice is tuned to catch. Each has a specific correction (borrowed and adapted from mod-102 chapter 04).

### Drift 1 — rubric-anchor drift under model improvement

The pattern. The baseline model capability improved; the chapter-02 rubric was not re-read; the old anchors describe work that is now easier than it was at writing. Promotions slow; attrition rises among engineers who meet the "old" L30 bar but are rated against an implicit new one nobody has written down.

The correction. Trigger an annual (or event-driven) rubric re-read on model-generation shifts. Publish the new anchor with a dated change log. Explain the shift to the team: "we tightened L30 applied on 2026-04-15 because baseline X moved; here's how the new anchor reads."

### Drift 2 — assistant-throughput inflation

The pattern. The baseline throughput for feature-shipping has risen because the assistant is doing more of the mechanical work. Reviews that weight shipped-feature count without weighting judgment-under-the-throughput drift toward rating engineers on assistant leverage rather than on their craft.

The correction. Calibration explicitly asks the chapter-05 diagnostic set per IC: does the throughput sit on owned judgment? If the calibration cannot tell for a given IC, the review packet is incomplete; send it back for the specific question. Over time, the artefact-class grid in chapter 02 (rather than throughput count) is the review's primary read.

### Drift 3 — shape-drift

The pattern. Mid-cycle re-anchoring (drift adaptation 2) has moved an IC's actual work toward a different shape than the one they are placed in on the rubric. An applied engineer who spent two-thirds of the cycle doing eval work is doing eval work, not applied work; the review that rates them on the applied rubric misses what they actually did.

The correction. The calibration meeting flags the shape-drift explicitly; the rating places the IC against the shape they actually worked in for the cycle, and the growth plan for next cycle names the shape placement question explicitly (do they want to transition fully? Trajectory 2 in chapter 04).

### Drift 4 — secret-language drift

The pattern. Internal shorthand ("that's a stretch-L4," "upper-L30 but not yet L40-ready") grows up next to the published rubric and replaces it in calibration meetings. Different managers mean different things by the shorthand; ratings diverge; candidates can't self-read.

The correction. The calibration meeting re-reads the published rubric aloud when the shorthand appears. If the shorthand captures a real concept, add it to the published rubric with a version note. If it does not, strike it from the meeting. Mod-102 chapter 04 is explicit about this on the hiring side; the correction is the same here.

## The calibration-meeting ritual, AI-specific

A calibration meeting on an AI engineering team looks like this:

1. **Open with the rubric read.** Someone reads the current version of chapter 02's anchors aloud for the levels under discussion. One minute per level. The rubric is in the room, not implicit.
2. **For each IC, read the packet aloud, in sections.** Section 2 (artefact-class evidence) and section 4 (drift notes) get the most time. The three drift questions (adaptation 1) are read in order; the drift notes are read against them.
3. **For each disputed rating, argue row by row.** The artefact-class grid is the unit of argument; "they're above-level at X" must point at specific cycle artefacts.
4. **Flag the four drift patterns.** Rubric-anchor drift, assistant-throughput inflation, shape-drift, secret-language drift. If any appears in a packet, name it and apply the correction.
5. **Produce the dated changes.** Any rubric change, any published correction, any growth-plan commitment coming out of the meeting is written down with a date and lands on the published rubric or the IC's growth plan.
6. **Never counterfactual-grade.** If the meeting starts grading against hindsight, the facilitator (usually the hiring manager or the AI Engineering Manager chairing) names the move and holds the line.

The meeting is three to five hours for a function of eight to twelve engineers, run twice a year. It is the ritual that keeps the system fair; the ritual is boring, procedural, and load-bearing.

## A concrete read: a drift-affected review

An L30 applied engineer's review at cycle-end. The compressed read from the packet and the calibration discussion:

- **Shape and level.** Applied-AI, L30, placed on the rubric currently published (v2026.02).
- **Planning memo.** Dated 2026-10-01. Committed to: ship the research-assistant feature; migrate to the new structured-output API; co-author the fixture-coverage work with the eval engineer. Assumptions: current model portfolio (Claude Sonnet 5.6 / GPT-5), framework version SDK 2.4.1.
- **Drift notes.** 2026-11-10: provider shipped SDK 2.5.0 with structured-output semantics change; IC spent weeks 6–9 on the migration. 2026-11-28: a frontier model release improved the research-assistant task's unassisted baseline enough that the originally planned orchestration became unnecessary; IC and manager agreed to simplify the orchestration and ship the lighter version in week 11. 2026-12-18: provider deprecated an endpoint used by the fixture work; IC spent two days re-pointing fixtures.
- **Artefact-class grid.** Traces: at. Evals: at. Registered artefacts: above (co-authored the fixture-coverage work on time). Incidents: at (led one incident on the research-assistant surface). Mentorship: at.
- **Rating.** Meets at L30 with a strength in registry hygiene; the drift-adapted deliverables (simplified orchestration, SDK migration memo) are read as cycle-defining work, not skipped scope.
- **Growth plan for next cycle.** Chapter 04's cross-shape rotation into eval (now committed with the eval specialist as sponsor); chapter 05's anti-atrophy loop renewed; chapter 07's one external artefact commitment (an upcoming eval-focused meetup talk).

What did not happen. Nobody at calibration said "but if we had known about SDK 2.5.0, we would have scoped differently." The drift was named, the re-anchor was named, the review graded against what the engineer did, not against what the counterfactual could have been. The engineer walked out of the review with a clear growth plan and no residual sense that drift had been held against them.

## What good looks like

- The published rubric (chapter 02) has a visible version and a dated change log; the last entry reflects a drift-driven adjustment.
- Every review packet in the last cycle has a drift-notes section, dated entries, and a synthesis paragraph.
- The calibration meeting template includes the three drift questions and the four drift-pattern checks.
- When drift was caught in rolling review, the correction was published to the rubric or the growth-plan template.
- No counterfactual-grading language ("if we had known...") appears in finalised review packets from the last cycle.

## What bad looks like

- The rubric has no version number, no change log, and reads the same as it did two years ago despite three model-generation shifts.
- Review packets have no drift-notes section; drift is argued orally in calibration and lands inconsistently.
- Calibration meetings routinely re-grade with hindsight; engineers on drift-exposed work learn that the fair thing to do is take stable work.
- Secret leveling shorthand ("that's clearly a stretch-L4") appears in calibration meeting notes; nobody can find it in the published rubric.
- The last model-generation shift was more than six months ago and the rubric has not been re-read.

## Summary

- Fair calibration under rapid technology drift is a specific practice on top of the canonical review cycle; it is not the whole of the review, it is the AI-specific adaptation that keeps the review honest when the ground is moving.
- The central principle: grade the engineer against the plan they agreed to at planning time, read against the information available then — not against a counterfactual you have now.
- Five adaptations on the calibration meeting: the three drift questions per IC; the shipped-against-replaced-model note; the model-generation-shift rubric re-read; the rolling review of recent reviews; the explicit no-counterfactual-grading rule.
- Four drift patterns to catch, each with a specific correction: rubric-anchor drift, assistant-throughput inflation, shape-drift, secret-language drift.
- The ritual is boring, procedural, and load-bearing. Teams that run it hold their drift-exposed engineers; teams that do not run it quietly lose them.

The next chapter is the last of the module: credential honesty. Conferences, publications, open-source contributions, vendor certifications — what is useful growth, what is theatre, how to weight external artefacts in review without rewarding the louder profile over the quieter contribution to the team's operating system.
