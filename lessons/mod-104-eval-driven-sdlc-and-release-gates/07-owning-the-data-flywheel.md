# Owning the Data Flywheel (R13)

Every chapter so far has assumed the datasets exist — the golden set, the adversarial corpus, the judge calibration set, the production trace sample, the holdout. They exist because somebody collected them, somebody labelled them, somebody versioned them, and somebody pays for the labelling that keeps them growing. That somebody, at the program level, is the manager. The craft of designing a rater guideline or running a labelling platform defers to the `ai-eval-engineer` and the `ai-data-engineer`; the discipline of standing up and defending the **data flywheel** is the manager's.

This is the R13 chapter. The data flywheel is the pipeline that turns production experience (traces, user feedback, red-team exercises) into labelled data, which feeds both the evals that gate releases (chapters 02–06) and the fine-tunes the team may run (see `mod-105` and the `fine-tuning-engineer` IC track). Without the flywheel, evals calcify and fine-tunes are gated on data of unknown provenance.

## Motivation

The flywheel fails in characteristic ways on teams that do not treat it as a first-class program:

- **The volunteer labelling team.** Labelling is "done by whoever has a slow afternoon." Nobody calibrates with anybody else; two labellers disagree on whether `search_kb("refund policy")` should have returned the 2021 or 2024 chunk. Inter-rater reliability is unknown. Every month's data has a different implicit rubric.
- **The budget that doesn't exist.** The eval engineer asks for 400 labelling hours a quarter and is told to "find room in the existing team." The applied engineers spend four hours each labelling, one week a quarter. Quality drops; eval cases stop growing; the suite calcifies.
- **The red-team exercise that never produces data.** The safety peer runs a red-team exercise; finds fifteen jailbreaks; writes a memo. None of the fifteen jailbreaks end up as a versioned case in the adversarial corpus. Six months later, the same jailbreaks work again.
- **The flywheel with no feedback.** Production traces are captured. Labelling happens. The labels never flow back into (a) the eval suite, (b) the judge calibration set, or (c) the fine-tune pipeline. The labelling cost accrues; the system does not improve.

The manager-owned flywheel addresses all four. Named pipeline. Named budget. Named owner for rater calibration. Named consumers of the labelled data. Enforced linking of red-team findings to corpus entries.

## Core concept 1 — the shape of the flywheel

A flywheel is a feedback loop with four stations. Every station has an owner, a cadence, and a measurable throughput.

```
          +-----------+
          | Capture   |  ← traces, user feedback, red-team outputs
          +-----------+
                |
                v
          +-----------+
          | Label     |  ← human / AI-assisted labelling, rater calibration
          +-----------+
                |
                v
          +-----------+
          | Version   |  ← dataset registry (chapter 04), access control
          +-----------+
                |
                v
          +-----------+
          | Consume   |  ← eval suite, judge calibration, fine-tune
          +-----------+
                |
                '--------- influences what to capture next
```

The loop closes when labelled data changes what the team captures next quarter. Teams whose loop does not close have four separate pipelines, not a flywheel.

### Station 1 — Capture

Sources (adapt to the surface):

- **Production traces.** Every call emits a structured trace (chapter 04 — OpenTelemetry GenAI conventions). A sampling policy picks a fraction for labelling; stratify by audience, surface, and error signals. Unflagged traces are a background pool; flagged traces (user downvote, support escalation, safety-rule hit) are routed to a priority queue.
- **User feedback.** Thumbs, comments, ratings. In high-stakes products, the thumbs-down is gold — pair the signal with the trace and label it. Thumbs-up alone is noisy but useful in aggregate.
- **Support-ticket follow-ups.** When a customer says "the assistant got this wrong," the ticket + the original interaction is a labelling candidate. The CS team is a flywheel contributor whose participation needs to be designed in.
- **Red-team exercises.** `mod-106` goes deeper. The design rule here: every red-team exercise ends with *every finding converted into one or more versioned corpus cases*. If no case lands, the exercise did not produce flywheel output.
- **Synthetic generation.** A model generates adversarial or edge-case candidates; humans triage. Useful for scale; dangerous without human review (synthetic data can collapse diversity). See `Promptfoo` and `DeepEval` support for synthetic-data generation, and read the discipline carefully before adopting.

Capture owner: the applied engineer running the trace pipeline + the manager for the sampling policy.

### Station 2 — Label

Labelling turns a captured trace into a (input, expected, rubric-fit) tuple that an eval or a fine-tune can read. Three labelling configurations, in practice:

- **In-house engineering labelling.** Engineers on the team label a few hours a week. Highest fidelity when well-calibrated; limited throughput; expensive per-label. Appropriate for safety-sensitive, high-expertise, or low-volume surfaces.
- **In-house domain expert labelling.** Non-engineers in the business (CS reps, clinicians, paralegals, whoever has the domain) label. Higher fidelity than crowd on domain-specific tasks; higher coordination cost. Common for enterprise AI.
- **Third-party labelling.** Specialist labelling vendors (`Scale AI`, `Surge AI`, `Snorkel Labs` type) or crowd (`Prolific`, `MTurk`). Higher throughput, variable quality, DPA / PII care required.

For most teams, the right answer is a blend: in-house for the calibration set and the holdout (quality dominates), vendor or crowd for bulk production-trace labelling (throughput dominates), domain-expert for domain-specific tasks.

Labelling tooling to consider at the manager's altitude: `Argilla` (open-source, flexible), `Label Studio` (open-source, flexible), `Prodigy` (commercial, developer-friendly), `Scale Studio` / `Surge Platform` (vendor-run), observability-platform-native labelling (`LangSmith`, `Langfuse`, `Braintrust` all ship labelling UIs tied to the trace store).

Labelling owner: the `ai-eval-engineer` (IC) + the `ai-data-engineer` (IC) when one exists + the manager for the budget + the rater-calibration owner (next concept).

### Station 3 — Version

Captured + labelled data lands in the golden-dataset registry (chapter 04). The data is now a first-class artefact: identifier, version, provenance, PII posture, retention, owner, change log.

Discipline points:

- **No unversioned dataset gets consumed by a gate.** A gate reading an undated file is a gate reading a mirage.
- **Access controls.** Holdout data is privileged-access. PII-containing data is privileged-access. The dataset registry enforces the rules.
- **Retention.** Captured data is subject to the same retention policy as the originating trace. If the surface deletes traces after 90 days, the labelled slice derived from them cannot persist past 90 days in identifiable form. Anonymised derivatives can; the transformation goes in the changelog.
- **Legal / licensing.** External-corpus data that is folded into the golden set carries its license. Fine-tune usage is often more restrictive than eval-only usage. This belongs to the data-owner's sign-off in chapter 05's heavy-change list.

Versioning owner: the dataset owner (named in each registry entry).

### Station 4 — Consume

Three downstream consumers:

- **The eval suite.** New labelled cases become eval cases. The release-gate run picks them up. A labelling campaign that does not produce new eval cases this quarter is a labelling campaign that spent budget without closing the loop.
- **The judge calibration set.** Human-labelled (input, output, score) triples are the ground truth the LLM-as-judge is calibrated against. Refreshing this set is how judge–human agreement stays above the policy floor (chapter 04). Cadence: typically quarterly, or on any judge-model upgrade.
- **The fine-tune (when applicable).** Labelled data — supervised examples, preference pairs, red-team examples — feeds supervised fine-tuning, DPO / preference tuning, or safety-tuning. See the `fine-tuning-engineer` IC track for depth. The manager's job is to verify that fine-tune consumers see versioned, provenance-tracked data, not a pile of CSVs in a shared drive.

Consumption owner: eval engineer (eval and judge sides) + fine-tuning engineer when fine-tune is the consumer + applied engineer validating the loop closed (did the new cases catch the regression we wanted them to catch?).

## Core concept 2 — rater calibration as a first-class discipline

A dataset is as good as the agreement among its labellers. If two labellers of the same input produce different labels, the dataset is noisy — and evals built on noisy data cannot resolve small deltas.

The manager-owned discipline has four pieces.

### 1. A rater guideline

A written document — the eval engineer drafts it; the manager reviews and publishes it — that tells a new labeller what "pass" looks like on this task. For each rubric dimension (faithfulness, task-success, tone, refusal appropriateness, tool-choice correctness), a one-paragraph definition plus two or three examples of pass, borderline, and fail. Guidelines evolve; the latest version is the ground truth, and old-version labels get re-labelled or flagged.

### 2. Rater onboarding

Every new rater — engineer, domain expert, vendor crowd — runs through a calibration test before their labels feed the dataset. The test is a small labelled subset. If the rater's agreement with the gold-standard labels is below threshold, they do not label production data until they re-train.

### 3. Inter-rater reliability measurement

On an ongoing basis, a fraction of production labelling tasks are double-labelled. The inter-rater reliability metric — Cohen's κ (two raters), Fleiss' κ (more than two), Krippendorff's α (general) — is reported quarterly. A falling κ is a flywheel-health alarm: the guideline needs tightening, raters need re-calibration, or the task has gotten harder.

### 4. A named rater-calibration owner

One person on the team owns rater calibration. Usually the eval engineer; on larger teams, a dedicated data-quality owner. Not "the team." The manager publishes the name.

The hard truth about rater calibration: it is boring and it is expensive. Teams that skip it discover, usually around month six, that their evals are measuring rater variance instead of system behaviour, and no dashboard can distinguish the two.

Primary reference: [*Who Validates the Validators?* (Shankar et al., UIST 2024)](https://arxiv.org/abs/2404.12272) is one of the clearest readings on why judge-and-rater calibration is load-bearing.

## Core concept 3 — the budget

Labelling has a cost. The manager owns the line item.

Common-sense budgeting at the manager's altitude:

- **Capacity.** Hours per quarter the team (and vendors, and domain experts) can dedicate. Write it as a plan: "160 engineer-hours of in-house labelling / quarter + ~1500 vendor-hours / quarter." Rough first-approximation for a 5-engineer team; scale with product risk and surface count.
- **Dollars.** Vendor labelling is dollars-per-label; the per-label cost ranges widely by complexity (a yes/no rating is cents; a multi-turn trace review with rubric is many dollars). The manager budgets based on the *mix* of labelling the team needs: cheap bulk + expensive gold.
- **Review overhead.** Every label needs a fraction of reviewer time (the quality-control auditor who confirms a sample of labels). Build it in — 10–20% of labelling capacity is a reasonable starting number.

Where the budget comes from (adapt to your organisation's finance pattern):

- **Product budget.** The product owning the feature funds its own labelling. Clean ownership; risk: new features get labelling investment and old ones starve.
- **AI-platform budget.** A central line item funds labelling across the engineering org. Smoother for cross-cutting work (judge calibration sets that serve multiple features); needs governance to prevent it becoming a dumping ground.
- **Compliance / safety budget.** Red-team exercises and adversarial corpora often sit under a safety or compliance program. Less volatile; sometimes further from the product than ideal.

In practice a blend is normal. The manager's job is to defend the line in the quarterly plan (`mod-110`) with the artefacts this chapter produces — current flywheel throughput, inter-rater reliability trend, labelled-case growth, eval-gate pass rate attributable to flywheel cases.

## Core concept 4 — the red-team corpus specifically

Red-team exercises are the single most-productive flywheel source for safety data, *and* the single most-common place where flywheel discipline breaks down.

The manager's rule, stated bluntly: **a red-team exercise is not complete until every finding has become a versioned case in the adversarial corpus.** Not a slide, not a memo, not a Jira ticket. A case in `datasets/.../redteam-vN`, PII-sanitised, with provenance (the exercise date, the red-team participant, the OWASP LLM category, the model version tested), and with an eval that would fail if the finding recurred.

The pairing: `mod-106` chapter on red-team teaches the design and execution of the exercise. This chapter owns the data-pipeline obligation that converts the output into corpus growth. When the two chapters are both honoured, the suite grows the shape of the threats the team actually faces; when the pipeline obligation is skipped, the exercise becomes a demonstration, not a defence.

External sources to seed the corpus (do not let the team stop at these — they are starting points, not endpoints):

- **OWASP LLM Top 10 reference examples** — [owasp.org / top 10 for LLM](https://owasp.org/www-project-top-10-for-large-language-model-applications/).
- **MLCommons AILuminate** — a cross-industry safety benchmark; its hazard taxonomy is a useful starting point for category coverage.
- **Public jailbreak corpora** — academic datasets (search for current benchmarks; several exist and more land each quarter). Use as seeds, measure your own system's resistance, promote only the ones your audience's attackers plausibly try.

## Core concept 5 — the flywheel quarterly review

One meeting per quarter, 60–90 minutes, produces one memo. The manager owns the agenda.

Agenda:

1. **Throughput.** Cases captured, labelled, and landed in registries this quarter, broken down by source (production trace, user feedback, support follow-up, red-team, synthetic).
2. **Quality.** Inter-rater reliability trend. Judge–human agreement trend. Any rater who fell below calibration threshold and was retrained.
3. **Consumption.** Which eval cases that were newly added caught which regressions. Which judge calibration refresh happened. Fine-tune data releases, if any.
4. **Budget.** Spend vs. budget. Hours, dollars, vendor usage. Overrun or underspend explained.
5. **Red-team obligation.** Every red-team finding from the quarter verified as a versioned corpus entry. Open items named.
6. **Changes.** Guideline edits, new rubric dimensions, retention / PII posture changes. Rater-calibration owner's status.
7. **Next quarter.** Target case counts, new rubric areas, re-calibration schedule.

Memo output: a one-page summary, attached to the overall AI engineering quarterly review (`mod-109`). Leadership reads this as part of the team's operating report.

Teams that run this review religiously stay out of the "our evals calcified" failure. Teams that skip it discover the failure every eighteen months.

## A worked example — the flywheel plan, for one feature

Returning one last time to the doc-Q&A agent.

- **Capture.** Every production call emits a trace (OpenTelemetry fields + registry identifiers). Sampling: 1% of all traces for background pool; 100% of traces where the agent emitted a safety-rule refusal OR received a user thumbs-down OR had a tool call fail. Stratification by (tool-calling turn / no-tool turn, length bucket). Capture pipeline: `Langfuse` with export to the dataset pipeline.
- **Label.** Three labelling streams:
  - *In-house eval engineer (A. Patel)*: writes rubric guidelines; labels the holdout and the calibration set personally; audits 10% of vendor labels.
  - *In-house CS representatives (3 raters)*: 4 hours/week each labelling production traces (their domain), for behavioural and faithfulness rubrics.
  - *Vendor (TBD)*: 1000 trace reviews/quarter for bulk behavioural. (Procurement handoff tracked separately.)
- **Rater calibration.** Guideline: `guidelines/support-agent/rubric-v2.md`. Onboarding: a 30-trace calibration test; κ ≥ 0.65 required to label production. Inter-rater reliability: 5% double-labelling. Owner: A. Patel. Review quarterly.
- **Version.** Dataset registry as in chapter 04. Weekly bump cadence during active growth; quarterly bump cadence once suite matures. Change-log entries per bump.
- **Consume.** Labelled cases flow into `golden-vN` (behavioural), `calibration-vN` (judge calibration), `redteam-vN` (adversarial). Fine-tune not currently in scope; when it starts, provenance will gate.
- **Budget.** Quarterly: 160 hours CS-rep time (approved line), 40 hours eval-engineer time, ~$8k vendor budget. Line in the quarterly plan; signed off by the engineering director.
- **Red-team obligation.** Two red-team exercises/quarter (per `mod-106`). Every finding → one case in `redteam-vN` with provenance within 14 days of the exercise. Tracked by A. Patel.
- **Quarterly review.** Last Friday of the quarter, 90 minutes. Memo attached to the quarterly engineering review.

This plan fits on two pages. It is specific enough that the budget line survives finance review and the labelling pipeline survives the first "I don't have time for this" conversation. It is general enough to apply to the next feature with minor edits.

## What good looks like

- Every red-team finding from the last quarter resolves to a numbered case in the adversarial corpus. Every one.
- Inter-rater reliability on the production labelling stream is published quarterly; the number has a trend line and a target floor.
- The labelling budget is a defended line item, not an invisible tax on engineering capacity.
- New eval cases from the flywheel are the reason last quarter's regression was caught — and someone can name which cases, from which source.
- A new rater goes through an onboarding calibration test before they touch production data. Every time.
- The judge-calibration set is refreshed on cadence; judge–human agreement is checked and published.

## What bad looks like

- Labelling is "when the team has time." Nobody can say how many labels landed last quarter.
- Red-team findings are a memo nobody opens again.
- The eval engineer burns out personally doing all labelling themselves.
- Inter-rater reliability is unknown. Different raters label the same item differently. Suite numbers are noisy and nobody can say why.
- Fine-tune training data is in a shared drive with unknown provenance; legal cannot clear it; the fine-tune ships anyway and later gets quietly rolled back.
- The quarterly review is skipped "this quarter"; six months later the flywheel has decayed and nobody noticed.

## Summary

- The data flywheel is a four-station loop: Capture → Label → Version → Consume. The manager owns the program; the IC craft lives in `ai-eval-engineer` and `ai-data-engineer`.
- Capture is from production traces (stratified, sampled), user feedback, support follow-ups, red-team exercises, and (carefully) synthetic generation.
- Label with a blend of in-house, domain-expert, and vendor; rater calibration is a first-class discipline with a published guideline, onboarding tests, ongoing inter-rater-reliability measurement, and a named owner.
- Version everything through the golden-dataset registry (chapter 04); respect access control, PII, retention, licensing.
- Consume into eval cases, judge calibration sets, and (where applicable) fine-tune data.
- Budget is a named line item in the quarterly plan; the labelling cost does not happen for free.
- Every red-team finding becomes a versioned corpus case. No exceptions.
- Review quarterly; produce a one-page memo; leadership reads it.

With chapter 07, the module closes. You now hold the four manager-owned artefacts of the eval-driven SDLC: the release-gate policy (chapter 03), the three registries (chapter 04), the code-review discipline that ties PRs to those registries (chapter 05), the offline + online eval cadence (chapter 06), and the data flywheel that feeds all of them (this chapter). The exercises below ask you to produce each artefact for a real feature of your choice.

Carry forward to `mod-106` (safety and the release triad), `mod-107` (incident response), and `mod-110` (budget) — each one depends on the artefacts this module builds, and each one specialises them for a different peer-team conversation.
