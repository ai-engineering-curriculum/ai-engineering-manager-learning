# The Manager's Altitude for AI Incident Response

Mod-106 taught the gate: the release triad reads eval evidence, data posture, and the safety case, and signs — or refuses. The gate is upstream of production. This module is downstream. Every feature of consequence that cleared the gate is now live, and some of them are going to misbehave in a way the gate did not catch. The question this module answers is not "how do we prevent incidents" — that was the gate's job and it is partial. The question is "when an incident happens, does the team *respond* in a way that limits harm, preserves the evidence, learns from the event, and makes the same incident impossible to repeat?"

Incident response is a specialty discipline. The site reliability engineering community has spent two decades producing a durable body of practice around it — the Google SRE Book, the Google SRE Workbook, Etsy's blameless-postmortem articles. An AI engineering manager does not need to reinvent any of it. What the manager needs to do is *adapt* that practice to AI-shaped failure modes, where the thing that went wrong is often not a crashed service but a model that returned a confidently wrong answer, a classifier that let through what it should have blocked, or an agent that correctly interpreted its instructions and used a tool in a way nobody wanted.

This chapter is the manager's altitude: what the manager owns, what defers to others, and the four artefacts the rest of the module builds.

## Motivation — three failure shapes when incident response has no manager-owned surface

- **The pageless incident.** The model-vendor pushed a minor version bump overnight; the next morning the team's happy-path numbers are 2% worse and customer tickets are up 20%. Nobody was paged because no monitor was wired to the eval-online signal. The team discovers the regression on Thursday from a product-manager Slack message, spends Friday reproducing it, and loses the week. The incident existed from Monday; the response began Thursday. The gap is a *detection and paging* gap the on-call system is supposed to close, and in most AI teams the on-call rotation was designed for service-outage shapes, not for model-quality shapes.
- **The incident that nobody wrote down.** A jailbreak path lands in production; the on-call engineer notices it in traces, deploys a guardrail patch, closes the ticket. No postmortem. No fixture added to the eval suite. Three months later, a colleague on another feature trips the same path, because nothing was learned outside the one engineer's head. The on-call *responded*; the team did not *learn*. The gap is a *postmortem-into-permanent-system-change* gap that distinguishes mature incident practice from ad-hoc firefighting.
- **The incident the regulator reads about before legal does.** A customer posts a Twitter thread showing the agent recommending unsafe behaviour to a vulnerable user. The engineering team sees the thread, scrambles to disable the feature, drafts an internal explanation, and sends it to leadership. A week later, counsel asks for the evidence to respond to a regulator inquiry under EU AI Act Article 73 — and the evidence is gone, because the traces were on a seven-day log retention and nobody preserved them. The team responded to the operational side; it did not preserve the regulator-facing narrative. The gap is a *compliance evidence* gap that no SRE book covers, because it is specific to regulated AI.

All three are governance gaps, not tooling gaps. Better monitoring would not fix any of them alone. The missing thing is an operating cadence the manager owns.

## Core concept 1 — the four manager-owned artefacts

Every chapter in this module builds one of four manager-owned artefacts. Memorise the list; the chapters and exercises track it.

1. **The on-call shape and runbook library (chapters 02–03).** Who is on call, for what surface, with what pager target, at what severity tier; the AI-specific failure-mode catalogue (chapter 02) and the runbook per failure mode (chapter 03). The manager owns the shape of the rotation, the severity schema, the runbook *library* (its coverage, not every page's craft), and the dry-run cadence. The actual craft of writing a monitor query or wiring a kill-switch defers to Platform / SRE / Applied AI.
2. **The incident-command system (chapter 04).** The named roles for an in-flight incident — Incident Commander, Communications Lead, Scribe, Subject-Matter Lead — adapted to the AI surface; TTD / TTM metrics; the four mitigation levers (kill-switch, route-to-safer-model, rollback, HITL-tightening) the Incident Commander draws from; the evidence-preservation discipline that survives both root-cause analysis and compliance review. The manager owns the system; the Incident Commander on the call runs it.
3. **The blameless postmortem discipline and the incident-to-fixture loop (chapters 05–06).** The postmortem template with AI-specific fields; the facilitation ritual; the action-item discipline; the mandatory conversion of every confirmed incident into a permanent eval fixture (mod-104) and a judge case in the registry. The manager owns the discipline — that it happens, that it is blameless, that the loop closes — not the craft of writing the fixture, which belongs to the `applied-ai-engineer` / `ai-eval-engineer`.
4. **The regulator-facing incident narrative, co-owned with counsel (chapter 07).** The engineering-side summary the counsel's legal filing reads off when EU AI Act Article 73 (or an equivalent obligation) is triggered; the delineation between engineering-owned facts and counsel-owned legal argument; the hand-off to security operations / DFIR when the incident is an attack rather than a misbehaviour. The manager owns the engineering half; counsel owns the legal half; neither owns both.

A reading discipline that cuts across all four: the **public AI-incident corpora** (chapter 08). The AI Incident Database, the OECD.AI Incidents Monitor, and the MIT AI Risk Repository are the industry's shared memory of what has already gone wrong. An AI engineering manager who reads them quarterly is one who meets fewer novel incidents.

A team has a **manager-owned incident posture** when: the on-call rotation knows which runbook to open for each of the ten failure modes in chapter 02; an Incident Commander role is defined and has been exercised; every confirmed incident in the last twelve months produced a blameless postmortem and a permanent eval fixture; counsel has a draft of what the engineering summary looks like for an Article 73-adjacent incident; and the manager has read the latest OECD.AI Incidents Monitor summary within the last quarter.

## Core concept 2 — what the IC owns, versus what the manager owns

The split from the rest of the track repeats here. Five rows worth being specific about.

| What the IC owns (craft) | What the manager owns (system) |
|---|---|
| Writing the monitor query that detects a false-negative wave on the moderation classifier; tuning the alert threshold; wiring the pager integration. Owned by `sre-engineer`, `platform-engineer`, or the `applied-ai-engineer` depending on org shape. | Specifying **that** a monitor exists for each failure mode in the catalogue (chapter 02) and that it pages on crossing a defined threshold, and that the paging target has been exercised in a dry-run. Chapter 03 is this. |
| Running the incident on the call — reading the trace, deciding which mitigation lever to pull, executing the kill-switch or rollback, communicating to the room. This is the Incident Commander's work in the moment. | Defining the **Incident Commander** role, the escalation matrix (when IC calls for help from governance / counsel / exec), the stakeholder-notification matrix, and the training that gets an IC to competence. Chapter 04. |
| Writing the forensic narrative — re-running the trace, identifying the root cause, proposing the fix. This is a mix of `applied-ai-engineer`, `ai-eval-engineer`, `ai-risk-engineer`, and `sre-engineer` depending on the incident shape. | Facilitating the **blameless postmortem** — convening the review, holding the room on causes-not-people, writing the action-item list, assigning owners, following up to closure. Chapter 05. |
| Authoring the eval fixture and judge case that encodes the incident as a regression test — redacting PII, parametrising the attack, writing the assertion, versioning the dataset. Owned by `ai-eval-engineer` / `applied-ai-engineer`. | Requiring **that** the fixture exists, that it is wired to the pre-merge or release gate (mod-104 chapter 03), and that the loop is closed before the postmortem is signed off. Chapter 06. |
| Running SOC-side attack forensics (log collection, chain-of-custody, attacker-attribution, disclosure coordination) when an incident is a security incident, and co-owning the Article 73 technical sections when counsel asks. Security operations / DFIR work. | Writing the **engineering-side incident narrative** (facts, timeline, mitigations, impact) that counsel's filing reads off, routing the SOC-side pieces to security, and co-signing the summary counsel sends to the regulator. Chapter 07. |

The pattern: *the IC operates the lever; the manager owns the system in which the lever exists.* Managers who personally drive the kill-switch during an incident are generally slowing the response; managers who leave the entire incident system to whoever happens to be on call are importing risk they later own at the postmortem.

## Core concept 3 — the four deferrals

Four roles the AI engineering manager explicitly defers to on this module. Each has standing authority the manager does not.

### SRE / Platform owns the on-call substrate

The paging system (PagerDuty, Opsgenie, equivalent), the monitoring and observability stack (metrics, traces, logs), the runbook platform, and the incident-management tooling (Statuspage, incident Slack channels, retrospective tooling) belong to the SRE / Platform function. The manager runs the AI side *on top of* that substrate. The craft of tuning paging to reduce noise, of defining service-level indicators, of writing the on-call compensation policy, of running the operational-review cadence, is the SRE discipline in `sre-engineer` / `platform-engineer`. The manager reads the Google SRE Book and Workbook to have the vocabulary and the shape of the mature practice, and partners with SRE when an AI-specific concern (model-output monitoring, prompt-trace retention) needs the substrate to accommodate it. Chapter 03 is specific about this split.

### Security operations / DFIR owns attack-side forensics

When the incident shape is a misbehaviour, the engineering team owns the response. When the incident shape is a *security incident* — a successful prompt injection used to exfiltrate data, a credential compromise that let an attacker modify a prompt in the registry, a tool-use exploit that caused a side-effect in a connected system — the response belongs to security operations / Digital Forensics and Incident Response (DFIR), with the AI engineering team as a subject-matter partner. The manager recognises the shape of a security incident early (chapter 04), routes the call-out, preserves the AI-side evidence, and does not freelance on chain-of-custody. The craft of attacker attribution, legal-admissible evidence handling, and coordinated disclosure belongs to security. Chapter 07 walks the hand-off.

### AI Risk / Red-Team owns the attack-vector analysis

The craft of taking an incident's attack path apart — what class of jailbreak is this, how does it chain through retrieval, what variants would evade the fix the team is proposing, what corollary attacks become possible once this one is known — belongs to the `ai-risk-engineer` (level 25, peer-to-manager) or the `agentic-safety-engineer` (level 40, agentic-surface specialist). The manager commissions the analysis, reads the findings, and ships the fix the AI Risk reviewer signs on. The manager does not personally write novel jailbreak chains in response to the incident. Chapter 06 is where this deferral binds (the fixture is only as good as the AI Risk review of its coverage).

### Counsel owns legal binding, the regulator conversation, and the Article 73 filing

The manager translates the shape of the incident into facts, timeline, mitigations, and impact at the altitude a lawyer can use. Counsel determines whether the event meets the Article 73 "serious incident" threshold, whether notification is required and to which authority, what disclosure obligations exist to customers and data subjects, and what language goes into any regulator-facing filing. The manager co-drafts the engineering narrative; counsel writes the legal argument and signs the filing. Chapter 07 is specific about this split.

A pattern to internalise: the manager's job across all four deferrals is *the connective tissue*. The SRE substrate knows nothing about prompt versions until the manager tells it to; the DFIR responder knows nothing about retrieval-store corruption until the manager explains it; the AI Risk reviewer needs the manager's framing of product scope to decide what adjacent attacks matter; counsel needs the manager's engineering summary to decide what to file. The manager is not the specialist in any of those roles; the manager is the one who makes the specialists talk to each other inside a single incident.

## Core concept 4 — what this module does not teach

Three deliberate exclusions, each a referral.

- **How to build the SRE substrate.** Monitoring, paging, service-level objective design, operational-review practice, blameless-postmortem *facilitation* at the craft level — all of this belongs to the SRE curriculum. This module teaches the AI-specific adaptation: which metrics an AI product needs that a classic web service does not, which failure modes the catalogue includes, which fields the postmortem template needs that a classic postmortem does not. Partner with SRE / Platform for the substrate; teach SRE / Platform the AI shape of the problem.
- **How to run DFIR on an AI-side security incident.** Chain-of-custody, attacker attribution, forensic log analysis, coordinated disclosure, law-enforcement engagement — all of this belongs to security operations. This module teaches the manager how to *recognise* a security-incident shape, *route* the call-out, and *preserve* the AI-specific evidence (traces, prompts, retrieval payloads, tool-call arguments) that the DFIR team will want.
- **How to write the Article 73 legal filing.** Classifying an event as a "serious incident", determining the notification authority, authoring the legal language — all of this belongs to counsel. This module teaches the manager to draft the *engineering summary* that counsel's filing reads off, and to recognise the trigger conditions early enough to involve counsel in time.

What the manager must know at depth: the **vocabulary** of incident response (page, incident commander, severity tier, TTD, TTM, kill-switch, rollback, routing-to-safer-model, blameless postmortem, action-item, fixture, judge case, harm classification, serious-incident threshold, evidence preservation); the **shape** of the on-call system and when it fires; the **ten AI-specific failure modes** in the catalogue; the **loop** from incident to permanent eval fixture; the **partnership** with counsel on the regulator conversation; and the **habit** of reading other organisations' incidents.

## The shape of the chapters ahead

- **Chapter 02 — The AI failure-mode catalogue.** Ten AI-specific failure modes: jailbreak, prompt injection (direct and indirect), hallucination, regression after model-vendor upgrade, judge drift, tool-use failure, embedding-store corruption, retrieval degradation, outage (provider), moderation-classifier false-positive storm. Per failure mode: signature, detection, mitigation lever, evidence to preserve, regulator-adjacent considerations.
- **Chapter 03 — Designing the on-call and incident path.** SRE discipline adapted to the AI surface: severity tiers, paging criteria, runbook anatomy, the four mitigation levers (kill-switch, routing-to-safer-model, rollback, HITL-tightening), the AI-shaped observability stack, the dry-run cadence. Manager-owned shape; Platform / SRE-owned craft.
- **Chapter 04 — Incident command for the AI-shaped surface.** Incident Commander, Communications Lead, Scribe, Subject-Matter Lead; TTD / TTM; evidence preservation at an altitude that survives both root cause and compliance; escalation to governance when a harm threshold is crossed.
- **Chapter 05 — Blameless postmortems that change the system.** The template with AI-specific fields; the facilitation discipline; action-item maturity; the review cadence; the four anti-patterns to defeat.
- **Chapter 06 — The incident-to-fixture loop.** Capture → redact → fixture → judge case → regression gate → close. Ownership; definition of done; why the loop must be enforced, not aspirational.
- **Chapter 07 — Regulator-facing incident narrative.** EU AI Act Article 73 at EM depth; the engineering-side summary that counsel's filing reads off; the delineation; the SOC hand-off.
- **Chapter 08 — Reading the AI incident corpora.** AIID, OECD.AI Incidents Monitor, MIT AI Risk Repository; the quarterly reading ritual; how the corpora feed tabletops, runbooks, red-team briefs.

## What good looks like

A team has a manager-owned incident posture when:

- The on-call rotation covers the AI surface, pages on AI-specific signals (classifier-error rate, judge-score drift, hallucination-rate spike, tool-use-failure rate, retrieval-coverage drop), and the pager target has been exercised in the last quarter.
- Every AI-specific failure mode in chapter 02 has a runbook page; the pages are current (dated within the last six months or re-validated in a dry-run).
- An Incident Commander role is named and rotated; at least one in-production AI incident in the last twelve months was run with the ICS pattern, and the retrospective produced a dated after-action.
- Every confirmed incident in the last twelve months produced a blameless postmortem; action items were tracked to closure; the incident-to-fixture loop was closed (the fixture is in the eval registry, wired to the release gate).
- Counsel has a draft of the engineering-side incident summary for an Article 73-adjacent scenario; the trigger conditions that would require notification are known in advance, not discovered on the day.
- The manager has read the AIID quarterly digest, the OECD.AI Incidents Monitor summary, and at least one MIT AI Risk Repository update in the last quarter.

## What bad looks like

- The on-call rotation is a service-outage rotation; model-quality regressions, classifier drift, and hallucination spikes do not page.
- The runbook page for "prompt injection incident" does not exist, or exists as a Confluence stub written three years ago and never rehearsed.
- Incidents are handled by whoever happens to be in the room; there is no named Incident Commander; nobody takes notes.
- Postmortems are a line in a sprint retrospective ("we had an incident; we fixed it") with no written document; action items are forgotten within two weeks.
- The eval registry contains no entries labelled "incident-derived"; the team has had incidents, but the regression tests have not grown.
- Counsel first hears about an AI incident from a regulator inquiry, not from the engineering team.
- Nobody on the team can name the last AIID entry they read; nobody is subscribed to the OECD.AI Incidents Monitor.

## Summary

- Incident response for AI products is SRE discipline adapted to AI-shaped failure modes; the manager runs the AI-side adaptation *on top of* the SRE substrate, not instead of it.
- The manager owns four artefacts: the on-call shape and runbook library, the incident-command system, the blameless postmortem discipline with the incident-to-fixture loop, and the regulator-facing engineering narrative co-owned with counsel.
- Four deferrals: SRE / Platform for the substrate, security operations / DFIR for attack-side forensics, AI Risk for attack-vector analysis, counsel for legal binding and the Article 73 filing. The manager is the connective tissue.
- Reading the public AI incident corpora quarterly is a habit, not a side-effect. Teams that read other organisations' incidents meet fewer novel ones themselves.

The next chapter opens the catalogue: ten AI-specific failure modes the on-call rotation must be ready for, with the signature, the detection, the mitigation lever, and the evidence-preservation note the manager needs on a page.
