# Reading RSP, Preparedness, and FSF

The three leading frontier labs publish standing frameworks that describe how they evaluate their own models for dangerous capabilities and what deployment protections they attach as those capabilities rise. Anthropic publishes the **Responsible Scaling Policy (RSP)**. OpenAI publishes the **Preparedness Framework**. Google DeepMind publishes the **Frontier Safety Framework (FSF)**. The specific contents of each framework evolve; the AI Engineering Manager reads them the way they read a DPA — enough to know what the provider is committing to, enough to translate the commitments into local deployment decisions, enough to know when a provider's new capability tier means the product's local safety posture needs to change.

This chapter is about *reading* these frameworks. The reader does not implement them; the frontier lab does. The reader does learn to recognise which parts of a deployed model's capabilities are now flagged by the provider as requiring extra care, and what that implies for the local release triad (chapter 05).

## Motivation — why an EM reads lab governance

Three real patterns that go wrong when the EM does not read the provider's framework.

- **Capability tier moved and nobody noticed.** The provider releases a new generation of the model the team depends on. The provider's system card discloses — truthfully, in advance — that the new generation has crossed an internal capability threshold that triggers elevated deployment safeguards. The product team upgrades in the normal model-portfolio cadence (mod-105 chapter 07), reads the change as "faster + smarter," and does not update the local red-team scope (chapter 04) or the guardrail classifier choice (chapter 03). Four months later a researcher publishes a finding that would have been caught by a tier-appropriate red-team.
- **The provider pulled back; the product did not.** The provider decides, after post-deployment monitoring, to remove a capability from a deployed model (lower the model's ceiling on some task). The team's product counted on that capability and now fails on a long-tail of traffic. The team was never reading the provider's post-deployment-change notices and did not learn until a user reported it.
- **Reasoning about "AGI-adjacent" without the vocabulary.** An executive or a board member asks the manager about "AI safety risk" in the next quarterly plan. The manager answers in the language of OWASP LLM Top 10 — which is correct but incomplete for the question. The frontier frameworks define the vocabulary (capability threshold, CBRN uplift, autonomy level, security commitment, deployment commitment) the executive is really asking about, often without knowing those are the words. A manager who can map one vocabulary to the other owns the conversation.

The chapter's job is to give the manager the second vocabulary without requiring expertise in the first.

## Core concept 1 — what each framework is, at commissioning-manager depth

The three frameworks are not identical. They have a similar *shape*: a set of dangerous-capability categories, evaluations the lab runs against its own models before deployment and periodically after, thresholds that trigger specific deployment-time protections, and a public commitment that the lab will not deploy (or will only deploy with specific safeguards) models that cross a given threshold without the protections.

### Anthropic — Responsible Scaling Policy (RSP)

Anthropic publishes the RSP on its public site. The framework defines **AI Safety Levels (ASL)** — tiers of model capability, where each tier triggers a required bundle of **deployment protections** (external-use safeguards, usage policies, trust-and-safety tooling, monitoring) and **security protections** (controls on model weights, internal access, information-security practices). At commissioning-manager depth:

- The ASL vocabulary is a widely-adopted frame for talking about "how worried should we be about this generation of model?" — read the current public RSP to see which ASL tier the model the team depends on is operating at.
- The RSP publishes a list of the dangerous-capability evaluations Anthropic runs against its own models — biological uplift, autonomous replication and adaptation, cyber capability, and others; the list evolves. When the EM is briefing an executive on "what safety work is upstream of our product," the RSP is one of the clearest sources.
- Anthropic publishes **system cards** per major model release that describe the evaluations run, the behaviours observed, and the ASL determination.

### OpenAI — Preparedness Framework

OpenAI publishes the Preparedness Framework on its safety page. The framework defines **tracked categories of risk** (currently including biological and chemical, cybersecurity, AI self-improvement, and persuasion; this list evolves across versions) and **risk levels** per category (low / medium / high / critical) with defined evaluation protocols. The commitment: the organisation will not deploy a model at the top-tier risk level in a category without specific mitigations, and will not develop beyond that level without further controls.

At commissioning-manager depth:

- The Preparedness category list is a complementary taxonomy — a system card published alongside a new OpenAI model will typically report where each category lands post-mitigation.
- The Preparedness Framework names an internal Safety Advisory Group (the specific structure evolves) that reviews model deployment decisions. Reading the framework lets the EM understand what the provider is signing off on.

### Google DeepMind — Frontier Safety Framework (FSF)

Google DeepMind publishes the FSF. The framework defines **Critical Capability Levels (CCLs)** — thresholds across capability categories including cybersecurity, autonomy, biosecurity (biological), and machine-learning R&D autonomy; specifics evolve across versions. The framework commits to evaluating frontier models against these thresholds and applying commensurate mitigations.

At commissioning-manager depth:

- The FSF is the frame Google DeepMind uses for Gemini. The Gemini technical reports discuss evaluations in these terms.
- The FSF emphasises *evaluations at a buffer below the threshold* — a model is evaluated before it reaches the CCL, not after — which translates into the provider's cadence of pre-deployment evaluations.

### Other frames worth naming

- **Meta** publishes a *Responsible Use Guide* for Llama and model-specific system cards; the open-weights posture makes the governance story different from the closed-weights labs — some of the protection relies on the licence terms (acceptable-use policy) rather than a deployment gate the provider controls.
- Other providers (Microsoft, Amazon, Cohere, Mistral) publish their own responsible-AI documentation; read the version applicable to the vendor on the team's portfolio.
- The **Seoul Frontier AI Safety Commitments** (May 2024) are a cross-lab public commitment under which many of these frameworks were strengthened; reading that document gives the EM context for why the frameworks exist in the shape they do.

## Core concept 2 — the four things an EM uses these frameworks for

A frontier-lab governance framework is a hundred-plus-page document. The EM is not implementing it. Four concrete uses that justify the reading time.

### Use 1 — translating capability tier to local safeguard change

When the provider publishes that a new model version is at a higher capability tier (ASL-level step up, OpenAI risk-category move from medium to high, FSF CCL approach), the EM asks four questions about the product:

- Does the product use this model on a surface where the elevated capability creates a new attack surface? (A model with meaningfully improved cyber capability raises the stakes on any surface that exposes a code-execution tool.)
- Does the red-team scope (chapter 04) need to be expanded to cover the new category?
- Does the guardrail stack (chapter 03) need an additional layer to bound the new capability — for example, a tool-allow-list contraction, an HITL extension, a classifier choice migration?
- Does the fallback model (mod-105 chapter 04) need to be re-selected if the primary's posture is now materially different?

A yes to any of these triggers a re-sign of the release triad memo (chapter 05 element 4). The framework's own threshold table tells the EM *when* this is likely.

### Use 2 — reading the system / model card for product-relevant disclosures

Each major model release carries a system card (Anthropic, OpenAI) or technical report (Gemini) that describes evaluations run, risks identified, and mitigations applied. These documents name — concretely — the behaviours the provider has observed and is willing to disclose. Read them before upgrading. Specific things to look for:

- Evaluations run against the model in categories relevant to the product (code generation, long-document reasoning, tool use, agentic planning).
- Behaviours observed that are **not fully mitigated** — the lab's own disclosure of residual risk is the shortest path to a prioritised local red-team category.
- Policy-level deployment constraints — the provider's usage policy applicable to the model, which may differ across tiers.
- Known regressions or capability trade-offs documented against earlier versions — these are the product evaluation surface's first candidate cases for a regression test.

The system card goes in the selection memo (mod-105 chapter 03) as a citation, and in the data-posture memo (mod-105 chapter 05) where its deployment-constraint disclosures are relevant.

### Use 3 — framing the quarterly exec conversation

The executive conversation about "AI safety" is almost always asking two questions: what is the organisation exposed to, and what is the mitigation story? The frontier-lab frames give the manager the vocabulary to answer concretely.

A sample two-paragraph answer:

> Our primary model is Anthropic Claude [snapshot], operating at ASL [tier per current RSP]. The published system card discloses evaluations in biological, cyber, and autonomous-replication categories; our product does not touch the biological category, is adjacent to cyber (via code-generation surface), and does not expose agentic self-replication affordances. Our local red-team engagement for Q4 scoped specifically to the cyber-adjacent surface against the new tier (chapter 04 commissioning brief dated [date]). Findings: [two sentences]. Our release triad for the current quarter is green on model and amber on safety (one open high finding, mitigation deployed, accepted-risk expiry 2027-06).

> Our fallback model is OpenAI GPT [snapshot], which per Preparedness Framework is at risk-level [current level] in [categories]. The fallback is invoked on [triggers]; when the fallback is active, the same guardrail stack applies, plus the following additional controls: [one sentence]. The decision to use this fallback (vs. switching to a different provider) is re-evaluated each quarter per the portfolio review (mod-105 chapter 07).

This is an answer that cites the provider's own framework, maps it to the local triad, and demonstrates the organisation's safety posture is coherent across the provider-to-product boundary.

### Use 4 — informing the model-tier migration brief

When the model portfolio (mod-105 chapter 07) proposes a migration to a new model generation, the migration brief cites the provider's governance framework determination for both the outgoing and the incoming model. Specific sections of the brief that read the framework:

- **Capability delta** — the provider's own framework-based characterisation of what has changed between the two generations (not just benchmark scores).
- **Deployment-tier change** — any change in the usage policy, the provider-applied mitigations, or the data-use defaults.
- **Local red-team re-scope** — which categories need to be re-run (chapter 04 cadence), given the capability delta.
- **Release triad re-sign** — the pre-condition on the migration shipping is a re-signed triad memo; the migration is, formally, a re-launch at the chapter 05 severity tier.

The migration brief is the mechanism that keeps the triad honest across model generations.

## Core concept 3 — what to subscribe to, as a reader

The frameworks and their associated documents are versioned and updated. The manager's subscription log (mod-105 chapter 07) should include:

- Each provider's governance-framework page as a top-level subscription.
- Each provider's blog / news feed, which is where updates to the framework and the publication of new system cards are announced.
- The provider's changelog and release-notes feeds (mod-105 resources).
- The provider's system card / technical report page.
- Cross-lab venues where the frameworks are discussed collectively — the Seoul Commitments follow-ups, the UK AI Security Institute's publications, the US AI Safety Institute, the OECD AI governance tracker. Read these at breadth once a quarter; they frame regulator-side expectations that will show up in customer questionnaires later.

The reading discipline mirrors the Model Card reading discipline from mod-105 chapter 07: a standing calendar entry, a named owner (the manager), a shared log in a format the team can grep later.

## Core concept 4 — what these frameworks do **not** do

Three things the frames are explicitly not.

- **They are not a product safety specification.** The provider's framework governs the provider's deployment of the model; it is upstream of the product's own guardrails, red-team, and release triad. A model being at ASL-N does not mean the product is safe; the product-level posture (chapters 02–05) is still required.
- **They are not legal binding on the downstream deployer.** The EU AI Act (chapter 08) is binding; the provider's framework is a public commitment the provider holds itself to. The EM's regulatory obligations are governed by the law, not by the provider's framework, and the mapping requires counsel (chapter 08).
- **They are not a substitute for the data posture.** A model deployed under a strong governance framework can still send prompts to a US-based inference endpoint under terms that do not match the customer's EU-residency expectation. The data leg of the triad (chapter 05) is independent.

Chapter 01's deferrals apply: reading the framework is the manager's job; translating it into product safeguards is a partnership with the IC safety practitioner; translating it into legal obligations is counsel's job.

## Anti-patterns to name and defeat

- **Reading the framework once and declaring the subject complete.** The frameworks are updated, often. A subscription log that includes the framework page but is read annually is not reading the framework; it is reading a stale copy.
- **Confusing "frontier safety" with "product safety."** The two share vocabulary (prompt injection, red-team, evaluation) but operate at different altitudes. A product that is safe at the product boundary can still be running on a model whose capability has advanced faster than the team has re-scoped the red-team; the frontier frame catches the second gap.
- **Over-indexing on one provider's vocabulary.** The three frameworks are not identical; adopting one lab's vocabulary as the team's exclusive language makes it harder to reason about products that span providers. The EM uses each provider's framework when discussing *that* provider's model, and a provider-agnostic language (OWASP, NIST) when discussing the product.
- **Mistaking a provider's framework for the regulator's requirement.** The EU AI Act Article 15 (chapter 08) requires accuracy, robustness, and cybersecurity measures for high-risk systems. The provider's RSP is a component the team may rely on *in part* for compliance, but the regulator is reading the law, not the framework.

## What good looks like

- The manager has read the current version of each framework applicable to the providers on the portfolio inventory (mod-105), with a note in the subscription log on the date and the salient disclosures.
- The release triad memo (chapter 05) for every HIGH-severity launch cites the provider's determination for the model the launch depends on.
- A new model generation does not ship on the portfolio (mod-105 chapter 07) until the EM has read the system card and determined whether the capability delta requires a local red-team re-scope.
- The quarterly executive update on AI safety uses the framework vocabulary when discussing the frontier posture, and the product vocabulary (chapters 02–05) when discussing the product posture.

## What bad looks like

- The manager has not read the applicable framework in the last six months.
- Model-generation migrations happen on the portfolio cadence without reference to the provider's governance determination for either generation.
- The team cannot say, in the frontier-lab vocabulary, where the model it depends on sits on the capability tier the provider publishes.
- An executive question about "AI safety risk" gets answered in product-only terms, missing the frontier-tier framing the executive was asking about.

## Summary

- Anthropic's RSP, OpenAI's Preparedness Framework, and DeepMind's FSF are the three leading frontier-lab governance frameworks. Each publishes capability tiers / thresholds / risk categories, evaluations the lab runs against its own models, and deployment-commitment bundles that attach as capabilities rise. Meta and other providers publish their own documents; the EU AI Act (chapter 08) is separate and binding.
- The manager reads these frameworks for four purposes: translating capability tier changes to local safeguard changes, reading the system / model card for product-relevant disclosures, framing the executive conversation on AI safety, and informing the model-tier migration brief.
- The frameworks are not a product safety specification, not legal binding on the downstream deployer, and not a substitute for the data posture. The product-level triad (chapter 05) is still required.
- The subscription log (mod-105 chapter 07) carries the framework pages, the blog feeds, and the system-card pages for every provider on the inventory; reading is a quarterly calendar discipline, not a once-and-done.

The next chapter picks up the fourth manager-owned artefact: the agent / tool-use authorisation boundary that governs what the model is allowed to do on the product's behalf.
