# Guardrails as Product Spec

A guardrail is a product behaviour. "Do not answer questions about self-harm without routing to a crisis-line handler." "Do not quote more than fifty consecutive tokens from the source documents." "Do not call the `send_email` tool without an explicit user confirmation." These are specifications — they belong in the same place feature requirements belong, written at a level of precision an engineer can build against and a reviewer can verify. If the guardrail lives only in an engineer's head, or in a classifier threshold someone set once, the team does not have a guardrail; it has a reflex.

This chapter is about writing the specification. Chapter 03 walks the tooling you build it with. Separating the two is deliberate: picking Llama Guard before you know what you want it to catch is the single most common guardrail failure mode on an AI team, and reversing that order is the first thing a manager puts right.

## Motivation

Three failure shapes recur when guardrails are not written as product specs.

- **The vendor-default guardrail.** The team wired in the provider's content moderation on the way to launch, picked the thresholds from the SDK documentation, and never wrote down what categories are in scope, what the error-on-classifier-failure behaviour is, how refusals are worded, or when a HITL review fires. Three months later, the product refuses a medical question that any clinician would answer, happily answers a thinly-disguised copyright exfiltration, and the team cannot say whether either is a bug — because the spec does not exist.
- **The guardrail-by-blog-post.** An engineer saw a jailbreak thread, adds a prompt-injection filter, and ships it on a Tuesday. There is no changelog, no eval, no spec entry, no review. The filter catches one real attack and silently breaks four legitimate workflows that happen to contain the words it flags. The team discovers both facts a week apart.
- **The missing refusal policy.** The guardrail fires; the model refuses. The refusal is a one-liner cobbled together from the prompt. Different surfaces refuse differently. Some refusals leak which classifier fired; some are so generic users retry three times before giving up; some apologise and promise to "do better next time" in a product context where that is both wrong and legally hazardous. The organisation has no refusal policy because no one has written one.

All three are fixed by the same thing: a written specification per feature, owned by the manager, that the applied-ai-engineer builds against and the risk-engineer verifies.

## Core concept 1 — the two frames the spec maps against

The guardrails spec should cite two external frames, because both will be cited at you by auditors, customers, and reviewers; you may as well cite them first.

### OWASP Top 10 for LLM Applications

OWASP publishes a Top 10 risk list specifically for LLM applications, versioned (the current series is maintained by the OWASP GenAI Security Project). The exact IDs and ordering move across versions; what stays stable is the taxonomy of the harms an LLM application faces. At commissioning-manager depth, the categories the spec should reference by name include at least:

- **Prompt injection** (direct and indirect) — untrusted content, whether from the user or retrieved from a tool or document, that attempts to override the system's instructions.
- **Insecure output handling** — model output passed to a downstream interpreter (shell, HTML renderer, SQL, code runner, tool-call dispatcher) without appropriate validation and escaping.
- **Training-data poisoning** — poisoned or manipulated training or fine-tuning data that embeds unsafe behaviour.
- **Model denial of service and resource exhaustion** — adversarial inputs that drive disproportionate compute or cost.
- **Supply-chain vulnerabilities** — third-party models, embeddings, or plugins that import vulnerabilities into the product.
- **Sensitive information disclosure** — model outputs that reveal secrets, PII, or confidential information the model should not surface.
- **Insecure plugin / tool design** — tools the agent can call that lack authorisation, input validation, or scoping.
- **Excessive agency** — agents authorised beyond the minimum needed, or acting without the human-in-the-loop checkpoint the risk model requires.
- **Overreliance** — product UX that encourages users to accept model output uncritically in domains where critical review is needed.
- **Model theft and extraction** — adversarial behaviour that attempts to replicate the model or its outputs at scale.

Pin the current version of the OWASP document. Map each in-scope category to either (a) one or more guardrails in the spec that address it, (b) a cross-reference to another artefact that addresses it (an SSDLC control, an infra posture, a legal control), or (c) an explicit "out of scope for this product because X" line. **A category with no mapping is a hole in the spec.**

### NIST Generative AI Profile

NIST publishes the AI Risk Management Framework's *Generative AI Profile* (AI 600-1), which organises risks and recommended actions for generative-AI systems around the four core functions of the AI RMF: *Govern*, *Map*, *Measure*, *Manage*. At manager depth, the Profile is used in two places in the spec:

- As the **organising frame** for the risk model section of the guardrails spec: *Govern* (who signs, who reviews, who escalates), *Map* (which harm categories apply to this product), *Measure* (which signals detect which harms), *Manage* (what the response is when a harm is detected or an incident occurs).
- As a **cross-reference** for the categories of risk the Profile enumerates — misinformation, confabulation, obscene content, CBRN uplift, dangerous or violent content, data privacy, intellectual property, cybersecurity, information integrity. The team's risk model names which apply to this product.

Treat NIST as the frame your legal / risk / security partners already read; treat OWASP as the frame your engineering counterparts recognise. Writing the spec against both makes the hand-off to each side much shorter.

A third frame worth being aware of without duplicating in every spec: MITRE's ATLAS (Adversarial Threat Landscape for Artificial-Intelligence Systems) catalogues adversarial tactics specifically against AI systems and is useful when authoring the red-team brief in chapter 04.

## Core concept 2 — the six sections of a guardrails spec

The spec is between three and six pages per feature, written in markdown, filed in the same repository as the feature's eval suite and prompt registry (mod-104 chapters 02, 04). Six sections, in order.

### Section 1 — Product surface and risk model

One paragraph describing the feature's surface — what it does, who uses it, what the trust posture is (public web, authenticated customer, employee-only), the deployment surface (API, chat, embedded agent, offline batch), and the data classes it handles.

A table of in-scope harms, mapped against the OWASP and NIST frames. Each row: the harm, the frame identifier (e.g. "OWASP LLM01 — Prompt Injection"), one-sentence product-specific instantiation (e.g. "An attacker embeds instructions in a retrieved customer email, causing the agent to email a competitor the customer's pricing"), and the one-liner mitigation strategy. Rows where the mitigation is "out of scope — mitigated at infra layer / mitigated by contract" are allowed but explicit.

A short paragraph on the audience and the *severity* of a worst-case incident: a wrong answer to a mobile-app trivia question is not a wrong answer to a clinical-decision support question; the spec should name the severity tier explicitly so every downstream choice (classifier threshold, HITL cadence, refusal strictness) can be calibrated against it.

### Section 2 — Input filters

What runs on user input (and on untrusted retrieved content, which is a second class of input) before it reaches the model.

- **Categories moderated.** Which categories the input moderator flags. Reference OWASP's taxonomy, NIST's category list, or the classifier's own category list — but name them. "Toxicity" alone is not a category; "hate speech, harassment, sexual content, self-harm, violence" (or the specific labels the classifier emits) is.
- **Classifier choice and thresholds.** Which classifier — defer specifics to chapter 03 for the landscape; the spec just names the chosen one and its version. Thresholds per category, with a one-line rationale per threshold.
- **Prompt-injection detection posture.** How the feature treats untrusted content. The three common postures: (a) separate it structurally from system instructions (recommended on any feature where retrieval or tool input is non-trivial), (b) sanitise or strip known injection markers, (c) quarantine and route to a dedicated sub-prompt that is given reduced tool scope. Name which posture applies.
- **Behaviour on classifier failure or timeout.** Explicit. Either *fail-closed* (block the request, show a maintenance message) or *fail-open-with-reduced-capability* (pass to a safer, more restricted path). Never "whatever the SDK does."
- **Logging.** What is logged about the filter's decision (category, score, decision, request id). What is not (raw content, if the product's privacy posture requires it). The retention window.

### Section 3 — Output validators

What runs on model output before it is displayed, returned over the API, or dispatched to a tool.

- **Structured-output validation.** For any output consumed programmatically, a schema — JSON Schema, Pydantic, Zod — against which output is validated. What happens on validation failure: retry with a schema-repair prompt, fail-closed, or degrade to a known-safe fallback.
- **Content-safety classifier on the output.** Same shape as section 2 — classifier, thresholds, behaviour on classifier failure.
- **PII / secrets redaction.** For products whose outputs may surface PII or credentials (RAG over corporate data, agent outputs from private-channel tool calls), the redaction pass applied before display. Which patterns / classifier are used; how the redaction behaviour is logged.
- **Citation / grounding checks.** For retrieval-grounded features, the grounded-faithfulness check (does the answer cite retrieved content; does it stay within the cited content) and what happens when it fails: regenerate with stricter grounding, refuse with a "I couldn't find this in the sources" message, or route to HITL.
- **Toxicity / factuality thresholds specific to this surface.** If this feature has a lower factuality tolerance than the default (clinical, legal, financial-advice adjacent), the elevated threshold and the test that demonstrates the model meets it.

### Section 4 — Tool-invocation policy

For any feature that calls tools, the policy. This section pairs with chapter 07's deeper authorisation review; here the spec names the policy at product level.

- **Tool catalogue for this feature.** The subset of tools this feature's agent can call. Features should not inherit the full tool catalogue by default.
- **Pre-invocation checks.** For each tool, the input validation the system applies before the call is dispatched: schema, authorisation scope, rate limit, cost budget.
- **HITL requirements.** Which tools require an explicit user confirmation before invocation (any action with external side-effects — sending email, scheduling a meeting, moving money, deleting a resource, posting publicly — and any action over a per-feature cost or blast-radius threshold).
- **Post-invocation treatment of tool output.** Tool output is untrusted input to the next LLM turn. State the output-sanitisation pass applied; state whether the tool output is treated as structured or as free-text; state the rate at which tool output is re-fed to the moderator from section 2.
- **Loop limits and cost caps.** Maximum steps per session; maximum concurrent tool calls; per-session cost cap. What the behaviour is when a limit is hit.

### Section 5 — Human-in-the-loop checkpoints

Where humans sit in the loop and what they are empowered to do.

- **Named checkpoints.** For each step where human review is required, the trigger (classifier score ≥ X, tool category = Y, user is in audience Z), the UX (inline confirmation, dedicated review queue, out-of-band notification), and the SLA (time budget until the review must complete or the request is cancelled / escalated).
- **Reviewer qualification.** Who reviews. For a consumer-chat moderation queue, the reviewer is a trust-and-safety agent against a published rubric; for a clinician-adjacent feature, the reviewer must be a credentialed clinician; for a code-execution agent, a security-cleared reviewer. The spec names the role, not the person.
- **Reviewer tooling.** What the reviewer sees. The full prompt context, the retrieved content, the classifier scores, the proposed action. What is redacted from reviewer view for privacy and what is not.
- **Reviewer-override logging.** Every HITL action produces a durable, auditable log entry: reviewer id, decision, rationale, timestamp, request id. This is what satisfies EU AI Act Article 14 (chapter 08) later.

### Section 6 — Refusal policy

The behaviour the product takes when a guardrail fires.

- **Taxonomy of refusals.** The spec names a short, closed list of refusal types: *content-safety refusal* (the request falls into a disallowed category), *grounding refusal* (the model cannot ground an answer in the retrieved source), *scope refusal* (the request is out of the product's declared scope), *technical-degradation refusal* (the system is operating in a degraded mode), *HITL-queued* (the request has been routed for human review and will not be answered inline). Each has a specific user-facing wording, specific logging, and a specific follow-up path.
- **Refusal wording.** The actual strings. Three principles — do not apologise for the product's policy (it is a design choice, not a mistake); do not reveal which classifier or threshold fired; do give the user an actionable next step (what to do, who to contact, whether to retry with different input). The spec includes the actual wording so engineers do not improvise.
- **Appeals and false-positives channel.** How a user reports a false-positive. For B2B features this is often a support ticket; for consumer features it is an in-product form; for employee-facing features it is a #channel in chat. The spec names the channel and the triage SLA.
- **Observability.** The rate at which refusals fire, bucketed by type and by category, is a production metric reviewed on the cadence from chapter 06 of mod-104. A refusal rate that moves silently is a product change the team did not sign up for.

## Core concept 3 — the spec review checklist

Before the spec goes to the applied-ai-engineer for implementation, the manager runs a checklist. This is the 10-minute discipline that separates a spec from a draft.

- **Every in-scope OWASP LLM Top 10 category is addressed** — either mitigated by a section 2–5 guardrail, cross-referenced to another artefact, or declared out of scope with a one-line rationale.
- **The NIST GenAI Profile's four functions — Govern / Map / Measure / Manage — each map to at least one section** of the spec, implicitly or explicitly. (Govern → section 5 ownership; Map → section 1 risk model; Measure → section 2 and 3 classifiers; Manage → section 6 refusals and the incident playbook pin.)
- **Every classifier and validator in sections 2 and 3 has an explicit error-on-failure behaviour.** No "SDK default."
- **Every tool in section 4 has an explicit HITL rule.** Either "requires confirmation" or "no confirmation needed because <reason>."
- **The HITL reviewer role is named** per checkpoint. "A human" is not a role.
- **The refusal wording is in the spec**, not promised for later.
- **The severity tier from section 1 is consistent** with the thresholds in sections 2–4. A safety-critical feature with a toxicity-score threshold of 0.9 is a spec bug; the review catches it.
- **The spec names its pre-launch verification evidence** — what the red-team engagement's scope will be (chapter 04) and what safety-eval cases will exist in the eval registry (mod-104 chapter 04).

## A worked example at skeleton depth

A worked skeleton for a consumer-facing medical Q&A assistant, abbreviated to the shape of the spec rather than the full content.

```markdown
# Guardrails Spec: Healthwise Chat
Owner: <manager name> · Reviewed: <date> · Next review: <date>

## 1. Surface and risk model
Consumer chat answering lay-language medical questions. Authenticated users. Severity tier: HIGH — advice wrongly given could cause harm.

| Harm (OWASP/NIST)          | Instantiation                                          | Mitigation summary               |
|----------------------------|--------------------------------------------------------|----------------------------------|
| OWASP LLM01 Prompt Inj.    | User asks model to "ignore safety rules"               | Structural separation + classifier |
| OWASP LLM06 Sensitive Info | Model surfaces a specific patient's records            | RAG scope restricted to public corpus |
| NIST GenAI Harmful Content | Model answers CBRN-adjacent synthesis question         | Category block + refusal          |
| NIST GenAI Confabulation   | Model invents a drug–drug interaction                  | Grounding + citation check        |
| OWASP LLM09 Overreliance   | User treats answer as clinical advice                  | Scope refusal + "consult clinician" overlay |

## 2. Input filters
- Classifier: <chosen classifier, version> — categories {self-harm, violence, sexual, hate, drugs, weapons, CBRN, medical-emergency}
- Thresholds: block if score ≥ 0.6 in {self-harm, CBRN}; HITL-queue if 0.4–0.6; route to crisis-line template on medical-emergency ≥ 0.5
- Prompt-injection posture: structural separation; sanitisation on retrieved content
- Classifier failure: FAIL-CLOSED with technical-degradation refusal
- Logging: category, score, decision, request id. No raw content retained beyond 30 days.

## 3. Output validators
- Structured output: none (free-text).
- Content classifier: same categories; threshold 0.5 blocks and regenerates once, then refuses.
- PII redaction: regex pass for names / phone / email appearing in model output (RAG misconfig insurance).
- Grounding check: answer must cite at least one source for any specific claim; absent citation → regenerate with stricter grounding prompt once, then refuse ("I couldn't find a trustworthy source for this").
- Factuality threshold on evals: faithfulness ≥ 0.90 on gold set.

## 4. Tool invocation
Tool catalogue: {search_corpus, cite_source, escalate_to_crisis_line}. No side-effect tools.
Pre-invocation: all tool inputs schema-validated.
HITL: escalate_to_crisis_line requires immediate handoff UX; no confirmation step (optimise for latency).
Post-invocation: tool output re-passed through input filter before next turn.
Loop limit: 3 steps; cost cap: $0.10 per session.

## 5. HITL
- Checkpoint A: input score 0.4–0.6 in {self-harm} → queue to crisis-trained reviewer. SLA: 90 seconds or auto-handoff to crisis-line.
- Checkpoint B: unresolved ambiguity → inline "Would you like me to connect you with a clinician?" prompt.
- Reviewer qualification: crisis-line staff per contract with <vendor>.
- Reviewer tooling: full conversation, classifier scores, user metadata (age band, geography).
- Audit log: decision, reviewer id, rationale, timestamp — retained 7 years.

## 6. Refusals
- Content refusal: "I can't help with that here. If you're in crisis, <crisis-line number / link>."
- Grounding refusal: "I couldn't find a trustworthy source for this. You might ask a clinician or check <recommended source>."
- Scope refusal: "I'm limited to general health questions. For specific medical advice, please consult a clinician."
- Technical-degradation refusal: "I'm running in a reduced mode right now. Please try again in a few minutes."
- Appeals: in-product "Report an issue" form → T&S queue, 48-hour SLA.
```

The real spec is longer in each section. The skeleton shows the *shape*: every section carries decisions, not placeholders.

## Anti-patterns to name and defeat

Three anti-patterns the review above catches.

- **"The classifier does safety."** No it does not. A classifier is one input to a guardrail; the guardrail is a decision the product makes. A spec that names the classifier but not the thresholds, the failure behaviour, the categories, and the refusal has not specified a guardrail — it has specified a dependency.
- **The threshold set by vibes.** A threshold of 0.5 because 0.5 "sounds safe" is a spec bug. Thresholds come from evaluation — the IC's craft (defer to the applied-ai-engineer / safety-engineer) runs the classifier against a golden set drawn from the team's own content, measures precision and recall, and chooses a threshold with the manager's severity tier in hand. The spec records the choice; chapter 04 of mod-104 holds the eval surface that justifies it.
- **The refusal that leaks.** A refusal saying "I can't answer because the toxicity classifier fired on your message" tells the attacker exactly which classifier to evade next time. A refusal saying "I can't help with that here" tells nothing. The spec's refusal wording is a security control, not a UX copy exercise.

## Summary

- The guardrails spec is a product document: six sections (surface and risk model, input filters, output validators, tool-invocation policy, HITL checkpoints, refusal policy), cross-referenced against OWASP Top 10 for LLM Applications and the NIST Generative AI Profile.
- Every classifier, validator, and HITL checkpoint carries an explicit error-on-failure behaviour, an explicit threshold, and an explicit refusal path. "Vendor default" and "a human will look at it" are failure modes, not specifications.
- The manager owns the spec because the ICs cannot credibly sign off on "this is the system's behaviour on refusals" — that is a product decision.
- The review checklist is a ten-minute discipline run before the spec hands to engineering: category coverage, failure behaviour, named reviewer roles, in-spec refusal wording, severity-consistency, pre-launch verification pointed at chapter 04's red-team and mod-104's eval registry.

The next chapter walks the tooling landscape — NeMo Guardrails, Llama Guard, OpenAI Moderation, Azure AI Content Safety — at the depth a commissioning manager needs to pick between them without becoming the one who tunes their thresholds.
