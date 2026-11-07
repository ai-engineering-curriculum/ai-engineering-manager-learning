# Exercise 03: Incident-to-Fixture Loop

**Estimated effort:** 2 hours · **Output:** (a) a half-page **loop policy** document that names the six stages, the owners, the definition of "loop closed," and the enforcement rules, and (b) a worked **loop-closed packet** for the incident in exercise 02, including a fixture YAML spec, the paired judge-case spec, a wiring diff against the eval-gate configuration, and the manager's loop-closed sign-off statement.

## Objective

Produce the two artefacts chapter 06 commissions. The policy is the shape of the loop as the team runs it; the packet is the proof that the loop closes on a specific incident. Together, they are what an AI Engineering Manager can point to when asked "how do incidents stop recurring here?"

A peer AI Engineering Manager, reading your artefacts, should be able to:

- Read the loop policy and describe, in two sentences, how an incident becomes a permanent regression test.
- Read the loop-closed packet and confirm the nine-item checklist from chapter 06 core concept 2 is complete — fixture in registry; fixture failing against broken code; fixture green against fixed code; judge case calibrated; AI Risk review signed; wiring to gate; postmortem action item closed; manager sign-off; dated.

## Prerequisites

- Chapter 06 — the six stages, the ownership table, the loop-closed checklist, the fixture / judge-case schemas, enforcement discipline, the five anti-patterns.
- Exercise 02's filled postmortem — the specific incident and the `fixture` action item in section 7 that points at a registry path.
- Mod-104 chapter 04 — prompt / judge / dataset registries, judge calibration against gold cases, the versioning discipline the loop fits into.
- Mod-104 chapter 03 — the PR vs. release gate architecture the fixture is wired into.
- Mod-106 chapter 04 — the red-team commissioning posture AI Risk's coverage sign-off rests on.

## Problem statement

Two artefacts, in markdown + YAML, filed where governance artefacts live.

### Artefact A — the loop policy (0.5 page, 30 minutes)

A half-page document. The team-wide rule for how incidents become fixtures. Sections:

1. **The six stages.** Capture → redact → author → broaden → wire → close. One sentence per stage naming the role that owns it.
2. **The definition of "loop closed."** The nine-item checklist from chapter 06 core concept 2 — evidence preserved; redaction reviewed; fixture authored against preserved evidence; fixture fails against broken code; fixture green against fixed code; AI Risk coverage sign-off; wired to pre-merge / release / online gate; postmortem action item closed; manager loop-closed sign-off.
3. **Enforcement rules.** Three specific rules, each explicit: (a) fixtures are wired to a gate that actually blocks regressions, not an informational check; (b) loop-closed is a precondition of postmortem sign-off (chapter 05); (c) fixtures are not deleted without manager sign-off and a successor fixture.
4. **Owners.** Named roles for each stage (not "the team") — IC, author (`ai-eval-engineer` / `applied-ai-engineer`), reviewer, AI Risk SME (`ai-risk-engineer`), platform wiring engineer, manager.
5. **Cadence.** Target: loop closed within three weeks of incident close. Slippage is reviewed at the next postmortem review (chapter 05).

### Artefact B — the loop-closed packet (one incident, 80 minutes)

Pick the incident from exercise 02. Produce the full packet:

#### B.1 — the fixture YAML spec (40 minutes)

A registry entry following the schema in chapter 06 core concept 3. Fields:

- `id`, `type: safety-fixture` (or `quality-fixture` for a quality-shaped incident), `source: incident-derived`, `incident_id`, `failure_mode` (chapter 02 mode), `severity`, `authored`, `author`.
- `ai_risk_review` — `reviewer`, `signed` (date), `coverage_note` (what the fixture *does* and *does not* cover).
- `redaction` — `policy`, `redacted_by`, `reviewed_by`.
- `cases` — at least one baseline-incident-repro case (the original trace, redacted), and at least two derived variants (the AI Risk broadening — delimiter variant, language variant, multi-turn variant, or product-specific variant). Each case has:
    - `name`.
    - `input` — the parametrised input (retrieval context / user turn / system-prompt deviation shape / tool-chain seed, as applicable), with structural tokens replacing PII / regulated content.
    - `expected` — specific behaviour the system *should* produce (refuses, produces a grounded answer, escalates to HITL, does not call out-of-scope tools).
    - `judge` — pointer to the judge-case ID (artefact B.2).
- `wired_to` — `pre_merge_gate`, `canary_online_sample`, or both.
- `threshold` — the gate's pass criterion (e.g., 100% pass pre-merge; alert on single failure online).
- `regression_history` — at least two entries: one pass against the fixed code; one fail-on-broken-code-replay against the pre-fix commit (demonstrating the fixture actually catches the regression).

#### B.2 — the paired judge-case spec (15 minutes)

A judge-registry entry (mod-104 chapter 04). Fields:

- `id`, `type: safety-judge` (or `quality-judge`), `rubric` (specific, numbered sub-questions the judge evaluates — refusal correctness, grounding, scope of action — tied to the fixture's `expected` field), `model` (the judge model, pinned), `calibration` (`gold_cases` count, `human_agreement` rate, `last_calibrated`).

Judge-case calibration is a real discipline; your calibration numbers can be plausible targets if you do not have measured data (e.g., 48 gold cases, 0.94 human agreement, last-calibrated date ≤ 30 days before the postmortem). Note calibration as `[placeholder: calibration in progress, target 2026-12-01]` where real data is not available.

#### B.3 — the wiring diff (10 minutes)

A unified-diff or configuration patch showing the fixture being added to the pre-merge gate configuration (and canary-online-sample, if applicable). Specifically:

- Where the fixture is enumerated in the gate config.
- The gate's enforcement setting (blocking vs. informational — it must be blocking for the loop to close).
- The alert configuration for the online-sample tier, if applicable.

The diff can be against a plausible config shape (`eval-gate.yaml`, `release-gate.yaml`, or your team's equivalent); specify the file name and the surrounding lines so the diff is readable.

#### B.4 — the loop-closed sign-off statement (5 minutes)

The manager's statement, as it would land in the postmortem's section 10:

> "Loop closed for inc-<id> on <date>; fixture `<registry path>` in registry; wired to `<gate tier>`; AI Risk sign-off on <date>; postmortem action item `<id>` marked complete. Loop-closed register updated."

The statement is explicit and dated. It is the thing the manager signs and the thing an auditor can later point at.

#### B.5 — the loop-closed check (10 minutes)

A nine-row table mapping each checkpoint from chapter 06 core concept 2 to the specific evidence in your packet. Columns: `checkpoint`, `signal` (what proves it is done), `location` (the specific line / file / doc).

### Remaining time (10 minutes)

In a short appendix, name at least two adjacent fixtures the AI Risk broadening stage identified but are not in this packet — fixtures the next engagement will produce. The appendix is the manager's way of recording that the loop closed on *this* instance without the broader class being assumed complete.

## Requirements

- Policy is 0.5 page; six stages named; definition of loop-closed matches the nine-item checklist; three enforcement rules stated; named-role ownership per stage; cadence target named.
- Fixture YAML covers ≥ 1 baseline-repro case + ≥ 2 derived variants.
- Fixture YAML names specific versions (model, judge, dataset, retrieval context) and structural-token redaction.
- Fixture YAML carries `ai_risk_review` with reviewer name, signed date, and coverage note that says what the fixture *does* and *does not* cover.
- Fixture YAML has ≥ 2 `regression_history` entries, one pass on fixed code and one fail on pre-fix code.
- Judge-case YAML has a specific numbered rubric tied to the fixture's `expected` field; calibration is named (gold-cases count, human-agreement rate, last-calibrated date) or marked as placeholder with a target date.
- Wiring diff shows the fixture added to a gate configuration with a **blocking** enforcement setting.
- Loop-closed sign-off is a specific dated statement referencing fixture path, gate tier, AI Risk sign-off date, and postmortem action item id.
- Loop-closed check-table maps all nine chapter-06 checkpoints to specific signals and locations.
- Appendix names ≥ 2 adjacent fixtures that the next engagement will produce.

## Starter guidance

- The fixture is written *against the preserved trace*, not from memory. This is the chapter-06 anti-pattern most teams fall into. Pin the evidence pointer at the top of the fixture; if you do not have the pointer, write `[placeholder: evidence-store path from inc-<id>]` and name the action to fill it.
- Structural-token redaction is the discipline that makes the fixture shareable without exposing PII or regulated content. Use specific tokens (`<USER_EMAIL>`, `<PATIENT_ID>`, `<INJECTION_PAYLOAD_STRUCTURE>`) that preserve the shape; readers of the fixture can tell what is being tested without seeing the content.
- The AI Risk `coverage_note` is a small field with a disproportionate payoff. "This fixture covers the X attack class but does not cover the Y variant that uses tool-returned content" is the sentence that prevents the "fixture too narrow" anti-pattern. Write it with the broadening stage in mind.
- The derived variants should extend coverage in specific, named directions — delimiter attack, language variant, multi-turn variant, tool-chain variant. One variant is a start; three show the AI Risk broadening happened.
- Regression history is where the fixture earns its credibility. A fixture that "passes on the fixed code" is unverified; a fixture that "fails on the pre-fix commit" proves it catches the regression. If you cannot produce the fail-on-broken-code history for your real incident (because the broken code is gone), write a staging-replay path and note the limit.
- The judge-case rubric is specific and numbered. "Evaluate whether the response refuses the injected instruction *and* answers the user's original question *and* avoids out-of-scope tool calls" is three sub-questions, not one; the judge's calibration is against all three.
- The wiring diff is often the step teams skip. A fixture that is merged but not wired to a gate is a document, not a constraint. The diff is proof the wiring step actually happened.
- The loop-closed sign-off is the manager's artefact; write it in the voice of a manager signing, not a developer merging. The phrase "loop closed for inc-<id>" is the diagnostic signature.
- The adjacent-fixtures appendix is where the exercise prevents the overclaim. Loop closed *on this instance* is not loop closed *on the class*; naming the next targets is intellectual honesty.
- If you are tempted to inline the full fixture content from chapter 06's skeleton verbatim, resist. Re-write for your incident; the specifics matter.

## Acceptance criteria

- Policy is 0.5 page; all six stages, enforcement rules, ownership, and cadence present.
- Fixture YAML in registry-ready shape with ≥ 3 cases (1 baseline + ≥ 2 variants).
- Fixture YAML has `ai_risk_review` with coverage note, `redaction` block with policy and reviewer, specific-version pins throughout.
- Fixture YAML has `regression_history` with ≥ 1 pass on fixed code and ≥ 1 fail on pre-fix code.
- Judge-case YAML has a numbered rubric and a calibration block (or a placeholder with target date).
- Wiring diff shows the fixture added to a gate with **blocking** enforcement.
- Loop-closed sign-off statement is dated and specific; cites fixture path, gate tier, AI Risk sign-off date, and postmortem action item id.
- Loop-closed check-table maps all nine chapter-06 checkpoints to specific signals and locations.
- Appendix names ≥ 2 adjacent fixtures.
- A peer AI Engineering Manager can read the packet in under 10 minutes and verify loop-closed without opening the underlying evidence.

## Stretch goals

- **End-to-end tooling diagram.** Add a one-page diagram showing the data flow: evidence store → redaction tool → fixture scaffolder → registry → gate config → CI runner → regression history → dashboard → loop-closed register. The diagram is the mod-110-adjacent artefact that gets the tooling investment funded.
- **Loop-closed register.** Append a short spec for the loop-closed register the manager maintains — one row per incident, columns `incident_id`, `fixture_id`, `wired_at`, `signed_at`, `signed_by`, `coverage_note`. The register is the manager's roll-up view; teams that run it end up with a visible count of loop-closed incidents per quarter.
- **Fixture deletion policy.** Append a short section to the policy (artefact A) spelling out the fixture-deletion discipline: when a fixture is retired (surface deprecated, attack class no longer viable, superseded by a broader fixture), what the manager's sign-off looks like, how the successor relationship is recorded.
- **Historical conversion drill.** Pick an older incident from the team's history (or a public incident from chapter 08's corpora) and apply the loop to it retroactively — produce the fixture, judge case, wiring diff, loop-closed statement. The drill tests whether the loop is applicable only to fresh incidents or to the backlog as well.
- **Cross-team propagation.** For the fixture produced, identify any *other* surfaces in the team's portfolio where the same fixture (or a variant) should run. A direct-injection fixture on Agent Surface A may also apply to Agent Surface B; the propagation is a manager-level action the exercise can name explicitly.
- **Online-sample judge wiring.** If your incident shape is amenable to sampled-online evaluation (mod-104 chapter 06), spell out the online-sample wiring in addition to the pre-merge wiring. The dual-wiring is where the loop closes both pre-merge (regressions in new code) and post-merge (regressions from external changes — model snapshot, index refresh, classifier update).
