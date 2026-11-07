# Exercise 04: Regulator-Facing Incident Summary Draft

**Estimated effort:** 2 hours · **Output:** (a) a draft **engineering-side incident summary** for a hypothetical Article 73-adjacent incident on the product you used in exercises 01–03, and (b) a short **delineation memo** that names, item by item, what counsel owns in the filing vs. what engineering owns in the summary, plus a trigger-list for when to page counsel during an incident.

## Objective

Produce the two artefacts chapter 07 commissions. The summary is the engineering-side facts counsel's regulatory filing reads off; the delineation memo is the shared understanding between the AI engineering manager and counsel about who writes what. Together, they are the posture that prevents the three failure shapes chapter 07 names — regulator-reads-it-first, filing-contradicts-the-evidence, security-incident-run-as-engineering.

This exercise does **not** ask you to draft a legal filing. The exercise asks you to draft the engineering summary that makes a correct filing possible, and to articulate the split with counsel that keeps you from accidentally writing the filing yourself.

A peer AI Engineering Manager (or counsel partner), reading your artefacts, should be able to:

- Read the engineering summary and recover the complete factual picture of the incident, including scope, awareness timeline, causal analysis, mitigation, and remediation, without opening any underlying evidence.
- Read the delineation memo and know, for each component of a plausible regulatory filing, whether the AI engineering manager or counsel is on the hook.

## Prerequisites

- Chapter 07 — the partnership with counsel; EU AI Act Article 73 at EM depth; the ten-section engineering-summary artefact; recognising a security-incident shape early; the hand-off; the timing, drafting, and sign-off discipline; the four anti-patterns.
- Chapter 04 — evidence preservation and the chapter-04 escalation thresholds that trigger counsel involvement.
- Exercises 02 and 03 — the postmortem and the fixture packet that this summary reads off.
- EU AI Act Regulation (EU) 2024/1689, especially Articles 3(49), 70, 72, 73, and 50 as context — linked in `resources.md`. Read the current text; the regulation and its implementing acts evolve.
- GDPR Article 33 as the adjacent-regime reference point.
- Where applicable, the sectoral-regulator reading your organisation operates under (FDA, SEC, EEOC, HHS, EU DPA, UK ICO, etc.).

## Problem statement

Two artefacts, in markdown, filed where governance / counsel-shared artefacts live.

### Artefact A — the engineering-side incident summary (2–3 pages, 75 minutes)

Pick (or escalate) the incident from exercises 02–03 to an **Article 73-adjacent severity**. If the exercise-02 incident was a lower severity, promote it: a tier-1 (HIGH) deployment serving EU users, where the misbehaviour affected a protected population or produced a scaled regulated-category output. The point is to produce a summary that counsel would read with the Article 73 question in mind.

Fill the ten-section engineering summary from chapter 07 core concept 3:

1. **Identification** — product, surface, markets, provider vs. deployer role (note which the organisation plays; counsel confirms), severity tier.
2. **The incident, factually** — what, when, where, to whom, what outputs. Factual; no causal language.
3. **Awareness timeline** — the three sub-timestamps: first detectable moment, first human awareness, serious-incident classification (or "not yet classified"). Specific to the minute (UTC).
4. **Scope of impact** — affected sessions, geographies, user segments (including vulnerable-population flags), duration, concrete harms traceable to the output (confirmed, under investigation, or none identified — be specific).
5. **Technical causal analysis** — the chain from input to output, the guardrails that failed or were absent, specific versions (model, prompt, judge, dataset, retrieval index, classifier, tool schema) at incident time. Accessible language — assume counsel can read engineering English but does not want jargon.
6. **Mitigation** — what, when, with what effect. Reference the chapter-03 levers explicitly.
7. **Remediation** — the chapter-05 postmortem's action items re-written at counsel's altitude. Include the loop-closed status (exercise 03).
8. **Supporting evidence** — pointers to the preserved-evidence store, the postmortem, the fixture, the judge case, the dashboards.
9. **Organisational contacts** — manager, counsel, security operations (if security incident), DPO (if GDPR-adjacent).
10. **Review and sign-off** — manager factual accuracy; counsel legal adequacy (placeholder for counsel to fill); dates.

Guidance on honesty: numbers match the evidence; timeline matches the Scribe's record; mitigation timestamps match the deploy log. If the TTM was 22 minutes, the summary says 22 minutes — not "within a half hour." If scope is estimated, the method is named (chapter 07 skeleton's section 4 has an example). The summary is the counsel's single source of truth for facts.

Length target: 2–3 pages. Shorter means you have skipped a section; longer means you have inlined analysis that belongs in the postmortem.

### Artefact B — the delineation memo (0.75 page, 30 minutes)

A short shared document between the AI Engineering Manager and counsel. Sections:

1. **What counsel owns.** A specific list (a dozen items; chapter 07 core concept 1 is the source), each in the form "counsel decides / counsel authors / counsel signs." Examples: whether the system is high-risk under Annex III; whether the event meets Article 73's serious-incident threshold; which national competent authority is notified; the timing of the filing; the language of the filing; the applicability of GDPR Article 33; the applicability of sectoral or state-level AI regulation; the content of customer-facing disclosure; the privileged / non-privileged status of internal documents; the engagement strategy with regulators and law enforcement.
2. **What engineering owns.** A matching specific list — the facts, the timeline, the versions, the mitigations, the impact assessment, the remediation plan, the evidence pointers, the security-incident shape recognition, the on-call escalation to counsel, the preservation of evidence to Article-73 retention, the technical accuracy of the summary.
3. **The trigger-list for paging counsel.** The chapter 04 core concept 5 escalation thresholds, written as unambiguous triggers: concrete user harm; prohibited-category output; data breach; scaled misbehaviour on a regulated surface; attack-rather-than-misbehaviour shape; regulator or significant press inquiry. Each with a one-line "what counsel does when paged" (confirm, assess, enter the room, make the applicability determination).
4. **The timing discipline.** Three timing rules from chapter 07 core concept 7: the draft begins during the incident; counsel sees drafts not final; the clock is counsel's to run. One line each.
5. **The hand-off to security operations.** The specific signals (chapter 07 core concept 5) that re-cast the incident as a security incident, and the specific steps when the hand-off fires — security operations becomes IC; chain-of-custody discipline tightens; disclosure controls apply; counsel is co-involved from day zero.
6. **Sign-off.** Manager + counsel signature block, dated.

The delineation memo is the shared agreement. Counsel reads and marks it up; the manager adjusts; the memo is pinned to the organisation's incident-governance pages and reviewed annually.

### Remaining time (15 minutes)

In a short appendix to artefact A, name at least two **specific things you did not put in the summary** because they belong in the postmortem (chapter 05) or in internal technical analysis — and one **specific thing you did not put in the summary** because counsel should decide whether and how to disclose it. The appendix is the manager's discipline against the "engineer drafts the legal filing" anti-pattern.

## Requirements

- Summary is 2–3 pages; all ten sections from chapter 07 core concept 3 present.
- Section 3 (awareness timeline) has three specific UTC timestamps; the timeline supports counsel running the 15-day Article 73 clock.
- Section 4 (scope) is specific — numbers, method, geographies, segments (including vulnerable-population flag), concrete harms traceable.
- Section 5 (causal analysis) names specific versions (model / prompt / judge / dataset / retrieval index / classifier / tool schema) and uses accessible language.
- Section 6 (mitigation) references the chapter-03 levers explicitly.
- Section 7 (remediation) references the fixture packet from exercise 03 and names the loop-closed status.
- Section 8 (supporting evidence) points at specific stores / documents / dashboards.
- Section 9 (contacts) names manager + counsel + security + DPO (or justifies absence).
- Section 10 (sign-off) has manager factual accuracy signed; counsel legal adequacy placeholder.
- Delineation memo is 0.75 page; all six sub-sections present.
- Trigger-list for paging counsel has ≥ 5 named triggers.
- Timing discipline covers the three rules.
- Hand-off section names security-incident signals and the specific hand-off steps.
- Appendix to artefact A names ≥ 2 things withheld (belongs-in-postmortem) + ≥ 1 thing withheld (counsel-decides disclosure).

## Starter guidance

- The hardest discipline is honesty. The temptation to narrate causes in section 2 is large; resist it. Section 2 is facts; section 5 is causes. If section 2 says "the agent produced X" and section 5 says "the agent produced X because Y," the structure is right. If section 2 says "the agent, having been manipulated by Y, produced X," section 5 has already started in section 2.
- Section 3 is often the field counsel cares about most. The Article 73 clock begins at awareness; the organisation's position is as strong as the summary's clarity about *when* awareness occurred. The three sub-timestamps are specific: when could the incident have first been detected (not when it was); when was the first human aware (not when the PagerDuty acked — a human somewhere at a customer may have been aware earlier via a complaint); when was the serious-incident classification made.
- For the scope section, use a method note. "Affected sessions estimated from production traces filtered on the injection signature between T1 and T2; see appendix" is concrete; "approximately 150 sessions" is not. Counsel cannot defend a number whose derivation is unknown.
- In the causal analysis, "versions at incident time" is non-negotiable. "Latest" is not a version; pin to the registry IDs. The version pins are also the hook the regulator uses in co-operation: "please provide the model snapshot ID for the affected sessions" has a specific answer.
- The remediation section is where the exercise rewards the loop-closed packet from exercise 03. "Fixture `<registry path>` in registry, wired to pre-merge gate, AI Risk sign-off on <date>; loop closed on <date>" is a sentence counsel can defend; "we added a test" is not.
- Section 8 (supporting evidence) pointers must survive the regulatory engagement. If the preserved-evidence store rotates in 90 days and the regulator engagement runs 18 months, the pointer is going to break. Note the retention adjustment the incident triggered (chapter 07 chapter-04 extension).
- Section 10's manager sign-off is explicit: "I attest the factual content of this summary matches the preserved evidence and the Scribe's record for inc-<id>." The attestation is specific; a signature without the phrase is weaker than a signature with it.
- The delineation memo is a political artefact as much as a technical one. Share it with counsel before using it; the shared ownership is the point. If your organisation has no counsel partner yet, write the memo as the request — "this is the division of responsibility we propose; counsel, please confirm or adjust."
- The trigger-list is where the exercise saves you in a real incident. Each trigger should be the sort of thing an on-call engineer at 03:14 can read and say "yes, that is this incident; I am paging counsel."
- The appendix on what-was-withheld is the discipline against drafting the legal filing. Items an engineer might be tempted to include but should not: the organisation's historical response to similar issues; the specific language the regulator should use; the opinion about whether the event is or is not a serious incident; the comparison to peer organisations' incidents (counsel may or may not want these, but the choice is counsel's).
- For the Article 73-adjacency, avoid asserting determinations the manager cannot make. "This incident may meet the Article 73 serious-incident threshold; counsel is asked to assess" is correct. "This is an Article 73 serious incident" is a legal determination; "this is not an Article 73 serious incident" is the same legal determination with the opposite sign.

## Acceptance criteria

- Summary is 2–3 pages; all ten sections present; honest against the exercise-02 incident and its exercise-03 loop.
- Awareness timeline has three specific UTC timestamps; serious-incident classification is named (or marked as counsel-pending).
- Scope section names method for any estimated numbers.
- Causal analysis names specific versions.
- Remediation section references the exercise-03 fixture and the loop-closed date.
- Supporting-evidence pointers include retention notes.
- Sign-off attestation language is specific ("I attest the factual content ... matches the preserved evidence ...").
- Delineation memo is 0.75 page; all six sub-sections present.
- Trigger-list for paging counsel has ≥ 5 named triggers.
- Appendix names ≥ 2 withheld-for-postmortem items + ≥ 1 withheld-for-counsel-disclosure item.
- A peer AI Engineering Manager (or counsel partner) can read the summary in 5 minutes and recover the complete factual picture; can read the delineation memo in 2 minutes and know the ownership split.

## Stretch goals

- **Counsel markup pass.** If you have access to a counsel partner (even a simulated one — a colleague role-playing), run the delineation memo past them and capture the markup. The real-world value of the exercise jumps when the memo has been reviewed; the value of the exercise is in the exchange.
- **Article 73 trigger table.** Append to the delineation memo a specific trigger table: for each of the ten chapter-02 failure modes, name the harm-shape under which the mode could be Article 73-adjacent. The table is the manager's cheat sheet during an incident. Counsel reviews and signs.
- **GDPR Article 33 variant.** Produce a shorter variant of the engineering summary for a pure GDPR Article 33 incident (personal-data breach, 72-hour window) without the Article 73 framing. The variant tightens scope, timeline, and data-subject impact; it does not need the Article 73 remediation section.
- **Security-incident variant.** Produce a security-incident-shape variant of the engineering summary — the chapter 07 core concept 5 case. The variant's hand-off to security operations is explicit; the chain-of-custody discipline is named; counsel is co-involved from day zero. The variant tightens how the AI engineering manager steps back from IC when the shape re-casts.
- **Historical replay.** Pick a published AI incident from the AIID that plausibly triggered a regulatory response (per the OECD.AI Incidents Monitor — chapter 08). Draft the engineering summary the responsible provider's manager *could have* produced. Acknowledge the limits of public data. The replay tests whether your summary template is usable from the outside.
- **Retention-and-cooperation playbook.** Append a short playbook for the evidence-retention and regulator-cooperation side: how the preserved-evidence store's retention is extended for an Article 73 engagement, how co-operation requests are handled operationally, what the manager does when the authority requests a demo or an interview. The playbook is adjacent to mod-111 (cross-functional partnership) and is where the manager and counsel agree on the operational mechanics, not just the legal theory.
