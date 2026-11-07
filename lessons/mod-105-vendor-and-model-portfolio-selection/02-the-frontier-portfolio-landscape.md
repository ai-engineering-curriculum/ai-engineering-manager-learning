# The Frontier Portfolio Landscape

The manager's job in this chapter is to build the **portfolio inventory** — the written list of every model and every provider the team is allowed to call from production, each with the dimensions that matter for a shipping product. This is not a vendor-selection paper. It is the artefact that a new engineer, a security reviewer, a procurement partner, or an on-call engineer reads to know what the surface area is. If the inventory does not exist, every conversation about model choice starts from zero.

The landscape moves. Any specific number you write in your inventory — a price per million tokens, a context-window size, a SLA percentage — is wrong within weeks to months. The *shape* of the inventory, the *dimensions* each entry carries, and the *cadence* that refreshes them (chapter 07) are the parts you own.

## Motivation

Three failure shapes recur when the inventory does not exist as a managed artefact.

- **The dimensions-mismatch call.** The team argues over which model to use for a new feature and the conversation stalls on "context window" versus "latency" versus "cost" because no one has written the dimensions out side-by-side. Thirty minutes of meeting; no decision; a week later somebody arbitrarily ships what they already use. Chapter 03 teaches the selection memo that resolves this, but the memo only works if the underlying inventory is already filled in.
- **The hidden-dependency call.** A feature inherits a model choice from an earlier feature. Nobody realises that the inherited model does not have a DPA that permits training-on-data to be disabled, or does not offer EU residency, until the enterprise customer's security review flags it. The inventory, if it existed, had a column for both facts.
- **The capability-gap call.** A competitor ships a feature your team cannot. The underlying reason is a capability only one provider offers right now — vision, long context, embedded search, a particular tool-use shape, a specific language. The team's inventory did not track capabilities at the resolution that would have surfaced the gap; the mid-quarter rapid-add path (chapter 07) kicks in six weeks too late.

The inventory solves all three the same way: by making every provider and every model a row in a table with a fixed set of columns, so the comparison is a column-by-column read rather than an argument.

## Core concept 1 — the categories of provider

Not every provider plays the same role in the portfolio. Four categories, each with a different reason to be in the inventory.

- **Frontier closed-weights labs.** The providers that train and operate their own flagship models and serve them only via API. OpenAI, Anthropic, Google DeepMind (via Google Cloud Vertex AI and the standalone Gemini API). These labs typically lead on capability and are the default primary for most production features. Each ships its own Responsible-Scaling / Preparedness / Frontier-Safety framework (see mod-104 resources) — read the framework; it tells you how the lab governs releases of its own models. Each publishes a Model or System Card per release that is the manager's required reading (chapter 07).
- **Frontier open-weights labs.** The labs that publish weights for their frontier or near-frontier models under a licence that permits self-hosting. Meta (Llama family), Mistral (Mistral / Mixtral / Codestral), and smaller labs that publish weights (DeepSeek, Qwen, Cohere's Command family partial-weights editions, etc.). Being in the inventory as *weights* lets you self-host (chapter 06); being in the inventory as *API* lets you call the same model from a managed provider (Together, Fireworks, AWS Bedrock, Azure, Google Vertex, Groq, Cerebras, Mistral's own API). Open-weights labs usually publish both a Model Card and a licence worth reading in full — the licences differ in material ways (acceptable-use clauses, revenue-threshold restrictions, geographic clauses).
- **Enterprise / specialist labs.** Labs whose strongest product is specialised to a category. Cohere (retrieval and enterprise-focused LLMs, with a strong rerank and embed line); Voyage (embeddings); AI21 (summarisation and long-form reasoning); Cresta, Writer, and other vertical specialists. These may be the right primary for a specific feature even when a frontier lab is the default for others — this is the point of multi-provider portfolio in the first place.
- **Inference platforms and clouds.** Providers that serve third-party models (often open-weights) with their own operational envelope — latency, throughput, regional availability, pricing structure, enterprise contract. Together AI, Fireworks AI, Groq, Cerebras Cloud, Replicate, Modal; and the hyperscaler AI platforms — AWS Bedrock (which federates Anthropic, Meta, Cohere, Mistral, AI21, Amazon's own Titan / Nova family, and more), Azure AI Foundry (hosts OpenAI, Mistral, Meta, Cohere), Google Vertex AI (Gemini plus third-party), Databricks (Mosaic / DBRX plus third-party), IBM watsonx. These show up in the inventory because *who serves the model* has material consequences for latency, SLA, DPA, residency, and contract — not only *which* model.

A healthy portfolio has entries from at least two of these categories — one frontier closed primary, one alternative from a different lab or platform — before you talk about a third. Chapter 04 is the discipline that makes the alternative meaningful rather than theoretical.

## Core concept 2 — the dimensions every row carries

Every entry in the inventory carries the same columns. The columns below are the minimum. Add columns as the organisation's governance asks for more (SOC2 letter on file, ISO 27001 evidence, HIPAA BAA on file, FedRAMP posture, etc.).

### 1. Identifier and version

The specific model name **and version identifier** as the provider documents it. `gpt-4o-2024-08-06` not "GPT-4". `claude-sonnet-4-5` or the dated snapshot the provider publishes. `gemini-1.5-pro-002` not "Gemini". `mistral-large-2411` not "Mistral". If the provider offers aliases ("latest"), document whether you pin the snapshot or ride the alias — pinning is almost always the right default for production; riding a non-pinned alias means an undated model change in your release pipeline. Chapter 04 of mod-104 covers this discipline.

### 2. Context window (input and output)

Separate numbers for maximum input tokens and maximum output tokens where the provider separates them, because most providers do. Context windows have expanded rapidly; several providers publish 200k / 1M / 2M input context windows. Use the number the provider publishes for the specific snapshot, not the generation marketing. Note also whether the full context is useful — some providers' long-context performance degrades sharply well before the stated limit; chapter 03 picks this up as part of the selection decision.

### 3. Pricing — input, output, cached input, batch, fine-tune

Four numbers for most frontier providers, five if fine-tune is offered:

- **Input tokens, standard.** Per million tokens.
- **Output tokens, standard.** Per million tokens; usually 2–5× the input rate.
- **Cached input tokens.** Several providers (OpenAI, Anthropic, Google) offer a discounted rate for prompt-prefix cache hits. The discount is often 10–90% depending on provider and cache mode. Caching is a material cost lever for RAG and agentic workloads where a long system prompt and retrieved context repeat across calls.
- **Batch tokens.** Several providers (OpenAI Batch API, Anthropic Message Batches, Google Batch, AWS Bedrock Batch, Mistral Batch) offer discounted asynchronous batch inference — commonly ~50% of standard. Useful for offline eval, data-flywheel labelling, backfills.
- **Fine-tune pricing** (if offered). Training cost per token (or per example), and the serving-time cost for the fine-tuned model, which is often higher than the base model.

Pricing changes often enough that whenever a number lands in a selection memo it is dated. The *structure* — that these five axes exist — does not change. <!-- needs-research: current price points per provider; these move frequently and should not be copied from here into a selection memo without checking the provider's pricing page on the day. -->

### 4. Latency profile

At minimum: time-to-first-token (TTFT) and tokens-per-second (TPS) at the modal input length for this feature, on the specific provider and the specific snapshot. Long-context and low-TTFT are sometimes the real reason a specialist provider (Groq, Cerebras) wins a surface — these providers often lead on TPS and TTFT and are sometimes the right primary for an interactive surface regardless of model identity. The eval suite (mod-104 chapter 02) should measure latency as a first-class eval category; the inventory carries the headline numbers from the eval.

### 5. Throughput, rate limits, and quota

Published rate limits (requests per minute, tokens per minute, context-adjusted per-minute caps) and the path to raise them. Enterprise tiers often negotiate quota as part of the contract; procurement (chapter 05) knows what your floor is. Provisioned-throughput options (OpenAI Scale Tier, AWS Bedrock Provisioned Throughput, Azure Provisioned Throughput Units, Google Vertex Provisioned Throughput) are a separate pricing model — reserved capacity in exchange for predictable cost — and belong in the inventory where used.

### 6. SLA and operational posture

The published availability SLA (e.g. 99.9%) and the remedies (credits, typically). The vendor's status page URL. Historical incident cadence read off the status page or third-party trackers. SLA is one of the clauses that enterprise-contract teams negotiate above the published default; chapter 05 is specific about how far that negotiation usually runs.

### 7. DPA posture — training-on-data, retention, residency, sub-processors

Four flags per row:

- **Training on your data?** Default and opt-out status. Most enterprise tiers of frontier providers now default to *no* training on customer data for API traffic, but defaults differ and consumer-grade tiers often differ from enterprise-grade tiers. Pin the current language from the DPA, not from marketing. Chapter 05 walks the clause.
- **Data retention.** How long the provider retains API inputs and outputs by default. Zero-retention (ZDR) modes exist on several providers for qualified enterprise customers; some require a specific contract amendment.
- **Residency.** The region(s) the provider can serve from under contract (US, EU, UK, Canada, Japan, Australia, India, etc.). Sovereign variants (AWS GovCloud, Azure Government, Vertex AI in specific regions, Anthropic via Google Cloud Vertex AI in specific regions, OpenAI via Azure in specific regions) belong here.
- **Sub-processors.** The list of the provider's own sub-processors. Enterprise customers are usually required to disclose their sub-processors under their own DPAs; this is where you discover whether using Provider X obliges you to disclose Provider X's sub-processors to your customers. Chapter 05 is specific.

### 8. Enterprise features

Check-boxes for the features that routinely appear in the manager's selection reasoning: SSO, SOC 2 Type II letter available on request, ISO 27001, HIPAA BAA available, PCI DSS evidence available, FedRAMP posture, audit logs API, admin console with role-based access, VPC / PrivateLink connectivity, customer-managed keys (CMK / BYOK), content-filtering off-ramp (for safety-case-handled workloads where the provider's upstream filter would collide with the team's own guardrails).

### 9. Capability tags

Short tags for capabilities that differentiate this entry: *vision*, *audio-in*, *audio-out*, *tool-use (parallel)*, *tool-use (structured)*, *prompt-caching*, *computer-use*, *code-interpreter / code-sandbox*, *built-in search*, *long context > 200k*, *streaming*, *predicted outputs*, *structured output / JSON mode*, *function-calling*, *vision OCR*, *document understanding*, *extended thinking / reasoning mode*. The tags are a filter when a feature's selection memo (chapter 03) asks "which models can do X."

### 10. The eval row

The last column is the pointer into the eval evidence: which eval run, on which date, produced the numbers (behavioural, judge-based, safety, latency, cost) that let this model enter the inventory. If the eval column is empty, the row is a candidate, not an entry.

## Core concept 3 — the current frontier at commissioning-manager depth

The specific products and the specific numbers move. What follows is the *shape* of the frontier at commissioning-manager depth — the providers the inventory should have an opinion on, not a snapshot of prices. Treat it as a reading guide; each entry's footnote is "check the vendor's current docs page before writing a selection memo." <!-- needs-research: specific current price points, context windows, and SLA percentages — these are the parts that move; cite from the provider's current docs page in any specific selection memo or inventory row. -->

### OpenAI

Flagship generative models under the GPT family (`gpt-4o`, `gpt-4.1`, `gpt-4o-mini`, and the `o`-series reasoning line), embeddings under `text-embedding-3-*`, audio and image under their own family names. API and Batch pricing published on the provider's pricing page. Enterprise tier via OpenAI Enterprise and via Microsoft Azure OpenAI; Azure's residency, SLA, and contract posture differ from OpenAI direct and are tracked as a separate inventory entry even though the model family is the same. DPA at `openai.com/policies/data-processing-addendum`. Enterprise-tier policy excludes API traffic from training by default; ZDR available for qualified customers. SOC 2 Type II letter available under NDA; the security portal lists current attestations.

### Anthropic

Flagship generative models under the Claude family (Claude Sonnet, Claude Opus, Claude Haiku, with version-and-date snapshots like `claude-sonnet-4-5` or similar; always cite the specific snapshot). API available direct from Anthropic, via AWS Bedrock, and via Google Cloud Vertex AI; the three routes have separate DPA / residency / SLA profiles and are three inventory entries even for the same model family. DPA at `anthropic.com/legal/dpa`. API inputs and outputs are not used to train Anthropic's models under the commercial Terms of Service (API Services terms). Anthropic publishes a *Responsible Scaling Policy* and a per-model system card (e.g. "Claude [version] System Card") worth reading before adopting the family on a new surface. Prompt-caching available; Message Batches API for discounted asynchronous workloads.

### Google (Gemini via AI Studio and Vertex AI)

Flagship generative models under the Gemini family (`gemini-1.5-pro`, `gemini-1.5-flash`, `gemini-2.0-*`, `gemini-2.5-*` lines; cite the specific snapshot). Gemini is available via Google AI Studio (developer-tier) and Google Cloud Vertex AI (enterprise-tier); Vertex is where enterprise residency, VPC Service Controls, CMK, and the enterprise DPA live. Google Cloud's data-processing terms are at `cloud.google.com/terms/data-processing-terms`; Vertex AI publishes supplementary "Generative AI on Vertex AI data governance" documentation worth reading alongside. Google DeepMind publishes a *Frontier Safety Framework* and Gemini technical reports / system cards worth tracking.

### Mistral

Frontier closed-weights models (Mistral Large, Mistral Medium, Mistral Small, Pixtral for vision, Codestral for code) available via Mistral's own API (La Plateforme) and via AWS Bedrock, Azure, Google Vertex. Several Mistral models are also open-weights (Mistral 7B, Mixtral 8x7B, Mixtral 8x22B, and others) available for self-hosting under Apache 2.0, with newer frontier releases under Mistral's own commercial research licence that differs — read the licence per model. European residency is a structural advantage for teams with EU-data posture; Mistral's EU-origin matters in some procurement conversations.

### Cohere

Command (generative), Embed (embeddings, multilingual), Rerank (retrieval reranker). Strong enterprise-posture (SOC 2, ISO 27001, HIPAA, PCI DSS available depending on tier; VPC / private deployment options). Available via Cohere's own API, via AWS Bedrock, Azure, and via on-prem / VPC. For many retrieval-heavy features, Cohere Rerank is the default reranker regardless of what generator sits downstream; keep it in the inventory as a specialist row even if Cohere is not your generator.

### Meta (Llama, open weights)

Llama 3, Llama 3.1, Llama 3.2 (multimodal), Llama 3.3, and the ongoing Llama family published under the Llama Community Licence (not OSI-open; permissive for most commercial use but carries revenue-threshold and acceptable-use clauses — read the current licence). Available for self-hosting on any inference stack and via inference platforms (Together, Fireworks, Groq, Cerebras, Databricks, AWS Bedrock, Azure, IBM watsonx). Llama's role in a portfolio is usually either (a) self-hosted for data-posture / cost / offline reasons (chapter 06) or (b) via an inference platform when a particular latency profile or price point beats the frontier closed options for a specific workload.

### DeepSeek, Qwen, and other open-weights labs

DeepSeek (DeepSeek-V3, DeepSeek-R1 reasoning line) and Alibaba's Qwen (Qwen2.5, Qwen2-VL for vision, Qwen2.5-Coder) are open-weights families that have been competitive with frontier closed models on specific benchmarks. The team's inventory should track them as self-host candidates; be deliberate about the licence (varies per model), the sovereignty posture (some teams have geopolitical reasons to screen specific labs — this is a governance decision above the AI Engineering Manager's altitude, surface it to counsel and security), and the Model Card for intended-use statements.

### Inference platforms and clouds

- **AWS Bedrock.** Federated access to Anthropic, Meta, Cohere, Mistral, AI21, Amazon's own Titan / Nova family, Stability, and others, under AWS's enterprise contract and IAM. The enterprise entry point for AWS-shop teams.
- **Azure AI Foundry (formerly Azure AI Studio / Azure OpenAI).** OpenAI, Meta, Mistral, Cohere, Databricks, and others under Microsoft's enterprise contract. The enterprise entry point for Microsoft-shop teams; Azure OpenAI's regional availability, data posture, and content-filter policy differ from OpenAI direct.
- **Google Cloud Vertex AI.** Gemini plus Anthropic (via a partnership), Meta, Mistral, and third-party models under Google Cloud's enterprise contract.
- **Databricks Mosaic AI.** DBRX plus third-party; strong when the data and the model live in the same lakehouse.
- **Together AI, Fireworks AI, Replicate, Modal.** Open-weights serving platforms; typical path to a cheap-and-fast Llama / Mixtral / Qwen production deployment without self-hosting.
- **Groq, Cerebras Cloud, SambaNova.** Specialist inference hardware with strong TPS / TTFT profiles on open-weights models (and, for some, their own frontier offerings). The right primary for a latency-floor-constrained feature in some workloads.

Each hosting path is a separate inventory row even when the underlying model name is the same, because DPA, residency, SLA, pricing, rate limits, and admin surface all differ.

## A worked example — the inventory row

A single row for one hosted model, as it might appear in the inventory. Values below are illustrative; a real row cites the vendor's current docs and the team's current eval run.

- **Identifier.** `claude-[snapshot-id]` via Anthropic API (direct). Separate row: same model via AWS Bedrock; separate row: same model via Google Vertex AI.
- **Context window.** Input up to the limit the provider publishes for this snapshot; output capped lower (also provider-published). Long-context quality measured on the team's long-context eval set; headline score noted.
- **Pricing.** Input $X / M, output $Y / M, cached input $Z / M (cache mode), batch $W / M. Dated `YYYY-MM-DD`. `<!-- needs-research: current numbers from anthropic.com/pricing on day-of -->`
- **Latency.** TTFT p50 / p95 and TPS at modal input length measured from team's eval run `[run-id]` dated `YYYY-MM-DD`.
- **Throughput.** Tier `[tier]` with `[RPM]` / `[TPM]` published default; negotiated ceiling via enterprise contract `[procurement reference]`.
- **SLA.** Published uptime SLA on the standard terms; enterprise-contract uplift via `[procurement reference]`.
- **DPA posture.** Training on customer data: not used under commercial terms. Retention: default / ZDR-eligible if contracted. Residency: `[regions]`. Sub-processors: per the vendor's public sub-processor list dated `YYYY-MM-DD`.
- **Enterprise features.** SSO yes; SOC 2 Type II letter available under NDA; HIPAA BAA available on `[tier]`; audit logs available; admin console available.
- **Capability tags.** `tool-use (parallel)`, `vision`, `prompt-caching`, `extended-thinking` (named per the vendor), `structured-output`, `computer-use` (where available / documented by vendor).
- **Eval evidence.** Behavioural `[score]` on golden set `[dataset-version]`; judge-based `[score]`; safety `[score]`; latency as above; cost per request at modal length `[cost]`. Run `[run-id]` dated `YYYY-MM-DD`; owner `[name]`.

## What good looks like

- The inventory is one document (a table, a wiki page, a markdown file with a schema); every production model your team calls appears there.
- Every row carries all ten dimensions. A row with an empty column is a flag, not a default.
- Rows are dated. The quarterly review (chapter 07) refreshes the numbers; staleness is visible.
- The same model via different hosting paths is multiple rows — API-direct, Bedrock, Azure, Vertex all get their own row because their DPA and operational posture differ.
- Capability tags are an index. A new feature's selection memo (chapter 03) opens the inventory and filters by tag.

## What bad looks like

- The inventory does not exist; "which models do we use" is answered by scrolling a billing dashboard.
- Rows are named by family ("Claude", "GPT-4") not by snapshot; two features on the same "Claude" are on different snapshots and nobody knows.
- Pricing, latency, and capability live in different docs (or different engineers' heads); the inventory does not join them.
- The inventory treats the model identity as the row and ignores hosting path; the DPA and residency columns are therefore wrong for half the entries.
- The inventory has never been refreshed; the price column is still last year's number and one row names a model that was deprecated three months ago.

## Summary

- The portfolio inventory is a flat table: every production-callable (model, provider, hosting-path) triple is a row.
- Each row carries ten dimensions: identifier and version, context window, pricing (input/output/cached/batch/fine-tune), latency, throughput, SLA, DPA posture (training / retention / residency / sub-processors), enterprise features, capability tags, eval evidence.
- Providers fall into four categories — frontier closed, frontier open-weights, enterprise/specialist, inference platforms/clouds — and a healthy portfolio has rows across at least two.
- Specific numbers move month-over-month. The shape and the cadence (chapter 07) are the part you own; the numbers get refreshed, dated, and cited from the vendor's current docs page.
- The inventory is the input to the per-feature selection memo in the next chapter and to the fallback / routing plan in chapter 04.

The next chapter picks up the second artefact: the two-page selection memo that commits one feature to one primary model (and one fallback) and defends the choice against the cost / latency / quality / safety trade-off.
