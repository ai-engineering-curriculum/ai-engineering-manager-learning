# The Operating Contract: Canon + AI-Specific Scope

Every manager works under an implicit contract: *here is what the business expects of you, here is what your team expects of you, here is what your peers expect of you*. For a level-45 AI Engineering Manager, that contract is the **union** of two surfaces: general engineering-management canon, and the AI-engineering-specific scope that sits on top of it. This chapter names both halves and says what shows up, concretely, when you accept the role.

## Motivation

New AI Engineering Managers tend to make one of two symmetric mistakes:

- **Treat the role as "engineering manager + AI words."** They apply canonical EM moves (1:1 cadence, feedback, hiring loop, OKR mechanics) and expect the AI-specific risk to be absorbed by someone else. It is not. There is no governance team quietly running your release gate, no infra team handling your hallucination incidents.
- **Treat the role as "all AI, no management."** They spend every hour on eval reviews, vendor calls, and incident command, and let the foundational EM practice slide. 1:1s get skipped, performance signals go un-recorded, feedback is only given at the quarterly review. The team becomes unmanaged in the old-fashioned way.

The contract is **both halves, in full**. Canon is the floor; AI-specific scope is the layer on top. Skip either, and the role fails.

## Half one: general engineering-management canon

This half is **assumed, not re-taught.** The references below carry the material; the prerequisite is that you have read them or the equivalent before taking a level-45 role.

### Core canonical reading

- **Fournier, *The Manager's Path*** — the progression from tech-lead → EM → director, and what the 1:1, feedback, and delegation moves look like at each altitude. [O'Reilly link](https://www.oreilly.com/library/view/the-managers-path/9781491973882/)
- **Grove, *High Output Management*** — managerial output as the output of the team and all teams influenced by the manager; meetings as production processes; the task-relevant maturity model for delegation. [Publisher page](https://www.harpercollins.com/products/high-output-management-andrew-s-grove)
- **Larson, *An Elegant Puzzle* + *Staff Engineer*** — org design, systems thinking about management work, and the staff-plus IC altitude your L48 partners occupy. [An Elegant Puzzle](https://lethain.com/elegant-puzzle/) · [Staff Engineer](https://staffeng.com/book)
- **Lopp (Rands in Repose)** — the lived-practice vocabulary of 1:1s, team dynamics, and the cultural operating system of an engineering group. [Rands in Repose archive](https://randsinrepose.com/archives/category/management/)
- **Google re:Work + Project Oxygen** — the empirical findings on what makes an effective manager (coach, empower, inclusive, results-oriented, communicator, career supporter, strategist, technical, collaborator, decisive) and the practices behind them. [re:Work guides](https://rework.withgoogle.com/en/guides) · [Project Oxygen](https://rework.withgoogle.com/en/guides/managers-identify-what-makes-a-great-manager)

### What this half includes

- Running 1:1s with intent (agenda ownership, career conversations, coaching vs. status).
- Giving feedback close to the event; using written performance narratives with specific examples.
- Running a hiring loop: scorecards, calibration, references, closing.
- OKR / planning mechanics: picking fewer goals than you think, writing measurable outcomes, de-committing honestly when the plan is off.
- Delegation calibrated to task-relevant maturity; stepping back on tasks the team has mastered, stepping in on tasks where the team is new.
- Writing the operating cadence: standups, reviews, planning, retros, with a clear purpose for each.

If any of this is new, pause and work through `ai-augmented-team-manager-learning` (level 40, AI Workforce family) first. The prerequisite is not ceremonial.

## Half two: AI-engineering-specific scope

This is the half that makes the role different, and the half the canon does not cover. Each item below is owned by you, is covered in depth in a later module, and is named here so you can see the whole surface at once.

### 1. Non-deterministic delivery

Your team ships features whose output is a probability distribution, not a value. The canonical "due Friday" promise does not apply the way it does in a deterministic backend project. Your contract:

- Roadmaps carry **confidence bands**, not point ETAs.
- Plans assume mid-quarter capability shifts from the model provider.
- The prototype → MVP → GA promotion gate is evidence-based (eval delta, user signal, cost ceiling, safety floor), not calendar-based.

Module 103 covers the mechanics. Here the point is: *every planning move inherits this.*

### 2. Eval-driven delivery and the eval-gated SDLC

Your team ships code *and* prompts *and* judges *and* golden datasets. The release gate is not "tests pass" — it is "evals pass with acceptable delta." Your contract:

- Prompt + trace + judge review is a code-review requirement, not an optional check.
- A registered judge + eval suite gates PRs vs. releases.
- Eval deltas page on-call, not just dashboards.
- The data flywheel — labelling, preference data, red-team data — is a funded program, not a side-of-desk hope.

Grounded in NIST AI RMF (and its GenAI Profile) and OWASP Top 10 for LLM Applications, with tooling patterns from OpenAI Evals, Inspect, promptfoo, Ragas, DeepEval, LangSmith, Langfuse, Braintrust, and Arize Phoenix. Module 104 covers this in depth.

### 3. The model + data + safety release triad

Every release has to clear three independent gates:

- **Model** — does this model, at this version, on this task, meet the quality bar?
- **Data** — is the retrieval / context / fine-tune data safe to use, properly sourced, and documented?
- **Safety** — are the guardrails (input filters, output validators, tool-invocation policy, HITL checkpoints, refusal policy) in place and passing?

You chair the gate. You do not have to be the deepest expert on any one of the three — but you have to know which evidence (eval report, data lineage, red-team findings, model card) you need to see before you say ship. Module 106 covers the triad; modules 104 and 105 feed it.

### 4. Model and vendor portfolio

Your team almost certainly does not build its own frontier model. You buy inference. That means your contract includes a surface the canonical EM role does not touch:

- Reading **DPAs** (Data Processing Addenda) well enough to spot the clauses that matter — training-on-data, retention, residency, sub-processors — and route legal opinion to counsel.
- Maintaining a **model + provider portfolio** across the current frontier (Anthropic, OpenAI, Google, Mistral, Cohere, Meta open-weights, specialists) with named criteria: context window, pricing (input / output / cached / batch / fine-tune), latency, throughput, SLA, SOC 2, residency, enterprise features.
- Designing **fallback and multi-model routing** so the product does not hard-fail when a provider rate-limits, deprecates, or ships a regression.
- Reading provider deployment frameworks — [Anthropic RSP](https://www.anthropic.com/rsp), [OpenAI Preparedness](https://openai.com/safety/preparedness/), [Google DeepMind Frontier Safety Framework](https://deepmind.google/discover/blog/introducing-the-frontier-safety-framework/) — as planning inputs, not policy you own.

Module 105 covers the mechanics.

### 5. AI-specific incident response

The canonical SRE incident path (Google SRE Book, SRE Workbook, Etsy blameless-postmortem practice) is the floor. The AI surface on top:

- **Novel incident types:** jailbreak, prompt injection, hallucination, post-vendor-upgrade regression, judge drift, tool-use failure, embedding-store corruption, retrieval degradation, moderation-classifier false-positive storm.
- **Novel mitigations:** kill-switch at the prompt layer, route-to-safer-model fallback, prompt/judge/tool rollback separate from code rollback, context-window scrub.
- **Novel evidence:** every confirmed AI incident must become a permanent eval fixture (regression test + judge case).
- **Novel reporting lines:** where EU AI Act Article 73 applies you may owe a regulator narrative. You do not draft legal language — you route to counsel — but you own the engineering-evidence package.

Module 107 covers this in depth.

## The two halves fit together

A useful way to read the operating contract is as a two-column table. Left column is canonical EM work; right column is the AI-specific scope that attaches to it.

| Canonical practice | AI-specific attachment |
|---|---|
| 1:1 cadence, career growth | Anti-atrophy coaching; growth across AI sub-ladders (applied, agentic, eval, RAG, fine-tune) |
| Hiring loop | AI-specific interview rounds: prompt + trace + judge review, model-selection reasoning, non-deterministic debugging |
| Roadmap and planning | Confidence-banded ETAs; mid-quarter capability-shift replanning |
| Code review / release | Eval-gated SDLC; prompt + trace + judge review as PR requirement |
| Vendor management | Model + provider portfolio; DPA literacy; fallback routing |
| Incident response | AI-specific incident types; eval-fixture feedback loop |
| Performance management | Calibration under rapid tech drift; anti-atrophy signal |
| Budget / FinOps | Inference economics: caching, batch, context-window discipline |
| Executive communication | Model-uncertainty-literate roll-ups; honest demo discipline |

Nothing in the left column goes away. Everything in the right column is yours now.

## A concrete read: the Monday morning after week one

Imagine you take the role on a Monday. By the following Monday, you have run first 1:1s with every direct report, read the last three postmortems, and asked your staff IC and senior engineers for the one thing each of them wishes the previous manager had held the line on. The canon tells you how to run those conversations. The AI-specific scope tells you what to listen *for*:

- Does the postmortem list "eval fixture added" as a required action item, or is it a vague "we'll be more careful"? (Module 107.)
- Does the senior IC mention a vendor deprecation they've been nervous about for two months? (Module 105.)
- Is there a prompt registry, or are prompts sitting in a shared Google Doc? (Module 104.)
- Does the hiring doc describe level differentiation for the agentic engineer vs. the eval engineer, or are they bucketed as "AI engineers"? (Module 102.)

The canon tells you to listen. The AI-specific scope tells you what the answers mean.

## Summary

- The operating contract is the **union** of two surfaces: general engineering-management canon and AI-engineering-specific scope.
- Canon is assumed (Fournier, Grove, Larson, Lopp, Google re:Work, Project Oxygen). It is the floor.
- AI-specific scope — non-deterministic delivery, eval-gated SDLC, model + data + safety triad, model-vendor portfolio, AI-specific incident response — is new to this role and is covered by the modules 103 through 112.
- Every canonical practice has an AI-specific attachment. Nothing drops; new work adds on top.
- Skipping either half fails the role — in opposite but equally recognisable ways.

The next chapter diagrams the handoff contracts: whom you commission work from, whose evidence you consume, whom you partner with sideways, and where you route work that is *not* yours.
