# Commissioning an Eval Suite

When a team starts work on a new AI feature — a doc-Q&A agent, a tool-using assistant, a code reviewer, a summariser — the eval suite does not exist yet. The IC eval engineer will build it. The manager's job is to write the brief that commissions the build: what the suite is for, what the baseline is, what categories of eval it must cover, what tooling it should use, and what thresholds it will serve. A good brief turns a two-week eval build into a one-week eval build; a bad brief turns it into a four-week rewrite three months later when the first regression exposes the gaps.

This chapter is a template for that brief, specialised to the shape of an LLM-based product and grounded in the public frameworks (NIST GenAI Profile, OWASP LLM Top 10) and tooling (`OpenAI Evals`, `Inspect`, `promptfoo`, `Ragas`, `DeepEval`, `LangSmith`, `Langfuse`, `Braintrust`, `Arize`) that a manager must be able to read fluently.

## Motivation

Two failures recur when eval suites are commissioned without a written brief.

- **The grab-bag suite.** The eval engineer writes what they know how to write — usually a dozen behavioural cases and a faithfulness judge — and calls the suite done. The suite does not cover the surface the product actually ships on. Six months later, the first real regression comes in and the suite catches nothing, because nobody asked whether safety, cost, latency, tool-use correctness, or long-input stability had to be covered.
- **The academic suite.** The eval engineer treats the brief as research and composes a suite that would be defensible in a paper — HumanEval, TruthfulQA, MMLU subset, published RAG benchmarks. The suite measures *the model*. It does not measure *the product*. The product-shaped signal that leadership cares about ("does the agent actually answer our customer-support tickets correctly") is not in the suite at all.

A written commissioning brief resists both failures. It says, in advance, what the suite must cover, on what data, against what baseline, with what tools, and to what threshold.

## Core concept 1 — the eight sections of a commissioning brief

A commissioning brief is one document, two to four pages, written by the manager (often with the eval engineer drafting beside them) before a line of eval code is written. The eight sections below are the minimum. Anything less and you are going to discover a gap during the first promotion vote.

### 1. Product surface and audience

One paragraph. What the feature does, who uses it, what shape of input it accepts, what shape of output it produces. If it is a tool-using agent, name the tools. If it is retrieval-augmented, name the corpus. If it takes long inputs, name the modal length and the 99th-percentile length. This paragraph is what every reader will refer back to when they ask "but what does the suite need to cover."

### 2. Baseline

What the new system's quality is measured *against*. Three common baselines:

- **Production system being replaced.** For a V2 or a refresh, the current production system is the baseline. The new suite must be runnable against both the old and the new, and the delta is the headline number.
- **A no-X ablation.** For a feature with a specific added capability (retrieval, re-ranking, a specific tool, a specific prompt technique), the baseline is the same system with that capability removed. The delta is the headline.
- **A published benchmark relevant to the task.** For a cold-start feature with no production predecessor, pin a public benchmark that is the closest proxy. Name the benchmark version (benchmarks evolve) and the known limitations. Prefer the benchmark whose dataset matches the audience in section 1; do not pin MMLU if the audience is a pathology lab.

A brief that skips this section always produces a suite whose numbers nobody knows how to read.

### 3. Risk posture and the governance pins

One paragraph that answers, in order: who is harmed if this feature misbehaves, how severely, and which published framework governs the floor. Pin two specific frameworks:

- **NIST AI RMF 1.0 and the Generative AI Profile (AI 600-1).** The GenAI Profile enumerates generative-AI-specific risk categories — confabulation / fabrication, harmful content, data leakage, privacy harms, information-integrity harms, dangerous recommendations. Pin the categories that apply to this feature. Pin them by name; the suite must have a case family for each pinned category.
- **OWASP Top 10 for LLM Applications.** For any feature that accepts untrusted input (indexed documents, user chat, tool outputs), pin the OWASP categories that apply — prompt injection (LLM01), insecure output handling (LLM02), sensitive information disclosure (LLM06), and so on. The suite must have a red-team or adversarial case family for each pinned category.

If the feature is regulated (EU AI Act high-risk, HIPAA, GLBA, ITAR), name the regulation and the specific article or control. The brief does not interpret the regulation — that is legal's job — but the brief must say "this feature sits under X; compliance is Y's call" so the eval work reflects it.

### 4. The six eval categories

A product-shaped eval suite covers six categories. Pick the ones that apply, and for each picked category, name the family of cases and the rough case count the first revision of the suite should carry. Drop categories that genuinely do not apply, but say why you dropped them so the next reader can audit the decision.

1. **Behavioural / task-success.** Does the system do the task? Measured on a golden dataset of input / expected-output pairs (or input / rubric pairs for generative tasks). This is the floor; every suite has it. Typical first-revision case count: 50–500, growing over time.
2. **Judge-based quality.** Dimensions of the output beyond pure task-success — helpfulness, faithfulness to retrieved context, instruction-following, tone, correctness-under-uncertainty, hallucination rate. Measured with an LLM-as-judge (with a published judge rubric and a judge–human agreement number) or a rubric judge. See `Ragas` for the retrieval-shaped canonical rubrics (faithfulness, answer relevance, context precision / recall), `DeepEval` for a broader metric library, and `G-Eval` for the general LLM-as-judge pattern.
3. **Safety.** The named risk categories from section 3. Jailbreaks, prompt injection (crucial for any agent or RAG system — see OWASP LLM01), toxic-content generation, policy violations, policy-aware refusal correctness. Pair with a red-team corpus from the data flywheel (chapter 07).
4. **Retrieval quality (if RAG).** Recall / precision at the chunk level, coverage of the known-answerable question set, answer-grounding to retrieved context, freshness. See `Ragas` and the `rag-engineer` IC track for depth.
5. **Tool-use correctness (if agentic).** Did the agent call the right tool? With the right arguments? In the right sequence? Did it recover from tool failure? See `Inspect` (strong at tool-use evals) and the `agentic-ai-engineer` IC track for depth.
6. **Cost and latency.** Not usually thought of as "evals" but they belong here because they share the gate. Per-request cost at modal input size; p50 / p95 / p99 latency; aggregate budget envelope.

For each category, the brief specifies: *who writes the first cases, on what data, and by what date*. Un-sourced "we'll come up with some cases" lines are a tell that the brief has not actually been filled in.

### 5. The dataset plan

Evals without data are evals of nothing. The brief names the datasets the suite will read from:

- The **golden dataset** — the small, high-quality set of input / expected-output pairs (or input / rubric pairs) on which behavioural eval runs. Named, versioned, owned by a specific person.
- The **production trace sample** — the larger, naturalistic set sampled from real traffic (once the feature has traffic). For a cold-start feature, this is populated after launch.
- The **adversarial / red-team corpus** — curated adversarial inputs. Usually sourced from (a) OWASP LLM Top 10 reference examples, (b) MLCommons AILuminate hazard categories, (c) internal red-team exercises.
- The **holdout** — a slice held out of training / fine-tuning / prompt-engineering to measure true generalisation. Write-only; if an IC starts "iterating against the holdout," it has stopped being a holdout. This is a manager-enforced discipline.

The brief says, for each dataset: provenance, PII / privacy posture (what the retention policy is), how the dataset refreshes, and who owns the refresh cadence.

### 6. The tool choice

Pick the eval-runner / observability stack. The landscape, at the manager's altitude:

- **Eval harnesses (test-side).** `OpenAI Evals` — flexible, YAML-first, OpenAI-biased but model-agnostic in practice. `Inspect` — the UK AI Safety Institute's framework, strong at agent and tool-use evals, open source. `promptfoo` — fast, developer-local, strong CI story, supports LLM-as-judge natively. `Ragas` — RAG-specific, ships the canonical retrieval-eval rubrics. `DeepEval` — pytest-integrated, broad metric library, ties to `Confident AI` as the SaaS side.
- **Observability / trace storage and online evals.** `LangSmith` — tightly integrated with LangChain / LangGraph apps, strong on dataset + experiment tracking. `Langfuse` — open-source, can self-host, model-agnostic tracing + evals. `Braintrust` — SaaS, strong on experiment comparison and human-review workflows. `Arize` (and `Phoenix`, its open-source companion) — ML-observability vendor that has re-entered LLMs with drift monitoring and online eval.
- **Open standards worth pinning.** `OpenTelemetry GenAI semantic conventions` — the field-naming convention for LLM traces. Insist on it even if you pick a proprietary store, so you can move stores later without re-instrumenting.

The brief picks one eval harness and one observability store. It explains the choice in one paragraph and names the exit criteria — what would cause the team to re-evaluate (e.g., "we'll re-evaluate when the suite grows past 5000 cases and CI runtime exceeds 30 minutes").

Do not pick more than one of each. Teams that run two overlapping tools end up maintaining both at half the quality.

### 7. The thresholds

The brief states, as numbers, what "passing" means:

- The **PR-gate thresholds** — the subset of evals that must pass on every PR. Typically a deterministic behavioural subset (see chapter 03).
- The **release-gate thresholds** — the full suite, with specific minimums per category.
- The **regression allowance** — how much degradation is tolerated against the prior release. Usually a bootstrap-confidence-interval-aware statement, not a raw point delta ("quality must not regress by more than 2 points, measured with a 95% CI on a bootstrap of the golden set").
- The **absolute floors** — numbers below which the feature cannot ship regardless of delta. Safety floors live here. "Jailbreak success rate above 2% is a block regardless of quality delta" is an absolute floor.

If the brief cannot name thresholds, the gate cannot enforce them, and you will discover this the first time a PR produces a 1-point regression and the team debates what to do.

### 8. Ownership and the quarterly review

Name three owners:

- **Suite owner (IC).** The eval engineer who writes and maintains the suite. Usually a level-30 `ai-eval-engineer`; on smaller teams, the applied engineer who ships the feature.
- **Judge owner (IC).** If an LLM-as-judge is in the suite, somebody owns the judge rubric, its calibration set, and its recalibration cadence.
- **Review owner (manager).** The person — you — who reviews the suite every quarter against the current product surface and risk posture. Features drift; new tools are introduced; new regulation lands. The quarterly review is the mechanism that keeps the brief alive.

The quarterly review produces a short memo: cases added, cases retired, thresholds changed, categories expanded. Teams that skip this review have an eval suite from the first week of the feature that has nothing to do with the feature by the end of year one.

## Core concept 2 — the governance pins, in more detail

Two frameworks ground the safety and risk-posture part of the brief. The manager does not implement either — they pin the suite to them.

### NIST AI RMF 1.0 and the Generative AI Profile (AI 600-1)

The RMF is a general risk-management framework (Govern / Map / Measure / Manage). The Generative AI Profile is the specialisation for generative systems and enumerates the AI-specific risk categories — confabulation, content-integrity harms, data-privacy exposure, dangerous recommendations, bias propagation at generation time, and so on.

Pin the Profile risk categories that apply to this feature. The suite must have a case family (set of eval cases) for each pinned category. A defensible brief answers, for each GenAI Profile risk category, either "this feature is exposed to X; suite covers via case family Y" or "this feature is not meaningfully exposed to X because Z."

The practical artefact is a two-column table in the brief: left column, GenAI Profile risk category; right column, case-family identifier or "not applicable because …". This table is the evidence the regulatory / governance reviewer (`ai-risk-engineer`, level 25, in the AI Governance track) will ask for — even if your organisation does not yet have an `ai-risk-engineer`, the table is good hygiene.

### OWASP Top 10 for LLM Applications

The OWASP Top 10 for LLM Applications enumerates the attack-and-failure surface categories for LLM-using software. Current (2025) top-ten: Prompt Injection (LLM01), Sensitive Information Disclosure (LLM02), Supply Chain (LLM03), Data and Model Poisoning (LLM04), Improper Output Handling (LLM05), Excessive Agency (LLM06), System Prompt Leakage (LLM07), Vector and Embedding Weaknesses (LLM08), Misinformation (LLM09), Unbounded Consumption (LLM10). Numbers and titles have evolved across versions — pin the OWASP release you are measuring against (version + year), since vendors and auditors reference specific editions.

For every category that applies, the suite must have an adversarial / red-team case family. For a RAG system that accepts user-submitted document uploads, LLM01 (prompt injection via the retrieved document — "indirect prompt injection"), LLM04 (data poisoning if the user can seed the corpus), and LLM05 (output handling if downstream code consumes the model's output as trusted) are at minimum obligatory. For a tool-using agent, LLM06 (excessive agency) becomes central, and a case family that checks what the agent refuses to do is as important as the family that checks what it does.

This is one of the places where it is easiest to end up with a paper suite — generic jailbreak prompts that do not reflect your audience. Ground the adversarial case family in your audience: what would *your* adversary say, given what *your* system has access to. See chapter 07 on sourcing this via the data flywheel.

## Core concept 3 — the tooling landscape at the depth a commissioning manager needs

You are not writing a vendor-selection paper. You are picking one eval harness and one observability store and defending the choice. Enough context to do that:

- **If the application is RAG-shaped, start with `Ragas`.** The canonical retrieval-eval rubrics (faithfulness, answer relevance, context precision, context recall) are documented, widely referenced, and interop well with human-labelled golden sets. Wrap them in `promptfoo` or `DeepEval` if you want the CI story; or run them standalone and surface results in the observability store.
- **If the application is agent-shaped or tool-using, strongly consider `Inspect`.** It is built by a government AI safety institute, open-source, and has first-class support for tool-use evals, multi-turn agent tasks, and solvers (planners) as evaluation subjects. Its graders align well with the kind of behavioural+safety suite agents need.
- **If the team is already in LangChain / LangGraph, `LangSmith` is the path of least resistance.** Its dataset-and-experiment model is well-fitted to the SDK it ships with. If the team is not in LangChain, this is a weaker fit than the general-purpose observability stores.
- **For model-agnostic observability with a self-host option, pick `Langfuse`.** For a SaaS with strong experiment comparison, pick `Braintrust`. For a mature ML-observability story (drift monitoring across models + datasets) and a free OSS trace viewer, pick `Arize` / `Phoenix`.
- **`OpenAI Evals` is useful as a format baseline and is MIT-licensed.** It is less a product than a reference harness — teams typically use it for quick structured-task evals and move more complex suites into `promptfoo`, `Inspect`, or a custom runner as scope grows.
- **`DeepEval` fits well with pytest-native teams** that want evals to live alongside unit tests. It ties to `Confident AI` as the SaaS observability side; the OSS story is self-contained if you don't want that.

Pin the version of each tool you adopt. Pin whether you are using a hosted vs. self-hosted deployment. Pin the privacy posture for any SaaS tool — what traces leave your network, what retention they enforce, what their DPA says. Consult `mod-105-vendor-and-model-portfolio-selection` for the vendor-contract depth; this chapter stops at "pick one, defend it, document it."

## A worked example — the commissioning brief, filled in

A hypothetical team: five engineers shipping a tool-using doc-Q&A agent for internal customer-support staff. Feature is new; no production baseline yet.

- **Product surface and audience.** A multi-turn agent, accessed from the support console. Audience: internal CS representatives handling customer tickets. Input: a customer ticket plus the agent's chat context. Output: a drafted response, with citations to the internal knowledge base, and (occasionally) a ticket-system tool call (update status, tag). Modal input length: ~800 tokens; p99 ~6k tokens. Tools: `search_kb`, `search_tickets`, `update_ticket_status`. Retrieval over the internal KB (1.2M chunks, embedded with `[embedding model]`, served via `[vector index]`).
- **Baseline.** No production predecessor; the current manual path is a CS rep searching the KB themselves. Baseline for behavioural eval is the agent **without** retrieval (generation-only) to measure retrieval's lift. Baseline for latency is a target of p95 < 3s.
- **Risk posture.** Audience is internal, but drafts can be sent to customers. NIST GenAI Profile categories pinned: *confabulation* (high — fabricated citations would be sent to customers), *information-integrity harms* (medium), *data leakage* (medium — KB contains customer PII in some chunks). OWASP LLM categories pinned: *LLM01 prompt injection* (customer tickets are untrusted input), *LLM02 sensitive information disclosure*, *LLM06 excessive agency* (agent can update tickets). Regulatory: not pinned.
- **Six eval categories.**
  - Behavioural task-success: 200 CS-rep-authored ticket / expected-response pairs. Writer: eval engineer (A. Patel), by [date].
  - Judge-based quality: faithfulness to retrieved KB (Ragas), answer relevance, tone appropriate for customer-facing text. Judge rubric owned by A. Patel.
  - Safety: prompt-injection via ticket content (red-team corpus below); sensitive-data-leakage probes; refuses to update ticket when it should not.
  - Retrieval quality: recall@10, precision@3 against 150 known-answerable ticket questions.
  - Tool-use correctness: did the agent call `update_ticket_status` only when the rep approved? Did it recover from a mocked `search_kb` failure? 50 cases minimum.
  - Cost and latency: per-request cost target $0.04 at modal length; p95 < 3s; p99 < 7s.
- **Dataset plan.** Golden set: 200 cases hand-labelled. Production trace sample: starts after launch; weekly sampling. Adversarial / red-team: 100 prompt-injection cases sourced from (a) OWASP LLM01 reference adversarial prompts, (b) internal red-team exercise, (c) real malicious ticket history (sanitised). Holdout: 50 cases, write-only, A. Patel is the gate.
- **Tool choice.** `Inspect` for the eval harness (agent + tool-use support); `Langfuse` for observability (self-hosted, no customer PII leaves the network). Exit criteria: revisit at 5000 cases or when CI wallclock > 30 minutes.
- **Thresholds.**
  - PR gate: behavioural subset (50 cases) must score ≥ prior release. Latency regression > 15% blocks.
  - Release gate: full suite, judge-based faithfulness ≥ 0.85 (Ragas), tool-use correctness ≥ 0.95, prompt-injection "resists" rate ≥ 95%.
  - Absolute floors: prompt-injection success rate ≥ 2% blocks regardless of delta; sensitive-data leakage > 0% blocks regardless of delta.
- **Ownership.** Suite: A. Patel (ai-eval-engineer). Judge: A. Patel. Review: the manager (you), quarterly.

This brief is three pages. It is specific enough that the eval engineer can start on Monday. It is general enough that a quarterly review can update thresholds without rewriting the shape. That is the shape of a good brief.

## What good looks like

- The brief is written *before* an eval line of code lands. The eval engineer starts from a brief, not from an empty folder.
- Every eval category either has a case family named or has a one-sentence justification for being out of scope.
- NIST GenAI Profile risk categories and OWASP LLM Top 10 categories are pinned by name; the suite has a case family per pinned category.
- Exactly one eval harness and exactly one observability store are picked, and both are pinned to a version.
- Thresholds — PR-gate, release-gate, regression allowance, absolute floors — are stated in numbers.
- The three owners (suite / judge / review) are named humans. "The team" is not an owner.

## What bad looks like

- The brief is written after the suite is half-built — the IC has already made category and tool decisions by the time the manager weighs in.
- "Safety: we will add safety evals later." The release gate ships without an adversarial family; the first bad input lands in production.
- Two eval harnesses adopted at once. One lives in CI; the other lives in a notebook nobody runs. The team splits attention across both.
- Thresholds like "high quality" or "good coverage" — unmeasurable. The release gate cannot enforce these.
- No quarterly review. The suite calcifies around the feature as it was on day one; a year in, a third of the surface is uncovered.

## Summary

- The manager writes an eight-section commissioning brief before the eval engineer writes an eval line: product surface, baseline, risk posture (NIST GenAI Profile + OWASP LLM Top 10), the six eval categories, dataset plan, tool choice, thresholds, ownership.
- The six categories to cover (as applicable): behavioural / task-success, judge-based quality, safety, retrieval quality, tool-use correctness, cost and latency.
- Pin one eval harness and one observability store. `Ragas` for RAG, `Inspect` for agents and tool-use, `promptfoo` or `DeepEval` for CI-first general suites, paired with `LangSmith` / `Langfuse` / `Braintrust` / `Arize` for traces and online evals.
- Pin thresholds as numbers: PR-gate, release-gate, regression allowance, absolute safety floors.
- Review the brief quarterly against the current product surface and risk posture.

The next chapter walks the two-tier gate the thresholds above imply — PR gate vs. release gate — and the design choices that keep both gates believable under production pressure.
