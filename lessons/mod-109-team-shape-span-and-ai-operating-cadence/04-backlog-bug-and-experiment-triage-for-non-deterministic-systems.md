# Backlog, Bug, and Experiment Triage for Non-Deterministic Systems

Classical engineering triage has a well-trodden shape. Bugs are classified by severity, by component, by reproducibility, and by priority; feature work is ranked against a roadmap; investigation work is sized against uncertainty. The practice is taught in every mature engineering organisation and is the prerequisite for this chapter (level-40 sibling territory, carried in *An Elegant Puzzle* and the GitLab handbook). The third manager-owned artefact of this module adapts the practice to the AI engineering surface, where three distinctive properties make classical triage insufficient on its own:

1. A production regression's root cause is often not a line of code the team changed; it is a *behaviour* of a prompt + judge + retrieval + tool + model-vendor chain whose inputs the team does not fully control.
2. "Experiment work" — a prompt A/B, a retrieval-method bake-off, a fine-tune pilot, an eval-judge calibration — is a first-class workload that can colonise delivery capacity if it is not sized and bounded.
3. "Reproducibility" has a different shape — a bug may be stochastic, dependent on temperature settings, or a function of a model version the team cannot pin and that the vendor may rotate next week.

This chapter builds the AI-specific triage cadence: the regression-by-cause taxonomy, the triage meeting's shape, the experiment-work sizing contract, and the three failure shapes classical triage produces on this surface along with the specific fixes.

## Motivation — three triage failures the generic shape produces

Before the framework, three failures that read, on the surface, as "the team is not triaging well" when the deeper problem is that the triage taxonomy has no language for what this team is looking at.

- **The four-day investigation that was a vendor-version change.** A bug is filed on Monday: "the summariser is producing shorter summaries than last week." The classical triage routes it as a bug on the summariser component; the applied engineer who owns it is tagged. She investigates for four days. The prompt did not change. The retrieval context did not change. The judge's scoring did not materially change. On Thursday she notices that the vendor shipped a new default model version on Sunday evening — the team's "latest" pin resolved differently from Monday forward. The root cause is a model-vendor change. The team's triage taxonomy had no slot for "model-vendor regression"; the bug was mis-routed for three days.
- **The experiment-colonisation of delivery capacity.** A senior IC proposes an experiment: a two-week retrieval-method bake-off, three re-ranker variants, two embedding models, four evaluation sweeps. The experiment is approved informally in a corridor conversation. Two weeks become six; two engineers become three; the delivery roadmap slips by a quarter. At the quarter-end review, nobody can quite articulate what the experiment produced, because the acceptance criteria were never written down. The failure is not the experiment; the failure is the manager's triage cadence never sized the experiment against delivery capacity in writing.
- **The judge regression that was called a prompt regression.** A regression lands: the eval harness reports that quality on a particular feature dropped by three points. The team spends a week tuning prompts. Quality on the harness improves. Then a customer reports that things are worse than before. Investigation reveals: the judge was quietly updated (a new rubric landed in the registry); the "regression" was the judge marking more strictly, not the model behaving worse; the prompt tuning masked the real system behaviour by over-fitting to the new judge. The root cause was a judge regression, not a prompt regression. The team's triage taxonomy did not distinguish; the fix was a quality regression on top of a measurement regression. Both have to be unwound.

All three failures look like "we should triage harder." The deeper problem is: *triage against a classical taxonomy, on an AI engineering team, mis-routes, under-sizes, and masks.* The AI-specific taxonomy is the fix.

## Core concept 1 — the regression-by-cause taxonomy

Every non-trivial production regression on an AI engineering system has one (or more) of five causes. Classical triage does not distinguish between them; AI-specific triage does. The taxonomy is the first thing the manager publishes and the first thing a triaged bug is tagged with.

### 1. Prompt regression

The team changed a prompt. The new prompt behaves worse on some input distribution than the old one, either in ways the judge catches or (worse) in ways the judge does not catch. The fix lives in the prompt layer — rewrite, rollback, or re-register with an updated judge.

**Telling signals.** The regression started after a prompt change in the registry (mod-104). Reproduces reliably against a specific prompt version. Isolated by swapping the prompt back.

**Common anti-pattern.** Treating a prompt regression as a model-vendor regression ("the model must have changed") and escalating to the provider when the problem is in the team's own registry.

### 2. Judge regression

The judge changed. The system behaviour is the same, but the measurement reads differently — more strict, less strict, mis-weighted, or newly brittle on inputs the old judge handled. The fix lives in the measurement layer — rewrite the judge, re-calibrate against a held-out set, re-score the historical corpus against both versions to isolate the drift.

**Telling signals.** The eval harness changed its verdict without a prompt, retrieval, or model change; the trace output looks identical to a human reader; the "regression" does not reproduce when the old judge is used.

**Common anti-pattern.** Treating a judge regression as a prompt regression and tuning the prompt against the new judge — over-fitting to a measurement that may itself be wrong.

### 3. Retrieval regression

The retrieval surface changed. Chunking boundaries moved, embedding model version shifted, re-ranker weights changed, the corpus itself grew or was re-indexed. The fix lives in the retrieval layer.

**Telling signals.** The passage grounding the generation has different content; the top-k retrieved differs from the previous version; the retrieval-quality metric (hit@k, MRR, or the application-specific measure) moved.

**Common anti-pattern.** Treating a retrieval regression as a prompt regression and rewriting the prompt to "handle the new context" when the underlying issue is that the retrieval surface delivered the wrong context.

### 4. Tool regression

A tool the agent or the application called behaves differently — an upstream API's shape changed, a function's semantics moved, a tool contract's parameter default was updated, a downstream service's rate-limit behaviour changed. The fix lives in the tool-contract layer or in the upstream service.

**Telling signals.** The trace shows a tool-call with new parameters, a different response shape, a new error code, a slower response that cascades into retries.

**Common anti-pattern.** Treating a tool regression as a model or prompt regression ("the model started calling the wrong function"), when in fact the tool's contract moved under the model's behaviour.

### 5. Model-vendor regression

The model-provider changed something. A new snapshot was promoted to "latest," a version was deprecated, a default parameter changed, a safety filter was updated, pricing or rate-limiting shifted. The fix lives in the vendor layer (migrate, pin, or escalate to the provider).

**Telling signals.** The regression starts on a date that correlates with the provider's release notes; the prompt, judge, retrieval, and tool layers are unchanged; pinning the model to a specific version reproduces the old behaviour.

**Common anti-pattern.** Treating a model-vendor regression as a prompt regression (four days tuning a prompt against a model that is rotating under you) or as "nothing we can do" (missing the pinning-and-migration decision the vendor-provider release review ritual is designed to produce; chapter 03).

A sixth case occurs: compound regression (the vendor shipped a new version, the judge was simultaneously updated, the regression is the interaction). The taxonomy handles it by allowing multiple tags on the same bug; the triage meeting isolates one cause at a time by rolling back each layer in turn.

## Core concept 2 — tagging and routing against the taxonomy

The taxonomy is only useful if every open bug and regression carries a tag. The routing follows from the tag.

| Cause tag | First-line owner (vertical pod) | First-line owner (horizontal eval capability team) |
|---|---|---|
| Prompt regression | The applied / agentic engineer who owns the surface | Escalated back to the owning vertical pod with trace evidence |
| Judge regression | The eval engineer | The eval engineer on the capability team |
| Retrieval regression | The RAG engineer (or the applied engineer if no dedicated RAG) | Escalated to the retrieval-capability team if one exists |
| Tool regression | The owner of the tool contract (often applied or agentic); the downstream-service team if the shape moved on their side | Same |
| Model-vendor regression | The manager + the senior IC on the vendor-portfolio watch; the vendor-release review is the resolution forum | The capability team's manager, if the capability team owns the vendor abstraction |

Three tagging-discipline rules.

- **No open bug may be in triage without a cause tag.** "Untagged" is a status that forces the triage meeting to classify.
- **Tags can be revised during investigation.** The initial tag is the *hypothesis*; if investigation isolates a different cause, the tag is updated and the owner is reassigned.
- **Compound tags are allowed but must be narrowed within one triage cycle.** A bug tagged "judge + model-vendor" cannot stay tagged that way for two meetings; by the second meeting the team has isolated which layer is causing the behaviour.

## Core concept 3 — the triage meeting's shape

The triage meeting is a 45–60 minute ritual that runs on the cadence the team's bug and regression rate demands — weekly for a hot product surface, biweekly for a stable one, as-needed for a very small team. It sits alongside the four AI-specific rituals from chapter 03, not in competition with them; the triage meeting is where bugs *become* fixture candidates for the prompt + eval + trace review and where incidents *become* candidates for the next replay.

### Who attends

The engineering manager (chairs or co-chairs), the senior IC per shape in the mix (applied, agentic, eval, RAG as applicable), the on-call engineer for the week, and the PM for severity-and-user-impact context. A triage meeting of more than six to seven attendees is a signal the team is over-wide for its triage surface; split the meeting by subsystem.

### Agenda (45–60 minutes)

1. **Open from the queue — 5 minutes.** New bugs since the last meeting, with initial cause-tag proposals from the on-call engineer.
2. **Classify — 15 minutes.** Walk each untagged or disputed-tag bug; isolate the cause against the taxonomy; assign the first-line owner.
3. **Experiment review — 10 minutes.** Running experiments against the sizing contract (core concept 4); which experiments are converging, which are overrunning, which are at their sizing boundary and must either finish or be re-scoped.
4. **Backlog re-rank — 10 minutes.** The open bugs and feature work, re-ranked against the current week's capacity and the current severity reads. The re-rank uses the standard severity / priority axes the level-40 sibling teaches, specialised with the cause-tag read (a model-vendor regression often jumps the queue because the vendor's release window closes in a week).
5. **Experiment proposals — 5–10 minutes.** New experiment proposals, sized against the capacity line (below); approved or deferred in the meeting.
6. **Close with the output — 5 minutes.** The dated triage log: what was classified, what was re-prioritised, what was approved or deferred.

### The output

A dated note in the team handbook. The note lists the newly classified bugs with their cause tags, the running-experiments status, the backlog re-rank changes, and the approved or deferred experiments. The output is the artefact the on-call engineer who missed the meeting reads to catch up.

## Core concept 4 — the experiment-work sizing contract

Experiment work is a distinctive AI engineering workload: a prompt A/B against a held-out set, a retrieval-method bake-off, a fine-tune pilot, a judge-calibration pass. The work is research-flavoured — the outcome is not known in advance, the sizing is harder, and the deliverable is a decision or a measurement, not a shipped feature. Left unsized, experiments quietly colonise capacity; the second motivation failure is the common shape.

The sizing contract is a written, pre-registered agreement on five fields. Every proposed experiment has to fill them before it is approved in the triage meeting.

1. **The question the experiment answers.** One sentence. "Does switching the re-ranker from bge-reranker-v2 to the new Cohere Rerank 3 improve hit@5 on the held-out registered fixtures by at least two points?" Not "explore re-rankers."
2. **The decision the answer enables.** One sentence. "If yes, migrate to Cohere Rerank 3 in the production retrieval pipeline next sprint. If no, hold on bge-reranker-v2 and re-read in six months." An experiment whose answer does not change a decision is not worth running.
3. **The sizing — engineer-days and calendar-days.** A hard ceiling. "Two engineer-days across one calendar week." The team's experiment capacity is bounded at the team level (see below); the sizing fits within.
4. **The acceptance criteria.** The empirical bar that lets the decision be made. "At least ten registered fixtures, stratified across the three content types; a paired-comparison score; a two-sided significance read at p < 0.1 on the primary metric." Not "we will know it when we see it."
5. **The exit conditions.** The conditions that terminate the experiment early. "Exit early if the first forty fixtures show no detectable effect in either direction (early-stop)." "Exit at the ceiling regardless of result."

A team-level experiment capacity is bounded as a fraction of the sprint's engineer-days — commonly 10–20% on a product-shipping team, up to 30–40% on a specialist research-heavy capability team. The manager publishes the fraction; the triage meeting enforces it. When the capacity is full and a new experiment is proposed, the triage decision is either (a) defer the new experiment, (b) kill a running experiment that has not converged, or (c) formally re-size the capacity line with the staff-plus IC and the PM (which is a leadership act, written down, visible to the exec tier via mod-112).

Three experiment-sizing anti-patterns.

- **The experiment that refuses to fill the sizing contract.** "We don't know enough to size it yet; let us explore first." The response: explore is an experiment of sizing contract one engineer-day, with the acceptance criterion of filling out a real sizing contract for the real experiment. Everything is sized.
- **The experiment with no decision.** "We want to understand how the new model behaves on our workload." The response: good instinct, poor contract. What decision does the understanding enable? Rewrite until the decision is explicit; if no decision is at stake, defer the experiment.
- **The experiment with no exit condition.** The experiment runs. It does not converge. It runs for another two weeks. Nobody wants to be the person who says "we did not learn enough; we are shipping without this." The exit condition is the forcing function; without it, experiments run until they are overtaken by events.

## Core concept 5 — the three failure shapes classical triage produces, and the fixes

The AI-specific triage cadence exists because classical triage produces three predictable failures on this surface. Name each; apply the fix.

### Failure 1 — the mis-routed bug

Classical triage routes by component. A production summariser bug is routed to "the summariser component" and to the applied engineer who owns it. The root cause lives in the retrieval, judge, tool, or vendor layer and the bug sits with the wrong person for days.

**Fix.** The cause-tag is required at triage. The on-call engineer proposes the first tag; the meeting disputes and refines. No bug leaves triage without a tag. The first-line owner is assigned against the tag, not against the component surface.

### Failure 2 — the unsized experiment

Classical backlog grooming does not have a concept for experiment capacity. A proposed experiment either ends up in the sprint as if it were feature work (and overruns it), or it ends up in a research-slack that is never sized (and quietly consumes 40% of the week).

**Fix.** The sizing contract. Every experiment has to fill the five fields before approval. The team-level experiment capacity is published and defended in the triage meeting. A new experiment at full capacity forces a decision — defer, kill a running one, or re-size the line in writing.

### Failure 3 — the masked measurement regression

Classical triage treats "eval score went down" as a single signal. On an AI engineering team, the signal is ambiguous — the system may have regressed, the measurement may have regressed, or both. Classical triage routes the signal to the engineer who owns the system; they tune the system against the (possibly broken) measurement; the system drifts further from reality.

**Fix.** A rule in the triage cadence: an eval-harness regression is tagged by two layers simultaneously (prompt/retrieval/tool/vendor for the system side; judge for the measurement side). The triage meeting isolates which layer by rolling back each in turn — rolling back the judge first is a cheap way to isolate a judge regression. The prompt + eval + trace review ritual from chapter 03 is the companion investigation surface.

## Core concept 6 — the bug-to-fixture, incident-to-fixture, experiment-to-decision loops

Three compounding loops the triage cadence sustains. Each takes an output of the triage meeting and lands it in a durable artefact.

### Bug-to-fixture loop

Every reproducible bug (any cause tag) that reaches a root-cause reading produces a candidate fixture for the eval harness (mod-104). The fixture is added by the first-line owner during the fix; the registry grows with every bug. A team that is not landing fixtures from bugs is leaking measurement surface — next quarter's regression of the same shape will not be caught.

### Incident-to-fixture loop

Every incident that reaches postmortem (mod-107 chapter 05) produces either a new fixture, a new judge, or an explicitly documented decision not to add one. The incident-replay ritual (chapter 03 ritual 3) is the forum. The triage meeting references the incident backlog and keeps the replay queue current.

### Experiment-to-decision loop

Every approved experiment ends in a decision, by design — the sizing contract requires it. The decision is recorded in the triage log; the implication for the backlog (migrate, hold, re-scope) is re-ranked into the next sprint. An experiment that ends without a decision has broken the loop; the contract failed and the root cause is in the triage discipline, not in the experiment.

## What good looks like

- Every open bug carries a cause tag from the five-layer taxonomy; untagged bugs are a visible status that forces classification at triage.
- The triage meeting runs on a predictable cadence, has a published agenda, and produces a dated log that an on-call engineer can read to catch up.
- A team-level experiment capacity is published as a fraction of sprint engineer-days; the triage meeting defends it; new experiments either fit, displace, or force an explicit re-sizing decision.
- Every approved experiment has a written sizing contract filling the five fields; experiments that reach their exit conditions terminate; experiments that cannot fill the contract are reshaped until they can.
- The bug-to-fixture, incident-to-fixture, and experiment-to-decision loops are closing: the registry grows after bugs, the incident-replay backlog is near empty, and experiment decisions are recorded.
- The eval harness regressions are tagged with both a system-side layer and the judge layer; isolation runs before any tuning starts.

## What bad looks like

- Bugs are tagged by component only; the cause taxonomy is in individual engineers' heads; the mis-routed-bug failure (motivation 1) recurs.
- Experiment work is approved by corridor conversation; no sizing contract exists; experiments quietly consume more than the team realises; the colonisation failure (motivation 2) recurs.
- Eval regressions are investigated against a single hypothesis (usually "the prompt is broken"); the judge-regression failure (motivation 3) recurs.
- The triage meeting is a status read-out of the bug board, not a decision forum; nobody re-prioritises, nobody re-classifies, nobody re-sizes experiments.
- The registry has not added a fixture from a bug in six weeks; the incident-replay backlog has grown; experiment decisions are undocumented or absent.
- The team's experiment capacity line has drifted: nobody can say what fraction of sprint time is experiment, the manager cannot defend the current posture under audit.

## Summary

- Classical triage is prerequisite; this chapter specialises it to the AI engineering surface.
- The regression-by-cause taxonomy classifies every open bug into one (or more) of five layers: prompt, judge, retrieval, tool, model-vendor. The tag drives routing and prevents the three classical failures (mis-routed bug, unsized experiment, masked measurement regression).
- The triage meeting runs a published agenda: open the queue, classify, review experiments, re-rank the backlog, approve or defer new experiments, close with the dated log.
- Experiment work is sized against a five-field written contract — question, decision, sizing, acceptance criteria, exit conditions — and enforced against a team-level capacity line.
- The bug-to-fixture, incident-to-fixture, and experiment-to-decision loops are the compounding mechanisms the cadence sustains.

The next chapter opens the fourth manager-owned artefact: the commissioned IC AI-assist stack (R14), the honest productivity-measurement discipline, and the data-flow policy the team actually needs.
