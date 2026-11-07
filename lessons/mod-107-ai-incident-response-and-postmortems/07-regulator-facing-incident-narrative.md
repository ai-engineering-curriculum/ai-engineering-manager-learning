# Regulator-Facing Incident Narrative

Most incidents the team handles are internal: an eval regression, a jailbreak caught in production, a tool-use misfire. Some fraction of incidents cross a different kind of threshold — one where a regulator, a data-protection authority, or a national competent authority has a plausible interest in knowing what happened. For AI engineering managers in Europe or selling into European markets, the most explicit version of this threshold is **EU AI Act Article 73**, the serious-incident reporting obligation for high-risk AI systems. Equivalent obligations exist under GDPR Article 33 (data-breach notification), under product-specific regimes (medical devices, financial services, employment / housing / credit discrimination), and under emerging regimes in other jurisdictions (US state-level AI laws, UK AI regulation).

This chapter is the manager's posture across all of those. The partnership with counsel; the engineering-side artefact counsel's filing reads off; the delineation between facts (engineering owns) and legal argument (counsel owns); the hand-off to security operations / DFIR when the incident is a security incident. The AI engineering manager does not author legal filings. The manager writes the engineering summary that makes a correct filing possible, and routes the incident to counsel at the right moment.

A grounding note. Specific legal obligations change. The current text of EU AI Act Article 73 as of the publication of Regulation (EU) 2024/1689 defines the obligation and the Commission is issuing implementing acts on the reporting template. Any specific implementation detail in this chapter should be verified against counsel's reading of the current text and the current implementing acts at the time of a real incident. The structural posture — partner with counsel, write the engineering narrative, route early — does not change; the specific thresholds and forms will.

## Motivation — three incident-and-regulator shapes that failed

- **The regulator read about it first.** A customer screenshot of an AI agent recommending unsafe behaviour to a minor circulated on social media. The engineering team saw it, mitigated the feature, drafted an internal note. Six days later the Irish DPC reached out about an EU AI Act Article 73-adjacent inquiry because the feature was deployed in a candidate high-risk configuration. Counsel discovered the incident at the same time as the regulator. The 15-day window for serious-incident notification had expired; the organisation's filing was already out of compliance before it was drafted. The failure was not the incident; the failure was the routing.
- **The filing contradicted the evidence.** The engineering team gave counsel a one-page summary; counsel wrote the filing from the summary. A week later, during the regulator's follow-up, the raw traces were requested — and the raw traces did not match the summary. The team had rounded TTM downward, had described the mitigation as "immediate" when it was 22 minutes in, and had underreported the scope by a factor of two. The filing was correct against the summary and wrong against the evidence. Nobody intended to mislead; the engineering-side narrative was written with the audience in mind rather than the evidence.
- **The hand-off to security never happened.** An embedding-store modification was investigated as a data-integrity failure; three weeks later a disgruntled-contractor pattern surfaced in access logs. By then the chain-of-custody on the store artefacts was broken, the attacker-attribution window had closed, and the organisation's position in any subsequent legal action was weaker than it would have been with a proper DFIR engagement from day zero. The engineering team ran the incident the way they knew how; the security-incident shape was never explicitly recognised.

All three are routing failures the manager owns the prevention of.

## Core concept 1 — the partnership with counsel

Counsel reads the regulation and determines whether the law applies. Engineering produces the artefacts counsel's reading needs to be true. Neither owns both.

### What counsel owns

- Whether the deployment is a "high-risk AI system" under EU AI Act Annex III.
- Whether the specific event meets the "serious incident" threshold under Article 3(49) as it applies to Article 73.
- Which national competent authority is notified under Article 70; whether other authorities (DPO's, DPAs, sectoral regulators) must also be notified.
- The timing of the filing (Article 73 specifies "immediately after the provider has established a causal link between the AI system and the serious incident, and in any event no later than 15 days after the provider or, where applicable, the deployer becomes aware of the serious incident" — exact phrasing per the regulation; counsel's reading controls).
- The language of the filing; any carve-outs for legal privilege; the disclosure strategy to other stakeholders (customers, data subjects, press).
- Analogous determinations for GDPR Article 33 (personal-data breach, 72 hours), sectoral regulators (FDA for medical devices, SEC for financial systems, EEOC for employment, HHS for HIPAA), and any contractual notification obligations in customer DPAs.

### What engineering owns

- The **facts**: what happened, when, on which surface, to how many users, with what outputs, under what conditions.
- The **timeline**: the Scribe's record (chapter 04), reconstructed and verified.
- The **versions**: model, prompt, judge, dataset, retrieval index, classifier, tool schema.
- The **mitigations**: what was done, when, with what effect.
- The **impact assessment** at the engineering altitude: scope, failure mode, whether a user suffered concrete harm traceable to the output, whether data was disclosed outside its intended scope.
- The **remediation plan**: the fix, the loop-closed status (chapter 06), the preventive work in flight.
- The **evidence pointers**: where the preserved traces, logs, and dashboards live; the chain-of-custody notes if the incident is also a security incident.

### Three specific things the manager does

- **Open counsel's channel early.** At the chapter 03 severity-escalation thresholds (concrete user harm, prohibited-category output, data breach, scaled misbehaviour on a regulated surface, security-incident shape), page counsel. The AI engineering manager does not wait for certainty; counsel's standing is to determine whether the threshold applies, not to be told that it did.
- **Translate engineering reality into counsel's frame.** Counsel asks "did a user suffer harm?" — not "what was the eval-score delta." The manager translates. "No concrete user harm; the output was intercepted by HITL in 94% of cases; the remaining 6% produced a disallowed-but-not-prohibited output that users were able to refuse or escalate" is the kind of engineering statement counsel can build a filing on.
- **Hold the engineering-side narrative to the evidence.** If the timeline says TTM was 22 minutes, the narrative says 22 minutes. The manager's job is to prevent the summary from drifting toward favourable rounding; counsel's filing is as good as the underlying narrative and no better.

## Core concept 2 — EU AI Act Article 73 at EM depth

Article 73 of Regulation (EU) 2024/1689 obliges providers of high-risk AI systems placed on the EU market to report "serious incidents" to the market-surveillance authority of the Member State where the incident occurred. The provider must also co-operate with the authority on investigation. The exact language of the article should be verified against the current text (see resources.md).

At EM depth, the manager must know:

### What is a "serious incident"

Article 3(49) (as it applies to Article 73) defines a serious incident broadly as any incident or malfunctioning of an AI system that directly or indirectly leads to one of several enumerated harms — including the death or serious damage to the health of a person, serious and irreversible disruption of the management or operation of critical infrastructure, infringement of fundamental rights obligations under EU law, or serious damage to property or the environment. Counsel's reading of the current text controls whether a given incident meets the threshold. The manager reads the definition well enough to recognise the shapes that *could* qualify, and routes early.

### What triggers notification

Awareness. The 15-day clock (short form; longer for widespread infringement; shorter for serious disruption of critical infrastructure — verify with counsel against the current text) begins when the provider becomes aware that the AI system is causally linked to the serious incident.

### What goes in the notification

The implementing-act template controls. At the time of drafting the notification, counsel reads the current template and determines the exact fields. Common fields across comparable regimes include:

- Identification of the provider and the AI system.
- Nature of the serious incident; the harm shape.
- Scope of impact (affected persons; geographies; durations).
- Timeline of events; when the provider became aware; the mitigation(s) applied.
- Causal analysis: the technical mechanism by which the AI system caused or contributed to the harm.
- Remediation plan: the corrective and preventive measures the provider has taken or is taking.
- Supporting evidence: pointers to traces, logs, test results, model / prompt / data / classifier versions.

### Co-operation with the authority

Once filed, the market-surveillance authority may request additional information or investigation co-operation. The provider's obligation under Article 73 includes continued co-operation. The engineering-side implication is that the evidence must remain available for the duration of the authority's engagement; the manager extends the evidence-store retention accordingly and co-ordinates with counsel on the format of any further-information response.

### Scope — what Article 73 does and does not cover

Article 73 covers providers of high-risk systems placed on the EU market. Deployers have related but distinct obligations under other articles. Non-high-risk systems do not fall under Article 73, but may fall under Article 50 transparency obligations, under sectoral regulation, or under horizontal regimes like the GDPR. Counsel determines applicability; the manager produces the engineering summary regardless, because if the system turns out to be in scope the summary is already ready.

### A note on adjacent regimes

Three other regulatory shapes an AI engineering manager meets, often alongside Article 73:

- **GDPR Article 33 (personal-data breach notification).** 72-hour window to notify the relevant supervisory authority. Triggered by incidents where personal data is disclosed, altered, lost, or destroyed outside the intended processing. Any AI incident that caused a data egress or an authorised-to-unauthorised-party disclosure is GDPR-Article-33-adjacent; counsel decides the threshold.
- **Sectoral regulators (FDA, SEC, EEOC, HHS, financial-services authorities).** Where the product operates in a regulated sector, the regulator may have incident-reporting obligations that pre-date and layer on top of EU AI Act obligations.
- **Contractual notification clauses.** Customer DPAs and MSAs often contain specific notification obligations — "notify within 24 hours of a security incident affecting customer data," "notify within 7 days of a service incident exceeding N minutes." Procurement / legal owns the catalogue; the manager keeps the list visible to the on-call so the incident-response path includes contractual notifications alongside regulatory ones.

### US state-level AI regulation

AI-specific obligations are emerging at US state level. Current examples (as of Q4 2026, subject to change) include jurisdictions regulating automated decision-making in employment, consumer-facing AI disclosures, and sector-specific uses. Counsel owns the current reading; the manager tracks the surface of the company's deployment against the regulations counsel highlights. <!-- needs-research: specific list of currently-in-force US state AI laws relevant to the module's audience; this changes too fast to pin without legal review. -->

## Core concept 3 — the engineering-side narrative, section by section

The artefact the manager drafts. Call it the **engineering summary** to distinguish from the legal filing (which counsel writes, reading off the summary). One document, kept current from incident open to filing close.

### Section 1 — identification

- Incident ID.
- AI system / product / surface identifier.
- Deployment context (which markets, which audiences, which severity tier under the mod-106 chapter 05 triad).
- Provider / deployer framing: is the organisation acting as provider, deployer, or both for this AI system under the AI Act's definitions? Counsel confirms; engineering records.

### Section 2 — the incident, factually

A factual account. What happened, when, on which surface, to how many users, with what outputs. No causal language yet; just what occurred. This is the hardest section to write well because the temptation to narrate causes is large; discipline is to split facts from causes.

### Section 3 — awareness timeline

Specifically: when did the organisation become aware of the serious incident? The Article 73 clock begins at awareness; the timeline must be specific. Three sub-timestamps:

- **First detectable moment** — when the first signal (monitor, user report, trace) that in retrospect shows the incident had begun.
- **First human awareness** — when a person in the organisation first knew that something was wrong (not the specific classification; just the awareness).
- **Serious-incident classification** — when counsel (or the manager in counsel's absence) determined that the event met the Article 73 threshold.

Counsel reads the three timestamps and determines when the regulatory clock began; the manager provides the specifics.

### Section 4 — scope of impact

- Number of users affected (or best estimate with method).
- Geographic distribution.
- User segments (where known) — vulnerable populations (minors, patients, financially distressed) get specific attention.
- Duration of the misbehaviour.
- Concrete harms traceable to the output (reports received, complaints logged, external evidence).

Scope is a factual section; the write-up is specific about method ("affected-user count estimated from the production trace volume filtered on the injection signature between T1 and T2; see appendix A").

### Section 5 — technical causal analysis

Here the narrative becomes causal. The chapter 05 postmortem's "what went wrong" (contributing causes), re-written at an altitude a regulator will read:

- The AI-system behaviour that caused or contributed to the harm.
- The chain from input to output (model, prompt, retrieval, tools).
- The guardrails and controls that failed or were absent.
- The versions of model, prompt, judge, dataset, retrieval index, classifier, tool schema at incident time.

The language is accessible. "The retrieved-content moderator was wired for user-provided content but not for RAG-ingested content; a malicious payload in a recently-ingested document was interpreted by the model as a privileged instruction; the agent followed the injected instruction on Agent Surface A" is what the regulator reads. Code names and acronyms are translated or glossed.

### Section 6 — mitigation

What was done, when, with what effect.

- Initial mitigation (kill-switch, HITL-tightening, route-to-safer-model, rollback — chapter 03 levers).
- Timestamp and effect.
- Follow-on mitigation.
- Current state of the system.

### Section 7 — remediation

The preventive and corrective actions already taken or scheduled. The chapter 05 postmortem's action items, re-written at the regulator's altitude:

- Technical changes (fixture added; moderator wired; detector tuned).
- Process changes (runbook updated; dry-run scheduled).
- Investigation (AI Risk review of attack class; red-team engagement scoped).
- Monitoring (new alarm on the specific signal).
- Loop-closed status (chapter 06).

### Section 8 — supporting evidence

- Evidence-store location and access instructions (for cooperation under Article 73).
- Pointers to the postmortem, the fixture, the judge case.
- Any external reports or third-party findings.

### Section 9 — organisational contacts

- Manager (engineering point of contact).
- Counsel (legal point of contact).
- Security operations (if security incident).
- DPO (if GDPR-adjacent).

### Section 10 — review and sign-off

- Manager sign-off on factual accuracy.
- Counsel sign-off on legal adequacy (to be filled when counsel reads).
- Dates.

The engineering summary is **not** the regulatory filing. The regulatory filing is what counsel writes, after reading the summary, mapped to the implementing-act template. The summary is the single-source-of-truth for facts; counsel's filing is one audience for those facts.

## Core concept 4 — the delineation, concretely

Specific examples of what each role says. Illustrative; not exhaustive.

| Engineering says | Counsel says |
|---|---|
| "No concrete user harm was identified in the preserved traces or the customer-support ticket set for the incident window." | "The incident does not meet the Article 73 serious-incident threshold on the health / rights / infrastructure criteria." |
| "The AI-system behaviour was: the moderator did not fire on retrieved content because retrieved-content moderation was not wired on this surface." | "The deficiency in the system affected its compliance with Article 15 (accuracy, robustness, cybersecurity) on the robustness sub-property." |
| "The system reverted to a safe fallback (route-to-safer-model) at 13:47 UTC; the regression on the affected surface was mitigated at 14:02 UTC; a persistent fix shipped on 2026-11-18." | "The provider has taken immediate corrective action; the corrective and preventive measures required under Article 73(3) are in progress." |
| "Fixture `agent-a-safety/injection/rag-ingest/v1` is in the eval registry, wired to the pre-merge gate; a regression on the same pattern cannot ship without a two-signature override." | "The provider has implemented a preventive control that satisfies the Article 73(3) obligation to prevent recurrence." |

The pattern: the manager describes the system state in engineering terms; counsel maps the description to the relevant article. Neither is interchangeable.

## Core concept 5 — recognising a security incident shape early

Some AI incidents are *misbehaviours*; some are *attacks*. The response differs. The manager must recognise the shape early and route.

### Signals of a security-incident shape

- The attacker's identity or persistence is potentially traceable (specific user account, IP pattern, API-key compromise).
- The incident's cause involves unauthorised modification — prompt-registry compromise, embedding-store poisoning, tool-schema tampering, credential breach.
- The incident involves a documented adversarial technique (OWASP LLM Top 10, MITRE ATLAS, a known attack chain).
- The incident involves exfiltration of data outside the privacy-notice scope.
- Legal action (civil or criminal) may follow from the attack.

### What changes when it is a security incident

- **Security operations / DFIR becomes IC.** The AI engineering manager steps back from the IC role; AI engineering is Subject-Matter. The IC rotates to someone with forensic-investigation training.
- **Chain-of-custody discipline tightens.** Evidence handling moves to a controlled process; informal preservation becomes insufficient. The format of the evidence must be legally admissible.
- **Disclosure controls apply.** The incident channel may be re-created in a restricted space. Communications are tightly controlled. The Comms Lead's templates no longer apply; a security-communications pattern takes over.
- **Counsel is co-involved from day zero.** Not for regulatory filing alone; for legal advice on law-enforcement engagement, insurance notification, and civil-litigation posture.
- **The attacker may still be in the system.** Mitigations that tip the attacker off (e.g., revoking their credentials visibly) may be delayed to allow forensics; the IC decides, not the engineering team.

### The hand-off

Specific:

- The AI engineering manager calls the hand-off: "This has the shape of a security incident; I am paging security operations."
- Security operations assesses and either takes IC or returns with a classification.
- If security takes IC, AI engineering continues to run the technical mitigation under security's direction; evidence preservation continues under security's chain-of-custody.
- Counsel is informed; the Article 73 / Article 33 / contractual-notification determinations continue under counsel's control, with security's input on the forensic facts.

The AI engineering manager's role in a security incident is to *facilitate*, *preserve evidence to the AI side*, and *not freelance* on the DFIR side. The craft of incident forensics is not AI Engineering Manager craft.

## Core concept 6 — the engineering summary in practice — a skeleton

```markdown
# Engineering summary — inc-20261108-01
## For counsel's regulator-facing filing (Article 73-adjacent / tbd)

Status: draft for counsel review · Last updated: 2026-11-18 · Manager: @me

## 1. Identification
- Product / AI system: Agent Surface A (Healthwise Chat v2.3).
- Markets deployed: EU (EU-residency inference endpoints), US, UK.
- Role: our organisation acts as **provider** of the AI system (to be confirmed by counsel).
- Severity tier (per mod-106 chapter 05): HIGH.

## 2. The incident, factually
Between 2026-11-08 13:40 UTC and 14:02 UTC, Agent Surface A deviated from its system prompt on approximately 0.6% of sessions when the retrieval pipeline returned a specific recently-ingested corpus. The deviation was bounded to the agent's conversational output; no tool calls outside scope were produced; no external side-effects were confirmed.

## 3. Awareness timeline
- First detectable moment: 2026-11-08 13:33 UTC (first malicious session; signature retrospectively identified).
- First human awareness: 2026-11-08 13:40 UTC (injection-detector alarm; on-call acknowledged).
- Serious-incident classification: 2026-11-09 09:30 UTC (counsel briefed; no Article 73 threshold triggered; classification updated on 2026-11-18 at 15:00 UTC pending further AI Risk review).

## 4. Scope of impact
- Affected sessions: 182 sessions, 122 unique users (estimated from production traces filtered on the injection signature between 13:33 and 14:02 UTC, 2026-11-08; see appendix A for method).
- Geographic distribution: EU (62%), US (26%), UK (12%).
- Segments: no minors identified; no vulnerable-population flag; no concentration in regulated-profession usage.
- Duration: 29 minutes from first detectable moment to mitigation holding.
- Concrete harms: none confirmed in user-support ticket review (2026-11-09 through 2026-11-15); no external reports; no financial / medical / rights harm identified.

## 5. Technical causal analysis
A corpus of health-information articles was ingested on 2026-11-07 into the retrieval index serving Agent Surface A. A pre-ingest moderation step had been reverted two weeks prior (performance regression); the revert inadvertently disabled moderation of ingested content. The retrieved-content moderator at the retrieval-output stage was not wired on Agent Surface A for RAG-ingested content (only for user-provided content); this gap was known in the red-team backlog (finding RT-2026-Q3-08). When a document in the ingested corpus contained injection-style instructions, the model interpreted them as privileged and deviated from its system prompt.
- Model: anthropic/claude-sonnet-X-snapshot-YYYYMMDD (pinned).
- System prompt: version sys-agent-a-v12.
- Judge: judge-rag-faithfulness-v4 (eval judge; not involved in the incident path).
- Retrieval index: index-agent-a-v34; corpus added ingest-a-20261107-A (+42,186 documents).
- Guardrails: input moderator — did not fire (not in path for RAG content). Retrieved-content moderator — not wired. Output moderator — fired at low confidence on 6 of 30 adversarial sessions, below auto-block threshold.

## 6. Mitigation
- 13:45 UTC — evidence preserved.
- 13:47 UTC — HITL-tightening applied on Surface A (input moderator threshold lowered; HITL queue widened).
- 15:30 UTC — rollback of ingest-a-20261107-A applied; retrieval returned to baseline.
- 14:02 UTC — mitigation confirmed holding; sessions-exhibiting-signature returned to zero.
- 2026-11-11 — retrieved-content moderator wired on Surface A for RAG-ingested content (fix shipped).
- 2026-11-18 — fixture agent-a-safety/injection/rag-ingest/v1 in eval registry; wired to pre-merge gate.
- Current state: Surface A operating normally; monitoring continues per chapter 05's post-launch review cadence.

## 7. Remediation
- Technical: retrieved-content moderator wired (AI-PM-002 closed 2026-11-11); ingest-pipeline pre-ingest moderation re-enabled (AI-PM-003 target 2026-11-30).
- Monitoring: injection-detector alarm window tightened from 5min to 2min (AI-PM-004 closed 2026-11-14).
- Process: runbook-direct-injection.md updated with the indirect-via-RAG branch (AI-PM-005 closed 2026-11-15).
- Investigation: AI Risk engagement commissioned on full indirect-injection attack class via ingest pipeline on all surfaces (AI-PM-006 target 2026-12-10).
- Loop-closed status: closed on 2026-11-20 for inc-20261108-01 (fixture in registry, wired to pre-merge gate, AI Risk sign-off on coverage).

## 8. Supporting evidence
- Evidence store: s3://evidence/inc-20261108-01/
- Postmortem: [internal link]
- Fixture: registry/golden-datasets/agent-a-safety/injection/rag-ingest/v1.yaml
- Judge case: registry/judges/judge-injection-refusal/v2.yaml
- Dashboards: [snapshots in evidence store, dashboard-id GRD-chat-07, retention through 2027-11-08]

## 9. Organisational contacts
- Engineering: @manager (<email>)
- Counsel: <counsel contact>
- Security operations: not activated (no security-incident shape)
- DPO: notified 2026-11-09 (no personal-data breach; no GDPR Article 33 trigger)

## 10. Review and sign-off
- Manager factual accuracy: @manager, 2026-11-20.
- Counsel legal adequacy: [pending]
- Next update: as counsel requests, or on any material change to remediation status.
```

The skeleton is deliberately full. A real engineering summary at this depth is 3–5 pages depending on the incident; the one-pager is for short cases. The point is that counsel can read the summary and know the facts; the regulator's filing (if any) is then derived.

## Core concept 7 — timing, drafting, and sign-off discipline

Three time-related disciplines bind the regulator-facing path.

### Timing discipline 1 — the draft begins during the incident

The engineering summary is **not** written after the incident closes. It is *started* during the incident, kept current by the Scribe and the manager as the incident unfolds, and tightened after close. Starting it late risks losing context and risks the drift described in motivation anti-pattern 2.

### Timing discipline 2 — counsel sees drafts, not final

The summary is shown to counsel in-progress. Counsel's reading informs the structure (what field does counsel want? what detail is material?); the manager's drafting informs the technical accuracy. Iteration is normal; final sign-off is after both are satisfied.

### Timing discipline 3 — the clock is counsel's

The 15-day Article 73 window, the 72-hour GDPR Article 33 window, any contractual windows — the clock is counsel's to run. The manager's job is to produce the evidence counsel needs *before* the window closes. Pre-stage the summary; do not wait for counsel to ask.

## Core concept 8 — anti-patterns the posture defeats

Four failure modes.

- **The regulator-reads-it-first pattern.** Prevented by counsel-in-the-room on the chapter 03 escalation thresholds, and by starting the engineering summary during the incident.
- **The filing-contradicts-the-evidence pattern.** Prevented by the manager's discipline that the summary tracks the evidence (timeline is the Scribe's record; numbers are the dashboards' numbers; mitigations are the deploy log); and by counsel's sign-off reading against the summary.
- **The security-incident-run-as-engineering pattern.** Prevented by the chapter 03 severity-escalation and the security-signal recognition of core concept 5; security operations becomes IC on the signals listed.
- **The "we told counsel afterwards" pattern.** Prevented by the rule that counsel is paged on the escalation threshold, not briefed after the fact. Over-escalation is normal; counsel can stand down.

## What good looks like

- Counsel has a pre-drafted template of the engineering-summary skeleton, reviewed and adapted to the organisation's regulatory shape.
- The chapter 03 escalation thresholds page counsel; the on-call rotation knows which thresholds apply and how to page.
- An AI incident in the last twelve months has run the engineering-summary process end-to-end at least once (real or tabletop); the discipline is practised, not theoretical.
- The security-incident hand-off has been tabletopped; the security operations function has met the AI engineering team outside an in-flight incident.
- The Article 73 window — and its GDPR / sectoral equivalents — are known to the on-call rotation; counsel is involved early enough on candidate cases that the windows are never discovered after expiry.
- The engineering summary is kept current; it is the single-source-of-truth for facts in any regulator-facing conversation.

## What bad looks like

- The team has never practised the engineering-summary process; the first time it runs is on a real Article 73-adjacent incident.
- Counsel first hears about an incident from the engineering team's quarterly review.
- The engineering summary is written after close, from memory, and diverges from the evidence.
- A security-incident shape is run as an engineering misbehaviour for weeks before DFIR is involved.
- Contractual notification obligations are discovered during a customer's review of a service-level credit, not during the incident response.

## Summary

- Regulator-facing incident narrative is a partnership: engineering owns the facts; counsel owns the legal argument and the filing. The manager is the connective tissue.
- EU AI Act Article 73 is the explicit EU obligation for serious-incident reporting on high-risk AI systems; GDPR Article 33, sectoral regulators, and contractual notifications are adjacent obligations with their own clocks.
- The engineering summary is a ten-section artefact (identification, facts, awareness timeline, scope, causal analysis, mitigation, remediation, evidence, contacts, sign-off); it begins during the incident, is kept current, and is the single-source-of-truth for facts in the regulator conversation.
- Security-incident shapes trigger a specific hand-off: security operations / DFIR becomes IC; chain-of-custody discipline tightens; disclosure controls apply; counsel is co-involved from day zero.
- Timing discipline: the draft begins during the incident, counsel sees drafts not final, and the clock is counsel's to run.
- Four anti-patterns — regulator-reads-first, filing-contradicts-evidence, security-run-as-engineering, counsel-told-afterwards — each have a specific preventive discipline.

The next and final chapter picks up the cross-cutting reading habit: the public AI-incident corpora the manager reads quarterly to learn from other organisations' incidents.
