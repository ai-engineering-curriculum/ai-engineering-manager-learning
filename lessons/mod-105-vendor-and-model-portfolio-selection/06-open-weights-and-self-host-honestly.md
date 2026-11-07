# Open-Weights and Self-Host, Honestly

Open-weights models — Llama, Mistral, DeepSeek, Qwen, and the growing list of competitive non-frontier families — have crossed the threshold of being genuinely production-viable for many workloads. The question is no longer *can we self-host?* The question is *should we, for this surface, right now, given the alternative cost structure of API access and the engineering surface we would be taking on?* The dishonest answer is either "always" (status, cost tunnel-vision) or "never" (risk aversion, familiarity bias). The honest answer is a deliberate decision per surface, defended in writing, re-opened quarterly.

This chapter is about writing that defense honestly: the four narrow cases where self-host earns its place, the hidden costs that invalidate most self-host spreadsheets, and the partnership with `ai-infra-team-lead` (peer manager, level 45, Infra ladder) that makes the decision real rather than aspirational.

## Motivation

Three failure shapes recur on self-host decisions without a clear partnership and a written memo.

- **The spreadsheet that didn't cost the engineers.** The AI Engineering Manager pitches self-host on the basis of a per-token cost comparison. Managed API: $X per million tokens. Self-hosted on reserved GPUs: $0.3X per million tokens. The spreadsheet shows a $200k annual saving. The spreadsheet does not include: the two engineer-years (of a specialised Infra role that costs more than most) to build, operate, and oncall the serving stack; the on-call burden distributed across a smaller team; the opportunity cost of those engineers not shipping product features; the depreciation and reserved-instance commitments; the quality-regression risk from staying on an older weights snapshot while the frontier moves. The "saving" is negative by any honest accounting.
- **The one-engineer dependency.** The team self-hosts because one engineer wanted to. The engineer builds it, operates it, and is the only person who understands it. Twelve months later the engineer changes teams or leaves the company. The replacement does not exist. The team is operating a production inference stack with no second-on-call. The right answer is to hire a peer Infra manager and a specialist IC *before* the self-host, not after.
- **The status self-host.** The team self-hosts because "we're an AI company — we should run our own inference." No specific surface requires it. No cost model justifies it. The eval does not say the open-weights model wins on quality. The team is operating infrastructure to express identity rather than to meet a product need. Chapter 02's inventory would not keep this entry; the memo should not sign it.

The four cases below are the honest ones. Everything else is "prefer API access, buy capability, re-open next quarter."

## Core concept 1 — the four narrow cases where self-host earns its place

Not every surface justifies self-host. Most do not. Four shapes do.

### 1. Data posture that no vendor's DPA will reach

Some data cannot leave the organisation's network, period. Regulated data under specific constraints (certain healthcare, defence, classified-government, certain financial-data categories under some jurisdictions), data under contract to a customer whose own contract forbids any third-party processing, data that a legal or policy decision has deemed non-transferable. In each case, chapter 05's DPA negotiation cannot produce a sufficient posture — the data is not allowed off-prem, or is not allowed into any cloud at all, or is not allowed to any processor regardless of posture.

For these surfaces, self-host is not a cost or performance decision; it is a prerequisite. Open-weights models (or on-prem deployments of closed-weights models where the vendor offers one — e.g. Cohere's VPC / on-prem path, Mistral's on-prem licencing, Llama via a self-hosted deployment on the organisation's own hardware) are the only viable path.

**Sanity check.** Before adopting this justification, confirm with counsel that the constraint is real and current. "Legal would never allow it" without legal having said so is a polite excuse, not a constraint. Many organisations discover on inspection that the constraint they feared does not exist in the form they thought.

### 2. Latency floor that API inference cannot meet

A specific surface needs TTFT or TPS that exceeds what the frontier APIs deliver on their standard endpoints. The specialist-inference providers (Groq, Cerebras, SambaNova) address much of this space without the team self-hosting — they operate inference on specialised hardware and sell the latency as a product. Self-hosting earns its place when (a) the latency floor is below what any specialist provider offers, (b) the specialist provider's model-set does not include a model that passes the quality bar, or (c) data-posture constraints from case 1 also apply.

Edge and on-device inference is a specific sub-case: a model running on a user's device (laptop, phone, dedicated hardware in a retail store) is self-host at the device level. The partnership here is with a different Infra counterpart (client / device engineering); the posture section still applies.

### 3. Cost crossover at extreme scale

At sufficiently high volume, self-hosting an open-weights model becomes cheaper than API access per token even after fully loading engineering cost, hardware depreciation, and on-call. The crossover point depends on the model, the hardware, and the utilisation. It is substantially higher than most teams estimate — a team that moves a hundred million tokens per day on a mid-sized model may *not* be past the crossover; a team moving ten billion tokens per day on the same model probably is. Numbers move quickly; this is a case where an Infra-partnered cost model (not a spreadsheet) is the required artefact.

**Sanity check.** The cost model must include: hardware (with depreciation and utilisation discounts for off-peak), electricity and cooling, networking, software-stack engineering time, operations and on-call time (fully loaded compensation), disaster recovery, quality-regression risk during the migration, and the opportunity cost of the engineers who are building and operating the stack *not* building product. A model that bakes in these line items and still shows crossover is defensible. A model that compares per-token list price to per-token hardware cost is not.

### 4. Specialist fine-tune that the API tier cannot host

A team has fine-tuned an open-weights model (or an API-offered tuneable model whose fine-tuned serving tier is not cost-effective) with sufficient customisation that moving it to an API provider's fine-tune endpoint is either not supported (not every provider offers fine-tune on every model) or is substantially more expensive than the API's base-model tier. Self-hosting the fine-tuned model may be the only path.

This case is genuinely narrower than it sounds — several frontier providers now host fine-tunes with pricing structures that compete, and the fine-tune-vs-RAG decision usually resolves in favour of RAG (mod-104 chapter 07) for most teams. When it does not, and the fine-tune is doing real work, the self-host case has standing.

## Core concept 2 — the hidden costs that invalidate most spreadsheets

A self-host cost model that omits any of these is not defensible. The AI Engineering Manager owns escalating each line to the peer Infra manager for a credible number.

- **Engineering cost — build.** Specialised engineers (`ai-infra-inference-engineer`, `ai-infra-platform-engineer`) are expensive. Building an inference stack is not a one-month project; a stable, autoscaled, monitored, benchmarked inference tier on open-weights is conservatively a half-year of engineering plus hardening.
- **Engineering cost — operate.** Ongoing operations: pager rotation, incident response, capacity planning, upgrades. The on-call burden falls on a smaller team than the API provider's and is harder to amortise.
- **Engineering cost — keep up.** The frontier moves. A model snapshot from fifteen months ago is behind the current state on most benchmarks; the team must re-eval and potentially re-deploy on a cadence or accept degradation relative to API alternatives. "Keeping up" is a recurring engineering cost.
- **Hardware cost — not list price.** On-demand GPU hourly rates are not what reserved-instance or committed-use agreements cost; both are below list price; both commit the organisation to capacity whether or not it is used. Reserved capacity that is 60% utilised in production costs the *full* reservation, not 60% of it. The cost model multiplies hourly rate by hours and divides by expected utilisation at the honest number.
- **Observability and operational cost.** Serving requires metrics, traces, logs, dashboards, GPU utilisation monitoring, batching tuning, autoscaling policy. Pager systems, SLO definitions, SRE discipline. All of this is either built or bought; neither is free.
- **Compliance evidence cost.** If the organisation needs SOC 2 Type II or similar attestation that covers the inference tier, the manager has moved that evidence burden onto the organisation. The API provider's attestation does not transfer; the organisation's own attestation now covers the inference tier, which is more work than it sounds.
- **Model-upgrade risk.** When a new open-weights model is released, the team must re-eval, migrate, and redeploy; migration cost and eval-regression risk are real. The API path has a similar risk but the migration is a config change, not an infrastructure migration.
- **Disaster recovery.** Self-host requires DR planning; API does not (the provider handles DR). The DR plan is additional engineering plus likely additional reserved capacity in a second region.

A cost-crossover memo that omits three or more of these lines is not a cost-crossover memo; it is an argument for self-host written by someone who already wanted self-host.

## Core concept 3 — the partnership with the Infra ladder

Self-host is an Infra discipline. The AI Engineering Manager does not operate the inference tier. Three partners.

### The peer AI Infra manager (`ai-infra-team-lead`, level 45)

The counterpart manager. Decisions to self-host are made jointly; the AI Engineering Manager owns the product rationale (why this surface), the Infra manager owns the serving capability and on-call. If no peer Infra manager exists at the organisation, that is the first thing to solve before self-host — self-hosting without a counterpart manager creates a one-engineer dependency by construction.

### The IC inference engineer (`ai-infra-inference-engineer`)

The specialist IC on the Infra side who runs the serving stack: GPU capacity planning, batching, quantisation (FP16 / BF16 / FP8 / FP4), speculative decoding, continuous batching, request routing, server choice (vLLM, SGLang, TGI, TensorRT-LLM, Hugging Face TEI, NVIDIA Triton, or a managed equivalent), autoscaling policy, observability. The AI Engineering Manager reads enough of this chapter to make the decision; the inference engineer does the work and is accountable for the quality and cost numbers the chapter 03 selection memo relies on.

### The IC platform engineer (`ai-infra-platform-engineer`)

The specialist IC on the Infra side who runs the surrounding platform: Kubernetes (or equivalent), identity, networking, secrets, CI/CD for model deploys, cost allocation, security posture. For teams whose platform is new, a lot of self-host cost actually falls on the platform engineer rather than the inference engineer — the manager accounts for both.

The joint artefact that commits to a self-host is a **capability brief** co-signed by both managers. It carries:

- The product rationale (which of the four narrow cases).
- The target model (identifier, licence, version).
- The target serving stack (server, hardware, region).
- The capacity commitment (reserved / on-demand / mixed).
- The eval evidence that the open-weights alternative meets the quality bar on the feature.
- The cost model (fully loaded, with all lines from core concept 2).
- The on-call commitment (who pages, with what SLA).
- The exit criteria (under what conditions would we migrate back to API).
- The quarterly review date.

A self-host decision without a co-signed capability brief is one manager's decision, not the organisation's.

## Core concept 4 — the serving-stack landscape at manager depth

The AI Engineering Manager does not pick the serving framework. The AI Infra manager does. The AI Engineering Manager reads enough to understand what the choice buys. At that depth:

- **vLLM.** Open-source, widely adopted, strong default for most Llama / Mistral / Qwen / DeepSeek deployments. Continuous batching, paged attention, broad model support, active community. The reasonable starting point for most open-weights serving.
- **SGLang.** Open-source, strong on structured-output and constrained-decoding workloads; strong latency profile for batched short-input workloads.
- **Text Generation Inference (TGI).** Hugging Face's open-source server; strong integration with HF model ecosystem; widely used in production.
- **TensorRT-LLM.** NVIDIA's closed-source inference optimisation stack; best-in-class latency and throughput on NVIDIA hardware when the model and the engineering effort are both tuned to it. Higher operational surface than vLLM.
- **NVIDIA Triton Inference Server.** NVIDIA's general-purpose inference server; often sits *in front of* a model backend (vLLM, TensorRT-LLM) to manage multi-model, multi-tenant serving.
- **Hugging Face Text Embeddings Inference (TEI).** For embedding and reranker workloads specifically; lighter weight than generative serving.
- **Ollama.** Developer-local serving; not a production tier but widely used for prototyping and offline eval.
- **Managed open-weights platforms.** Together AI, Fireworks AI, Groq, Cerebras Cloud, Replicate, Anyscale, Modal, Databricks Serving — these are "self-host without self-hosting"; the organisation buys open-weights serving as an API from a provider that operates the stack. For many teams considering self-host, a managed open-weights platform is the right answer — it captures open-weights advantages (model identity, licence, data-posture through the provider's enterprise contract) without the operational cost.

The managed-open-weights option is the one AI Engineering Managers underweight most often. It is often the right intermediate step: ship on a managed open-weights platform first, prove the quality and cost case for six to twelve months with real production traffic, *then* decide whether true self-host earns its place.

## Core concept 5 — the licence reading

Open-weights licences are not uniform. Read the licence per model before adopting.

- **Apache 2.0, MIT, BSD-3.** Permissive OSI-approved. No material restrictions for commercial use. Several Mistral models (Mistral 7B, Mixtral 8x7B, Mixtral 8x22B) and several embedding / reranker open-weights releases are on these.
- **Llama Community Licence.** Not OSI-open. Permissive for most commercial use but carries (a) a revenue-threshold clause (very large organisations by monthly active users at the time of the Llama release must request a licence from Meta), (b) acceptable-use restrictions that mirror Meta's AUP, (c) requirements around branding and attribution. Read the current version per Llama release; versions have differed.
- **Gemma Terms of Use.** Google's Gemma family under a bespoke licence with acceptable-use clauses. Read the current version.
- **Qwen licences.** Vary per release — some Apache 2.0, some bespoke. Read per model.
- **DeepSeek licences.** Vary per release; several are MIT; some have additional terms. Read per model.
- **Mistral Research Licence** (and its successors). Several of Mistral's frontier releases are under a non-commercial research licence; commercial use requires a separate agreement with Mistral. Easy to miss.

The licence read runs alongside the DPA read (chapter 05). Counsel owns the opinion on licence sufficiency for the organisation's use. The manager's job is to flag the licence category, note the obligations (attribution, revenue thresholds, AUP flow-down), and route to counsel.

Acceptable-use clauses are worth attention. Several open-weights licences prohibit specific use cases — some military, some adult content, some surveillance, some medical-decision uses. If the organisation's product sits near any of those, counsel must sign.

## Core concept 6 — the honest memo

The self-host decision produces a two-page memo that mirrors the shape of chapter 03's selection memo, with additional sections. Minimum structure:

### 1. Feature profile and product rationale (one paragraph)

Which feature, what shape, what traffic. Which of the four narrow cases the self-host justification falls under; if it falls under none, the memo is a *prefer API* memo and should not recommend self-host.

### 2. The candidate stack (half a page)

Model + licence + serving framework + hardware + region + reserved-vs-on-demand split. Co-signed by the peer Infra manager.

### 3. The full cost model (half a page)

The eight line items from core concept 2, each with a number and a basis. Compared against the API alternative for the same model (or closest comparable) at the same traffic.

### 4. The quality evidence (two paragraphs)

The open-weights model's eval score against the surface's thresholds (mod-104 chapter 02). The regression (or improvement) vs. API alternative. The fallback plan if self-host underperforms in production.

### 5. The operational commitment (one paragraph)

On-call rota, SLO targets, pager threshold, the named humans who carry the pager, the escalation path. Co-signed by the peer Infra manager.

### 6. The exit criteria

Under what conditions would we migrate back to API? Specific numbers: on-call load, eval regression, cost overrun, hardware availability, licence changes.

### 7. Ownership and review

AI Engineering Manager (you), AI Infra manager (peer), IC owners on both sides, next quarterly review date.

## A worked example — a *prefer API* memo for the customer-support agent

For the surface in chapters 02–04, the honest self-host memo reaches a different conclusion.

- **Product rationale.** Multi-turn tool-using agent for internal CS reps. Data posture: customer PII in KB and in tickets, but existing DPAs with both Anthropic and OpenAI permit this posture (chapter 05). None of the four narrow cases applies: posture is satisfiable via API, latency floor is met by current frontier APIs, traffic is five thousand calls/day (well below any cost crossover), no fine-tune in scope.
- **Recommendation.** Prefer API. Do not self-host for this surface.
- **What would change the recommendation.** (a) A regulatory change that forbids the current DPA posture (would re-open posture-case analysis). (b) Traffic growth to ten million calls/day with sustained production for 12+ months (would re-open cost-crossover analysis). (c) A fine-tune decision that the API tier cannot host (would re-open fine-tune-case analysis).
- **Review.** Reopen at the next quarterly portfolio review or earlier if the above triggers fire.

This is the right output for a surface where none of the four cases applies. A healthy team produces more *prefer API* memos than *self-host* memos.

## What good looks like

- Every self-host decision is a co-signed capability brief by the AI Engineering Manager and the peer AI Infra manager, citing one of the four narrow cases by name.
- The cost model includes the eight line items from core concept 2; a single-column spreadsheet is a flag, not a memo.
- Managed-open-weights platforms are considered as an intermediate step before true self-host.
- Licences are read on every open-weights adoption and routed to counsel for opinion where applicable.
- The on-call commitment is a named rota on the Infra side; one-engineer dependencies are a flag, not an operational model.
- Exit criteria are specific — a self-host decision that cannot be unwound is a trap.
- Most selection memos for most surfaces land on API, not self-host. The portfolio shows this.

## What bad looks like

- A self-host decision was made by one manager without the Infra counterpart signing; six months later the inference stack is a one-engineer dependency.
- The cost model is a per-token comparison that ignores engineering, operations, and hardware depreciation.
- Managed-open-weights platforms were not considered; the team jumped from API to self-host in one step.
- Licences were not read; the organisation is using a model whose acceptable-use clauses the legal team has never seen.
- No exit criteria; self-host is a direction, not a decision.
- Status-driven: "we run our own inference because we're an AI company." No feature requires it.
- API alternative continues to improve while self-hosted model stays on the snapshot the team deployed eighteen months ago; eval score on the surface quietly regresses.

## Summary

- Self-host earns its place in four narrow cases: data posture no vendor can reach, latency floor below specialist-provider offers, cost crossover at honest-load extreme scale, specialist fine-tune the API tier cannot host.
- The honest cost model includes build and operate engineering, keep-up engineering, fully-loaded hardware (not list price), observability, compliance, upgrade risk, DR.
- Decisions are co-signed by the peer AI Infra manager; the IC inference and platform engineers own the serving stack.
- Managed open-weights platforms (Together, Fireworks, Groq, Cerebras, Databricks, Anyscale, Modal) are the underweighted intermediate step — open-weights advantages, operated as an API.
- Licences are not uniform; read per model, route to counsel where applicable; acceptable-use clauses are a common surprise.
- The honest memo often recommends *prefer API*; a healthy team produces more of those than *self-host* memos.

The next chapter picks up the cadence that keeps all of the above alive: the quarterly portfolio review, the mid-quarter rapid-add path for new capabilities, the decommissioning calendar, and the Model Card / release-notes reading discipline that keeps the manager current with what the team is shipping.
