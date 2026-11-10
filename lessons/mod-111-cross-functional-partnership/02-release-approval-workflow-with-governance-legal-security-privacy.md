# Designing the Release-Approval Workflow — Governance, Legal, Security, Privacy

A GA release of a user-facing AI feature crosses six desks, not one. Engineering, AI Governance, Legal, Security, Privacy, Product — each with its own evidence appetite, cycle time, and idea of what "ready" means. If the manager treats the release as a single-desk event ("we hit the eval gate, we ship"), the first production escalation is already shaped. A privacy office that has never seen the data-flow diagram will first see it on launch morning and demand two weeks to assess it.

The manager's job is to make that invisible crossing visible — to know, for every desk, which evidence it consumes, how long it needs, who signs off, and which inputs the manager owes versus which outputs are the partner's problem. A manager who cannot name the six partners and their artefacts has a release schedule that is a work of fiction — a PRD date that will slip between two weeks and a quarter when the first partner surfaces a concern.

## Motivation — the four shapes of a release that crossed the wrong desk last

Four failure shapes recur across teams that ship AI features without a designed release-approval workflow. Each ends in a slip, an override, or an incident that mod-107 would have inherited.

- **The "ship on Friday" release where privacy had never seen the data flow.** The feature indexes customer-support tickets. Engineering has clean eval-gate numbers, judge rubrics, retrieval metrics. On Thursday the manager loops in privacy "for a heads-up." Privacy asks: what personal data classes land in the retrieval store? What's the retention policy? Which sub-processor hosts the embeddings? Can a customer exercise GDPR Article 15 on their indexed content? None of these answers exist. Launch slips three weeks while the data-flow diagram is drawn, the DPIA inputs assembled, and a DSR runbook written. Attributable to the manager not knowing when privacy needed to engage.
- **The DPIA that arrived two days before launch and demanded a scope change.** Privacy *did* engage — four weeks out. They produced a finding that the feature's use of customer email bodies for retrieval exceeds the lawful basis declared for that data class. Remediation is either a lawful-basis change (customer-facing notice update — ten business days) or a scope change that excludes email bodies (which breaks the quality numbers). Both miss launch. Not a privacy failure; a workflow failure — the lawful-basis analysis should have been drafted at T-6 weeks, not T-2 days.
- **The security review that blocked launch on a prompt-injection scenario the eval suite had never covered.** Security reviewed at T-1 week and asked whether the agent's `send_email` tool could be invoked by indirect prompt injection via a ticket body. The eval suite has a prompt-injection family — but every case targets the chat surface, not the retrieved-document surface. Security's single hand-crafted example triggers the tool call. Launch blocks until the tool is gated by an explicit user-confirmation step, the suite is extended, and a new red-team pass is commissioned. Two-week slip.
- **The model card written the morning of launch with factual errors.** AI Governance (`ai-risk-engineer` (L25)) asks for the card at T-3 days. The team writes it in two hours. It lists training-data provenance incorrectly (names the base-model vendor's card, omits the fine-tune dataset); claims "no known limitations" (false — the suite has a documented tone-drift failure on inputs over 4k tokens); lists metrics that don't match the release-gate thresholds. Governance rejects the card, release blocks 48 hours, and the manager now owns a credibility debt with the governance desk that persists into the next release.

A designed workflow — the six partners, the evidence packet, the timeline, the RACI — resists all four. Privacy's engagement becomes a scheduled event, not a Friday surprise. DPIA inputs become a T-6-week deliverable, not a T-2-week scramble. The data-flow diagram is a shared artefact security and privacy read from one surface. The model card is drafted weeks before launch, not hours before the governance meeting.

## Core concept 1 — the six sign-off partners and what each owns

The six partners below are the sign-off surface for any user-facing AI feature at a scale where governance exists. For a small org the partners collapse — one person wears legal and privacy, or governance and security — but the *evidence* each one consumes does not collapse. Map every artefact to a partner even when two partners are the same person, so you can split them later.

1. **Engineering (own team).** Your team ships code, runs the eval gate, writes the release note. Two specific outputs you own: the release-gate pass from mod-104 (pre-release eval report, with thresholds vs. absolute floors), and the release note R12 from mod-104 — what changed (prompts, judges, datasets, tool schemas, model versions, guardrails), what evals ran, what the numbers were. Every other partner reads these first. If your team cannot produce them cleanly, the workflow stops before it starts.

2. **AI Governance.** A peer function, not a subordinate, with its own ladder (`ai-risk-engineer` (L25), `ai-evaluation-engineer` (L35), `senior-ai-governance-architect` (L50), `head-of-ai-governance` (L60)) and cadences. For a release it consumes the release note, eval report, red-team findings, and model card. It owns the risk-register update, the model-card sign-off, and participation in the quarterly governance program (chapter 03 is the depth). The rule for the AI Engineering Manager (L45): AI Governance is **accountable** for the governance view of this release; the manager is **responsible** for the evidence that feeds it.

3. **Legal / Counsel.** Two deliverables sit with legal, both written *by* legal: a **DPA fit-check** against the vendor/sub-processor stack (training-on-customer-data clauses, retention, data residency, sub-processor list, audit rights — see mod-105 on DPA clauses the manager owes), and a **regulatory-alignment memo** scoping the feature against the applicable regimes. For an EU-exposed product that memo answers: is this a high-risk AI system under Annex III of the EU AI Act?<!-- needs-research: confirm current Annex III scope at time of writing --> Is it a general-purpose AI model being deployed, inheriting GPAI-provider obligations when configured a certain way? Does the Article 50 transparency obligation (interacting with an AI, generating synthetic content) attach? The manager does *not* answer these; the manager *asks* them, by name, and the memo is the artefact. Routing legal opinion to counsel is explicit role scope for the AI Engineering Manager.

4. **Security.** Security consumes two outputs, both of which the manager owes: a **threat model** of the feature's trust boundaries (what accepts untrusted input, where the agent's authority flows, which tools cross which boundaries), and **red-team findings** — the adversarial / prompt-injection / data-exfiltration pass, usually produced by `ai-risk-engineer` (L25) and consumed jointly by security and governance. Security additionally reviews OAuth / tool-use authorisation against mod-106 R15: which agent actions are high-authority (write, send, pay), how is authorisation gated (user-confirmation step? deterministic policy wrapper? role-based tool whitelist?), which boundaries an exfiltration attempt crosses. Security does **not** run SOC investigation if an incident lands — that is security-operations / DFIR, an explicit out-of-scope deferral for the manager.

5. **Privacy / Data Protection Office.** Privacy's output is the **DPIA** (Data Protection Impact Assessment — GDPR Article 35 for EU-exposed features, with equivalent regimes elsewhere). The DPIA is privacy's document, not the manager's. The manager owes a specific set of **DPIA inputs**: the data-flow diagram, categories of personal data touched, lawful-basis analysis fragments (consent, legitimate interest, contract performance — scoped to this feature), retention policy per data class, sub-processor list (from mod-105), international-transfer posture (SCCs, transfer-impact assessments), and the DSR (data-subject rights) runbook — how access, rectification, erasure, and portability work on content the system has ingested. The manager is **never** accountable for the DPIA. The manager is accountable for the inputs.

6. **Product.** Commercial fit, launch comms, support readiness, pricing posture, customer-facing documentation, the launch-day owner and escalation tree. Chapter 04 is the depth on product partnership under model uncertainty; for release approval, product is the sign-off that this release *should* ship on this date to this audience with this message.

Six partners. Four (governance, legal, security, privacy) are peer functions outside the manager's reporting line. Two (engineering, product) sit inside or next-door. The manager's lever across all six is one: a single, dated, versioned evidence packet every partner reads from.

## Core concept 2 — the evidence packet the manager ships to every sign-off

A release-approval workflow lives or dies on the evidence packet. The packet is a named, dated bundle — a directory, a wiki page, a release-tracking ticket — every partner reads from. Not a thread, not an attachment chain. A single addressable surface with a version number.

On a specific release, some artefacts may be *references* to earlier documents ("model card v1.3, unchanged from last release") but every artefact must be named in the manifest. Implicit artefacts are the shape of a release that goes sideways.

- **Release note** — the mod-104 R12 output. What changed: prompts touched (registry refs), judges touched, golden-dataset version bumps, tool schemas added or modified, model versions changed, guardrail policy changes, retrieval-store schema changes. Each entry links to the PR. The release note is the first page every reviewer opens; invest in scannability.

- **Eval report.** Pre-merge offline suite results plus any canary numbers. Named thresholds against named metrics. Explicit treatment of the four drift shapes from mod-104: generation drift (model-under-test produces different distributions), judge drift (LLM-as-judge has moved), retrieval drift (index or embeddings changed), tool-use drift (agents call tools differently). For each, a short "what we saw, what the threshold allowed, pass/fail." Absolute floors called out separately from regression allowances. Confidence intervals on deltas, not point estimates.

- **Red-team findings.** The adversarial pass, commissioned from `ai-risk-engineer` (L25) at T-4 weeks. Mapped explicitly to OWASP Top 10 for LLM Applications<!-- needs-research: pin OWASP LLM Top 10 edition and year at time of writing -->: LLM01 prompt-injection (direct and indirect), LLM02 sensitive-information-disclosure, LLM06 excessive-agency / tool-authority, and any other category the threat model implicates. The report covers jailbreak-success rate on direct and indirect (retrieval-surface) corpora, any agent-authority violations (did the agent invoke a write-authority tool it should not have?), and tool-call tampering (did arguments arrive as the attacker intended rather than the user?). Security and governance read this before anything else.

- **Model card.** Mitchell et al., *Model Cards for Model Reporting*, FAccT 2019 — the canonical structure. Intended use (what the system is for; what it is not). Metrics (headline numbers from the eval report, with the dataset and judge). Training-data provenance (if fine-tuning, the fine-tune dataset's provenance, PII posture, licensing; if calling a vendor base model, cite the vendor's card and document this release's configuration). Limitations (named failure modes the team *knows* — tone-drift at long inputs, retrieval-recall shortfall on a specific topic cluster, prompt-injection resistance ceiling). Ethical considerations (who could be harmed; what is mitigated; what is not). An `applied-ai-engineer` (L30) or `ai-eval-engineer` (L35) drafts; the manager reviews; AI Governance signs off. Living document across releases.

- **Data-flow diagram.** A Mermaid-style (or equivalent) diagram that is the *shared reading surface* for privacy and security. Must show every user input surface, every model-provider call (and which model), every retrieval store (with what embedded in it), every log sink (with retention), every sub-processor crossed. Trust-boundary crossings labelled — where data leaves your network, crosses into a vendor, is written to a shared index, or returns to the user.

  ```mermaid
  flowchart LR
      user[Internal CS rep] -->|ticket text| api[Our API]
      api -->|prompt + context| llm[(Vendor LLM)]
      api -->|query| retriever[(Internal vector store)]
      retriever -->|chunks| api
      api -->|audit log| sink[(Observability store, 90d retention)]
      llm -. trust boundary .-> api
      api -->|drafted reply| user
  ```

  Trust-boundary labels are mandatory. The diagram is not a product one-pager; it is read at the altitude a security reviewer or privacy officer scans for data flows.

- **DPIA inputs.** Not the DPIA itself — privacy writes that. The inputs the manager owes: the data-flow diagram above, a lawful-basis analysis fragment (one sentence per personal-data class, identifying the lawful basis and its scope), the sub-processor list (from mod-105), a per-data-class retention policy, and the DSR runbook (access, rectification, erasure, portability). The DSR runbook for an AI feature has a specific wrinkle: if the system has indexed customer content, erasure means erasing from the index; if the system has fine-tuned on customer content, erasure is substantially harder and legal will need to opine.<!-- needs-research: specific regulator guidance on erasure obligations for fine-tuned models -->

- **Change-management summary.** SLO deltas (did this release move p95 latency or per-request cost against mod-110 SLOs?). Incident-runbook updates (did the agent-authority model change in a way that affects R17 or mod-107?). Dashboard updates. On-call handover notes.

A short example of what the packet manifest looks like:

```yaml
# release-packet.yaml — v2026.11.1-rc3
release:
  id: csrep-agent-v1.4
  candidate: rc3
  target_launch: 2026-11-24
  manager: j.ferguson
  feature_owner: a.patel

artifacts:
  release_note:
    path: releases/csrep-agent-v1.4/release-note.md
    source: mod-104 R12
    status: complete
  eval_report:
    path: releases/csrep-agent-v1.4/eval-report.md
    gate: release-gate
    pass: true
    thresholds_met: [behavioural, faithfulness, tool-use, latency, cost]
    absolute_floors: [prompt-injection-direct, sensitive-data-leakage]
  red_team_findings:
    path: releases/csrep-agent-v1.4/red-team.md
    author: ai-risk-engineer (L25) - r.lin
    owasp_mapping: [LLM01, LLM02, LLM06]
    date: 2026-11-07
  model_card:
    path: releases/csrep-agent-v1.4/model-card.md
    revision: v1.3 -> v1.4
    governance_signoff: pending
  data_flow_diagram:
    path: releases/csrep-agent-v1.4/data-flow.mmd
    revision: unchanged-from-v1.3
  dpia_inputs:
    path: releases/csrep-agent-v1.4/dpia-inputs/
    contents:
      - data-flow.mmd
      - lawful-basis-analysis.md
      - sub-processor-list.md
      - retention-policy.md
      - dsr-runbook.md
    status: delivered 2026-11-03
    dpia_owner: privacy-office (not us)
  change_management:
    path: releases/csrep-agent-v1.4/change-management.md

signoffs:
  engineering: a.patel [pending]
  ai_governance: r.lin [pending]
  legal: m.oliveira [pending]
  security: s.khan [pending]
  privacy: dpo-office [pending]
  product: d.west [pending]
```

Every partner reads this manifest first. Every unresolved sign-off has a name. The packet is live — it moves from `pending` through `requested-changes` to `signed` as the window closes.

## Core concept 3 — the end-to-end timeline

The manager publishes this timeline at cycle start. The specific weeks compress for a smaller change and expand for a bigger one. The *shape* — pre-review, evidence drafting, red-team, formal sign-off, launch, launch review, governance read-back — is constant.

- **T-6 weeks.** Product and engineering commit a target. The manager schedules the **pre-review with governance** — a 30-minute conversation with `ai-risk-engineer` (L25) or `ai-evaluation-engineer` (L35). Name the risk posture, identify which OWASP and NIST GenAI Profile categories apply, agree the red-team scope, surface anything that needs legal (new data class, new regulated jurisdiction, Article 50 transparency question). Three dated commitments come out: packet-draft date, red-team commissioning date, formal sign-off meeting date. Nothing is signed at T-6; everything is scheduled.

- **T-4 weeks.** First draft of the evidence packet lands. Release note scaffolded (populated as PRs merge). Eval report has pre-merge offline results. Model card drafted. Data-flow diagram at v1 goes to privacy and security. Red-team formally commissioned to `ai-risk-engineer` (L25) with a scope document — which attack surfaces, which OWASP categories, what tool-authority probes. If legal has a regulatory-alignment memo to produce, commission here. DPIA inputs go to privacy; the DPIA itself is now on privacy's queue.

- **T-2 weeks.** Red-team findings land. Eval canary starts (mod-104 chapter 06). The packet is near-final: release note updated, eval report with canary numbers, model-card revisions reflecting any red-team-surfaced limitations, data-flow diagram pinned. Privacy's DPIA is drafted (by privacy). Legal's memo is drafted. The manager chases any late artefact — earliest point at which a slip is visible and still fixable without crunch.

- **T-1 week.** Formal sign-off meeting. Every partner in the room or signed asynchronously. Incident runbooks updated to reflect anything the red-team or data-flow diagram changed. Dashboards wired. On-call knows what is launching. The meeting is ratification of a packet everybody has already read, *not* the first sight of the artefacts. A substantive concern surfaced here means the release slips — attributable to the manager not socialising at T-4 or T-2.

- **T-0 (launch gate decision).** A named decision-maker chairs. In a mature org a product leader (VP-level) or the AI Engineering Manager chairing jointly with product. The chair reads the signoffs section, confirms every partner has signed (or that any outstanding sign-off is explicitly overridden — see Core concept 5), issues go/no-go. Launch happens, or does not, based on this decision — not on a PRD date.

- **T+1 week — the launch review.** Not the postmortem (mod-107). A scheduled read of the first week in production: online-eval signal (mod-104 chapter 06), SLO numbers, support-ticket shape, whether the model-card's documented limitations match what the team is seeing. Output is a short memo appended to the evidence packet and a list of items for next release.

- **T+quarter — the governance read-back into the risk register.** AI Governance reads the launch-review memo into the risk register. The register is their artefact; the manager participates via the quarterly governance program (chapter 03), bringing the launch review and any overrides from the cycle.

A prompt-and-judge-only release that doesn't touch tool authority, change data classes, or add a sub-processor runs on a two-week version: brief pre-review, small packet diff, shorter red-team, thirty-minute sign-off. A release that introduces a new high-authority tool, first indexes a new personal-data class, or first ships to a new regulated jurisdiction runs on a ten-to-twelve-week version. Publish the shape at cycle start so no partner is surprised.

## Core concept 4 — the sign-off matrix (RACI)

RACI (Responsible, Accountable, Consulted, Informed) formalises who does what on each artefact. The table below is the shape for a user-facing AI feature at a mid-sized org with all six partner functions present. Columns: Eng (your team), Gov (AI Governance), Legal, Sec (Security), Priv (Privacy / DPO), Prod (Product).

| Artefact | Eng | Gov | Legal | Sec | Priv | Prod |
|---|---|---|---|---|---|---|
| Release note (R12) | A/R | I | I | I | I | C |
| Eval report | A/R | C | I | I | I | I |
| Red-team findings | C | A | I | C | I | I |
| Model card | R | A | C | C | C | C |
| Data-flow diagram | A/R | C | I | C | C | I |
| Threat model | R | C | I | A | C | I |
| DPA fit-check | I | C | A/R | C | C | I |
| Regulatory-alignment memo | I | C | A/R | I | C | C |
| **DPIA** | C | C | C | C | **A/R** | I |
| DPIA inputs | A/R | I | I | C | C | I |
| Launch-day plan | C | I | I | I | I | A/R |
| Risk-register update | C | A/R | I | I | I | I |

Two rules the manager must internalise and defend when partners try to re-RACI during a release:

**The manager is never Accountable for the DPIA.** This is not stylistic; it is the regulatory architecture. GDPR Article 35 assigns the DPIA to the controller, operationalised through the DPO or privacy function. The AI Engineering Manager supplies inputs — accurately, completely, on time — and is informed of the outcome. If the DPIA surfaces a finding that blocks launch, the finding is the privacy office's call, not something the manager overrides or argues down. The manager's lever is *input quality and timeliness*, not final judgment. A team that lets the manager drift into Accountable on the DPIA creates a conflict-of-interest shape — the team producing the risk also attests to its acceptability — and an auditor will find it.

**The manager is Accountable for the evidence that governance reads.** Release note, eval report, data-flow diagram, DPIA inputs — the manager's outputs, not the team's. "The team forgot to update the data-flow diagram" is not a defence. The quarterly override-log reads a manager's record, not a team's.

A manager who cannot produce this matrix from memory, specific to their org's partner topology, has a workflow that lives in heads. The matrix is what makes it debuggable.

## Core concept 5 — override mechanics

Every workflow will, at some point, run a release without every sign-off in hand. An exec will ask for a launch on a date privacy is three days behind on. A security concern will land at T-2 days that the team wants to address with a post-launch mitigation rather than a slip. The question is not whether overrides happen — they do — but whether they are documented, named, and visible.

Mirror mod-104 chapter 03's override-log discipline. The AI Engineering Manager maintains, publishes, and quarterly reviews a **release-approval override-log**. Fields per entry:

- **Release ID.** The release-packet manifest reference.
- **Which sign-off was overridden.** By partner function: "privacy sign-off outstanding" or "security sign-off conditional."
- **Who overrode.** A named decision-maker — a VP, a director, the manager if that is the sanctioned shape. "The team decided" is not a legal entry.
- **Why.** One or two sentences. "Launch date fixed by customer commitment; privacy DPIA window compressed; privacy office consulted on the compression and raised no blocking finding" is defensible. "Launch pressure" is not.
- **Remediation.** A date for the DPIA to land. A date for the extended red-team pass. A date for the model-card revision.
- **Non-overridable invariants that were *not* bypassed.** Mod-104 chapter 03's absolute safety floors (prompt-injection success rate, sensitive-data leakage) are non-overridable. If an entry implies one was bypassed, the release did not ship, full stop.

The log is *published* to partner functions, not kept inside engineering. Governance reads it quarterly (chapter 03). Privacy and security see every entry that involved their sign-off. A rising override count against a specific partner is an early-warning signal — either the manager is over-promising timelines (fix: adjust) or the partner is under-delivering (fix: raise through the manager's own chain; do not override your way through quarters of this).

Two override patterns are forbidden, and the manager must say so under pressure:

- **Overriding a privacy block on a DPIA with an actual finding.** The finding is the DPO's call. The override path is to accept the recommended mitigation or slip the launch. "Ship despite the DPIA finding" is a path the manager routes to counsel immediately — legal-opinion territory, out of scope for this role.
- **Overriding an absolute safety floor from the eval gate.** Release-gate non-overridable floors do not become overridable because the partner-sign-off workflow failed. They remain floors.

The log also makes a credible argument when a release asks to move faster. "Our last four releases had zero privacy overrides and all DPIA inputs landed at T-4 — this release has an exec commitment compressing us to T-2 and we'd like a three-day review window" is a conversation the history enables. A manager with no log has only the current ask to argue from.

## What good looks like

- The manager names the six partners, their artefacts, and the manager's input to each without opening the wiki. The workflow is how the manager thinks about every release.
- Every release has a release-packet manifest at a single addressable URL with a version number and date. Every "pending" entry has a named individual, not a function.
- The packet is substantially drafted at T-4 weeks. Partners respond by T-2; T-1 is ratification, not discovery.
- The model card is living across releases, not a two-hour-before-launch artefact. Its limitations section grows honestly; "no known limitations" never appears.
- The DPIA is the privacy office's document; the manager has never been labelled Accountable in any release's RACI. The team cannot recall the last time privacy was surprised.
- Red-team scope is commissioned at T-4 weeks to `ai-risk-engineer` (L25) with a written scope document mapping to OWASP LLM Top 10 categories the threat model implicates. Findings land at T-2 and feed the model card's limitations.
- The override-log is published quarterly. A manager recalls last-four-quarter override counts from memory; they are stable or trending down per partner.
- Under exec pressure for a compressed launch, the answer is "here is the compressed packet and the conversation with each partner; here is what we cannot compress (DPIA finding, red-team scope implicating a new tool, safety floors)." Not "we'll figure it out."

## What bad looks like

- Partners first hear about a release in the week before launch. The data-flow diagram is drawn the morning privacy asks for one.
- The packet lives in ten places: a release-tracking ticket, two wiki pages, a Google Doc, a shared drive, four Slack DMs, and an email chain. No partner can say which version of which artefact they are reviewing.
- The model card is written in the two hours before the governance review, with factual errors on training-data provenance and omitted limitations the suite already catches. The governance peer loses trust for the next two releases.
- The DPIA is treated as something engineering writes. Privacy is Consulted on their own DPIA; when an auditor asks who signed the Article 35 DPIA, the answer is the manager.
- The red-team pass is done by the team on itself — no independent scrutiny, the prompt-injection corpus is the eval suite's corpus (chat surface only, not retrieval surface), OWASP categories not mapped by name.
- Overrides are verbal. "The exec said it was fine" is the paper trail. No log, no remediation dates, no quarterly review. The next release expands the override shape because no record says it shouldn't.
- Legal is asked for an opinion the week of launch. The DPA fit-check and regulatory-alignment memo are skipped or written reactively; the first customer-facing legal question after launch surfaces a clause the manager should have caught at T-6.
- Asked "who signs off on this release?" the manager answers "we send it through CI and it goes out." The release has not been signed off; it has been merged.

## Summary

- A GA release crosses six desks — Engineering, AI Governance, Legal, Security, Privacy, Product. A manager who cannot name all six and their artefacts has a release schedule that will slip when the first unknown partner surfaces a concern.
- Every partner reads from one versioned evidence packet: release note (R12), eval report, red-team findings (`ai-risk-engineer` (L25)), model card (Mitchell et al. 2019 structure), data-flow diagram, DPIA inputs (not the DPIA itself), change-management summary.
- Timeline shape: T-6 (commit, pre-review), T-4 (packet draft, red-team commissioned, DPIA inputs to privacy), T-2 (findings land, canary runs), T-1 (ratification), T-0 (launch gate), T+1 week (launch review), T+quarter (governance read-back). Publish at cycle start.
- The manager is **never** Accountable for the DPIA — Article 35, privacy's artefact, no exceptions. The manager **is** Accountable for the release note, eval report, data-flow diagram, and DPIA inputs.
- Overrides are logged with named decision-maker, reason, and remediation date; the log is published quarterly; absolute safety floors from mod-104 chapter 03 remain non-overridable regardless of partner-workflow override state.

The next chapter is the depth on the AI Governance interface — how the manager consumes engineered-risk evidence from `ai-risk-engineer` (L25), release-assurance from `ai-evaluation-engineer` (L35), and participates in the governance program owned by `senior-ai-governance-architect` (L50) and `head-of-ai-governance` (L60) without duplicating scope.
