# The AI Engineering Role Matrix

The hardest hiring mistake an AI Engineering Manager can make is to post a role called "AI Engineer" and run one loop against it. The title covers at least seven distinguishable IC shapes in the current market, each with its own daily work, its own evidence of competence, and its own pipeline. This chapter sorts them into a usable matrix, specialises the three public leveling exemplars this track leans on (Dropbox, Google, GitLab), and names the ML- and infra-adjacent peers who are often confused with AI engineers but hire against a different ladder.

## Motivation

The symptom of a missing role matrix is interchangeable JDs. The same bullet list — "ship LLM features, build RAG pipelines, use evals, work with product" — gets posted for four different openings, four different loops hit it, and four different offers are extended against four different unwritten rubrics. The visible failures:

- An offer goes to a candidate who writes beautiful prompts but has never shipped a judge, and the team's eval coverage stops growing on their quarter.
- A fine-tuning-shaped role is filled with an application developer who has never read a loss curve, and the next fine-tune slips by six weeks because no one can tell whether the run converged.
- A "senior AI engineer" is hired against an unwritten agentic-systems rubric, arrives, and discovers the team only ships one-shot LLM calls — they disengage within a quarter.

The matrix below is the antidote. It is not a vocabulary you invent and then force on candidates; it is a vocabulary that already exists in the IC ladders this track links out to. Your job as the hiring manager is to **name** which shape you are hiring, write the JD against the artefacts that shape actually owns, and run the loop against the rubric that shape's level demands. Everything downstream — the JD (chapter 02), the loop (chapter 03), the calibration (chapter 04), the sourcing plan (chapter 05) — assumes the matrix is in place.

## The seven AI-engineering IC shapes

Each row below names one IC shape: what they ship, what they own that no other shape does, and where the craft depth is taught in the paired IC track. The level numbers match the AI Engineering family on the ladder used by this curriculum. For every shape, the IC curriculum is the authoritative source on craft — your matrix just has to pick the shape and level; depth belongs to the IC track.

### 1. `llm-application-developer` — L20 (on-ramp)

The on-ramp role. Ships LLM-powered features under more senior guidance. Owns the first integration pass: wire an API call, render a response, handle the obvious error path.

- **Primary artefacts:** small-to-medium feature PRs; a prompt they registered; a trace sample they attached to a PR; a basic eval case they wrote; a bug report with a reproducing trace.
- **What only they own:** nothing architecture-defining. The point of the level is to graduate into applied or a specialist track.
- **Confused with:** applied-AI engineer (L30). The telling tell is unassisted architectural judgment — can they say *why* the prompt is structured this way, not just that it works.
- **IC track:** [`llm-application-developer`](https://github.com/ai-engineering-curriculum/llm-application-developer-learning).

### 2. `applied-ai-engineer` — L30 (the generalist)

The default shape on most AI engineering teams. Ships end-to-end AI features — from product code through prompt, context, model, output, back to product code. Reaches into retrieval, evals, and tool-use when the feature needs it; defers to specialists when it needs depth.

- **Primary artefacts:** shipped features with eval results, trace samples, a cost-per-call number, a readiness read against the release triad.
- **What only they own:** the **integration craft** — turning product requirements into a non-deterministic feature that meets a quality bar, inside a real codebase, on real traffic.
- **Confused with:** full-stack engineer. The applied-AI engineer carries the eval and trace muscle; a full-stack engineer who has written one prompt does not.
- **IC track:** [`applied-ai-engineer`](https://github.com/ai-engineering-curriculum/applied-ai-engineer-learning).

### 3. `agentic-ai-engineer` — L30 (agent specialist)

Specialises in multi-step agent and tool-use systems: planning, tool contracts, state management across turns, failure-mode maps, human-in-the-loop checkpoints.

- **Primary artefacts:** a working agent with tool-invocation traces; a failure-mode map; an authorisation posture at the product boundary; a documented kill-switch surface; a HITL policy.
- **What only they own:** **multi-step reasoning under failure**. One-shot LLM features are not this role.
- **Confused with:** applied-AI engineer who has shipped a function call. The agentic engineer can walk you through what happens when the tool fails on turn three of a seven-turn trace, and why their agent still converges.
- **IC track:** [`agentic-ai-engineer`](https://github.com/ai-engineering-curriculum/agentic-ai-engineer-learning).

### 4. `senior-agentic-ai-engineer` — L40 (agent tech-lead)

The tech-lead level for agent-shaped teams. Carries the design judgment, the kill-switch discipline, and the mentorship scope the L30 does not.

- **Primary artefacts:** architecture decision records for the agent; a cross-team tool-contract standard; a failure-taxonomy document; a mentorship record (how many L20/L30 engineers they have pulled up); on-call leadership for agent incidents.
- **What only they own:** **agent-system judgment at team scale** — when to add a sub-agent vs. a tool, when to shorten the planning loop, when to require a HITL gate, when to ship vs. when to hold.
- **Confused with:** staff-plus architect (L48). The senior agentic engineer is tech-lead altitude for *one team's* agent; the architect sets direction across teams.
- **IC track:** [`senior-agentic-ai-engineer`](https://github.com/ai-engineering-curriculum/senior-agentic-ai-engineer-learning).

### 5. `ai-eval-engineer` — L30 (eval specialist)

Specialises in evaluation craft: golden-dataset design, LLM-as-judge design, rubric judges, human-review workflows, the harness that runs them.

- **Primary artefacts:** a registered eval suite runnable in CI; a judge-case corpus with change history; a labelling workflow; a documented methodology defending judge choice; an eval-delta report against the current release.
- **What only they own:** the **measurement contract**. If you cannot defend what "passing" means for your product, this role is missing.
- **Confused with:** QA engineer. QA ships pass/fail on deterministic behaviour; eval engineers ship defensible thresholds on probability distributions.
- **IC track:** [`ai-eval-engineer`](https://github.com/ai-engineering-curriculum/ai-eval-engineer-learning).

### 6. `rag-engineer` — L30 (retrieval specialist)

Specialises in retrieval-augmented systems: chunking, embeddings, index choice, re-ranking, retrieval-side evals, freshness and corpus hygiene.

- **Primary artefacts:** a retrieval pipeline with coverage, latency, and recall/precision measurements on the product's golden set; a corpus contract (what's in, what's out, how it refreshes); a freshness SLO; a re-ranker rationale.
- **What only they own:** **retrieval quality as a first-class measurement**. Many teams treat retrieval as "the index returns something" and discover, late, that the LLM was hiding retrieval failures by confabulating. This role closes that gap.
- **Confused with:** applied-AI engineer who has used a vector store. The RAG specialist can walk you through chunking choices and the recall/precision trade-off on *this* corpus; the applied engineer defers that depth.
- **IC track:** [`rag-engineer`](https://github.com/ai-engineering-curriculum/rag-engineer-learning).

### 7. `fine-tuning-engineer` — L30 (post-training specialist)

Specialises in post-training on top of a base model: supervised fine-tuning, preference data, PEFT/LoRA, evaluation of the resulting checkpoint, deployment handoff.

- **Primary artefacts:** a trained checkpoint; a model-card-shaped documentation page (per Mitchell et al. 2019); eval results against baselines and the vendor-API alternative; a training-data contract (origin, license, PII posture); a cost/latency read on serving the fine-tune vs. buying inference.
- **What only they own:** the **post-training craft** — when a fine-tune is worth doing, how to sample data, when LoRA is enough vs. full SFT, how to read a loss curve, when to stop.
- **Confused with:** MLE. The fine-tuning engineer specialises in LLM post-training, often on vendor or open-weights base models; a classical MLE may never touch an LLM.
- **IC track:** [`fine-tuning-engineer`](https://github.com/ai-engineering-curriculum/fine-tuning-engineer-learning).

## The ML- and infra-adjacent peer roles

Three more roles overlap enough on the résumé to show up in the same inbox, and should be hired against a different ladder.

### `principal-ml-engineer` — L48, ML Engineering family

ML-methodology direction at staff-plus altitude: model-behaviour analysis, training/evaluation methodology, ML-system design, deep eval-design judgment. On a team doing meaningful fine-tuning, preference data, RLHF, or distillation, the principal MLE is the partner the agent-architect alone cannot replace.

- **Hire against the ML Engineering ladder, not this one.** The IC track is [`principal-ml-engineer`](https://github.com/ml-engineering-curriculum/principal-ml-engineer-learning).
- **You partner with them,** you do not run their hiring loop in this track.

### `staff-ml-engineer` / classical MLE — L40 adjacent, ML Engineering family

Classical MLOps and ML-system engineering: training platform, feature stores, non-LLM model deployment, pre-LLM evaluation methodology. Many "AI Engineer" résumés arrive from here; good adjacent-pipeline candidates (chapter 05) often come from it too.

- **Hire against the ML Engineering ladder when the job is classical ML.** Hire against the AI Engineering ladder (applied, eval, fine-tune) when the job is LLM-first and the candidate has done the conversion.
- The IC track is [`staff-ml-engineer`](https://github.com/ml-engineering-curriculum/staff-ml-engineer-learning).

### `ai-infra-team-lead` and the AI infra ICs — AI Infra family

Serving, GPU capacity, inference infrastructure, training platform. Hired against the AI Infra family, not this one.

- **The giveaway:** the role's primary artefact is a serving SLO, not an eval delta. If the inbox has a résumé of mostly Kubernetes, Triton, vLLM, inference benchmarks, and GPU scheduling — route to the infra track.
- Peer manager is [`ai-infra-team-lead`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning) (L45, peer of this role, covered in mod-101 chapter 01).

## Grounding the matrix in public leveling exemplars

This track links to three public leveling frameworks and specialises them for AI engineering. You should be able to point at each and say *this is where my matrix is anchored*; it keeps the leveling language transparent, which chapter 04 treats as a first-order calibration rule.

### Dropbox Engineering Career Framework

The [Dropbox framework](https://dropbox.github.io/dbx-career-framework/) is the most useful public template because it enumerates expectations across **dimensions** (project management, technical skills, people, influence, feedback) at every level. Its IC2 → IC5 progression is close to the L20 → L48 arc in this track. Adaptation for AI engineering:

- **"Technical skills"** at L30 reads differently for applied vs. eval vs. RAG vs. fine-tune — specialise the row.
- **"Influence"** at L40 for `senior-agentic-ai-engineer` is partly measured in **mentorship across the AI sub-ladders** — how many L20/L30 engineers across adjacent shapes have they pulled up, not just how many of their own shape.
- **"Project management"** is where AI-specific scope attaches: confidence-banded estimation, kill-switch design, eval-fixture discipline (modules 103, 104, 107).

### Google engineering levels

The [levels.fyi Google SWE explainer](https://www.levels.fyi/blog/google-swe-levels-explained.html) describes a public-facing view of the L3 → L8 ladder. Two useful borrowings:

- **L3 vs. L4 vs. L5:** matches this track's L20 (on-ramp, needs guidance) → L30 (independent in-scope) → L40 (tech-lead, cross-scope influence) arc.
- **The idea that each level has a clear statement of "scope you own alone."** For this matrix, "scope you own alone" is written as the primary-artefact bullet in each role row above. Keep it that short.

### GitLab Engineering handbook

The [GitLab engineering handbook](https://handbook.gitlab.com/handbook/engineering/) is useful not because its levels match (they are company-specific) but because its **handbook culture** publishes JDs, role expectations, and promotion criteria openly. Two things to borrow:

- The practice of maintaining role expectations as a **living artefact** you can link in a JD, not a secret internal document.
- The habit of writing competency expectations as **behaviours**, not years of experience. ("Owns incident response for the team's primary agent" vs. "5+ years of AI experience.") Chapter 02 builds the whole JD approach on that habit.

## A concrete read: four openings, four shapes

A team of eight has four open headcount approved. The manager has drafted "AI Engineer × 4" in the applicant tracking system. Applying the matrix:

- **Opening 1 — ship a new document-analysis agent in Q2.** The dominant work is multi-step planning, tool use, and HITL. Shape: `agentic-ai-engineer` (L30) or `senior-agentic-ai-engineer` (L40) depending on whether tech-lead scope is needed.
- **Opening 2 — unblock the eval backlog; the team has 40 PRs waiting on regression-eval work.** Shape: `ai-eval-engineer` (L30). Not an applied engineer.
- **Opening 3 — the knowledge-base retrieval is flaky, precision is bad and nobody owns the corpus contract.** Shape: `rag-engineer` (L30). An applied engineer will patch; a RAG engineer will fix the measurement and the contract.
- **Opening 4 — the team ships one more LLM feature per quarter than it can.** Shape: `applied-ai-engineer` (L30). The role is end-to-end integration throughput.

Those are four JDs, four loops, four pipelines. One of them might legitimately be filled by a dual-specialist candidate — you may hire one RAG-plus-eval engineer, or one applied engineer growing into agentic. But you cannot hire the matrix away by posting a generic title. The first act of hiring is to pick the shape.

## Summary

- "AI engineer" is at least seven distinguishable IC shapes across L20 → L40 in this track: `llm-application-developer`, `applied-ai-engineer`, `agentic-ai-engineer`, `senior-agentic-ai-engineer`, `ai-eval-engineer`, `rag-engineer`, `fine-tuning-engineer`.
- Three ML/infra-adjacent roles — `principal-ml-engineer`, classical `staff-ml-engineer`, and the AI Infra family — are hired against different ladders; your matrix routes to them.
- Public exemplars (Dropbox, Google, GitLab) are the anchor you specialise, not re-invent. Keep the language public.
- Every row in the matrix lists a **primary artefact** — what the role actually produces that no other row does. That artefact is what the JD (chapter 02) will name and the loop (chapter 03) will measure.

The next chapter turns each matrix row into a JD built from artefacts, not from years-of-experience fiction.
