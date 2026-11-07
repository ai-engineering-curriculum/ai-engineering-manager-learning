# The Manager's Altitude for Budget, FinOps, and Build-vs-Buy

A team cost model is an engineering artefact. An ROI projection is a commercial artefact. A build-vs-buy call lives in the middle, and a quarterly portfolio allocation is the governance that keeps all three alive from quarter to quarter. The AI Engineering Manager is the person at the intersection of all four, and this module exists because the generic FinOps playbook that most engineering managers inherit was written for a world where compute scaled linearly with usage, where SaaS build-vs-buy had two options (buy a SaaS tool, or build an internal one), and where attribution was a sampling problem solved by tags.

AI engineering violates all three assumptions. Model-provider spend has five sub-lines with different unit economics (input tokens, output tokens, cached input, batch tokens, fine-tune tokens) and output-token pricing is often three to ten times input-token pricing; the build-vs-buy axis has a third option (fine-tune a vendor's base model) that is neither buying off-the-shelf nor building from scratch; and attribution has to work across workflows that call multiple models in a single user interaction. The classical FinOps canon still matters — it is the base this module assumes — but a manager who runs an AI engineering team against the classical playbook will ship a budget they cannot defend.

This first chapter sets the altitude. What the AI Engineering Manager owns on the budget-and-economics surface, what defers to Finance, Procurement, Counsel, and the Infra ladder, which five artefacts the rest of the module builds, and what the module deliberately does not re-teach.

## Motivation — five failure shapes when the budget-and-economics layer has no manager-owned surface

A team that imports a classical engineering-team budget template works, at first glance. A line for headcount, a line for cloud compute, a line for SaaS tools, a quarterly variance review with Finance. The bill is paid, the variance explanations are written, nobody flags it. Under the surface, five failures repeat.

- **The cost model that lumps provider spend.** The team's monthly invoice from the primary model provider is a single number in the budget spreadsheet. Nobody has broken it into input / output / cached / batch / fine-tune. When the CFO asks "why is this quarter up 40%", the manager cannot say whether the growth is in output-token volume (a product-adoption signal), cached input (a retrieval-rewrite signal), or fine-tune (a research-project signal that should route to a different budget bucket). The variance is a mystery because the attribution was never done. Finance stops trusting the AI engineering line.
- **ROI projections that only publish the hits.** Each quarter the team pitches three new initiatives with projected ROI; two land, one quietly misses. The next quarter's slide deck shows the two hits as case studies and makes no mention of the miss. Six quarters in, the organisation has an inflated model of AI-initiative success rate, future projections are compared against that inflated baseline, and the first honest slide — "this is why it missed" — reads as a crisis rather than as business as usual. The manager has lost the ability to project confidently because every projection is implicitly a bid against a false hit rate.
- **Build-vs-buy by vibes.** The team decides to fine-tune a vendor's base model because "the quality on our domain isn't good enough with prompting alone." No cost comparison against vendor-API with better retrieval. No reversibility analysis. No measurement of what "not good enough" actually meant on the eval suite. Nine months later, the fine-tune is live, it costs three times the vendor-API baseline, and the eval gap it closed could have been closed by a retrieval improvement. The decision was not wrong on evidence; it was never made on evidence.
- **The zombie initiative.** The product-triage board carries an initiative that launched as a prototype six months ago. It has not productised — never cleared the eval bar or the adoption threshold. It has not been wound down — no one has written the memo. Each quarter it carries one engineer's time and one line of model-provider spend. The accumulated cost is defensible item-by-item but is indefensible in aggregate; and because no one has published a wind-down decision, two more initiatives on the roadmap are implicitly waiting for the zombie to either resurrect or die before they get the attention they need. The zombie is not a technology failure; it is a portfolio-allocation discipline failure.
- **FinOps levers nobody pulled.** The team's inference bill is growing linearly with usage. The caching rate on the primary model has been 8% for a year. The context window being sent is 140k tokens for a task that needs 12k. Batch mode has never been enabled on the offline workflows that could use it. The levers are there — none of them require infra-platform depth to act on — but nobody is reading on them because the operating cadence has no FinOps beat. Six months later the CFO asks "why is the bill up 60% this quarter?" and the answer is "usage grew" when the honest answer is "usage grew *and* we never pulled any of the manager-owned levers."

All five failures look, at first glance, like finance or infrastructure problems. They are not; they are *manager-owned budget-and-economics surface* failures. The team was composed and operated against a template that pre-dates the surface it is paying for. This module gives the manager the AI-specific template.

## Core concept 1 — what this module does not re-teach

Everything on the shortlist below is assumed. If the reader has not internalised it, the referral is `ai-augmented-team-manager` (level 40), where the general engineering-management canon on budgets and vendor contracts lives.

- **Generic FinOps canon.** The FinOps Foundation's phases (inform, optimise, operate), classical cost-allocation discipline (tagging, showback, chargeback), AWS / Azure / GCP cost-explorer mechanics, unit-economics vocabulary. This module uses the vocabulary and specialises it; it does not re-derive it. If the reader has not read the FinOps Foundation framework or a practitioner reference (*Cloud FinOps*, Storment & Fuller) first, read it.
- **Engineering-manager budgeting mechanics at base.** How a quarterly budget is proposed, variance-reviewed, and reforecast; how headcount envelopes read against roadmap; how a SaaS tool budget line is justified. Owned by the level-40 sibling.
- **Classical build-vs-buy framing.** The two-column SaaS-vs-build decision, total-cost-of-ownership math, the standard vendor-lock-in discussion, the "buy the commodity, build the differentiator" heuristic (Joel Spolsky, *In Defense of Not-Invented-Here Syndrome*). This module adds the third axis (fine-tune) and the AI-specific reversibility reading; it does not re-teach the base canon.
- **The eval-gated SDLC and the vendor portfolio.** Owned by mod-104 and mod-105 of this track. The cost model's eval-tooling line reads against mod-104's registry; the build-vs-buy call reads against mod-105's portfolio inventory and open-weights memo. This module composes both; it does not re-teach them.
- **The team shape and operating cadence.** Owned by mod-109. The FinOps read (chapter 06) is a beat on that cadence; the portfolio-allocation review (chapter 05) is a quarterly beat; the ROI projection (chapter 03) is one of the artefacts the team's roadmapping rituals (mod-103) produce. This module sequences against the cadence; it does not design it.

If any of the above are unfamiliar, read them before reading this module. Everything here assumes the mechanics are in your hands and asks: *given those mechanics, how do you specialise them to the AI engineering surface?*

## Core concept 2 — the five manager-owned artefacts

The rest of this module builds five manager-owned artefacts. They map one-to-one to the learning objectives and to the chapters.

1. **The team cost model (chapter 02).** The written, dated model covering the six AI-engineering cost buckets — model-provider spend (split into input / output / cached / batch / fine-tune), labelling data, observability and eval tooling, training compute, infrastructure (serving, retrieval, vector store), and headcount — attributed along three axes (per feature / per user / per workflow) with a defensible allocation key for every shared line. The classical FinOps tagging discipline is the base; the specialisation is the AI-specific line splits and the workflow-level attribution that generic tagging does not produce.
2. **The ROI projection with confidence bands and the misses ledger (chapter 03).** The three-band (low / mid / high) projection per bounded initiative, with the assumptions behind each band written down, and the paired misses ledger that publishes the initiatives that landed below the low band with the reason. Specialised from classical value-realisation framing by the honesty discipline the non-determinism of AI outputs forces: a point estimate for an AI initiative's ROI is a dishonest estimate, and a hit list without the paired miss list is a dishonest hit list.
3. **The build-vs-buy matrix (chapter 04).** The written, dated matrix scoring the three AI-engineering axes — vendor-API, fine-tune, self-host — on six decision inputs (cost now, cost projected, performance, data posture, lock-in, owned complexity) and the reversibility cost of each. Specialised from classical two-column build-vs-buy by the third axis (fine-tune) and by the reversibility dimension, which the fine-tune and self-host axes carry more heavily than classical SaaS-vs-build.
4. **The quarterly portfolio allocation (chapter 05).** The written, dated quarterly review that places every active AI initiative in one of three bins — prototype, productise, wind-down — with the entry and exit criteria for each and the published reason for every wind-down. The specialisation is the refusal-to-carry rule for zombie initiatives and the discipline of publishing the wind-down decision with the reason the way a successful launch is published with the win.
5. **The FinOps lever set (chapter 06).** The five manager-owned levers — caching rate, batch utilisation, fallback routing, prompt-size discipline, context-window waste — with a measurement surface for each and the operating-cadence beat that reads on them. Specialised from generic FinOps by the explicit scope: these are the five levers a manager at this altitude can own without infra-platform depth, and the deferral to `ai-infra-inference-engineer` and the peer `ai-infra-team-lead` (level 45, Infra ladder) is as important as the levers themselves.

A sixth surface — the partnership with Finance and Procurement (chapter 07) — threads through all five. It is not a separate artefact; it is the operating discipline that makes the five artefacts land with the two external partners who sign off on them.

A team has a **manager-owned budget-and-economics surface** when: the cost model is on file with an allocation key for every shared line; each initiative on the roadmap has an ROI projection with confidence bands and a reason-for-misses discipline; the build-vs-buy call for every initiative that cost non-trivial engineering effort is on file in writing; the quarterly portfolio allocation is current and names every initiative's bin with explicit entry / exit criteria; the FinOps lever set has a measurement dashboard and a cadence beat; Finance and Procurement partnerships are named, current, and read the artefacts above as inputs.

## Core concept 3 — what the IC owns, what the manager owns

The pattern from the rest of the track repeats here. The IC owns craft; the manager owns the system in which craft is organised and funded.

| What the IC owns (craft) | What the manager owns (system) |
|---|---|
| Writing the prompt or agent or retrieval pipeline the feature uses; optimising it for quality against the eval suite. | Attributing the cost of running that prompt in production back to the feature it serves; defending the cost against the value (chapter 02 and chapter 03). |
| Measuring the latency and quality delta between two candidate models for a feature. | Choosing the build-vs-buy axis the team is on (vendor-API / fine-tune / self-host) and defending the choice against the six inputs; owning the reversibility cost of the chosen axis (chapter 04). |
| Benchmarking a fine-tune against a prompt-plus-retrieval baseline; writing the eval report. | Deciding whether the fine-tune earns the productise or wind-down bin this quarter; publishing the decision and the reason (chapter 05). |
| Implementing a prompt-caching pattern; shrinking a prompt; moving a background workflow onto the batch endpoint; writing the fallback-routing logic. | Reading on the caching rate, the prompt-size trend, the batch utilisation, the fallback-routing hit rate; naming the lever the team will pull this quarter; sequencing the lever into the operating cadence (chapter 06). |
| Writing the SDK retry logic that keeps the per-feature cost bounded during a vendor spike. | Negotiating the enterprise contract that bounds the per-token price the team is paying in the first place; running the renewal cadence with Procurement (chapter 07). |

The healthy pattern: *the manager sets the shape of the budget, the ROI narrative, the build-vs-buy direction, the portfolio bins, and the FinOps lever set; the IC fills each with the specific implementation that fits the surface; the manager reads the whole surface every quarter and reforecasts when the frontier has moved or an initiative has drifted.*

When the manager drifts into the craft column — "I ran the numbers on the fine-tune; let's do it" — the ICs feel steamrolled and the decision is usually under-evidenced. When the manager neglects the system column, the team runs into the five failures in the motivation section and the organisation stops trusting the AI engineering line.

## Core concept 4 — the four deferrals

Four partners the AI Engineering Manager explicitly defers to on this module. Each has standing authority the manager does not.

### Finance — cost-model attribution authority and the variance explanation of record

Finance owns the chart of accounts, the attribution-key canon the organisation enforces across all cost centres, the variance explanation that lands with the CFO, and the budget envelope the manager plans against. The manager owns the AI-engineering-specific cost model — the six buckets, the three axes, the allocation keys at the AI-engineering surface — and reads it against Finance's chart of accounts so the two reconcile. The partnership is bidirectional: Finance tells the manager what the organisational attribution rules are; the manager tells Finance what AI-engineering-specific lines Finance's generic rules do not natively carry. The manager does not write the variance explanation that Finance signs off on; the manager supplies the AI-engineering content Finance needs to write it honestly. Chapter 07 is specific about the partnership mechanics.

### Procurement — contract execution, vendor relationship, and renewal mechanics

Procurement owns the enterprise-licence negotiation mechanics — the sign-off path, the vendor's account-team relationship, the counter-signature, the renewal calendar. The manager owns the **asks** the procurement team negotiates on — the pricing tier the team can commit to, the discount justification against committed volume, the enterprise features the team needs (data-flow controls, higher rate limits, enterprise support, enterprise SLA, DPA addenda per mod-105 chapter 05), the deprecation-notice floor the team requires. The manager does not negotiate directly with the vendor's sales representative and does not sign the contract; the manager tells Procurement what to negotiate for and defends the asks when Procurement challenges them. Chapter 07 draws the line.

### Counsel — legal opinion on clauses

Reading a Data Processing Addendum and flagging the clauses that matter (training-on-data, data retention, residency, sub-processors, deletion rights, SLA, indemnity, deprecation notice) is the manager's job — mod-105 chapter 05 teaches it. **Writing a legal opinion** on whether a specific clause is enforceable in a given jurisdiction, under a specific regulatory regime, is counsel's job. A manager who drafts legal arguments is slowing the review; a manager who cites a legal position without counsel's sign-off is importing risk. The model: *read, flag, escalate; do not opine solo.* Chapter 07 reinforces the deferral.

### The Infra ladder — inference-platform depth

`ai-infra-inference-engineer` (IC) and the peer `ai-infra-team-lead` (manager, level 45, Infra ladder) own the inference platform: serving-stack tuning (vLLM / TensorRT-LLM / SGLang configuration, speculative decoding, continuous batching, FP8 / FP4 quantisation trade-offs), GPU capacity planning, autoscaling strategy, KV-cache implementation, the self-host serving tier when the build-vs-buy call lands on self-host. The AI Engineering Manager owns the **decision** to self-host a specific model for a specific product reason (chapter 04) and the **lever set** they can act on without touching the platform (chapter 06). The platform operation itself is Infra's. A manager who tries to tune the inference stack themselves will discover six months later that one engineer has become the single person who understands the serving layer; a manager who refuses to decide self-host because the Infra side is not their expertise will miss the four narrow cases where self-host earns its place (mod-105 chapter 06). The deferral is a boundary, not a wall.

A pattern to internalise: *the manager is the connective tissue between the technical direction (staff-plus ICs), the people substrate (People / HR, mod-108 and mod-109), the AI governance surface (mod-106, mod-107, mod-111), and the financial / commercial surface (Finance, Procurement, Counsel — this module).* Nobody else is sitting at the intersection of all four.

## Core concept 5 — what this module does not teach

Three deliberate exclusions, each a referral.

- **The IC-level cost-engineering craft.** This module does not teach how to implement a prompt-caching layer, how to tune a vLLM serving config, how to shrink a prompt with a technique like prompt compression or cache-aware prompting, or how to build a cost-attribution tagging layer at the SDK level. The IC-level craft lives in the IC role tracks (`applied-ai-engineer`, `agentic-ai-engineer`, `rag-engineer`, `ai-eval-engineer`, `fine-tuning-engineer`) and in the Infra ladder for the inference-platform depth. The manager recognises what good looks like; this module uses the recognition to defend the budget.
- **The inference-platform depth.** How to run a self-hosted model well, how to tune a serving stack, how to do GPU capacity planning at seven-figure scale — all Infra-ladder territory. Chapter 04 teaches the manager to make the self-host *decision* at commissioning-manager depth; chapter 06 names the *levers* a manager can own without platform depth. The platform depth itself lives one ladder over. If the reader is looking for "how to configure vLLM for a 70B model on an H100 pod," they are in the wrong module.
- **The organisational-finance canon.** How a chart of accounts is structured, how a variance review is run, how a financial close is produced, how CapEx vs. OpEx classification is decided in a specific organisation — Finance's territory. This module teaches the manager to have the conversation with Finance competently; it does not teach the Finance side of the conversation.

What the manager must know at depth from this module: the **six cost buckets** at AI-engineering depth and the **three attribution axes**; the **three-band ROI projection** shape and the **misses-ledger discipline**; the **three build-vs-buy axes** and the **six decision inputs** with reversibility named explicitly; the **three portfolio bins** and the **wind-down communication discipline**; the **five FinOps levers** and the **deferral boundary** to the Infra ladder; and the **two partnership shapes** with Finance and Procurement that make the artefacts land.

## The shape of the chapters ahead

- **Chapter 02 — Building the team's cost model honestly.** The six buckets at AI-engineering depth; the three axes; the allocation-key discipline; the three attribution-failure shapes and the fixes; a worked rollup for one AI product.
- **Chapter 03 — ROI projection with confidence bands and the misses ledger.** The value-realisation taxonomy; the three-band shape with named assumptions; the misses-ledger discipline; the translation to Finance-language that does not collapse the bands into a single number.
- **Chapter 04 — Build-vs-buy across vendor-API, fine-tune, and self-host.** The three axes; the six decision inputs with reversibility named; the pair-reads with mod-105 and mod-111; the three common mistakes; a worked matrix.
- **Chapter 05 — Quarterly portfolio allocation — prototype / productise / wind-down.** The three bins with entry and exit criteria; the quarterly review's six-item agenda; the zombie-initiative anti-pattern; the wind-down communication; a worked quarterly allocation.
- **Chapter 06 — FinOps for inference — the manager's levers.** The five levers with measurement and intervention; the deferral boundary; the operating-cadence beat; the FinOps dashboard the manager actually needs.
- **Chapter 07 — Partnering with Finance and Procurement.** The Finance partnership; the Procurement partnership; the deferrals; the four anti-patterns managers fall into.

## What good looks like

A team has a manager-owned budget-and-economics surface when:

- A written, dated cost model is on file; every shared line has an allocation key; every initiative on the roadmap can be read back to its cost attribution without a Slack thread.
- Every bounded AI initiative has an ROI projection with three confidence bands and named assumptions; a misses ledger is maintained next to the hits ledger; the two are read side by side at every quarterly operating review.
- Every initiative that cost non-trivial engineering effort has a written build-vs-buy call on the three axes with the six decision inputs scored; the reversibility cost of the chosen axis is named explicitly.
- The quarterly portfolio allocation is current; every initiative is in one of three bins; the wind-down decisions are published with the reason; no initiative has been "quietly running" across more than one quarterly review.
- The FinOps lever set has a dashboard the manager reads at the cadence mod-109 sets; the current-quarter lever intervention is on the roadmap with a measurable target; the deferral to the Infra ladder is explicit and current.
- Finance can read the AI-engineering cost lines back to the chart of accounts without a translation layer; Procurement knows the current renewal calendar, the enterprise-features ask list, and the escalation path to counsel.

## What bad looks like

- The provider invoice is one line on the budget; nobody has broken it into input / output / cached / batch / fine-tune; the variance explanation to the CFO is "usage grew."
- ROI projections are point estimates and only the hits are published; the misses quietly disappear from the next quarter's slide deck; the organisation has an inflated hit rate baked into every new projection.
- The current build-vs-buy direction is "whatever a senior IC proposed last quarter"; no written matrix exists; the reversibility cost of the chosen axis has never been named.
- Two initiatives have been "in prototype" for three quarters with no productise plan and no wind-down decision; the zombies carry engineer time and model spend; no one has written the reason either way.
- The caching rate and prompt-size trend are nowhere on the team's dashboard; the operating cadence has no FinOps beat; the Infra partnership is "we send them a ticket when something breaks."
- Finance does not trust the AI-engineering line; Procurement is surprised by every renewal; counsel sees the DPA for the first time two days before the contract ships.

## Summary

- Budget, FinOps, and build-vs-buy are the manager-owned layer that keeps AI engineering craft funded, attributed, and defensible against the economic surface the team operates on. The generic FinOps canon (FinOps Foundation, cloud cost-explorer mechanics, classical build-vs-buy framing) is the prerequisite; this module specialises it.
- The manager owns five artefacts: the cost model, the ROI projection with confidence bands, the build-vs-buy matrix, the quarterly portfolio allocation, and the FinOps lever set. A sixth surface — the Finance and Procurement partnership — threads through all five.
- Four deferrals: Finance (cost-model attribution authority and variance explanation of record); Procurement (contract execution and renewal mechanics); Counsel (legal opinion on clauses); the Infra ladder (inference-platform depth, `ai-infra-inference-engineer` and the peer `ai-infra-team-lead`).
- What this module does not teach: the IC-level cost-engineering craft (owned by the IC role tracks), the inference-platform depth (owned by the Infra ladder), the organisational-finance canon (owned by Finance).

The next chapter opens the first manager-owned artefact: the written, dated team cost model, broken across the six AI-engineering buckets and attributed along the three axes that a honest AI-engineering budget requires.
