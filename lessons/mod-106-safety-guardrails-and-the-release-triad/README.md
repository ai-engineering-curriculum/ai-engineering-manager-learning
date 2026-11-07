# mod-106: Safety, Guardrails, and the Model + Data + Safety Release Triad

> Sixth module of the AI Engineering Manager track. Builds the manager-owned operating system for product safety: guardrails written as product specs, red-team work commissioned and tracked, a model + data + safety release triad gated at launch, an agent / tool-use authorisation boundary reviewed before every new tool, the public-provider governance frameworks (Anthropic RSP, OpenAI Preparedness, DeepMind FSF) read as inputs to deployment-tier decisions, and the EU AI Act's Articles 14, 15, and 50 translated into concrete engineering deliverables. Red-team craft defers to `ai-risk-engineer` (level 25) and `agentic-safety-engineer` (level 40); classifier internals defer to `applied-ai-engineer` (level 30); agent-authorisation implementation defers to `agent-platform-engineer` (level 40); legal binding defers to counsel. This module teaches the commissioning and gating discipline the manager owns around all of them.

**Estimated effort:** 14 hours · **Role level:** 45 (AI Engineering family)

## Learning objectives

By the end of this module you should be able to:

- Own safety as a product requirement: write guardrails as product specs (input filters, output validators, tool-invocation policy, human-in-the-loop checkpoints, refusal policy, content classifier choice) that an applied-ai-engineer IC can build against — grounded in OWASP Top 10 for LLM Applications, NIST GenAI Profile, NeMo Guardrails, Llama Guard, OpenAI Moderation.
- Commission red-team work from `ai-risk-engineer` (peer, level 25) and `agentic-safety-engineer` (peer, level 40): author the red-team brief, review the findings, route the fixes — without performing the red-team at IC level.
- Operate the model + data + safety release triad as the manager's gate: can we ship this if we honestly see the eval score + the red-team findings + the data posture + the incident playbook?
- Read Anthropic RSP, OpenAI Preparedness Framework, DeepMind FSF as public-provider deployment-tier framing an EM uses when depending on provider models at new capability tiers.
- Shape the agent / tool-use authorization story at the product boundary (R15) — OAuth, scoped service identity, MCP contract, human-in-the-loop checkpoints, blast-radius containment — where IC craft defers to `agentic-ai-engineer` (level 30) and `agent-platform-engineer` (level 40).
- Translate EU AI Act Articles 14 (human oversight), 15 (accuracy / robustness / cybersecurity), 50 (transparency) into engineering deliverables the team ships without replacing legal review.

## Chapters

1. [The Manager's Altitude for Safety](01-the-managers-altitude-for-safety.md) — what the manager owns (guardrails spec, red-team brief, release triad, authorisation boundary) vs. what the IC owns (classifier internals, attack-prompt craft, OAuth / MCP implementation); the four manager-owned artefacts; the deferrals to AI Risk, Applied AI, Platform, and counsel.
2. [Guardrails as Product Spec](02-guardrails-as-product-spec.md) — OWASP Top 10 for LLM Applications and NIST Generative AI Profile as the two frames the spec maps against; the six sections (surface and risk model, input filters, output validators, tool-invocation policy, HITL checkpoints, refusal policy); the review checklist; a worked skeleton.
3. [The Guardrails Tech Landscape at EM Depth](03-the-guardrails-tech-landscape.md) — the five categories of tooling (orchestration, hosted classifiers, open-weights / self-hosted classifiers, structured-output validators, injection-specific detectors); defence in depth; the pick-rules; operational concerns the manager must name.
4. [Commissioning Red-Team Work](04-commissioning-red-team-work.md) — the commissioning brief's six sections; the three engagement shapes (internal IC specialist, external vendor, bug-bounty); the findings-review ritual (fix / mitigate / accept / block); the cadence that prevents launch-week scrambles.
5. [The Model + Data + Safety Release Triad](05-the-model-data-safety-release-triad.md) — the three legs (model, data, safety) plus the incident-playbook pin; the one-page launch memo; ambers and the override protocol; triad patterns at HIGH / MEDIUM / LOW severity tiers.
6. [Reading RSP, Preparedness, and FSF](06-reading-rsp-preparedness-and-fsf.md) — the three leading frontier-lab governance frameworks at commissioning-manager depth; the four EM uses (capability tier → local safeguard, system card reading, exec conversation framing, model-tier migration brief); the subscription log discipline.
7. [Agent / Tool-Use Authorisation at the Product Boundary](07-agent-tool-use-authorization-at-the-product-boundary.md) — the five properties (identity, scope, pre-invocation checks, HITL checkpoints, blast-radius containment); the boundary-review ritual; the deferrals to `agent-platform-engineer` on OAuth / MCP / HITL-UX craft; anti-patterns to defeat.
8. [EU AI Act Articles 14, 15, 50 as Engineering Deliverables](08-eu-ai-act-articles-14-15-50-as-engineering-deliverables.md) — Article 14 (human oversight) → HITL inventory, reviewer competence, automation-bias mitigation, oversight monitoring, stop capability; Article 15 (accuracy / robustness / cybersecurity) → eval registry, guardrails + red-team + fallback + robustness evals, authorisation boundary; Article 50 (transparency) → AI disclosure, machine-readable marking (C2PA), deepfake labelling; the compliance-mapping memo counsel co-signs.

## Exercises

- [Exercise 01 — Guardrails as product spec for one feature](exercises/exercise-01-guardrails-as-product-spec-for-one-feature.md) *(3 h)*
- [Exercise 02 — Red-team commissioning brief](exercises/exercise-02-red-team-commissioning-brief.md) *(2 h)*
- [Exercise 03 — Release triad go/no-go template](exercises/exercise-03-release-triad-go-no-go-template.md) *(3 h)*
- [Exercise 04 — Agent tool-use authorisation boundary review](exercises/exercise-04-agent-tool-use-authorization-boundary-review.md) *(3 h)*

Each exercise ships its prompt here; reference solutions live in the paired [`ai-engineering-manager-solutions`](https://github.com/ai-engineering-curriculum/ai-engineering-manager-solutions) repo.

## Supporting material

- `labs/` — long-form hands-on labs (planned).
- `quizzes/` — knowledge checks (planned).
- [`resources.md`](resources.md) — curated external references used across chapters.

## Where this module fits

- **Builds on mod-101 through mod-105.** The IC role matrix from mod-102 is where `ai-risk-engineer`, `agentic-safety-engineer`, `agent-platform-engineer`, `agentic-ai-engineer`, and `applied-ai-engineer` are defined. The release-gate policy from mod-104 chapter 03 is the surface the release triad (chapter 05) extends. The portfolio inventory and selection memos from mod-105 are inputs to the data and safety legs of the triad.
- **Pairs with mod-104 (eval-gated SDLC).** The safety-eval cases are entries in the eval registry (mod-104 chapter 04); the model leg of the triad reads mod-104's eval evidence; the online-eval cadence (mod-104 chapter 06) is where safety regressions surface after launch.
- **Pairs with mod-105 (vendor and model portfolio).** The data leg of the triad reads the DPA posture from mod-105 chapter 05; the fallback plan from mod-105 chapter 04 is the resilience side of Article 15.B; the quarterly portfolio review (mod-105 chapter 07) consumes RSP / Preparedness / FSF reading (chapter 06 of this module).
- **Feeds mod-107 (AI incident response).** The incident-playbook pin on the triad (chapter 05) is the module-107 entry; the kill-switches, audit logs, and runbook sections from chapters 05 and 07 are mod-107's day-one playbooks.
- **Pairs with mod-108 (performance management).** The data flywheel's labelling role (mod-104 chapter 07) extends to safety-finding labelling here; growth paths for ICs partnering on this module span the AI Risk ladder and the Applied AI ladder.
- **Pairs with mod-109 (team shape and operating cadence).** The red-team cadence (chapter 04), the release-triad review clock (chapter 05), and the quarterly authorisation-boundary review (chapter 07) are beats on the operating cadence mod-109 designs.
- **Pairs with mod-110 (budget, FinOps, build-vs-buy).** Guardrail-stack spend is a FinOps line item (chapter 03); red-team engagements are funded on a quarterly cadence (chapter 04).
- **Pairs with mod-111 (cross-functional partnership).** The counsel partnership (chapter 08), the procurement / DPA partnership (triad data leg), and the security / risk partnership (chapters 04, 07) are three of the cross-functional surfaces mod-111 covers.
- **Pairs with mod-112 (executive communication).** The quarterly safety update (chapter 06's exec-framing section), the release-triad memo (chapter 05), and the compliance-mapping memo (chapter 08) are inputs to mod-112's executive-communication artefacts.
