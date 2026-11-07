# EU AI Act Articles 14, 15, and 50 as Engineering Deliverables

The EU AI Act — Regulation (EU) 2024/1689 — is binding law. It sets risk-tiered obligations on providers and deployers of AI systems placed on the EU market or whose output is used in the EU, regardless of where the provider is established. For an AI engineering manager, the law is **neither** a legal homework problem counsel handles in isolation **nor** a product-design problem engineering solves by reading the regulation and improvising. It is a partnership: counsel reads the law and tells the team what applies; the team ships the engineering deliverables that make counsel's reading true; counsel re-reads and signs the mapping. This chapter is the team's side of the partnership.

Three articles in particular translate into engineering work the AI engineering manager owns:

- **Article 14 — Human oversight** of high-risk AI systems: the engineering mechanisms through which a human can effectively oversee the system in operation.
- **Article 15 — Accuracy, robustness, and cybersecurity** of high-risk AI systems: the engineering measures demonstrating the system is accurate, robust, and secure.
- **Article 50 — Transparency** obligations for certain AI systems (not only high-risk): disclosure that users are interacting with an AI system, labelling of synthetic content and deepfakes.

The manager does **not** need to be the one who determines whether the product is a "high-risk AI system" under Annex III. That determination is a legal question counsel answers. The manager does need to ship the engineering deliverables each article enumerates, so that when counsel's answer lands, the team's product is already in a position to be compliant.

A grounding note: this chapter reads the law *as published* in the consolidated text referenced in `resources.md`. Treat the specifics as *what the law requires*; treat the implementations as *engineering patterns that have proven to satisfy the requirements* at the time of writing. Verify the current text against the official EUR-Lex version before signing anything.

## Motivation — three ways teams fail at EU AI Act compliance

- **"Counsel said we're compliant."** Counsel wrote a two-page memo citing the articles. The engineering team never saw the memo; no artefacts changed. When the first regulator-adjacent request arrives (a customer's procurement questionnaire, a DPO's data-processing-impact-assessment request), the team has nothing to show. Counsel's memo was not wrong; it was not enough.
- **"Our product isn't on Annex III."** Possibly true; still misses the point. Article 50's transparency obligations apply to certain AI systems that are not high-risk; chatbots must tell users they are AI, synthetic image / audio / video must be machine-readable-marked. A product that is not high-risk can still trigger Article 50 obligations and ship uncompliant because the team read only the high-risk section.
- **"We'll sort it out before the deadline."** The Act's application rolls out in waves (certain prohibited-practice obligations early, general-purpose AI model obligations later, high-risk system obligations latest). "Before the deadline" is a different date for different obligations. A team planning against one deadline is often surprised by another.

The cure to all three: a specific mapping from Articles 14, 15, and 50 to engineering deliverables the team already produces (eval reports, guardrails specs, audit logs, incident playbooks) or now must. The manager owns the mapping.

## Core concept 1 — Article 14 translated into engineering deliverables

Article 14 requires high-risk AI systems to be designed and developed so that they can be effectively overseen by natural persons during the period they are in use. The engineering deliverables that answer this are, broadly, five.

### Deliverable 14.A — human intervention points

A documented inventory, per high-risk system, of the points at which a human can intervene in the system's operation — including stopping the system, overriding an output, changing a configuration, or disabling a sub-component. In this module's vocabulary, this reads out to:

- The HITL checkpoints from the guardrails spec (chapter 02 section 5) and the authorisation boundary (chapter 07): named points where a human decides.
- The kill-switches from chapter 07 (tool-level and surface-level feature flags).
- The release-triad override protocol (chapter 05) — a human reviewing and signing that the system should (or should not) continue to operate.
- The on-call runbook actions from mod-107 (rollback, disable, escalate).

Ship: the inventory is a document, filed under the high-risk-system's governance folder, co-signed by engineering and legal.

### Deliverable 14.B — competency and information requirements for overseers

Article 14 requires that the humans doing the oversight have the necessary competence, training, and authority, and that the information they need to make a decision is provided to them intelligibly. Engineering deliverables:

- Named reviewer roles per checkpoint (chapter 02 section 5).
- Reviewer tooling: what the reviewer sees (the inputs, the retrieved content, the classifier scores, the proposed action, the model's reasoning summary where applicable) at the moment of decision.
- Explanations surfaced to the reviewer at the altitude the decision requires — not the full trace, not nothing.
- Training materials and rubrics for the reviewer role, maintained on a known review cadence.

Ship: the HITL-reviewer tooling specification, the reviewer-training materials, the rubric.

### Deliverable 14.C — automation-bias mitigation

The Article names automation bias (the tendency to over-rely on the system's output) as a specific risk to mitigate. Engineering deliverables:

- UX features that reduce automation bias: confidence indicators where calibrated and meaningful; "the system does not know this with high certainty" disclosures; forced-pause patterns on high-stakes actions; dashboards that show reviewers their override rate against known-ground-truth cases.
- Reviewer-calibration rituals — periodic calibration sessions against gold cases — borrowed from the data-flywheel practice in mod-104 chapter 07.

Ship: UX specifications for confidence / uncertainty disclosure; the reviewer-calibration ritual (ties to mod-104 chapter 07).

### Deliverable 14.D — monitoring of the oversight mechanism itself

A human-oversight mechanism that nobody checks is a notional oversight mechanism. Engineering deliverables:

- Dashboards: HITL queue depth, approval SLA adherence, approval / override rate by reviewer, missed-SLA auto-decision rate.
- Alerts: when any of the above moves outside the established operating range.
- Audit trail: every oversight decision durably logged (chapter 02 section 5, chapter 05 incident-playbook pin).

Ship: oversight-metric dashboards and alerts, retained to the compliance window.

### Deliverable 14.E — the "stop the system" capability

A final, explicit deliverable: a mechanism by which oversight personnel can halt operation of the system, either for all users or for a specific subset, safely and quickly. The kill-switches from chapter 07 satisfy the mechanism side; the runbook tested in a dry-run (mod-107) satisfies the "safely and quickly" side.

Ship: tested kill-switches; dated last-dry-run record.

## Core concept 2 — Article 15 translated into engineering deliverables

Article 15 requires that high-risk AI systems be designed and developed so as to achieve an appropriate level of **accuracy, robustness, and cybersecurity**, and to perform consistently throughout their lifecycle. Engineering deliverables break along those three sub-properties.

### Deliverable 15.A — accuracy

The Act requires that the appropriate accuracy level be declared, and performance measured against that level, documented, and maintained. Engineering deliverables:

- A dated **accuracy claim** per system, expressed in metrics the eval registry (mod-104 chapter 04) evaluates. Not "high accuracy" — a specific metric ("faithfulness ≥ 0.90 on golden set v3.2") per the eval-gate policy (mod-104 chapter 03).
- The eval suite (mod-104 chapter 02) with golden datasets that are representative of the deployment population (and the *fairness-and-bias* evaluations that Article 15 cross-references in its accuracy framing).
- Ongoing online-eval evidence (mod-104 chapter 06) that the accuracy claim is maintained in production.
- Documentation of known inaccuracy modes and their mitigations — accepted-risk entries for inaccuracies the product currently ships with (chapter 04's dispositions).

Ship: the eval-registry links, the online-eval history, the accuracy declaration.

### Deliverable 15.B — robustness

Robustness, per the Act, includes resilience to errors, faults, inconsistencies, and adversarial manipulation. Engineering deliverables:

- The guardrails spec (chapter 02) and its red-team evidence (chapter 04), which together demonstrate adversarial robustness across the risk model.
- Fallback architecture (mod-105 chapter 04) demonstrating resilience against upstream provider failure.
- Input / output validation (chapter 02 sections 2–3) as the defence against inconsistent inputs and malformed outputs.
- Robustness evals in the eval suite (mod-104) — adversarial / perturbation / distribution-shift evaluations, not just happy-path.
- The incident playbook (mod-107) covering degraded modes.

Ship: red-team report, guardrails spec, fallback plan, robustness eval cases, incident playbook references.

### Deliverable 15.C — cybersecurity

Cybersecurity, in Article 15, includes resilience against attempts by unauthorised third parties to exploit vulnerabilities, including data poisoning, model poisoning, model evasion, confidentiality attacks, and the specific AI-era attack surface. Engineering deliverables:

- The agent authorisation boundary (chapter 07): OAuth / service-identity / scope / HITL / kill-switch / audit log posture.
- Vulnerability management for the model supply chain — covered partially by mod-105 chapter 06 (open-weights provenance), partially by the application-security discipline.
- Prompt-injection, model-extraction, and supply-chain-poisoning red-team coverage (chapter 04).
- Secrets management for API keys and service identities (platform-engineering responsibility; the manager verifies the review covers it).
- Incident-response readiness (chapter 05 pin, mod-107).

Ship: authorisation boundary document, red-team report (cyber chapter specifically), incident-response playbook, cross-reference to the organisation's SSDLC / ISO 27001 / SOC 2 posture.

### Lifecycle maintenance

Article 15 requires that accuracy, robustness, and cybersecurity be maintained **throughout the lifecycle**. The engineering mechanism is the release-triad review clock (chapter 05 element 4): monthly post-launch for three months, quarterly thereafter, triggered on material change. The clock proves "throughout the lifecycle" in writing.

## Core concept 3 — Article 50 translated into engineering deliverables

Article 50 imposes transparency obligations on providers and deployers of certain AI systems — not only high-risk ones. The three deliverables.

### Deliverable 50.A — "You are interacting with an AI"

Providers of AI systems intended to interact directly with natural persons must design and develop the system so that affected persons are informed that they are interacting with an AI system — unless this is obvious from the context of use to a reasonably well-informed person taking into account the circumstances. Engineering deliverables:

- A durable **disclosure** in the chat / interaction UI (not just a privacy-notice link). Common patterns: a header strip, a chat-opening message, a visible branding element.
- Audit evidence of when the disclosure was shown (in product logs or screenshots as part of the compliance-evidence archive).
- The disclosure wording reviewed by counsel for jurisdiction-appropriate language (certain Member States may have additional guidance).

Ship: the in-product disclosure, the audit-trail that shows it fired, the wording-review record.

### Deliverable 50.B — machine-readable marking of synthetic content

Providers of AI systems generating synthetic audio, image, video, or text content must mark the outputs in a machine-readable format and make them detectable as artificially generated or manipulated. Engineering deliverables:

- **C2PA (Coalition for Content Provenance and Authenticity)** content credentials on AI-generated images / video / audio where the standard applies. C2PA provides a signed, standard-compliant manifest describing the content's provenance; it is the primary machine-readable marking the industry has coalesced around.
- **Metadata tags / watermarks** in the standard-appropriate format for the output modality. For text, the Act's "machine-readable" requirement remains the subject of ongoing technical guidance — watermark approaches (e.g. the published research on cryptographic text watermarking, the provider-supplied synthetic-text metadata) are the current state.
- Configuration of the generation pipeline to produce the markings by default, with exceptions documented.

Ship: C2PA / watermarking configuration, output-sampling audit that confirms the markings are present, documentation of the technical choice and its rationale.

### Deliverable 50.C — deepfake and public-interest-content labelling

Deployers of AI systems generating image, audio, or video content constituting **deepfakes** must disclose that the content is artificially generated or manipulated, except where use is authorised by law (e.g. certain law-enforcement purposes) or where the content is part of artistic or satirical expression — the Act's exceptions are specific. Similarly, deployers generating text published for public-interest matters have labelling obligations.

Engineering deliverables:

- A **labelling mechanism** — the visual overlay, the embedded disclosure, the publication-system flag — that labels deepfake-category outputs when they are generated or at publication time.
- The classification mechanism that determines whether an output is in-scope ("is this image a deepfake under the Act's definition?"). This is typically a product-decision (what the product is used for) more than a model-output classification — a face-swap feature is always in-scope; a photo-restoration feature may not be.
- The documentation of exceptions claimed (artistic / satirical / law-enforcement) with the basis for each.

Ship: the labelling mechanism, the in-scope-determination logic, the exceptions documentation.

## Core concept 4 — the mapping memo the manager owns

The engineering deliverables above do not file themselves. The manager owns a per-product **compliance-mapping memo** that links each applicable Article's obligations to the specific artefacts the team ships.

```markdown
# EU AI Act mapping — <product name>
Legal determination (per counsel memo, <date>): High-risk under Annex III §<N>, OR Not high-risk but subject to Article 50 obligations §<X>, OR Not applicable (reasoning).

Review cadence: this memo re-read with counsel on <cadence>.

## Article 14 — Human oversight (if high-risk)
| Obligation | Deliverable | Artefact | Last reviewed |
|---|---|---|---|
| 14.A intervention points | HITL inventory | chapter 02 section 5; chapter 07 boundary doc | 2026-11-30 |
| 14.B competence and information | reviewer roles + tooling | spec sections 5; HITL UX spec | 2026-11-30 |
| 14.C automation-bias mitigation | confidence UX; calibration ritual | UX spec; mod-104 chapter 07 practice | 2026-11-30 |
| 14.D monitoring of oversight | HITL dashboards and alerts | dashboard-id GRD-chat-05 | 2026-11-30 |
| 14.E stop capability | kill-switches + runbook | chapter 07; mod-107 runbook | 2026-10-30 (last dry-run) |

## Article 15 — Accuracy, robustness, cybersecurity (if high-risk)
| Obligation | Deliverable | Artefact | Last reviewed |
|---|---|---|---|
| 15.A accuracy | accuracy claim + eval registry | mod-104 chapter 04; online-eval history | 2026-12-01 |
| 15.B robustness | guardrails spec; fallback plan; robustness evals | chapters 02–03; mod-105 chapter 04; mod-104 | 2026-12-01 |
| 15.C cybersecurity | authorisation boundary; red-team cyber chapter | chapter 07; chapter 04 report | 2026-12-05 |
| Lifecycle | release-triad review clock | chapter 05 element 4 | ongoing |

## Article 50 — Transparency (always check)
| Obligation | Deliverable | Artefact | Last reviewed |
|---|---|---|---|
| 50.A "interacting with AI" disclosure | header strip + opening message | UI spec v3.2 | 2026-11-20 |
| 50.B machine-readable marking (if generating synthetic content) | C2PA + watermark config | pipeline config; sample audit | 2026-11-20 |
| 50.C deepfake / public-interest labelling (if applicable) | labelling mechanism + exceptions | UI spec; legal exceptions log | 2026-11-20 |

## Other obligations counsel has flagged
- Article 9 (risk management system) — see RMF document <link>.
- Article 10 (data and data governance) — see data-governance memo.
- Article 13 (transparency to deployers) — see model-card-equivalent for internal deployers.

## Open items
- 50.B text-watermarking approach pending vendor capability + technical guidance; interim approach: metadata-only; re-evaluate <date>.

## Sign-off
- Manager: <name>, <date>.
- Counsel: <name>, <date>.
```

The memo is as much counsel's artefact as the manager's. Both sign. It is re-read on the review cadence and when the product changes materially.

## Core concept 5 — anti-patterns to name and defeat

- **"The lawyers will tell us what to do."** They will tell you the law; they will not ship the kill-switch or the eval registry or the C2PA integration. The engineering-side obligations are engineering-side. The manager owns the mapping the lawyers verify.
- **"Transparency is just a disclaimer."** Article 50.A can be a disclaimer; 50.B and 50.C are mechanisms (marking, labelling) that require engineering work. A disclaimer in the privacy notice is not Article 50.B compliance.
- **The audit trail nobody tested.** The whole oversight-mechanism obligation presumes the audit trail works. Dry-run reading the audit logs at least once per quarter; if the on-call cannot pull the last 24 hours of tool calls or HITL decisions on request, the audit trail does not exist.
- **The accuracy claim that is not an eval threshold.** "We maintain high accuracy" is not Article 15.A compliance. A dated metric against a named golden set, maintained on an online-eval cadence, is.
- **Mapping once and not re-mapping.** The Act's application rolls out across years; the product changes across quarters; the model generation changes across months. The compliance mapping is a living document, not a one-off.

## What good looks like

- The compliance-mapping memo is dated, signed by counsel and the manager, and names the article's obligations against engineering artefacts.
- When a customer's procurement questionnaire asks "how do you comply with EU AI Act Article 14," the manager can answer with the deliverable list and the artefact links in under ten minutes.
- When a new feature ships, the mapping memo is reviewed as part of the release-triad gate (chapter 05); any new Article obligation is reflected in the mapping before launch.
- The transparency disclosures (50.A, 50.B, 50.C where applicable) are *enforced* by the system, not relied on per engineer discretion.
- Counsel is unsurprised by the mapping — they helped draft it, and the mapping cites their memos.

## What bad looks like

- Counsel wrote a two-page compliance memo; engineering has not read it.
- The product's chatbot does not announce it is an AI.
- Generated images do not carry C2PA content credentials.
- There is no accuracy threshold in the eval registry; "faithfulness ≥ 0.9" exists but no one re-runs it on the online-eval cadence.
- The authorisation boundary document was last updated before the agent gained three new tools.
- The first time the team discovers an Article's obligation applies is in a regulator inquiry.

## Summary

- Articles 14 (human oversight), 15 (accuracy, robustness, cybersecurity), and 50 (transparency) are the three EU AI Act sections an engineering team most often ships deliverables for. Articles 9 (risk management), 10 (data governance), and 13 (deployer transparency) are additional and counsel's call.
- Article 14 reads out to: HITL inventory (14.A), reviewer competence and information (14.B), automation-bias mitigation (14.C), oversight-mechanism monitoring (14.D), stop-the-system capability (14.E).
- Article 15 reads out to: accuracy claim + eval registry (15.A), robustness via guardrails + red-team + fallback + robustness evals (15.B), cybersecurity via authorisation boundary + cyber red-team (15.C), lifecycle maintenance via the release-triad review clock.
- Article 50 reads out to: in-product AI-disclosure (50.A), machine-readable marking via C2PA / watermarking (50.B), deepfake / public-interest labelling (50.C).
- The manager owns the per-product compliance-mapping memo linking each obligation to its artefact. Counsel verifies the mapping; the manager maintains it; it is re-read on the review cadence and before every material change.

## Where this module reads out

This chapter closes the module. The four manager-owned artefacts — guardrails spec, red-team commissioning brief, release-triad memo, agent authorisation boundary — plus the two cross-cutting reading disciplines (RSP / Preparedness / FSF; EU AI Act) are the operating system an AI engineering manager runs around product safety. The module's exercises ask you to produce each artefact for a real product you ship or plan to ship.

The next module (mod-107) picks up the incident side — what the on-call rotation does when something that passed the triad still behaves wrong. Chapter 05's pin, operationalised.
