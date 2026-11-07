# Exercise 02: PR vs. Release Gate Eval Policy

**Estimated effort:** 3 hours · **Output:** one written release-gate policy (2–3 pages) plus the one-page override-policy sidecar, specialised for one feature

## Objective

Produce the written release-gate policy from chapter 03 — the document that every engineer on your team reads to answer "which of my changes trigger which gate, with what threshold, and what happens when a gate fails." The policy must be specific enough that CI can enforce it (names the subsets, thresholds, and the override surface) and portable enough that a new team member can follow it on day one without asking you.

## Prerequisites

- Chapter 03 (`Deciding PR Gates vs. Release Gates`).
- Chapter 02 (`Commissioning an Eval Suite`) at a skim — the policy reads the suite's thresholds.
- Chapter 06 (`Offline and Online Eval Cadence`) at a skim — the release gate is one of five stages; the policy's scope ends at pre-release.
- Exercise 01 for the feature's commissioning brief, OR a well-understood feature at your team that already has evals in place.

## Problem statement

Produce two artefacts for one specific feature.

### Artefact 1 — The release-gate policy (2 hours)

A 2–3 page markdown document with the following sections. Every section is specific to your feature; generic language fails the exercise.

#### Section A — The two-tier architecture (15 minutes)

One paragraph. State that your team runs a PR gate and a release gate, and why — in your own words tied to the pressures on your team specifically. If your team has an operational reason to depart from the two-tier shape (e.g., a very-low-traffic internal tool for which "release gate" and "merge" are the same action), state that departure here and explain.

#### Section B — PR-gate contents (30 minutes)

A numbered list of every eval or test that runs in the PR tier. For each entry:

- Name (e.g., `prompt-render-unit`, `behavioural-subset-50`, `guardrail-smoke`, `registry-lint`).
- Mechanism (deterministic test, cached-response subset, contract test, lint).
- Approximate wallclock time.
- Pass criterion (a number or a bool).

Rule: the full PR tier must fit in **under 5 minutes** of CI wallclock. If your list exceeds that, cut it; what did not make the cut goes to the release tier.

#### Section C — Release-gate contents (30 minutes)

A numbered list of every eval that runs in the release tier. For each entry:

- Name.
- Mechanism (full suite behavioural, LLM-as-judge faithfulness, retrieval recall/precision, tool-use correctness, red-team suite, cost / latency acceptance, …).
- Approximate wallclock and dollar cost (provide ranges if exact numbers are not known; mark `[placeholder: measure in first run]` where needed).
- Pass criterion (as a number or set of numbers, see section D).

Rule: the full release tier should fit in **under 1 hour** of wallclock, ideally under 30 minutes. If it does not, name the reason and the path to compression (parallelism, judge-sampling, subset strategy).

#### Section D — Thresholds, with the full anatomy (30 minutes)

For each release-gate eval, state:

- The **metric definition** (which metric, which version of which tool, which judge version).
- The **number** (e.g., `0.85`).
- The **confidence posture** (point estimate, or lower CI bound; which CI, how computed).
- The **regression allowance** against the previous release (e.g., `-0.02 allowed`).
- The **absolute floor** (e.g., `jailbreak success rate must be ≤ 2%`, non-overridable).

Produce one summary table of the thresholds plus any per-metric commentary. A reviewer of a failing gate should be able to find the relevant row and know what the failure means in one lookup.

#### Section E — How gates are invoked and recorded (15 minutes)

- Where the PR gate runs (CI system name, job name).
- Where the release gate runs (CI system name, job name, when triggered — tag push, release-candidate branch, manual button).
- Where the results are stored and how far back they are retained.
- Where a running engineer sees the status (PR check, release dashboard, Slack channel).

### Artefact 2 — The override policy sidecar (60 minutes)

A one-page document. Keep it short; a long override policy is a sign the team has not decided what overrides are for.

Sections:

- **Who can override a PR-gate failure.** Role, not individual (e.g., "the on-shift reviewer" or "the eval engineer"). Log format and where overrides land.
- **Who can override a release-gate failure.** Usually a named role plus one additional stakeholder on safety-adjacent failures (the safety peer per `mod-106`). Include the release-triad sign-off requirement if applicable.
- **What is non-overridable.** Enumerate the specific metrics and thresholds. Safety absolute floors go here. Explicitly: "no single engineer, no leadership escalation, overrides these." The remediation is to fix the system, not to bypass.
- **Override costs.** The log entry (one paragraph justification), the release-notes mention, the quarterly review inclusion.
- **Remediation expectations.** For each non-safety override — the "when fixed by" expectation: inline next-day, tracked work-item with a date, or explicit written decision not to fix with a sign-off.
- **The quarterly override review.** When, by whom, what the memo covers.

## Requirements

- The policy is a specific document, filled in for a specific feature, not a template with placeholders left behind. If a number is unknown, use `[placeholder: measure in first run]` **plus** the action that will produce the value.
- The PR tier fits under 5 minutes of CI wallclock. If your plan does not, the fix is to move items to the release tier, not to shrug.
- Every threshold is a number with a definition. "High quality" or "no regression" fails the exercise. "Faithfulness ≥ 0.85 (lower 95% bootstrap CI bound on Ragas faithfulness v0.1 on golden-vN); no more than 0.02 regression against the last release" passes.
- Every release-gate eval has a threshold entry. No eval runs without a pass criterion.
- At least one absolute floor is marked non-overridable. Most teams have one for prompt-injection resistance or sensitive-data leakage; name yours.
- The override policy fits on one page. If it is longer, you have not decided what overrides are for.
- At least one quarterly review ritual is scheduled (date or beat), with a named memo output.

## Starter guidance

- Begin with Section C (release-gate contents). It is the longest section and it clarifies what is **not** release-tier — everything else is a candidate for PR-tier.
- Build Section B (PR tier) with the 5-minute budget as a hard constraint. If a candidate test takes 90 seconds, you have room for maybe three such tests plus a dozen fast unit tests. Choose accordingly.
- When you write thresholds (Section D), ask the eval engineer for the actual numbers from the most recent gate run on this feature. Do not invent numbers to make the policy look good; the policy should match the gate's current state and name the aspiration separately.
- The absolute-floors list is the piece leadership will most often push on. Prepare a one-paragraph justification per floor so the first "why is this non-overridable" conversation goes well. Chapter 03 has the frame; apply it to your feature.
- The override policy is where most policies rot. The cheapest preventive move is the quarterly review — pin the first one on the calendar before you finish this exercise.

## Acceptance criteria

- Artefact 1 is 2–3 pages, has all five sections (A through E), is specific to one feature, and matches your team's current infrastructure (CI system, observability store, release process).
- Artefact 2 is one page, has all six override-policy sections, and names at least one non-overridable floor.
- PR tier fits in under 5 minutes of CI wallclock (stated); release tier fits in under 1 hour (stated).
- Every release-gate eval has a numeric threshold with confidence posture, regression allowance, and absolute-floor flag.
- The quarterly override review has a date / beat and a named memo output.
- A new engineer joining the team can read these two documents and answer, within five minutes: "which of my changes trigger which gate, and what happens when one fails."

## Stretch goals

- **Walk the policy against last quarter's releases.** For each of the last 3–5 releases of this feature, retroactively check: would the policy you wrote have passed or failed the release? What would have been overridden? What would have been blocked? Write a one-page retrospective — your policy should agree with the team's real-world judgment most of the time, and when it disagrees, either the policy or the past judgment needs revision.
- **Portability test.** Hand the policy to a peer AI Engineering Manager at another team / company (or in your organisation's AI engineering guild). Ask them to adapt it to one of *their* features. The extent to which they have to rewrite tells you how feature-specific vs. template-grade your policy really is. A policy that transfers with only Section A–D numeric edits is a strong policy.
- **CI dry-run.** Codify the policy's PR-tier contents into a draft CI configuration (GitHub Actions / GitLab CI / your tool). Run it against a recent PR and check the wallclock budget. If you exceed 5 minutes, document the compression plan.
