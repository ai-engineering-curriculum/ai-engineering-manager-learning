# Prompt, Trace, and Judge Review as Code Review (R12)

A code review that treats a prompt change the same as a formatting change has already failed. The change in question is a modification of a probabilistic decision-maker that will run against real traffic. "LGTM, string change" is not a review of it. This chapter is about operating prompt / trace / judge review as **a code-review requirement** — what reviewers check, who signs off, what the release notes carry when a prompt ships, and the anti-patterns that otherwise defeat the discipline.

This is the R12 chapter. The craft of actually reading a prompt or a trace and telling whether it is good is the `ai-eval-engineer` / `applied-ai-engineer` IC craft; the manager's job is to **require the review surface, assign reviewers with the right competencies, and make the review visible in release notes.**

## Motivation

Three patterns that recur when code review for AI-side changes is treated as general code review:

- **The prompt change reviewed only for grammar.** A PR renames a few variables and changes `"Be concise"` to `"Be very concise, and never speculate."` The reviewer approves on the variable rename. Downstream, behaviour shifts in ways that evals would have caught — if the review had asked for them.
- **The trace change nobody reviewed semantically.** A PR adds a new span to the trace, naming it `"agent-loop"`. It does not emit the OpenTelemetry GenAI fields. Three months later, the online monitor that was supposed to detect drift cannot distinguish "agent generation" from "tool invocation" spans. Nobody flagged it at review.
- **The judge rubric that landed through a UI.** The eval engineer edited the judge prompt in the observability tool's UI — "just a wording tweak." No PR. No review. The next week's release gate reads 3 points higher than the one before; the "improvement" is noise from the rubric edit, and because there's no PR, there's no audit trail.

Every one of these is a case the general code-review policy does not catch, because it is not optimised to. The AI-side review surface needs a specific, named discipline.

## Core concept 1 — the five change types and their minimum reviewers

An AI-engineering codebase has five change types that require AI-specific review. For each, the manager publishes a minimum-reviewer rule in the repository's `CODEOWNERS` (or equivalent) so the PR tool enforces it automatically.

### Change type 1 — a prompt change

A change to any entry in the prompt registry (chapter 04) — system prompt, few-shot template, tool-description string, retrieval-formatted-context template, judge prompt.

- **Minimum reviewers:** one applied-AI engineer who ships against this prompt + one eval engineer who owns the evals that exercise it. On safety-sensitive prompts (system prompts for the whole surface, refusal policies, red-team-relevant refusals), additionally the safety peer (`agentic-safety-engineer` / `ai-risk-engineer`) — see `mod-106`.
- **What they check:** registry entry is bumped and intent is written; eval case added or updated for the targeted behaviour; expected eval delta is stated in PR description; the PR does not change two artefacts at once without justification; release-notes fragment is present.

### Change type 2 — a judge rubric change

A change to any entry in the judge registry — rubric text, scoring scale, calibration-set reference, judge model.

- **Minimum reviewers:** eval engineer (always) + one applied engineer who ships against the evals this judge gates.
- **What they check:** registry entry is bumped; judge is re-run against the calibration set; the new judge–human agreement number is reported in the PR and in the entry; agreement is above the policy floor (chapter 04); the release-gate history view is annotated with the version boundary.

### Change type 3 — a tool-schema change

A change to any tool a tool-using agent can call — its name, its JSON schema, its description string, its authorisation posture.

- **Minimum reviewers:** the agentic / applied engineer who owns the agent + safety peer (tools affect the agent's effective capabilities — a `mod-106` concern) + the downstream service's owner (the tool calls a backend; that backend's team must agree to the contract).
- **What they check:** tool-registry entry is bumped; prompts that reference the tool are updated; tool-use evals are refreshed and re-run; the authorisation-posture document is up to date; release-notes fragment includes the tool-schema change under a "capability change" heading.

### Change type 4 — an eval-dataset change

A change to any entry in the golden-dataset registry — added cases, removed cases, edited expected outputs.

- **Minimum reviewers:** eval engineer + the applied engineer whose feature consumes the eval (so the change doesn't silently reshape the feature's measurement).
- **What they check:** dataset-registry entry is bumped; changelog documents added / removed / edited cases with reasons; the release-gate history view is annotated at the version boundary so eval deltas remain interpretable; removed cases carry justifications.

### Change type 5 — a trace / observability change

A change to the application's trace emission, span naming, attribute shapes, or sampling policy.

- **Minimum reviewers:** the applied / agentic engineer who owns the surface + an observability-platform owner (if one exists — in smaller teams this is the manager).
- **What they check:** trace fields align with OpenTelemetry GenAI semantic conventions; registry identifiers (prompt id, prompt version, judge id) are emitted; the online monitor that reads from this trace still makes sense; the sampling policy respects retention and PII posture.

### The "normal" application code change

Not every PR is one of the five above. A PR that only changes product code (adds an endpoint, refactors a service) uses the normal code-review path. The discipline here is: do not *downgrade* normal PRs to the AI-side rules, and do not *upgrade* AI-side PRs to the normal rules. The two sets compose cleanly — a PR that touches both lives under both.

## Core concept 2 — what a reviewer actually looks for

The review is not a vibe check. The reviewer answers specific questions. The manager publishes these as a review checklist (a PR-template "Reviewer checklist" section, or a `REVIEW_CHECKLIST.md` referenced from the PR template).

### Prompt-change checklist

1. Does the registry entry carry: intent, author, date, expected eval delta, models validated against?
2. Is at least one eval case added or updated that exercises the targeted behaviour?
3. Is the PR description's expected eval delta reconciled with the release-gate result? (On the gate run, is the delta in the expected direction?)
4. Does the PR change only this prompt — or, if multi-artefact, is the exception justified?
5. Does the release-notes fragment mention this prompt change under "prompts"?
6. On safety-sensitive prompts, does the change affect refusal behaviour? If yes, is a red-team case family covering the new refusal boundary added?

### Judge-change checklist

1. Is the new judge–human agreement number ≥ the policy floor?
2. If the judge model changed, is the agreement re-measured on the current calibration set?
3. Is the release-gate history annotated so pre-change and post-change numbers are not read as comparable?
4. Is the calibration-set version referenced correctly?
5. Is there a plan for the pending re-measurement on the full gate dataset?

### Tool-schema-change checklist

1. Did the downstream service team agree to the contract?
2. Are prompt-registry entries that reference the tool updated?
3. Does the tool-use correctness eval have a case for the new signature (success path *and* failure path)?
4. Does the change affect authorisation? If yes, is the authorisation-posture document (R15, `mod-106`) updated?
5. Is a tabletop or review of the new capability scheduled (if the change meaningfully expands agency)?

### Dataset-change checklist

1. Provenance for new cases is documented (hand-authored, production-sampled, red-team, external corpus — which).
2. PII / privacy posture of new cases is compatible with the dataset's existing posture.
3. Removal justifications are present for removed cases.
4. If cases moved between training-side and holdout, the rationale is documented and the holdout-access log is updated.

### Trace-change checklist

1. Spans align with OpenTelemetry GenAI semantic conventions (or the deviation is justified).
2. Registry identifiers (prompt id, prompt version, judge id, dataset id when applicable) are emitted as attributes.
3. The sampling policy is compatible with the PII / retention posture.
4. Existing online monitors still work against the updated trace shape; broken monitors are fixed in the same PR or flagged as a follow-up.

A reviewer who checks every item on the relevant list is doing R12. A reviewer who skims a prompt and clicks approve is not — and the manager's job is to say so, and to tighten the PR template or the automation until the checklist items are enforced.

## Core concept 3 — stakeholder sign-offs for the heavy changes

Most changes land under the two-reviewer rule above. A smaller class of changes requires a named sign-off from a stakeholder outside the engineering team. The manager pins which changes require sign-offs and from whom.

The heavy-change categories, and the sign-offs they demand:

- **A change to the system prompt or the refusal / policy-aware prompt layer.** Sign-off from the safety peer (`agentic-safety-engineer` / `ai-risk-engineer`), and, where the organisation has one, the governance owner. Reasoning: the system prompt is the surface a jailbreak most often tries to subvert; a change to it is a safety posture change.
- **A new tool or a tool-authorisation expansion.** Sign-off from the safety peer (effective agency change) and the owner of the downstream resource (if the tool writes to an external system). See `mod-106` R15 depth.
- **A model-change in the production serving path.** Different from a prompt change — this is a provider / model-ID change. Sign-off from the manager (you), and release-triad sign-off per `mod-106` (the model is one leg of the triad).
- **A judge-model change in a gating judge.** Sign-off from the eval engineer and the manager. Rationale: a judge-model change rotates the gate's yardstick and requires explicit acknowledgement that historical numbers are now a different measurement.
- **A dataset-provenance change that brings in externally-sourced data.** Sign-off from the data-owner (owner-of-record for data provenance — often the eval engineer or a dedicated `ai-data-engineer`) and, if the data is licensed, from legal.

These sign-offs do not block every PR; they block the small number of PRs that materially shift the system's posture. The manager's hardest work here is to pre-publish the list so the sign-off is not negotiated in the PR thread.

## Core concept 4 — release-note content for AI-side changes

A release note that says "improved prompt" is not a release note. The manager's policy pins what release notes carry when AI artefacts change, and the release-notes-fragment attached to a PR is the input.

Release-note sections to publish (adapt to your team's existing format):

- **Prompt changes.** For each: the prompt identifier, the old → new version, the one-sentence intent, the measured eval delta (release-gate numbers), and the PR link.
- **Judge changes.** For each: the judge identifier, the old → new version, the new judge–human agreement number, and a note that pre-change eval numbers are not directly comparable to post-change numbers.
- **Tool / capability changes.** For each: the tool identifier, the signature change summary, the new authorisation posture (if different), and the safety sign-off attestation.
- **Dataset changes.** For each: the dataset identifier, the old → new version, cases added / removed / edited, and the reason (concise).
- **Trace-shape changes.** For each: the new or renamed span / attribute, the dashboards or monitors affected, and the follow-up tickets if any.
- **Model changes.** For each: provider, old → new model ID, release-triad sign-off, measured deltas on behavioural / safety / cost / latency.

The reason for publishing all of this is that the release note is the single artefact an incident responder reads at 3 a.m. (chapter `mod-107`) and the single artefact the quarterly governance review reads. If the note is thin, both reviews are blind.

For external-facing products, filter the public release note for what is safe to share (prompt text usually stays internal; eval deltas and capability-change descriptions can be externalised). Keep the internal release note complete.

## Core concept 5 — the reviewer competency matrix

A review is only as good as the reviewer. The manager cannot do this review personally for every PR, but the manager can ensure the people who do are trained.

The reviewer-competencies the team needs, and where each competency lives:

- **Prompt engineering judgment** — can tell a well-structured prompt from a brittle one; recognises few-shot leakage, instruction stacking, format-induced regressions. Lives in the applied and eval engineers.
- **Trace / observability judgment** — can read a trace and tell whether the span structure is defensible, whether the attributes capture what matters, whether sampling will undercount an important class. Lives in the applied engineer with observability depth, or the platform team if one exists.
- **Judge judgment** — can tell whether a judge rubric is operationalisable, whether the calibration set is adequate, whether the judge–human agreement number is a real floor. Lives in the eval engineer.
- **Agent / tool-use judgment** — can read a multi-turn agent trace and tell when the agent should have refused, when it should have called a different tool, when authorisation was exceeded. Lives in the agentic engineer.
- **Retrieval judgment** — can tell whether a chunking choice or a re-ranker setting is right for this corpus. Lives in the RAG engineer.
- **Safety / policy judgment** — can tell whether a prompt / tool change widens the attack surface, whether a refusal boundary is defensible, whether a red-team case family covers the new behaviour. Lives in the safety peer.

The manager's job is to **map the five change types to the competencies above and publish the mapping in CODEOWNERS.** A PR touching a system prompt gets pulled into a queue that at minimum includes the applied engineer and the eval engineer; a PR touching a tool additionally gets the agentic engineer and the safety peer. A reviewer who does not have the competency the change requires routes it to someone who does.

## Core concept 6 — anti-patterns that defeat R12

The discipline above is undermined by four recurring anti-patterns. Name them so the team can call each other out.

- **"LGTM, string change."** The prompt change is treated as a string literal. The reviewer approves without reading the intent or the eval case. Mitigation: automated registry-lint (the PR cannot merge without a bumped registry entry and an updated eval case).
- **The in-UI edit.** The eval engineer edits the judge prompt in the observability tool's UI "to save time." No PR exists. The audit trail is lost. Mitigation: the observability-tool UI is set to read-only for gate-critical artefacts, or edits are auto-converted into PRs (several platforms support this), or the policy is explicit: UI edits are for exploration, not for gate state.
- **Multi-artefact drive-by.** A PR changes a prompt, a judge, and a dataset "because we investigated all three." The eval delta is unreadable. Mitigation: the one-artefact-per-PR rule (chapter 04), enforced by the reviewer, with explicit multi-artefact-exception justification required.
- **Review-while-running.** The PR author merges the PR, then "runs the evals after." The release gate catches it, maybe. The reviewer did not read the eval evidence because no eval was yet run. Mitigation: the PR cannot merge without the PR-gate result present and the expected release-gate delta stated.

## A worked example — the review of one prompt change

A PR titled "support-agent system prompt: more aggressive citation requirements."

The author's PR description:

> Updates `support-agent/system` to v4 (from v3). Adds the sentence "Every factual claim must cite a specific KB chunk; if none applies, say so and do not answer." Expected eval delta: faithfulness +2 points (based on pre-run on a 20-case subset); behavioural task-success roughly neutral; latency neutral.
> 
> Registry: `prompts/support-agent/system-v4.md` (new); frontmatter intent, author, date, expected delta, validated-against.
> 
> New eval cases: three faithfulness-targeting cases added to `datasets/support-agent/golden-v4` (dataset bump to `v4-faith`). Rationale: existing suite was light on "no answer available" cases.
> 
> Release notes: `release-notes/2026-10-07-pre.md` fragment attached — section "Prompts" and section "Datasets."

The reviewers:

- The applied engineer (owns the production surface) checks: prompt reads well, no instruction-stacking surprises; the intent is reflected in the prompt text; the eval cases actually probe the new behaviour; the latency expectation is realistic.
- The eval engineer checks: the registry entry has the right shape; the dataset bump is justified; the pre-run subset was large enough to be a signal (they ask the author to add two more "answer available but citation ambiguous" cases); the release-gate result on this branch, when run, will cover the full suite and the new cases.
- (If the change had widened refusal posture — e.g., "never answer questions about X" — the safety peer would also have been pulled in.)

The release gate runs. Faithfulness lands +1.6 points (95% CI [0.9, 2.3]), behavioural task-success -0.2 points (NS), latency -4 ms (NS). All three are in the expected direction, no absolute floors broken. Manager approves. PR merges. Release-notes fragment lands under the next release's "Prompts" section verbatim.

A reviewer at week eight can read the release note, find the PR, find the registry entry, find the eval cases, and reconstruct every decision. That is R12 operating correctly.

## What good looks like

- CODEOWNERS automatically pulls in the right reviewers for each AI-side change type.
- The PR template has an AI-side section (registry version, eval cases added, expected delta, release-notes fragment) that cannot be left blank.
- Release notes read like a system-change record, not marketing copy. An incident responder can use them.
- UI edits to gate-critical artefacts are either not possible or auto-converted to PRs.
- The sign-off list for heavy changes is pre-published; sign-offs are not negotiated in the PR thread.

## What bad looks like

- A prompt change approved in two minutes with a `:+1:` reaction.
- A judge rubric updated through a vendor UI; no PR, no audit trail.
- Release notes that say "improved reliability." Incident responders cannot use them. Nobody reads them.
- The same person authors a prompt change and approves it. CODEOWNERS is empty.
- A multi-artefact PR lands with "+5 points" in the title; nobody can tell which artefact caused which part of the gain, and three months later nobody remembers.

## Summary

- Five AI-side change types each have a minimum-reviewer rule in CODEOWNERS: prompt, judge, tool-schema, dataset, trace.
- Each change type has a review checklist. The PR cannot merge until the checklist is satisfied.
- Heavy changes require stakeholder sign-offs — safety peer, governance, release-triad, legal (dataset provenance), manager. The sign-off list is pre-published.
- Release notes for AI-side changes carry the artefact identifier, version delta, intent, measured eval delta, and sign-off record. They are the input to incident response and quarterly governance review.
- Reviewer competency matrix maps change types to the engineers who can read them. The manager publishes the mapping.
- Four anti-patterns to name and defeat: "LGTM, string change," the in-UI edit, multi-artefact drive-by, review-while-running.

The next chapter picks up the other half of the eval cadence — what runs *after* merge: canary, A/B, shadow, continuous production monitoring, and the drift taxonomy that distinguishes generation / judge / retrieval / tool-use regression.
