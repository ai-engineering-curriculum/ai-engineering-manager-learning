# mod-108: Performance Management, Growth Paths, and Anti-Atrophy for AI Engineers

> Eighth module of the AI Engineering Manager track. Builds the manager-owned performance-management operating system for a team of AI engineers: an AI-engineering-specific level rubric across six IC shapes and four level anchors, a 1:1 / feedback / review cadence that surfaces the artefact classes the rubric reads on, a growth-plan surface that lets ICs move across sub-ladders (applied / agentic / eval / RAG / fine-tune / infra) and across the IC ↔ manager pivot, an anti-atrophy loop against frontier-assistant over-reliance, a drift-fair calibration process that handles rapid technology drift without punishing drift-exposed engineers, and a credential-honesty read that distinguishes useful growth from theatre. The general management canon (1:1s, feedback mechanics, the review cycle, promotion-packet discipline, PIP mechanics) defers to `ai-augmented-team-manager` (level 40) and is this module's prerequisite, not its content.

**Estimated effort:** 12 hours · **Role level:** 45 (AI Engineering family)

## Learning objectives

By the end of this module you should be able to:

- Specialise the 1:1, feedback, and performance-review practice to AI engineering ladders — what "good" looks like at applied-AI L20/L30/L40, at agentic-AI altitudes, at eval / RAG / fine-tune specialist altitudes, and at the staff-plus IC altitude.
- Author growth plans that cross the applied / agentic / eval / RAG / fine-tune / infra sub-ladders — including the lateral move between an IC and a tech-lead track, and the IC ↔ manager pivot (co-owned with `ai-augmented-team-manager` at level 40).
- Address anti-atrophy risk from over-reliance on frontier assistants: deliberate-practice rotations across model + framework shifts, mandatory read-time against vendor cookbooks and provider release notes, a deliberate "learn without the assistant" loop that keeps IC craft current.
- Run performance calibration that handles rapid technology drift fairly — the engineer who shipped against a model that got replaced mid-quarter is not graded against a counterfactual.
- Design the honest feedback cadence for frontier-tooling-era seniors (leaning on an agent without leading) vs. juniors (leveraging an agent responsibly to learn) — distinguish the shapes, apply the correct feedback and growth-plan shape to each.
- Position AI-engineering credentials, conference talks, publications, open-source contributions, and vendor certifications honestly in reviews — what is useful growth, what is credential theatre, and how to weight external signal against internal evidence.

## Chapters

1. [The Manager's Altitude for Performance Management in AI Engineering](01-the-managers-altitude-for-performance-in-ai-engineering.md) — what the manager owns (the four manager-owned artefacts) vs. what defers to the IC, to HR, to the staff-plus ICs, and to the general-management peer; what this module does not re-teach.
2. [What "Good" Looks Like At Each Level and Shape](02-what-good-looks-like-at-each-level-and-shape.md) — the published, versioned, behaviour-worded level rubric across L20 → L48; anchor per shape (applied, agentic, eval, RAG, fine-tune, senior agentic, staff-plus) read through three dimensions (scope, autonomy, influence) and five artefact classes; the "unmistakable artefact" per row.
3. [The 1:1, Feedback, and Review Cadence Specialised to AI Craft](03-the-1-1-feedback-and-review-cadence-specialised-to-ai-craft.md) — the AI-specific 1:1 agenda spine (eight-week rotation surfacing the five artefact classes); the SBI-shape feedback library; the review-packet template; the honest-cadence split between frontier-era seniors leaning on the assistant and juniors leveraging it responsibly.
4. [Growth Plans Across the Sub-Ladders and the IC ↔ Manager Pivot](04-growth-plans-across-sub-ladders-and-the-ic-to-manager-pivot.md) — the one-page growth-plan template; four named trajectories (level-up within shape, cross-shape rotation, tech-lead stretch, IC ↔ manager pivot); the two-manager contract for cross-team rotations; the Phase 0 honest trajectory walk-through for the manager-track pivot.
5. [Anti-Atrophy Against Frontier-Assistant Over-Reliance](05-anti-atrophy-against-frontier-assistant-over-reliance.md) — three loops the growth plan carries (deliberate-practice, reading, rotation); right-sizing per level; the manager's four responsibilities (protect time, read artefacts, share practice, model the pattern); the four warning signs.
6. [Fair Calibration Under Rapid Technology Drift](06-fair-calibration-under-rapid-technology-drift.md) — the counterfactual-at-planning-time principle; the shipped-against-replaced-model note; the model-generation-shift rubric re-read; five adaptations on the calibration meeting; four drift patterns to catch with specific corrections.
7. [Credential Honesty — Growth Signal vs. Theatre](07-credential-honesty-growth-signal-vs-theatre.md) — five categories of external signal (publication, OSS, certification, leaderboard, safety / evaluation contribution); three test questions applied uniformly; how to weight external artefacts at promotion; the three specific credential theatres to name.

## Exercises

- [Exercise 01 — What Good Looks Like At Each Level and Shape](exercises/exercise-01-what-good-looks-like-at-each-level.md) *(2 h)* — write the published level rubric for L20 → L48 across the six AI-engineering shapes.
- [Exercise 02 — Growth Plan Across Sub-Ladders](exercises/exercise-02-growth-plan-across-sub-ladders.md) *(2 h)* — author one complete growth plan for each of the four trajectories.
- [Exercise 03 — Anti-Atrophy Rotation Plan](exercises/exercise-03-anti-atrophy-rotation-plan.md) *(2 h)* — design the three loops for three ICs at different levels.
- [Exercise 04 — Fair Calibration Under Tech Drift](exercises/exercise-04-fair-calibration-under-tech-drift.md) *(2 h)* — produce a drift-fair review packet and run a mock calibration meeting.

Each exercise ships its prompt here; reference solutions live in the paired [`ai-engineering-manager-solutions`](https://github.com/ai-engineering-curriculum/ai-engineering-manager-solutions) repo.

## Supporting material

- `labs/` — long-form hands-on labs (planned).
- `quizzes/` — knowledge checks (planned).
- [`resources.md`](resources.md) — curated external references used across chapters.

## Where this module fits

- **Depends on `ai-augmented-team-manager` (level 40).** The general management canon (1:1s, feedback mechanics, the review cycle, promotion packets, calibration craft at base, PIP mechanics, exits) is this module's prerequisite, not its content. The 40-level sibling in the AI Workforce family is the right place to learn the base management craft; this module specialises it to the AI engineering surface.
- **Builds on mod-101 (role ladder and operating contract).** The six neighbouring roles the manager defers to (chapter 01 of mod-101) are the partners this module names for growth-plan sponsorship, promotion testimony, and cross-team rotation — notably the staff-plus ICs (`agentic-systems-architect`, `principal-ml-engineer`), the peer manager (`ai-infra-team-lead`), and the sibling-below manager (`ai-augmented-team-manager`).
- **Pairs with mod-102 (hiring, leveling, interview loops).** The level rubric this module writes (chapter 02) specialises the same public exemplars mod-102 chapter 04 uses for hiring calibration; the artefact classes are shared. The performance-side of calibration drift (mod-108 chapter 06) parallels the hiring-side (mod-102 chapter 04).
- **Pairs with mod-104 (eval-driven SDLC).** The artefact-class read depends on the registry, judges, and fixtures mod-104 builds; the growth plans assume the registry mod-104 produces.
- **Pairs with mod-105 (vendor and model portfolio).** The anti-atrophy rotation loop (chapter 05) exposes ICs to the model + framework landscape mod-105 teaches the manager to track; the drift-fair calibration (chapter 06) references the portfolio drift mod-105 chapter 06 names.
- **Pairs with mod-106 (safety and the release triad).** The release-triad incident-playbook pin (mod-106 chapter 05) is the operational surface the on-call-command artefact class reads on.
- **Pairs with mod-107 (AI-specific incident response and blameless postmortems).** The incident-command artefact class is grown on mod-107's practice; the blameless discipline is a cultural investment the review reads on (postmortem facilitation at L30, incident command at L40).
- **Pairs with mod-109 (team shape and operating cadence).** The on-call rotation, the deliberate-practice slot, the reading loop, and the rotation loop are beats on the operating cadence mod-109 designs; this module names the beats, mod-109 sequences them against the whole team's rhythm.
- **Pairs with mod-111 (cross-functional partnership).** Cross-team rotations and the IC ↔ manager pivot often cross functions; mod-111 covers the broader partnership shapes.
- **Pairs with mod-112 (executive communication).** The manager-to-exec read-out on team health, promotion velocity, and attrition (mod-112) reads off the review packets and calibration outputs this module produces.
