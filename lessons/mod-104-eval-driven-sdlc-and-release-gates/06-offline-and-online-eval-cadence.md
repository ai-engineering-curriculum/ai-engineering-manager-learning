# Offline and Online Eval Cadence

Pre-merge evals (chapter 03) catch the regressions you expected to catch. They cannot catch the regressions production shows you — changed user population, changed query distribution, upstream model drift, retrieval corpus drift, tool-backend changes you did not see coming. Online evals exist to catch *those*. This chapter is about the five-stage cadence the manager owns — pre-merge, canary, A/B, shadow, continuous production — the eval-delta thresholds that gate deploy and that page on-call, and the four drift shapes you must be able to distinguish when a monitor fires.

The manager does not instrument the trace, implement the monitor, or bisect the regression. The manager publishes the cadence, sets the thresholds, picks what pages whom, and defends the response when a monitor fires in the wrong week.

## Motivation

Common shapes of online-eval failure:

- **Launched with pre-merge only.** Pre-merge suite passes; feature launches; a week in, users submit a class of query the golden set never captured; quality is visibly bad in support tickets but no eval signal exists. The team learns about the regression from customer success.
- **Canary that is not an eval.** A canary rollout exists — 10% traffic for a day — but no eval runs on the canary traffic. The canary is a traffic-shaping vehicle, not an evaluation vehicle. By the time anyone notices, the canary has been promoted and the regression is at 100%.
- **Continuous monitoring with no drift taxonomy.** A dashboard shows a quality score sliding downward. Nobody knows whether the generation got worse, the judge drifted, retrieval regressed, or the tool backend changed. The team spends two days bisecting the wrong surface.
- **Pager without a runbook.** An online eval wakes on-call at 02:00. The on-call engineer sees "quality below threshold." They do not know what to do. They acknowledge, sleep, and the system is still bad in the morning.

The right posture is five explicit stages, each with a purpose, a threshold, a response, and a clear ownership of the four drift shapes.

## Core concept 1 — the five stages

Each stage answers one question. Together they produce a layered defence where each stage catches the regressions the prior stage missed.

### Stage 1 — Pre-merge (offline)

Answers: *Does this change regress the suite we already have?*

Mechanism: the two-tier PR + release gate from chapter 03. The release gate is the heaviest offline test the team runs. It is deterministic, judge-mediated, and runs on the golden dataset. It gates whether a release *candidate* can become a *release*.

Thresholds (recap from chapter 03): defined per metric, with regression allowances and absolute floors.

Owner: author, eval engineer, manager (for the override surface).

### Stage 2 — Canary (online, phased)

Answers: *On a small slice of production traffic, does this release behave the way the pre-merge evals predicted it would?*

Mechanism: route a percentage of production traffic (1% → 10% → 50% → 100%) to the new version over a phased window (hours to days, depending on the surface's traffic volume and risk). On the canary traffic, run a subset of online evals in near-real-time: the judge-based quality rubric, latency, cost, safety-category rates. Compare against the prior version's performance on the matched slice.

A canary is an **eval** only if it is instrumented: traces land in the observability store (`Langfuse`, `LangSmith`, `Braintrust`, `Arize` — whichever was commissioned in chapter 02), a sampling policy produces a review slice, and a judge runs against that slice.

Thresholds: the canary auto-promotes only if the measured delta on the canary slice is within the release-gate's regression allowance, and no absolute floor is broken. Measured on an N large enough to be a signal — for low-traffic surfaces this may mean extending the canary window; a canary that promotes after 12 requests is theatre.

Owner: release engineer / on-call + the manager for the promotion decision.

### Stage 3 — Shadow (online, mirror)

Answers: *Does the new version behave acceptably on the full production distribution — without exposing any user to it?*

Mechanism: in parallel with the production serving path, run the new version on the same inputs and log its outputs without returning them to the user. Judges run offline on a sampled fraction of shadow outputs, compared to the production output on the same input (pairwise preference or absolute scoring).

Shadow is the best tool for changes where the surface is too risky for canary — a safety-posture change, a tool-authorisation change, a model swap in a regulated flow. It is also expensive: you pay for every call twice. Use it selectively.

Owner: the applied engineer running the shadow + eval engineer analysing the paired samples + manager for the decision to promote.

### Stage 4 — A/B (online, assignment)

Answers: *Does the new version produce better user-observable outcomes than the old — over a window large enough to decide?*

Mechanism: randomly assign users (or sessions, or workspaces — pick a unit that respects the product's unit of behaviour) to A (control) or B (treatment). Each cohort experiences one version. Collect outcome metrics (task completion, user satisfaction, downstream action, churn) and judge-based quality metrics over the window. Report the delta with a confidence interval.

A/B is where business outcome evidence lives. Behavioural evals say "this is correct"; A/B says "this is useful." A release that improves behavioural evals but hurts A/B outcome signal is a release that is better at the wrong thing.

Thresholds: pre-register the A/B's primary outcome, the window, and the decision rule. Do not stop early because "the trend is good"; `Hamel Husain` and others have written extensively on how peeking wrecks A/B evidence. Multiple comparisons on secondary metrics require a correction.

Owner: the applied engineer running the experiment + data-analyst peer (if one exists) + the product partner. Manager approves the primary-outcome choice and the window.

### Stage 5 — Continuous production monitoring

Answers: *Right now, in production, is the system within expectations — and if not, what shape of drift is it?*

Mechanism: on live production traffic, a constant pipeline runs. Sampling policy picks a fraction of traces for judging (1% is a common starting point; sampling fraction scales with risk and budget). Judges score the samples; aggregates land on a dashboard; thresholds fire alerts.

This stage distinguishes **monitoring thresholds** from **paging thresholds**:

- **Monitoring thresholds** draw a line on the dashboard and ping the team channel. Not a page. Triaged during business hours.
- **Paging thresholds** wake on-call. Reserved for safety-category drift, hard-floor cost / latency breaches, and quality drops large enough that the next wave of users will suffer.

Owner: on-call rotation — applied engineer + the manager for policy.

Continuous monitoring is where the four drift shapes (next concept) are distinguished. The dashboard that cannot distinguish them is a dashboard that produces two days of bisection every incident.

## Core concept 2 — the four drift shapes

An LLM product has four independent surfaces that can regress. The manager's dashboard, runbook, and paging policy must distinguish them, because the response to each is different.

### Drift 1 — Generation-quality drift

The model's outputs got worse, independent of retrieval and tools. Causes: provider silently updated the model behind the alias (`gpt-5`, `claude-sonnet-4-6`); the system prompt interacted badly with a new model behaviour; the input distribution shifted in a way the prompt was not written for.

Signal: judge-score drop on the production sample, correlated across many case types; retrieval metrics unchanged; tool-use correctness unchanged.

Diagnosis path: pin the model version (not the alias) and re-run on the stable production slice from last week. If quality returns, the model's behind-the-alias has moved — contact the provider (and see `mod-105` on pinning strategy). If quality does not return, the input distribution has moved and the prompt / few-shot / judge may need adjustment.

Runbook owner: applied engineer + eval engineer.

### Drift 2 — Judge drift

The *scoring* changed, not the system. Causes: the judge model behind an alias moved; the judge prompt was edited without recalibration (chapter 04); the calibration set is stale and the judge's agreement with humans has drifted even if the judge itself did not.

Signal: the judge-based quality metric moved but behavioural (task-success) metrics did not; the judge–human agreement on a fresh labelling sample shows the judge has drifted.

Diagnosis path: pull a fresh sample of (input, output, human-score) triples. Compare judge score to human score. If judge–human agreement has fallen below the policy floor, the judge is the signal that moved, not the system. Recalibrate or roll back the judge version.

This is the drift shape that most often fools teams new to eval-driven delivery. "Quality dropped" and the team looks for a model regression; the actual problem is that the yardstick changed.

Runbook owner: eval engineer.

### Drift 3 — Retrieval drift

The retrieval system (RAG) regressed. Causes: the corpus was re-indexed with a different chunking strategy; the embedding model was updated; the corpus itself drifted (new documents added, old ones not deprecated); the re-ranker was replaced; the vector index was repacked.

Signal: retrieval metrics (recall@k, precision@k, context precision) moved; faithfulness / answer-relevance moved (because the retrieval is now feeding worse context to the generator); generation-only metrics (judged on a generation-only path with no retrieval) did not move.

Diagnosis path: run the retrieval suite against the known-answerable set. If recall / precision degraded, investigate at the retrieval layer. If retrieval numbers held but faithfulness dropped, the generator is confabulating around good retrieval — a generation-side problem that looks like retrieval.

Runbook owner: RAG engineer + applied engineer.

### Drift 4 — Tool-use drift

The agent's tool interactions regressed. Causes: a tool backend's schema changed silently; a tool backend's latency rose, making timeouts more frequent; a prompt change moved the agent's tool-selection behaviour; a model change moved tool-choice reliability.

Signal: tool-use correctness metrics moved; downstream outcomes (ticket-updates, right-tool-called) moved; behavioural quality on *non-tool-using* paths did not move.

Diagnosis path: look at tool-invocation traces. Is the agent calling the wrong tool? Is it calling the right tool with wrong arguments? Is it recovering from tool failures? Is the tool backend returning differently? Separate "agent picked wrong tool" from "right tool returned garbage."

Runbook owner: agentic engineer + the tool's downstream-service owner.

### The cross-cutting discipline

A drift that is *several* of the four simultaneously is often a single root cause (a model change under an alias can degrade generation *and* tool-use; a corpus update can look like retrieval *and* generation-quality drift). The diagnosis path above runs in parallel; the convergence is the real cause.

The manager-owned artefact here is the **drift-triage runbook** — one page per drift, one page for the cross-cutting pattern, linked from the pager's acknowledge message so the on-call engineer has the path at 02:00. Without this, on-call pages produce acknowledges and no mitigations.

## Core concept 3 — eval-delta thresholds that gate deploy vs. that page on-call

Two decision policies share thresholds but mean different things. The manager pins both explicitly.

### Deploy-gating thresholds (the release gate — chapter 03)

Point-in-time decision: *should this candidate become a release*. Thresholds expressed as:

- Regression allowance per metric (with a CI).
- Absolute floors (non-overridable for safety).
- Overridable floors with remediation plans (cost, latency).

The threshold is tight because the alternative to shipping the candidate is to not ship it; a tight threshold is survivable.

### Pager-firing thresholds (continuous monitoring)

Trend-over-time decision: *has production drifted enough to warrant waking a human*. Thresholds are different:

- **Short-window ceiling.** "Rolling-hour prompt-injection resist rate below 95% → page." Reserved for safety and cost/latency hard floors.
- **Day-over-day delta.** "Daily faithfulness delta -0.05 vs. the seven-day baseline → page." Catches genuine quality drops.
- **Week-over-week trend.** "Seven-day rolling faithfulness continuously below the fourteen-day median by 2% for three consecutive days → page." Catches slow creeps.
- **Monitoring-only thresholds.** "Latency p95 up 10% for a day → post to #ai-ops channel, not a page." Noise-tolerant signals go here.

The page-firing threshold is **more tolerant** than the deploy-gate threshold for the same metric, because the alternative to paging on-call is to let the degradation continue — and a page is a disruption to a human. Pages should fire on drops the on-call engineer could not reasonably wait until morning to see.

The split matters for safety. The deploy gate may require faithfulness ≥ 0.85 with -0.02 regression allowance. The pager may fire only when production faithfulness falls below 0.82 for a sustained window. Setting the page-threshold identical to the gate-threshold produces alert fatigue; setting it dramatically wider produces missed regressions.

## Core concept 4 — sampling, cost, and the practical shape of online evals

Online evals are not free. The manager's job is to size them.

The levers:

- **Sampling fraction.** 1% of production traffic is a common default. For very low-traffic surfaces, go higher; for very high-traffic surfaces, lower. The number must be high enough that the daily sample is statistically useful.
- **Judge frequency.** Judges are LLM calls — expensive. Options: run the judge on every sampled trace (most honest, most expensive); run a cheap classifier judge (keyword / regex / small-model) on every sampled trace and escalate to an LLM judge on a subset (common cost compromise); run the full judge on a smaller sub-sample.
- **Sampling stratification.** Random sampling from the overall stream misses rare-but-important slices. Stratify by audience, surface, or input shape so each slice has enough samples. A tool-using agent should sample tool-call traces separately from no-tool traces.
- **Budget visibility.** The online-eval line item lands in `mod-110-budget-finops-and-build-vs-buy`. Teams that treat it as free end up throttling it when the cost shows up in a quarterly bill.

The cadence table — the manager-owned artefact — summarises this in a page. Columns: stage, mechanism, sampling, judge, threshold-type, who it pages. Rows: the five stages. See the worked example below.

## Core concept 5 — tying the cadence to the release-triad and incident response

Two cross-module couplings:

- **The release-triad gate (`mod-106` chapter 06).** The release gate (stage 1) + canary + shadow stages compose the model+data+safety release-triad sign-off. A triad sign-off is only credible if the stages have been genuinely run — not "we checked the dashboard."
- **AI incident response (`mod-107`).** The paging thresholds above produce the raw pages that `mod-107`'s on-call rotation handles. The drift-triage runbook is the first page of the incident playbook. An incident response without a drift taxonomy guesses; one with the four drifts can diagnose.

The manager owns the connection: this chapter's cadence feeds `mod-106`'s triad and `mod-107`'s on-call. If the connection is weak, each module's artefacts become theatre.

## A worked example — the cadence table, for one feature

Returning again to the doc-Q&A agent.

| Stage | Mechanism | Sample / scope | Judging | Threshold | Fires on |
|---|---|---|---|---|---|
| Pre-merge PR gate | CI (chapter 03) | 50-case behavioural subset + contract tests, cached | None (deterministic) | Regression ≥ 1 point on subset → PR blocked | PR check (author) |
| Pre-release gate | CI, release candidate (chapter 03) | Full 200-case suite + red-team + retrieval + tool-use | Ragas faithfulness + answer-relevance judge, 3 runs | Per-metric regression allowance + absolute safety floors | Release blocker (manager override log) |
| Canary | 1% → 10% → 50% → 100% over 48h | Canary traffic, matched slice vs. prior | Judge on 10% of canary traces | Faithfulness delta within -0.03 vs. prior; latency p95 within target; safety floor unbroken | Canary promotion vote (release engineer + manager) |
| Shadow | Parallel run of new version on full production (when applicable, e.g., model swap) | Full production mirror, 24h | Pairwise judge on 2% sample | Pairwise preference ≥ 50%; safety floor unbroken | Promotion decision (manager) |
| A/B | 50/50 assignment by workspace, 14-day window | All eligible workspaces | Primary outcome: ticket deflection rate; secondary: judge faithfulness | Pre-registered win criterion with CI | Decision on retention vs. rollback (manager + product partner) |
| Continuous production monitoring | Always-on | 1% sampling, stratified by audience + tool-use/no-tool | Lightweight keyword + LLM judge on 10% of samples | Short-window prompt-injection resist < 95% → page; daily faithfulness -0.05 vs. 7-day baseline → page; latency p95 up 10% → channel-only | On-call rotation (applied engineer, escalates to eval engineer for judge drift) |

Drift-triage runbook (one page per shape) lives at `runbooks/ai-ops/drift-{generation,judge,retrieval,tool-use}.md`, linked from every page the alerting platform issues.

Owner of the cadence table: the manager, reviewed quarterly against incident evidence and the data flywheel (chapter 07) — case classes the flywheel discovers often trigger cadence updates.

## What good looks like

- There are five named stages. Everyone on the team knows which stage they are responsible for and when each one runs.
- Canary is instrumented; canary traces land in the observability store; a judge runs on canary traffic and the promotion vote reads it.
- A/B pre-registration is documented; nobody peeks and stops early.
- The dashboard distinguishes the four drift shapes; the runbook for each is one click from an acknowledge.
- Pages are rare and specific — they fire on signals the on-call can act on before dawn, not on noise.
- Quarterly cadence review surfaces: "we removed a stage because evidence says it was theatre," or "we added a new monitor because an incident last quarter had no alert."

## What bad looks like

- Only a pre-merge stage. Online is "we'll know if customer success tells us."
- Canary that is just traffic-shaping — no eval runs on the canary traffic.
- A/B peeked at daily "to see how we're doing," decisions made on noise.
- Dashboard with one quality number that cannot distinguish a judge drift from a generation drift.
- Pages that fire on "latency changed by 1 ms"; alert fatigue sets in; the real safety-floor page is silenced during the fatigue.
- No drift runbook; on-call acknowledges and sleeps; the issue is bisected the next day by the eval engineer.

## Summary

- Five stages, each answering a different question: pre-merge (offline), canary (phased online), shadow (mirrored online, selective), A/B (randomised outcome), continuous production (always-on monitoring).
- Four drift shapes to distinguish: generation-quality, judge, retrieval, tool-use. Each has a signal pattern, a diagnosis path, and a runbook.
- Deploy-gating thresholds and pager-firing thresholds are different policies. Tight for gates; tolerant-of-noise but safety-floored for pages.
- Sampling, judge frequency, and stratification size the online-eval cost. The cost is a budget line, not a free good.
- The cadence table is a manager-owned one-page artefact. Review quarterly against incidents and flywheel evidence.

The last chapter picks up the data flywheel (R13) — how production, red-team, and preference data become the labelled corpus that feeds both the evals this chapter runs and the fine-tunes `mod-105` and `mod-106` may call for.
