# Prerequisites — AI Engineering Manager track

**Role level:** 45 (manager scope — AI Engineering family)
**Track:** `ai-engineering-manager-learning`

This is an engineering-leadership track that assumes a solid foundation in both general engineering management and in applied-AI engineering. Fundamentals from either side are **not** re-taught here — the modules step directly onto the AI-engineering-management surface: hiring and leveling across the AI-engineering sub-ladders, planning under model non-determinism, eval-driven SDLC, vendor + model portfolio, release triad, AI-specific incident response, performance management specialised for AI engineers, team shape and operating cadence, budget / FinOps / build-vs-buy, cross-functional partnership, and executive communication.

## Assumed lower-level curriculum

Complete or be practically fluent in the equivalent of these lower-level tracks before starting:

- **[`ai-augmented-team-manager-learning`](https://github.com/ai-workforce-curriculum/ai-augmented-team-manager-learning) — level 40** (AI Workforce family) — the team-scale operating system for AI-augmented work: team adoption roadmap and workflow redesign, review norms and eval commissioning, team productivity and AI-ROI measurement, team-scope enablement, agent-delegation at team scope, hiring / performance / feedback with AI in play, team-scope governance translation, vendor / budget / tool selection at team scope. The engineering-management track layers the AI-engineering-building scope on top of this sibling-below; team-management rituals are not re-taught.
- **[`applied-ai-engineer-learning`](https://github.com/ai-engineering-curriculum/applied-ai-engineer-learning) — level 30** (AI Engineering family) — core applied-AI IC craft (prompting, structured output, tool use, retrieval, evals, traces, observability, lightweight fine-tune). This manager role must be able to read a trace, review a judge rubric, challenge a prompt change — the IC craft is not re-taught; it is the hiring bar.
- **[`ai-eval-engineer-learning`](https://github.com/ai-engineering-curriculum/ai-eval-engineer-learning) — level 30** — evaluation engineering IC craft (golden datasets, judges, trace instrumentation, eval-gated CI/CD, online eval). This manager commissions and requires the practice from mod-104; the craft depth is owned by this IC track.
- **[`agentic-ai-engineer-learning`](https://github.com/ai-engineering-curriculum/agentic-ai-engineer-learning) — level 30** — agentic / tool-use IC craft. This manager hires and manages this IC family; the agent-authoring mechanics are not re-taught.
- **[`ai-assisted-software-engineer-learning`](https://github.com/ai-engineering-curriculum/ai-assisted-software-engineer-learning) — level 20** — IC-scope AI-assisted engineering, prompt craft, agent-authoring mechanics. The manager commissions the team's AI-assist stack (mod-109); IC-level craft defers down.

## Assumed working knowledge

Even without holding the equivalent role formally, you should be practically fluent in:

- **General engineering management canon** — 1:1s, feedback, OKRs, hiring loops, performance review cycle. The canon: Fournier *The Manager's Path*, Grove *High Output Management*, Larson *An Elegant Puzzle* + *Staff Engineer*, Lopp *Rands in Repose*, Google re:Work. This track does NOT re-teach management fundamentals — mod-108 specialises them for AI engineering.
- **General software engineering** — the shape of a modern SWE team (source control, PR + review, CI/CD, test strategy, observability, on-call rotation, SRE practice). This track assumes it; mod-104 and mod-107 extend it with AI-specific additions.
- **Program management** — status reporting, risk registers, decision logs, portfolio coordination. Not re-taught; mod-103 layers AI-roadmap specifics on top.
- **Working AI-engineering fluency** — comfortable reading a prompt, a trace, an eval report, a judge rubric, a retrieval-eval breakdown, a tool-use schema, a model card, a red-team summary. Comfortable using Cursor / Claude Code / Copilot at a daily-user level. This role is a hands-off manager, not an IC — but cannot credibly review work the holder cannot read.
- **Working financial literacy** — reading a cost model, defending a budget, projecting ROI on bounded initiatives, partnering with Finance without being Finance. mod-110 layers AI-engineering economics on top.
- **Executive communication** — writing a one-page memo, an exec pre-read, a board update, a strategic briefing; speaking to a VP / C-suite audience without over-claiming. mod-112 adds AI-specific framing.
- **Basic AI / ML literacy** — distinguishing a foundation model from a fine-tune, an LLM from a classifier, a retrieval system from a trained model; familiar with OWASP Top 10 for LLM Applications and the NIST AI RMF at the vocabulary level.

## Not assumed, taught here

You do **not** need prior experience with:

- The AI-engineering ladder, the role-matrix across applied / agentic / eval / RAG / fine-tune / senior IC / staff IC, or the AI-specific interview loop. mod-102 covers them.
- The confidence-banded roadmap, prototype → MVP → GA promotion gate, or research-to-production handoff. mod-103 covers them.
- The eval-gated SDLC, the prompt / judge / golden-dataset registries as code-review artifacts, the offline + online eval cadence, or the data flywheel. mod-104 covers them.
- The model + provider portfolio across the current frontier, DPA-reading for AI vendors, fallback + multi-model routing, or open-weights vs. API cost reasoning. mod-105 covers them.
- The guardrails-as-product-spec discipline, red-team commissioning briefs, the model + data + safety release triad, or the agent-tool-use authorization boundary. mod-106 covers them.
- AI-specific incident response — jailbreak, hallucination, judge drift, vendor-upgrade regression, tool-use failure, moderation false-positive storm — or the blameless postmortem → eval-fixture loop. mod-107 covers them.
- Performance management calibrated to AI-engineering levels, anti-atrophy practice for frontier-tooling-era ICs, or growth paths across AI sub-ladders. mod-108 covers them.
- The AI-specific operating cadence (model-watch meeting, prompt + eval + trace review, incident replay, vendor-release review) or the IC AI-assist stack rollout. mod-109 covers them.
- FinOps for inference, the team cost model with per-feature attribution, or the quarterly portfolio allocation with wind-down. mod-110 covers them.
- The cross-functional release-approval workflow, the AI-infra customer relationship, or the governance-evidence consumer contract. mod-111 covers them.
- The weekly / monthly / quarterly exec-reporting rhythm specialised for AI engineering, or the demo-discipline review checklist. mod-112 covers them.

## Recommended reading before starting

- [NIST AI Risk Management Framework 1.0 (AI 100-1)](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf) — once, for the GOVERN + MEASURE + MANAGE vocabulary a manager meets at the governance interface.
- [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/) — vocabulary for the risks a release gate must name.
- [EU AI Act, Articles 14 (human oversight), 15 (accuracy / robustness / cybersecurity), 50 (transparency), 72 (post-market monitoring), 73 (serious-incident reporting)](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) — the deployer obligations an EM translates into engineering deliverables.
- [Camille Fournier — *The Manager's Path*](https://www.oreilly.com/library/view/the-managers-path/9781491973882/) and [Will Larson — *An Elegant Puzzle*](https://lethain.com/elegant-puzzle/) — baseline engineering-management canon; mod-108 and mod-109 build on top.
- [Chip Huyen — *AI Engineering* (O'Reilly 2024)](https://www.oreilly.com/library/view/ai-engineering/9781098166298/) — the applied-AI engineering surface this manager leads.
- [Eugene Yan — Patterns for Building LLM-based Systems](https://eugeneyan.com/writing/llm-patterns/) — industry survey of the eval + guardrail + data-flywheel patterns mod-104 and mod-106 require.
- [Anthropic Responsible Scaling Policy](https://www.anthropic.com/rsp), [OpenAI Preparedness Framework](https://openai.com/safety/preparedness/), [Google DeepMind Frontier Safety Framework](https://deepmind.google/discover/blog/introducing-the-frontier-safety-framework/) — read as public provider-side deployment-tier framing an EM depends on.
- [Google SRE Book](https://sre.google/sre-book/table-of-contents/) — Blameless Postmortems, Emergency Response, On-Call chapters; mod-107 adapts them to AI-specific incidents.
- [Dell'Acqua et al. — *Navigating the Jagged Technological Frontier* (HBS WP 24-013)](https://www.hbs.edu/faculty/Pages/item.aspx?num=64700) — the empirical foundation for the mod-109 IC AI-assist rollout and the mod-112 investment-ask narrative.
- A current public model card (Claude, GPT, or Gemini family) — read as a manager would, tracing what you can and cannot promise users.

## Not required, useful to have

- **Prior IC experience** on an AI engineering ladder ([`applied-ai-engineer-learning`](https://github.com/ai-engineering-curriculum/applied-ai-engineer-learning), [`agentic-ai-engineer-learning`](https://github.com/ai-engineering-curriculum/agentic-ai-engineer-learning), [`ai-eval-engineer-learning`](https://github.com/ai-engineering-curriculum/ai-eval-engineer-learning)) or in staff-IC software engineering with AI delivery in the stack.
- **Prior management experience** on a non-AI SWE team is a strong signal — the engineering-management fundamentals transfer; the AI-engineering specifics are the delta this track teaches.
- **PMP / PgMP / Agile certifications** at the vocabulary level — useful for program-management shared language; this track does not require them.
- **[IAPP AIGP](https://iapp.org/certify/aigp/)** — AI Governance Professional certification; useful signal for the governance-interface side of the role (mod-111).

## Not in scope for this track (linked out)

- **IC craft across the AI engineering ladder** — prompting, retrieval, evals, tool-use, fine-tune — owned by [`applied-ai-engineer-learning`](https://github.com/ai-engineering-curriculum/applied-ai-engineer-learning), [`agentic-ai-engineer-learning`](https://github.com/ai-engineering-curriculum/agentic-ai-engineer-learning), [`ai-eval-engineer-learning`](https://github.com/ai-engineering-curriculum/ai-eval-engineer-learning), [`rag-engineer-learning`](https://github.com/ai-engineering-curriculum/rag-engineer-learning), [`fine-tuning-engineer-learning`](https://github.com/ai-engineering-curriculum/fine-tuning-engineer-learning), [`llm-application-developer-learning`](https://github.com/ai-engineering-curriculum/llm-application-developer-learning), [`senior-agentic-ai-engineer-learning`](https://github.com/ai-engineering-curriculum/senior-agentic-ai-engineer-learning).
- **Team-scope general AI-adoption operating system** — owned by [`ai-augmented-team-manager-learning`](https://github.com/ai-workforce-curriculum/ai-augmented-team-manager-learning) (level 40). This track assumes it.
- **Org-wide AI enablement** — CoE, policy co-authoring, org literacy curriculum — owned by [`ai-enablement-lead-learning`](https://github.com/ai-workforce-curriculum/ai-enablement-lead-learning) (level 45, sibling).
- **AI infra manager scope** — model serving, cluster, GPU capacity, training platform — owned by [`ai-infra-team-lead-learning`](https://github.com/ai-infra-curriculum/ai-infra-team-lead-learning) (peer manager, level 45).
- **Engineered risk, red-team, adversarial-ML, guardrail code depth** — owned by [`ai-risk-engineer-learning`](https://github.com/ai-governance-curriculum/ai-risk-engineer-learning) (peer, level 25) and [`agentic-safety-engineer-learning`](https://github.com/ai-engineering-curriculum/agentic-safety-engineer-learning) (peer, level 40).
- **Release-assurance / audit-trail / regulator-facing methodology** — owned by [`ai-evaluation-engineer-learning`](https://github.com/ai-governance-curriculum/ai-evaluation-engineer-learning) (peer, level 35).
- **Internal agent-platform IC build depth** — owned by [`agent-platform-engineer-learning`](https://github.com/ai-engineering-curriculum/agent-platform-engineer-learning) (peer, level 40).
- **Control-library architecture, policy taxonomy, cross-jurisdiction reconciliation** — owned by [`senior-ai-governance-architect-learning`](https://github.com/ai-governance-curriculum/senior-ai-governance-architect-learning) (level 50).
- **AI governance program leadership, board-level reporting, regulator engagement** — owned by [`head-of-ai-governance-learning`](https://github.com/ai-governance-curriculum/head-of-ai-governance-learning) (level 60).
- **Enterprise AI strategy, portfolio P&L, M&A, external executive positioning** — owned by [`chief-ai-officer-learning`](https://github.com/ai-governance-curriculum/chief-ai-officer-learning) (level 70).
- **Staff-plus IC architecture** — owned by [`agentic-systems-architect-learning`](https://github.com/ai-engineering-curriculum/agentic-systems-architect-learning) (level 48) and [`principal-ml-engineer-learning`](https://github.com/ml-engineering-curriculum/principal-ml-engineer-learning) (level 48). This manager partners with but does not re-teach.
- **Legal opinion** — routed to counsel.
- **Contract execution, billing mechanics** — routed to procurement and finance.
- **SSO / IAM implementation** — routed to IT identity teams.
- **SOC-side incident handling** — out of scope; this role feeds AI-specific signal to SecOps.
