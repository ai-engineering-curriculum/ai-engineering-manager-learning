# The Guardrails Tech Landscape at EM Depth

Chapter 02 wrote the spec; this chapter walks the shelves. An AI engineering manager does not benchmark Llama Guard against OpenAI Moderation in a weekend — that is the `applied-ai-engineer`'s job. But the manager has to know the shelf well enough to recognise a bad choice in a PR, to ask the right questions when a vendor proposes a change, and to tell procurement which safety services are even on the table.

Treat this chapter the way chapter 02 of mod-105 treats the frontier portfolio landscape: as a map at commissioning-manager depth, not a benchmark. Pin the current version of the vendor documentation; everything in this space moves quickly.

## Motivation — the three bad picks a map prevents

- **The "we already have a classifier, so we're covered" pick.** The team wires an off-the-shelf moderator that was designed for general chat content and ships it on a code-generation assistant. The classifier has never seen a request to "write a function that reads /etc/passwd" and does not flag it. The team is covered against "write a hateful comment about my coworker" and uncovered against the actual harm shape of the product. The spec from chapter 02 should name the harm shape; this chapter names which classifier matches which harm shape.
- **The "one layer is enough" pick.** The team picks exactly one safety control — input moderation, or output moderation, or structured validation, or a system-prompt discipline — and treats the rest as redundant. The first adversarial example that routes around that one layer lands in production. Guardrails are defence-in-depth; the question is which layers for which surface, not whether to have layers.
- **The "safety is deterministic" pick.** The team treats the classifier as a binary gate (score > threshold = block). On a well-calibrated classifier this is correct for the extremes and badly wrong in the middle. A map that includes "HITL queue" and "degraded-capability fallback" as valid tier-two responses — not just block / allow — is a map that lets the spec breathe.

## Core concept 1 — the five categories of guardrail tooling

At EM depth, the market organises into five categories. Memorise the categories; each spec from chapter 02 ends up picking one or more items from each.

### Category A — orchestration frameworks

A guardrail orchestration framework sits in the application path, intercepts inputs and outputs, and runs a set of **flows** or **rails** against them. It does not do the detection itself; it calls classifiers, regex, LLM-judges, or custom logic in a declared order and decides whether to let the request through, to rewrite it, to refuse, or to escalate.

- **NVIDIA NeMo Guardrails** — an open-source toolkit that lets you express input / output / dialogue / retrieval rails in a declarative language (Colang). Composes with multiple classifiers and LLM-based checks. Commissioning-manager read: pick when the product wants rail logic as code rather than hand-rolled chains in the application, when many surfaces share the rails, or when the compliance story needs "our safety logic lives in one declarative file."
- **Guardrails AI** — an open-source library that validates LLM input and output against type- and content-level validators (regex, classifiers, custom rules), with a hub of pre-built validators. Smaller surface than NeMo; useful when the primary need is **structured-output validation** and content-level checks, not full rail orchestration.
- **Framework-native rail primitives** — LangChain / LangGraph interrupts, Vercel AI SDK middleware, Mastra guardrails hooks. These are framework-local; they do not transfer across stacks but are "free" if the stack is already chosen.

Commissioning-manager rule: if the product has one or two surfaces and the rails are simple, framework-native is fine. If the product has many surfaces or a shared guardrail posture across a platform, invest in NeMo (or an equivalent) so the rails do not diverge silently.

### Category B — content-safety classifiers, hosted

A content-safety classifier is a model you call to score an input or output against a category list. The hosted options:

- **OpenAI Moderation** — free API, returns scores against a published category list (hate, harassment, self-harm, sexual, violence, including minors-specific variants). Commissioning-manager read: broad default moderator; useful on any LLM surface regardless of model provider; low operational cost. Published policy on data use for moderation calls applies — check the current version.
- **Azure AI Content Safety** — hosted classifier covering hate / sexual / violence / self-harm plus prompt-shield (direct and indirect injection attempts), protected-material detection, groundedness checks, and custom categories. Commissioning-manager read: the most **enterprise-complete** offering as of the current inventory — SOC 2, EU residency, custom categories, prompt-shield feature. Useful when procurement already has Microsoft and the spec needs the injection-detection and groundedness features in the same place.
- **Google Cloud Vertex AI safety features** — built into the Gemini API responses (safety ratings per category) and available as standalone classifiers via Vertex and the Responsible AI Toolkit. Useful when the product is already on Vertex and the native-provider path is preferred.
- **Perspective API (Jigsaw)** — toxicity-focused, free API, strong coverage on the specific "comment toxicity" distribution (for UGC / community features). Older than the generative-AI-era moderators; still the strongest pick when the task really is comment moderation.

Commissioning-manager rule: pick the hosted moderator whose category coverage matches the spec's section 1 risk model; pick two if the risk model requires one for input and a different one for output, or if the DPA posture (mod-105 chapter 05) rules a given hosted service out for a given data class.

### Category C — content-safety classifiers, self-hosted or open-weights

When data residency, cost at scale, or offline / air-gapped operation precludes a hosted classifier, or when the categorical taxonomy the team needs is narrower than any hosted classifier covers, an open-weights safety classifier is the pick.

- **Meta Llama Guard** — a family of Llama-based safety classifiers that classify both input prompts and output responses against a configurable taxonomy, with newer releases adding multimodal and more granular category support. Commissioning-manager read: the de facto default for self-hosted content safety; the taxonomy is customisable, which is uniquely useful when the product's harm shape does not map cleanly to a hosted service's categories.
- **Meta Prompt Guard** — companion lightweight classifier specifically for prompt-injection and jailbreak detection, intended to run in front of the primary model. Commissioning-manager read: pair with Llama Guard when the risk model has a specific prompt-injection leg; do not treat it as a replacement for a general content moderator.
- **ShieldGemma** — Google's open-weights safety classifier family trained for content-policy detection.
- **Granite Guardian** (IBM) — IBM's open-weights safety classifier family, with categories targeted at the enterprise risk model.

Commissioning-manager rule: self-host when the DPA / residency / cost / taxonomy constraint is real; when it is not, the hosted services in category B come with less operational cost. The deferral to the Infra ladder (mod-105 chapter 06) applies: the manager does not operate a Llama Guard deployment personally.

### Category D — structured-output validators and schema enforcers

These sit on model output and enforce shape, grammar, or domain validity. Not a moderator — a validator. The spec's section 3 names at least one if the output is machine-consumed.

- **Pydantic / Zod / JSON Schema** — the baseline; call the model with a structured-output mode or a tool-call-with-schema, then validate.
- **Instructor / Outlines / Guidance / XGrammar** — libraries that enforce the schema *during* generation (constrained decoding) rather than after, reducing retry rates on structured-output tasks.
- **Guardrails AI validators** (category A overlap) — library of reusable content-level validators the output is run against (e.g. profanity check, PII scrub, URL allow-list).

Commissioning-manager rule: for any output consumed programmatically, this category is non-negotiable. The spec names the schema and the behaviour on validation failure.

### Category E — prompt-injection and jailbreak-specific detectors

Specialised classifiers for the single OWASP category most likely to bite a shipping product.

- **Meta Prompt Guard** (also category C) — built for this task.
- **Lakera Guard** / **Protect AI (Rebuff, LLM Guard)** — commercial offerings covering prompt-injection and broader LLM-security scanning; useful when the product is high-value or the security team wants a specialised vendor on the ticket.
- **Pattern-based detectors** (within NeMo Guardrails, Guardrails AI, or custom) — regex / dictionary matchers for known injection markers. Low recall against novel attacks; fine as a cheap first-pass filter when paired with a model-based detector.

Commissioning-manager rule: on any surface where retrieval or tool input brings untrusted content into the context window, the spec names a detector here. On surfaces where the only input is the direct user and the user has no incentive to attack their own agent (internal tools where auth binds the request to the user's identity), the risk is lower but non-zero.

## Core concept 2 — defence in depth: three layers, each decidable

Never pick one tool from the five categories. Pick a layered posture — at minimum three layers — and decide each.

Layer 1 — **input moderation and injection detection.** One hosted or self-hosted classifier (category B or C); one injection detector (category E) if retrieval or tool input brings untrusted content into context. The spec's section 2.
Layer 2 — **generation-time structuring and grounding.** Structured-output mode when the output is machine-consumed (category D); grounded-generation mode when retrieval evidence exists; constrained decoding when the schema is strict. The spec's sections 3 and 4.
Layer 3 — **output validation and content moderation.** Schema validation (category D); content-safety classifier on the output (category B or C); PII / secrets redaction. The spec's section 3.

Layer 4 (when the product warrants it) — **human-in-the-loop checkpoints** (category A orchestration gates a HITL queue when either classifier returns a mid-range score). The spec's section 5.

An AI engineering manager's job on this layering is to *require* all layers on surfaces whose severity tier demands them, and to call out single-layer designs in review.

## Core concept 3 — the pick-rules at commissioning-manager depth

The five rules the manager applies when reviewing an IC's choice.

### Rule 1 — match the classifier to the harm shape

"Toxicity" is not "safety." A toxicity classifier trained on Reddit comments will not reliably catch a request to write a function that reads a sensitive file. A moderator trained on general chat will not catch a request to exfiltrate a prompt via indirect injection. The spec's section 1 names the harm shape; the classifier chosen in sections 2 and 3 should cover it. If it does not, the pick is wrong.

### Rule 2 — honour the DPA

The input moderator and the output moderator are themselves API calls that carry the user's content. Their DPA posture matters. If the product's content cannot leave EU residency, the moderator cannot either. If the vendor's moderation calls are not covered by the product's enterprise data-use terms, they may not be allowed. The DPA-reading discipline from mod-105 chapter 05 applies here — including to the safety services, not just to the primary model.

### Rule 3 — define failure behaviour before deployment

Every classifier can be down. Every classifier has tail latency. The spec's error-on-classifier-failure line (chapter 02) is the manager's non-negotiable. The common postures:

- **Fail-closed on input moderation** — safer; appropriate when the severity tier is HIGH.
- **Fail-open on input moderation with reduced capability** — user experience is preserved on classifier outage; appropriate when severity is LOW to MEDIUM and the downstream model has its own safety training to fall back on.
- **Fail-closed on output moderation always** — an output that cannot be moderated should not be shown.
- **Timeout treated as failure** — a classifier that returns in 60 seconds is a classifier that did not return. The timeout threshold is part of the spec.

### Rule 4 — structured-output validation is cheaper than output moderation

For machine-consumed output, constrained decoding + schema validation is a cheaper safety mechanism than a post-hoc moderator in two senses: it fails fast, and the thing it is validating is deterministic (schema conformance) rather than a model's opinion (does this score above the threshold). If the output is a tool call or a JSON payload, invest in category D before investing in a heavier category B.

### Rule 5 — a safety classifier is not an eval and is not a judge

Three different roles the manager must keep straight. A **safety classifier** is an online inference call in the production path that decides whether a specific request / response is unsafe. An **eval case** is an offline test that verifies the system's end-to-end behaviour on a known input (mod-104 chapter 02). An **LLM-as-judge** is an offline (or sampled-online) scorer that produces a numeric quality rating. A single classifier can appear in all three — Llama Guard as the online guardrail, Llama Guard in the eval CI, Llama Guard in a judge chain — but the roles have different error tolerances and different review cadences. The spec names which role the classifier plays; the review surface (chapter 05 of mod-104) tracks it.

## Core concept 4 — operational concerns the manager must name

Four operational dimensions the spec should settle before deployment. These are where the Infra ladder's deferral (mod-105 chapter 06) and the SRE discipline apply.

- **Latency.** Each guardrail adds p50 and p95 to the user-perceived response time. On an interactive surface, three serial classifier calls can double the time-to-first-token. The spec notes which guardrails run in parallel, which in sequence, and whether streaming is interrupted by the output moderator (common patterns: run output moderation on windows of the stream, or run a cheap classifier inline and a stronger one on completion).
- **Cost.** Each hosted classifier is a line item. Each self-hosted classifier is a GPU-hours line item. The quarterly FinOps review (mod-110) should see the guardrail stack's spend as its own line, not hidden in "inference."
- **Rate limits and SLA.** Hosted classifiers have their own rate limits and SLAs. These matter at traffic peaks and during provider incidents. The fallback posture from mod-105 chapter 04 extends to safety services: a hosted moderator can go down; the plan names what the product does when it does.
- **Observability.** Every classifier decision is a log event. Dashboards track: fire-rate per category, refusal rate per surface, HITL queue depth, classifier latency, classifier error rate. These are what on-call reads during a safety incident (mod-107).

## A worked skeleton

For the Healthwise Chat spec from chapter 02, the chosen stack at skeleton depth:

| Layer | Component | Vendor / tool                              | Posture                   |
|-------|-----------|--------------------------------------------|---------------------------|
| 1 — input moderation | Content classifier | OpenAI Moderation (hosted)         | Fail-closed; 2s timeout   |
| 1 — injection detect | Prompt-injection check | Meta Prompt Guard (self-hosted)    | Fail-open-reduced (input treated as untrusted); 500 ms timeout |
| 2 — generation    | Grounded generation | RAG with structural separation of retrieved content | Spec section 2 enforces   |
| 3 — output moderation | Content classifier | OpenAI Moderation (hosted)         | Fail-closed; 2s timeout   |
| 3 — output validation | PII regex pass     | custom validators (Guardrails AI hub)       | Redact; log redaction event |
| 3 — grounding     | Citation-coverage check | LLM-judge against retrieved sources (eval-registry judge) | Regenerate once; refuse with "no trustworthy source" |
| 4 — HITL          | Crisis-queue router | Custom (routes to crisis-line vendor)       | SLA 90s; auto-handoff on breach |
| Orchestration     | Rails framework   | Application-level (middleware)              | Rails code-reviewed; single file per surface |

Five tools, three categories, four layers; all driven by the spec from chapter 02. The manager signs on the picks in the table; the applied-ai-engineer owns the thresholds and the implementation.

## Anti-patterns to name and defeat

- **"One moderator does everything."** A single moderator at the input is not defence in depth. Output is where many of the real failures emit.
- **"We built it ourselves to save money."** A homemade regex-only input filter is a false sense of security; a homemade safety classifier without an eval against a golden set is unknowable in production. If the team self-hosts, they self-host an open-weights model with a known evaluation; the homemade version is a smell.
- **"We'll turn safety on at launch."** The guardrail stack is a feature of the product; it is built on day one, not bolted on when the risk assessment is read for the first time at the launch review.
- **"Safety services don't need a fallback."** They do. A classifier outage during a provider incident is a safety outage; the mod-105 chapter 04 fallback discipline includes the safety services.

## Summary

- The guardrails tech landscape organises into five categories: orchestration frameworks (NeMo Guardrails, Guardrails AI, framework-native), hosted content-safety classifiers (OpenAI Moderation, Azure AI Content Safety, Vertex / Gemini safety features, Perspective), open-weights self-hosted classifiers (Llama Guard, Prompt Guard, ShieldGemma, Granite Guardian), structured-output validators (Pydantic / Zod / Instructor / Outlines / Guidance), and injection-specific detectors (Prompt Guard, Lakera, Protect AI).
- Defence in depth is non-negotiable: minimum three layers (input moderation + injection detection, generation-time structuring and grounding, output moderation and validation), HITL as a fourth layer where severity demands it.
- The manager's job is to pick-rules: match the classifier to the harm shape; honour the DPA; define failure behaviour before deployment; prefer structured validation over post-hoc moderation for machine-consumed output; keep the three roles of safety classifier / eval case / LLM-judge distinct.
- Operational concerns — latency, cost, SLA, observability — are first-class in the spec; the Infra ladder's deferral applies, but the manager names the posture.

The next chapter picks up the second manager-owned artefact: the red-team commissioning brief, and the discipline of reviewing findings so they land as fixes rather than as unactioned reports.
