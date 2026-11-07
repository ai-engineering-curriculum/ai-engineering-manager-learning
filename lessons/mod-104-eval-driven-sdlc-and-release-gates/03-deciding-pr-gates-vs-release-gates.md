# Deciding PR Gates vs. Release Gates

An eval suite that runs *somewhere* is not a gate. A gate is a specific decision rule — "the merge is blocked unless the suite meets threshold X" — enforced by automation, with an auditable override path. This chapter is about the two tiers of gate every AI product needs (pre-merge PR gate and pre-release gate) and the design choices that keep both believable when the team is under pressure to ship.

The manager's job is **not** to pick between a PR gate and a release gate. It is to design both, and to decide which evals belong in which tier. A one-tier gate always fails one of two ways: either it is fast enough to run on every PR but too shallow to catch release-blocking regressions, or it is thorough enough to catch regressions but too slow to run on every PR, so engineers learn to skip it. Two tiers, with a clear policy on what lives in each, is the architecture that survives.

## Motivation

The failure modes recur predictably:

- **Everything on the PR.** The manager asks the eval engineer to "run all the evals on every PR." The suite takes 45 minutes. Engineers batch changes and start pushing on Friday night to skip the full suite. Flaky judges time out the CI job. Within a quarter the gate is turned off "while we figure out flakiness." It never comes back.
- **Nothing on the PR; everything at release.** The manager puts the full suite in a pre-release pipeline. PRs merge in a day; releases happen every two weeks. On release day, something regresses. The team has to bisect 40 PRs to find the regression. By the time they do, two of the PR authors are on vacation.
- **One-tier gate overridden to death.** The gate exists; thresholds are set tight; the first release on a visible timeline fails the gate. Leadership overrides. The second override is faster. By the fourth, the gate is procedural theatre.

A two-tier gate — fast + narrow on every PR, slow + comprehensive at release — resists all three. It gives engineers immediate feedback on obvious regressions, gives release engineers thorough feedback before production exposure, and makes the override surface small enough (release-tier only) that each override is visible and discussable.

## Core concept 1 — the two tiers

### Tier 1 — the PR gate

Runs on every PR. Finishes in **under five minutes** of CI wallclock, ideally under three. Blocks merge when it fails. Overrides require a review from a specific role (the eval-engineer, or the senior-on-shift) and are logged automatically.

What belongs in the PR gate:

- **Deterministic unit evals.** Prompt-template rendering tests, parser tests, tool-schema contract tests, cost-budget tests (dry-run token counts). These are not "AI evals" in the LLM sense but they live beside them, run in CI, and block merge the same way.
- **Small, fast, deterministic behavioural subset.** A small subset of the golden dataset (typically 20–100 cases out of hundreds or thousands) that a) runs against a cached model response when input hasn't changed, or b) runs against the model with temperature = 0 for reproducibility. The subset is picked to cover the critical surfaces of the feature; it is not random. See `promptfoo` and `DeepEval` for the CI patterns.
- **Guardrail-contract tests.** If the system has guardrails (input moderation, output moderation, policy-aware prompts), smoke tests that the guardrails are wired in and produce the expected refusals on canonical examples.
- **Trace-shape tests.** If the application emits traces, a few tests that assert the trace fields are present and typed correctly (OpenTelemetry GenAI semantic conventions — `gen_ai.request.model`, `gen_ai.usage.prompt_tokens`, `gen_ai.usage.completion_tokens`, etc.). These never regress; when they do it is always a serious observability bug.
- **Lint-style tests for the three registries (chapter 04).** Does this PR touch a prompt? If so, is the registry entry updated? Does this PR touch a judge rubric? Is the judge-calibration metadata current?

What **does not** belong in the PR gate:

- **LLM-as-judge evals.** Judges are slow, cost money, and introduce their own variance. Running a judge on every PR produces flaky CI. See tier 2.
- **Large-corpus retrieval evals.** Embedding + search + scoring against hundreds of queries is minutes of work; not PR-tier.
- **Red-team / adversarial suites.** You want these to run comprehensively, which is minutes-to-hours; not PR-tier.
- **Full-population A/B or shadow evals.** Online by construction; see chapter 06.

### Tier 2 — the release gate

Runs on every release candidate — not on every PR, not on every merge to main (unless main is the release). Finishes in **under an hour** of total wallclock, ideally under thirty minutes. Blocks the release if it fails. Overrides require an explicit written decision by the manager and, for safety-related failures, a release-triad sign-off (`mod-106`).

What belongs in the release gate:

- **The full behavioural suite.** All of the golden dataset.
- **The judge-based evals.** Faithfulness, relevance, tone, hallucination rate, instruction-following — whichever rubrics the suite includes. On a sampled dataset large enough to detect the regression size you care about (bootstrap-CI sized).
- **The full retrieval-quality suite (if RAG).** Recall / precision against the known-answerable set.
- **The full tool-use correctness suite (if agentic).** Multi-turn, with mocked tool failures, in `Inspect`-style harnesses.
- **The adversarial / red-team suite.** OWASP LLM Top 10 case families, prompt-injection corpus, policy-boundary probes, jailbreak attempts.
- **Cost-budget acceptance.** Modal / p95 cost against the cost ceiling from the brief.
- **Latency acceptance.** p50 / p95 / p99 against the SLO target.
- **The release-triad sign-off (if applicable).** Chapter 06 of `mod-106` is the depth.

What **does not** belong in the release gate:

- **Online evals.** A/B, canary, shadow — these are operational, not pre-release. They are release-adjacent but not pre-release. See chapter 06.
- **Continuous production monitoring.** Also chapter 06.

## Core concept 2 — the design choices that make each tier survive

### Speed is a first-class constraint on the PR tier

PRs block real humans. A PR gate that takes ten minutes causes engineers to task-switch, which halves their throughput. A PR gate that takes thirty minutes causes engineers to stop running local-side checks and treat CI as "the thing that catches it," which doubles the number of pushed-but-failing commits.

Target under 5 minutes, ideally under 3. Design the PR tier to fit that budget *first*; what fits in the budget is what the PR tier covers. If cases cannot fit, they go in the release tier.

Techniques that keep PR-tier evals fast:

- **Response caching.** If the input hasn't changed, re-use the cached model response (`promptfoo` and many eval harnesses support this natively). The PR-tier eval becomes a pure comparison against the cache.
- **Temperature-zero / seed-pinned sampling.** For non-cached runs, pin determinism knobs so the eval is reproducible. Some providers support seed; where they don't, cache instead.
- **Subset selection.** Pick the 20–100 cases that exercise the surfaces the feature actually ships on, not a random slice.
- **Parallelism.** Eval frameworks parallelise case execution; use this. CI runners are cheap compared to engineer time.

### The release tier owns comprehensiveness

The release tier is where the slow, expensive, judgment-heavy evals live, because the release tier runs once per release candidate — not once per PR. Its budget is "small enough that releases happen on time," not "small enough that engineers don't notice."

Design for:

- **Statistical power.** The release-tier dataset must be large enough, and its runs reproducible enough, to detect the regression size you care about with reasonable confidence. The `ai-eval-engineer` IC track has the depth; at the manager's altitude, insist that the eval reports a confidence interval on the delta, not just a point estimate.
- **Judge variance.** Judges are LLMs; they have run-to-run variance. Either run the judge multiple times and average (expensive but defensible), or publish the judge's variance on the calibration set and widen thresholds accordingly. Shreya Shankar et al.'s *Who Validates the Validators?* (UIST 2024) is the canonical reference for how brittle naive judge setups can be.
- **Cost realism.** The release-tier eval costs real money — the judge is an LLM call, the model-under-test is an LLM call, on hundreds or thousands of cases. Pin the dollar cost of a full release-tier run in the brief (chapter 02). Treat it as a line item in `mod-110-budget-finops-and-build-vs-buy`.

### Thresholds, in numbers

A gate is only a gate if the threshold is a specific number. The anatomy of a defensible threshold:

- **A metric with a definition.** "Faithfulness" means Ragas faithfulness v0.1 on the golden-retrieval set, averaged across 3 judge runs. Not "faithfulness" with no referent.
- **A number.** 0.85 is a number. "High" is not.
- **A confidence posture.** "0.85 lower bound on a 95% bootstrap CI" is defensible. "0.85 point estimate" is weaker because it treats noise as signal.
- **A regression allowance.** "No more than a 2-point decrease versus the previous release, on the same dataset version" — an explicit delta allowance separate from the floor.
- **An absolute floor.** Below this number the gate blocks regardless of delta ("we can't ship a release where jailbreak success rate exceeds 2%, even if quality is up").

Thresholds come in two flavours that a manager must distinguish:

- **Quality thresholds** — behavioural task-success, judge-based quality, retrieval metrics, tool-use correctness. Regression allowances are negotiable; absolute floors exist but the delta is usually what gates.
- **Safety and cost floors** — these do not have regression allowances. "Safety improved by 3% but cost tripled" is still a block if cost broke its floor. "Quality up 5% but jailbreak success doubled" is a block.

Teams that conflate the two flavours end up tolerating a safety regression because quality improved. Keep them explicit.

### Override policy — the single most-skipped design decision

Every gate will fail at the wrong time. The override policy says what happens when it does.

A well-designed override policy has four parts:

1. **Who can issue an override.** For the PR gate: the on-shift reviewer or the eval engineer. For the release gate: a named manager role (you) plus, for safety-category failures, a release-triad sign-off from the `agentic-safety-engineer` / `ai-risk-engineer` peer (per `mod-106`). Do not allow single-person overrides on safety.
2. **What the override costs.** An override is not free. Logged by name, with a one-paragraph justification. Attached to the release notes or PR. Reviewed in the quarterly gate review (below).
3. **What makes a failure non-overridable.** Safety absolute floors are non-overridable. "Jailbreak success rate > 2%" is a block regardless of who asks — the correct response is to fix the system, not to override. The policy enumerates these cases.
4. **The remediation expectation.** Every override comes with a "when is this fixed by" — either an inline fix in the next 24 hours, a tracked work item with a date, or an explicit decision not to fix (which itself requires a sign-off).

The quarterly gate review surfaces the override log. A rising override count is the signal that the gate has drifted from reality — either thresholds are tuned wrong, or the team's engineering shape has changed, or the suite has a flaky case that keeps getting overridden. All three deserve attention.

## Core concept 3 — the decision rule, in a short table

For each eval the brief names, decide which tier it belongs in. Use these rules:

| If the eval is … | … put it in |
|---|---|
| Deterministic, fast (< 5s), prompt / parser / contract shape | PR gate |
| Small behavioural subset against cached responses | PR gate |
| A guardrail contract smoke test | PR gate |
| A trace-field / observability contract test | PR gate |
| LLM-as-judge based, any size | Release gate |
| Retrieval quality against the full known-answerable set | Release gate |
| Tool-use correctness, especially multi-turn | Release gate |
| Red-team / adversarial / jailbreak suite | Release gate |
| Cost-at-modal-request-size on a realistic trace sample | Release gate |
| Latency at p95 / p99 on a realistic trace sample | Release gate |
| Online behaviours — A/B win rate, canary metrics, shadow comparisons | Neither — chapter 06 |

The rule is **not** "slow things go in release"; it is "non-deterministic, judgment-heavy, and expensive things go in release." A behavioural task-success eval on 500 cases with temperature-zero sampling can be fast and belongs in the PR tier; the same 500 cases judged by an LLM-as-judge belongs in release.

## A worked example — the gate policy for one feature

Returning to the doc-Q&A agent from chapter 02.

- **PR gate (budget: 4 minutes).**
  - Prompt-template rendering unit tests (10 cases, < 10s).
  - Tool-schema contract tests — every tool's JSON schema validates against its declared signature (< 5s).
  - Trace-field contract tests — the application emits OpenTelemetry GenAI fields on every call (< 5s).
  - Behavioural subset: 50 cases from the golden set, run with response cache; cache-miss falls back to live model call with temperature-zero (~2 minutes).
  - Guardrail smoke tests: 5 canonical refusals (ticket contains "ignore all previous instructions" → agent still answers in-scope) (< 30s).
  - Registry lint: does this PR touch `prompts/*.md`? If so, is the registry metadata updated and does an eval case reference the new prompt?

- **Release gate (budget: 25 minutes).**
  - Full behavioural suite: 200 golden cases.
  - Judge-based quality: Ragas faithfulness + answer relevance on the 200 cases, 3 judge runs averaged, 95% bootstrap CI reported.
  - Retrieval quality: recall@10 and precision@3 on 150 known-answerable queries.
  - Tool-use correctness: 50 multi-turn cases in Inspect, including mocked tool failures.
  - Red-team / adversarial: 100 prompt-injection cases (LLM01), 20 sensitive-data probes (LLM02), 20 excessive-agency probes (LLM06).
  - Cost: median per-request cost against $0.04 ceiling.
  - Latency: p95 against 3s target.

- **Thresholds (release gate).**
  - Behavioural: no more than 2-point regression vs. prior release (95% CI).
  - Faithfulness ≥ 0.85 (lower CI bound); no more than 0.02 regression.
  - Retrieval recall@10 ≥ 0.90; no more than 0.03 regression.
  - Tool-use correctness ≥ 0.95.
  - Prompt-injection resist rate ≥ 95%. **Absolute floor** — no override.
  - Sensitive-data leakage = 0. **Absolute floor** — no override.
  - Modal cost ≤ $0.04. Overridable with a cost-remediation plan.
  - p95 latency ≤ 3s. Overridable with a latency-remediation plan.

- **Override policy.**
  - PR gate: reviewer on shift + eval engineer. Logged.
  - Release gate (quality): manager + feature owner. Logged in release notes.
  - Release gate (cost / latency): manager + feature owner + remediation date within 14 days.
  - Release gate (safety absolute floors): **no override**. Fix and re-run.

This policy fits on one page. It is specific enough that the on-shift engineer can read it and know what to do when CI fails. It is specific enough that a new engineer can look at a failing gate and know whether they can merge.

## What good looks like

- There are exactly two tiers. Engineers do not debate which gate a case "really" belongs to — the written policy answers it.
- PR gate finishes in under 5 minutes, and the team can recite why (what caching, what subset, what determinism strategy).
- Release gate takes under an hour, costs are pinned, and the last three releases list the eval numbers that gated them.
- Overrides are logged. The quarterly review looks at the log and tightens thresholds or refactors the suite in response.
- Safety absolute floors exist and have never been overridden. A team that has overridden a safety floor has already had an incident.

## What bad looks like

- One tier. Either "evals run on every PR" (slow, flaky) or "evals run before release" (missed-regression-prone).
- PR tier contains an LLM-as-judge eval; CI is flaky; reruns are routine; engineers lose trust.
- Thresholds expressed as adjectives ("high quality"); the gate can never reliably block or pass, because the number is not specified.
- Safety floors overridable by a single engineer. The first incident teaches the team they were never real floors.
- No override log. Every failure is debated from scratch; the system never learns.

## Summary

- Two tiers: PR gate (fast, deterministic, narrow) and release gate (comprehensive, judge-based, expensive). One tier always fails.
- PR gate owns prompt / parser / contract / observability tests, a small behavioural subset with caching, and registry-lint checks. Target: under 5 minutes.
- Release gate owns judge-based evals, full retrieval and tool-use suites, adversarial / red-team suites, cost and latency acceptance. Target: under 1 hour.
- Thresholds are numbers with definitions, confidence postures, regression allowances, and absolute floors. Safety floors are non-overridable.
- Override policy names who can override, what it costs, what is non-overridable, and the remediation expectation. The quarterly review reads the override log.

The next chapter is about the versioned artefacts the gates read from — the prompt, judge, and golden-dataset registries that make PR and release gates reproducible across time.
