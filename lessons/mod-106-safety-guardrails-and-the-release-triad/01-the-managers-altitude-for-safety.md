# The Manager's Altitude for Safety

Safety is not a late-stage polish, a legal review, or a trust-and-safety team's deliverable the AI engineering team hands work over to. In a production LLM product, safety is a property of the thing the team builds: the input filters, the output validators, the tool-invocation policy, the human-in-the-loop checkpoints, the refusal policy, the red-team posture, the release gate that reads all of the above, the incident playbook the on-call rotation uses when something goes sideways. The manager's job in this module is to own the system that commissions those things, reviews them, and gates releases on them — not to write the classifier call or author the red-team prompts, but to ensure both exist and are honest.

If mod-104 taught the eval-gated SDLC, this module teaches the **safety-gated SDLC** that sits next to it. The eval score tells you *does the feature work well enough?*; the safety case tells you *does the feature misbehave safely enough?*; the data posture tells you *are we allowed to ship this with this data?*. Taken together, these three are the model + data + safety release triad (chapter 05). This module's job is to make all three operate as a manager-owned gate, not a hope.

## Motivation

Three failure shapes recur when safety has no manager-owned surface. All three are governance failures, not technology failures, and no amount of better tooling fixes them on its own.

- **Safety-by-launch-day panic.** The feature is built, the eval suite says it clears the gate, the launch is on Friday — and the day before launch someone asks "has anyone tried to jailbreak it?" A round-the-clock red-team sprint ensues, uncovers four problems nobody has a fix for, and the launch slips two weeks or ships anyway with a known exposure logged somewhere no one will read. The organisation treated safety as a milestone, not a discipline.
- **The guardrail that was never specified, so it does whatever the vendor's default does.** The team wired an input moderator and an output classifier, picked thresholds from a blog post, and never wrote down what the thresholds mean, which categories trigger which outcome, how refusals are phrased, or what happens when the classifier itself is down. Six months in, the product is refusing legitimate medical questions and happily answering a thinly-disguised instruction to exfiltrate a prompt, and nobody can say whether that is working-as-specified or a bug — because there is no spec.
- **The release that shipped because the eval score passed, and nobody ran the safety leg.** The eval gate from mod-104 fired green. The team deployed. Three weeks later a journalist publishes a thread of screenshots where the agent, given a benign email, has booked a flight on a corporate card and emailed the receipt to an outside address. The data-exfiltration path was known in theory. It was not tested because no one commissioned the test, and the release-gate policy did not require the safety leg before deploy.

None of the three is "we didn't have the right tool." All three are the absence of an operating cadence around safety, owned by the manager, that *required* the artefacts and *gated on them*.

## Core concept 1 — what the manager owns, versus what the IC owns

The split from mod-104 and mod-105 repeats here: craft is the IC's, system is the manager's. Four rows of the table are worth being specific about, because this is where EMs most often either over-reach into IC craft (writing jailbreak prompts, tuning classifier thresholds) or under-reach into leaving the entire safety posture to the ICs (and then being surprised when something lands wrong).

| What the IC owns (craft) | What the manager owns (system) |
|---|---|
| Writing and tuning input filters, output validators, classifier integrations, HITL UX, refusal messages, structured-output validators. The `applied-ai-engineer` (level 30) builds these; the `ai-safety-engineer` or `ai-risk-engineer` (level 25, where the role exists) specialises in them. | Writing the **guardrails product spec** — categorical list of in-scope harms, required inputs/outputs for each guardrail, error-on-classifier-failure behaviour, refusal policy, escalation-to-HITL contract — so the ICs can build against an unambiguous target. Chapter 02 is this. |
| Authoring and executing red-team attack suites; writing jailbreak prompts; running capability evals; writing the findings report. This is the craft of the `ai-risk-engineer` (level 25, peer) and the `agentic-safety-engineer` (level 40, peer on agentic surfaces). | Writing the **red-team commissioning brief** that scopes the engagement: product surface, risk model, categories of concern, success criteria, who the findings route to, how long the engagement is funded for. Chapter 04. |
| Choosing between Llama Guard, NeMo Guardrails, OpenAI Moderation, Azure Content Safety for a specific surface. Benchmarking classifier performance on the team's golden eval. | Requiring **that** a content-classifier exists on every surface the risk model says needs one, that its choice is justified in the guardrails spec, and that the three-tier fallback (primary, secondary, hard-fail) is defined. Chapter 03 walks the landscape at EM depth. |
| Implementing OAuth token scopes, service identities, MCP server authorisation, tool-call allow-lists, HITL confirmation flows. This is `agent-platform-engineer` (level 40) and `agentic-ai-engineer` (level 30) work. | Shaping the **agent authorization story** at the product boundary: which tools exist, which scopes they hold, where the HITL checkpoints live, what the blast-radius containment looks like, when the manager personally signs on a new tool. Chapter 07. |

Memorise the pattern: *manager sets non-negotiable shape; IC fills in craft; manager reviews the shape every quarter and tightens it when evidence says it is loose.* Managers who write classifier calls are slowing the team; managers who leave the shape of the guardrail stack to whoever happens to be building the feature are importing risk they will later own.

## Core concept 2 — the four manager-owned artefacts

Every chapter in this module builds one of four manager-owned artefacts. Memorise the list; the chapters and exercises are organised around them.

1. **The guardrails product spec (chapters 02–03).** A written specification, per feature or per surface family, of the guardrails that must exist: input moderation, output validation, tool-invocation policy, HITL checkpoints, refusal policy, classifier choice, error-on-classifier-failure behaviour, logging contract. The spec is unambiguous enough that an applied-ai-engineer IC can build against it; it is owned by the manager because no IC role has standing to say "this is the system's behaviour on refusals."
2. **The red-team commissioning brief and findings-review posture (chapter 04).** The brief the manager writes when funding a red-team engagement — product surface, risk model, categories of concern (from OWASP LLM Top 10, NIST GenAI Profile, and the product-specific harms), the time budget, the escalation path, the fix-triage ritual. Plus the findings-review ritual: how a finding becomes a fix, a mitigation, an accepted-risk entry, or a shipping-block.
3. **The release triad — model, data, safety (chapter 05).** The go/no-go gate the manager personally signs at each release of consequence. Model leg: does the eval evidence support ship (mod-104)? Data leg: are the inputs, outputs, and retained traces inside the posture legal and product have agreed to (mod-105 chapter 05, mod-111)? Safety leg: have the red-team findings been addressed or explicitly accepted, and is the incident playbook (mod-107) current? None of the three legs is optional; the manager's signature on all three is the shape of the gate.
4. **The agent authorization boundary (chapter 07).** The written policy, per surface where the LLM calls tools, of which tools are available, which identity scopes each tool holds, which actions require a human-in-the-loop checkpoint, and what the blast-radius containment is if a tool call misfires. Owned by the manager because the craft of implementing tool authorisation defers to `agent-platform-engineer` — but the *decision* of what a tool is allowed to do on the product's behalf is a product decision the manager signs.

Plus two cross-cutting reading disciplines the chapters interleave: the public-provider governance frames (chapter 06 — Anthropic RSP, OpenAI Preparedness, DeepMind FSF), and the EU AI Act Articles 14 / 15 / 50 translations into engineering deliverables (chapter 08).

A team has a **manager-owned safety posture** when all four artefacts exist in writing, the release-triad gate from chapter 05 is required on every launch of consequence, the red-team commissioning cadence is funded, and the manager has read the applicable provider's governance framework for the model tier being depended on.

## Core concept 3 — who defers to whom, and why

This module is deliberately a *leading-and-commissioning* track, not a *building-the-safety-stack* track. Four deferrals, each worth being explicit about, because this is where the role most often either reaches too far or stops too soon.

### Red-team craft defers to the AI Risk / Safety ladder

The craft of authoring and executing red-team attack suites — writing jailbreak prompts, chaining prompt-injection vectors, probing for data exfiltration, running capability evaluations at a sophistication that catches real adversaries — belongs to the `ai-risk-engineer` (level 25, peer-to-manager relationship at this role level) and, on agentic surfaces, to the `agentic-safety-engineer` (level 40). The AI Engineering Manager **commissions** the engagement, **reviews** the findings, and **routes** the fixes. The manager who writes jailbreak prompts personally will usually produce less evidence than an IC with a week's dedicated focus would — and will own both the quality of the red-team and the quality of the engineering, which is one hat too many.

Where an in-house red-team role does not exist (common in smaller orgs), the commissioning posture is to an external vendor (a trust-and-safety firm, a specialist AI red-team consultancy) or to a bounty / internal rotation. Chapter 04 walks all three shapes.

### Classifier internals defer to Applied AI / ML Engineering

The craft of choosing between Llama Guard-N and OpenAI Moderation for a given surface, benchmarking a safety classifier against a team golden set, picking a threshold on the ROC, deciding when to fine-tune a smaller guard model — this belongs to the `applied-ai-engineer` (level 30) or, where the ML specialisation exists, an `ai-safety-engineer` (level 25) or `ml-engineer` (level 30). The manager reads chapter 03 to have the vocabulary and the pick-rules. The manager does not tune thresholds.

### Agent authorisation implementation defers to Platform

The craft of implementing scoped service identities, OAuth flows, MCP server authorisation, tool-call allow-lists, and blast-radius containment on a shared agent platform belongs to the `agent-platform-engineer` (level 40) and to the `agentic-ai-engineer` (level 30, feature-level work). The manager owns the *decision* of which tools get which scopes, which actions escalate to HITL, and what blast-radius containment the product requires — not the implementation. Chapter 07 is very specific about this split.

### Legal binding defers to counsel; audit defers to Risk / Security

The manager reads the EU AI Act Articles 14, 15, and 50 the way they read a DPA in mod-105: enough to translate the obligations into engineering deliverables the team can ship, enough to flag when a product decision crosses a legal line, enough to route to counsel at the right moment. Writing the legal opinion on whether a given deployment is a "high-risk AI system" under Annex III belongs to counsel. The manager ships the human-oversight mechanism (Article 14), the robustness-and-cybersecurity measures (Article 15), and the transparency disclosures (Article 50) that the legal opinion presupposes. Chapter 08 walks this split.

A note on terminology: "AI safety" is used in this module in the **applied, product-safety** sense — guardrails, content classifiers, red-teaming against product misuse, agentic tool-call containment. This is the sense OWASP, NIST, the EU AI Act, and the industry's trust-and-safety practice use. The distinct research tradition of "AI alignment" / "frontier safety" (scalable oversight, interpretability, deceptive-alignment research) is upstream of the provider governance frames in chapter 06; an EM does not operate in it directly, but reads the public frames to understand what capability tiers depend on which practices.

## What this module does not teach

Three deliberate exclusions, each a referral.

- **How to write a jailbreak.** The craft of crafting attack prompts, chaining prompt injection through retrieval and tool use, probing multi-step agent exploits, and understanding the taxonomy at the depth needed to author a novel attack belongs to the `ai-risk-engineer` and `agentic-safety-engineer` curricula. This module teaches you to commission the work and read the findings.
- **How to train or fine-tune a safety classifier.** Fine-tuning Llama Guard on your team's taxonomy, training a custom moderation head, calibrating a reward model for harmful content — all of these belong to the ML Engineering / Applied AI ladders. The manager picks *that* a classifier exists; the IC picks *which*.
- **How to litigate an EU AI Act compliance obligation.** The manager translates Articles 14 / 15 / 50 into engineering deliverables; counsel writes the legal argument. If a product decision depends on whether a feature falls under Annex III, that is a question for counsel, not for this module.

What the manager must know at depth: the **vocabulary** of safety (prompt injection, indirect prompt injection, jailbreak, refusal policy, HITL, blast-radius containment, structured output validation, canary token, jailbreak arena, data exfiltration via tool-use), the **mapping** from recognised frames (OWASP LLM Top 10, NIST GenAI Profile, EU AI Act Articles 14 / 15 / 50) to concrete engineering deliverables the team ships, the **public provider frames** (Anthropic Responsible Scaling Policy, OpenAI Preparedness Framework, DeepMind Frontier Safety Framework) as inputs to deployment-tier decisions, and the **mechanics** of the release-triad gate. Chapters 02–08 walk these in order.

## The shape of the chapters ahead

- **Chapter 02 — Guardrails as product spec.** OWASP Top 10 for LLM Applications as the floor; NIST GenAI Profile as the organising frame; the six-section spec (input filters, output validators, tool-invocation policy, HITL checkpoints, refusal policy, classifier choice + error behaviour). What an applied-ai-engineer IC needs on the page to build.
- **Chapter 03 — The guardrails tech landscape at EM depth.** NeMo Guardrails, Llama Guard, OpenAI Moderation, Azure AI Content Safety, Google Vertex Safety, Perspective API. What each is good for, where the manager-depth decision rules land. Not a benchmark; a map.
- **Chapter 04 — Commissioning red-team work.** The brief. The three engagement shapes (in-house `ai-risk-engineer`, external vendor, internal rotation / bug-bounty). The findings-review ritual (fix / mitigate / accept / block). The cadence that keeps it from being a launch-week scramble.
- **Chapter 05 — The model + data + safety release triad.** The go/no-go gate the manager personally signs. Three legs, each with a specific evidence requirement. The override posture that keeps the gate honest. The handshake into mod-104 (eval evidence), mod-105 (data posture), mod-107 (incident playbook).
- **Chapter 06 — Reading RSP, Preparedness, and FSF.** Anthropic Responsible Scaling Policy, OpenAI Preparedness Framework, DeepMind Frontier Safety Framework — what the EM reads them for when depending on a provider's model at a new capability tier. The capability-threshold vocabulary. The deployment-tier decisions these frames influence.
- **Chapter 07 — Agent / tool-use authorisation at the product boundary.** OAuth and scoped service identity, MCP contracts, HITL checkpoints, blast-radius containment. The boundary-review ritual on every new tool. Where the manager personally signs and where the craft defers to `agent-platform-engineer`.
- **Chapter 08 — EU AI Act Articles 14, 15, 50 as engineering deliverables.** Human oversight (14), accuracy / robustness / cybersecurity (15), transparency (50). Translation into concrete artefacts — audit logs, intervention points, watermarking / disclosure, incident-reporting infrastructure — the team ships without replacing legal review.

## What good looks like

A team has a manager-owned safety posture when:

- A new engineer can open the guardrails spec on their first day and know exactly which input moderator fires on which categories, what the classifier-down fallback is, which outputs route to a human, and how a refusal is phrased.
- A red-team engagement has been commissioned, funded, and completed at least once per major feature; the findings are tracked to closure in the same tracker as any other bug; a dated "accepted risk" entry exists for each finding not fixed.
- The release-gate policy requires the signed triad (eval evidence + data posture + safety case) before any release of consequence; overrides are two-signature events and are logged.
- The incident playbook (mod-107) has been dry-run; the on-call rotation knows which runbook to open for a guardrail outage, a classifier false-negative stream, a prompt-injection incident, and a data-exfiltration attempt.
- The applicable provider's governance framework has been read by the manager; the deployment-tier decision for the current model is documented against the provider's capability tiers.
- Each product surface that calls tools has a one-page authorisation review on file: tools, scopes, HITL checkpoints, blast-radius containment. Any new tool requires an update before the change ships.
- EU AI Act Articles 14 / 15 / 50 have been translated into concrete artefacts the team owns; counsel has reviewed the mapping; the mapping is revisited each time the feature set changes materially.

## What bad looks like

- Guardrails are the vendor's default; nobody has written down what counts as unsafe for this product.
- "Red-team" is the word for the two-hour Friday-afternoon jailbreak party; no commissioning brief; no findings tracker; no funded cadence.
- The release gate reads only the eval score. Data posture and safety case are assumed because "legal reviewed it" or "safety looked at it" — unsigned, undated, unknown.
- The LLM provider's model family has moved up a capability tier (per the provider's own RSP / Preparedness / FSF) and nobody on the team has read the system card for the new tier.
- An agent calls tools in production and no one can produce a page listing the tools, their scopes, and the HITL checkpoints — because no such page exists.
- EU AI Act compliance is a line item on counsel's board that the engineering team has never seen; the first time engineering hears about Article 14 is at the regulator's request.

## Summary

- Safety is a product property the team builds, a discipline the manager owns, and a release gate the manager signs — not a department someone else runs.
- The manager owns four artefacts: the guardrails product spec, the red-team commissioning brief and findings-review ritual, the release triad go/no-go gate, and the agent authorisation boundary.
- Red-team craft defers to the AI Risk ladder; classifier internals defer to Applied AI; agent authorisation implementation defers to Platform; legal binding defers to counsel. In each case the manager commissions, reads, and gates — not implements.
- The rest of the module walks the four artefacts in turn, with the public-provider governance frames (chapter 06) and the EU AI Act translation (chapter 08) as cross-cutting reading disciplines.

The next chapter picks up the first artefact: the guardrails product spec that converts OWASP's harm taxonomy, the NIST GenAI Profile's organising frame, and the team's own risk model into a specification an applied-ai-engineer can build against.
