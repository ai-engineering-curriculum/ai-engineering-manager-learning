# Where This Role Sits on the Ladder

The AI Engineering Manager (level 45, AI Engineering family) is one of several roles that look similar on a résumé and overlap on Slack. The first job of this module is to draw the lines between them precisely enough that you can say, in one sentence per neighbour, what is yours and what is not. Everything downstream — the hiring loop you run, the evals you commission, the release gate you chair — depends on holding that line.

## Motivation

A level-45 engineering manager who cannot name their neighbours ends up absorbing the wrong work. The two common failure modes:

- **Scope drift up.** The manager quietly starts owning architectural decisions that belong to a staff-plus IC, or AI strategy decisions that belong to a CAIO. The staff IC disengages; the strategy gets set by the person loudest in the room rather than the person accountable for it.
- **Scope drift sideways.** The manager starts "owning" governance because governance is new and nobody else is around, or starts running enablement sessions for non-engineers because the ask lands in their inbox. Within a quarter they're doing two half-jobs instead of one full one.

Both failures look like initiative in month one and attrition in month nine. Reading the ladder is prevention.

## The six neighbouring roles

Six roles border this one. For each: where they sit on the level ladder, what they own that you do not, and the one handoff that most often gets confused.

### 1. `ai-augmented-team-manager` — level 40, AI Workforce family

The team-management sibling *below* this role. They own the general AI-adoption operating system for a team whose work *uses* AI: picking the assistant stack, writing the team-level AI usage policy, running the adoption roadmap, adapting rituals around AI-assisted work. The general engineering-management canon (1:1s, feedback, hiring loop mechanics, team-level roadmap) lives at this altitude too, and this track assumes it rather than re-teaching it.

- **What they own that you do not:** the fundamentals of running a team of humans who use AI. You inherit these.
- **What you own that they do not:** the engineering-specific scope that applies when the team is *building* with models — eval-gated SDLC, model-vendor portfolio, release triad, AI-specific incident response, AI-engineering sub-ladder leveling.
- **Most common confusion:** "write a team AI usage policy." The 40-level sibling owns the usage-policy artefact for how humans use AI in daily work. You own the engineering-side policy that gates what ships to production (eval thresholds, release gate, on-call path).

### 2. `ai-enablement-lead` — level 45, AI Workforce family (sibling)

Same level, different scope. The enablement lead runs the org-wide AI Center of Excellence: literacy programs, cross-function training, non-engineer policy co-authoring with legal and HR, platform decisions that apply to everyone (not just engineers). Their customers are every function; yours is one engineering team or group.

- **What they own that you do not:** org-wide enablement, non-engineer literacy, CoE charter.
- **What you own that they do not:** the engineering function's build operating system.
- **Most common confusion:** your team's AI-assisted-development rollout. The enablement lead may set the overall tooling landscape; you make the engineering-team call (which assistant stack, which repos, what the measurement looks like for your engineers).

### 3. `ai-infra-team-lead` — level 45, AI Infra family (peer manager)

The infra-shaped sibling. Same level, same management craft; different stack. The infra manager runs a team that ships model-serving, GPU capacity, inference infrastructure, training platform. You run a team that ships AI products and agents on top of that infrastructure.

- **What they own that you do not:** serving, capacity, GPU economics, inference SLOs, platform-level reliability.
- **What you own that they do not:** the application layer — prompts, context, retrieval, agents, orchestration, product-level evals, product incidents.
- **Most common confusion:** who owns an outage. The infra side owns "serving is down / slow / rate-limited." You own "the feature is producing wrong answers / jailbroken output / regressed since the last release." The shared surface — model routing, fallback, cost per feature — is co-owned and needs an explicit SLO and incident path.

### 4. `agentic-systems-architect` — level 48, AI Engineering family (staff-plus IC)

The staff-plus IC *above* this role on the architecture track. The architect designs the agent-and-tool-use systems your team builds; they go deeper on orchestration patterns, tool contracts, multi-agent composition, and failure-mode analysis than any manager can while also running a team.

- **What they own that you do not:** the architecture decisions themselves — design docs, cross-team technical standards, multi-quarter technical direction.
- **What you own that they do not:** the team that builds against those decisions — people, headcount, roadmap, performance, delivery.
- **Most common confusion:** who decides "do we adopt framework X / refactor the agent runtime / rewrite the retrieval layer." The architect leads the decision; you resource it, sequence it, and defend the plan upward. If you are making the architectural call alone, you are missing a staff-plus partner.

### 5. `principal-ml-engineer` — level 48, ML Engineering family (staff-plus IC, peer of the architect)

The ML-flavoured counterpart to the architect. Where the architect specialises in agent systems, the principal MLE specialises in model behaviour, training/evaluation methodology, and ML-system design. For a team doing significant fine-tuning, RLHF, preference data, or distillation, a principal MLE is the technical partner the architect alone cannot replace.

- **What they own that you do not:** ML-methodology direction, deep eval-design judgement, fine-tune / data-mix / preference-data decisions at an architectural level.
- **What you own that they do not:** the engineering team and its operating cadence.
- **Most common confusion:** "did we measure that correctly." You chair the release gate; the principal MLE can tell you whether the eval methodology supporting that gate is sound. They do not run the gate.

### 6. `chief-ai-officer` — level 70, AI Governance family (executive)

The executive altitude. The CAIO sets enterprise AI strategy, owns the AI P&L at the company level, represents AI to the board and externally, and makes M&A-shaped calls. Your team is one line in their portfolio.

- **What they own that you do not:** enterprise strategy, executive narrative, portfolio-level investment allocation, external positioning.
- **What you own that they do not:** the operating system of a specific engineering team or group.
- **Most common confusion:** "our AI strategy." The CAIO sets it. You translate it into roadmap, headcount, release cadence, and evidence that the strategy is or is not working. Managers who try to set strategy without CAIO alignment, or wait for strategy without providing the ground-truth evidence to shape it, both fail.

## The ladder as a diagram

```
                                   L70  chief-ai-officer                      (executive strategy)
                                          │
                       ┌──────────────────┼──────────────────┐
                       │                  │                  │
            L48  agentic-systems-architect  L48  principal-ml-engineer         (staff-plus IC)
                       │                  │
                       └────────┬─────────┘
                                │
                        L45  ai-engineering-manager   ◄── you are here         (manager, AI build)
                                │
             ┌──────────────────┼───────────────────┐
             │                  │                   │
   L45  ai-enablement-lead   L45  ai-infra-team-lead   L40  ai-augmented-team-manager
   (org-wide enablement)     (infra-shaped peer)       (team-management sibling below)
                                │
                                ▼
                        IC tracks (L20–L40)
```

Everything else — governance peers, IC sub-ladders you manage, out-of-scope partners — is covered in chapter 03 (`Handoff contracts`). For now, memorise these six and the one sentence that separates each from you.

## A concrete read: one message, six replies

A product lead sends the following to Slack:

> "We want to ship the new agent next quarter. Can one of you own this?"

How each neighbour correctly answers, and what the AI Engineering Manager says:

- **ai-augmented-team-manager (L40):** "If you mean the internal team that will *use* the agent in their workflow, I own the adoption plan for them."
- **ai-enablement-lead (L45):** "If this is a cross-function rollout and we need literacy / policy / CoE support, I'll partner."
- **ai-infra-team-lead (L45, peer):** "If you need me to stand up serving, capacity, and routing for this, I'll scope that side."
- **agentic-systems-architect (L48):** "Happy to lead the architecture — tool contracts, multi-step design, failure-mode map."
- **principal-ml-engineer (L48):** "I'll own the eval and model-choice methodology."
- **chief-ai-officer (L70):** "Where does this fit in the portfolio and the external narrative? Build me a case."
- **you (L45, AI Engineering Manager):** "My team can own delivery. I'll partner with the architect on design, the principal MLE on eval, the infra lead on serving, the governance peers on release evidence. I'll bring a plan with a confidence band and a release gate by Friday."

If more than one of those reads sounds like something you would say, you are absorbing scope that is not yours. Pick the line that matches level 45 in the AI Engineering family and leave the rest.

## Summary

- You are at level 45 in the AI Engineering family: a manager of a team that *builds with* models.
- Six roles border yours: a sibling below (ai-augmented-team-manager), two siblings at level (ai-enablement-lead, ai-infra-team-lead), two staff-plus ICs above (agentic-systems-architect, principal-ml-engineer), and the executive altitude (chief-ai-officer).
- For each, you should be able to say in one sentence what they own that you do not, and what you own that they do not.
- The default failure is scope drift up or sideways in month one. Reading the ladder is the first prevention.

The next chapter reads the operating-contract surface this role inherits from general engineering-management canon and layers AI-engineering-specific scope on top.
