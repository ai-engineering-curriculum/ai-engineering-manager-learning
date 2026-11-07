# Reading a DPA and Partnering with Procurement

A Data Processing Addendum (DPA) is the contract clause that governs what the vendor is allowed to do with the data your team sends it. Every enterprise AI vendor publishes one. Most teams sign the vendor's standard DPA without reading it; a smaller number of teams read it carefully once at onboarding and never again, even when the vendor amends it. Both are mistakes, and both are the manager's accountability — not legal's, not procurement's, not the IC's.

The AI Engineering Manager's job in this chapter is to (a) know the clauses that matter, (b) hold the redline list the organisation defends on every renewal, (c) route legal opinion to counsel and contract execution to procurement without doing either job themselves. Chapter 01's deferral principle: **read, flag, escalate; do not negotiate solo and do not sign blind.**

## Motivation

Three failure shapes recur when DPAs land on the manager's desk unread.

- **The training-on-data surprise.** The vendor's consumer-tier terms permit training on API traffic; the enterprise tier excludes training by default. The team signed up on the consumer tier because it was the fastest way to get an API key, and the enterprise upgrade was never routed through procurement. Six months in, the organisation's own customer DPA promises no third-party training; the organisation is in breach and does not know it.
- **The silent DPA amendment.** The vendor updates the DPA mid-contract. The vendor's notification lands in an email address nobody owns ("legal@" or a shared DL that is not read). The amendment adds a sub-processor, changes a retention window, or weakens a clause. The team discovers it six months later when a customer asks "who is your vendor's sub-processor for region X?" and nobody knows.
- **The redline drift.** Legal approved the vendor's DPA two years ago with specific redlines. The redlines were not written down; they were a conversation. A new deal with a new vendor starts, procurement asks "what are our standard redlines," legal asks "what did we do last time," nobody has the answer. The organisation rebuilds its redline list from scratch every contract.

The manager's redline list, kept in writing and maintained quarterly, prevents all three.

## Core concept 1 — the clauses that matter

Not every clause in a DPA warrants manager attention. These do. Read each on every vendor contract; flag any deviation from the organisation's standard.

### Training on your data

The single most-asked question from enterprise customers about AI vendors. Three postures, in increasing strictness:

- **Permitted by default.** Vendor may use API inputs and outputs to improve / train its models. Typical of consumer tiers and free-API access. **Non-starter** for any production feature touching customer data unless the organisation's customer DPA explicitly permits it.
- **Permitted with opt-out.** Vendor trains by default; customer may opt out via account setting or amendment. Requires a durable mechanism (not "we flipped it last year") — the manager owns verification that the opt-out is still in effect at each quarterly review.
- **Excluded by default.** Vendor does not train on API inputs or outputs under the commercial / enterprise terms. The current posture of the enterprise tiers of major frontier providers (OpenAI Enterprise, Anthropic API Services, Google Cloud Vertex AI enterprise terms, Azure OpenAI, AWS Bedrock). The clause to cite in the DPA is usually named "Confidentiality" or "Customer Data" or an addendum called "Data Usage".

Also watch for the asymmetric case: *inputs* excluded from training but *outputs* retained for quality monitoring with a human-review loop. Some providers draw this distinction; it may be acceptable for some surfaces and not for others. The team's own eval and data-flywheel discipline (mod-104 chapter 07) interacts with this.

### Data retention

How long does the vendor retain inputs and outputs? Default retention windows vary — common patterns are 30 days for abuse-monitoring, zero for "zero data retention" (ZDR) qualified customers, longer for Enterprise tiers that bundle audit retention. ZDR is often an **amendment**, not a default — the manager asks procurement whether the organisation's contract is on ZDR, and verifies annually.

Pay attention to the *abuse-monitoring exception*: most providers retain enough data to detect misuse even on ZDR customers. This is usually acceptable but is a line item the DPA will carry; it should not be a surprise to a customer who reads the organisation's own DPA and then asks about the vendor's.

### Data residency

Where is data processed, and where is it stored, under the contract? Four axes:

- **Processing residency.** The region(s) where inference runs. Enterprise tiers of major providers now offer region selection — US, EU, UK, Canada, Japan, Australia, India. Some offer explicit pins (requests from EU go only to EU endpoints); some offer a best-effort routing. Read which.
- **Storage residency.** For any data the vendor retains (traces, abuse monitoring, feedback), where is it stored? Processing region and storage region are sometimes different; the DPA will say.
- **Sovereign-cloud variants.** AWS GovCloud, Azure Government, Google Sovereign Cloud, and specific EU-sovereign offerings (Mistral is EU-headquartered; some Vertex / Bedrock regions carry additional sovereign guarantees). The team's residency commitments may require one of these for a specific regulated subset.
- **Transfer mechanism.** For EU → US transfers specifically, the DPA will cite Standard Contractual Clauses (SCCs) and typically a reference to the EU-US Data Privacy Framework. Counsel owns the opinion on whether a specific mechanism is sufficient for the organisation's jurisdictions.

For regulated features (HIPAA, GLBA, FCRA, GDPR, CCPA, PIPEDA, LGPD), residency intersects with the underlying regulation. Chapter 05 of mod-106 (safety and governance) is specific about the governance partners.

### Sub-processors

Every enterprise DPA has a sub-processor list — the third parties the vendor uses to deliver the service (hosting infrastructure, logging services, abuse-monitoring vendors, payment processors). Three things to look for:

- **Is the list published and current?** The vendor should maintain a public or accessible list. If the only way to see the list is to ask, that is a redline.
- **What is the change-notification period?** Thirty days is common; some vendors offer fifteen, some ask for ten. The manager wants a window to assess (and potentially object to) new sub-processors before they take effect.
- **Does the vendor flow down obligations?** The DPA should state that sub-processors are bound to the same (or substantially the same) data-processing obligations as the primary vendor. Flow-down failure is a common auditor-finding.

The manager's practical artefact: a quarterly check that the current sub-processor list matches what the organisation's customer DPA has disclosed to its own customers. If a vendor has added a sub-processor the organisation's customers have not been told about, the disclosure is the manager's to initiate.

### Deletion rights and data subject requests

Can the organisation delete the data the vendor holds? Under what SLA? The DPA will state a window (often 30 days, sometimes longer for backup-retention cycles). Data-subject-request (DSR) obligations under GDPR / CCPA / similar flow through the vendor: the DPA will state the vendor's cooperation obligation and the SLA for responding to DSR requests routed via the organisation.

The manager's job: confirm the DSR SLA matches the organisation's own customer SLA, so a customer asking for deletion cannot produce a chain where the organisation promises 30 days and its vendor is on 60.

### SLA, credits, and remedies

The service-availability SLA (uptime percentage) and the remedy when it is breached (service credits, typically — rarely refunds). Standard default is often 99.9% monthly; enterprise negotiates higher (99.95%, 99.99%) in some contracts. The remedy is usually credits — a fraction of the monthly fee — not damages. Credits are commercially modest compared to the cost of a product outage; **the SLA does not make the fallback plan optional** (chapter 04).

Related clauses: *force majeure* (what the vendor is excused from), *maintenance windows* (scheduled downtime excluded from the SLA), *latency SLA* (rarer, but where offered, worth reading). For production-critical features, procurement often negotiates a committed-use capacity or a reserved-throughput tier (OpenAI Scale Tier, AWS Bedrock Provisioned Throughput, Azure PTU, Google Vertex Provisioned Throughput) — the terms for reserved capacity are a separate contract annex and carry their own posture.

### Indemnity

Three indemnities land in AI-vendor DPAs with increasing frequency:

- **IP indemnity on vendor outputs.** The vendor will defend the customer against third-party claims that the model's output infringes a copyright, patent, or trade secret. Several frontier vendors now offer this to enterprise tiers as a differentiator. Read the scope carefully: typical exclusions are (a) customer uses output in a way the provider's AUP forbids, (b) customer has materially modified the output, (c) customer has disabled a content-filter or safety-feature the provider required.
- **Data-breach indemnity.** The vendor indemnifies the customer for breaches caused by the vendor. Usually subject to a liability cap. Read the cap; some are low enough to be commercially meaningless.
- **Confidentiality / DPA breach.** General counterparty obligation.

Counsel owns the opinion on whether a specific indemnity is sufficient. The manager's job is to flag the three indemnity clauses to counsel and wait for the opinion before the deal closes.

### Confidentiality and restrictions on vendor's use

Beyond training-on-data: can the vendor use the customer's name as a reference? Publish a case study? Aggregate customer data for benchmarking (even if de-identified)? Use the data for any "legitimate business interests" clause that functions as a residual permission? Standard enterprise DPA language draws these lines; read them.

### Audit rights

The customer's right to audit the vendor's data-processing practices. Standard enterprise language is typically "audit via third-party attestation" (SOC 2 Type II, ISO 27001) rather than a direct customer audit. A direct-audit right is uncommon and is often not worth the redline fight; a clear right to receive and review the SOC 2 Type II letter annually is the practical equivalent and is defensible in the organisation's own customer audits.

### Term, termination, and transition assistance

How does the contract end, and what does the vendor commit to during wind-down? Watch for (a) auto-renewal clauses with short notice windows, (b) termination-for-convenience rights for the customer, (c) transition-assistance obligations (vendor must provide reasonable assistance to migrate), (d) data-return-or-deletion-at-termination SLA.

### Model deprecation and change notice

An AI-specific clause that is now standard in several providers' terms: how much notice does the vendor give before deprecating or retiring a model? Many frontier providers now publish a formal deprecation policy with a notice window (commonly 6–12 months for frontier-tier models, less for preview models). The manager's practical artefact: the portfolio inventory (chapter 02) carries the deprecation policy per vendor, and the quarterly review (chapter 07) checks for new deprecations.

## Core concept 2 — the redline list

The redline list is a one-page artefact the organisation defends on every vendor DPA. It is maintained by the AI Engineering Manager (or a security / governance peer, depending on org shape) and renewed quarterly against the current customer DPA language. Its purpose: procurement knows what to insist on, counsel knows what has been approved, the vendor knows what will be countered.

A typical redline list carries, for each clause:

- **The organisation's position.** One sentence.
- **The minimum acceptable fallback.** If the vendor refuses the primary position, what is the next-tier ask.
- **The escalation point.** The clause this cannot be settled on without counsel's sign-off (as opposed to procurement's).

Example rows (illustrative — your organisation's own redline list is informed by your counsel and your customer DPA):

- *Training on customer data.* Position: excluded by default under commercial terms, with no opt-in. Minimum fallback: explicit opt-out mechanism documented and verifiable annually. Escalation: counsel.
- *Data retention.* Position: zero retention (ZDR) for inputs and outputs; abuse-monitoring exception is acceptable if scoped and SLA-ed. Minimum fallback: 30-day retention with documented access controls. Escalation: counsel for ZDR terms, procurement for retention window.
- *Sub-processor change notice.* Position: 30 days' notice; customer right to object with reasonable grounds. Minimum fallback: 15 days' notice with same objection right. Escalation: procurement.
- *Data residency.* Position: processing and storage pinned to `[region]` for identified data categories. Minimum fallback: best-effort routing with documented monitoring. Escalation: counsel if a specific regulation applies; procurement otherwise.
- *SLA.* Position: 99.95% monthly uptime with service credits scaling to full monthly fee at severe breaches. Minimum fallback: 99.9% with standard credits. Escalation: procurement.
- *IP indemnity on outputs.* Position: offered; uncapped for the enterprise tier. Minimum fallback: capped at annual contract value or substantial multiple thereof. Escalation: counsel.
- *Model deprecation notice.* Position: 12 months for stable-tier models. Minimum fallback: 6 months. Escalation: AI Engineering Manager (this is the clause that is often the manager's unique ask).
- *Audit rights.* Position: SOC 2 Type II letter annually; penetration-test summary on request. Minimum fallback: SOC 2 Type II letter annually only. Escalation: security peer.
- *Termination assistance.* Position: 30-day wind-down with full API availability and data export. Minimum fallback: 15-day wind-down. Escalation: procurement.

The list is a living artefact. The quarterly review (chapter 07) updates it against (a) changes in the organisation's customer DPA, (b) new regulatory developments, (c) outcomes from the last quarter's negotiations.

## Core concept 3 — the escalation model

Three audiences take the DPA from the manager. Knowing which clause goes where is the whole point of this chapter.

### To counsel

Anything that is a **legal interpretation** or requires a **binding opinion**:

- Indemnity clauses (all three).
- Transfer mechanisms (SCCs, Privacy Framework interpretation).
- Specific regulatory applicability (HIPAA BAA sufficiency, GLBA, GDPR Article 28 compliance, CCPA-specific language).
- Audit-rights clauses in contested form.
- Termination-for-cause language in contested form.
- Any clause the vendor has countered with language the organisation has not seen before.

Counsel's deliverable is an opinion ("this is acceptable / not acceptable / acceptable with language X"). The manager carries the opinion back into the negotiation as a position, not as an argument.

### To procurement

Anything that is a **commercial negotiation**:

- SLA levels and credit amounts.
- Pricing tiers, committed-use discounts, reserved-capacity contracts.
- Term length, auto-renewal, notice periods.
- Change-of-control clauses.
- Vendor-relationship management day-to-day.

Procurement's deliverable is an executed contract. The manager provides the technical position; procurement runs the negotiation and signs.

### To the AI Engineering Manager (you)

Anything that is a **technical / operational decision**:

- Model-deprecation notice period (nobody else knows what the team needs).
- The list of models under contract (the inventory in chapter 02 is yours).
- Residency and data-posture decisions tied to a specific feature's design.
- Capability-specific contract terms (fine-tune availability, batch pricing, prompt-caching discount).
- Rate-limit / quota negotiations at the technical tier.

These do not escalate up; they are the manager's call, with procurement executing.

A clean DPA review is the manager reading the draft, marking each clause with its escalation target, and routing in parallel. Procurement and counsel run their reviews independently; the manager consolidates and comes back with a single list of counters.

## Core concept 4 — the renewal and the quarterly review

Contracts renew; DPAs get amended between renewals. Two cadences.

### At renewal

Procurement initiates the renewal cycle; the manager opens the redline list as the starting point, consults the past year's incidents / escalations for new positions, and provides the counter-list to procurement. The eval evidence (chapter 02 inventory) is a renewal input — if the vendor's quality has drifted, the leverage is real; if it has grown, the renegotiation posture softens.

### Quarterly, between renewals

The quarterly portfolio review (chapter 07) explicitly checks:

- Has the vendor amended the DPA? (Subscribe to the vendor's DPA page; the vendor is often obligated to notify but not always well.)
- Has the sub-processor list changed? (Many vendors maintain an RSS or email subscription for sub-processor changes; subscribe.)
- Has the vendor published a deprecation notice? (The portfolio inventory needs updating.)
- Has the vendor shipped a new DPA feature (ZDR newly available, HIPAA BAA newly available, a new region, a new enterprise feature) that the team should adopt?

The quarterly review produces a one-page memo that goes to procurement and, where relevant, to counsel.

## A worked example — a DPA redline session

A hypothetical vendor's standard DPA lands for review. The manager walks the clauses:

- *Training on data.* DPA says "vendor may use inputs and outputs to improve service." Redline: enterprise tier must exclude training by default; cite the organisation's own customer DPA as the business reason. Escalate to counsel; procurement countersigns.
- *Retention.* DPA says "inputs retained for 90 days for abuse monitoring, outputs not retained." Position: 30 days acceptable; 90 days is longer than the organisation has been representing to its own customers. Redline, escalate to counsel for sufficiency.
- *Sub-processors.* DPA references a public list; notice period is 10 days. Redline: 30 days with objection right. Escalate to procurement.
- *Residency.* DPA offers US-only; team needs EU processing for `[feature]`. Non-starter; the DPA cannot be signed as-is for this feature. Escalate to procurement to find out if EU is available on a different tier.
- *SLA.* 99.9% monthly, credits capped at 10% of monthly fee for 99.0-99.9%, 25% for below 99.0%. Acceptable for the current feature; for the next, higher-stakes feature, procurement will reopen.
- *IP indemnity.* Not offered. Redline: must be offered for the enterprise tier; the team's outputs reach external customers. Escalate to counsel.
- *Deprecation notice.* Not addressed. Redline: must offer 6+ months' notice on stable-tier models. Flag for manager's own ask in the counter.
- *Audit rights.* SOC 2 Type II letter annually. Acceptable.
- *Termination.* 30-day wind-down; data-return supported. Acceptable.

The manager produces the counter-list in one page. Procurement takes it to the vendor. Counsel's opinions on indemnity and residency land in parallel. The negotiation runs in weeks, not months, because the manager's work has already pre-digested it.

## What good looks like

- Every vendor DPA on file is dated with its last review; staleness is visible.
- The redline list exists in one page; procurement and counsel both know where it lives.
- Clauses are mapped to escalation targets; counsel reviews only what needs a legal opinion, procurement runs only what needs a commercial negotiation, the manager owns the technical positions.
- Sub-processor change notifications land in a monitored inbox; quarterly reviews actually open the vendor's list.
- Model-deprecation notices surface at the portfolio review within the same quarter they are issued.
- A new vendor onboarding takes weeks, not months, because the standard counter-list is already written.

## What bad looks like

- The team is on consumer-tier terms because the engineer who onboarded first signed up with a credit card; nobody has elevated to enterprise.
- The DPA was read once at onboarding and has not been read since; the vendor has amended it twice.
- Counsel reviewed the DPA two years ago; the clauses they approved are not written down and the next deal is being negotiated from scratch.
- The redline list does not exist; procurement asks "what do you want" on every contract and the manager improvises.
- The sub-processor list is checked only when a customer asks; the organisation's own customer DPA is out of date.
- The model-deprecation clause is not in any contract because the manager did not ask for it; models are deprecated with 90 days' notice and the team scrambles twice a year.

## Summary

- A DPA has roughly ten clause families the manager reads on every vendor contract: training on data, retention, residency, sub-processors, deletion rights, SLA and remedies, indemnity, confidentiality, audit rights, term and deprecation notice.
- The manager's artefact is a one-page redline list: the organisation's position, minimum fallback, and escalation target per clause. Maintained quarterly.
- Three escalation targets: counsel (binding opinion), procurement (commercial negotiation), the manager (technical and operational).
- A clean review runs in parallel: the manager routes the clauses, counsel and procurement produce their outputs, the manager consolidates and comes back with one counter-list.
- The quarterly portfolio review checks for DPA amendments, sub-processor changes, and deprecation notices between renewals. At renewal, the redline list and the eval evidence frame the counter-position.

The next chapter picks up the honest answer to a question that reappears every quarter: when does self-hosting an open-weights model actually earn its place, and how is that decision made in partnership with the Infra ladder rather than in isolation.
