# The Model + Data + Safety Release Triad

The eval-gated SDLC from mod-104 produces an eval score. The portfolio from mod-105 produces a model and a data posture. The guardrails spec and the red-team work from chapters 02–04 of this module produce a safety case. Three streams of evidence; three legs of a release gate. The manager's job, every time a feature of consequence ships, is to read all three and sign off — or refuse.

This chapter is the mechanics of that sign-off. Call it the release triad. The question the manager asks on every release:

> Can we ship this if I honestly see the eval score, the red-team findings and the safety case, and the data posture, and know the incident playbook will catch what we miss?

The triad is binding or it is a ritual. This chapter's job is to make it binding.

## Motivation — what the triad prevents

Three shipping disasters the triad is designed to prevent, each a composite of real patterns the industry has seen.

- **Great eval, bad safety.** The feature's eval score crosses the threshold. Nobody commissioned the red-team. Launch ships. Three weeks later a prompt-injection path turns the agent into a data-exfiltration tool. The eval was measuring quality on the happy path; the safety leg would have measured misuse resistance. The triad requires both.
- **Good safety case, bad data posture.** The feature's red-team engagement completed with no critical findings. The team shipped. Legal review the following month discovers the product sends untokenised PII to a US-based classifier covered by neither the DPA nor the EU-US Data Privacy Framework for this data class. Launch is pulled, the incident is reported, and the organisation loses a quarter. The safety leg caught misuse; the data leg catches "are we allowed to be doing this." The triad requires both.
- **Good gates, no playbook.** Eval green, safety green, data green. The product ships. Two months later, mid-afternoon, the content classifier returns false negatives on a specific attack pattern for ninety minutes; nobody is paged; by the time someone notices, three thousand bad responses have gone out. The failure was not in the gate; the failure was that the incident playbook from mod-107 never ran. The triad's fourth leg — the incident-playbook pin — is what connects the gate to the on-call.

## Core concept 1 — the three legs, named

The release triad is three legs plus a connecting pin. Each leg is a specific evidence requirement; each has a sign-off; the pin is the incident playbook that the on-call rotation uses after launch.

### Leg 1 — the model leg (mod-104, mod-105)

**Evidence:** the eval evidence from the eval registry (mod-104 chapter 04), read against the release-gate thresholds (mod-104 chapter 03); the selection memo from the portfolio (mod-105 chapter 03) committing the feature to a specific model and fallback.

**Sign-off:** the IC lead reviewer (typically the `applied-ai-engineer` or `ai-eval-engineer` of record for the feature) attests that the eval suite is complete and the thresholds are met. The manager counter-signs the selection memo and the eval attestation.

**What a "green" looks like:**
- Pre-merge evals pass at the release-gate threshold.
- The feature's shadow / canary / A/B evidence (mod-104 chapter 06) is above the floor the policy requires.
- The selection memo is dated within the review window and names the primary model, fallback, and the exit criteria.
- Known regressions are logged with mitigations; "we did not run this eval against this model because we ran out of time" does not count as green.

**Red flags that override green:** a shadow-eval signal below the online eval threshold; an unresolved sampled-online-eval pattern; a known eval the team has decided to "watch" rather than gate on.

### Leg 2 — the data leg (mod-105 chapter 05, mod-111)

**Evidence:** a one-page data-posture memo per launch, naming the data classes the feature processes (prompts, retrieved content, model outputs, tool-call payloads, user metadata, operational logs), the retention windows, the sub-processors involved (the model provider, the classifier provider, the embedding provider, the retrieval vendor, the logging platform), and the applicable contract clauses (DPA, BAA, HIPAA scope, FedRAMP scope where relevant).

**Sign-off:** procurement / legal have reviewed the data classes and signed the memo (usually a counter-signature on a legal-review ticket). The manager signs for the engineering side. For products touching regulated data classes (health, children's data, financial, EU-residency), the sign-off path is longer and named in advance.

**What a "green" looks like:**
- Every sub-processor the feature touches is in the organisation's approved sub-processor list, with a DPA in force.
- The data residency posture matches the customer-facing promises for this product surface; features serving EU users route through EU-residency inference and classifier endpoints, or carry an adequacy-decision basis the legal memo documents.
- Retention windows, logging, and redaction behaviours match the privacy-notice obligations the customer has been shown.
- Any data class classified "restricted" or "regulated" has an explicit contract path (BAA for PHI, standard contractual clauses or adequacy decision for EU personal data, etc.).

**Red flags that override green:** a safety classifier whose DPA posture nobody has read (chapter 03's rule 2); a logging platform the feature now sends prompts to without a DPA update; a retrieval source newly added that pulls customer data the DPA does not permit transferring to the model provider.

### Leg 3 — the safety leg (chapters 02, 03, 04)

**Evidence:** the guardrails spec (chapter 02) complete and reviewed; the guardrail stack (chapter 03) deployed and monitored; the red-team findings (chapter 04) with every finding carrying a disposition (fix shipped, mitigation deployed, accepted-risk entry signed, or release-blocking); automated safety-eval coverage running on every deploy.

**Sign-off:** the `ai-risk-engineer` (or the external vendor practitioner, for engagements run under contract) attests to coverage and lists any open critical or high findings. The manager signs for the engineering side.

**What a "green" looks like:**
- Guardrails spec dated within the review window; sections 1–6 complete; all OWASP LLM Top 10 in-scope categories addressed.
- Guardrail stack deployed in production with observability dashboards wired.
- Red-team findings: zero open criticals; open highs have mitigations deployed or accepted-risk entries signed; mediums have tickets in the backlog with SLAs.
- Automated safety-eval suite running in the pre-merge and canary cadences; false-negative rate monitored in production.

**Red flags that override green:** an open critical finding; an accepted-risk entry that was signed by the manager alone when the severity warranted a second signature (legal, security); a guardrail whose failure behaviour was changed since the last review and not re-tested; a classifier whose eval has not been refreshed against the current risk-model harms.

### The connecting pin — the incident playbook (mod-107)

**Evidence:** the incident playbook for this feature exists in the team's runbook repo; the on-call rotation has read it; at least one dry-run / tabletop has been run in the last quarter; the specific failure modes the three legs listed as "accepted risk" or "monitor" have runbook sections.

**Sign-off:** the on-call lead attests the rotation is briefed. The manager signs the playbook is current.

**What a "green" looks like:**
- Runbook pages exist for: content-classifier outage, content-classifier false-negative wave, prompt-injection incident, data-exfiltration via tool-use, PII leak via output, cost-blast-radius via loop, provider-outage with fallback (mod-105 chapter 04).
- Each page names the pager target, the kill-switch (if one exists), the stakeholder-notification matrix, the decision-tree for escalation to security / legal / customer-comms.
- The dry-run in the last quarter produced a written after-action; any gap found in the dry-run is fixed or tracked.

The pin is not a *fourth leg* — it is the thing that catches what the three legs miss. The three legs ask "is it safe to launch?"; the pin asks "when something that passed the gate still goes wrong, can we respond?" Mod-107 is the depth on the pin.

## Core concept 2 — the triad sign-off ritual

The manager runs the triad as a ritual, not as a side-channel. Four elements.

### Element 1 — the one-page launch memo

For every launch of consequence, a one-page launch memo pinned to the launch ticket. Sections:

- Feature name, launch date, severity tier.
- Model leg — green / amber / red; one-line summary; link to selection memo and eval evidence.
- Data leg — green / amber / red; one-line summary; link to data-posture memo and sub-processor diff since last launch.
- Safety leg — green / amber / red; one-line summary; link to guardrails spec, red-team report, open-findings list.
- Incident playbook — current / stale; link to the runbook pages; date of last dry-run.
- Known-issues / accepted-risks list for this launch, each with dated signature.
- Sign-off signatures — manager, IC lead reviewer, data owner (procurement / legal), safety reviewer, on-call lead.

The memo is boring on purpose. A manager who has to write a memo this specific *to themselves* on every launch stops finding ways to skip the gate.

### Element 2 — the ambers

Amber ratings are not "yellow lights to proceed cautiously." They are **named conditions** the launch ships *despite*, each of which carries a mitigation or an accepted-risk entry. Amber is a signal to the organisation that the launch is proceeding under a known imperfection, documented and timed.

Three example ambers from a real launch memo:

- *Model leg amber — one eval regression (hallucination rate +2%) accepted because the mitigating guardrail (citation-coverage check) reduces the user-facing impact; next review 2026-12-15 against the expanded golden set.*
- *Safety leg amber — one high red-team finding open (indirect-injection path in retrieved content) with mitigation deployed (input moderator extended to retrieved content); accepted-risk entry co-signed by manager and security lead.*
- *Data leg amber — new sub-processor (classifier vendor X) covered under existing DPA, pending counterparty signature on the updated appendix; green by 2026-12-01 or the launch is rolled back.*

A launch with three ambers can still ship. A launch with three ambers and no mitigations cannot.

### Element 3 — the override protocol

Overrides exist. The product has a demo on Friday; the feature has to ship. The protocol that keeps the triad honest:

- **Overrides are two-signature events.** The AI Engineering Manager plus one other authorised role — the engineering director, the VP of product, the head of risk / security — depending on which leg is being overridden.
- **Overrides are logged.** In the same launch-memo artefact, with the rationale and the time-bound commitment: "override valid for 10 days; by that date the overridden leg must be re-run green or the feature is rolled back."
- **Overrides do not apply retroactively.** A launch that shipped on an override is on a *shortened* review clock, not a longer one; the risks the override carried are tested *first*, not last.
- **Repeated overrides trigger a pattern review.** If the same leg is overridden twice in a quarter, the manager runs a pattern review on why — the gate may be set wrong (too strict for the surface), the team may be under-staffed on the practitioner needed for that leg, or the practice may have drifted. Chapter 03 of mod-104 covers the same pattern on eval gates; the same discipline applies here.

The protocol is binding or it is theatre. The ICs will recognise the difference within a quarter.

### Element 4 — the review clock

Every signed launch memo carries a *next review date*. The triad is not a one-shot — the model moves, the risk model moves, the data posture moves, the regulatory landscape moves. The review clock reads out to:

- A **monthly post-launch review** for the first three months after a feature of consequence launches — are the legs still green; did any ambers resolve; are the known-issues drawn down.
- A **quarterly re-sign** of the launch memo on steady-state features — the memo is not valid forever.
- A **triggered re-sign** on material changes: new model (mod-105 chapter 07); new classifier; new sub-processor; new HITL flow; new tool in the agent catalogue (chapter 07); a critical red-team finding against a related surface.

A launch memo that has not been re-signed in a year is not a valid gate; it is a shelf document.

## Core concept 3 — triad patterns at different severity tiers

Not every launch runs the full ritual. The triad scales with severity. Three tiers.

### Tier 1 — HIGH severity

Features whose worst-case failure could cause harm to a user, a customer, the company's standing, or a regulated-data breach. Clinical-adjacent, legal-adjacent, financial-advice-adjacent, children-facing, high-trust B2B, agentic with external side-effects.

Ritual: full triad. One-page launch memo with all five sign-offs. Red-team engagement scoped to the feature before launch (chapter 04). Incident playbook with a dry-run in the last quarter. Review clock monthly for three months, quarterly thereafter, triggered on any material change.

### Tier 2 — MEDIUM severity

Features whose worst-case failure is embarrassing, annoying, or materially costly but not harmful. Internal-tool launches, low-stakes consumer features, read-only agentic features.

Ritual: a lighter memo. Model leg (eval evidence) full. Data leg (sub-processor check, DPA diff) full. Safety leg: guardrails spec required; red-team engagement may be a subset of a broader quarterly engagement rather than feature-scoped; incident playbook required.

### Tier 3 — LOW severity

Internal experimentation; prototype toggled behind a feature flag to a small employee audience; developer tools used only by the team.

Ritual: light but real. A paragraph-length memo attached to the launch toggle, naming the model, the data flow, and the safety posture. Incident playbook may be a single "if anything looks wrong, roll back the feature flag" line — but the memo exists.

The severity tier is declared at spec time (chapter 02 section 1) and reviewed at the launch gate. A feature that cannot articulate its tier is a feature whose tier is HIGH until it can.

## Core concept 4 — anti-patterns the triad defeats

Four failure modes the triad is designed to rule out.

- **The signature chain where nobody read anything.** Four signatures on the memo, none of them from someone who read the underlying evidence. Prevention: the memo's sign-off line is "I have read the evidence linked above and found it adequate for launch at the declared severity tier." Signing is an attestation; a signature on a memo you did not read is a disciplinary matter, not a mistake.
- **The "known issue" dumping ground.** Every open issue the team cannot close before launch gets labelled "known issue" and ships. Prevention: known-issues entries have (a) a severity, (b) a mitigation line, (c) a dated signature, and (d) a named owner for follow-up. The list of known issues is a managed register; items age out; a known issue open for ninety days is either closed or escalated.
- **The gate without a rollback plan.** The triad signs off on launch but says nothing about what happens when a leg goes red after launch. Prevention: every signed memo names the **rollback trigger** and the **rollback target** — "if content-moderator false-negative rate exceeds X for Y minutes, roll back to version Z." The incident playbook executes this.
- **The gate the ICs stopped reading.** After two years of ritual the launch memo becomes a copy-paste form nobody reads. Prevention: rotate the sign-off requirements — periodically, require the IC lead to attach a two-paragraph narrative explaining *why* the memo is green this release, not just the checkboxes. The narrative forces re-reading.

## A worked skeleton — the launch memo

```markdown
# Launch memo — Healthwise Chat v2.3
Date: 2026-12-10 · Severity tier: HIGH · Owner: <manager>

## Legs
### Model leg — GREEN
- Eval report: run-id 2026-12-08-H3; faithfulness 0.92 (≥ 0.90), harm-eval false-negative rate 0.04%.
- Selection memo: Claude <snapshot> primary, GPT-4o <snapshot> fallback; selection-memo-2026-11-15.
- Shadow / canary: seven-day shadow against prod traffic, no net regression vs. v2.2.
- IC attestation: <name>, <date>.

### Data leg — AMBER
- Sub-processor diff since v2.2: added <classifier vendor X> for output moderation.
- DPA posture: existing enterprise DPA with <vendor X> covers EU residency and no-training-on-input; appendix for Healthwise-specific logging retention pending signature, expected by 2026-12-15.
- Residency: EU users route to EU endpoint; US users route to US endpoint; India users excluded until Q1 2027 pending residency build-out.
- Amber resolution: on 2026-12-15 the appendix is counter-signed OR v2.3 is rolled back.
- Procurement / legal sign-off: <name>, <date>.

### Safety leg — AMBER
- Guardrails spec: dated 2026-11-20; sections 1–6 complete; OWASP LLM Top 10 coverage matrix green.
- Red-team engagement: scoped to v2.3 by <ai-risk-engineer name>, completed 2026-12-05.
  - 0 critical, 1 high open, 2 medium open, 4 low closed.
  - High open: indirect-injection path via retrieved health articles — mitigation deployed (retrieved content moderator; structural separation); accepted-risk entry #AR-2026-14, co-signed manager + security lead, expiry 2027-06-15.
- Automated safety eval: running pre-merge and canary; false-negative rate monitored dashboard-id #GRD-chat-07.
- Safety sign-off: <ai-risk-engineer name>, <date>.

## Incident playbook — CURRENT
- Runbook pages: classifier outage, false-negative wave, injection incident, PII leak via output, crisis-line handoff failure.
- Last dry-run: 2026-10-30 (false-negative wave scenario); after-action gaps closed.
- Rollback trigger: false-negative rate > 0.3% for 10 minutes, OR classifier error rate > 5% for 15 minutes.
- Rollback target: v2.2 (last clean memo-signed version).
- On-call lead attestation: <name>, <date>.

## Known-issues / accepted-risks for this launch
- #AR-2026-14 — indirect-injection via retrieved content (described above).
- #KI-2026-22 — crisis-handoff UX has a 400ms animation stutter on low-end Android; cosmetic; T&S queue unaffected; fix in v2.4.

## Sign-offs
- Manager: <name>, <date>.
- IC lead reviewer: <name>, <date>.
- Data owner (procurement/legal): <name>, <date>.
- Safety reviewer: <name>, <date>.
- On-call lead: <name>, <date>.

## Next review
- 2027-01-10 (monthly post-launch).
- On any change in model snapshot, classifier, sub-processor, or HITL flow.
```

The template is deliberately spartan. The memo is one page; the evidence it links to is many pages. The point of the memo is that the manager can read it in five minutes before signing and know what they are signing.

## Summary

- The release triad is three legs — model, data, safety — plus a connecting pin (the incident playbook). Each leg has an evidence requirement, a sign-off path, and a definition of "green."
- The ritual is a one-page launch memo per launch of consequence, with amber conditions explicit, overrides two-signature and time-bound, and a review clock that re-reads the memo monthly then quarterly.
- The triad scales to severity tier — HIGH features run the full ritual; MEDIUM a lighter version; LOW a paragraph — but no feature ships without an attestation.
- Four anti-patterns to prevent: signatures without reading, "known issues" as a dumping ground, gates without a rollback plan, gates the ICs stopped reading. Each has a specific preventive discipline.
- The pin — the incident playbook — is where mod-107 picks up; the triad decides whether to ship, mod-107 decides what to do when something that shipped behaves wrong.

The next chapter picks up the public-provider governance frames — Anthropic RSP, OpenAI Preparedness, DeepMind FSF — as the input to deployment-tier decisions the manager makes when the model portfolio touches a new capability tier.
