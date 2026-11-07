# Per-Feature Model Selection and the Selection Memo

The inventory in chapter 02 tells you *what is callable*. This chapter is about committing *one feature* to *one primary model* (and one fallback — see chapter 04) with a two-page written memo that stands up to a quarterly portfolio review, a procurement renewal, an incident postmortem, and a new engineer joining the team six months from now. The selection memo is the manager's defense artefact: when the eval engineer runs the numbers, the memo is what the manager signs and files.

Model selection is where every one of the four trade-off axes — cost, latency, quality, safety — collides. Teams that skip the memo and just "use what we already use" end up either over-paying (always on the flagship for a surface that would meet SLA on a cheaper model) or under-shipping (always on the cheap one for a surface that quietly fails the safety floor). The memo exists to force the trade-off into the open and date the decision.

## Motivation

Three failures recur on selection decisions without a written memo.

- **Cost tunnel-vision.** The manager asks "which is cheapest" and lands on the smallest model in the family that passes the behavioural eval, ignoring that the behavioural eval didn't cover the long-input edge cases that will dominate production traffic. Six months in, p99 quality on long inputs is terrible; the team blames the model but the selection memo (if it existed) would have forced the long-input edge-case eval into the decision.
- **Latency tunnel-vision.** The manager asks "which is fastest" and lands on a specialist inference provider for a surface that didn't actually need interactive latency. The team is now paying a premium for latency the product never exposes to users, and the provider's model-set is thinner than the frontier alternative.
- **"Big model always wins."** The manager defaults to the highest-end flagship from the lab they trust most, reasoning that quality dominates everything. The feature works. The feature's unit economics are then wrong at the scale the finance team planned for, and the manager discovers in mod-110 that the budget line does not close.

The two-page memo prevents all three by naming the trade-off explicitly: each of the four axes carries a stated weight, a stated threshold, and a stated piece of evidence.

## Core concept 1 — the feature profile that drives selection

Before the memo picks a model, it describes the feature. A feature's shape determines which axes dominate. Four shapes that come up often:

- **Interactive, synchronous, user-facing.** A chatbot turn, a "autocomplete this" surface, a generate-while-you-type UX. Latency dominates: p95 TTFT matters more than peak quality on long form. Cost matters per call and is amplified by volume. Safety floor is high because responses are shown directly.
- **Asynchronous, batch, back-office.** Overnight summarisation, data extraction from a document corpus, offline evaluation. Cost dominates per-token (and batch pricing is the primary lever); latency is loose (minutes, hours); throughput and rate-limit headroom matter more than TTFT.
- **Agentic, tool-using, multi-turn.** A workflow where the model plans, calls tools, consumes outputs, plans again. Tool-use correctness (did it call the right tool with the right arguments?) dominates. Quality on structured output and tool-schema conformance matters more than prose quality. Safety enters as excessive-agency (OWASP LLM06) and prompt-injection (LLM01) risk via tool outputs.
- **Retrieval-augmented (RAG).** The model reads a corpus-retrieved context and produces a grounded answer. Faithfulness-to-retrieved-context dominates (see mod-104 chapter 02 on `Ragas` metrics). Long-context quality matters if the retrieved context is long. Cost is often dominated by the prompt-caching story (chapter 02 inventory dimension).

The feature profile is one paragraph at the top of the memo. It is not optional and it is not generic — "a chatbot" is not a profile; "a multi-turn chatbot for customer-support reps that must cite the KB and must respond within 3 seconds p95" is.

## Core concept 2 — the four axes, each with a weight, a threshold, and evidence

A selection memo weighs the four axes explicitly. Not every feature weighs them equally; the memo states the weights up front so the trade-off is visible.

### Cost

- **The question.** What does a representative call cost at the modal input length and the modal output length, on the candidate model? What is the cost per month at projected traffic?
- **The structure.** Cost = (input tokens × input rate) + (output tokens × output rate) − (cache-hit savings) + (reasoning tokens, if a reasoning-mode model) + (tool-call overhead, if the model is agentic). Caching discounts are a material lever on repeat prompt-prefix workloads; see chapter 02's pricing axis.
- **The evidence.** Cost is measured on the eval set at the modal input and output length, not in a vacuum. The eval engineer produces a cost-per-call column in the eval report.
- **The failure mode.** Picking a cheap model to save 60% per token and shipping a 3× longer output because the cheap model is less concise — net cost goes up. The memo compares total cost at eval-measured output length, not per-token list price.

### Latency

- **The question.** What is TTFT p50 / p95 / p99 and TPS on the candidate model at modal input length? What does the product need?
- **The structure.** TTFT dominates for streaming UI (user sees the first word fast). TPS dominates for non-streaming surfaces where the whole response is awaited. On an agentic surface, total latency is a sum of model + tool-call + model again, so a model's TTFT matters once per turn and TPS matters N times.
- **The evidence.** Latency is measured on the same provider endpoint and the same region the production deployment will use, not from a different continent. Specialist providers (Groq, Cerebras, SambaNova) have produced TTFT and TPS numbers substantially better than frontier-closed APIs on specific open-weights models; the trade-off is model identity and roadmap stability.
- **The failure mode.** Measuring latency from a developer laptop in a different region to the production-serving region, then being surprised at p95 in production. The eval engineer runs latency measurements from the production environment or a representative proxy.

### Quality

- **The question.** Does the model meet the behavioural, judge-based, retrieval, and tool-use thresholds in the eval suite (mod-104 chapter 02)?
- **The structure.** Quality is not a single number. For a RAG surface, faithfulness (Ragas), answer relevance, and context precision are all first-class. For an agent, tool-use correctness and schema conformance are. For a summariser, judge-based rubric scores dominate. The memo cites the specific eval categories and the specific thresholds.
- **The evidence.** The eval report. The memo does not re-run the evals; it cites them, including the run id, the date, and the dataset version. If the eval evidence is older than the inventory's refresh date (chapter 07), the memo re-opens a run before it is signed.
- **The failure mode.** Judging quality on aggregate numbers and ignoring that the aggregate hides a tail where the model fails on the specific input distribution production will see (long inputs, specific languages, specific domains). The eval set must be representative of production, and if it is not, the memo says so.

### Safety

- **The question.** Does the model meet the safety floor the feature's risk posture requires (mod-104 chapter 02, section 3 — NIST GenAI Profile and OWASP LLM Top 10)?
- **The structure.** Safety is not one number either. The red-team / adversarial case family's results are the headline. The provider's upstream safety stance — its Model Card, its Responsible Scaling Policy / Preparedness / Frontier-Safety framework, its usage policy — is a secondary input. For regulated features, the evidence set escalates (BAA on file, legal sign-off attached).
- **The evidence.** Safety eval numbers and the Model Card reading (chapter 07). For regulated features, the governance sign-off (`ai-risk-engineer` or the organisation's equivalent).
- **The failure mode.** Picking a model whose safety evals pass on generic jailbreak prompts but whose Model Card declines to warrant it for the audience the feature serves. The memo reads the Model Card's intended-use statements, not only the benchmark numbers.

## Core concept 3 — the selection memo's shape

The memo is two pages of markdown. No more, no less. A six-page memo is a design doc. A one-paragraph memo is a Slack message. Two pages forces the right discipline and still gets read.

### Section 1 — Feature profile (one paragraph)

The feature paragraph from core concept 1 above. What the feature is, who uses it, the shape (interactive / batch / agent / RAG / mixed), the volume projection (requests per day), the latency target the product needs, the audience and the risk posture (one-line pin to mod-104's commissioning brief).

### Section 2 — The candidates considered (half a page)

A short table: three candidates minimum, from at least two different providers or hosting paths (otherwise you are not genuinely considering alternatives). For each candidate, the inventory row's headline numbers (context, pricing, latency profile, DPA posture) and a one-sentence note on why it is a plausible fit for this feature.

Candidates that are plausible but screen out on a non-negotiable (no EU residency, no HIPAA BAA, wrong capability tag) are listed with a one-sentence exclusion. Writing the exclusion forces the reader to see what was considered rather than wonder what was overlooked.

### Section 3 — The trade-off, with weights and thresholds (half a page)

The four-axis table. For each axis:

- **Weight** — low / medium / high / blocking. "Blocking" means failing this axis disqualifies regardless of the other three.
- **Threshold** — the number from the eval suite. Cost: dollars per call at modal length. Latency: p95 TTFT in milliseconds. Quality: the headline eval metric and the floor. Safety: the adversarial-case pass rate and the absolute floor (mod-104 chapter 03 taught the vocabulary).
- **Evidence** — the eval run id and date, or `[placeholder: run to be commissioned before memo is signed]` with an action.

The weights are not equal by default. A batch back-office feature weights cost high and latency low; an interactive user-facing feature weights latency high and cost medium. The memo justifies the weights in one sentence each.

### Section 4 — The recommendation (two short paragraphs)

Which candidate is the primary, which is the fallback (chapter 04), and why. The *why* is a direct read of the trade-off table: "primary picked for highest-weighted-axis score; fallback picked for alternative-provider and comparable quality." Hand-waving in this section is the main failure mode; keep it to a direct read.

### Section 5 — The exit criteria (one paragraph)

Under what conditions would we re-open this decision? Examples: "If the eval score on `[named category]` regresses by more than X over two consecutive quarterly reviews." "If the DPA posture on the primary changes materially (e.g. sub-processor added without notification period honoured)." "If a new entrant's eval score on `[named category]` exceeds primary's by Y with no material cost or latency regression." "If the fallback is exercised more than Z times in a quarter."

Exit criteria that are hypothetical ("if it ever gets worse") fail the memo. Exit criteria that are specific numbers against the eval surface pass.

### Section 6 — Ownership and review date

- **IC owner.** The applied engineer (or eval engineer) accountable for the eval evidence this memo cites.
- **Manager owner.** You. Signs the memo.
- **Review date.** The next quarterly portfolio review in which this memo is re-opened (chapter 07). Default: next quarter's review; earlier if the exit criteria flag.

## Core concept 4 — the three selection anti-patterns

Three failure shapes come up often enough on manager-reviewed memos that they are worth naming and defeating in the memo's structure.

### Anti-pattern 1 — "The team is already in the SDK"

"We're already using Provider X's SDK everywhere; it would be a lift to switch." This is a *cost of change* argument, not a quality / cost / latency / safety argument. It can be legitimate — switching costs are real — but the memo must say so explicitly, under its own line item ("switching cost: estimated at `[eng-weeks]`; absorbed by the alternative's advantage in `[axis]` over `[horizon]`"). If the switching cost is the only reason, the memo has made a lock-in call; chapter 04's abstraction layer discipline is the fix.

### Anti-pattern 2 — "The new model is better on `[benchmark]`"

A vendor releases a new model and the public benchmarks look striking. The memo cannot cite public benchmarks — they measure the model, not the feature. The memo cites the team's eval suite. If the eval has not been run yet, the memo says `[placeholder: run commissioned, due YYYY-MM-DD]` and is not signed until the placeholder resolves.

### Anti-pattern 3 — "Safety is default-handled by the vendor"

The provider's upstream content filter or safety classifier is treated as if it discharges the team's safety obligation. It does not. The feature's safety posture is what the team's own eval and the organisation's own governance sign off; the vendor's filter is a *layer*, not the *floor*. Chapter 05 of mod-106 (the release triad) is specific about this. The memo cites the team's own red-team evals; it does not cite the vendor's marketing.

## A worked example — the memo, filled in

The hypothetical customer-support agent from mod-104 chapter 02:

- **Feature profile.** Multi-turn tool-using agent for internal CS reps; drafts customer-facing responses with KB citations; p95 target 3s; ~5000 calls/day projected; audience internal; drafts sent to external customers; risk posture pinned to NIST confabulation + OWASP LLM01 / LLM02 / LLM06 (mod-104 chapter 02 brief).
- **Candidates considered.**
  - Anthropic `claude-sonnet-[snapshot]` via Anthropic API direct. Headline: strong tool-use, prompt-caching, EU residency available, HIPAA BAA available.
  - OpenAI `gpt-4o-[snapshot]` via Azure OpenAI (team is already on Azure for storage). Headline: strong tool-use, enterprise contract via existing Azure agreement, content-filter default-on.
  - `llama-3.3-[size]` via `Together AI` (open-weights, hosted). Headline: lower list-price per token, longer time-to-fallback than closed, weaker tool-use parity.
  - Excluded: `gemini-[snapshot]` via Vertex AI (plausible, excluded because the team has no existing Google Cloud contract and procurement cycle time exceeds this quarter's launch window).
- **Trade-off, weights, thresholds, evidence.**
  - Cost: medium weight. Threshold $0.04 / call at modal length. Evidence: eval run `[id]`, dated `[YYYY-MM-DD]`.
  - Latency: high weight. Threshold p95 TTFT < 1500ms, total p95 < 3s. Evidence: latency eval from production region.
  - Quality: high weight. Thresholds — behavioural ≥ `[floor]`, Ragas-faithfulness ≥ 0.85, tool-use correctness ≥ 0.95. Evidence: eval run `[id]`.
  - Safety: blocking weight. Prompt-injection "resists" rate ≥ 95%; sensitive-data-leakage = 0% absolute floor. Evidence: red-team case family `[id]`.
- **Recommendation.** Primary: Anthropic `claude-sonnet-[snapshot]` via Anthropic API direct. Rationale: highest score on tool-use correctness and faithfulness; meets latency with margin; EU residency and HIPAA BAA satisfy audience-sensitivity notes; prompt-caching reduces per-call cost at modal RAG input. Fallback: OpenAI `gpt-4o-[snapshot]` via Azure OpenAI; comparable quality, different provider; abstraction layer per chapter 04.
- **Exit criteria.** Reopen if (a) Ragas-faithfulness on monthly online eval regresses by more than 2 points for two consecutive months; (b) Anthropic sub-processor list changes materially without the DPA-required notice period; (c) Azure OpenAI's content-filter posture changes such that the fallback's behaviour diverges from primary's on safety-case cases.
- **Ownership.** IC owner: A. Patel (eval engineer). Manager owner: `[manager name]`. Next review: `[date of next quarterly portfolio review]`.

This is two pages filled. The decision is specific, defensible, and dated.

## What good looks like

- Every feature that commits to a model has a two-page selection memo on file, dated, signed by the manager, citing eval evidence.
- The memo considers at least three candidates from at least two providers or hosting paths; excluded candidates get a one-sentence exclusion in writing.
- The trade-off table carries weights, thresholds, and evidence — not adjectives.
- Exit criteria are specific numbers or specific contract events; "if things change" fails.
- The memo cites the team's eval suite, not a public benchmark.
- Primary and fallback (chapter 04) are picked in the same memo; features with no fallback are flagged as SPOF.

## What bad looks like

- Model choice is in a Slack thread or a hallway conversation; "we use GPT-4" is the memo.
- The memo cites public benchmarks ("outperforms X on MMLU") rather than the team's eval.
- Cost is the only axis considered because "the eng team said it worked"; safety and latency are not weighed.
- The memo names "Claude" or "GPT-4" rather than a specific snapshot; the team is one undated alias change away from an unreviewed model swap.
- No exit criteria. The decision is never re-opened and the team is still on the first model on the first feature three years in.
- Only one candidate was considered, framed as a memo rather than a decision document.

## Summary

- The selection memo is a two-page artefact the manager signs that commits a feature to a primary (and fallback) model with explicit weights, thresholds, evidence, exit criteria, and ownership.
- The feature profile (interactive / batch / agent / RAG) determines which of the four axes — cost, latency, quality, safety — dominate; the memo states the weights.
- Three candidates minimum, from at least two providers / hosting paths; eval evidence from the team's own suite, not from public benchmarks.
- Three anti-patterns to name and defeat: switching-cost-as-justification, new-benchmark-as-justification, vendor-safety-as-justification.
- Exit criteria are specific numbers or specific contract events; the memo is re-opened on the quarterly cadence (chapter 07) and whenever an exit criterion fires.

The next chapter picks up the companion artefact: the fallback and multi-model routing plan — what happens when the primary model is down, degraded, or quota-limited, and the discipline that keeps the fallback meaningful rather than theoretical.
