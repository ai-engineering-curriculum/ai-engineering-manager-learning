# Exercise 02: Blameless Postmortem Template with AI-Specific Fields

**Estimated effort:** 2 hours · **Output:** (a) a reusable blameless-postmortem template with explicit AI-specific fields, filled with inline guidance for each section, and (b) a filled example of the template against one real (or realistic) incident on the product you used in exercise 01.

## Objective

Produce the two artefacts chapter 05 commissions. The template is the shape your team runs postmortems against; the filled example is the proof that the template covers the AI surface and does not degenerate into a generic SRE template with a sprinkling of AI words. A peer AI Engineering Manager, reading your filled example, should be able to read the AI-specific-fields section, name the exact model / prompt / judge / dataset versions at incident time, map the incident to a chapter 02 failure mode, and see the fixture action item that closes the chapter 06 loop.

## Prerequisites

- Chapter 05 — the eleven-section postmortem template, the blamelessness practice, action-item maturity, the review cadence, the facilitation discipline, the five anti-patterns.
- Chapter 02 for the failure-mode classification and chapter 06 for the loop the postmortem action items will reference.
- Exercise 01 — the runbook pages the postmortem reads from.
- Allspaw's "Blameless Post-Mortems and a Just Culture" (Etsy, 2012), and the Google SRE Book's chapter on postmortem culture, as the primary sources for the discipline the template codifies. Linked in `resources.md`.
- The eval registry schema (mod-104 chapter 04) — the fixture metadata that section 6 references.

## Problem statement

Two artefacts, in markdown, filed where incident artefacts live.

### Artefact A — the postmortem template (1.5 pages + inline guidance, 60 minutes)

A reusable template a facilitator fills during / after each incident. The template carries section headers, placeholder values, and **inline guidance** (as HTML-style comments, or in a sidebar) that tell the filler what counts as complete, what to avoid, and when to escalate. The template is adapted from the chapter 05 eleven-section shape:

1. **Summary** — 2–4 sentences; readable by someone who has never heard of the incident. Inline guidance: "No jargon the reader will not have; one sentence on impact; one on cause; one on mitigation; one optional on follow-up."
2. **Incident facts** — id, severity, timestamps (open / mitigated / closed, UTC), TTD / TTM / MTTR, scope of impact, **harm classification** (concrete harm y/n and type; regulated-category output y/n and which; data breach y/n and scope; security-incident shape y/n). Inline guidance on each harm sub-field — specifically what triggers a "yes."
3. **Timeline** — the Scribe's record, curated. Inline guidance: timestamps UTC, every state change / decision / mitigation / escalation / external communication, terse attribution, no analysis.
4. **What went wrong (contributing causes)** — a list, system-level framing. Inline guidance: "the question to repeat is 'what about the system made this mistake easy to make / hard to detect / hard to prevent'; list, not single-cause."
5. **What went right** — specific mitigations, monitoring catches, IC decisions that shortened response. Inline guidance: do not skip this section.
6. **AI-specific fields** — the adaptation:
    - **Model, prompt, judge, dataset, retrieval-index, classifier, tool-schema versions** at incident time. Inline guidance: all pinned to specific IDs from the registries; "latest" is not a version.
    - **Recent changes** in the window leading to the incident. Inline guidance: scan the deploy log for the 24h preceding incident open; include ingest jobs, index rebuilds, threshold changes.
    - **Failure-mode classification** — which of the ten chapter 02 modes (or product-specific); link the catalogue entry. Inline guidance: if it does not map, flag for the yearly catalogue refresh (chapter 08).
    - **Guardrail-stack behaviour** — which classifiers / detectors fired, which did not, which were absent; score distributions where available. Inline guidance: cite dashboard snapshots in the appendix.
    - **Trace / evidence location** — pointer to the preserved-evidence store (chapter 04). Inline guidance: the pointer must survive the log-retention window.
    - **Red-team cross-reference** — a known attack class from the AI Risk / red-team corpus (mod-106 chapter 04)? If yes, how did the finding not yield a preventive control? If no, add to red-team backlog.
    - **Harm classification** (duplicated from section 2 for emphasis). Inline guidance: the field triggers chapter 07 regulator-narrative consideration; counsel owns the legal determination.
    - **Regulator-adjacency status** — Article 73 candidate? Article 33? Sectoral? Contractual? Inline guidance: the field is a status, not a legal opinion; counsel determines.
7. **Action items** — table with columns `id`, `type` (mitigation / preventive / detection / response / fixture / red-team), `description`, `owner` (named individual), `target`, `signal` (completion criterion). Inline guidance: 3–7 items; every incident has at least one **fixture** item (chapter 06); no "someone should" items.
8. **What we almost missed (near-misses)** — specific. Inline guidance: what nearly went worse, what the lucky save was.
9. **Hindsight-bias check** — a paragraph the facilitator writes. Inline guidance: specifically "which of our 'what went wrong' items look obvious *now* that was *not* obvious *then*?"
10. **Review and sign-off** — facilitator, participants (named), manager sign-off, date of next review (loop-closed milestone for chapter 06).
11. **Appendix** — evidence pointers, related incidents, action-item tickets.

The template is reusable — rename the incident, swap the IDs, swap the versions, and it still works. Inline guidance is written so a less-experienced facilitator on a different team could follow it.

### Artefact B — the filled example (1–2 pages, 45 minutes)

Pick one real (or realistic) incident on the product from exercise 01. Fill the template end-to-end. Guidance:

- Use real values where you have them; use `[placeholder: …]` with a specific action and date where you do not. The filled example is primarily a proof the template works; keep it tight.
- At least one AI-specific field under section 6 should surface something that would be invisible in a generic postmortem — the specific model snapshot, the prompt-registry version, the judge-case version that fired, the retrieval-index version. If section 6 does not surface a concrete AI-only detail, the template failed at this exercise.
- The failure-mode classification links a specific chapter 02 entry (or declares a product-specific mode and flags the catalogue refresh).
- Section 7 has at least one `fixture` action item that names the registry path where the fixture will land (readies exercise 03).
- Section 9 (hindsight-bias) must actually include a hindsight-bias observation; "no hindsight bias identified" is almost never correct and is almost always a fill-skip.
- Harm and regulator-adjacency are honest — if the incident did not reach the Article 73 threshold, say so and say why; if counsel was not involved, note the reason.
- The appendix points at the preserved evidence and the related-incident links; use plausible placeholders where you do not have specific incidents to cite.

### Remaining time (15 minutes)

Write a short **"template changelog"** at the bottom of artefact A — the two or three fields you added or tightened as a result of filling artefact B. The changelog is the discipline that turns the exercise into a system; filling the template once reveals the gaps that the next filler should not face.

## Requirements

- Template covers all eleven sections; inline guidance is present for each.
- Section 6 (AI-specific fields) has the eight sub-fields enumerated above, each with inline guidance.
- Template's action-item table carries the six item types (mitigation / preventive / detection / response / fixture / red-team).
- Template's review-and-sign-off section names the manager sign-off and the loop-closed-milestone next-review date.
- Filled example maps to a chapter 02 failure mode (or declares a product-specific mode and flags the catalogue refresh).
- Filled example's section 6 names specific versions (model / prompt / judge / dataset / retrieval index / classifier / tool schema) at incident time; no "latest."
- Filled example has ≥ 1 `fixture` action item with a specific registry path (readies exercise 03).
- Filled example has honest harm and regulator-adjacency classifications; counsel involvement (or non-involvement) is justified.
- Filled example includes a hindsight-bias observation; "none identified" is not acceptable unless specifically explained.
- Appendix points at a preserved-evidence store location, a related-incident list, and the action-item tracker.
- Template changelog reflects ≥ 2 adjustments made from filling the example.

## Starter guidance

- The template's inline guidance is the artefact's long-term value. A facilitator who inherits your template a year from now has nothing except the template; the guidance is how you talk to them from the past. Write it in the voice of a colleague, not a style guide.
- Section 6 (AI-specific fields) is the section the exercise lives on. If you find yourself inlining chapter 05's text verbatim, re-write in your own words and tighten for your product's surface. A medical-adjacent surface's section 6 will have different emphasis than a developer tool's.
- The failure-mode classification is the link between chapter 02 and chapter 06. Get the classification right; the fixture it points toward is only as useful as the classification's accuracy.
- Resist a one-section "root cause" framing. The chapter 05 discipline is multi-causal; the template reflects that; facilitation discipline enforces it.
- Harm classification is where the template earns its keep. Inline guidance should say, specifically, "concrete user harm is traceable to the output — financial, medical, physical, significant privacy harm — not just 'users were annoyed.'" The specificity is the protection against a blurry filling.
- Regulator-adjacency is a status, not a legal opinion. Inline guidance should say, specifically, "the field is for engineering to flag that counsel should look; counsel determines Article 73 / Article 33 applicability." The field itself is a flag, not a decision.
- The hindsight-bias section is often skipped. Inline guidance should make it uncomfortable to skip — "'none identified' is a red flag; the facilitator writes the observation even if the observation is 'the retrospectively-obvious signal was explicitly not obvious at 13:35 because ...'"
- Action-item typing (mitigation / preventive / detection / response / fixture / red-team) is where the quarterly aggregation of chapter 05 core concept 5 earns its insight; include the type as a required column.
- For the filled example, pick an incident that has a *fixture* story — a failure the team could have caught with a regression test but did not. The exercise then flows naturally into exercise 03.
- The template changelog is the discipline that keeps the template alive. A template that never changes is one whose next facilitator will meet the same gaps you did.

## Acceptance criteria

- Template is 1.5 pages + inline guidance; all eleven sections present with guidance for each.
- Section 6 (AI-specific fields) has eight sub-fields with inline guidance.
- Action-item table template specifies the six action-item types.
- Filled example is 1–2 pages; maps to a chapter 02 failure mode or declares a product-specific mode.
- Filled example's section 6 names specific versions (model / prompt / judge / dataset / retrieval index / classifier / tool schema).
- Filled example has ≥ 1 `fixture` action item pointing at a specific eval-registry path.
- Filled example has honest harm and regulator-adjacency classifications with reasoning.
- Filled example includes a substantive hindsight-bias observation.
- Appendix in filled example points at preserved evidence, related incidents, action-item tracker.
- Template changelog reflects ≥ 2 adjustments made from filling the example.
- A peer AI Engineering Manager can read the filled example and recover the complete factual picture of the incident, including the AI-specific detail, in under 5 minutes.

## Stretch goals

- **Facilitator script.** Append a half-page facilitator script to the template: pre-meeting prep checklist (24h before), opening remarks (2 minutes), section-by-section timing (what to spend on each), closing remarks (loop-closed commitment). A new facilitator should be able to run a 90-minute postmortem from the script.
- **Blamelessness enforcement.** Add a half-page "blameless discipline" page referenced from the template: the three interventions from chapter 05 (reframing blame-language, protecting the first speaker, naming hindsight bias), with example phrasings the facilitator uses when the room drifts.
- **Register schema.** Append a tiny schema / JSON spec for the action-item register — the fields every postmortem's action items populate, and the status transitions (open → in-progress → blocked → closed). This is what chapter 05 core concept 5's weekly chase operates on.
- **Severity-specific variants.** Produce short variants of the template for SEV3 and SEV4 (the lighter / tracked-only tiers), reusing the core sections but omitting what is not warranted at those tiers. Document the omissions.
- **Historical replay.** Pick a published AI incident from the AIID (chapter 08) and fill the template against that incident (acknowledging the limits of public reporting). The replay exercises whether your template is usable from the outside and sharpens the AI-specific-fields section.
- **Cross-reference to exercise 03.** The `fixture` action item in your filled example should name the specific fixture registry path that exercise 03's artefact will produce. Explicit continuity across the two exercises is the pedagogical reason they are side by side.
