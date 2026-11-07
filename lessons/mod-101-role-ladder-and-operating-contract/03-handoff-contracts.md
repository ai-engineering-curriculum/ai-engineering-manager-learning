# Handoff Contracts: Down, Sideways, Up, and Out

Chapter 01 drew the six neighbours around you. This chapter goes further: it enumerates every role you either manage, consume evidence from, partner with as a peer, report to, or deliberately route work to. For each, it names *what crosses the boundary*: an artefact, a decision, an SLO, or a sign-off. If a boundary has nothing crossing it, the relationship is undefined — and under-defined relationships are where work falls on the floor.

## Motivation

The failure mode this chapter prevents has three signatures:

- "I thought *they* owned that." Two roles each assumed the other was handling the eval design / incident sign-off / vendor DPA review. Nobody did.
- "Why is this landing on me?" An ambiguous handoff gets resolved by whichever role is nearest, not whichever role is correct. Over a quarter this adds up to a miscalibrated job.
- "We need to loop in …" said for the third time in a planning meeting, which means nobody has written down who gets looped in and for what.

The contracts below are the antidote. They are not RACI charts in the full ceremonial sense — they are the one-line answer to "what moves across this boundary, in which direction, in what shape."

## Down: the IC tracks you manage

Your team is drawn from these ladders. Different teams have different mixes; most AI engineering teams at 10–30 people include at least four of them.

### `applied-ai-engineer` (level 30, AI Engineering family)

The default generalist on an AI engineering team. Owns the integration from product code through prompt / context / model / output back to product code.

- **You hand them:** feature-level requirements, confidence-banded ETAs, the registered prompt and eval the feature must meet, the release triad bar they ship against.
- **They hand you:** an implemented feature with eval results, trace samples, cost per call, and a readiness read on the triad.

### `agentic-ai-engineer` + `senior-agentic-ai-engineer` (levels 30 / 40)

Specialists in multi-step agents and tool-use systems. The senior at level 40 is often the tech-lead for an agent-shaped team.

- **You hand them:** a tool contract, a HITL policy, an authorization story at the product boundary (R15 in the requirements catalog).
- **They hand you:** a working agent with tool-invocation traces, a failure-mode map, and a kill-switch surface you can operate under incident.

### `ai-eval-engineer` (level 30)

Specialist in evaluation craft — golden-dataset design, LLM-as-judge design, eval harness implementation, continuous eval pipelines.

- **You hand them:** the quality claim the product wants to make, the acceptable regression threshold, the budget for labelling.
- **They hand you:** an eval suite registered and runnable in CI, a judge-case corpus, and a report showing what the current product does and does not pass.

### `rag-engineer` (level 30)

Specialist in retrieval-augmented systems — chunking, embedding, index choice, re-ranking, retrieval evals.

- **You hand them:** the corpus contract (what's in, what's out, how it's updated), freshness SLO, retrieval-quality bar.
- **They hand you:** a retrieval pipeline with coverage, latency, and recall/precision measurements on the product's golden set.

### `fine-tuning-engineer` (level 30)

Specialist in post-training on top of a base model — SFT, preference data, PEFT/LoRA, evaluation of the resulting checkpoint.

- **You hand them:** the task spec, the training-data contract (origin, license, PII posture), the acceptance evals, the deployment surface.
- **They hand you:** a trained checkpoint with model-card-shaped documentation, eval results against baselines, and a cost/latency read vs. the vendor-API alternative.

### `llm-application-developer` (level 20)

The on-ramp IC role — ships LLM-powered features under more senior guidance.

- **You hand them:** scoped feature work, a senior pairing structure, the prompt/judge registry conventions they are expected to use.
- **They hand you:** shipped feature work, questions that often surface gaps in your onboarding, and growth signal you feed into performance management.

### Cross-cutting: `ai-assisted-software-engineer` (level 20, commissioned stack)

This one is sideways in the ladder but *consumed* by every IC on the team: the AI-assist stack (Cursor, Claude Code, Copilot, Windsurf, Cody, Sweep, etc.) the engineers themselves use.

- **You own (R14):** choosing the stack, measuring it honestly (Dell'Acqua et al., Peng et al.), setting policy for when and how it is used.
- **You defer down for depth** to the `ai-assisted-software-engineer` curriculum — the IC-level skill of using the stack well is not something you re-teach; you hire and coach for it.

## Sideways: the governance and specialist peers whose evidence you consume

These are peers, not reports. They produce evidence you need; your team produces evidence they need.

### `ai-risk-engineer` (level 25, AI Governance family)

Engineered red-team / adversarial-ML / guardrail depth. Peer at a lower level but a specialist whose output you commission.

- **They hand you:** red-team findings, adversarial eval results, guardrail-effectiveness readouts, specific jailbreak reproductions.
- **You hand them:** access to pre-release artefacts, a scoped brief (threat model, scope, out-of-scope), and action-item ownership for findings routed to the team.

### `ai-evaluation-engineer` (level 35, AI Governance family)

Release-assurance / audit-trail / regulator-facing evaluation methodology.

- **They hand you:** release-assurance eval design, audit-trail requirements, regulator-shaped documentation (useful for Article 73 narratives when applicable).
- **You hand them:** eval results, data lineage, model cards, release decisions and the evidence behind them.

### `agent-platform-engineer` (level 40, AI Engineering family)

Peer specialist in the internal agent platform your team may build on.

- **They hand you:** platform SLOs, tool-contract standards, deprecation notices.
- **You hand them:** workload shape, bug reports, feature requests, and shared on-call expectations.

### `agentic-safety-engineer` (level 40, AI Engineering family)

Frontier-agent red-team specialist for the harder end of safety work.

- **They hand you:** frontier-shaped red-team results, propagation / self-replication / autonomy assessments.
- **You hand them:** access, brief, and a decision owner for findings.

### `forward-deployed-engineer` (level 35, AI Engineering family)

Customer-embedded AI build, when the team or its siblings ship that shape of work.

- **They hand you:** field reports, specific customer failure modes, pre-sales constraints.
- **You hand them:** reusable components from your team's build, release-gate expectations, incident collaboration.

### `ai-infra-team-lead` (level 45, peer manager, AI Infra family)

The infra-shaped sibling manager. Co-owner of the shared surface — serving, routing, fallback, cost.

- **They hand you:** serving SLOs (latency, error, throughput), rate-limit posture, cost telemetry, incident signals from the infra side.
- **You hand them:** traffic shape, model-mix forecasts, feature-level cost attribution, and eng-side incident signal that crosses into their surface.

### `ai-augmented-team-manager` (level 40, AI Workforce family, sibling below)

The team-management sibling. Partners when your team's output is adopted by a human-plus-AI team elsewhere.

- **They hand you:** workflow failure reports, adoption signal, usage policy they've written for their team's consumption of your tools.
- **You hand them:** release notes, change notices, honest capability description.

### `ai-enablement-lead` (level 45, AI Workforce family, sibling at level)

The org-wide enablement peer.

- **They hand you:** CoE policy, cross-function training programs, platform decisions affecting all teams.
- **You hand them:** engineering-side input to policy, ground-truth about what "real" AI engineering looks like, so literacy programs stay honest.

## Up: the staff-plus ICs and the executive

### `agentic-systems-architect` (level 48, AI Engineering family)

The staff-plus IC architecture partner.

- **They hand you:** architecture decision records, technical direction documents, cross-team standards, review of your team's design docs.
- **You hand them:** headcount and roadmap that lets their direction land, delivery evidence that grounds the next decision, the specific technical problems that need an architectural answer.

### `principal-ml-engineer` (level 48, ML Engineering family)

The ML-flavoured staff-plus counterpart.

- **They hand you:** ML-methodology direction, eval-design review, fine-tune / data-mix decisions at architectural altitude.
- **You hand them:** product constraints, eval-fixture coverage gaps, the hard measurement questions you need settled.

### `senior-ai-governance-architect` (level 50, AI Governance family)

Control-library architecture, policy taxonomy, cross-jurisdiction reconciliation.

- **They hand you:** control-library primitives, policy mappings, structural review of your release-gate design.
- **You hand them:** application-layer feedback — where the controls are too tight, too loose, or miscast.

### `head-of-ai-governance` (level 60, AI Governance family)

Governance program leadership.

- **They hand you:** the program's shape, deadlines (including regulatory ones), prioritisation across functions.
- **You hand them:** evidence, deliverables, and the engineering-side view of program feasibility.

### `chief-ai-officer` (level 70, AI Governance family)

Executive altitude.

- **They hand you:** strategy, portfolio allocation, external and board-facing narrative constraints.
- **You hand them:** weekly roll-ups, monthly deep-dives, honest regression stories, demo discipline, and investment asks defended with empirical evidence.

## Out: deliberately not yours

Each of these is work the role is sometimes asked to absorb and should not. Naming them is part of the contract.

- **Legal counsel** — you do *not* give legal opinions on DPAs, model-provider contracts, EU AI Act applicability, or privacy regulation. You read for the vocabulary, flag risks, and route to counsel.
- **Procurement and finance** — contract execution, billing mechanics, PO workflow. You bring the request and the business case; procurement and finance own the paperwork.
- **IT / identity** — SSO, IAM, device management, directory integration. You consume the identity surface; you do not build it.
- **Security operations / DFIR** — SOC-side investigation, breach response, forensics. You feed AI-specific signal to SecOps; they run the SOC-side investigation.
- **Research** — frontier research from ML research scientists or external partners. You own the research-to-production handoff gate (reproducibility, eval scope, documentation, on-call readiness — R16), not the research itself.

## A concrete read: one release, four handoffs

A single feature launch for an agent that can read customer documents and answer questions. Trace the handoffs:

1. **Down:** the applied-ai-engineer builds the integration; the agentic-ai-engineer designs the tool/HITL surface; the rag-engineer owns retrieval over the document corpus; the ai-eval-engineer commissions the eval suite.
2. **Sideways:** the ai-risk-engineer runs a red-team pass and returns findings; the ai-evaluation-engineer reviews the release-assurance package; the ai-infra-team-lead signs off on serving capacity and routing.
3. **Up:** the agentic-systems-architect approves the design doc; the principal-ml-engineer reviews the eval methodology; the CAIO is briefed on the launch narrative.
4. **Out:** legal reviews the DPA implications of the document corpus; procurement finalises the enterprise contract; IT configures the SSO; SecOps is briefed on the incident path.

Across all four directions, the manager's role is the same: *name the handoff, name what crosses the boundary, and own the moments where no neighbour would.*

## Summary

- Every relationship around you should have a one-line answer to "what crosses this boundary, in which direction, in what shape."
- Down: the AI engineering IC ladders — you commission work and consume artefacts.
- Sideways: governance peers, platform peers, sibling managers — you co-own release evidence and the shared on-call surface.
- Up: staff-plus ICs set architecture and methodology; the executive sets strategy and portfolio.
- Out: legal, procurement, IT identity, SecOps, research — explicitly not yours, routed with respect.
- Under-defined boundaries are where work falls on the floor. The contract names them before the floor shows up.

The next chapter translates all of this into the 90-day entry plan — what you do in weeks 1–2, 3–6, and 7–13 when you walk into this role.
