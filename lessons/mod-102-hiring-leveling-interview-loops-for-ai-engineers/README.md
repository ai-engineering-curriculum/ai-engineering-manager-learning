# mod-102: Hiring, Leveling, and Interview Loops for AI Engineering Roles

> Second module of the AI Engineering Manager track. Builds the hiring operating system for an AI engineering team: the role matrix across applied / agentic / eval / RAG / fine-tune (plus the ML- and infra-adjacent peers), JDs built from concrete artefacts rather than years-of-experience fiction, a five-round AI-specific interview loop that pairs with general software-engineering rounds, calibration across the L20 → L48 ladder with a public-style rubric, and a four-source sourcing plan that prevents the single-source funnel and the superstar-trap hire.

**Estimated effort:** 20 hours · **Role level:** 45 (AI Engineering family)

## Learning objectives

By the end of this module you should be able to:

- Author a **role matrix** distinguishing `applied-ai-engineer`, `agentic-ai-engineer`, `ai-eval-engineer`, `rag-engineer`, `fine-tuning-engineer`, `llm-application-developer`, `senior-agentic-ai-engineer`, and the ML / infra peer roles — grounded in public leveling exemplars (Dropbox, Google, GitLab) and specialised to the AI-engineering families.
- Write **JDs from artefacts** — traces reviewed, eval suites commissioned, prompts and judges registered, incidents led, mentorship scope — rather than years-of-experience fiction.
- Design **interview loops** that assess AI-specific craft in addition to general software engineering: a prompt + trace + judge review round, a model-selection reasoning round, an eval-design exercise, a non-deterministic debugging round, and a system-design round for the LLM + retrieval + tool-use + eval stack. Pair them with the general-SWE loop; do not delete it.
- Run **calibration** across L20 → L40 of the IC ladder and up to the staff-plus altitude (`agentic-systems-architect`, `principal-ml-engineer`) using a public-style rubric — refusing a secret internal leveling language.
- Design a **sourcing and pipeline plan** for a new AI engineering team across four sources (adjacent SWE, ML-adjacent, research-to-production, agent/LLM community), and defend the operating rule that prevents the "AI-first-ever" superstar hire who refuses to work the eval and incident surface.

## Chapters

1. [The AI engineering role matrix](01-the-ai-engineering-role-matrix.md) — the seven IC shapes, their primary artefacts, and the ML/infra-adjacent peers.
2. [Writing JDs from artefacts, not years of experience](02-writing-jds-from-artefacts.md) — the five-artefact-class template and worked examples.
3. [Interview loops for AI engineering roles](03-interview-loops-for-ai-engineering.md) — five AI-specific rounds, written rubrics, and round-to-shape sequencing.
4. [Calibration across the AI engineering ladder](04-calibration-across-the-ladder.md) — per-candidate, rolling, and annual calibration against a published rubric.
5. [Sourcing, pipeline, and the superstar trap](05-sourcing-pipeline-and-the-superstar-trap.md) — four-source sourcing, artefact-based screening, and the operating rule that prevents the most expensive AI hiring mistake.

## Exercises

- [Exercise 01 — Role matrix across applied / agentic / eval / RAG / fine-tune](exercises/exercise-01-role-matrix-across-applied-agentic-eval-rag-fine-tune.md) *(3 h)*
- [Exercise 02 — JD for one concrete opening](exercises/exercise-02-jd-for-one-concrete-opening.md) *(2 h)*
- [Exercise 03 — Interview loop and rubric design](exercises/exercise-03-interview-loop-and-rubric-design.md) *(3 h)*
- [Exercise 04 — Run a mock calibration](exercises/exercise-04-run-a-mock-calibration.md) *(3 h)*
- [Exercise 05 — Sourcing and pipeline plan](exercises/exercise-05-sourcing-and-pipeline-plan.md) *(2 h)*

Each exercise ships its prompt here; reference solutions live in the paired [`ai-engineering-manager-solutions`](https://github.com/ai-engineering-curriculum/ai-engineering-manager-solutions) repo.

## Supporting material

- `labs/` — long-form hands-on labs (planned).
- `quizzes/` — knowledge checks (planned).
- [`resources.md`](resources.md) — curated external references used across chapters.

## Where this module fits

- **Builds on mod-101** — chapter 01's role matrix refines the IC-ladder-you-manage half of mod-101 chapter 03's handoff contracts. The operating-contract frame from mod-101 chapter 02 is what makes the AI-specific rounds in this module's chapter 03 defensible.
- **Feeds the performance and growth half** — the artefact bullets this module uses to hire are the same bullets mod-108 uses to grow and evaluate once the hire is on the team.
- **Feeds team shape work** — mod-109 (`team-shape-span-and-ai-operating-cadence`) treats the role-matrix mix as the first-order input to team shape. If the matrix is unclear, the cadence drifts.
- **Cross-cuts the eval and incident work** — chapter 03 rounds §1, §3, §4 all draw on mod-104 (eval-driven SDLC) and mod-107 (AI incident response); the hiring loop measures the operating system those modules describe.
- **Anchors project 101** — this module is the primary source for [`project-101-hire-and-level-an-ai-engineering-team`](../../projects/project-101-hire-and-level-an-ai-engineering-team).
