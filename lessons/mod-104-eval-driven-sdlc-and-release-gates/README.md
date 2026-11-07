# mod-104: Eval-Driven Delivery and the Eval-Gated SDLC

> Fourth module of the AI Engineering Manager track. Builds the manager-owned operating system for the eval-gated SDLC: a two-tier release-gate policy that CI can actually enforce, the three versioned registries (prompts, judges, golden datasets) that make eval history readable across time, the code-review discipline that ties every prompt / judge / trace / dataset change to its review (R12), the five-stage offline + online eval cadence with the four-shape drift taxonomy, and the data flywheel that funds and feeds all of the above (R13). The IC craft of writing evals and judges defers to `ai-eval-engineer` (level 30) and `applied-ai-engineer` (level 30); this module teaches the system you require around that craft.

**Estimated effort:** 20 hours · **Role level:** 45 (AI Engineering family)

## Learning objectives

By the end of this module you should be able to:

- Commission an eval suite for a concrete AI product (not build it IC-level) and decide which evals gate PRs vs. which gate releases — grounded in NIST GenAI Profile, OWASP Top 10 for LLM Applications, and the current tooling landscape (`OpenAI Evals`, `Inspect`, `promptfoo`, `Ragas`, `DeepEval`, `LangSmith`, `Langfuse`, `Braintrust`, `Arize`).
- Operate the prompt registry, the judge registry, and the golden-dataset registry as first-class artefacts with versioning and evaluation history — and set the standard for what a PR must touch when it changes a prompt, a judge rubric, or a tool schema.
- Design the offline + online eval cadence — pre-merge, canary, A/B, shadow, continuous production monitoring — set eval-delta thresholds that gate deploy and that page on-call, and distinguish generation-quality drift from judge drift from retrieval drift from tool-use drift.
- Own the data flywheel (R13) — how human labelling / preference / red-team-example data gets collected, versioned, consumed by eval and fine-tune; where the labelling budget comes from; who owns rater calibration.
- Operate prompt + trace + judge review as code-review (R12) — minimum review set per change type, stakeholder sign-offs, release-note content for prompt changes. The manager's job is to require the discipline; the IC craft defers to `ai-eval-engineer` and `applied-ai-engineer`.

## Chapters

1. [The Manager's Altitude for Eval-Driven Delivery](01-the-managers-altitude-for-eval-driven-delivery.md) — what the manager owns (release-gate policy, three registries, eval cadence, data flywheel) vs. what the IC owns; the shape of the module.
2. [Commissioning an Eval Suite](02-commissioning-an-eval-suite.md) — the eight-section commissioning brief; the six eval categories; the governance pins (NIST GenAI Profile + OWASP LLM Top 10); the tooling landscape at the depth a commissioning manager needs.
3. [Deciding PR Gates vs. Release Gates](03-deciding-pr-gates-vs-release-gates.md) — the two-tier architecture; what belongs in each tier; threshold anatomy with regression allowances and absolute floors; the override policy that keeps the gate believable.
4. [The Three Registries — Prompts, Judges, Golden Datasets](04-prompt-judge-dataset-registries.md) — what each registry contains; what a PR must touch when it changes one; the holdout discipline, judge-stability thresholds, and the one-artefact-per-PR rule.
5. [Prompt, Trace, and Judge Review as Code Review (R12)](05-prompt-trace-judge-review-as-code-review.md) — the five AI-side change types and their minimum-reviewer rules; the review checklist per type; stakeholder sign-offs; release-note content; the four anti-patterns to name and defeat.
6. [Offline and Online Eval Cadence](06-offline-and-online-eval-cadence.md) — the five stages (pre-merge, canary, shadow, A/B, continuous monitoring); the four drift shapes (generation / judge / retrieval / tool-use); deploy-gating vs. pager-firing thresholds; sampling and cost.
7. [Owning the Data Flywheel (R13)](07-owning-the-data-flywheel.md) — the four stations (Capture → Label → Version → Consume); rater calibration as a first-class discipline; the labelling budget; the red-team obligation; the quarterly review.

## Exercises

- [Exercise 01 — Commission an eval suite for one AI feature](exercises/exercise-01-commission-an-eval-suite-for-one-ai-feature.md) *(4 h)*
- [Exercise 02 — PR vs. release gate eval policy](exercises/exercise-02-pr-vs-release-gate-eval-policy.md) *(3 h)*
- [Exercise 03 — Prompt and judge registry standards](exercises/exercise-03-prompt-and-judge-registry-standards.md) *(3 h)*
- [Exercise 04 — Offline and online eval cadence design](exercises/exercise-04-offline-and-online-eval-cadence-design.md) *(4 h)*
- [Exercise 05 — Data flywheel plan and budget](exercises/exercise-05-data-flywheel-plan-and-budget.md) *(4 h)*

Each exercise ships its prompt here; reference solutions live in the paired [`ai-engineering-manager-solutions`](https://github.com/ai-engineering-curriculum/ai-engineering-manager-solutions) repo.

## Supporting material

- `labs/` — long-form hands-on labs (planned).
- `quizzes/` — knowledge checks (planned).
- [`resources.md`](resources.md) — curated external references used across chapters.

## Where this module fits

- **Builds on mod-101, mod-102, and mod-103.** The role matrix from mod-102 is where the `ai-eval-engineer` / `applied-ai-engineer` / `rag-engineer` / `agentic-ai-engineer` roles this module hands off to are defined. The promotion-vote mechanics from mod-103 chapter 02 depend on the artefacts this module produces — the MVP → GA gate is enforced by the release-gate policy chapter 03 teaches you to write.
- **Feeds mod-106 (safety and the release triad).** The release-gate policy here is one leg of the release triad; the safety case families this module commissions are the measurement surface mod-106's red-team and safety programs produce evidence for.
- **Feeds mod-107 (AI incident response).** The paging thresholds and the four-shape drift-triage runbooks chapter 06 produces are mod-107's day-one playbooks. The release-note content required by chapter 05's R12 is the single artefact an on-call engineer reads during triage.
- **Pairs with mod-110 (budget, FinOps, build-vs-buy).** The labelling budget from chapter 07 and the release-gate run cost from chapter 03 are line items in the quarterly plan mod-110 teaches you to defend.
- **Pairs with mod-109 (team shape and operating cadence).** The quarterly flywheel review (chapter 07) and the quarterly override-log review (chapter 03) are beats on the operating cadence mod-109 designs.
- **Pairs with mod-105 (vendor and model portfolio).** The model-change sign-off surface from chapter 05 draws on mod-105's portfolio inventory; a model swap that touches a gate-critical surface is a mod-104+105 collaboration.
- **Pairs with mod-111 (cross-functional partnership).** The release-note content for prompt / judge / tool / dataset changes (chapter 05) is the evidence mod-111's governance, security, and legal partners ask for.
- **Anchors project 102.** This module is the primary source for [`project-102-eval-gated-sdlc-for-one-ai-product`](../../projects/project-102-eval-gated-sdlc-for-one-ai-product), together with mod-103 (promotion-vote discipline), mod-106 (release triad), and mod-107 (AI incident response).
