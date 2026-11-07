# Fallback and Multi-Model Routing

A product with one vendor has one vendor's incidents. Every frontier provider has had multi-hour outages; every provider has had capacity-contended periods where rate limits are the active constraint; every provider has had regional incidents that hit some customers and not others. If your product has no fallback path, each of these is a product outage. The manager's job in this chapter is to require a fallback for every production surface, keep the fallback tested, and set the policy that decides what "degraded but shipping" means.

A fallback that has never been exercised is a liability pretending to be an asset. The chapter is about building the ones you can defend.

## Motivation

Three specific failure shapes recur when no fallback plan is in writing.

- **The six-hour outage.** The primary provider has a multi-hour incident. Your product is down for the duration. On-call cannot bring it back because there is no alternative path. Leadership asks the AI Engineering Manager why no fallback existed; the answer, every time, is "nobody asked us to build one."
- **The silent quota incident.** A traffic spike pushes the account over its rate limit. Calls return 429s. The application does not route traffic elsewhere because there is no elsewhere; it either retries into the same limit (making it worse) or surfaces errors to users. Most providers publish per-minute and per-day quotas; most production incidents from quota exhaustion are predictable.
- **The silent degradation.** The primary model is up and responding, but something has changed — a model version has rolled, an upstream content filter has shifted behaviour, retrieval quality has drifted — and the fallback would have scored better on the online eval that day. Without a fallback wired in and an online-eval-driven routing trigger (mod-104 chapter 06), the product rides the degradation until a user complains.

The fallback plan solves all three. The multi-model routing policy — which model handles which traffic, under which conditions — is the operational envelope that makes the plan real.

## Core concept 1 — the four fallback shapes

Not every "fallback" is the same thing. Four distinct shapes, picked per surface based on what degradation the product can tolerate.

### 1. Same-family downgrade

The primary is a flagship; the fallback is a smaller model from the same family on the same provider. Example: primary is a flagship model, fallback is the small / mini / haiku variant of the same provider. Only mitigates *model-level* incidents (capacity contention, flagship-specific rate limits) and *cost spikes*. Does **not** mitigate provider-level outages — if the provider is down, both are down. Cheapest to build; weakest to depend on. A reasonable first fallback for a cost-sensitive surface; an insufficient one for a surface where provider outage is the primary risk.

### 2. Cross-provider swap

The primary is on one provider; the fallback is a comparable model on a different provider. Example: primary is Anthropic `claude-[snapshot]` on Anthropic's API; fallback is OpenAI `gpt-[snapshot]` on Azure OpenAI. Mitigates provider-level outages, provider-level quota exhaustion, and provider-specific regional incidents. Carries two obligations: (a) the fallback must have an eval score comparable to primary on the surface's quality thresholds (or a documented degradation the product tolerates), and (b) the DPA posture of the fallback must match the surface's data posture — a fallback whose DPA permits training on your data when primary's does not is not a usable fallback for sensitive surfaces.

### 3. Cached-response or degraded-mode

The primary is a generative model; the fallback is a non-generative degradation: cached-response lookup, FAQ-style template response, "I can't help with this right now — would you like to be routed to a human?" refusal. Mitigates total loss of generative capacity when no provider fallback is operational. Correctly scoped is often the right fallback for a surface where a wrong answer is worse than no answer — safety-sensitive or compliance-sensitive surfaces. The product UX specification covers what this looks like; the manager's job is to require it.

### 4. Hard-offline refusal

The primary is unavailable and no fallback is appropriate; the product refuses the request with a clear user-facing message. Sometimes the right call: a feature that must be correct (safety-regulated, compliance-regulated) and whose correctness depends on a specific model that is currently unavailable may be better off refusing than degrading silently. The manager's job is to pre-authorise this in writing so on-call does not improvise at 2 a.m.

A production feature usually needs at least two of these in combination: cross-provider swap as primary fallback, hard-offline refusal as the last resort if both are down.

## Core concept 2 — the four routing policies

Routing is the live decision — for any given request, which model answers it. Routing is distinct from fallback: fallback is *what happens when the chosen model fails*; routing is *how we chose the model in the first place*. Four common policies; a surface may combine them.

- **Quality-first (default routing).** Every request goes to the primary; fallback engages only on failure. Simplest; easiest to reason about; appropriate for most production surfaces. Chapter 03's selection memo named a primary — this is routing to it.
- **Cost-first routing.** Requests classified as "easy" go to the smaller / cheaper model; "hard" requests escalate to the flagship. The classifier can be rule-based (length, presence of certain tokens, tool-use requirement) or model-based (a small model does a cheap triage). Has a *routing-quality eval* obligation: the classifier's decisions must be measured separately, because a classifier that routes 10% of hard requests to the cheap model is a quality regression that the primary-model eval won't catch.
- **Latency-first routing.** Requests with hard latency floors (user-visible interactive surfaces at specific times of day; synchronous tool-call sub-steps) go to the fastest option; others go to primary. Specialist fast-inference providers (Groq, Cerebras, SambaNova) earn their place here.
- **Capability-first routing.** The classifier decides which capability the request needs (vision, tool-use, long context, extended thinking) and routes to the model that has it. Reasonable for a multi-feature surface; carries the same routing-quality eval obligation as cost-first.

A hybrid is common: quality-first for the default surface, cost-first for the high-volume subset where cost dominates, latency-first on a specific interactive sub-feature, capability-first for the vision sub-feature.

## Core concept 3 — the abstraction layer

Every application call should go through a thin abstraction, not the raw provider SDK. The abstraction is manager-mandated (chapter 01); the implementation is the IC's. Three reasons:

- **Swap-out is a config change, not a rewrite.** When an exit criterion (chapter 03) fires and a model is swapped, the change should be in one place.
- **Routing policy lives in one place.** Cost-first, latency-first, and capability-first classifiers live here; application code does not sprinkle `if hard: flagship else: cheap` across call sites.
- **Observability lives in one place.** OpenTelemetry GenAI semantic conventions (mod-104 chapter 06), eval sampling, trace emission, structured-output parsing — all consolidate in the abstraction layer.

The industry has produced several options; the manager picks one or writes a thin one. At manager depth:

- **LiteLLM.** Open-source Python library that normalises the API shape across ~100 providers; serves as both a library and an on-prem proxy. Strong default for teams that want to swap providers without re-coding per-provider SDK calls. Ships cost tracking, retry logic, structured outputs, and routing.
- **LangChain / LangGraph.** The dominant application-framework path. Comes with routing primitives and callback-based observability; strongly opinionated — the abstraction brings a lot of the rest of its surface with it.
- **Portkey AI, OpenRouter.** Managed gateways that sit in front of multiple providers; handle fallback, routing, caching, cost tracking. Fine for teams willing to put a third-party in the hot path; may not fit teams whose DPA posture requires no-third-party-in-the-path.
- **Vercel AI SDK, Mastra, and others.** Framework-specific options; useful when the application is already in that framework.
- **A thin internal wrapper.** Thirty lines around each provider's SDK plus a shared interface. The right call on teams that do not want framework lock-in and have the bandwidth to maintain it. The weakest option when the team is small and the surface grows fast.

Whichever is picked, two non-negotiables: (a) every production call goes through it; (b) the abstraction emits traces in a convention compatible with the eval and observability stack (mod-104).

## Core concept 4 — the triggers that engage fallback

Fallback engages on signals, not on a vibe. Four signal shapes, written into the routing policy:

- **Hard failure.** HTTP 5xx, connection error, timeout. Fallback engages immediately on a configurable count (common pattern: engage after 2 consecutive failures within a short window, do not engage on a single transient).
- **Rate-limited.** HTTP 429 (or provider-specific equivalent) and header-reported quota exhaustion. Fallback engages when sustained for more than a threshold (brief 429s during a spike are often recoverable with exponential backoff; sustained ones are not).
- **Quality degradation (online eval triggered).** The online-eval cadence (mod-104 chapter 06) reports the primary's judge-score has fallen below a threshold on recent production samples. Fallback engages via configuration change (not auto), after a quality-review sign-off from the manager.
- **Content policy block.** The provider's upstream content filter has refused the request where the team's own safety case indicates it should have answered (or vice versa). Fallback engages per-request when the pattern is detected.

Each trigger has a **restore condition** — the reverse signal that returns traffic to primary. Fallback modes that engage but never disengage are indistinguishable from permanent swaps. The incident-response runbook (mod-107) owns the triage and restore.

## Core concept 5 — the data-posture constraint on cross-provider fallback

A cross-provider fallback only works if the fallback's DPA posture is **as strict or stricter than** the primary's. Three specific failures to design against:

- **Training-on-data mismatch.** Primary excludes training on your API data under enterprise contract; the fallback's default tier permits it. If the fallback engages, data that was never meant to train anyone's model just entered a training set. The fallback must sit on the same tier of posture as the primary.
- **Residency mismatch.** Primary serves from EU; the fallback serves from US only. If the fallback engages for an EU-residency-required feature, the data has crossed a residency boundary the DPA promised it would not. The fallback must match the residency commitment.
- **Sub-processor disclosure mismatch.** The fallback's sub-processor list differs from the primary's. Engaging the fallback exposes customer data to a sub-processor the customer was not notified of (depending on the team's own customer DPA language). The manager's job is to ensure sub-processors across primary and fallback are both disclosed upstream.

The fallback's DPA posture is a row in chapter 02's inventory for this specific reason. A fallback whose posture is looser is not actually a fallback for sensitive surfaces; it is a theoretical SPOF-shield that fails the moment it engages.

## Core concept 6 — the tabletop and the restore drill

A fallback is real only if it has been exercised. Two cadenced rituals.

### The tabletop

Quarterly, the AI Engineering Manager runs a thirty-minute tabletop with the IC owner and the on-call rotation: "primary provider is down for four hours; walk the steps." The tabletop produces one page of notes — steps missing from the runbook, config values that nobody knew to flip, people who were not reachable, dependencies on the primary that nobody had noticed (e.g., the embedding model is also on the primary and the retrieval layer silently breaks). Chapter 07's quarterly review consumes the output of the tabletop.

### The restore drill

Twice yearly, actual traffic is routed to the fallback for a scheduled window (small percent, growing to the surface's "degraded-but-shipping" tolerance). The drill produces real numbers — did eval scores hold? Did latency hold? Did cost explode? Did any downstream system fail because it assumed primary's response shape? The drill is coordinated with mod-104's online eval cadence; the fallback's quality numbers from the drill are what make the chapter 03 selection memo's fallback choice defensible.

The drill is not a chaos-engineering stunt; it is a scheduled, announced exercise with on-call engaged and rollback ready. The manager's job is to put it on the calendar; the IC's job is to run it.

## A worked example — the fallback plan, filled in

For the customer-support agent from chapters 02 and 03:

- **Shape.** Cross-provider swap (primary: Anthropic via Anthropic API direct; fallback: OpenAI via Azure OpenAI). Plus cached-response for a small set of top-10 FAQ patterns the product can serve offline. Hard-offline refusal is the last-resort for the "update ticket status" tool path if both are unavailable.
- **Routing policy.** Quality-first default; cost-first applied only to the "classify ticket category" sub-call (a smaller model is sufficient and measured separately).
- **Abstraction layer.** Internal thin wrapper (team is small; LiteLLM reviewed and deferred for later). Every production call routes through it; OTel traces emit per `gen_ai.*` conventions.
- **Triggers.**
  - Hard failure: two consecutive 5xx or timeouts within 60 seconds routes to OpenAI/Azure for the surface, with a 5-minute cooldown before reattempting primary.
  - Rate-limited: sustained 429s (> 30 seconds) route the surface to fallback for 10 minutes before reattempting.
  - Quality degradation: online-eval monitor on faithfulness; drop below threshold triggers a page, not an auto-swap; manager's sign-off engages fallback for the subset.
  - Content policy: not expected on this surface (no safety-case collision anticipated); monitored only.
- **Data-posture check.** Primary and fallback both no-training; both HIPAA-BAA-eligible; both EU-residency-capable. Sub-processor lists documented and surfaced to customer DPA notice.
- **Tabletop.** First tabletop scheduled for Q`[X]`; owner A. Patel; on-call rotation represented; output page filed with next quarterly review.
- **Restore drill.** Scheduled for Q`[X]` + 1, 2% of traffic for one hour, growing to 10% if eval scores hold. Observability pre-wired; rollback runbook in place.

The plan is one page. The incident runbook that composes it (mod-107's territory) adds specifics.

## What good looks like

- Every production feature has a fallback plan on file, naming the shape (same-family / cross-provider / cached-response / hard-offline), the triggers, and the restore conditions.
- Primary and fallback both appear in the inventory (chapter 02) with DPA postures that match the surface's data posture.
- Every production call routes through an abstraction layer; raw-SDK calls from application code are a code-review failure.
- The tabletop ran this quarter; the restore drill ran last half.
- When a provider has an outage, on-call engages the fallback via a runbook step that was last rehearsed in the last six months.
- The fallback is in chapter 03's selection memo — primary and fallback are picked together, not bolted on later.

## What bad looks like

- "We'll figure it out if the provider goes down." No fallback plan.
- Fallback exists in theory but has never been exercised; the first time it engages in production, three things break at once.
- Cross-provider fallback is configured, but the fallback's DPA permits training on customer data; the moment it engages, the organisation is in breach of its own customer DPA.
- Fallback engages automatically on a single 5xx and thrashes; the product spends more time on fallback than on primary because the triggers are too twitchy.
- Fallback engages but never restores; the team is on "temporary" fallback for six months and the original primary selection memo is orphaned.
- Application code is sprinkled with provider-SDK calls; swapping any model requires a PR to fifteen files.

## Summary

- Every production feature needs a fallback: same-family downgrade, cross-provider swap, cached-response degradation, or hard-offline refusal — or a combination.
- Routing policies (quality-first, cost-first, latency-first, capability-first) are chosen per surface; cost-first and capability-first carry a routing-quality eval obligation.
- Every call goes through an abstraction layer — LiteLLM, LangChain, a managed gateway, or a thin internal wrapper; raw provider-SDK calls from application code are a lock-in gift.
- Triggers are signals (5xx, 429, online-eval threshold, content-policy block), and each has a restore condition; fallbacks that engage but never disengage are permanent swaps.
- Cross-provider fallback requires DPA posture parity — training-on-data, residency, sub-processors. A looser-posture fallback fails the moment it engages.
- Quarterly tabletops and twice-yearly restore drills keep the plan real; a fallback that has not been exercised is a liability pretending to be an asset.

The next chapter picks up the third artefact: reading a Data Processing Addendum, the redline list the organisation defends, and the partnership with procurement and counsel that executes the contract.
