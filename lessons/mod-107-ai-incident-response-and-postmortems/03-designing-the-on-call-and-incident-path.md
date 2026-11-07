# Designing the On-Call and Incident Path

The ten failure modes of chapter 02 are only incidents if someone is paged on them. Without a wired on-call system, each failure mode is a tweet, a customer-ticket pattern, or an after-the-fact discovery — and each hour the system misbehaves before anyone notices is an hour of user harm and preserved evidence rotting out of the log retention window. The on-call system is the mechanism by which the catalogue of failure modes becomes a set of incidents that get run through incident command (chapter 04), postmortemed (chapter 05), and converted into fixtures (chapter 06).

This chapter is the design of that system, at the AI engineering manager's altitude. The SRE substrate — paging, monitoring, observability, runbook platform, incident-management tooling — belongs to the SRE / Platform function. The manager runs the AI-adapted layer on top: the AI-specific severity schema, the AI-shaped observability stack, the runbook library, the four mitigation levers, and the dry-run cadence. The craft of writing a specific monitor query or wiring a specific kill-switch belongs to the ICs; requiring that *they exist and that they fire* is the manager's.

The Google SRE Book and the Google SRE Workbook are the primary sources for the discipline this chapter adapts. Both are free online and should be read by any AI engineering manager who has not yet. This chapter does not re-derive them; it extends them.

## Motivation — three on-call shapes that fail on the AI surface

- **The on-call rotation that only knows "service down".** The rotation was designed for API uptime, 5xx rates, and p99 latency. The alert thresholds were tuned against those signals. When the moderation classifier starts false-positiving at 30%, the API is "up" and the thresholds are "green"; nobody is paged. The on-call rotation is not the problem; the signals it pages on are. AI-specific failure modes need AI-specific signals, and the manager has to require their existence or import them from whoever already owns the eval-online pipeline.
- **The runbook library that was written three years ago.** A runbook exists for "prompt-injection incident" — six bullet points, authored during a security tabletop, never re-read, never dry-run. The on-call engineer opens the page during the incident, reads the "page the AI risk lead" bullet, discovers that the named person has left the organisation, and falls back to improvisation. The library existed; its freshness did not.
- **The pager that nobody respects.** The rotation pages on noisy signals three times a week; the on-call engineer has learned to acknowledge-and-ignore. When a real incident fires on the same signal, the response is 45 minutes late. This is a tuning problem in the classical SRE sense and a confidence problem in the human sense. On the AI surface it compounds: AI signals are often *distribution-shaped* (something is slowly getting worse) rather than *boolean* (something broke), and the tuning that distinguishes signal from noise takes months of feedback. The manager's role is to protect the time budget for the tuning work.

Each of the three is a system failure, not a tool failure. Buying a better paging product fixes none of them.

## Core concept 1 — severity tiers for AI incidents

The classic SRE severity schema (SEV1 / SEV2 / SEV3, or P0 / P1 / P2) still applies; the trigger conditions differ. Four tiers worth being explicit about, with the AI-specific triggers that place an incident into each.

### SEV1 — "all hands, exec involved"

An incident is SEV1 if **any** of the following is true:

- A user has suffered concrete harm (financial, medical, physical, or significant privacy harm) traceable to the product's output.
- The product output has caused a **data breach** (user data has been disclosed to a party outside the privacy-notice scope).
- The product is actively producing content in a prohibited category (CSAM, direct incitement, specific harms-against-persons output) and the mitigation is not holding.
- An attacker has successfully altered the product's behaviour at scale (prompt-registry compromise, embedding-store poisoning at scale, successful supply-chain attack against the model vendor with product-side impact).
- A regulator or significant press outlet is asking about the incident in a way that suggests an Article 73-style disclosure obligation.

SEV1 pages the manager, the engineering director, the AI risk lead, counsel, security operations / DFIR, and customer-communications. Executive involvement is the default. The incident commander runs the room; the manager's job is to keep the response coherent across the involved specialties (chapter 04).

### SEV2 — "incident channel open, mitigation in progress"

An incident is SEV2 if:

- The product is producing incorrect or disallowed outputs at a rate that materially affects users, but no user-side concrete harm has been confirmed.
- A critical guardrail is down or degraded (moderation classifier returning errors, HITL queue backed up past SLA, retrieval system returning no results on a critical surface).
- A tool-use failure is causing side effects on external systems (unwanted emails, writes to the wrong records, cost-blast on a tool).
- A model-vendor regression is causing a widespread user-visible quality drop.

SEV2 pages the on-call, convenes an incident channel, and runs incident command without exec involvement unless escalated. The manager is informed and may attend.

### SEV3 — "track and fix, no page"

An incident is SEV3 if:

- The product is misbehaving on a narrow slice (one language, one persona, one tool path) with no scaled user harm.
- A monitor has drifted (judge-drift, slow retrieval-coverage decline) but has not crossed the page-worthy threshold.
- A single-user complaint has surfaced a plausible reproduction path that is not evidently widespread.

SEV3 is a ticket, not a page. The manager reviews SEV3 patterns weekly; a cluster of SEV3s on one surface is often a SEV2-in-waiting.

### SEV4 — "logged, not acted on yet"

Known-issue-style entries that do not warrant work but are worth tracking. Rarely used; the risk is that SEV4 becomes a dumping ground (chapter 05 anti-patterns). Accepted-risk entries from the release triad (mod-106 chapter 05) sometimes live here until their expiry.

### A specific trigger table

The severity schema is only useful if the triggers are specific enough that the on-call does not have to guess. Pin the following table to the runbook library index:

| Failure mode | SEV1 trigger | SEV2 trigger | SEV3 trigger |
|---|---|---|---|
| Jailbreak | Output in prohibited category *and* mitigation not holding | Output in disallowed-but-not-prohibited category, scaled | Narrow path, no user harm confirmed |
| Direct injection | Injection + external side-effect (data exfiltration, unwanted action at scale) | Injection without external side-effect, scaled | Narrow pattern, caught before action |
| Indirect injection | Multi-tenant impact confirmed | Injection vector reproducible, single tenant affected | Pattern identified in test environment |
| Hallucination with harm | User-side concrete harm (medical, financial, legal) traceable to output | Scaled hallucination affecting a product surface | Single-case report, no action-taken |
| Model-vendor regression | Regression + concrete user harm | Regression affecting a significant slice | Regression on narrow slice, recoverable |
| Judge drift | Drift invalidating a regulator-facing eval claim | Drift crossing gate threshold on multiple features | Drift detected, control-set in progress |
| Tool-use failure | Side-effect at scale (data egress, external writes) | Side-effect on a single tenant, scaled misuse | Isolated misfire |
| Embedding corruption | Adversarial cause (security incident) | Non-adversarial corruption, user-visible | Partial-ingest corruption detected pre-release |
| Retrieval degradation | Prolonged outage of critical surface | Degraded latency / coverage, user-visible | Brief degradation, SLO budget intact |
| False-positive storm | Harm-category refusals (crisis handoff, safety-critical) | Scaled refusal rate on legitimate traffic | Narrow refusal pattern |

The table does not replace judgment. It gives the on-call a starting point so that judgment is applied *on top of* the floor, not *instead of* a floor.

## Core concept 2 — the AI-shaped observability stack

An AI product's observability needs are the sum of a normal web-service's needs plus a layer specific to the AI surface. The manager requires the AI-specific layer. Four categories.

### Category A — model-quality signals

Running evaluations against live traffic (sampled or shadow) and surfacing their scores as time-series. Mod-104 chapter 06 is the design. The manager requires:

- A **production online-eval pipeline** emitting at least: faithfulness (for RAG), refusal rate, judge-score distribution per surface, and sampled-online human-agreement rate. Dashboards per surface, alarms on distribution shift.
- **Slice stratification** — the online eval reports per-surface, per-language, and per-user-segment. A regression on a slice is invisible in the aggregate.
- A **control-set replay** for judge stability (mod-104 chapter 04). If the control set moves, the judge drifted, not the system.

### Category B — safety-guardrail signals

The guardrail stack (mod-106 chapter 03) emits its own signals. The manager requires:

- **Classifier error rate** and **classifier-score distribution** per surface.
- **Refusal rate** per surface, split by category.
- **HITL queue depth** per workflow, with SLA alarms.
- **Prompt-injection detector hit rate** per surface.
- **Tool-call-moderation hit rate** for agentic surfaces.

### Category C — agentic / tool-use signals

For agentic surfaces (mod-106 chapter 07), beyond the guardrail signals:

- **Tool-call error rate** per tool (invalid schema, tool-not-found, downstream error).
- **Tool-call chain-length distribution** per workflow.
- **Tool-call cost per session** with per-tenant circuit breakers.
- **Downstream-system error correlation** — the external systems the agent writes to, correlated against the agent's call volume.

### Category D — operational / infrastructure signals

The conventional SRE stack adapted:

- **Model-provider latency / error / rate-limit** signals, per provider, per model.
- **Retrieval-tier latency / coverage** signals.
- **Embedding-service availability** and **ingest-job health**.
- **Prompt-registry change events** (chapter 05 requires this — "did a prompt change immediately before the incident?" needs an answer in under a minute).
- **Model-version change events** (same question for model migrations).

The four categories together are the AI-adapted observability stack. The SRE / Platform function builds and operates the stack; the manager requires that each category is *present* and that the alarms on the thresholds that should page actually fire.

## Core concept 3 — the four mitigation levers

The Incident Commander (chapter 04) needs a small, memorable set of levers. Four. In priority order (fastest-to-safest to slowest-but-most-correct).

### Lever 1 — kill-switch

The feature, the surface, or the tool goes off. Users see a degraded mode (a feature flag disables the AI path) or a fallback human-staffed route. Kill-switches must be **pre-wired** at the surface level and the tool level; demanding one be built mid-incident is a failed design. Mod-106 chapter 07 is specific about tool-level kill-switches; the surface-level one is the feature flag the launch memo (mod-106 chapter 05) committed to.

- Pre-condition: the kill-switch is tested quarterly in a dry-run.
- When to use: the output is causing concrete harm, or the attack is still active and the team does not yet understand it.
- Trade-off: the feature is unavailable. Communicate the degraded mode (chapter 04 communications lead).

### Lever 2 — route-to-safer-model

The surface is kept live, but the model is swapped to a safer / more-refusal-prone / less-vulnerable one. Requires a pre-wired fallback (mod-105 chapter 04) and evidence that the fallback does not have the same vulnerability. Fastest meaningful mitigation when the attack is model-specific or the vulnerability is model-shape-specific.

- Pre-condition: the fallback is kept warm; parity evals exist against the primary; the routing is a single switch.
- When to use: jailbreaks or injections that are model-specific, provider-side regressions, model-generation vulnerabilities (GCG-class attacks against a specific family).
- Trade-off: the fallback often costs more, is slower, or has different qualitative behaviour. Communicate internally and (where visible) externally.

### Lever 3 — rollback

Rollback of the recent change that introduced the regression. Requires versioned registries (mod-104 chapter 04 — prompt registry, judge registry, dataset registry) and a disciplined deploy history. Rollback targets by artefact type:

- Prompt change → rollback to the prior prompt version in the registry.
- Judge change → rollback the judge version; re-enable the prior judge.
- Model snapshot → pin back to the prior snapshot.
- Classifier threshold → revert to the prior threshold.
- Retrieval index → restore the prior snapshot (requires backups; require them).
- Tool schema or allow-list → revert to the prior config.

- Pre-condition: every artefact type is versioned; every deploy touches one artefact type at a time where possible (so the rollback is scoped).
- When to use: a recent change correlates with the onset of the incident; the change is reversible without loss of other work.
- Trade-off: rollback can lose work that was not causing the incident. Rollback is reversible; prefer it over destructive mitigations.

### Lever 4 — HITL-tightening

Route high-stakes traffic through human review while the fix is engineered. Requires a HITL queue (mod-106 chapter 02 section 5), a competent reviewer rotation, and the capacity to absorb the traffic.

- Pre-condition: HITL capacity exists (chapter 04); the queue has a surge plan.
- When to use: the failure is slow-and-silent (judge drift, hallucination-with-harm), the attack class is narrow, or the alternative is a user-facing kill-switch that is too disruptive.
- Trade-off: HITL has a throughput ceiling and a latency floor. The storm of chapter 02 mode 10 is precisely a HITL-saturation event.

### How the levers compose

Levers combine. A typical SEV2 incident response:

1. **Lever 1 (kill-switch)** on the narrowest scope that stops the harm (e.g., disable the one tool).
2. **Lever 4 (HITL-tightening)** on the surface that is adjacent.
3. **Lever 3 (rollback)** on any recent change that correlates.
4. Investigate, patch, re-enable.

A typical SEV1 incident response puts Lever 1 wider (surface-level), Lever 2 to a safer model on the surrounding surfaces, and keeps Lever 4 on everything adjacent while root-cause work proceeds in parallel.

## Core concept 4 — the runbook library

Every failure mode in chapter 02's catalogue has a runbook page. Every runbook page has the same structure. The manager's job is to require the structure and the coverage; the IC's job is to write the content.

### Runbook page anatomy

One page per failure mode (plus product-specific failure modes your catalogue adds). Sections:

1. **Signature.** How the on-call recognises the shape (one paragraph; borrows from chapter 02).
2. **Severity-tier table.** The row from the chapter 02 severity table, with any product-specific trigger adjustments.
3. **First response, first 15 minutes.** The acknowledgement, the incident-channel open, the IC-assignment, the comms-lead call-out. What to say in the channel opening message. Who to page for help.
4. **Evidence preservation.** The specific logs / traces / payloads to snapshot immediately, with the commands / dashboard-paths. Preserve-before-mitigate; the next section's mitigations will often destroy evidence.
5. **Mitigation.** The specific lever(s) for this failure mode, the commands / dashboards, the toggle names, the order.
6. **Communication templates.** Internal (incident channel), customer-facing (if applicable), status-page language, exec-briefing summary. Pre-drafted, with placeholders.
7. **Escalation matrix.** Who gets paged / looped in at each severity tier — AI Risk lead, security operations, counsel, customer-communications, specific exec owners.
8. **Root-cause anchors.** The three questions the postmortem will want answered. Pre-staged so the IC can start answering them in the moment.
9. **Rollback target.** The specific prior version(s) to rollback to.
10. **Known-adjacent failure modes.** Links to the runbook pages for failure modes that often co-occur (jailbreak + injection, retrieval degradation + hallucination, tool-use failure + injection).

Pages are living documents. Each page has a `last-reviewed` date and a `last-dry-run` date. A page older than six months without a review is a page the manager pulls back into scope.

### A runbook page skeleton

```markdown
# Runbook — Direct Prompt Injection Incident
Last reviewed: 2026-11-10 · Last dry-run: 2026-09-18 · Owner: <manager>

## Signature
The model follows user-provided instructions that should have been treated as content: "ignore previous instructions", "you are now ...", or subtle instruction-hierarchy violations. Output deviates from the system prompt.

## Severity
- SEV1 if injection caused external side-effect at scale (data egress, action taken on behalf of multiple users).
- SEV2 if injection reproducible and output deviates for a surface in production.
- SEV3 if injection pattern reported but single-case, no side-effect.

## First response (first 15 minutes)
- Ack the page in PagerDuty.
- Open #inc-<yyyymmdd>-<slug> in Slack. Pin this runbook.
- Assign roles: IC, Comms, Scribe, Subject-Matter (Applied AI).
- Page AI Risk lead; page security on SEV1.
- Opening message: "Confirmed / investigating a direct-injection pattern on <surface>. Preserving evidence. ETA for next update: 15 min."

## Evidence preservation (first)
- Snapshot the suspect traces to <evidence-bucket>/inc-<yyyymmdd>: run `preserve-traces.sh --surface=<x> --window=<T-2h..T+5m>`
- Snapshot the injection-detector logs for the same window.
- Snapshot the prompt-registry current version and the previous version.
- Note the model snapshot and classifier versions.

## Mitigation
1. If side-effect at scale → **kill-switch** on the affected tool(s); feature-flag `agent.tool.<name>.enabled = false`.
2. **HITL-tightening**: feature-flag `moderation.input.threshold` to <tightened value>.
3. If a recent prompt change correlates → **rollback** the system prompt to <prior version>.
4. If injection is model-shape-specific → **route-to-safer-model** on the affected surface (`model.route.<surface>` to fallback).

## Communication
- Internal channel: update every 20 min with: scope, mitigation, ETA, blockers.
- Customer-facing (SEV1 or scaled SEV2): coordinate with <comms-partner>; template at <link>.
- Status page: "We are investigating a safety issue affecting <surface>; <feature> is temporarily unavailable."

## Escalation
- SEV3: ticket; AI Risk lead notified.
- SEV2: page on-call + AI Risk lead; manager in the loop.
- SEV1: exec paged, counsel paged, security on-call paged, customer-comms paged.

## Root-cause anchors (postmortem seeds)
- Was the injection pattern a known one in the red-team corpus, or novel?
- Did the input-filter / detector fire? If not, why not? If yes, why was the output still returned?
- Did any recent prompt / detector / threshold change contribute?

## Rollback target
- Last clean prompt version: <ref>
- Last clean detector threshold: <value>

## Related runbooks
- [Indirect prompt injection](runbook-indirect-injection.md)
- [Tool-use failure (injection-driven)](runbook-tool-use-failure.md)
```

The page is boring on purpose. A page an on-call engineer can read in 90 seconds is a page they will actually read during an incident.

## Core concept 5 — the on-call rotation and the dry-run cadence

The rotation is the people side of the on-call system. Three disciplines matter.

### Discipline 1 — competence coverage

The AI on-call rotation is often *additional* to the SRE / Platform on-call, not instead of it. Model two roles:

- **Primary on-call (SRE-shape).** Pages on infrastructure and operational signals (API error rate, latency, provider outages, retrieval-tier health). Responds to the SRE-classic shapes. Owned by Platform / SRE.
- **AI on-call (AI-shape).** Pages on AI-specific signals (eval-online drift, classifier anomalies, judge drift, tool-use misfires, injection-detector hits). The responder is either a dedicated AI-on-call rotation or the primary on-call *plus* a named "AI subject-matter escalation" — either works; the manager picks and documents.

A team of six AI engineers plus a shared SRE rotation often runs the SME-escalation model. A team of twelve plus dedicated AI-side signals often runs a secondary AI-on-call rotation. Match the shape to the team's size and the surface's severity.

### Discipline 2 — the dry-run cadence

A runbook page that has never been exercised is a page the team has not confirmed works. The manager runs:

- **Tabletop exercises** — quarterly, 90 minutes, scenario-driven. Pick a failure mode from chapter 02, walk the runbook, identify gaps. The tabletop is cheap; the cost of not running it is paid in the first real incident of that shape.
- **Dry-runs** — annually per critical runbook page, or after a material change. Actually touch the kill-switch, actually run the rollback, actually open the incident channel, actually page the (pre-warned) stakeholders. Produces an after-action with concrete gaps.
- **Game days / chaos days** — where the organisation's maturity supports it, inject a controlled failure into a non-production path (e.g., simulate a classifier returning random scores for 15 minutes in staging) and run the response. Borrowed from SRE game-day practice. Pre-communicate; coordinate with security; never run against production without extraordinary rigor.

Tabletops and dry-runs are the single highest-leverage investment the manager can make on the incident-response surface. A team that has tabletopped the ten failure modes once each is an order of magnitude more effective in its first real incident than a team that has not.

### Discipline 3 — on-call health

On-call is work. The manager tracks:

- **Page volume per rotation period** — is the rotation fair, sustainable, and appropriately noisy?
- **Noise rate** (acks-without-action / total pages) — a persistently noisy signal is one to tune (reduce threshold-triggering) or remove.
- **Handoff quality** — the rotation hands off open incidents, known-watchpoints, and in-flight investigations at every rotation boundary.
- **Burnout signals** — on-call duty is a frequent source of attrition; the manager watches for over-rotation, no-time-for-primary-work patterns, and compensation / time-off-in-lieu policy gaps. Partner with SRE / Platform on the structural answer.

Mod-109 (team shape and operating cadence) covers the rotation design at a deeper altitude. The on-call health signals *belong* to mod-109; chapter 03 of this module requires that they *exist* and that the AI-side rotation is part of them.

## Core concept 6 — what the AI engineering manager owns vs. what Platform / SRE owns

A specific split, because the shared surface is where most friction lives.

| What Platform / SRE owns | What the AI engineering manager owns |
|---|---|
| The paging system (PagerDuty, Opsgenie), the monitoring and observability stack, the runbook platform (Confluence, Notion, a repo), the incident-management tooling (incident channels, Statuspage), the on-call compensation policy. | The AI-specific **signals** (categories A–D above) that need to flow into the stack, and the alarm thresholds specific to the AI surface. |
| The service-level indicator / objective design discipline, for infrastructure services. The error-budget practice. The operational-review cadence. | The service-level objectives for **AI output quality** (faithfulness / refusal / judge-score distributions), negotiated with Product. The eval-regression-alarm policy. |
| The on-call rotation shape at the organisational level — who rotates, when, with what compensation. | Who the **AI-side escalation** is on the rotation, what SMEs are reachable, what AI-specific training / onboarding the on-call engineer needs. |
| The incident-management process at the organisational level — severity schema, incident-command practice, postmortem ritual, action-item tracking tooling. | The **AI-specific adaptations**: failure-mode catalogue, severity triggers, AI-specific mitigation levers, AI-specific postmortem fields (chapter 05). |
| The runbook platform itself. The content standards and templates. | The **content** of each AI-specific runbook page, authored by the on-call engineer against the chapter-02 catalogue, reviewed by AI Risk where relevant. |

The pattern: the AI engineering manager plugs into the SRE substrate. The manager does not build the substrate; the manager does not operate around it. The partnership is specific enough to be funded (chapter 03 of mod-110 — budget for the AI-side signals is a FinOps line item) and discussed at the operational review (mod-109 cadence).

## Core concept 7 — anti-patterns the design defeats

Four failure modes the on-call design is meant to rule out.

- **The signal-free surface.** A product is live on top of a provider API; the SRE substrate pages on 5xx and latency; nothing else fires. The team learns about incidents from customer tickets. Prevention: the four categories of AI-shaped signals (core concept 2) are present and wired to the pager at severity-appropriate thresholds.
- **The library that was written once.** Runbook pages exist but are stale, unreviewed, and have never been dry-run. Prevention: every page carries a `last-reviewed` and `last-dry-run` date; quarterly tabletops are on the operational calendar; a page older than six months without review is in-scope for the next quarter.
- **The one-engineer system.** One senior engineer knows how to respond; everyone else defers. The engineer gets sick, leaves, or goes on parental leave; the first incident during that window takes four times as long to mitigate. Prevention: the dry-run cadence rotates the IC role across the team; runbook pages are written so a less-experienced engineer can execute them.
- **The pager that nobody respects.** Noisy signals train the on-call rotation to ignore pages. Prevention: a monthly noise-tuning review; signals that persistently page without action get re-tuned or removed. Partner with SRE on the practice; the AI engineering manager watches the AI-specific signals for this specifically.

## Summary

- The on-call system is the mechanism by which the ten AI-specific failure modes become incidents. Without it, failure modes are tweets.
- The severity schema (SEV1 / SEV2 / SEV3 / SEV4) extends classic SRE practice with AI-specific triggers — a specific trigger table per failure mode (chapter 02) lives alongside the runbook library.
- The AI-shaped observability stack has four categories: model-quality signals, safety-guardrail signals, agentic / tool-use signals, operational / infrastructure signals. The manager requires that each category is wired.
- The Incident Commander draws from four mitigation levers in priority order: kill-switch, route-to-safer-model, rollback, HITL-tightening. They compose; the composition is in the runbook pages.
- The runbook library is one page per failure mode, with a fixed ten-section anatomy; each page has a `last-reviewed` and `last-dry-run` date; quarterly tabletops and annual dry-runs are the discipline that keeps the library honest.
- The AI engineering manager plugs into the SRE substrate rather than replacing it. The specific signals, thresholds, severity triggers, and runbook content are the AI-side; the paging, observability, and incident-management tooling are the SRE side.
- The four anti-patterns (signal-free surface, stale library, one-engineer system, pager nobody respects) have specific preventive disciplines.

The next chapter takes the design to the moment of an in-flight incident: the Incident Commander system, TTD / TTM, evidence preservation, and the escalation to governance when a harm threshold is crossed.
