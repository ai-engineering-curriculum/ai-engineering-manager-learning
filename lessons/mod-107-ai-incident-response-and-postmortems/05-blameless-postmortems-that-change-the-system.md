# Blameless Postmortems That Change the System

The blameless postmortem is the single most durable practice the SRE and DevOps communities have produced. Etsy's 2012 articulation — "Blameless Post-Mortems and a Just Culture" by John Allspaw — remains the canonical source, and the Google SRE Book / SRE Workbook chapters on postmortem culture carry the discipline into production operations at scale. An AI engineering manager who has internalised the practice does not need to reinvent it; what the manager needs is the *adaptation* to the AI surface, where the thing that went wrong is often a behaviour, the evidence is more fragile, and the output of the postmortem must close a specific loop into the eval suite (chapter 06).

This chapter is that adaptation. The template, the facilitation discipline, the action-item maturity, the review cadence. The practice is the same; the AI-specific fields and the loop into fixtures are new.

A grounding note on terminology. "Postmortem" is the SRE term; "retrospective" is the broader engineering term; "after-action review" is the term from military and emergency-response origins; "incident review" is the neutral alternative. Different organisations favour different words; they refer to the same practice when done well. This chapter uses "postmortem" throughout and does not require the term.

## Motivation — three postmortem failures that re-create the incident

- **The postmortem that blamed a person.** The on-call engineer who missed the alarm is named; the postmortem reads as a performance review. The next time the same failure mode occurs, the next on-call engineer hides their mistake. The organisation lost the chance to learn from the first incident, and it imported a culture that guarantees the next one will take longer to surface. This is the failure Allspaw's article opens with; it is older than AI products and still kills them.
- **The postmortem with 14 action items and no owners.** The retrospective produced a list — "add better monitoring; improve the HITL UX; write a runbook for X; red-team the adjacent surfaces." No dates. No owners. Three months later, zero of the 14 have landed. The team has run the postmortem but has not changed the system; the incident will recur.
- **The postmortem that closed the ticket but not the loop.** The root cause is found; the fix is deployed; the ticket is closed. No fixture was added to the eval suite. The next time a prompt change is made, a dataset change happens, or a judge is updated, the regression is possible again — because the thing that would catch it was never added. On an AI surface this is specifically the failure chapter 06 is built to defeat; the postmortem is the moment where the fixture *must* land, not a side-channel.

Each is a specific governance failure the manager owns the prevention of.

## Core concept 1 — blamelessness as a practice, not a slogan

Blamelessness is often misread as "nobody is responsible." That reading is wrong and corrosive. Blamelessness means:

- The investigation focuses on **systems and incentives**, not on the person who was closest to the failure.
- The person closest to the failure is treated as the **best source of information** about what happened, not as the party to blame.
- The question is "what about the system made this mistake easy to make / hard to detect / hard to prevent" rather than "who screwed up."
- The output is a **system change**, not a punishment.

Allspaw's framing: the person who pushed the broken config is the hero of the postmortem, because their account of what they saw at the moment of decision is the single most valuable data in the room. Punish them and the account is stunted; the organisation loses the information.

### What blamelessness is not

- It is **not** absence of accountability. People are accountable for the system they are part of; the postmortem produces action items with named owners who are expected to deliver.
- It is **not** absence of performance management. Pattern failures — a specific person repeatedly making the same category of mistake across contexts — are a performance conversation on the one-on-one, not a postmortem topic. Mod-108 (performance management) is where this conversation belongs.
- It is **not** a free pass on dangerous behaviour. Wilful bypassing of safety controls, suppression of evidence, or dishonesty about what happened is a disciplinary matter; blamelessness does not shield it.

### The manager's role in holding the blameless standard

The manager is the person in the room with standing to redirect the conversation. Three specific interventions:

- **Reframing blame-language.** "The engineer missed the alarm" → "The alarm was not noticed; let us look at the alarm design, the on-call load that evening, and the acknowledgement UX." The reframing is not a rhetorical trick; it genuinely redirects the investigation to the recoverable surface.
- **Protecting the first speaker.** The person closest to the failure often speaks first. The manager ensures they are thanked for their account, not judged on it, and that the rest of the room follows that tone.
- **Naming hindsight bias.** "Of course you should have seen it" is the exact phrase to flag. Hindsight bias — the tendency to view past events as predictable in retrospect — is the single most common failure mode in postmortem discussion. Woods, Dekker, and Allspaw's writing on hindsight bias is the primary reading.

## Core concept 2 — the postmortem template with AI-specific fields

A generic postmortem template is widely available (Google SRE Book Appendix D, Atlassian, PagerDuty). The AI engineering manager adapts it with AI-specific fields. The composite template has eleven sections.

### Section 1 — summary

Two to four sentences. What happened, who was affected, when, and how it was mitigated. Readable by someone who has never heard of the incident. No jargon, no acronyms the reader will not have.

### Section 2 — incident facts

- **Incident ID** and **severity**.
- **Open / mitigated / closed** timestamps (ISO 8601, UTC).
- **TTD / TTM / MTTR**.
- **Scope of impact**: how many users, which surface, which geographies, which user segments.
- **Harm classification**: concrete user harm (y/n, type); regulated category output (y/n, which); data breach (y/n, scope); security-incident shape (y/n). This field is what triggers the chapter 07 regulator-narrative consideration; the manager fills it carefully.

### Section 3 — timeline

The Scribe's record, curated. Every state change, every decision, every mitigation, every escalation, every external communication. Timestamps. Attributed actions. Terse. The timeline is the source of truth for the rest of the postmortem.

### Section 4 — what went wrong (contributing causes)

A list, not a single "root cause" (the discipline is multi-causal; Allspaw and Dekker are specific about this). Each item is a system-level factor, not a person-level one:

- The alarm existed but was tuned too loose to fire in time.
- The runbook page was six months stale on the rollback target.
- The retrieved-content moderator was not wired on this surface because the launch memo (mod-106 chapter 05) did not catch the gap.
- The prompt-registry entry's regression risk was not flagged in review because the eval suite did not have a fixture for this attack pattern.

Each contributing cause pairs with an action item (section 7) and, where applicable, a fixture (chapter 06).

### Section 5 — what went right

Often skipped; always worth including. What mitigations worked. What monitoring caught what. What the IC did that shortened the response. The team learns what to preserve as much as what to change.

### Section 6 — AI-specific fields (the adaptation)

The fields that differentiate an AI postmortem from a generic one:

- **Model, prompt, judge, dataset versions** at incident time. The Prompt Registry entry (mod-104 chapter 04) and the model snapshot pin uniquely.
- **Recent changes** to any of the above in the window leading up to the incident. "Prompt version X landed at 10:14 UTC; incident onset 10:47 UTC" is the sort of correlation that jumps out of this section.
- **Failure-mode classification** (chapter 02 catalogue) — which of the ten (or product-specific additions) does this incident match? The classification is what the incident corpus (chapter 08) reads in later.
- **Guardrail-stack behaviour** — which classifiers / detectors fired, which did not, which were absent. Score distributions where available.
- **Trace / evidence location** — the pointer to the preserved evidence (chapter 04). Ensures the postmortem reader can find the raw material.
- **Red-team cross-reference** — is this incident a known attack class from the AI Risk / red-team corpus (mod-106 chapter 04)? If yes, how did the finding not yield a preventive control? If no, add to the red-team backlog.
- **Harm classification** (duplicated from section 2 for emphasis): concrete user harm, regulated-category output, data breach, security-incident shape.
- **Regulator-adjacency** — is this incident Article 73-adjacent (chapter 07)? Did counsel review? Is any filing required? The field is a tracked status, not a legal determination; counsel owns the determination.

### Section 7 — action items

A tracked, dated, owned, prioritised list. Chapter 06 is the specific loop; this section is the broader set.

Each item carries:

- **A unique ID** (so follow-up is possible; "AI-23 (3h)" notation works if your tooling supports it).
- **A description** of what will change.
- **A type** — **mitigation** (shipped as part of incident close), **preventive** (the thing that stops this specific failure class), **detection** (the monitor / alarm that would have caught it faster), **response** (the runbook / process change that would have shortened the response), **fixture** (the eval fixture + judge case — chapter 06), **red-team** (the attack-class exploration).
- **An owner** (a named individual, not a team).
- **A target date** (specific; if long, scheduled via the tooling that chases it).
- **A completion signal** (merged PR, dashboard added, runbook page dated, fixture in the registry).

A rule of thumb from mature practice: three to seven action items is healthy. Fourteen is a sign the postmortem is treating the incident as a dumping ground.

### Section 8 — what we almost missed (near-misses)

Specifically: what nearly went worse, and what the lucky save was. "If the response had been 30 minutes later, the moderation classifier would have been saturated and the HITL queue would have fallen over." Near-misses are often more instructive than the incident itself because they point at latent risks.

### Section 9 — hindsight-bias check

A deliberate paragraph where the facilitator asks: "Looking back, which of our 'what went wrong' items look obvious *now* that was *not* obvious *then*?" The point is to name the bias so the subsequent analysis can account for it. Dekker's "The Field Guide to Understanding Human Error" is the deeper reading.

### Section 10 — review and sign-off

- Facilitator.
- Participants (named).
- Manager sign-off.
- Date of next review (chapter 06 — the loop-closed check).

### Section 11 — appendix

- Links to preserved evidence (traces, dashboard snapshots, deploy log).
- Links to related incidents, if applicable (recurring pattern).
- Links to action-item tickets.

## Core concept 3 — a worked skeleton

```markdown
# Postmortem — inc-20261108-01 (direct injection on Agent Surface A)
Severity: SEV2 · Facilitator: @manager · Date reviewed: 2026-11-11

## Summary
On 2026-11-08 between 13:40 and 14:02 UTC, a direct-injection pattern caused Agent Surface A to deviate from its system prompt on ~0.6% of sessions. No external side-effect was confirmed. Mitigation was HITL-tightening and a rollback of the previous day's ingest job.

## Incident facts
- Incident ID: inc-20261108-01
- Severity: SEV2
- Open: 2026-11-08 13:40 UTC · Mitigated: 14:02 UTC · Closed: 2026-11-09 09:00 UTC
- TTD: 7 min · TTM: 22 min · MTTR: 19h 20min
- Scope: Agent Surface A; ~0.6% of sessions in the 20-minute window; English-speaking users; no segment pattern identified.
- Harm classification: no concrete user harm; no regulated-category output; no data breach; no security-incident shape.

## Timeline
- 13:40 UTC — alarm (injection-detector hit-rate >1% over 5 min).
- 13:42 UTC — on-call acked; incident channel opened; roles assigned.
- 13:45 UTC — evidence snapshot taken; traces preserved at s3://evidence/inc-20261108-01.
- 13:47 UTC — IC authorised HITL-tightening on Surface A; applied by applied-ai.
- 13:52 UTC — AI Risk SME paged in; characterised attack class as a known indirect-via-retrieval pattern (ref AIID-INC-NNNN similar).
- 13:58 UTC — HITL-tightening live; detector hit-rate trending down.
- 14:02 UTC — IC confirms mitigation holding; next cadence 14:20.
- 15:30 UTC — Rollback of the previous day's ingest job completed; detector hit-rate returned to baseline.
- 22:30 UTC — IC declares incident mitigated.
- 2026-11-09 09:00 UTC — IC declares incident closed; postmortem scheduled.

## What went wrong (contributing causes)
- The ingest job from 2026-11-07 included a corpus whose pre-ingest moderation was disabled because of a performance regression in the ingest pipeline reverted two weeks prior; the revert reverted the moderation too.
- The retrieved-content moderator was wired on Surface A for user-provided content but not for RAG-ingested content; the chapter 06 of mod-106 red-team brief had surfaced this gap and the fix was in the backlog.
- The injection-detector alarm threshold was tuned to a wide window (5 min); the first malicious session landed at 13:33 UTC but did not cross the threshold until 13:40.

## What went right
- Evidence was preserved before mitigation; the traces were complete and usable.
- The runbook page for direct injection was current and used; roles were assigned within 2 minutes.
- Comms Lead ran the cadence; stakeholders had a status-page update at 14:10 UTC.

## AI-specific fields
- Model: `anthropic/claude-sonnet-X-snapshot-YYYYMMDD` (pinned).
- System prompt: version `sys-agent-a-v12` (unchanged in the window).
- Judge: `judge-rag-faithfulness-v4` (unchanged).
- Dataset: eval dataset `agent-a-eval-v7` (unchanged).
- Recent changes: ingest job `ingest-a-20261107-A` (corpus added, +42k docs).
- Failure-mode: indirect prompt injection via RAG (chapter 02 mode 3) — not direct injection despite initial classification; postmortem updates the classification.
- Guardrails: input moderator fired on 0 of 30 adversarial sessions; retrieved-content moderator absent; output moderator caught 6 of 30 at low confidence, below the auto-block threshold.
- Evidence: s3://evidence/inc-20261108-01/.
- Red-team cross-reference: finding RT-2026-Q3-08 ("retrieved-content injection via ingest"), accepted-risk entry AR-2026-05, backlog ticket GUARD-142 (open).
- Harm classification: no concrete user harm; no regulated-category output; no data breach.
- Regulator-adjacency: counsel briefed on 2026-11-09 09:30 UTC; no Article 73 threshold triggered; no filing required; closed for Article 73 purposes.

## Action items

| ID | Type | Description | Owner | Target | Signal |
|---|---|---|---|---|---|
| AI-PM-001 | fixture | Add 30-case injection fixture (chapter 06 of mod-107) to eval registry at `agent-a-safety/injection/rag-ingest/v1`; wire to pre-merge release gate; add judge case `injection-rag-ingest-v1` to judge registry. | @alice (applied-ai) | 2026-11-20 | Fixture merged + release gate enforcing + first green run |
| AI-PM-002 | preventive | Wire retrieved-content moderator on Surface A, parity with input moderator thresholds. | @frank (platform) | 2026-11-25 | Moderator live; dashboard reports non-zero score events |
| AI-PM-003 | preventive | Re-enable the ingest-pipeline pre-ingest moderation that was reverted; re-benchmark the perf regression. | @frank (platform) | 2026-11-30 | Pre-ingest moderation live; ingest-job-duration benchmarks within SLA |
| AI-PM-004 | detection | Tighten injection-detector alarm window (5m → 2m); add per-tenant-rate signal. | @carol (sre) | 2026-11-18 | Alarm tuned; tested with synthetic injection; dry-run after-action |
| AI-PM-005 | response | Update runbook-direct-injection.md to add the indirect-via-RAG branch; add rollback target for ingest-job. | @hannah (on-call) | 2026-11-15 | Page re-reviewed; last-reviewed date set |
| AI-PM-006 | red-team | Commission AI Risk engagement to characterise the full indirect-injection attack class via ingest pipeline on all surfaces (ref RT-2026-Q4-02). | @manager | 2026-12-10 | Engagement briefed; findings tracked |

## What we almost missed
- The injection pattern was active for ~7 minutes before the alarm fired. If the pattern had been higher-volume, output-moderator saturation could have followed. The alarm-window tuning (AI-PM-004) is the preventive here.
- One trace in the preserved set showed a near-successful data-egress attempt via a tool the agent did not have scope for. The scope enforcement held; the scope-check itself is the last line of defence. If the scope had been one tool wider, the incident would have been SEV1.

## Hindsight-bias check
The retrieved-content moderator gap was in the red-team backlog as RT-2026-Q3-08 with an AR-2026-05 accepted-risk entry; its absence was known. The postmortem's "we should have wired it" is true but was also known; the correct learning is about the *accepted-risk expiry discipline* and the *ingest-pipeline revert review* — not about the one engineer who pushed the ingest revert.

## Review and sign-off
- Facilitator: @manager
- Participants: @bob (IC), @carol (Comms), @dan (Scribe), @alice, @eve, @frank, @hannah
- Manager sign-off: @manager, 2026-11-11
- Next review: 2026-11-25 (chapter 06 loop-closed check for AI-PM-001)

## Appendix
- Evidence: s3://evidence/inc-20261108-01/
- Related: inc-20260814-02 (distant pattern, different surface).
- Action-item tracker: [tickets]
```

The skeleton is deliberately full. A postmortem that fills every section honestly at this depth is roughly 90 minutes of facilitation plus two to three hours of pre-work on the timeline, the AI-specific fields, and the action items. Postmortems that take less are often skipping section 6 (the AI-specific fields) or section 7 (the action items). Both are the point.

## Core concept 4 — facilitation discipline

The facilitator (typically the manager; sometimes a peer manager on larger incidents) runs the meeting. Specific skills:

- **Prep the timeline before the meeting.** The Scribe's record is raw material; the facilitator curates it into a clean timeline and shares it 24 hours ahead so the room does not spend 40 minutes reconstructing what happened.
- **Open with the summary and the harm classification.** Set altitude. The room is here to understand and change the system, not to re-live the incident.
- **Run the contributing-causes discussion with a specific question.** "What about the system made this mistake easy to make / hard to detect / hard to prevent?" repeated as needed. The question keeps the discussion on the system.
- **Watch for blame-language and redirect.** Specifically watch for passive-voice-but-attributed ("the config was pushed by..."); it is often blame in disguise.
- **Enforce the action-item discipline.** Every action item has an owner by the end of the meeting; no "someone should" items. The facilitator's acceptance criterion is: can I read this list in a month and chase every one?
- **Protect time for the AI-specific-fields section.** The team will want to spend time on familiar terrain (the timeline, the mitigation). The manager protects 15 minutes for section 6 explicitly.
- **Close with the loop-closed commitment.** "The fixture action item (AI-PM-001) is tracked. The next postmortem review (2026-11-25) will check that the fixture is in the registry and the gate is enforcing. We are not done with this incident until the fixture lands."

## Core concept 5 — action-item maturity and the review cadence

Action items are only valuable if they close. Three disciplines.

### Discipline 1 — the action-item register

Every postmortem's action items land in a single, team-wide register — a Jira / Linear project, a GitHub-project, or an engineering tracker. The register has:

- The item ID, description, type, owner, target date, completion signal.
- A status (open / in progress / blocked / closed).
- A link to the source postmortem.

The register is the authoritative place. Items not in the register do not exist.

### Discipline 2 — the weekly / bi-weekly chase

The manager (or a delegated action-item owner) reviews the register weekly or bi-weekly. Items past due are either re-committed to a new date (with a reason for the slip) or escalated. Slipping an action item twice is a conversation. Items blocked are examined: the blocker is often itself an action item worth tracking.

### Discipline 3 — the quarterly aggregation

Quarterly, the manager aggregates action items across postmortems: how many items were closed on time, how many slipped, which types (mitigation / preventive / detection / response / fixture / red-team) are over- or under-represented. Patterns:

- A quarter with lots of **detection** items and few **preventive** items means the team is chasing monitors rather than root causes.
- A quarter with lots of **fixture** items closed means the incident-to-fixture loop is running; a quarter with fixture items opened-but-not-closed is a red flag.
- A quarter with lots of **red-team** items from postmortems suggests the red-team cadence is finding attacks only after incidents; mod-106 chapter 04 has the preventive cadence.

The quarterly aggregation is an input to mod-109 (operating cadence) and mod-108 (performance reviews — team-level growth signals).

## Core concept 6 — review cadence and ritual

A postmortem is a ritual. The ritual has beats.

- **Within 24 hours of close**: facilitator pre-work on the timeline.
- **Within 3–5 days of close** (not sooner, to allow fatigue to pass; not later, to allow memory to be fresh): the postmortem meeting.
- **Within 7 days of close**: the written postmortem posted; sign-off from the manager; action items in the register.
- **At the next postmortem review (bi-weekly / monthly)**: action-item status reviewed.
- **At the loop-closed milestone** (chapter 06): the fixture-action-item is verified in the registry; the incident is "loop closed."

The cadence is boring on purpose. Teams that respect the cadence have postmortems that produce system change; teams that let the cadence slip have postmortems that produce documents.

## Core concept 7 — anti-patterns

Five failure modes the discipline rules out.

- **The postmortem that blamed a person.** Addressed by the blameless practice; the manager's facilitation is the enforcement.
- **The postmortem with no fixture.** Chapter 06's loop is the structural fix; section 7's action-item type "fixture" is the specific item.
- **The postmortem that was never written.** Addressed by the "within 7 days" cadence beat; a postmortem not written in 7 days is called out in the operational review.
- **The postmortem that was written and then forgotten.** Addressed by the register and the weekly chase. Items not reviewed are items not closed.
- **The postmortem that was a dumping ground of 14 action items.** Addressed by the facilitator's acceptance criterion — can I read this list in a month and chase every one? Items that cannot pass that test are either consolidated or dropped (with the reasoning noted).

## What good looks like

- Every confirmed incident in the last twelve months has a written postmortem using the eleven-section template.
- Every postmortem has three to seven action items, each with an owner and a date.
- Every postmortem has at least one **fixture** action item (chapter 06).
- Action-item close rate is ≥ 80% by target date; slippage is reviewed and re-committed.
- The quarterly aggregation shows a mix of mitigation / preventive / detection / response / fixture / red-team items, with the type distribution matching the team's incident shape.
- Section 6 (AI-specific fields) is filled; model, prompt, judge, dataset versions are pinned; failure-mode classification maps to chapter 02; red-team cross-reference is present where applicable.

## What bad looks like

- Postmortems are oral — "we discussed it in the retro."
- Action items have no owners, or have teams-as-owners ("the AI team will do X").
- Fixtures are never action items; the eval registry does not grow from incidents.
- The AI-specific-fields section is absent or filled generically; the model / prompt / judge versions are not pinned.
- The same failure mode recurs across quarters because the previous postmortem's preventive action item was not closed.
- The facilitator was the on-call engineer who was closest to the failure; the room spent 20 minutes being hard on them.

## Summary

- Blameless postmortems are the single most durable practice SRE has produced; AI products use the same practice with AI-specific fields and a mandatory loop into the eval suite.
- The eleven-section template covers summary, facts, timeline, contributing causes, what went right, AI-specific fields, action items, near-misses, hindsight-bias check, sign-off, appendix.
- The AI-specific fields (section 6) pin the versions, classify the failure mode, record the guardrail behaviour, cross-reference the red-team corpus, and name the harm / regulator-adjacency status.
- Action items are typed, owned, dated; every postmortem has at least one fixture item; the register and the weekly chase keep them honest; the quarterly aggregation surfaces patterns.
- Facilitation discipline protects the blameless frame, redirects blame-language, protects time for the AI-specific-fields section, and closes with the loop-closed commitment.
- The review cadence (24h pre-work, 3–5 day meeting, 7-day write-up, bi-weekly register review, loop-closed milestone) is boring on purpose.

The next chapter picks up the loop the postmortem commits to — the mechanics by which every confirmed incident becomes a permanent eval fixture and judge case.
