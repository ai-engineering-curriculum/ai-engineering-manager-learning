# Designing the Research-to-Production Handoff (R16)

Somewhere in or near your organisation is a research function — an in-house ML research team, a principal MLE with a research charter, an external research lab you collaborate with, or a frontier-lab customer-success team feeding you preview capabilities. Research produces things your team ships: a new retrieval approach, a fine-tune, a judge, a tool-use pattern, a safety classifier. The point where research hands something to your team is a **contract**, not a casual drop-off. This chapter designs that contract.

The internal shorthand this track uses for this handoff is **R16** — research-into-engineering, sixteenth and last of the core handoff contracts. The number is a label, not a ranking; the content is what matters.

## Motivation

The failure modes are symmetric and expensive.

- **Pick-up-and-pray.** Research throws a Jupyter notebook and a dataset over the fence; the engineering team spends weeks reproducing results, re-implementing in production-grade form, discovering the eval scope was too narrow, and shipping a feature that regresses in ways no one predicted. Research complains "they didn't use what we built." Engineering complains "what they built wasn't shippable."
- **Infinite-research orbit.** A research track keeps producing work that never lands. Engineering is polite about it; research is unaware; the organisation pays for both sides for quarters. No one has a mechanism to decommission the track.

The contract below exists to prevent both.

## The core idea

A research artefact is accepted into the engineering team only if it clears a named acceptance gate, and research tracks get an explicit decommissioning path when they stall. The gate is light enough that the research team can clear it in days, not quarters, if the work is actually ready — and firm enough that research cannot push "almost ready" into production as "ready."

## Acceptance gate — four dimensions

Four categories of evidence open the gate. If any is missing, the artefact does not promote into the production roadmap.

### 1. Reproducibility

The engineering team can reproduce the headline numbers with a one-line command on their own hardware (or on documented cloud hardware the team has access to), from a commit SHA, within an agreed-on budget of machine time.

- The repo has a top-level `README.md` that contains the reproduction command.
- All non-trivial dependencies are pinned. Any external data fetch is scripted and versioned.
- The reproduced numbers land within a stated tolerance of the research team's headline numbers.
- If the artefact is a fine-tuned model, there is a shipped checkpoint plus the training recipe; the engineering team does not need to re-train to accept, but if they chose to, they could.

A common failure here is "it reproduces on the research team's hardware but not on the engineering team's hardware." That counts as *not reproducible*. Hardware-fragile results do not belong in production.

### 2. Eval scope

The research artefact has been evaluated on a set of suites broad enough that the engineering team can reason about where it will and will not work. Required:

- Evaluation against the research team's primary benchmark **and** against at least one of the engineering team's registered eval suites — ideally the suite the production surface is gated by.
- Evaluation against the same data distribution the production feature runs on. If this does not exist yet, it is a commitment the research team makes before acceptance, not after.
- An honest write-up of the suites the artefact **does not** beat, if any, and the research team's read on why. Research artefacts with no stated weak spots are almost always misrepresenting their coverage.

The engineering manager and the principal MLE (or architect) co-review this piece. It is the one most often rushed.

### 3. Documentation

A single document — typically a model card or system card, in the sense of Mitchell et al. (2019) — covers:

- What the artefact is and is not designed to do.
- Training or evaluation data provenance at a level sufficient for the engineering team's data-governance posture.
- Known limitations and failure modes. This section is non-optional; a model card without a "limitations" section is incomplete.
- Safety considerations relevant to this artefact (not a full safety review — that is a separate gate, owned with the governance peer).
- Dependencies (model families, APIs, external services) the artefact pulls in.
- Suggested usage patterns and anti-patterns.

If the research team is external, this document is the primary contract artefact and must be delivered before engineering begins integration.

### 4. On-call readiness

Research artefacts do not typically ship with on-call expectations. The handoff is the point where that changes. Required at acceptance:

- A written **first-ninety-days support agreement**: who the engineering team pages when the artefact misbehaves in production, and how. For in-house research, this is usually a named individual on the research team committed to a bounded on-call fraction (e.g. secondary-tier pager, not primary) for the first ninety days after promotion. For external research, it is a support contact and a response-time expectation.
- A decommissioning path (see below) in case the artefact is pulled from production for reasons the research team must help resolve.
- A **retrospective window** scheduled at day 90 — a post-handoff review where research and engineering look at the first ninety days of production behaviour together.

Without this piece, the engineering team ends up owning every behaviour of an artefact it did not build and cannot debug. Attrition risk on your team is highest here.

## The acceptance vote

Same shape as the prototype → MVP and MVP → GA votes in chapter 02. Engineering manager chairs; principal MLE or architect is the technical veto; governance peer joins if the artefact has safety implications; research representative presents; a product partner joins if the artefact lands on a user-facing feature. Each of the four dimensions passes or fails. A fail returns the artefact with a remediation list and a re-vote date — typically within the quarter.

The vote is **not** "do we like this research." It is "does this clear the gate." Research that fails the gate is still valuable; it is just not yet a committed-roadmap input.

## Owner-of-record transition

The acceptance vote formally transitions ownership. Write the transition down in three lines:

- **Before:** the research team or external lab owned the artefact. They could change it under any process they chose.
- **At acceptance:** a named engineering team member is **owner of record** for the artefact in production. Version-pinned. Source-of-truth repo transfers (or a fork is taken and the research upstream is treated as advisory).
- **After:** the research team contributes via PR into the engineering team's repo under the engineering team's review process. The ninety-day support agreement runs; research still has stewardship for new variants, but any production variant is the engineering team's.

The owner-of-record is a single human, not a team. Shared ownership with no name attached is no ownership. The owner-of-record is probably a senior IC (L30 or above on the ladder you build in `mod-102`), not the manager.

## Decommissioning path for stalled tracks

Half the work is picking up things that are ready. The other half is calling out things that are not.

Define up front, when the research track is first chartered, the conditions under which the track is decommissioned.

- **Time-boxed.** A research track has a budget — in capacity, calendar, or both. Spending that budget without clearing the acceptance gate triggers a decommissioning review.
- **Three outcomes at the decommissioning review.** Extend (the research has produced enough signal to justify more budget — name the new budget), park (the research is paused; the artefacts and findings are archived; the research team is freed for other work), or kill (the work is not continued and is not resumed later without a new charter).
- **Who decides.** The engineering manager and the research lead (or the principal MLE / architect, where there is no separate research lead) co-decide. The CAIO or equivalent arbitrates if the two disagree. A decision not made is a track that quietly consumes capacity forever.
- **Write the decision down.** A one-paragraph decommissioning memo goes in the same archive as acceptance memos. "The X track was decommissioned on date Y because conditions Z were not met; the artefacts are at path W" is sufficient.

Research teams that respect the decommissioning path trust the acceptance gate more. Research teams that have seen "the one that got away" — a track killed well — do better work next quarter, because they trust the organisation to make honest calls.

## Running the handoff in practice

Not every research input goes through the full ceremony. Right-size by shape:

- **Research artefact going into production** (the artefact will be served, called, or relied on by a production feature) — full gate.
- **Research insight going into production** (a methodology, prompt pattern, or training approach engineering will re-implement) — the engineering team owns the production implementation from day one; the "handoff" is a write-up and a joint review. No ownership transfer because engineering never ceded ownership.
- **Exploratory prototype in discovery capacity** — no gate. The prototype is in the discovery line (chapter 05) and does not touch production.

Avoid the gate for the second and third categories; it creates friction with no safety benefit.

## Where the external research surface sits

If your organisation is partnered with an external research lab or ingesting frontier-lab preview capabilities, the contract is denser:

- The acceptance gate's reproducibility criterion is often harder to clear; some external artefacts (particularly frontier-model previews) are not available to self-host. The gate still applies, but the "reproduction" standard is "the engineering team can produce the eval numbers using the vendor's API" rather than "the engineering team can run the model locally."
- Documentation is almost always a model card / system card published by the vendor. Read it. Cite it in the acceptance memo.
- The on-call piece is replaced by a **vendor support contract** with a stated response-time SLA, escalation path, and named account contact.
- Decommissioning is often forced on you by the vendor (deprecation; see chapter 03). The decommissioning path pre-plans for that.

The shape of the contract is the same; the enforcement mechanism is different.

## A worked acceptance: a fine-tuned judge (hypothetical)

Continuing the running example. The research function has fine-tuned a judge model for the doc-Q&A agent's retrieval-grading step and wants to hand it to engineering.

- **Reproducibility.** Research publishes a commit SHA and a one-command script that reproduces the headline score within a 1-point tolerance on the engineering team's hardware. The judge's model checkpoint is uploaded to an internal model registry. — pass.
- **Eval scope.** Judge evaluated on research-team benchmark **and** engineering team's suite-B judge-grade benchmark. Weak spot (longer than 8k-token retrieval contexts) is disclosed. Production-distribution eval is committed for the first two weeks after handoff. — pass, with the committed production-distribution eval as a dated follow-up.
- **Documentation.** Model card published with intended use, data provenance, limitations, safety notes. — pass.
- **On-call.** Named research team engineer committed to secondary pager for 90 days. Retrospective window scheduled at day 90. Decommissioning path documented (if the judge regresses on production distribution by more than X points, roll back to the prior judge and re-open the acceptance vote). — pass.

Acceptance memo filed. Owner of record: `J. Park` (senior engineer on the AI engineering team). Research team moves onto the next track. The engineering team adds a line to the quarterly roadmap for "judge integration + 90-day retrospective" and sizes it in the capacity plan.

## What good looks like

- Each research-to-production handoff ships with a one-page acceptance memo on file.
- Every production-serving artefact has a named human owner-of-record.
- Research tracks that do not clear the gate inside their budget get a decommissioning memo. Nothing lingers.
- Research and engineering share the ninety-day retrospective ritual; both sides learn from it.
- The engineering team never debugs an artefact it has no on-call support agreement for.

## What bad looks like

- Research hands over a notebook; no memo; no owner-of-record; no eval scope beyond the research team's benchmark.
- A stalled research track that was "almost ready" six quarters ago still sits in the backlog, periodically asked after by leadership, never closed.
- Engineering pages research at 2am during a production incident and gets a bounce reply — there is no written agreement on who takes the page.
- Decommissioning is treated as a negative event rather than a calibration input.

## Summary

- The handoff is a contract with four acceptance dimensions: reproducibility, eval scope, documentation, on-call readiness.
- The acceptance vote is chaired like the promotion gates in chapter 02, with an explicit pass / fail on each dimension.
- Ownership transfers to a single named owner-of-record on the engineering team at acceptance.
- Research tracks have a decommissioning path, time-boxed and co-decided. Nothing lingers quietly.
- Right-size the gate by shape; apply it to artefacts going to production, not to research insights or discovery prototypes.
- External research surfaces follow the same contract shape with vendor-specific enforcement (model card + support contract + vendor deprecation calendar).

The next chapter zooms back out to the capacity-mix question that every chapter in this module has touched: how much of the team's time should be delivery, discovery, and incident / regression, and how do you defend the mix to leadership.
