# Exercise 01: Guardrails As Product Spec For One Feature

**Estimated effort:** 3 hours · **Output:** (a) a 3–6 page guardrails spec for one feature covering the six sections from chapter 02, and (b) a one-page OWASP LLM Top 10 × NIST GenAI Profile coverage matrix cross-referencing the spec.

## Objective

Produce the two artefacts chapter 02 commissions — for **one real feature** you ship or plan to ship:

1. A **guardrails product spec** that an applied-ai-engineer IC could pick up and build against, covering the six sections (surface and risk model, input filters, output validators, tool-invocation policy, HITL checkpoints, refusal policy).
2. A **coverage matrix** that maps each in-scope OWASP LLM Top 10 category and each in-scope NIST Generative AI Profile risk to the specific section (and line) of the spec that mitigates it, or declares it out-of-scope with a one-line rationale.

A peer AI Engineering Manager, on reading both, should be able to either (a) ask three sharp questions about the mitigation strategy, or (b) sign the spec as the pre-engineering gate and route it to an IC for build.

## Prerequisites

- Chapter 01 (`The Manager's Altitude for Safety`) for the manager / IC split and the four manager-owned artefacts.
- Chapter 02 (`Guardrails as Product Spec`) for the six-section shape and the two frames (OWASP, NIST) the spec cites.
- Chapter 03 (`The Guardrails Tech Landscape at EM Depth`) at a skim — the spec picks classifiers from this landscape; you do not need to tune them, but you do need to name them.
- A feature you know enough about to write the spec honestly. Three usable patterns:
  - A feature your team currently ships or is about to ship.
  - A feature at a prior employer with numbers abstracted where needed.
  - A hypothetical feature on a composite team — in which case write the feature's product charter first and mark the parts you are inventing with `[placeholder: …]`.

## Problem statement

Two artefacts, written in markdown, filed in a location your team's governance surface would accept (co-located with the eval registry and prompt registry from mod-104 chapters 02 and 04).

### Artefact A — the guardrails product spec (3–6 pages, 120 minutes)

The spec carries all six sections from chapter 02 core concept 2, with decisions on each. The level of specificity target: your `applied-ai-engineer` IC should be able to open the spec, pick each classifier / validator / rule, and start implementing without asking you a second round of clarifying questions. Placeholders are allowed where the eval evidence is not yet available (point to a commissioning brief with a date), but **not** allowed on the product decisions.

Minimum content per section:

1. **Surface and risk model** — one paragraph on the feature; a table mapping each in-scope harm (minimum five rows) against the OWASP / NIST / product-specific taxonomies with one-sentence instantiation and one-line mitigation summary per row; a declared severity tier (LOW / MEDIUM / HIGH) with one-sentence justification.
2. **Input filters** — named classifier (with version), categories moderated, thresholds per category with one-line rationale, prompt-injection detection posture (if retrieval or tool input brings untrusted content), explicit behaviour on classifier failure / timeout, logging contract.
3. **Output validators** — structured-output schema if applicable (name the schema, not just "JSON"); content-safety classifier on output; PII / secrets redaction posture; grounding / citation check (if retrieval-grounded); surface-specific factuality thresholds.
4. **Tool-invocation policy** — the subset of the tool catalogue this feature has access to; pre-invocation checks per tool; HITL rules per tool with one-sentence reason; post-invocation treatment of tool output; loop limits and cost caps.
5. **HITL checkpoints** — named checkpoints (minimum one), trigger (concrete classifier score / category / audience / tool category), UX pattern, SLA, reviewer role named (not "a human"), reviewer tooling view, audit-log schema.
6. **Refusal policy** — taxonomy of refusals (minimum four of the five types from chapter 02); actual user-facing wording per type; appeals channel and triage SLA; observability contract (which dashboard reads refusal rate).

Append a one-paragraph **spec hygiene** block: who owns it (manager + IC lead), when it was last reviewed, when the next review is, which eval-registry entries verify it.

### Artefact B — the coverage matrix (1 page, 45 minutes)

A table with one row per in-scope OWASP LLM Top 10 category **and** one row per in-scope NIST GenAI Profile risk category (dedupe where categories overlap; name both IDs in the first column if so). Columns:

- Category (OWASP ID + NIST category).
- In-scope for this feature? (yes / no / partial, with one-line rationale).
- Mitigating section of the spec (section number + the specific rule / line).
- Residual risk (accepted-risk summary; one line; empty if fully mitigated).
- Verification (which eval case, red-team category, or runtime monitor demonstrates the mitigation; may reference a commissioning placeholder).

Rows for which "in-scope?" is "no" still carry a one-line rationale. Blank cells are a spec bug; fix them before the review.

### Remaining time (15 minutes)

Run the chapter 02 core concept 3 review checklist against both artefacts. Fix anything the checklist surfaces. A well-formed spec usually produces two or three fixes on this pass.

## Requirements

- The spec carries all six sections, each with concrete decisions (not "TBD"; `[placeholder: …]` with an action and date is acceptable; blank is not).
- Minimum five harm rows in section 1's risk-model table; the severity tier is justified in one sentence.
- Every classifier named in sections 2 and 3 has an explicit threshold and an explicit error-on-failure behaviour.
- Every tool named in section 4 has an explicit HITL rule ("requires confirmation" or "no confirmation needed because <reason>").
- HITL reviewer role is **named by role**, not "a human."
- Refusal wording for each refusal type is written verbatim, not promised.
- The coverage matrix cites both the OWASP ID and the NIST category for every row; mitigating-section references point to specific sections / rules (not "section 2").
- The matrix's residual-risk cells for accepted risks are dated and signed where present.
- Artefact A cross-references specific verification evidence (eval-registry run id, red-team category, dashboard id) in each section's hygiene block.

## Starter guidance

- Start with section 1. The risk-model table is the backbone of everything else; a weak table produces a weak spec. Spend 20 minutes here.
- Write the severity tier before sections 2–6. A LOW-severity feature with a 0.9 toxicity-blocking threshold is a tell that the tier was retrofitted; the review catches this.
- For section 4, if the feature does not call tools, write that explicitly ("no tool-invocation on this surface; this section intentionally empty") rather than skipping it. A later version of the feature might add tools; the empty section is the reviewer's checkpoint when that happens.
- For section 5, resist naming "customer support" as the reviewer for anything safety-critical. If the feature is HIGH severity, the reviewer role may have a credentialing requirement; name it.
- For section 6, write the refusal wording out loud. Read each one and ask: "Does this tell an attacker which classifier fired?" and "Does this leave the user with an actionable next step?" The two rules from chapter 02 anti-patterns apply.
- Pin the OWASP LLM Top 10 version in the coverage matrix (versions move; a 2025-dated spec that cites the 2023 Top 10 is a tell). Similarly, pin the NIST GenAI Profile version (AI 600-1, specific publication date).
- For retrieval-grounded features, section 3's grounding check and section 6's grounding refusal are commonly under-specified. Spend an extra ten minutes on the pair.
- The coverage matrix is meant to be readable in three minutes. Keep it tight; residual-risk cells are often one phrase, not a paragraph.
- Do not invent classifier behaviours. If you do not know whether your chosen classifier returns an `error` or a score of 0 when it is down, mark `[placeholder: verify timeout behaviour with IC]` and move on.

## Acceptance criteria

- The spec is 3–6 pages; all six sections present; every section has concrete decisions.
- Section 1's risk-model table has ≥ 5 rows; each row cites a frame (OWASP / NIST / product-specific).
- Severity tier is named and justified.
- Classifiers in sections 2 and 3 have thresholds, failure behaviour, and logging contracts named.
- Tools in section 4 have HITL rules named.
- HITL checkpoints in section 5 have role, trigger, UX, SLA, and reviewer-view named.
- Refusal wording in section 6 is verbatim (not "something polite will go here").
- The coverage matrix carries ≥ 10 rows (every OWASP LLM Top 10 category + any NIST GenAI Profile risks not covered by overlap).
- No blank cells in the matrix.
- A peer AI Engineering Manager can read both in 10 minutes and either sign or ask three sharp questions.

## Stretch goals

- **Pair with a mock red-team brief.** Write a half-page preview of the exercise-02 brief for the same feature; where the brief's categories exceed the spec's coverage, mark the gaps as follow-up items on the spec's hygiene block.
- **Automate the coverage check.** Write a one-page description of how the coverage matrix could become a lint on the spec (a CI job that fails if an OWASP ID has no cross-reference in the spec, or if an eval-registry id in the matrix does not resolve). You are not implementing it; you are specifying it for an IC.
- **Two-surface diff.** Write the spec for a second surface of the same product (one more interactive, one more batch, for example) and produce a one-page **diff** of the two specs — which decisions are shared; which are different; which are different only because of the severity tier. The diff is the input to a shared base-spec template.
- **Deprecation path.** Add a one-paragraph section at the end of the spec describing what triggers a re-review of each section (new classifier release, new attack class published in OWASP update, new tool added in chapter 07's review, severity-tier change, substantive model-generation migration). This is the hygiene the mod-105 chapter 07 cadence reads.
