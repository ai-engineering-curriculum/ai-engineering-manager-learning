# The AI Failure-Mode Catalogue

A runbook library is only as good as the coverage of failure modes it names. A team whose runbook has pages for "service down" and "database slow" but not for "retrieval quality collapsed after we re-indexed yesterday" will run the yesterday-reindex incident off the cuff — and most of the AI-specific incidents a team will see are shapes a classic SRE runbook library does not enumerate. The manager's job is to require that the library *covers* the surface, and to update coverage as the surface evolves.

This chapter is the catalogue. Ten AI-specific failure modes recur across production LLM products. Each gets the same five-field treatment: a signature description that lets the on-call recognise it, the detection signal(s) that should page, the mitigation lever(s) the Incident Commander draws from, the evidence to preserve before the mitigation destroys it, and a regulator-adjacent note where relevant. The list is not exhaustive — your product may have failure modes specific to its surface (clinical-adjacent, children-facing, high-trust B2B) — but it is a floor, not a ceiling. If your runbook library does not cover these ten, cover them before next quarter.

The catalogue reads out to chapter 03 (runbook anatomy and the four mitigation levers), chapter 04 (evidence preservation and incident command), and the exercise in this module where you write the runbook for one product.

## How to read each entry

- **Signature** — the shape the on-call will see first: which signal moves, in which direction, over what timescale. Written so a less-experienced on-call engineer can read it and say "that's this one."
- **Detection** — the monitor(s) that should have paged, and the typical gap between the first move of the signal and the alert threshold. Monitor *craft* defers to Platform / SRE; monitor *existence* is the manager's requirement.
- **Mitigation** — which of the four levers (kill-switch, route-to-safer-model, rollback, HITL-tightening — chapter 03) applies, and any failure-mode-specific wrinkles.
- **Evidence** — what to preserve before mitigation makes it unrecoverable. The AI-shaped failure modes are high-evidence: traces, prompts, retrieved context, tool-call payloads, classifier scores, and model-version metadata are all needed for root-cause analysis and for any Article 73-adjacent inquiry (chapter 07).
- **Regulator / harm note** — when this failure mode, under plausible circumstances, crosses a harm threshold (serious incident under EU AI Act Article 73, breach obligation under GDPR, product-specific regulatory shape — HIPAA, PCI, EEOC). Not a legal opinion; a trigger to involve counsel early.

## 1. Jailbreak in production

**Signature.** The model, on a specific input pattern, returns content it was configured to refuse: disallowed content, operational instructions from an adversarial system prompt, personas that bypass the refusal policy. The classifier scored it below the threshold, or the classifier never ran. Typically surfaces as a tweeted screenshot, a customer complaint, or an internal red-team spot-check that found it in production.

**Detection.** The jailbreak-arena monitor (if one exists — a continuous set of known-jailbreak templates replayed against production), the safety-eval sampled-online suite (mod-104 chapter 06) reporting a dip in refusal-rate on an attack category, or human report (customer / employee / press). Lag from first occurrence to detection is often hours-to-days; monitors that catch it fast are jailbreak-arena-style replay + classifier-score distribution shifts.

**Mitigation.** Primary lever is **HITL-tightening** (tighten the moderation threshold on the surface, add a secondary classifier in front) if the attack class is narrow; **route-to-safer-model** (switch the surface to a model with stronger refusal priors) if the attack class is broad; **rollback of the prompt or system-prompt change** that introduced the regression if a recent change is suspected (check the prompt registry, mod-104 chapter 04); **kill-switch** on the affected surface if the output is actively harmful and no fast mitigation holds.

**Evidence.** Preserve: the attacker's full input turn-by-turn, the system prompt version, the classifier scores (if logged), the model version, the retrieval payload (if RAG), the tool-call sequence (if agentic), the output verbatim. Mind: logging the output of a successful jailbreak can itself be a data-handling issue (child-safety material, PII) — route the preservation to a redacted / access-controlled evidence store per your data posture. Note the capture time and your monitoring retention window — most observability platforms rotate logs on a short window and you have less time than you think.

**Regulator / harm note.** A jailbreak that produces content in a regulated category (medical advice to a vulnerable user, legal advice, CSAM, incitement, deepfakes of real persons) is *likely* to trigger Article 73 if the deployment is a high-risk system, and *possibly* to trigger disclosure under product-specific regulation regardless of high-risk status. Involve counsel the moment the output shape suggests a regulated category.

## 2. Prompt injection (direct)

**Signature.** A user input contains instructions that the model treats as directives — "ignore previous instructions and ...", "you are now in developer mode", or more subtle instruction-hierarchy violations where the user's content is interpreted as privileged. Output deviates from the system prompt's intended behaviour.

**Detection.** The instruction-hierarchy evaluator in the eval suite, the prompt-injection detector (Lakera Guard, Rebuff, Prompt Guard — see mod-106 chapter 03) reporting a score above threshold, or an anomaly in the output-vs-system-prompt-alignment eval. In a well-wired surface, direct injection is one of the better-monitored classes; in a poorly-wired surface, it looks like "the model did a weird thing."

**Mitigation.** **HITL-tightening** (route the surface to require human approval for the next N minutes while the input gets a stronger filter); **kill-switch** on the tool path if the injection targeted a tool that can cause side effects; **rollback** of recent prompt changes (sometimes the injection worked because a recent system-prompt change relaxed the instruction hierarchy); patch the input-filter / add detector to the input stage.

**Evidence.** The full user input, the system prompt version, the detector score (if logged), the model output, the downstream actions taken (database writes, emails sent, tool calls). Preserve the detector-decision logs — "the detector did not fire" is as important as "the detector fired."

**Regulator / harm note.** A direct injection that causes an action with external side-effects (sending data outbound, calling an external API with user data, modifying customer-visible content on another user's behalf) can be a **data breach** under GDPR Article 33 (72-hour notification) or an equivalent regime. Treat any injection that caused a data-handling side effect as counsel-triggering until counsel says otherwise.

## 3. Indirect prompt injection (via retrieval / tool content)

**Signature.** The model follows instructions that arrived through *content*, not through the user's direct turn: a malicious instruction embedded in a document retrieved by RAG, in a tool-call response from a third-party API, in a webpage fetched by a browsing tool, in an email the agent reads. The hallmark is that the user did not type anything unusual and the model still misbehaves. See Greshake et al. (arXiv:2302.12173) for the canonical taxonomy.

**Detection.** Retrieved-content moderation (chapter 02 of mod-106 extends the input-filter stage to retrieved content for exactly this reason); tool-output moderation; structural-separation markers in the prompt template that the eval detects when they are violated. Lagging indicator: the user says "I asked a simple question and the agent did X with my data."

**Mitigation.** **Rollback** of the retrieval index to a prior clean snapshot if the vector was ingested recently and the content is identifiable; **kill-switch** on the retrieval source until the content is cleaned; **HITL-tightening** by reducing the agent's tool scopes until the patch is in; **route-to-safer-model** generally does not help here because the problem is upstream of the model. Engage AI Risk to characterise the attack-class scope.

**Evidence.** The retrieved documents verbatim at the time of the incident, the retrieval index version, the embedding model version, the document-source metadata (where did this document come from, when was it ingested, by whom), the full tool-call chain, the agent's final action. The retrieved-content traces are often the hardest evidence to preserve because retrieval stores rotate content; freeze a snapshot immediately.

**Regulator / harm note.** Indirect injection is the single class of AI-specific attacks most often raised in regulator conversations, because it is a vector by which data from one tenant can affect another tenant's experience. Treat it as counsel-triggering by default on any multi-tenant surface.

## 4. Hallucination with harm

**Signature.** The model confidently returns fabricated information — a non-existent legal citation, an invented medication dosage, a fake product feature, a mis-attributed quote — and the user acts on it before the error is caught. "With harm" distinguishes this from benign-hallucination (the model invented a plausible but wrong restaurant recommendation): the hallucination matters because a decision rode on it. See OWASP Top 10 for LLM Applications LLM09 ("Misinformation") for the frame.

**Detection.** Faithfulness / groundedness evals (RAG-specific: Ragas faithfulness, context-recall) showing a drop; citation-coverage check failures; customer complaints with "the AI told me ..." phrasing; user-report tickets referencing fabricated specifics; the sampled-online judge pipeline (mod-104 chapter 06) reporting an unfaithful-answer spike.

**Mitigation.** **HITL-tightening** (route the surface's high-stakes outputs through a reviewer until the faithfulness fix lands); **rollback** of a recent prompt change that reduced grounding; **route-to-safer-model** if the hallucination rate on the current model is categorically worse than the fallback's; patch the prompt to require citations, lower temperature, add a grounding step. Where the surface is clinical / legal / financial-adjacent, consider a moratorium on open-ended answering until the fix is signed off.

**Evidence.** The full prompt, the retrieved context (if RAG), the model's output verbatim, the user's follow-up action if any, the faithfulness-eval score for this trace, the prompt and model versions. Where the hallucination caused user-side harm (bad medical decision, lost money, lost opportunity), preserve the reporting-ticket chain and the timestamps.

**Regulator / harm note.** Hallucination that caused user-side harm in a regulated domain is the EU AI Act Article 15 (accuracy) incident shape most likely to converge on Article 73. If the deployment is high-risk under Annex III and the output caused harm to a user's health, finances, or rights, counsel in on day zero.

## 5. Regression after a model-vendor upgrade

**Signature.** The provider pushed a minor version bump (silent or announced), or the team migrated to a new snapshot of the same family (mod-105 chapter 07), and quality metrics have regressed on specific input shapes. The headline eval score is roughly flat; the regression is on a slice (one language, one topic, one agentic tool-path, one persona). The product-side experience: "customers are complaining about X; X was fine last week."

**Detection.** The pre-merge / canary eval cadence (mod-104 chapter 06) catching a slice regression before full rollout — this is the *good* case. The *bad* case is a silent provider bump the team did not catch because the online-eval sampling did not stratify by the affected slice. Signals: slice-stratified online eval, judge-score drift on a slice, customer-ticket pattern matching a slice.

**Mitigation.** **Route-to-safer-model** (switch to the fallback from the mod-105 portfolio); **rollback** of the migration (pin back to the prior snapshot, where the provider supports pinning); patch prompts on the affected slice if a prompt-side fix exists. Communicate to the provider — most providers want to know about regressions on documented behaviours, and the escalation path is usually the enterprise support channel.

**Evidence.** The model snapshot versions (pre- and post-migration), the eval report for the affected slice (before and after), the customer-ticket set with timestamps, the trace of a representative regression case (prompt, context, output, judge score), the provider's release notes for the version bump.

**Regulator / harm note.** Rarely regulator-adjacent on its own; becomes so if the regressed slice is a protected category (bias regression on a demographic slice → EEOC-adjacent if employment, FHA-adjacent if housing, ECOA-adjacent if credit) or if the regressed slice is high-stakes (clinical, legal) and the product is Annex III. Counsel involvement on category shape, not on every regression.

## 6. Judge drift

**Signature.** The LLM-judge used in the eval pipeline (mod-104 chapter 04 — judge registry) starts returning different verdicts on the *same* fixtures it previously scored consistently. The release-gate eval looks worse or better than production warrants. The hallmark: the eval is moving when the system has not moved, or the eval is *not* moving when the system clearly has.

**Detection.** The judge-stability monitor (the control-set replay from mod-104 chapter 04 — a frozen set of fixtures replayed through the judge on a schedule, with the expected verdict distribution monitored for drift); the pre-merge-vs-production eval-delta alarm (when the gap between pre-merge eval and production online-eval widens beyond threshold); the sampled-online human-agreement rate dropping on judge verdicts.

**Mitigation.** **Rollback** of the judge prompt / judge-model version to a known-good snapshot from the judge registry; freeze the judge until a re-calibration against the control set succeeds; downgrade the eval's gate authority until the judge is re-calibrated (temporarily rely on human review for the affected fixtures). Judge drift is the failure mode most likely to be mis-classified as a model-quality incident — correct diagnosis is what prevents the wrong fix.

**Evidence.** The judge prompt version, the judge-model version, the control-set verdict distribution at the time of drift, the pre-merge-vs-production delta over time, any recent change to the judge rubric (the judge registry entry).

**Regulator / harm note.** Judge drift is internal; rarely user-visible directly. Indirect regulator relevance: the judge is the mechanism through which the team's eval evidence is produced, and the evidence is what Article 15 (accuracy) and (if invoked) Article 73 (serious incident) filings rely on. A silently drifted judge means the Article 15 evidence is unreliable. Treat judge-drift resolution as a pre-condition of any evidence-based regulator filing.

## 7. Tool-use failure (agentic surface)

**Signature.** The agent invokes a tool with mis-shaped arguments, hallucinates a tool that does not exist, calls a tool outside its scope, chains tools in a way nobody intended, or loops a tool call to cost / blast-radius failure. Output includes exception traces, data written to the wrong place, external API-rate alarms, or cost spikes.

**Detection.** Tool-call monitors — invalid-schema error rate, tool-not-found rate, tool-call-chain-length distribution shift, tool-call-cost-per-session distribution shift, downstream-system error-rate correlation. The cost-blast-radius signal is the one that pages fastest in practice (mod-110 — FinOps guardrails). Error-shape telemetry on each tool is a Platform concern; the AI engineering manager requires that it exists per tool.

**Mitigation.** **Kill-switch** on the affected tool (yank it from the catalogue; mod-106 chapter 07); **rollback** of the recent tool-schema or prompt change that introduced the misuse; **HITL-tightening** (require human approval for the tool until the fix lands); **route-to-safer-model** if the misuse rate is model-specific and the fallback is better on this capability. For cost-blast-radius, the circuit-breaker (per-session, per-tenant, global) is the fastest mitigation.

**Evidence.** The tool-call sequence verbatim (arguments, responses, exceptions), the agent reasoning traces (where logged), the model version, the tool schemas as of the incident, the downstream-system side effects (what records were written, what were sent, what external calls were charged). For agentic surfaces with authorisation scopes (chapter 07 of mod-106), preserve the identity and scope tokens used for each call.

**Regulator / harm note.** Tool-use failures are the single most frequent path to a **data-handling incident**: sending a user's data to an external API without consent, writing a record visible to another tenant, emailing data to an outside address, posting content to a public channel. Any tool-use failure with a data-side effect is counsel-triggering.

## 8. Embedding-store corruption

**Signature.** The vector store or retrieval index has been corrupted — a bad batch ingest wrote mis-aligned vectors, a schema change invalidated existing embeddings, an embedding-model migration ran partial and left two generations mixed, a ransomware-adjacent attack modified entries. Retrieval quality collapses; the model answers with wrong context or no context.

**Detection.** Retrieval-quality evals (Ragas context-recall, context-relevance) showing a collapse; the retrieval-source-attribution distribution shifting; a correlated spike in "the AI doesn't know things it used to know" tickets; integrity-check jobs on the store (if they exist) failing; embedding-dimension or embedding-model-version mismatch errors. The integrity-check job is a thing you want to have before you need it.

**Mitigation.** **Rollback** to a prior snapshot of the index (where backups exist — require them); **kill-switch** on the retrieval surface until the store is clean; **route-to-safer-fallback** (return "I don't have the information needed to answer this; a human can help" — a known-good degraded mode). A poisoned-ingest investigation is adjacent-to-security (next failure mode).

**Evidence.** The index snapshot immediately before the corruption window (and after, for comparison), the ingest-job logs for the suspect window, the integrity-check output, the embedding-model version and the index schema version, the authentication logs for who / what wrote to the store during the window.

**Regulator / harm note.** Embedding-store corruption is a candidate for a **security incident** if the cause is adversarial (poisoning attack, credential compromise, insider exfiltration-write). Route to security operations / DFIR early; preserve chain-of-custody on the store artefacts. GDPR Article 32 (security of processing) obligations apply; if personal data was compromised, Article 33 notification obligations may trigger.

## 9. Retrieval degradation (slow or empty)

**Signature.** The retrieval system is up, but retrieval latency has spiked, retrieval top-K is returning fewer or no results, or retrieval is returning stale content. The model then answers either slowly, from no context, or from obsolete context. Distinguishes from corruption (previous entry) by the integrity signal: retrieval degradation is operational; corruption is a data-integrity failure.

**Detection.** Retrieval latency p50 / p95 / p99 alarms, retrieval-coverage (top-K hit rate against known-good queries) monitor, context-coverage eval regression, the embedding-service health check, the vector-database queue-depth alarm. Depending on the underlying tech (pgvector on Postgres, a hosted vector DB, an in-memory index), the operational signals differ; the manager requires that *some* signal per tier exists.

**Mitigation.** **Route-to-safer-fallback** (serve the model without retrieval, or with a cached retrieval result, and label the output as "may be out of date"); **kill-switch** on the retrieval surface if the degraded responses would mislead; **rollback** of recent infrastructure changes to the retrieval stack; scale / recover the retrieval tier per the SRE substrate. Communicate internally — the team's product experience degrades gracefully only if the surface is labelled.

**Evidence.** Retrieval-latency time series across the incident window, the retrieval-coverage metric over the same window, retrieval-tier infrastructure health signals, recent changes to the retrieval stack, the degraded outputs' traces for comparison with pre-incident outputs.

**Regulator / harm note.** Generally not regulator-adjacent on its own; the engineering-side obligation is the Article 15 (robustness) story — the team's resilience plan is what prevents retrieval degradation from becoming a user-harm event. Document the fallback and its label; the label itself is often the Article 50 (transparency) deliverable (mod-106 chapter 08).

## 10. Moderation-classifier false-positive storm

**Signature.** The moderation classifier (input-side, output-side, or both) has started refusing legitimate inputs or outputs at a high rate. Users receive refusals for benign queries; completion rates collapse; the HITL queue (where false-positives route) saturates. Can be triggered by a provider-side classifier update, a threshold change, a new content class that was silently rolled out, or a correlated change in user-input distribution (a viral template that happens to look adversarial).

**Detection.** The refusal-rate monitor crossing threshold, the HITL-queue-depth alarm, user-complaint volume, the eval-suite false-positive-rate check against a legitimate-query holdout (mod-106 chapter 03 rule 1: run your legitimate-query eval against your classifier). Monitor refusal rate split by surface, language, and user segment; the storm is often slice-shaped.

**Mitigation.** **Route-to-safer-model** on the classifier (switch from primary to secondary classifier, preserve the policy); **HITL-widening** (lower the moderation threshold temporarily while triage happens) — note this is the opposite direction from HITL-tightening and must be an explicit decision because it accepts risk; **rollback** of the recent classifier-threshold change; **kill-switch** on the moderation stage only in extreme cases where the alternative is complete product non-function. Document the decision — accepting higher false-negative risk to reduce false-positive harm is a decision the manager signs (chapter 05 red flags).

**Evidence.** The refusal-rate time series, the classifier-score distribution per refusal, a sampled set of refused inputs for later review, the threshold or model version at incident time, the user segments most affected.

**Regulator / harm note.** False-positive storms are user-harm events when the refused content is a legitimate request (medical / safety / crisis-adjacent — refusing a user in a mental-health crisis because their message matched a self-harm pattern is a known harm shape, not a theoretical one). Document the harm; where the product is high-risk, Article 15 (accuracy — which includes refusal accuracy) and Article 14 (human oversight — the HITL path when refusals are wrong) are the relevant obligations. Counsel involvement scales with the product's harm surface.

## A note on provider outages

An eleventh failure mode that is not AI-specific but recurs enough to pin: **the model-provider is down**. The provider's status page shows a regional incident; completions return 5xx; latency has spiked. This is the SRE-classic "upstream dependency down" shape, and the mitigation is squarely in the mod-105 fallback-plan domain: route to the fallback provider / model, degrade the surface gracefully, communicate the degraded mode to users. The AI engineering manager runs this failure mode mostly off the mod-105 fallback plan and the SRE substrate — the runbook page is still required (chapter 03), but the craft of the mitigation is largely conventional. The failure mode worth owning *at AI altitude* is the one *after* the provider returns and the fallback / primary have different behaviours: the "silent model-swap" variant of regression (mode 5 above) and the judge-drift risk if evals were run against one provider while production ran on another.

## How the catalogue reads out

The catalogue is an index. For each failure mode, the manager requires:

- A **runbook page** (chapter 03) with: signature, detection, mitigation steps, evidence-preservation note, communication template, rollback target, who-to-page-for-help list.
- A **monitor** that detects the signature at an actionable threshold (chapter 03).
- A **dry-run** of the runbook page at least annually, or after any material change to the surface (chapter 03, chapter 04).
- An **incident-corpus mapping** (chapter 08) — a link from the failure mode to the entries in AIID, OECD.AI Incidents Monitor, or the MIT AI Risk Repository that document other organisations' incidents of this shape. Reading them before your own incident is cheap.

An exercise in chapter 03's exercise block is to write the runbook page for one failure mode on one product. Chapter 05's exercise is to design the postmortem template that the ten failure modes feed into. Chapter 06's exercise closes the loop: every incident on this list becomes a fixture.

## Summary

- Ten AI-specific failure modes recur across production LLM products: jailbreak, direct prompt injection, indirect prompt injection, hallucination with harm, model-vendor upgrade regression, judge drift, tool-use failure, embedding-store corruption, retrieval degradation (operational), moderation false-positive storm — plus the classic "provider outage" shape runbook.
- Per failure mode: a signature the on-call recognises, a detection signal the monitor fires on, a mitigation lever the Incident Commander draws from, and an evidence note that preserves what root-cause analysis and any regulator filing will need.
- Several of the failure modes (jailbreak with regulated output, injection with data side effect, hallucination with harm, embedding-store corruption from an adversarial cause, false-positive storm with user harm) are counsel-triggering by default; the manager recognises the shape early.
- The catalogue is an index into the runbook library (chapter 03); every entry has a page, a monitor, a dry-run cadence, and a cross-reference into the public incident corpora (chapter 08).

The next chapter picks up the runbook library and the AI-adapted on-call substrate the library runs on.
