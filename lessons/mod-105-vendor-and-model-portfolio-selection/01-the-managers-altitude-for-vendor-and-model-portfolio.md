# The Manager's Altitude for Vendor and Model Portfolio

Model selection is an engineering decision. Vendor selection is a legal and commercial decision. The product shipping on top of both is the manager's accountability. If the model your team uses is wrong for the task, the eval suite from mod-104 will catch it — eventually. If the vendor your team uses is wrong for the data, no eval suite catches it; the auditor does, the DPO does, or a procurement review on a renewal does. The manager's job in this module is to run the artefacts that prevent both failure modes and keep them current across a landscape that changes every quarter.

If mod-104 taught you to require the eval-gated SDLC, this module teaches you to require the **portfolio-gated** SDLC: a written inventory of the models and vendors the team is allowed to call, a selection memo per feature that defends why *this* model for *this* surface, a fallback and routing plan so a single provider outage does not take the product down, and a cadence that keeps all of the above alive as the frontier moves.

## Motivation

Three failure shapes recur when the model and vendor portfolio has no manager-owned surface.

- **Single-vendor dependence that nobody wrote down.** The team starts on one provider for the first feature. The second feature inherits the first feature's SDK. The third feature inherits both. Eighteen months in, the entire product is on a single provider; the first time that provider has a six-hour incident (and every provider has had one), the product is down and there is no fallback because nobody built one and no one in leadership asked for one. The organisation discovers its SPOF via an outage.
- **DPA roulette.** Each feature team signs up for API access independently; nobody reads the Data Processing Addendum until legal does during the enterprise-customer security review. The team learns the current contract permits training on customer data, or ships PII to a sub-processor the organisation has not disclosed to its customers, and the ship date slips three months while procurement renegotiates. The team blames legal; legal correctly points out they were never asked.
- **Quiet model drift.** The team pinned a model name two years ago. The vendor has released three generations since. The team ignored the release notes. The pinned model is being deprecated in sixty days; the migration has not been evaluated; the eval suite does not run against any alternative. The team scrambles.

None of the three is a technology failure. All three are the absence of a manager-owned operating cadence around vendor and model choice.

## Core concept 1 — what the manager owns, versus what the IC owns

The split mirrors mod-104's: craft is the IC's, system is the manager's. Four columns of the table where it is worth being specific, because this is where managers most often drift into either doing the IC's work badly or leaving the system entirely to the ICs.

| What the IC owns (craft) | What the manager owns (system) |
|---|---|
| Benchmarking a candidate model against the eval suite for a specific feature; writing the eval report. | Requiring a **written selection memo** for every feature that commits to a model, before the feature ships. The memo cites the eval report. |
| Implementing the provider SDK; plumbing retries, timeouts, structured output; writing the client library. | Requiring that application code route through an **abstraction layer** (not raw SDK) so swap-out is a config change, not a rewrite; requiring at least one tested fallback. |
| Reading a vendor release note, running the regression suite against a new snapshot, filing a PR to adopt it. | Owning the **quarterly portfolio review** calendar; naming the mid-quarter rapid-add path for new capabilities; owning the deprecation watch. |
| Negotiating a specific technical clause in a DPA with vendor engineering (e.g. endpoint residency). | Partnering with procurement to **read every DPA before signing**, flagging which clauses need legal review, holding the redline list the organisation defends on each renewal. |
| Implementing a self-hosted inference server (vLLM / TensorRT-LLM / TGI) with autoscaling and observability. | Defending the **build-vs-buy call** quarterly; partnering with `ai-infra-team-lead` (peer manager, level 45, Infra ladder) and `ai-infra-inference-engineer` on serving; avoiding self-host as a status symbol. |

The healthy pattern: the manager sets the shape of the portfolio and its governance; the IC fills it with the specific model and specific SDK choices that fit the surface; the manager reviews the whole surface every quarter and tightens it when the frontier has moved or a vendor has let the team down.

When the manager drifts into the craft column — "I read the API docs; let's use X" — the ICs feel steamrolled and the choice is usually under-justified. When the manager neglects the system column, the team runs into the three failures in the motivation section and the manager is surprised each time.

## Core concept 2 — the four manager-owned artefacts

Every chapter in this module builds one of four artefacts. Memorise them; the chapters and exercises are organised around them.

1. **The portfolio inventory and model-selection policy (chapters 02–03).** A single document listing every model and every provider the team is allowed to call from production, each with its intended surfaces, pricing and latency profile, DPA posture, SLA, and the eval evidence that justified its inclusion. Plus a per-feature *selection memo* template: two pages that commit a surface to a specific model with reasoning. Without this, "we use GPT-4" or "we use Claude" is a vibe, not a decision.
2. **The fallback and multi-model routing plan (chapter 04).** The written policy for what happens when the primary model is unavailable, degraded, or quota-limited: which fallback, what quality delta is acceptable on the fallback, how the application code routes, how incident response escalates. Without this, every vendor outage is a product outage.
3. **The contract and DPA posture (chapter 05).** The redline list the organisation defends on every vendor DPA, the escalation to counsel, the hand-off to procurement for execution, and the catalogue of which vendors sit under which contract. Without this, every renewal is a fresh negotiation and every new feature that touches sensitive data is a surprise to legal.
4. **The portfolio review cadence and Model Card reading discipline (chapter 07).** A quarterly standing review (adds / retires / threshold changes), a mid-quarter rapid-add path when a new capability justifies acceleration, a decommissioning calendar for deprecated models, and the discipline of reading Model Cards and vendor release notes as a team ritual. Without this, the portfolio drifts out of date silently.

A team has a **manager-owned portfolio** when all four artefacts exist in writing, the selection memo is required by the eval-gated SDLC's release gate (mod-104 chapter 03), and the quarterly review happens.

## Core concept 3 — who defers to whom on serving and on the law

Two deferrals the manager of an AI engineering team must get right, because they are the most common places to over-reach or under-reach.

### Serving defers to Infra

Running a model yourself — self-hosting an open-weights model on your own GPUs, using vLLM / TensorRT-LLM / Text Generation Inference / SGLang for serving, provisioning capacity, monitoring tail latency — is a *platform* discipline, not an application discipline. It is owned by the Infra ladder: `ai-infra-team-lead` (peer manager, level 45) and `ai-infra-inference-engineer` (the IC specialising in GPU-backed model serving) and `ai-infra-platform-engineer` (the IC specialising in the surrounding platform). An AI Engineering Manager who decides to self-host without an Infra counterpart to run it will discover six months later that one engineer has become the single person who understands the inference stack, and the day they take holiday is the day quality regresses with no one on call to notice.

The deferral runs the other way too. The **decision** to self-host a specific model for a specific product reason — cost curve, data posture, latency floor, offline/edge requirement — is an AI Engineering Manager decision in partnership with the peer Infra manager. The *operation* of the serving tier is an Infra decision. Chapter 06 walks the partnership.

### Contract execution defers to Procurement and Legal

Reading a Data Processing Addendum, flagging the clauses that need attention, maintaining the redline list the team defends every renewal — this is the manager's job. **Writing a legal opinion** on whether a specific clause is enforceable in a given jurisdiction is counsel's job. **Executing the contract** — negotiation, countersignature, relationship with the vendor's account team — is procurement's job. A manager who drafts legal arguments is slowing the process; a manager who signs contracts without reading them is importing risk. The model to run is: **read, flag, escalate; do not negotiate solo and do not sign blind.** Chapter 05 is specific about which clauses escalate to counsel and which to procurement.

## What this module does not teach

Three deliberate exclusions.

- **How to benchmark a model.** The craft of running an eval against a candidate model, interpreting delta vs. the current production model, and writing the eval report lives in [`ai-eval-engineer`](https://github.com/ai-engineering-curriculum/ai-eval-engineer-learning) and the eval-gated SDLC (mod-104). The manager cites the eval report when defending a selection; the manager does not run the eval.
- **How to operate self-hosted inference.** The craft of GPU capacity planning, batching strategies, speculative decoding, FP8 / FP4 quantisation trade-offs, continuous-batching tuning, autoscaling, failure modes of inference servers — all of this is the Infra ladder's. The manager reads enough of chapter 06 to make the decision; a peer Infra manager runs the thing afterwards.
- **How to litigate a DPA.** The manager flags redlines and routes to counsel; counsel takes it from there. Chapter 05 teaches you to recognise the clauses, not to write the binding language.

What the manager must know at depth: the *categories* of provider and model (frontier closed, frontier open-weights, specialist, inference-platform), the *dimensions* each is compared on (context, pricing, latency, SLA, DPA, residency, enterprise features), the *vocabulary* of the DPA (sub-processor, training-on-data, data retention, residency, SLA, indemnity, service availability), and the *mechanics* of a cadence (quarterly review, mid-quarter rapid-add, decommissioning). The IC curricula teach the craft; this module teaches the operating discipline that chooses and renews the portfolio.

## The shape of the chapters ahead

- **Chapter 02 — The frontier portfolio landscape.** The providers an AI engineering team must track, the capabilities and dimensions to compare them on, and the inventory table you keep up to date. OpenAI, Anthropic, Google DeepMind, Mistral, Cohere, Meta (open weights), and the specialist and inference-platform providers (Cerebras, Groq, Together, Fireworks, AWS Bedrock, Azure OpenAI, Google Vertex AI, Databricks). Pricing, context, latency, SLA, data-use policy at commissioning-manager depth.
- **Chapter 03 — Per-feature model selection and the selection memo.** The two-page memo that commits a surface to a model: feature profile, cost / latency / quality / safety trade-off, eval evidence, exit criteria. The three axes managers get wrong most often (cost tunnel-vision, latency tunnel-vision, "big model always wins") and how the memo structure prevents them.
- **Chapter 04 — Fallback and multi-model routing.** Why a single-provider product is a product with a single point of failure. Fallback shapes (same-family downgrade, cross-provider swap, cached-response degradation, hard-offline refusal). Multi-model routing policies (quality-first, cost-first, latency-first, capability-first). The abstraction layer every feature should route through.
- **Chapter 05 — Reading a DPA and partnering with procurement.** The clauses that matter: training-on-data, data retention, sub-processors, data residency, deletion rights, SLA and remedies, indemnity, confidentiality. The redline list the organisation holds. The escalation path to counsel. The hand-off to procurement for execution.
- **Chapter 06 — Open-weights and self-host, honestly.** The real reasons to self-host (data posture, offline / air-gapped requirement, extreme-scale cost crossover, latency floor on specialised hardware). The real reasons not to (status, "we can save a bit", one-engineer dependency). The partnership with `ai-infra-team-lead` and the serving stack landscape at manager depth.
- **Chapter 07 — Portfolio review cadence and Model Cards.** The quarterly review ritual (adds, retires, threshold changes). The mid-quarter rapid-add path for a capability that cannot wait. The decommissioning calendar. Reading Model Cards and system cards, vendor release notes and deprecation schedules, as the manager's reading discipline that keeps the whole system honest.

## What good looks like

A team has a manager-owned vendor and model portfolio when:

- A new engineer can open the portfolio inventory on their first day and see every model the team is allowed to call from production, with the intended surfaces, pricing, and DPA posture.
- Every feature in the release register has a two-page selection memo on file, dated, naming the model, the fallback, the eval evidence, and the exit criteria. "We use Claude for this" is not a memo.
- A vendor outage triggers a tested fallback, not a scramble; the product degrades in a documented way and on-call has a runbook (mod-107 companion).
- A procurement request to renew a vendor contract reaches the manager a week before the deadline, with the redline list pre-filled from the last renewal; legal and procurement are not surprised.
- A new model generation lands; the manager has read the Model Card by the end of the week, the regression suite is queued, and a decision to adopt (or not) is written up within the quarter.

## What bad looks like

- The portfolio inventory is "whoever wrote the first feature picked the first vendor; the rest followed."
- Selection memos do not exist; model choice is a hallway conversation that is re-litigated every time a new engineer joins the team.
- The product has one provider end-to-end, no fallback is configured, and nobody has run the "what if this provider is down for four hours" tabletop (mod-107 companion).
- DPAs are signed without reading because "legal already reviewed it last year" — but the vendor updated their DPA six months ago and nobody noticed.
- The team is still calling a model name that is on the vendor's deprecation list with sixty days to go, and the migration has not been scheduled.
- Self-hosting happened because "it's cheaper in a spreadsheet" and no one costed the engineer-time to operate it or the on-call to pager it.

## Summary

- The craft of benchmarking a model and the craft of operating inference belong to ICs on the AI Eval and AI Infra ladders respectively; the system that governs *which* models and *which* vendors the team uses is a manager-owned system.
- The manager owns four artefacts: the portfolio inventory and per-feature selection memos, the fallback and routing plan, the DPA / contract posture, the review cadence and Model Card reading discipline.
- Serving defers to Infra; contract execution defers to Procurement and counsel. The manager's job is to decide *that* a model is used, *that* a fallback exists, *that* a DPA has been read; the depth of the serving stack and the legal binding both defer out.
- The rest of the module walks the four artefacts in turn, with an exercise per artefact that fills it in for a real product you ship or plan to ship.

The next chapter picks up the first artefact: the frontier portfolio inventory and the dimensions on which every model and every provider is compared.
