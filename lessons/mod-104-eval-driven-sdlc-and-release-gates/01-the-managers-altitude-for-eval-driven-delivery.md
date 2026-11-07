# The Manager's Altitude for Eval-Driven Delivery

The eval-gated SDLC is an engineering practice. The manager's job is not to invent it, build it, or hand-craft each eval — those crafts belong to the `ai-eval-engineer` (level 30), the `applied-ai-engineer` (level 30), and, on retrieval-heavy surfaces, the `rag-engineer` (level 30). The manager's job is to **require the discipline**, **commission the artefacts**, and **set the gates** — then hold the team to them when a deadline-driven PR tries to slip the net. This module is the operating-system view of that job.

If the previous module (mod-103) taught you to publish confidence-banded roadmaps and promotion gates, this one teaches you to run the mechanism that *produces* the evidence those gates read. The promotion-vote from chapter 02 of mod-103 only works if an eval suite exists, if its history is trustworthy, and if a failing release gate actually blocks a merge. That is the surface this module covers.

## Motivation

Two shapes of team expose why the manager has to own the altitude of eval-driven delivery personally — neither failure is a tooling problem, and no amount of better evals-library adoption fixes them.

- **The "we have evals" team that is not eval-gated.** A team adopts an evals framework — `promptfoo`, `Inspect`, `OpenAI Evals`, `Ragas`, `DeepEval` — writes a dozen cases, runs them on a laptop before big releases, and calls itself eval-driven. The evals do not run in CI. They do not block anything. Prompt changes merge without touching an eval file. Judge rubrics are undated markdown. Six months in, nobody knows which eval regressed and nobody knows which prompt change caused the regression. The team has *evals*; it does not have an *eval-gated SDLC*. The manager's job is to close the gap.
- **The "eval-gated in theory" team that has been overridden to death.** A team wires up a release gate, sets a threshold, and ships. The first time the gate blocks a feature flagship ("the demo on Friday"), leadership overrides the gate. The second override is documented but unpunished. By the fourth, the gate is a joke the team no longer trusts — and the team quietly stops writing evals at the strictness the gate assumed, because why bother. The manager's job is to defend the gate — or, better, to design the gate so overrides are survivable (see chapter 03's two-tier structure) without discarding the discipline.

Both failures are governance failures, not tooling failures. The manager does not fix them by writing more evals. The manager fixes them by naming the four artefacts of the eval-gated SDLC, requiring each on every change, and defending the gates against the pressure that recurs every quarter.

## Core concept 1 — what the manager owns, versus what the IC owns

The craft is the IC's; the system is the manager's. A clean split, repeated every time the line gets blurry.

| What the IC owns (craft) | What the manager owns (system) |
|---|---|
| Writing an individual eval case, judge prompt, or rubric. | Requiring that every PR that changes a prompt also changes the eval suite or writes a signed exemption. |
| Picking between `Ragas` faithfulness and a custom rubric for this surface. | Picking **that** a faithfulness-class signal exists for every retrieval-shaped feature, and that it belongs in the release-gate tier. |
| Tuning an LLM-as-judge prompt until judge–human agreement crosses a threshold on the calibration set. | Requiring judge–human agreement **as a published metric**; setting the minimum acceptable agreement before a judge may serve as a gate; naming the quarterly judge-recalibration ritual. |
| Instrumenting a trace, picking OpenTelemetry semantic-convention fields, choosing between LangSmith / Langfuse / Braintrust / Arize Phoenix for tracing storage. | Requiring that **every** production call emit a trace, that traces are retained to the retention window the privacy posture allows, and that at least a sampled fraction gets a judgment every day. |
| Labelling a trace, writing a rater guideline, pulling a preference pair. | Requiring that the **labelling pipeline exists**, naming the budget line, picking the owner-of-record for rater calibration (chapter 07). |

The split survives contact with reality because the manager's items are all *process* items — they do not require the manager to write a single eval line. The IC's items are all *craft* items that require level-30 competence this role hires rather than performs.

When the manager drifts into the IC column, the team feels undermined; when the manager neglects the system column, the team feels unsupported. The healthy pattern is: manager sets a non-negotiable shape of the system, IC fills it with craft appropriate to the surface, manager reviews the shape every quarter and tightens it when evidence says the current shape is loose.

## Core concept 2 — the four manager-owned artefacts

Every module in the eval-driven SDLC composes the same four artefacts. Memorise the list; this module's chapters are organised around it.

1. **The release-gate policy (chapter 03).** A written policy that names which evals gate PRs, which gate releases, what the thresholds are, what counts as a PR-blocker vs. a release-blocker, who can override, and the cost of an override. Without this, "we have evals" cannot become "we have a gate."
2. **The three registries — prompt, judge, golden-dataset (chapter 04).** The versioned sources of truth for every prompt the system runs, every judge rubric that produces an eval score, and every golden-dataset split the evals read. Each entry carries version, authorship, evaluation history, and the identity of the review that promoted it.
3. **The eval cadence — offline + online (chapter 06).** Pre-merge offline evals, canary / shadow / A/B in production, continuous production monitoring. Different cadences for different latencies and different purposes; a single document names them and who runs each.
4. **The data flywheel (chapter 07).** The pipeline that turns production traces, red-team examples, and user feedback into labelled data, which feeds evals and (when it exists) fine-tuning. Includes budget, labelling platform, and the rater-calibration ritual.

A team is **eval-gated** when all four exist in writing, are referenced from every relevant PR template, and the quarterly review happens. A team with two of the four is a team that will one day land a bad release and discover there was no mechanism to catch it.

## Core concept 3 — who defers to whom, and why that matters for your reading

This track is deliberately a *leading-and-commissioning* track, not a *building* track. Three specific deferrals, and why you should respect them while reading the module.

- **Eval design depth** — the craft of writing a golden dataset, choosing a judge, tuning a rubric, measuring judge–human agreement, running a holdout, computing a bootstrap confidence interval on an eval delta — lives in [`ai-eval-engineer`](https://github.com/ai-engineering-curriculum/ai-eval-engineer-learning). You will read this module and still not know how to design a faithfulness judge by yourself. That is the intended outcome.
- **Application-side integration craft** — the craft of wiring an eval runner into CI, propagating traces, instrumenting the application, surfacing eval deltas on a PR — lives in [`applied-ai-engineer`](https://github.com/ai-engineering-curriculum/applied-ai-engineer-learning).
- **Retrieval-side evals** — recall, precision, re-ranker ablations, retrieval coverage against a corpus — live in [`rag-engineer`](https://github.com/ai-engineering-curriculum/rag-engineer-learning). The manager calls out that a retrieval signal must exist; the `rag-engineer` picks which.

What the manager must know at depth: the *categories* of eval (behavioural, judge-based, retrieval-side, safety, cost / latency), the *vocabulary* of each eval tool (enough to read a proposal and push back when a choice is wrong for the surface), the *governance frames* (NIST GenAI Profile, OWASP LLM Top 10) that pin the safety floor, and the *mechanics of a gate* (pre-merge vs. canary vs. shadow vs. continuous monitoring). The IC curricula teach the craft; this module teaches the operating discipline that hires and reviews the craft.

## The shape of the chapters ahead

- **Chapter 02 — Commissioning an eval suite.** The brief the manager writes when they ask an eval engineer to build the suite for a new feature: product surface, baseline, risk posture (NIST / OWASP mapping), tool choice, acceptance thresholds. The chapter walks the current tooling landscape only to the depth a commissioning manager needs.
- **Chapter 03 — Deciding PR gates vs. release gates.** The two-tier gate. What goes in each tier, how thresholds are chosen, how overrides work, and why a single-tier gate fails under production pressure.
- **Chapter 04 — The three registries.** Prompts, judges, golden datasets as versioned first-class artefacts with evaluation history. What a PR must touch when it changes a prompt, a judge rubric, or a tool schema. The release-note content that must accompany each kind of change.
- **Chapter 05 — Prompt + trace + judge review as code review (R12).** The minimum review set per change type. Reviewer competencies. Stakeholder sign-offs. The anti-patterns that defeat code review on non-deterministic systems.
- **Chapter 06 — Offline and online eval cadence.** Pre-merge, canary, A/B, shadow, continuous production monitoring. Eval-delta thresholds that gate deploy and those that page on-call. The four drifts — generation-quality, judge, retrieval, tool-use — and the signals that distinguish them.
- **Chapter 07 — Owning the data flywheel (R13).** Human labelling, preference pairs, red-team examples. Versioning. Budget. Rater calibration. How eval data feeds fine-tuning when fine-tuning exists.

The exercises paired to the chapters ask you to produce the manager-owned artefact for a real feature of your choice. By the end of the module, you will have a release-gate policy, a registry standard, an eval cadence, and a data-flywheel plan that are specific to one product surface — and a template-shape generic enough to apply to the next one.

## What good looks like

A team is eval-gated — at the manager's altitude — when:

- A new engineer can open the release-gate policy on their first day, find the thresholds for PR vs. release, and know without asking which of their changes will trigger which gate.
- A prompt change PR that forgets to update the eval suite fails CI with a message pointing at the registry standard (chapter 04), and nobody asks the manager for a one-off exemption.
- An LLM-as-judge judging gate-critical evals has a published judge–human agreement score (chapter 02), a last-recalibrated date within the policy window (chapter 07), and a dated rater-guidelines document.
- A production monitor for generation drift wakes on-call before a user does, and the triage runbook distinguishes the four drift shapes (chapter 06) without the manager in the room.
- The labelling budget is a line item in the quarterly plan, with a named owner, and the data-flywheel review is a thirty-minute standing meeting (chapter 07) that produces deltas against the prior quarter.

## What bad looks like

- Evals exist but do not run in CI; or run in CI but never fail; or fail in CI but every failure is overridden "for the demo."
- Prompts live in application code as string literals; the registry is "whatever is at `HEAD`."
- Judges live in a Google Doc somebody wrote before the team was formed. No last-recalibrated date. No judge–human agreement number.
- Traces are instrumented for one product surface and not another, so the online eval cadence has blind spots nobody has flagged.
- Labelling is done ad hoc, by whoever has a slow afternoon. No rater calibration. Preference pairs collected but never versioned, and never consumed by an eval. Fine-tuning, when it happens, is gated on data of unknown provenance.

## Summary

- The craft of evals is an IC craft; the eval-gated SDLC is a manager-owned system. Separate them explicitly.
- The manager owns four artefacts: the release-gate policy, the three registries (prompts / judges / golden datasets), the offline + online eval cadence, and the data flywheel.
- Depth on eval-engineering craft defers to `ai-eval-engineer`; application-integration craft defers to `applied-ai-engineer`; retrieval-side craft defers to `rag-engineer`. The manager reads enough of each to commission and review, not to build.
- The rest of the module walks each manager-owned artefact in turn, with an exercise per chapter that fills it in for a real feature.

The next chapter picks up the first artefact: how a manager writes the brief that commissions an eval suite in the first place, and how they decide between the half-dozen eval tools the team could adopt.
