# What This Role Does NOT Do — the Four Deferrals That Keep the Partnership Honest

Cross-functional partnership works when every partner is operating inside their scope. The preceding chapters drew five surfaces the AI Engineering Manager (L45) actively owns — the release-approval workflow, the consumption interface with the AI Governance family, product collaboration under non-determinism, the Infra peer relationship, the AI Workforce sibling surface. Each is defined as much by what the manager owns as by what the manager hands off. This final chapter closes the module on the handoffs themselves: four specific places where the AI Engineering Manager is tempted to drift into another partner's scope, each with a predictable shape and a predictable harm.

The drift is always for a plausible reason. The work is adjacent to something the manager already touches. The alternative — waiting on a partner whose queue is slower than engineering's cadence — looks like a tax the team cannot afford. The manager has read enough of the partner's canon to feel competent on a specific question. And nobody in the room is stopping the drift, because the person with the standing authority to stop it is not in the room, which is precisely how the drift was able to start. The deferral discipline is the counter-pattern: four partners, four scopes, four hand-offs the manager performs instead of the work itself. This chapter names each deferral, the characteristic failure that follows when it is blurred, what the manager contributes inside the partner's surface, and the common drift to resist.

## Motivation — four failures when the deferral discipline is absent

Each of the four scopes below has a characteristic failure shape. None of them is an incident in the mod-107 sense. Nothing crashed on the day. Each one costs the team a partner's trust, and the trust is harder to rebuild than the launch was to run.

- **The legal position the manager posted that counsel had to retract.** A product manager asked in a team-wide Slack channel whether the team was "covered" under the EU AI Act for a specific agentic feature. The AI Engineering Manager, wanting to unblock, replied: "We're fine — this feature isn't in Annex III scope." Plausible, and a regulatory determination the manager is not authorised to make. Counsel found the message two days later and had to post a formal retraction. Nothing crashed. The manager imported an unknown amount of regulatory risk, undermined counsel's authority with the engineering function, and lost standing with the Slack audience that had read the original position as official.
- **The shadow pricing tier Procurement found at renewal.** The vendor's account executive, familiar with the manager from a conference, emailed directly with a "special enterprise offer" — committed-use discount in exchange for a volume uplift. The manager, treating it as a technical conversation about the right pricing tier, agreed in writing to the shape of the offer. Six weeks later Procurement discovered the exchange during the renewal cycle and was now negotiating from a baseline the vendor could cite as "already agreed by the engineering lead." The informal response had collapsed Procurement's leverage.
- **The SSO integration the manager provisioned around IT identity.** A vendor tool offered SAML SSO with a self-service admin console. IT identity's queue was weeks out; the team wanted the tool in sprint. The manager configured the integration through the vendor's admin console using a shared service account. Months later the organisation rotated the underlying auth credentials and the integration broke — because IT identity did not know it existed, it was not in the rotation runbook, and offboarding flows did not touch the vendor account. The failure stayed invisible until an audit found an ex-employee still had an active login.
- **The Slack-based incident response the SOC could not reconstruct.** The agent started producing outputs that referenced documents outside users' retrieval scope. The on-call engineer treated it as retrieval-store corruption, rolled the retrieval index back to a known-good snapshot, and closed the ticket. Three weeks later a broader security investigation found the same window had evidence of credential compromise upstream of the retrieval store — an active attacker had been reading documents through the agent as a reconnaissance channel. The engineering-side response had mitigated the symptom and destroyed the chain-of-custody the SOC needed.

Each failure is a *deferral failure*, not a competence failure. The manager was competent enough to act; the problem is that the person with standing authority on the question was not the one acting.

## Core concept 1 — the four deferrals

Four partners the AI Engineering Manager (L45) explicitly does not supplant. The chapter 01 release-approval altitude named them; this chapter is the detailed read.

1. **Legal opinion → counsel.** Counsel owns the organisation's legal position: which clauses are enforceable in which jurisdiction, which regulations apply to which features, which disclosure obligations attach to which events, which language is safe to publish externally.
2. **Contract execution → procurement / finance.** Procurement owns the commercial relationship with every vendor — negotiation, counter-signature, renewal calendar, escalation on commercial disagreements. Finance owns payment and billing mechanics.
3. **SSO / IAM implementation → IT identity.** IT identity owns the organisation's identity substrate — SSO integration, SCIM provisioning, MFA enforcement, break-glass access, service-account governance, offboarding automation.
4. **SOC-side investigation → security operations / DFIR.** SecOps and Digital Forensics and Incident Response own attacker attribution, chain-of-custody, cross-environment pivot analysis, coordinated disclosure, and law-enforcement engagement.

Each has been named elsewhere in the track. Mod-107 chapter 07 teaches the SOC hand-off in the context of the regulator-facing incident narrative. Mod-110 chapter 07 teaches the Procurement and Finance partnerships and names counsel as the legal-opinion deferral on DPA clauses. Mod-106 R15 covers the agent / tool-use authorisation story that makes the IT-identity deferral load-bearing. This chapter is the *partnership-angle* consolidation.

## Core concept 2 — legal opinion deferral

The pattern mod-110 chapter 07 opened — *read, flag, escalate; do not opine solo* — holds across every legal-adjacent surface, not just DPA clauses. The surfaces this chapter extends it to: regulatory-scope reads ("does this feature fall under EU AI Act Annex III?"), incident-disclosure determinations ("do we need to notify under Article 73?" — the mod-107 chapter 07 question), customer-facing communications that make legal claims, training-data provenance claims, and any public position the organisation might be held to.

What the manager **can** say:

- Engineering positions. "This DPA clause on training-on-data is a redline for us because the feature processes customer content we have committed not to re-use for training." That is an engineering requirement, not a legal determination.
- Factual positions about the feature's behaviour. "This feature processes data in categories X, Y, Z." "This feature does not retain user inputs beyond the current session." Facts counsel can rely on when writing the legal position.
- Routing statements. "I'm holding on this and opening it with counsel; expect a confirmed answer by $date."

What the manager **cannot** say without counsel's sign-off:

- Enforceability claims. "This clause is unenforceable in California."
- Regulatory-scope determinations. "This feature is not in Annex III scope of the EU AI Act." "This system is out of scope for HIPAA."<!-- needs-research: specific regulation names and scope tests should be confirmed against counsel's current advisory list before publication -->
- Disclosure-obligation determinations. "We don't need to notify the regulator because the incident doesn't meet the serious-incident threshold."
- Compliance statements of record. "We comply with the EU AI Act." "We are SOC 2 certified." Even when substantively true, publishing a compliance statement without counsel's sign-off is the act the deferral forbids.

The signal: *if the manager is drafting language that would be referenced in a dispute — regulator, auditor, customer counsel, litigation — counsel should be the author, not the editor.* The manager's job is to keep the factual content accurate; counsel's job is to decide what the organisation says about the law. The partnership-angle addition: the discipline covers not just contracts and filings but *every surface where the manager has a Slack audience*. A message read by thirty engineers that cites a regulatory position is, functionally, a published legal position for those thirty engineers; counsel's retraction can recover the record but not the first read.

## Core concept 3 — contract execution deferral

Mod-110 chapter 07 established the Procurement partnership mechanics. This chapter extends the same pattern across every vendor-adjacent conversation.

What the manager **owns**:

- The **list of asks** Procurement negotiates on. Enterprise features the team needs (data-flow controls, higher rate limits, enterprise support, enterprise SLA, DPA addenda per mod-105 chapter 05, deprecation-notice floor, model-pinning guarantees); the pricing tier the team can commit to; the discount justification against committed volume.
- The **business case** for every ask — why the feature matters, which initiatives on the roadmap depend on it.
- The **alternatives** from mod-105's portfolio inventory — which other vendors cover the same surface, at what cost, at what switching cost. Procurement's leverage is a function of how credibly the organisation could walk away, and only engineering can supply the credibility.

What Procurement **owns**:

- The **negotiation mechanics** — the vendor's commercial-contacts protocol, the sign-off path, the counter-signature, the renewal calendar.
- The **vendor relationship on commercial terms** — the account-executive relationship on pricing, contract, and commercial escalation.
- The **counter-signature and the paper trail**.

The partnership-angle addition: the manager does **not** take escalations from the vendor's account executive directly on commercial terms. If the AE emails with a "special offer" or a "committed-use tier" proposal, the correct response is to acknowledge and route. "Thanks — I'm looping in our procurement partner on commercial terms." The vendor is likely to try the manager repeatedly because the manager is softer to negotiate with; the discipline is to be consistently the wrong audience for the commercial conversation.

An important distinction: when a vendor's **technical account team** (solutions engineer, technical account manager, developer-relations partner) engages with the manager on engineering matters — eval methodology, model-version migration, API usage, fine-tune workflow, serving configuration — that is the normal engineering partnership and does not route through Procurement. The deferral covers commercial terms, not technical collaboration. The line is between *what to build with the vendor* (engineering's conversation) and *what to pay the vendor* (Procurement's conversation). A second extension: **Finance** owns the billing mechanics and the chart-of-accounts attribution. Mod-110 chapter 02 is the cost-model artefact; invoice processing, dispute resolution with the vendor's billing team, and variance explanations that land with the CFO are Finance's work.

## Core concept 4 — IT identity deferral

IT identity is the deferral most likely to be invisible to the AI Engineering Manager, because "configure a vendor tool's SSO" looks like a technical integration engineering can obviously handle. It is also the deferral where the drift is least recoverable, because the misconfigurations compound silently until an audit or a rotation surfaces them.

What IT identity **owns**:

- The **SSO substrate** — the identity provider (Okta, Entra ID, Google Workspace, or the organisation's equivalent), the SAML / OIDC / OAuth integration pattern the organisation enforces, the integration catalogue, attribute-mapping conventions.
- **SCIM provisioning** and lifecycle — just-in-time user provisioning, group-based access, automated offboarding on termination. A vendor tool outside SCIM is a vendor tool whose ex-employees keep access after they leave.
- **Enterprise auth policies** — MFA enforcement, device-posture requirements, conditional-access, session-timeout policies, break-glass patterns.
- **Service-account governance** — non-human identities, their lifecycle, secret rotation, scope. In the AI engineering context this is load-bearing: the agent / tool-use authorisation story mod-106 R15 teaches depends on IT identity holding the line that agents use scoped, rotated, auditable identities — not a shared "team service account."

What the manager **owns**:

- The **AI-feature-side auth requirements**. "This agent must authenticate as the user, not as a shared service identity, because R15 agent-authority boundaries require it." "This retrieval surface must enforce the caller's row-level access, not the service's." "This tool-use path requires scoped delegation, not a static API key." The manager is the one who knows which auth shape the AI feature requires because only the manager has the mod-106 R15 context.
- The **engineering-side implementation** of the OAuth / OIDC flow, the token-exchange logic, the authorisation-check wrapping on tool-use.
- The **read of IT identity's auth policy** to confirm fit — enough of the integration catalogue and service-account policy to recognise when a vendor's auth shape does not fit before engineering commits to adopting the tool.

The common failure: the manager provisions a vendor tool's "SAML SSO" through the self-service admin console because the IT identity queue is weeks out. Three predictable harms follow: the integration is not in SCIM, so offboarding does not touch the vendor account; the integration is not in IT identity's catalogue, so the next auth rotation breaks it silently; and the integration's security posture (attribute mapping, session timeout) is whatever the vendor's defaults happen to be, not what the organisation's policy requires.

The partnership-angle pattern: *every new vendor tool with auth implications is an IT identity ticket; the manager scopes the ticket but does not implement the integration.* When the IT identity queue is slower than the roadmap requires, the escalation path is the IT identity manager — a partner-to-partner conversation — not a workaround through the vendor's admin console. A cross-reference: mod-106 R15 is the module-level treatment of agent / tool-use authorisation; the IT identity deferral is the partnership that makes R15 implementable. A team that owns R15 at engineering depth but has no partnership with IT identity will produce agents whose authorisation model is correct in principle and brittle in practice.

## Core concept 5 — SOC-side investigation deferral

Mod-107 chapter 07 opened this deferral in the context of the regulator-facing incident narrative; chapter 01 of this module named it in the release-approval altitude. This chapter deepens it on the partnership-angle axis.

What the manager **does** in an AI-shaped incident:

- **Detect, mitigate, and stabilise** at the AI surface. The four mitigation levers mod-107 chapter 04 named — kill-switch, route-to-safer-model, rollback, HITL-tightening — belong to the on-call Incident Commander under the incident-command system the manager owns.
- **Write the engineering-side incident narrative** — the facts, timeline, mitigations, and impact at the altitude counsel's filing or a postmortem reads off (mod-107 chapters 05, 06, and 07).
- **Preserve AI-shaped evidence** for the retention window the regulatory posture or the forensic investigation requires: traces, prompts, model outputs, retrieval payloads, tool-call arguments, judge scores, moderation-classifier decisions, and the prompt / model versions in effect at the time. The preservation window is longer than default log retention; the manager owns that the preservation happens.<!-- needs-research: specific retention windows vary by jurisdiction and regulatory posture; confirm against counsel's retention schedule before quoting a number -->

What SecOps / DFIR **owns**:

- **Attacker attribution** — who did this, by what method, from where. Admissible only if produced under chain-of-custody discipline the engineering team does not natively run.
- **Log-collection chain-of-custody** — the forensic-grade preservation that proves nothing was tampered with. AI-side evidence routinely becomes a DFIR input only when DFIR has established chain-of-custody over it.
- **Pivot analysis across the broader environment** — identity logs, endpoint telemetry, network logs, cloud-audit logs — to reconstruct the full path, which engineering has no visibility into.
- **Coordinated disclosure and law-enforcement engagement** — under counsel's direction, the decision to notify law enforcement, the disclosure sequencing across customers / partners / regulators, coordination with the vendor's own security team when the incident touches a vendor product.

**The signal that the incident has flipped** — and the response ownership with it — is evidence of adversarial intent. Specifically: pivoting (the attacker is moving through the environment); credential compromise (service accounts or user credentials appear to be in use outside their expected posture); data exfiltration (volume, pattern, or destination suggests theft, not use); persistence or reconnaissance. Any one flips the lead to SecOps. The manager's job is to recognise the flip early and hand off — not to run the attacker-attribution work solo, and not to continue the engineering response in a way that contaminates evidence SecOps now needs.

Concretely, when the flip is suspected: stop the engineering-side rollback or mitigation *if* it would destroy evidence SecOps needs, *unless* continued harm requires immediate action — a joint call between the Incident Commander and SecOps, not a unilateral engineering action; open the SecOps bridge; brief them on the AI-side evidence that has been preserved and the mitigation steps already taken; shift the manager's role from incident-owner to subject-matter partner. A cross-reference: mod-107 chapter 07 names the regulator-facing incident narrative as a tri-authorship — counsel, engineering, and SecOps. Counsel writes the legal argument; engineering writes the facts, timeline, and mitigations on the AI side; SecOps writes the attacker-side facts, the attribution finding if any, and the pivot analysis. This chapter is specific that SecOps's half is SecOps's alone — the manager composes the three pieces; SecOps owns the content of their third.

## Core concept 6 — the common drift pattern and how to resist it

Across all four deferrals, the drift is almost always driven by the same legitimate pressure: *we need this now, the other partner is slow, I know enough to do this.* The partner is understaffed or on a slower cadence; the manager is competent on the adjacent material; the team's roadmap is at risk. The temptation is to act. The resistance pattern is a short discipline.

- **Name the partner by role in the Slack thread where the drift would start.** When the question that would invite the drift is asked in channel, the first move is to say out loud which partner owns the answer. "Counsel owns the regulatory-scope read; I'm opening this with them." "Procurement owns the commercial conversation; I'm forwarding to them." "IT identity owns the SSO integration; I'll open the ticket and share the scoping." "This flip looks like a SecOps call; I'm opening the bridge." Naming the role sets the expectation for everyone reading and makes the manager's subsequent "I'm not the person to opine on that" consistent, not evasive.
- **Ask the partner what the fastest-possible path through their process looks like.** The partner's queue is often slower *on the default path*. Partners almost always have an expedited path for the right shape of ask — counsel has triage mechanics, Procurement has emergency-purchase workflows, IT identity has fast-track paths for certain vendor categories, SecOps has rapid-engagement protocols. The manager's job is to ask the question *and* shape the ask to fit the fastest-possible path — not to route around the process.
- **If the partner's process is genuinely too slow for a specific urgent case, escalate — do not bypass.** The escalation is partner-to-partner: the manager raises the queue-depth problem to the partner's manager (or the manager's own skip-level), with the specific business case for why the usual timing does not fit. Escalation preserves the partnership; bypass destroys it.
- **Keep a short list of which drifts the manager has resisted this quarter.** A small private record of the Slack threads where the manager did not opine, the vendor conversations routed to Procurement, the SSO temptations routed to IT identity, the incident-flip calls made early. It is the manager's own evidence of partnership discipline.

The underlying mindset: *the deferral is not a competence statement; it is an authority statement.* The manager is competent enough to recognise the question; the authority to answer lives with the partner. Keeping the two distinct is the discipline that keeps the partnership honest over time.

## Core concept 7 — what the manager DOES do inside each partner's surface

Deferral is not the same as absence. Each of the four partners expects the manager to show up inside their surface with specific contributions.

- **With counsel.** Supply **facts** about the feature's engineering behaviour — what the feature does, what data it processes, where the data goes, how long it is retained, what the vendor contract permits, the model's capabilities and limitations. Read DPAs for **engineering redlines** the way mod-105 chapter 05 and mod-110 chapter 05 teach — training-on-data, data-residency, sub-processors, deletion rights, deprecation notice, SLA, indemnity. Review legal language **for factual accuracy** before it ships — counsel owns the law, but the manager owns that "the feature processes X categories of data" is accurate. A counsel-drafted filing with a factual error embedded is engineering's failure.
- **With procurement.** Supply the **ask list** — what the team needs the vendor to agree to, prioritised, with the business case for each. **Maintain the renewal calendar** — when the current contracts expire, when renewal negotiations should start (months, not weeks, before expiry), which vendors are approaching renewal and what asks each should carry. **Defend asks when Procurement challenges** — Procurement will push back on asks that look expensive or unusual, and the manager's job is to say why the ask matters, not to collapse under the first challenge.
- **With IT identity.** Supply the **auth requirements** for each new AI feature or vendor adoption — the auth shape, attribute mapping, lifecycle requirements, service-account scope. **Co-review the integration design** when IT identity stands up the integration — read the SAML or OIDC configuration, confirm the attribute mapping matches what the feature needs, raise concerns about session policy. **Test the identity integration during release gating** — the release-approval workflow chapter 02 drew should include a step that confirms the production auth flow matches the designed auth flow.
- **With SecOps / DFIR.** Supply the **AI-side telemetry** at the depth DFIR needs when an investigation begins — trace schema, prompt-history retention, output-logging posture, retrieval-payload capture, tool-call audit. **Co-author the investigation runbook for AI-shaped attacker patterns**: prompt-injection as reconnaissance; agent-authority abuse (the agent is induced to call tools on behalf of an attacker); model-exfiltration via output (the attacker uses the output channel to leak data the retrieval layer should have filtered); poisoning of the retrieval store or the fine-tuning pipeline. **Participate in tabletops** — the SOC's incident-response exercises, when they cover AI-shaped scenarios, are where the manager's partnership with SecOps compounds.

Deferral held well looks like active partnership, not absence. The manager who holds the four deferrals cleanly is also the manager whom counsel, procurement, IT identity, and SecOps experience as a competent, engaged partner.

## What good looks like

A team has a healthy deferral discipline when:

- Counsel has never had to walk back a legal position the manager posted; the Slack-channel audience knows legal-adjacent questions are routed to counsel, and "I'm holding this and opening it with counsel" is a routine, not a rare, response.
- Procurement is not surprised by any vendor commercial conversation; vendor AEs know, after at most one attempt, that commercial asks route through Procurement and the manager will acknowledge-and-redirect rather than negotiate.
- Every vendor tool with auth implications in the last twelve months was onboarded through IT identity; the integration catalogue is current; no vendor account escaped the offboarding automation at the last audit.
- The last incident where the shape flipped to security was recognised by the engineering Incident Commander before SecOps had to reach in; SecOps received preserved AI-side evidence under a chain-of-custody they could accept.
- The manager keeps a running list of drift-resistance moments this quarter — threads not opined in, vendors routed, SSO temptations deferred, incident-flips surfaced — and can describe three of them from memory.
- Counsel, Procurement, IT identity, and SecOps each describe the AI Engineering Manager as a competent, present partner — not a manager who is absent from their surface and not a manager who freelances on it.

## What bad looks like

- The manager has posted at least one legal-shaped position in the last quarter that counsel later had to walk back or qualify; the Slack audience has absorbed the unretracted claim regardless.
- The vendor's AE has the manager's direct line for "special offers"; Procurement discovers at least one shadow commitment at the next renewal cycle.
- Vendor SSO integrations have been provisioned through the vendor's admin console because IT identity's queue was too slow; at least one has broken silently at the next auth rotation, or an ex-employee has retained access past offboarding.
- An AI incident in the last twelve months was treated as a misbehaviour when its shape was a security incident; the SOC learned of it after engineering had destroyed evidence through routine rollback.
- The manager drafts legal language, commercial terms, identity configurations, or SOC-side attribution narratives in artefacts the partners later rewrite; the "I did this to be helpful" framing recurs.
- At least one of the four partners has raised, directly or indirectly, that the AI Engineering Manager either overreaches into their scope or is absent from the parts of their scope the manager should contribute to.

## Summary

- The AI Engineering Manager (L45) has four specific places where the drift into another partner's scope is tempting — because the adjacent material feels tractable, because the partner's queue is slower than the roadmap, because nobody in the room is stopping the drift. Each drift has a predictable harm.
- The four deferrals: legal opinion to counsel; contract execution to procurement / finance; SSO / IAM implementation to IT identity; SOC-side investigation to security operations / DFIR. Each has been named elsewhere in the track; this chapter consolidates them on the partnership-angle axis.
- The discipline is *read, flag, escalate; do not opine solo; do not bypass the slow process — escalate inside it.* Resistance to drift is a routine, not an event: naming the partner by role in the thread, asking for the fastest-possible path, escalating when the default path cannot fit, keeping a private record of the drifts resisted.
- Deferral is not absence. Each partner expects specific contributions from the manager — facts for counsel, asks and renewal-calendar discipline for procurement, auth requirements for IT identity, AI-side telemetry and tabletop participation for SecOps. Held cleanly, the deferrals compose into a visible, durable partnership surface.
- This chapter closes mod-111. Across the module the manager built five partnership surfaces — release-approval, AI Governance consumption, product collaboration under non-determinism, AI Infra as customer-partner, AI Workforce siblings — and now holds four deferrals that keep the surfaces honest. The composite is what the AI Engineering Manager (L45) runs: the connective tissue across every function the team must land with, with scope boundaries that neither overreach nor disclaim.
