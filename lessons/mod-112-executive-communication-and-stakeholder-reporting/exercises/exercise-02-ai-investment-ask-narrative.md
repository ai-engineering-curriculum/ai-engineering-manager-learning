# Exercise 02: AI Investment Ask Narrative

**Estimated effort:** 2.5 hours · **Output:** two full investment-ask documents, each covering a different ask shape, both with honest citations and three-band confidence projections.

## Objective

Produce two complete investment-ask documents that would land honestly at a CFO's or VP of engineering's desk. Each must cite the empirical productivity research (Dell'Acqua et al.'s jagged frontier, Peng et al.'s Copilot-productivity RCT) honestly — by name, within the generalisability window — rather than as blanket "AI productivity gains" framing. Each must project three confidence bands with named assumptions and an exit criterion that makes the ask reversible.

A peer reviewer reading either document should be able to say: what is being asked, what is being promised, under what assumptions, with what evidence, and what happens if the assumptions do not hold.

## Prerequisites

- Chapter 03 (`Defending the AI investment ask`).
- Mod-110 chapter 03 (ROI projection with confidence bands and the misses ledger) — the band-projection mechanics.
- Mod-110 chapter 06 (FinOps lever set) — the pre-pulled-levers discipline.
- Access to Dell'Acqua et al. 2023 (HBS Working Paper 24-013) and Peng et al. 2023 (arXiv:2302.06590). Read both at least to the abstracts and the methods sections before authoring; you cannot cite honestly what you have not read.
- Optional: your organisation's actual Q+1 budget cycle calendar and finance-reporting template; the exercise lands more concretely when grounded in a real planning window.

## Problem statement

Two investment-ask documents, each 2–3 pages, covering two different ask shapes from chapter 03's four (model-spend, labelling-data budget, headcount, tooling). Pick any two of the four — but at least one must be the headcount ask (because it most often misuses the productivity-research citations).

### Part A — Ask one, honest and defensible (60 minutes)

The document has the sections chapter 03's worked example demonstrates:

1. **The ask in one sentence.** The specific line-item being asked for, the quarter the ask covers, the one-sentence hypothesis.
2. **Pre-ask lever status.** For a model-spend ask, this is the mod-110 chapter 06 five-lever reading. For a labelling ask, this is the pre-labelling-levers reading (fixture synthesis tried, auto-labelling tried, sampling discipline tried). For a headcount ask, this is the pre-headcount-levers reading (cross-shape rotation tried, contract work tried, growth plan tested). For a tooling ask, this is the build-vs-buy matrix (mod-110 chapter 04).
3. **Three-band projection.** Low / mid / high with named assumptions for each; the mid band is the planning number; the low band acknowledges the loss if assumptions do not hold.
4. **Citations.** Dell'Acqua et al. and / or Peng et al. cited by name, with the generalisability window stated. The team's context read against the window. Internal citations (eval-registry entries, cost-model lines, historical slice-closure rates) alongside the external literature.
5. **Exit criterion.** The specific condition under which the ask is wound down, the budget returned, or the plan re-negotiated. Dated.

### Part B — Ask two, same structure, different shape (60 minutes)

Produce a second ask of a different shape than the first. If ask one was model-spend, ask two is labelling-data, headcount, or tooling. If ask one was not the headcount ask, ask two must be. The structure is the same; the specific sections vary per chapter 03's parameter-variation notes.

### Part C — Red-team pass (30 minutes)

For each of your two asks, write a 200-word red-team note from the perspective of:

- **The CFO's finance analyst.** What would a research-literate finance person flag? Where is the generalisability window stretched? Where is the mid-band assumption likely to move? Where does the pre-pulled-levers section invite a "come back when you've optimised" response? Rewrite any section that fails the red-team.
- **A research-literate peer AI engineering manager.** Where would they say "that's not quite what the paper found"? Which citation is doing work the underlying study does not support?

The red-team produces a revised version of each ask. Submit both the first and revised versions; the diff is where the honesty discipline is visible.

## Requirements

- Two complete asks produced, each 2–3 pages.
- At least one of the two is the headcount ask.
- Each ask has all five sections from chapter 03.
- Dell'Acqua et al. (2023) cited by name — full reference, not just "the HBS study" — in at least one of the two asks. The generalisability window (consulting population, 18 task set, GPT-4 mid-2023) is stated honestly.
- Peng et al. (2023) cited by name — full reference, arXiv ID — in at least one of the two asks. The generalisability window (HTTP-server task, moderate-experience developers, bounded implementation task) is stated honestly.
- At least one of the two asks also cites a non-productivity-literature source — the EU AI Act, the NIST AI RMF, a vendor's own technical report, a published AI safety or evaluation paper — relevant to the specific investment.
- Every ask has a three-band projection; the mid band is the planning assumption; the low band names a loss condition; the high band is labelled as upside and not planned against.
- Every ask names an exit criterion with a specific condition and a specific date.
- Each ask has a pre-pulled-levers or pre-alternative section demonstrating the alternatives attempted.
- The red-team notes are produced and the revised versions ship with the diff visible.

## Starter guidance

- Read Dell'Acqua et al. before authoring. The paper's actual finding — the "jagged frontier" — is more nuanced than most citations give it credit for. The 40% quality gain on inside-frontier tasks is paired with a 19-percentage-point *decline* on outside-frontier tasks. Citing only the first number is misleading; citing both and framing your ask as "investment in the capability to tell the difference" is honest.
- Read Peng et al. before authoring. The 55.8% speedup is on a specific bounded task (implement an HTTP server in JavaScript); the generalisability to open-ended engineering work is not established by the paper. An ask that invokes Peng to justify "55% more capacity across the team's workload" is overclaiming.
- For the headcount ask: the hardest version is asking for a new `fine-tuning-engineer` or `ai-eval-engineer` to cover a shape the team does not currently have. The alternative-paths reading (contract work, cross-shape rotation, growth plan for an existing IC) is the discipline the ask must defend against.
- For the labelling-data ask: cost-per-fixture is measurable on your team's own eval registry. The honest ask reads the unit math, not the dollar envelope. A $240k ask without a "we expect this to produce ~2,400 fixtures at ~$100 per fixture, distributed as X across the Y slices" is a dollar envelope, correctly rejected.
- For the tooling ask: the build-vs-buy matrix (mod-110 chapter 04) is the pre-ask surface. A tooling ask without a filled matrix is an ask that has not done the alternatives work.
- For the model-spend ask: the mod-110 chapter 06 FinOps-lever reading is mandatory. A CFO reads "we need more model-spend headroom" from a team whose caching rate has been 8% for a year as "come back when you've pulled the levers." Pull them first; then ask.
- The red-team pass is where the exercise earns its name. Resist the urge to red-team gently. The finance analyst's job is to find the generalisability gap; the peer manager's job is to find the citation that is doing unsupported work. If the red-team finds nothing, the red-team was not honest.

## Acceptance criteria

- Two ask documents exist, each 2–3 pages, one of which is a headcount ask.
- Dell'Acqua et al. (2023) and Peng et al. (2023) are cited by name in at least one of the two asks; the generalisability windows are stated; the team's context is read against each.
- Three confidence bands with named assumptions are present in both asks; the low band names a loss condition; the mid is the plan; the high is labelled upside.
- Exit criteria are specific and dated.
- Pre-pulled-levers or pre-alternative-paths sections demonstrate the alternatives already attempted.
- Red-team notes are produced from the finance-analyst and peer-manager perspectives; a revised version of each ask addresses the red-team's findings.
- A peer AI engineering manager, reading either ask, can summarise in one minute what is being asked and what happens if the assumptions do not hold.

## Stretch goals

- **The ask that was declined.** Produce a third ask that you deliberately write to be declinable — the exit criterion is specific, the low band is below the ask's cost, the mid-band is honest about a 40% probability of moving to the low band. The exercise tests whether the honest ask is an ask you can *stand behind even if declined*. If you cannot bring yourself to write it, the four asks you have been writing until now may have been marketing.
- **The multi-year trajectory.** Produce a 3-year rolling ask for one of the two shapes, with the three-band projection extended across each year and the external-literature citations updated for the window (if newer research exists at the time of authoring). The compounding of the confidence bands year-over-year is the exercise; naïve multiplication of the bands produces a range so wide it is meaningless, and the discipline is the conditional-independence reading between year 1 and year 2 outcomes.
- **The denied ask autopsy.** Pick a real ask (yours or a public industry case) that was declined. Write the retrospective ask document — what was the ask, what was the denial reason, what would the chapter-03 discipline have changed. The exercise is calibration: your draft asks should resemble the ones that would have been approved, not the ones that were declined.
- **The cross-ask portfolio read.** If your team has three or four live asks across a budget cycle (model-spend, labelling, headcount, tooling simultaneously), produce the portfolio-level read — how the asks compose, where they are substitutes versus complements, which one survives if the envelope shrinks 25%. The CFO reads the portfolio, not the individual asks; the exercise is to write to that altitude.
- **The AI-literacy pre-brief for the funder.** Produce a two-page "how to read the AI engineering asks" primer for a CFO or VP who is new to the function. The primer explains the three-band discipline, the jagged-frontier framing, the pre-pulled-levers convention, and the exit-criterion rule. The primer is itself a credential-honesty artefact (chapter 05) — it makes future asks easier for *both* sides to read.
