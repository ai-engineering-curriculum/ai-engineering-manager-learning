# Exercise 03: Prompt and Judge Registry Standards

**Estimated effort:** 3 hours · **Output:** one registry standards document + three sample registry entries (one prompt, one judge, one dataset) + one updated PR-template section + one CODEOWNERS fragment

## Objective

Produce the standards and example entries that turn chapters 04 and 05 from theory into enforcement: a single document that names the registry schemas for prompts / judges / datasets, three worked examples showing what a good entry looks like for your feature, a PR template that an engineer actually has to fill in, and a CODEOWNERS fragment that automatically pulls in the right reviewers. The exercise is enforcement-side: the artefacts should allow registry-lint in CI to block a PR that bumps a prompt without updating the eval cases.

## Prerequisites

- Chapter 04 (`The Three Registries — Prompts, Judges, Golden Datasets`).
- Chapter 05 (`Prompt, Trace, and Judge Review as Code Review (R12)`) — the PR-template and CODEOWNERS pieces of this exercise lean on R12.
- The feature from exercises 01 and 02 (same feature, so the three exercises compose into a coherent operating setup).

## Problem statement

Produce four artefacts.

### Artefact 1 — The registry standards document (90 minutes)

A 2–3 page markdown document, published in the repo (e.g., `docs/eval/registries.md`). Sections:

#### 1. Prompt registry

- **Location** — the directory (e.g., `prompts/<feature>/`), the file naming convention, and the single-source-of-truth statement (see chapter 04 on avoiding dual registries).
- **Schema** — the required frontmatter fields: `id`, `version`, `author`, `date`, `intent`, `supersedes`, `eval-delta`, `validated-against` (model list), `deprecation-plan`. The mandatory body: the prompt text (or template). Optional: notes on few-shot examples, formatting constraints.
- **Versioning** — one-sentence rule: "every text change bumps the version." Version-naming convention.
- **Reference discipline** — how application code references a prompt by identifier (not by string literal), how tests reference prompts, how traces emit the prompt identifier + version as an attribute.

#### 2. Judge registry

- **Location**, **schema**, **versioning** parallel to the prompt registry.
- **Judge-specific fields** — `type` (llm-as-judge / rubric / statistical / external), `judge-prompt` (reference to a prompt-registry entry if LLM-as-judge), `judge-model`, `calibration-set` (reference to a dataset-registry entry), `agreement-metric` + `agreement-value` (e.g., `cohens-kappa: 0.74`), `last-recalibrated`, `policy-floor`.
- **Calibration discipline** — when the judge is re-measured, who owns it, what happens when agreement falls below the policy floor.
- **Historical boundary flagging** — how the release-gate-history view is annotated at judge-version boundaries (so eval readings pre/post are not read as directly comparable).

#### 3. Golden-dataset registry

- **Location**, **schema**, **versioning** parallel to the prompt registry.
- **Dataset-specific fields** — `id`, `version`, `provenance` (per-source), `size`, `pii-posture`, `retention`, `owner`, `license-if-applicable`, `changelog` (per-version delta: added / removed / edited).
- **Holdout discipline** — how the holdout entry's access is controlled, who may run against it, how runs are logged.
- **Access and PII** — who may read, which fields require redaction, the retention policy (chapter 07 for upstream).

#### 4. Cross-cutting rules

- Single-source-of-truth per registry — no dual systems.
- The one-artefact-per-PR rule (chapter 04) with the exception path.
- The trace-emits-registry-identifiers discipline.
- The deprecation discipline: deprecated entries are flagged, not deleted; successors are named.

### Artefact 2 — Three worked example entries (45 minutes)

Pick one of each and write a complete, valid example that conforms to your standards document. These must be shippable-to-the-repo examples, not sketches:

1. A **prompt-registry entry** — e.g., the system prompt for the agent in your feature. Include full frontmatter and the full prompt text.
2. A **judge-registry entry** — e.g., a faithfulness judge for the feature. Include the LLM-as-judge prompt-reference, the judge-model, the calibration-set reference, and an invented-but-realistic agreement number (or `[placeholder: measure before gate-promotion]`).
3. A **golden-dataset entry** — e.g., 2–3 sample cases plus the full `metadata.yaml` and the `changelog.md` for the first two versions (initial + a later addition).

Keep examples short on content (2–3 cases for the dataset) but complete on metadata.

### Artefact 3 — The PR template section (20 minutes)

A markdown section titled `## AI-side changes` to append to the repository's PR template. It must have:

- Checkboxes the author fills in: `[ ] prompt registry entry bumped`, `[ ] judge registry entry bumped (if applicable)`, `[ ] dataset registry entry bumped (if applicable)`, `[ ] tool-schema registry entry bumped (if applicable)`, `[ ] trace changes reviewed (if applicable)`.
- Fields the author fills in: `Expected eval delta: …`, `Artefacts changed in this PR (one-artefact-per-PR rule — justify multi-artefact changes): …`, `Release-notes fragment: <path/to/fragment.md>`.
- A pointer to the registry-lint check in CI and the standards document.

This section must be concise — engineers skip long PR templates. Three lines of "yes/no" plus two or three short text fields is ideal.

### Artefact 4 — The CODEOWNERS fragment (25 minutes)

A fragment of `CODEOWNERS` (GitHub syntax; adapt to your SCM) covering the five AI-side change types from chapter 05:

- `prompts/** @applied-oncall @eval-engineer` (adapt to your handle/team names).
- `judges/** @eval-engineer @applied-oncall` (plus `@safety-peer` on gate-critical judges if your org structures it this way).
- `datasets/** @eval-engineer @data-owner`.
- `tools/schemas/** @agentic-oncall @safety-peer @downstream-owner`.
- The feature's trace-emission surface — point at the applied engineer owning the surface (plus `@safety-peer` on safety-adjacent spans).

Where no appropriate owner yet exists, name the gap in a one-line comment: `# TODO: safety-peer not yet staffed; defaulting to <manager> until <date>`.

## Requirements

- The standards document is 2–3 pages, has all four sections, and reads as a standard — not a tutorial. A new engineer should be able to grep it for "what does a judge entry need" and find the answer in 30 seconds.
- The three worked example entries are schema-valid against the standards document. They are specific enough to actually ship (not `foo` / `bar` placeholders); they can carry `[placeholder: …]` for numbers you cannot measure in-exercise, but not for structure.
- The PR-template section fits in roughly a quarter-page when rendered. Three to five checkboxes + two to three short text fields. The AI-side rule *must* be visible in the PR UI.
- The CODEOWNERS fragment matches the five change types and names actual owner handles (or marked gaps).
- Where your feature does not use one of (judges, datasets, tool-schemas, traces), you still provide the standard — the standards document is not scoped to your feature's features; it is the team's standard. You can say in the worked-example section "my feature does not currently include a tool-schema, so no example is produced — the standard still applies."

## Starter guidance

- The hardest field in the judge entry is the `agreement-value`. Most teams starting this exercise don't have one. The correct move is to produce the entry with `[placeholder: measure before this judge serves as a gate]` and name the action: who runs the calibration, with what set, by when. Chapter 07's rater-calibration discipline is the companion.
- The one-artefact-per-PR rule tends to be contested in review. Pre-write your justification — chapter 04 has it; adapt to your voice. An engineer who understands *why* the rule exists accepts it; an engineer who sees it as bureaucracy routes around it.
- The CODEOWNERS fragment is the leverage artefact. If you get it right, registry discipline is enforced by tooling — the PR cannot merge without the right reviewers, and the right reviewers know their checklist. Spend the twenty minutes carefully.
- Borrow shape, not content, from other eval-mature repos — `Inspect`'s and `Ragas`'s public evals, if they are open, can illustrate judge / dataset shapes. Do not adopt their schemas wholesale; adapt what rings true.
- If your team is new to registries and you expect pushback: pilot the standards on one feature's PRs for a quarter and run a retrospective (stretch goal below). A pilot defuses the "this is bureaucracy" response because it generates evidence.

## Acceptance criteria

- Standards document covers prompt / judge / dataset registries + cross-cutting rules, and is referenceable by the PR template and CODEOWNERS.
- Three worked example entries conform to the schema and are specific to your feature.
- PR-template section is short, specific, and names the enforcement artefact (registry-lint).
- CODEOWNERS fragment covers the five change types, names real owner handles, and flags gaps where no owner exists yet.
- A peer manager reading all four artefacts together can set up equivalent registry enforcement on their repo in under a day.
- A new engineer reading these artefacts and opening a PR to change a prompt knows exactly what to update, who to assign, and what CI will check.

## Stretch goals

- **Registry-lint check script.** Write a minimum-viable `registry-lint` script (any language) that enforces: (a) a PR touching `prompts/*.md` must also touch at least one file in the `datasets/*` or `evals/*` tree; (b) every prompt file has valid frontmatter per your schema; (c) every judge entry has a non-empty `agreement-value` or a `[placeholder: …]`. Hook it into the PR tier (chapter 03). Pilot on one feature for a quarter.
- **Judge-stability dashboard.** Build (or specify) a simple dashboard that reads the judge registry, shows every active judge with its last-recalibrated date and current agreement value, and flags judges past their policy's recalibration cadence. This surfaces the "silent judge drift" failure chapters 04 and 06 name.
- **Full-team rollout retrospective.** Pilot the standards on your team for one full quarter. At the end, write a 1–2 page retrospective: what the standards caught, what engineers pushed back on, what was over-engineered, what was missing. Refine the standards. This is how manager-authored discipline becomes team-owned discipline.
