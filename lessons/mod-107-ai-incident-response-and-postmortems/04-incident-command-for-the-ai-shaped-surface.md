# Incident Command for the AI-Shaped Surface

The on-call system of chapter 03 pages someone. This chapter is about what happens in the minutes after the page lands. Incident command is a practice — it has roles, a cadence, a vocabulary, and a set of metrics (TTD, TTM) the discipline is measured against. Google SRE's chapter on incident management and the broader Incident Command System (ICS) tradition are the sources for this discipline. Nothing about an AI surface makes the practice obsolete; the adaptations are additive, not replacing.

What changes on the AI surface: the thing that went wrong is often a *behaviour* rather than a *failure*, the evidence is more fragile (traces rotate, prompts get redeployed, retrieval indices get re-ingested), the mitigation levers are different (chapter 03 — kill-switch, route-to-safer-model, rollback, HITL-tightening), and the threshold at which an incident must escalate to governance / counsel / exec is often crossed on *harm* rather than on outage duration. This chapter is the AI engineering manager's handbook for running incident command on that surface.

## Motivation — three incident-room shapes that fail on AI

- **The room where everyone is a responder.** Six engineers are in the incident channel; four of them are typing mitigation ideas; no one has read the full trace; no one is writing anything down. Forty minutes in, the mitigation that got tried was not the right one, and nobody can reconstruct why it was tried. Without named roles, the room does *more* work and *less* progress.
- **The room that fixed it but preserved nothing.** The IC pulled the kill-switch, the mitigation held, the channel closed in 30 minutes. A week later the postmortem asks for the attacker's input and the pre-kill-switch trace — and they are gone, because the pre-mitigation state was the thing the kill-switch overwrote, and nobody snapshot anything. Fast mitigation without evidence preservation is a postmortem one cannot run.
- **The room that waited for counsel to hear about it.** An output in a prohibited category went live for an hour. The team mitigated, closed the channel, drafted an internal note. Counsel heard about it three days later in a weekly sync and said, through visible surprise, "that might have been an Article 73 serious incident and we had 15 days to notify." The incident-response practice did not include the escalation to governance as an *in-room* decision; counsel was treated as a downstream audience, not an in-the-moment stakeholder.

Each is a specific failure of the incident-command practice. The design in this chapter prevents all three.

## Core concept 1 — the four in-room roles

Four named roles cover almost every AI incident. Smaller incidents collapse roles onto a single person; larger incidents may add specialists (an extra Subject-Matter, a dedicated Scribe for legal purposes). The roles are *functions*, not titles; the manager often is not the IC.

### Role 1 — Incident Commander (IC)

The IC owns the response. They do not write code; they do not pull levers; they *decide*. Specifically the IC:

- Keeps a mental model of the state of the world and the state of the response.
- Chooses which mitigation lever to pull (chapter 03), authorising the ICs who will execute it.
- Decides when to escalate (bump the severity, loop in counsel, loop in exec, declare the incident over).
- Owns the outbound communication cadence (every 15–20 minutes during active response, longer intervals during monitor-and-recover).
- Owns the handoff when the incident spans shifts.

The IC is not necessarily the most-senior person in the room. Separation-of-concerns is the point: the IC decides; the SMEs execute. A fine IC with mediocre AI expertise outperforms a brilliant AI engineer who tries to be both IC and responder.

### Role 2 — Communications Lead

The Comms Lead owns the *outside-the-room* conversation. Three audiences:

- **Internal stakeholders** — product, exec, sales, support, customer success — pre-drafted update templates on a cadence the IC sets.
- **Customer-facing comms** — status-page updates, in-product banners, support-ticket macros, press statements. On SEV1 or scaled SEV2, this is co-owned with customer-communications; the Comms Lead is the engineering voice.
- **Executive** — a concise status note per cadence; longer brief at material state changes (incident upgraded, major mitigation applied, incident declared over).

The Comms Lead protects the IC from being pulled off decision-making to answer "what should I tell the CEO" every six minutes. In a small team, this role sometimes collapses onto the IC; it is a known anti-pattern and worth avoiding whenever a second competent person is in the room.

### Role 3 — Scribe

The Scribe writes down what happened, as it happens. Timestamps, decisions, actions taken, mitigations applied, who said what. The Scribe does *not* investigate; the Scribe *records*. On AI incidents this role is doubly important because the evidence is fragile and the postmortem will draw heavily on the scribed timeline. A good Scribe writes:

- Every state change ("13:42 UTC — classifier false-positive rate crossed 10%; paged.")
- Every decision ("13:47 — IC decides to apply HITL-tightening on surface X; applied-ai @alice executing.")
- Every evidence-preservation action ("13:45 — snapshot of traces for inc-20261108-01 taken; stored at s3://evidence/inc-20261108-01/traces.jsonl")
- Every escalation ("13:52 — IC pages counsel per runbook SEV1 escalation on content-harm category.")
- Every external communication ("14:01 — status page updated: 'investigating classifier regression on X'.")

Mod-109 can pair the Scribe role with an automated transcript (Slack + bot, incident-tool-native transcripts) so the human Scribe's job is curation rather than full transcription. The role still exists; the tool makes it cheaper.

### Role 4 — Subject-Matter Lead (one per concern)

The Subject-Matter Lead is the responder with hands on the system. On an AI incident this is typically one or more of:

- **Applied AI SME** — reads the trace, decides what prompt / model / tool / retrieval change is implicated; executes the mitigation.
- **AI Risk SME** — on injection / jailbreak shapes, characterises the attack class and the adjacent variants; informs the IC whether the mitigation closes the class or only a single path.
- **SRE / Platform SME** — on infrastructure-adjacent shapes (retrieval degradation, provider outage, cost-blast), executes the platform-level mitigation.
- **Security / DFIR SME** — on security-incident shapes (embedding-store compromise, prompt-registry compromise, credential breach), owns chain-of-custody and attacker-side forensics; in these shapes, security operations often becomes the IC and AI engineering is the SME. The hand-off is in chapter 07.

The Subject-Matter Lead's output is what the IC needs to decide, phrased at decision-altitude. "The trace shows the injection fired because the retrieved-content moderator was not wired on this surface; a kill-switch on the retrieval tier closes the path; rollback of the ingest-job from yesterday closes the root cause" is Subject-Matter-Lead output. "The RegEx in the pre-filter is missing a comma after the special-token escape" is not — unless the IC is making that specific decision.

### A specific role-assignment pattern

At the top of the incident channel, pinned, within the first five minutes:

```
IC:        @bob
Comms:     @carol
Scribe:    @dan
SMEs:      Applied AI @alice, AI Risk @eve, Platform @frank
Severity:  SEV2 (direct injection, scaled, no external side-effect confirmed)
Open at:   2026-11-08 13:40 UTC
Runbook:   runbook-direct-injection.md
```

Changing a role requires an explicit handoff ("IC passing to @grace; @grace confirming") recorded by the Scribe. No ambiguity about who is deciding.

## Core concept 2 — the metrics that matter: TTD, TTM, and friends

Two metrics anchor the response, borrowed directly from SRE discipline and applicable without modification.

### TTD — time to detect

The clock-time from when the system first misbehaved to when the alarm paged. TTD is primarily an **observability** metric; a short TTD means the stack of chapter 03 is wired correctly. On AI surfaces TTD can be artificially short (a tweet about a jailbreak is instant) or artificially long (a judge-drift event may take weeks to surface). Track TTD per failure mode; a chronically long TTD on a mode is a signal to invest in the monitor.

### TTM — time to mitigate

The clock-time from alarm to the moment the user-facing impact is stopped (not when root cause is fixed; just when the harm is stemmed). TTM is primarily a **response** metric; a short TTM means the IC had the right lever and the right pre-conditions. Four sub-metrics worth naming:

- **Time-to-ack** — alarm to first human ack. Minutes, not hours.
- **Time-to-IC** — ack to IC assignment. Minutes.
- **Time-to-lever** — IC assignment to first mitigation attempt. Depends on the failure mode; for kill-switch-able ones, minutes.
- **Time-to-stable** — first mitigation to confirmed user-facing-impact stopped. Depends on the mitigation's propagation.

### MTTR — mean time to resolve

Classic SRE metric. The clock-time from alarm to incident-closed (mitigation holding, root cause understood or being investigated post-incident). Track per severity tier. The AI engineering manager watches MTTR per failure mode as an input to runbook-library prioritisation: the failure mode with the worst MTTR is the one the next quarter's investment should target.

### Error-budget analogues

For SLO-shaped surfaces (eval-score floor, refusal-rate ceiling), an *error budget* in the SRE sense is applicable: cumulative deviation from the SLO per quarter, with an exhaustion policy. On AI surfaces this is still maturing as a practice; the manager can read the SRE Workbook on error budgets and apply the pattern to AI metrics where the eval signal is reliable enough.

### A specific review cadence

Monthly review (SRE-shaped operational review): TTD, TTM, MTTR per failure mode and per severity. Patterns: which failure modes are getting faster, which are getting slower, which have a monitor-gap, which have a runbook-gap. The monthly review feeds the quarterly tabletop prioritisation (chapter 03) and the postmortem action-item register (chapter 05).

## Core concept 3 — evidence preservation at the AI altitude

Classical incident response preserves logs, metrics, and system state. AI incident response preserves all of those *plus* a layer specific to the AI surface. The discipline is **preserve before mitigate** — the mitigation often destroys the evidence.

### What to preserve, per incident shape

Covered per failure mode in chapter 02. A superset:

- **Full traces of the misbehaving sessions** — the user turn-by-turn input, the model output, the intermediate tool calls, the retrieval payload, the reasoning summary (where logged).
- **Versions** — the model snapshot, the system prompt version from the prompt registry (mod-104 chapter 04), the judge versions, the retrieval-index version, the classifier versions, the tool-schema versions.
- **Classifier / detector scores** — the outputs of the moderation stack on the misbehaving inputs and outputs, even when they did not fire.
- **Deploy history** — what changed in the last 24 hours (prompt deploys, model migrations, index re-ingests, classifier threshold changes, tool-schema updates). Any of these could be the recent change (chapter 03 mitigation lever 3).
- **External-system side-effects** — what records were written, what emails were sent, what API calls were made, what cost was incurred, by which identity.
- **Dashboard snapshots** — the state of the monitors and dashboards during the incident. Useful because dashboards are usually only queryable for recent windows.

### Preservation as a pinned early action

The runbook page (chapter 03 anatomy section 4) names the specific preserve-before-mitigate commands. On a mature team this is a single script that takes a time window and a surface and writes to an access-controlled evidence store. The IC's first explicit decision after assigning roles is often "Preserve state; Mitigation on hold for N minutes." N is small — the discipline is not to wait long, but to capture before overwriting.

### The evidence store

A specific store, with:

- **Access control** — write-once, append-only where possible; readers restricted to incident / legal / security personnel; access logged.
- **Retention** — longer than the baseline observability retention; typically 90 days to 1 year (verify against your log-retention policy and your Article 73 / Article 33 reporting windows with counsel).
- **Redaction policy** — PII and regulated-category content in traces often requires redaction before wider sharing; the preservation keeps the raw copy under access control and the redacted copy for the wider postmortem audience.
- **Chain-of-custody** — for incidents that may be security incidents (chapter 07), the preservation is the first step of forensic chain-of-custody; coordinate with security on the format and the handoff.

### A note on model-vendor evidence

For incidents that may involve the model provider (regression after upgrade, a provider-side behaviour change, a provider-side moderation policy change), preserve:

- The provider's release notes / changelog for the window.
- The specific model snapshot identifiers (not "gpt-4o" — the pinned version, if your provider offers it).
- Your own request logs with timestamps so you can later ask the provider about the specific requests.

The provider is often willing to investigate a documented regression; providing them with concrete requests shortens the loop materially.

## Core concept 4 — the four mitigation levers in command

Chapter 03 named the four levers (kill-switch, route-to-safer-model, rollback, HITL-tightening) and their priority order. In the incident room, three disciplines around the levers matter.

### Discipline 1 — the IC picks the lever; the SME confirms it is pullable

The IC's decision authority includes picking the lever. The Subject-Matter Lead's job is to confirm that the pre-conditions hold: "yes, the kill-switch exists on this surface and I can pull it in 30 seconds" or "no, the fallback model has not been exercised on this surface in the last 90 days — route-to-safer-model carries unknown risk." The IC's decision is informed by the SME's reality check.

### Discipline 2 — one lever at a time where possible; compose explicitly when not

Composed mitigations (kill-switch + rollback + HITL) are frequent and necessary. The IC names the composition explicitly: "First the kill-switch on tool X; then we tighten HITL on surface Y; then we rollback the ingest-job from yesterday. I want the Scribe's log to show the order." Serial execution of the composition makes causal attribution of the mitigation's effect possible; parallel execution does not.

### Discipline 3 — every lever has a reversal plan before it is pulled

A kill-switch pulled is a feature unavailable; the plan to re-enable it is part of the pull. "We kill-switch the agent tool now; we re-enable when the retrieved-content moderator is deployed and verified on the next 50 samples" is a plan. "We kill-switch the agent tool now" is half a plan.

## Core concept 5 — escalation to governance when a harm threshold is crossed

Classical incident response escalates on *technical* thresholds (SEV1 vs SEV2). AI incident response escalates on *harm* and *regulator-adjacency* thresholds too. The IC must recognise the thresholds and know the escalation targets.

### Threshold — user-side concrete harm

If a user has suffered concrete harm — financial, medical, physical, or significant privacy — traceable to the product's output, the escalation is:

- Counsel: paged. Chapter 07 is the delineation.
- Customer-communications: looped in for user-facing response.
- Governance (where the role exists): looped in on the incident classification.
- Exec: briefed at the executive-briefing cadence the Comms Lead runs.

### Threshold — prohibited-category output

If the output is in a prohibited category (CSAM, direct incitement, specific harms-against-persons output), the escalation is additionally:

- Trust & Safety (where the role exists): looped in on content handling.
- Security operations: on potential platform-abuse patterns.

### Threshold — data breach

If data has been disclosed outside its privacy-notice scope:

- Counsel: paged. GDPR Article 33 has a 72-hour notification window where applicable; other regimes vary.
- Security operations: on forensics.
- DPO (where the role exists): looped in.

### Threshold — scaled misbehaviour on a regulated surface

If the deployment is a candidate high-risk AI system under EU AI Act Annex III and the misbehaviour is at scale on a safety-critical property (accuracy, robustness), the escalation includes **counsel on the Article 73 question**. The IC does not decide the threshold; the IC *routes* the question, which counsel answers (chapter 07).

### Threshold — attack rather than misbehaviour

If the incident is a security incident (chapter 07 — embedding-store compromise, prompt-registry compromise, credential breach, successful exploitation of the agent surface), the escalation includes security operations / DFIR early, and the ICS often re-forms with security as IC and AI engineering as SME. Recognising this threshold early is a specific skill (chapter 07 section 2).

### A specific escalation discipline

The IC does not wait for certainty to escalate. "We are investigating a potential Article 73-adjacent shape; I am paging counsel so counsel can decide on the threshold" is correct. Over-escalation is a smaller cost than under-escalation. The partnership works best when counsel is *in* the room for ambiguous cases and can leave if the ambiguity resolves inward.

## Core concept 6 — the cadence during an incident

The room needs rhythm. A specific pattern:

- **Every 15–20 minutes**: the IC runs a cadence check. State of the world: what has changed since the last check? State of the response: what are we doing next? Blockers: what is slowing us down? Comms Lead sends the internal / external update.
- **Every state change** (mitigation applied, severity changed, SME role added or changed, escalation paged): the Scribe records; the IC decides if a cadence check is warranted earlier.
- **The declaration of "mitigated"**: the IC states explicitly — "Mitigation is holding at <metric value>; monitoring for N minutes; incident remains SEV2 until monitor is clean; postmortem opened; this is incident-channel update #N." No ambiguity about whether the incident is active.
- **The declaration of "closed"**: the IC states explicitly — "Incident closed at <timestamp>. Mitigation held. Post-incident monitoring continues. Postmortem convened for <date / time>. Thank you to responders."

Cadence is the IC's responsibility. It is specifically the thing the IC must not let slide while they are thinking hard about the failure. If the IC cannot do cadence and think at the same time, the Comms Lead takes cadence; the IC thinks.

## Core concept 7 — handoffs across shifts

A long incident spans shifts. Handoff quality is the single most common place a long incident regresses.

### The handoff template

```
Incident: inc-20261108-01 (direct-injection, SEV2)
Open at:  2026-11-08 13:40 UTC
Handoff:  2026-11-08 20:10 UTC
Current IC: @bob → @hannah
Current status:
- Mitigation: HITL-tightening on surface X applied at 14:02; holding.
- Root-cause: suspected recent ingest-job (yesterday); AI Risk reviewing.
- Open questions: (1) whether rollback of ingest is safe; (2) whether injection pattern is narrow or broad.
Decisions pending for @hannah:
- At 20:30, if ingest-rollback has been validated, authorise rollback.
- At 21:00, if monitor is clean, reduce HITL-tightening to normal thresholds.
Not yet done:
- Postmortem calendared.
- Status page final update.
Pin location: #inc-20261108-01 pinned message.
```

The handoff is a scribed, pinned, three-minute conversation between the outgoing and incoming IC. The incoming IC confirms back: "I have the mental model; I am taking IC at 20:10 UTC." The Scribe records the handoff.

## A worked skeleton — the incident channel

```markdown
# #inc-20261108-01 — direct-injection on <surface>

PIN: 
IC: @bob (handed to @hannah at 20:10 UTC)
Comms: @carol
Scribe: @dan
SMEs: Applied AI @alice, AI Risk @eve, Platform @frank
Severity: SEV2 (direct injection, scaled, no external side-effect confirmed)
Open at: 2026-11-08 13:40 UTC
Runbook: runbook-direct-injection.md
Evidence: s3://evidence/inc-20261108-01/

Timeline:
13:40 UTC — alarm (injection-detector hit-rate >1%, 5-min window).
13:42 UTC — @bob ack; channel open; roles assigned; runbook pinned.
13:45 UTC — @alice snapshot preserve-traces.sh; evidence captured.
13:47 UTC — IC decides HITL-tightening on <surface>; @alice executing.
13:52 UTC — IC pages AI Risk lead @eve; @eve joins.
13:58 UTC — HITL-tightening live; detector hit-rate trending down.
14:02 UTC — IC confirms mitigation holding; next update 14:20.
14:05 UTC — Comms update: internal #ai-eng + status page.
...
20:10 UTC — IC handoff @bob → @hannah. Handoff template pinned.
...
22:30 UTC — Mitigation stable for >2h; monitor clean. @hannah declares incident mitigated.
2026-11-09 09:00 UTC — @hannah declares incident closed; postmortem scheduled for 2026-11-11.
```

The channel is boring on purpose. A reader who has never heard of this incident can read the channel in five minutes and know what happened, who did what, and what comes next.

## Core concept 8 — anti-patterns the practice defeats

Five failure modes the design rules out.

- **Everyone decides; nobody decides.** Six responders typing mitigation ideas, no one named IC. Prevention: role assignment within five minutes of channel open; no mitigation lever pulled without explicit IC authorisation.
- **Fast mitigation, zero evidence.** The kill-switch landed; the traces are gone. Prevention: preserve-before-mitigate is the first step on the runbook page; the IC confirms evidence preserved before authorising mitigation.
- **The IC who is also an SME.** The person reading the trace is also the person deciding; neither job is getting enough attention. Prevention: on SEV1 and SEV2, a dedicated IC. Where team size does not allow it, the convention is that the IC does not touch code.
- **The incident that nobody told counsel about.** The response wrapped; counsel found out from the regulator. Prevention: the SEV1 / SEV2 escalation matrix pages counsel on the specific thresholds (user-side harm, prohibited output, data-breach, scaled misbehaviour on a regulated surface, attack-shape). Over-escalate; counsel can stand down.
- **The response that ended before it ended.** The channel closed at 22:30, the mitigation failed at 23:00, nobody noticed until 06:00. Prevention: the "mitigated" declaration is explicit, with a monitor window; the "closed" declaration is a separate declaration after the window. The on-call rotation retains eyes on the monitor through the window.

## What good looks like

- Every incident has a named IC, Comms Lead, Scribe, and at least one Subject-Matter Lead; role assignments are pinned in the channel.
- Evidence preservation is a step in the runbook page and is executed before mitigation.
- TTD / TTM / MTTR are tracked per failure mode and reviewed monthly.
- Counsel is paged at specific severity thresholds; over-escalation is normal and tolerated.
- Handoffs across shifts use a template; the incoming IC confirms the mental model back.
- Incidents are declared "mitigated" and "closed" explicitly; the on-call retains eyes through the monitor window.

## What bad looks like

- Incidents are run by whoever has the loudest voice in the channel; no named IC.
- Mitigation is applied before the trace is snapshot; the postmortem starts from "we don't have the evidence."
- TTD / TTM metrics are not tracked; the team cannot say whether response is getting faster or slower.
- Counsel first hears about an incident in a quarterly review; by then, 72-hour-window obligations have expired.
- Handoffs across shifts are a slack message that says "I'm heading out, @hannah has it from here."

## Summary

- Four named roles — Incident Commander, Communications Lead, Scribe, Subject-Matter Lead — cover almost every AI incident; small incidents collapse roles onto a single person; the roles are functions, not titles.
- TTD and TTM are the anchoring metrics, borrowed from SRE discipline; MTTR is the monthly operational-review metric; error-budget analogues apply where the AI signal is reliable.
- Evidence preservation at the AI altitude includes traces, versions, classifier scores, deploy history, side-effects, dashboard snapshots. The discipline is preserve-before-mitigate; the runbook page names the specific commands.
- The four mitigation levers from chapter 03 are drawn by the IC, confirmed pullable by the SME, named explicitly in composition, and carry a reversal plan.
- Escalation to governance / counsel / exec runs on harm and regulator-adjacency thresholds — user-side concrete harm, prohibited-category output, data breach, scaled misbehaviour on a regulated surface, attack rather than misbehaviour. Over-escalate; counsel can stand down.
- Cadence (15–20 minute updates, explicit state changes, explicit "mitigated" and "closed" declarations) is the IC's responsibility and the single most protective discipline during a long incident.

The next chapter picks up the postmortem — the discipline that converts the scribed record, the preserved evidence, and the mitigation history into a permanent system change.
