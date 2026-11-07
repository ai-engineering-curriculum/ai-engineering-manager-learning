# The Three Registries — Prompts, Judges, Golden Datasets

A release gate is a decision rule over three inputs: the prompt (what the system said to the model), the judge (the yardstick the eval scored against), and the golden dataset (the cases the eval ran on). If any of those three drifts without versioning, the gate's historical numbers become unreadable, and so does the decision to ship. The manager's job is to require that each is treated as a first-class, versioned, reviewed artefact — not as "whatever is at `HEAD`."

This chapter is about the three registries, what they contain, how they are versioned, and what a PR must touch when it changes a prompt, a judge rubric, or a tool schema. The IC craft of writing a good prompt or a good judge rubric defers to the `ai-eval-engineer` and `applied-ai-engineer`; the discipline that keeps them traceable is the manager's.

## Motivation

Three failure modes repeat on teams that treat prompts / judges / datasets as "code is the registry":

- **The silent prompt change.** A string literal in `src/agent/prompt.py` changes from `"Answer in under three sentences"` to `"Answer concisely"`. Behavioural evals regress by 4%. The eval engineer opens the PR to bisect, finds the prompt change buried in a 400-line refactor whose title is "code cleanup." There is no record of *why* the change was made, who reviewed it, or what eval was expected to catch the regression. It takes three days to reconstruct.
- **The judge rubric that quietly rotated.** The LLM-as-judge prompt was rewritten six months ago to "improve clarity." Judge–human agreement was never re-measured. The team has been reading eval numbers against the new rubric as if they were comparable to numbers against the old. The quarterly review discovers two "wins" were actually judge-shift artefacts.
- **The dataset that drifted under the eval.** The golden dataset is a Google Doc. Somebody added fifteen cases last month because a customer complaint matched one of them. The suite's pass rate moved by 3 points — either because the new cases are hard, or the system genuinely regressed. The eval engineer has no way to separate the two signals because the dataset is not versioned.

A versioned registry per artefact — prompt, judge, dataset — resolves all three. The gate numbers become *comparable across time*, which is the precondition for interpreting any eval delta.

## Core concept 1 — what each registry is, and what it contains

### The prompt registry

The source of truth for every prompt the production system runs. Every prompt that the model sees — system prompts, few-shot templates, tool-description strings, retrieval-formatted contexts, judge prompts — is a registry entry. A prompt is not a string literal; it is a versioned artefact.

Each registry entry carries:

- A **stable identifier.** A slug like `support-agent/system-v3` or `faithfulness-judge/rubric-v2`. Not a filename; filenames rename. The identifier is what the application code references and what traces emit on every call.
- A **version.** Semver-ish is fine. Most teams use `v1`, `v2`, `v3` because prompt changes are not "patches" vs. "minor" in a software sense; the discipline is "every change bumps the version, period."
- The **prompt text.** The exact string sent to the model. For templated prompts, the template plus the inputs' schema.
- **Authorship and date.** Who wrote the change, when it landed, which PR introduced it.
- **Intent.** One paragraph: what this prompt is for, what it is replacing, what behaviour change the author expected.
- **Eval results.** The eval-delta at the moment the prompt was promoted. This lives beside the entry, not in a separate doc, so a reviewer can see "prompt v3 shipped with +3 points on behavioural, -1 point on faithfulness (within CI), no change in safety."
- **The models it was validated against.** A prompt validated on `claude-sonnet-4-6` may misbehave on `gpt-5`. Record the matrix.
- **Deprecation / rotation plan.** If this prompt is paired to a model that is being deprecated, when does the prompt rotate? Who owns it?

Storage: a dedicated folder in the repo (e.g., `prompts/`), each prompt in its own file with YAML frontmatter for the metadata, is enough. Observability stores (`LangSmith`, `Langfuse`, `Braintrust`) also ship prompt-management features; some teams use them as the registry of record. Either is fine as long as there is one registry of record — not both.

### The judge registry

The source of truth for every judge (LLM-as-judge rubric, rubric-judge, keyword rule, scoring function) that a gate reads. A judge is an evaluator; its rubric is a thing that can drift; its outputs are only interpretable if the rubric is versioned.

Each judge-registry entry carries, in addition to the prompt-registry fields:

- The **judge type** — LLM-as-judge (with a reference to the prompt-registry entry for the judge prompt), rubric-based scorer, statistical rule, external service.
- The **calibration set.** The human-labelled set of (input, model output, human score) triples that the judge is calibrated on. Versioned like a dataset (see below).
- The **judge–human agreement number.** The specific metric — Cohen's kappa, Spearman correlation, agreement rate — and the specific value on the calibration set. The paper [*Who Validates the Validators?* (Shankar et al., UIST 2024)](https://arxiv.org/abs/2404.12272) is the canonical reference for why this matters; the minimum agreement acceptable for a *gating* judge is a policy call (see "Judge-stability thresholds" below).
- The **rotation date.** When was the judge last recalibrated against a refreshed calibration set? The data flywheel (chapter 07) feeds this.
- The **models the judge runs against.** If the judge itself is an LLM, which model is the *judge model*. The judge model's drift is independent of the model-under-test's drift.

The judge registry is the most-skipped of the three on teams new to eval-driven delivery. It is also the registry whose absence causes the most-expensive eval-reading mistakes, because a drifting judge makes every downstream number a mirage.

### The golden-dataset registry

The source of truth for every dataset the suite reads from. This includes the golden set, the production-trace sample, the adversarial corpus, and the holdout (which has a special discipline — see below).

Each dataset-registry entry carries:

- A **stable identifier and version.** `support-agent/golden-v4` or `support-agent/redteam-v7`.
- **Provenance.** Where each case came from. Hand-authored, sampled from production, sourced from a public corpus (name the corpus and the license), red-team exercise on a specific date.
- **PII / privacy posture.** Whether cases contain customer PII, what the retention policy is, whether cases can be shared externally (with an eval vendor, with an auditor).
- **Change log.** For each version bump, what was added, what was removed, and *why*. The provenance of *removed* cases matters too — a case removed "because it was flaky" is different from a case removed "because it duplicated case #35."
- **Owner.** A named human who merges changes to this dataset. Not a team.
- For the **holdout** specifically: an access-control declaration. Who may run against it? The gate may; iteration may not. See below.

A dataset in the registry is not a Google Doc. It is a file (JSONL, CSV, Hugging Face datasets, whatever the tool consumes) in the repo or in a trusted artefact store, with the metadata above in version control.

## Core concept 2 — what a PR must touch when it changes each artefact

This is the manager-owned rule-set that turns "we have registries" into "every change is traceable." Express it as a PR-template item and enforce it in the PR gate (chapter 03) via a registry-lint check.

### When a PR changes a prompt

- **The prompt-registry entry is bumped.** Version increments. Intent is written. Author is tagged.
- **An eval case is added or updated** that exercises the specific behaviour the prompt change is targeting. If the change is "answer more concisely," an eval case or rubric must capture conciseness, or the registry-lint flags the PR with a request for a justification.
- **The release-notes fragment includes the prompt change.** Chapter 05 goes deeper on release-note shape.
- **The PR description names the expected eval delta.** "I expect faithfulness up ~2 points, latency unchanged." The release-gate run will confirm or disconfirm.
- **Reviewers include the applied engineer on-call and the eval engineer**, by PR assignment rule. Chapter 05 on R12 goes deeper.

### When a PR changes a judge rubric

- **The judge-registry entry is bumped.** Version increments.
- **The judge is re-run against the calibration set** and the new judge–human agreement number is published in the entry. If agreement drops below the policy threshold, the PR cannot merge without a sign-off from the eval engineer.
- **All prior eval numbers that used the prior judge version are marked** in the release-gate history view as "judge v1 → v2 boundary." This is how you prevent the quiet drift failure above.
- **Release notes mention the judge change.** Judge changes are not invisible internal plumbing; they change how everything downstream reads.

### When a PR changes a tool schema (for tool-using agents)

- **The tool schema is versioned in its own registry entry** (functionally a sibling of the prompt registry). A tool is a contract; it versions.
- **Any prompt that references the tool is checked** — if the system prompt names `search_kb(query)` and you rename it to `search_knowledge_base(query, scope)`, the prompt registry entry references the tool schema and the PR must update both.
- **Tool-use correctness evals are re-run.** The release gate's tool-use suite must have cases exercising the new signature.
- **Agent authorisation posture is re-reviewed** (per `mod-106`). Tool changes often change the agent's effective capabilities, which is a release-triad concern, not a pure code concern.

### When a PR changes a golden dataset

- **The dataset-registry entry is bumped.** Case additions, removals, and edits are logged.
- **The release gate's reported history is annotated** so a reader can see "dataset bump from v3 to v4 here; the +2 point jump is partially a harder-case addition, not a system improvement." This discipline is what allows anyone to read a historical eval chart.
- **Removed cases are justified.** A case removal needs a reason. "Flaky" is a reason; so is "duplicated"; "the system kept failing this one" is not.
- **Holdout additions require a different review path.** Adding cases to a holdout is cheap; adding cases to the training-side of the golden set after an eval has used it is a form of leakage. See below.

## Core concept 3 — three disciplines that keep the registries honest

### The holdout discipline

The holdout is the slice of the golden dataset that iteration does not see. It is used **only** at the release gate, to measure true generalisation. The temptation — universal, every team — is to peek at the holdout mid-cycle to see "how are we doing," which immediately turns it into a validation set.

The manager's discipline:

- A single owner holds holdout access (usually the eval engineer).
- Running the suite against the holdout is a privileged action, logged, and tied to a release candidate.
- If the suite has been run against the holdout more than a handful of times for the same code state, you are iterating against the holdout and it has stopped being one.
- When the holdout is "used up" — the team has iterated enough to leak its signal — refresh it. The quarterly review is where this is decided.

This discipline is boring to enforce and expensive to skip. Skipped holdouts produce releases that score well on the gate and badly in production.

### Judge-stability thresholds

A judge is only as good as its agreement with human raters on the calibration set. The manager-owned policy pins the minimum agreement acceptable for a *gating* judge — typical ranges, from published work and practice, are Cohen's κ ≥ 0.6 (substantial) or Spearman ρ ≥ 0.7 for scoring judges, with higher bars (κ ≥ 0.8) for safety-gating judges. The exact number is your eval engineer's call, but the *policy that a number must exist* and *must be above threshold* is yours.

Equally important: **judge re-calibration cadence.** Judges drift with the model being used to run them and with the data distribution. The policy pins a cadence — quarterly is common — and a trigger — any judge whose agreement drops below threshold at calibration, or any judge whose underlying model gets upgraded, is re-reviewed before it re-enters the gate.

### The "don't change two things" rule

The temptation in a prompt-eval iteration loop is to change the prompt *and* the judge *and* add a dataset case in the same PR because "it's all one investigation." The result is unreadable eval deltas — you cannot tell whether the +3 points is from the prompt, the judge, or the added case.

The discipline: one artefact change per PR (where practical). If an exception is needed, the PR description explicitly lists each artefact change, the expected direction of its contribution to the delta, and the reviewer explicitly signs off on the exception. The release-note fragment carries the same list.

Teams new to this rule push back because it feels bureaucratic. Teams a quarter in find it has turned eval readings from an argument into a result. Keep it.

## Core concept 4 — storage and tooling

The manager does not need to pick the perfect storage. The manager does need to pick *one* storage, defend it, and prevent the team from running two overlapping registries.

### Storage options, quickly

- **In-repo files with YAML frontmatter.** Simple, versioned with git, works for all three registries, zero vendor lock-in. The default for small-to-medium teams. Enforce schema via a pre-commit hook or CI lint.
- **Observability-platform prompt management.** `LangSmith`, `Langfuse`, `Braintrust` ship prompt / dataset / experiment management features. These are strong when the team is already deep in the platform's traces and wants dataset-and-experiment UX on top of a common store. The risk is divergence between the platform's state and the repo's state — mitigate by exporting / importing in CI.
- **Hybrid.** Repo is the registry of record; the observability platform is a mirror and a UX. Common; works; adds a small sync burden.

Whichever you pick, the policy items that matter are:

1. **Single source of truth.** The application code and the eval suite both resolve against the same registry. Do not allow a prompt to live in code that the registry does not know about.
2. **Change goes through PRs.** Not through a UI that bypasses code review.
3. **Trace emits the registry identifier on every call.** When a trace lands in the observability store, you can tell which prompt-version, judge-version, and dataset-version produced it. This is non-negotiable; without it, chapter 06's drift taxonomy is uninferable.
4. **Deprecation discipline.** When a prompt or judge is retired, the registry entry does not get deleted. It is marked deprecated, with the pointer to its successor. Historical traces still resolve.

### Standards worth aligning to

- **OpenTelemetry GenAI semantic conventions** for the trace-side identifier fields. Pin `gen_ai.request.model`, the model identifier, and the prompt-registry identifier as a trace attribute.
- **Model Cards** ([Mitchell et al. 2019](https://arxiv.org/abs/1810.03993)) for the *model* side of versioning when the team fine-tunes (see `mod-103` chapter 04). Not a registry in this chapter's sense, but the discipline rhymes.
- **Datasheets for Datasets** ([Gebru et al.](https://arxiv.org/abs/1803.09010)) for the dataset-registry entry shape. A good dataset entry is a Datasheet-shaped document.

## A worked example — the three registries, for one feature

Returning to the doc-Q&A agent from chapters 02 and 03.

- **Prompt registry.** `prompts/support-agent/` folder.
  - `system-v3.md` — frontmatter: `id: support-agent/system`, `version: v3`, `author: j.park`, `date: 2026-09-14`, `intent: shorten responses, more aggressive citation`, `eval-delta: +2.1 behavioural (95% CI [1.4, 2.8]), -0.3 faithfulness (NS)`, `validated-against: [claude-sonnet-4-6, gpt-5]`, `supersedes: v2`.
  - `faithfulness-judge-rubric-v2.md` — same shape; judge-prompt text; judge–human κ = 0.74 on calibration-set-v3.
- **Judge registry.** `judges/support-agent/`.
  - `faithfulness-v2.yaml` — `type: llm-as-judge`, `prompt: faithfulness-judge-rubric-v2`, `judge-model: [specific model id]`, `calibration-set: calibration-v3`, `kappa: 0.74`, `last-recalibrated: 2026-09-01`, `policy-floor-kappa: 0.60`.
  - `tool-use-correctness-v1.yaml` — `type: rubric-based`, `scorer: inspect/match-first-tool`, `calibration-set: tool-use-calibration-v2`, `agreement: 0.91`.
- **Dataset registry.** `datasets/support-agent/`.
  - `golden-v4/` — 200 JSONL cases, `metadata.yaml` with provenance, PII posture (contains customer PII; retention 1 year; no external sharing), `changelog.md` with v3 → v4 delta (+10 hard-case additions from Q3 production failures, -2 duplicate cases, +1 edit for ambiguous expected output).
  - `redteam-v7/` — 100 adversarial cases, provenance (OWASP LLM01 reference pack v2024 + internal red-team 2026-08-21 + sanitised real malicious tickets from support-queue audit), no PII (synthetic names).
  - `holdout-v2/` — 50 cases, access-control: `owner: a.patel`, `runs-this-quarter: 1`, `last-refresh: 2026-07-01`.
  - `calibration-v3/` — 120 human-labelled (ticket, draft, score-1-to-5) triples used by the faithfulness judge.

Every eval run and every production trace emits, as attributes: the prompt identifier (`support-agent/system`), its version (`v3`), the judge identifiers in use, and the dataset identifier for eval runs. A reader of any historical result can resolve what produced it.

This looks like paperwork. It is the paperwork that lets a team read six months of eval history and say "quality is up, judge is stable, retrieval regressed" with confidence — instead of debating what the numbers mean every release.

## What good looks like

- An engineer can `grep` for a prompt identifier and find exactly one authoritative entry.
- A new engineer, told "the gate failed on faithfulness," can find the rubric, its last-calibrated date, its agreement number, and its history, in under two minutes.
- Every historical eval chart is annotated at the points where a prompt / judge / dataset version changed; the annotations explain why.
- Removing a case from a golden set requires a justification in the PR, not a verbal "yeah, we agreed to drop it."
- The holdout has an access log, and the log is short.

## What bad looks like

- Prompts live in code as `"""system prompt here..."""` string literals; the registry is "`HEAD`".
- The judge rubric is in a Google Doc with the last-edited stamp from eighteen months ago.
- Golden-set versioning is "we added some cases."
- The holdout has been "spot-checked" fifteen times this quarter.
- A regression is detected; nobody can tell whether the prompt, the judge, the dataset, or the model changed between the baseline and the regressed run.

## Summary

- Three registries — prompt, judge, golden-dataset — are versioned, owned, and referenced by every call and every eval run.
- A PR that changes a prompt / judge / tool-schema / dataset bumps the registry, writes intent, cites expected eval delta, re-runs calibration if applicable, and updates the release notes. Enforced by registry-lint in the PR gate.
- Three cross-cutting disciplines: hold the holdout sacred, pin a judge-stability floor plus a recalibration cadence, and change one artefact per PR where practical.
- Pick one storage. In-repo files are the simplest default; observability-platform prompt management works when carefully synced; never run two overlapping registries.
- Every trace and every eval run emits the registry identifiers so historical results remain readable.

The next chapter takes the registry discipline above and turns it into the code-review surface that R12 names: what reviewers check, who signs off, and what the release note content looks like for a prompt change.
