# Operating the Interface with the AI Governance Family

An AI engineering team **produces** evidence: model cards, eval reports, release notes, incident narratives, risk-register entries, red-team findings. The AI Governance program **consumes** that evidence, maintains the organisation-wide control library those artefacts map against, and reports upward — governance council, board, regulator — in a shape neither audience could read directly from engineering. The interface between the two is where most AI programs quietly fail, because the roles look symmetric (both have "AI" in the title, both care about eval numbers, both show up in launch reviews) and they are not.

Blurring produces two failure modes. The first is the engineering team doing governance's job — writing its own control library, interpreting its own regulatory posture, authoring its own board-facing risk narratives — poorly, because it is not their altitude and not their standing. The second is the governance team doing engineering's — specifying evals, authoring threat models, running parallel eval infrastructure — accreting into a release-blocking shadow team that slows every launch to a quarterly cadence. The manager's job is to hold the line between **evidence production** and **evidence consumption**, and to operate the interface with the four peer roles so neither side crosses it.

## Motivation — three failure shapes when the interface is not operated

- **The team that built a parallel control library.** The team wrote its own catalogue of "AI controls" mapped to NIST AI RMF 1.0, ISO/IEC 42001, and the EU AI Act, and treated it as the canonical reference for release gates. Nobody consulted the `senior-ai-governance-architect` (L50), who had been maintaining the organisation-wide library for eighteen months. An internal audit later asked for mapping between team-side controls and the organisation library; nothing mapped. The work was redundant and had to be rewritten. A *structural* gap: the team did not know the library existed because no interface with governance was running.
- **The team that commissioned no red-team work.** The team shipped a tool-using agent through its own eval-gated SDLC (mod-104): behavioural suite comprehensive, judge rubric calibrated, latency and cost measured. At the launch gate, the sponsoring VP asked, "what did risk-engineering find?" The manager had no answer. The release slipped while an `ai-risk-engineer` (L25) was scrambled in. A *commissioning* gap: the manager had treated red-team work as something that would happen if someone noticed it was missing.
- **The governance program demanding evidence the team was not producing.** The `head-of-ai-governance` (L60) sent a quarterly request for "model risk register entries per deployed feature, in the format Annex A of the governance policy." The team had deployed six features in the quarter and had none in that format. The backfill was lower quality than in-stream authoring would have been. A *contract* gap: nobody had written down what evidence flowed left-to-right and in what shape, so both sides discovered the misalignment at the moment of consumption.

All three are interface failures, not competence failures. The team was not bad at engineering, the governance program not bad at governance; they were operating without a named, dated contract between them.

## Core concept 1 — the four peer roles in AI Governance, and what each produces and consumes

The AI Governance peer family has four roles the manager interacts with routinely. Each produces a different artefact; each consumes a different slice of what the team produces.

### `ai-risk-engineer` (L25) — engineered-risk and red-team

Produces: engineered-risk evidence against specific features. Threat models (what an adversary would do against this agent, this retrieval surface, this tool). Red-team results (jailbreak transcripts, prompt-injection chains, exfiltration attempts). Jailbreak catalogues and refusal-fidelity evidence. Guardrail-coverage reports mapping which attack families each shipped guardrail actually resists.

The manager **commissions** this work, per feature, on a brief the manager owns. The brief names scope, threat-model inputs (OWASP LLM Top 10 by edition; MITRE ATLAS tactics relevant to the feature), timebox, acceptance criteria, deliverables, escalation path, and cadence. Core concept 3 walks the brief in detail.

`ai-risk-engineer` (L25) is not an eval engineer and does not write the team's behavioural suite. They are an adversary-perspective reviewer whose output is one input to the release gate the triad reads in mod-106.

### `ai-evaluation-engineer` (L35) — release-assurance and audit-trail

Produces: **release-assurance** artefacts. An independent verification run against a release candidate; a judge-calibration evidence packet for any LLM-as-judge in use; an audit-trail document (what was run, on what data, by whom, when, against what version) in a shape a regulator or external auditor can read; an attestation that the release-gate numbers on the release notes match the numbers the eval actually produced.

The manager **consumes** this work — specifically, the attestation and the audit trail — and includes it in the release-gate packet. The methodology is owned at governance altitude, often constrained by external frameworks (ISO/IEC 42001, EU AI Act conformity assessment, sector regulation).

The distinction that kills AI programs that get this wrong: release-assurance is **not** the team's own evals, and `ai-evaluation-engineer` (L35, AI Governance family) is **not** `ai-eval-engineer` (L30, AI Engineering family). The team's own eval craft — the commissioning brief from mod-104 chapter 02, the PR gate, the release gate, the quarterly suite review — lives with `ai-eval-engineer` (L30) on the manager's team. Release-assurance is an independent read of that work by a role the manager does not manage. Two roles, two families, two altitudes. Say the names right out loud every time, or the team conflates them within a quarter.

### `senior-ai-governance-architect` (L50) — the control library

Produces: the organisation-wide **control library**. One catalogue, versioned, mapping controls to external frameworks: NIST AI RMF 1.0, ISO/IEC 42001, the EU AI Act (high-risk, transparency, general-purpose AI obligations), and sector regulation — HIPAA for health, model-risk-management expectations (e.g., US federal supervisory guidance on model risk management<!-- needs-research: current citation / successor to SR 11-7 as of 2026 -->) for financial services, GLBA, FTC Section 5 for US consumer contexts, UK ICO guidance for UK data-subject exposure.

The manager **implements** team-side controls that satisfy library entries and **maps** every control the team operates to one. If a team-side control cannot be mapped, the manager raises the gap to the architect and asks for the library to be extended, rather than inventing a parallel entry. Parallel libraries fragment governance and make audit impossible. This role is also the one mod-105 connects to: vendor-portfolio decisions made against control-library coverage.

### `head-of-ai-governance` (L60) — the program

Produces: the governance program. The governance council (membership, cadence, decision authority). Board-level AI-risk reporting. Regulator engagement (filings, queries, voluntary disclosures). Policy — AI-use, model-risk-management, acceptable-use. External framework alignment at program level.

The manager **attends** the governance council on behalf of the team, as a presenter of evidence and receiver of guidance. The manager does **not** attend as a governance-owner: they do not vote on policy, write board narrative, or conduct regulator conversations in that forum. The manager's role is to be the engineering witness for shipped features and to receive the control-library updates that shape next quarter's work.

## Core concept 2 — the evidence-exchange contract

The interface runs off a named, dated contract. Not an operational SLA — an evidence-exchange contract that says, in advance, what artefacts move left-to-right (engineering → governance) and what moves right-to-left, on what cadence, with what owners. One page. Signed by the manager and the `head-of-ai-governance` (L60). Reviewed quarterly.

**Engineering → Governance** (what the team produces for governance to consume):

- **Model cards** for every deployed feature, in the format the architect specifies.
- **Release notes with eval deltas** at every release-gate crossing (the artefact from mod-104).
- **Incident narratives** (mod-107 chapter 07 summary) for every incident meeting the severity threshold defined in the contract.
- **Risk register entries** for every new feature, mapped to control-library entries.
- **Red-team findings reports** from `ai-risk-engineer` (L25) engagements the manager commissioned.
- **Vendor and model change notifications** when the team swaps a model-vendor or crosses a model-version boundary (the surface mod-105 defines).

**Governance → Engineering** (what governance produces for engineering to consume):

- **Control-library updates** — new entries, revised entries, retired entries, with change notes.
- **Red-team scope agreements** — the shape and depth of engagement `ai-risk-engineer` (L25) will provide per feature class, so the team knows what to commission.
- **Regulatory alignment memos** — "feature class X is in scope of EU AI Act Annex III (Y); here is what evidence the conformity assessment will want" — from the architect, under the head of governance's sign-off.
- **Cross-feature risk patterns to watch for** — signal the governance family has seen across teams: an attack pattern the AIID has catalogued, a jailbreak family another team hit, a vendor-reliability trend.
- **Release-assurance attestations** from `ai-evaluation-engineer` (L35), per release-gate crossing.

A sketch of the contract, as a one-page document, in a shape a team actually keeps:

```yaml
# ai-governance-evidence-contract
version: 2026.Q4-v1
effective: 2026-10-01
next_review: 2027-01-15
signatories: [ai-engineering-manager, head-of-ai-governance]

engineering_to_governance:
  - { artefact: model_card,                   cadence: per_feature_launch,            consumer: senior-ai-governance-architect }
  - { artefact: release_notes_with_eval_delta, cadence: per_release_gate,             consumer: ai-evaluation-engineer }
  - { artefact: incident_engineering_narrative, cadence: per_incident_sev2_or_higher, consumer: head-of-ai-governance }
  - { artefact: risk_register_entry,          cadence: per_feature_and_quarterly,     consumer: senior-ai-governance-architect }
  - { artefact: red_team_findings_report,     cadence: per_feature_and_mature_quarterly, consumer: head-of-ai-governance }
  - { artefact: vendor_or_model_change_notice, cadence: on_event,                     consumer: senior-ai-governance-architect }

governance_to_engineering:
  - { artefact: control_library_revision,      cadence: quarterly,  owner: senior-ai-governance-architect }
  - { artefact: red_team_scope_agreement,      cadence: annually,   owner: ai-risk-engineer }
  - { artefact: regulatory_alignment_memo,     cadence: on_classification_and_annually, owner: senior-ai-governance-architect }
  - { artefact: cross_feature_risk_bulletin,   cadence: quarterly,  owner: head-of-ai-governance }
  - { artefact: release_assurance_attestation, cadence: per_release_gate, owner: ai-evaluation-engineer }
```

Publish the contract. Put it in the engineering handbook and in the governance handbook. Review every quarter against features actually shipped and artefacts actually exchanged. Teams that run this contract have predictable governance interactions; teams that do not have governance interactions that feel like ambushes from both directions.

## Core concept 3 — commissioning the red-team engagement from `ai-risk-engineer` (L25)

Same shape as the commissioning brief from mod-104 chapter 02 — a short brief written before the IC starts work — specialised for red-team. Six sections.

### 1. Scope

Name the agents, tools, guardrails, retrieval surfaces, and model endpoints in scope. Name what is *out* of scope — data-pipeline infrastructure, identity/authentication, host OS. For an agent, name the tools callable (`search_kb`, `update_ticket_status`, `send_email`) and which are privileged. For RAG, name the corpus and the ingestion path (indirect prompt injection lives in the ingestion path).

### 2. Threat-model inputs

- **OWASP Top 10 for LLM Applications** — pin the version-year (versions and titles have evolved; name the edition you are using). Pin the categories in scope: for a tool-using agent, at minimum LLM01 (Prompt Injection), LLM05 (Improper Output Handling), LLM06 (Excessive Agency), LLM02 (Sensitive Information Disclosure).
- **MITRE ATLAS** — pin the tactics and techniques relevant to the surface. For a RAG agent, *Initial Access* (via indirect prompt injection in documents) and *Impact* (via excessive agency on connected tools) are typically in scope.<!-- needs-research: current MITRE ATLAS tactic / technique IDs and naming as of 2026 release -->
- **Internal attack-pattern library** — if the architect maintains one, pin the version.

### 3. Timebox and acceptance criteria

State the wallclock ("two engineer-weeks of red-team time, concluded by [date]") and what "done" means ("coverage of every pinned OWASP and ATLAS category with at least one confirmed-or-refuted attack chain; findings report; replay fixtures for every confirmed finding"). Without this, the engagement runs until the IC runs out of time, not until scope is covered.

### 4. Deliverables

- **Findings report** — one narrative per confirmed finding, with attack chain, evidence, severity rating, proposed mitigation, and proposed regression fixture.
- **Replay fixtures** — adversarial inputs in a form the team can wire into the eval suite (mod-104). The incident-to-fixture loop (mod-107 chapter 06) applied to red-team findings.
- **Severity ratings** — on a published rubric (the architect typically owns it; if not, agree one in the brief). Reproducible by another reviewer against the same evidence.
- **Guardrail-coverage read** — against the shipped guardrails, which attack families they catch and which they do not. The artefact mod-106's release triad will read.

### 5. Escalation path

State what severity triggers what. "Severity-1 (confirmed PII exfiltration, privilege escalation on a tool, unsafe-content generation to a user-facing surface) blocks release regardless of delta. Severity-2 requires a mitigation plan in the release packet. Severity-3 goes to the backlog with named owner and due date." Without this in advance, the manager discovers at the launch review that severity-2 means different things to engineering and governance, and the launch slips.

### 6. Cadence

- **Per-feature at launch.** Every feature of consequence gets a red-team pass before the release gate.
- **Quarterly for mature features.** The surface drifts — new tools, new model versions, new data sources. Quarterly re-engagement catches the drift.
- **On trigger events.** A model-vendor swap (mod-105), a major prompt-registry change, a new tool with privileged scope, an incident of related shape (mod-107). Name the triggers.

Running the brief faithfully means the manager has an answer to "what did risk-engineering find?" at the launch gate, on the day.

## Core concept 4 — consuming release-assurance from `ai-evaluation-engineer` (L35)

The team runs its own evals (mod-104). The outcome is a release-gate packet — eval report, delta against baseline, cost and latency, safety-case against pinned frameworks. This is the artefact the team owns and the release triad reads internally.

**Release-assurance is the independent read of that work.** `ai-evaluation-engineer` (L35) runs the team's eval pack (or a parallel pack calibrated against it) independently, on the release candidate, and produces three artefacts: independent-verification numbers, judge-calibration evidence (has the judge drifted; does judge–human agreement still hold), and an **attestation** — a signed document saying "we ran the gate, the numbers match (or do not), on date X, against version Y."

The manager reads this twice, from two altitudes:

- **Team ops** reads the raw eval delta — quality numbers, regression analysis, faithfulness and tool-use scores — because that is the operational signal for shipping. Engineering-internal.
- **Governance consumes the attestation** — because that satisfies the control-library entry on release assurance and the regulator-facing audit trail. Not engineering-internal.

The attestation is useless for ops (nothing new); the raw delta is useless for governance (unauditable without the attestation).

**When the two disagree.** The team's eval report says go; release-assurance disagrees — the independent run produces different numbers, the attestation declines to sign, or the judge calibration shows drift. Who resolves? Not the manager, not the `ai-evaluation-engineer` (L35) alone. The `senior-ai-governance-architect` (L50) is the escalation. The disagreement is a control-library question — what does the gate mean, what does the attestation attest to, what is the methodological reconciliation — and the architect owns the methodology. The manager surfaces the disagreement, provides engineering context, and ships the architect's decision. Managers who resolve release-assurance disagreements themselves — overruling the governance read (unauthorised) or delaying releases while re-running evals until numbers agree (wasteful) — are at the wrong altitude.

## Core concept 5 — participating in the governance program without duplicating it

Three tactics for the manager's own conduct.

### 1. Attend the governance council as a producer-of-evidence, not as a governance-owner

The governance council meets on some cadence (monthly, quarterly, depending on organisation). The manager attends on behalf of the team. The role is to **present evidence** — this is what we shipped, these are the eval deltas, these are the incidents, these are the red-team findings, this is the state of the risk register — and to **ask questions** — "we are classifying feature X as Annex III high-risk; is that your reading? what evidence will the conformity assessment want?" The manager does not **write policy** from the chair, **vote** on organisation-wide controls (unless explicitly delegated, which is rare), or **author** the board narrative. Those are `head-of-ai-governance` (L60) and `senior-ai-governance-architect` (L50) products, informed by the evidence the manager brings.

A manager who shows up and says, "and here is our proposal for the organisation's prompt-injection policy," is operating two levels above their standing. The governance owners will be polite; privately, they stop asking for structured input and start treating the manager as unpredictable.

### 2. Map every team-side control to the control library

For every control the team operates — a PII-classifier output filter, a HITL review for agent actions that modify a connected system, a monthly judge re-calibration — map it to a specific library entry. The mapping is a short table in the engineering handbook; the manager keeps it current.

If a team-side control cannot be mapped, **raise the gap** to the architect. Do not invent a parallel entry; the gap is the input the architect uses to decide whether to extend the library. If the library has an entry the team's current control does not satisfy, that is a backlog item with a named owner and target date. Teams that let this table drift discover during an external audit that half their controls are unmapped and the other half point to retired entries.

### 3. Route regulatory questions to the `head-of-ai-governance` (L60)

"Does EU AI Act Article 6 apply to this feature?" is not a manager-answered question. Route it upward with engineering context: "feature X does Y, is used by audience Z, in jurisdiction W; please advise on classification." The `head-of-ai-governance` (L60), in coordination with counsel (core concept 6), produces the answer. The manager implements it.

Managers who self-answer regulatory classification questions import legal risk they cannot discharge. The typical pattern: the manager reads the regulation, forms a reasonable-looking view, and ships against it; a year later the governance program classifies the feature differently, and the retrofit costs more than the original build.

## Core concept 6 — the boundary with legal counsel

The AI Governance peer family produces **regulatory alignment**: mapping features to AI-specific frameworks (EU AI Act, ISO/IEC 42001, NIST AI RMF, sector regulation), the control-library expression of those obligations, the shape of evidence that satisfies them. The engineering-meets-regulation interpretive layer.

Legal counsel produces **regulatory opinion**: whether a specific clause applies in a specific jurisdiction to a specific product configuration; whether an argument is likely to hold in litigation, enforcement action, or regulator inquiry; what language goes into filings and contracts; what disclosure obligations exist to customers and data subjects; the jurisdictional surface when the product ships multi-geo. The legally-binding interpretive layer.

Many AI engineering managers conflate the two because both are "the people who tell us what the law says." They are not interchangeable.

Governance says: "this feature is in scope of EU AI Act Annex III as a credit-scoring high-risk system; the conformity-assessment evidence pack we need is X, Y, Z." That is alignment: here is where the feature maps; here is the evidence shape we produce.

Counsel decides: "here is what that legally obligates us to do before placing on the EU market, here is our litigation posture if an enforcement action lands, here is the ToS language, here is the data-protection impact assessment the DPO must sign." That is legally-binding: what the organisation must do, in what jurisdiction, by what date, against what penalty.

Day-to-day example: the team launches a feature that classifies loan applications. Governance says, "Annex III high-risk; our control-library entry applies; we need the release-assurance pack, the risk register, the human-oversight evidence, and the fundamental-rights impact assessment input." The manager produces all of that. Counsel then determines: "given the jurisdictions and the deployer-vs-provider role in each, here is the EU AI Act database registration we need to file (if applicable), here is the conformity-assessment path, here is the customer-facing disclosure."<!-- needs-research: current state of EU AI Act database registration mechanics, conformity-assessment modalities for Annex III, deployer-vs-provider obligation split, as of 2026-11-10 --> Governance produced the alignment; counsel produced the opinion; the manager implements both without arbitrating.

This chapter opens the boundary. Chapter 07 closes it with the detailed partnership pattern for counsel across contracts, filings, incident response, and customer disclosure.

## What good looks like

- The manager can name the four roles in the AI Governance family, their level, what each produces, and what the manager does with each (commission, consume, implement, attend).
- The evidence-exchange contract exists as a one-page document, signed by the manager and the `head-of-ai-governance` (L60), reviewed within the last quarter.
- Every feature of consequence in the last twelve months had a commissioning brief for an `ai-risk-engineer` (L25) red-team engagement before the release gate; findings are in the release packet; severity-1 findings were resolved or blocked the launch.
- Release-assurance attestations from `ai-evaluation-engineer` (L35) are in the release-gate packet for every release in scope of the control library; the team's own eval report and the attestation are stored side-by-side and read at different altitudes.
- Every control the team operates maps to a named entry in the control library; the mapping table is current within a quarter; gaps are raised to the architect as library-extension requests, not patched with invented entries.
- The manager attends the governance council on its cadence, presents evidence, asks questions, and does not freelance policy from the chair.
- Regulatory classification questions are routed to `head-of-ai-governance` (L60) with full engineering context; the manager implements the answer rather than inventing one.
- The manager can state, for any shipped feature, the distinction between what governance says (alignment) and what counsel says (opinion).

## What bad looks like

- The manager cannot name the four roles or their levels, and in practice interacts only with whichever role turns up first in a launch review.
- No evidence-exchange contract exists; each governance interaction is renegotiated from scratch; the team is surprised by quarterly evidence requests.
- Features ship without commissioned red-team work; "what did risk-engineering find?" is answered with a shrug at the launch gate.
- The team's own eval report is treated as release-assurance; no independent attestation exists; the gap is discovered during a regulator or auditor request.
- The team maintains a parallel "AI controls" document that does not map to the organisation's library; an internal audit later requires the mapping redone against the real library.
- The manager drafts organisation-wide AI policy from the governance council chair or votes on controls they do not own; governance owners route around the manager.
- The manager self-answers regulatory classification questions and later discovers the classification was wrong at the cost of a feature redesign.
- Governance says a feature is in scope of a framework and the manager treats that as the full legal picture; counsel is first engaged at the point of regulator inquiry.

## Summary

- An AI engineering team *produces* evidence; the AI Governance family *consumes* it, maintains the control library, and runs the program. Blurring the roles produces either a team doing governance badly or a governance program doing engineering slowly.
- The four peer roles: `ai-risk-engineer` (L25) — engineered-risk and red-team, which the manager **commissions**; `ai-evaluation-engineer` (L35) — release-assurance and audit-trail, which the manager **consumes** (distinct from `ai-eval-engineer` (L30) in the engineering family, which the manager manages); `senior-ai-governance-architect` (L50) — the control library, against which the manager **maps** team-side controls; `head-of-ai-governance` (L60) — the governance program, which the manager **attends** as an evidence-producer, not an owner.
- The evidence-exchange contract — named artefacts, cadences, owners, one page, signed, reviewed quarterly — is the interface. Writing it is what turns ad-hoc governance ambushes into predictable exchange.
- Red-team commissioning from `ai-risk-engineer` (L25) is a six-section brief (scope, threat-model inputs from OWASP LLM Top 10 and MITRE ATLAS, timebox, deliverables, escalation path, cadence). Per-feature at launch; quarterly for mature features.
- Release-assurance from `ai-evaluation-engineer` (L35) is an independent read of the team's eval work; the attestation is what governance consumes, the raw delta is what team ops reads; disagreements escalate to `senior-ai-governance-architect` (L50).
- The boundary with legal counsel is between **alignment** (governance maps features to frameworks) and **opinion** (counsel decides what the law binds the organisation to do). Chapter 07 closes it.

The next chapter walks the interface with the Product function — the shape of the roadmap conversation when the surface is probabilistic, and what the manager owes the product manager that is specific to AI.
