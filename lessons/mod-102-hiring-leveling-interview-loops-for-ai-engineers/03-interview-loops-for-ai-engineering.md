# Interview Loops for AI Engineering Roles

An AI-engineering interview loop is a **general software-engineering loop plus AI-specific rounds**. If you delete the general software-engineering rounds you hire people who cannot read a stack trace at 2 a.m.; if you delete the AI-specific rounds you hire people whose features ship a 70-percent eval score and nobody notices. This chapter specifies five AI-specific rounds that pair with the general-SWE loop, their rubrics, anti-patterns, and how to sequence them against the role matrix from chapter 01.

## Motivation

Three failure modes drive bad AI-engineering loops:

- **The "LLM leetcode" loop.** Every round is "implement a transformer attention block on a whiteboard" or "derive the DPO loss." You test research-shaped signal and hire for an applied role; the candidate ships nothing in their first quarter because the job is not research.
- **The "chat with the model" loop.** Every round is a conversation about prompts, models, and papers. You test enthusiasm and hire against it; the candidate discovers in week three that the job involves a real codebase, a real on-call rotation, and real evals, and they are miscast.
- **The copy-pasted SWE loop.** The team adds one round called "AI round" where someone riffs on model choice for 45 minutes with no rubric. The loop grades inconsistently; the AI-specific signal is drowned by the general-SWE signal and offers go out with no defence of the AI-specific bar.

The fix is to design the AI-specific rounds as **first-class signal with written rubrics**, sequence them around the general-SWE loop, and keep both sides honest.

## The loop at a glance

The default loop for an AI-engineering IC at L30 — adapted up or down for level — is six to seven interviews across two rounds (phone-screen plus onsite / virtual onsite). Three to four are general-SWE; three are AI-specific.

| # | Round | Shape | Length | Who runs it |
|---|---|---|---|---|
| 1 | Phone screen (SWE) | General SWE, scoped | 45 min | A senior engineer on the team |
| 2 | AI-specific phone screen | Prompt + trace + judge review (§1) | 60 min | AI eval engineer or senior-agentic |
| 3 | Coding (general SWE) | Pairing on a non-AI problem | 60 min | Any senior engineer |
| 4 | Model-selection reasoning | §2 | 45 min | Senior IC or hiring manager |
| 5 | Eval-design exercise | §3 | 60 min | AI eval engineer |
| 6 | Non-deterministic debugging | §4 | 60 min | Senior IC |
| 7 | LLM + retrieval + tool-use system design | §5 | 60 min | Senior-agentic or architect |
| 8 | Hiring-manager + closing | Behavioural, values, context | 45 min | Hiring manager |

Not every role needs every round. §6 (sequencing) at the end names which rounds attach to which shape.

The general-SWE rounds (rows 1, 3) are **assumed**, not re-taught here. This track defers their design to the general EM canon — the fundamentals of running a hiring loop live in [`ai-augmented-team-manager`](https://github.com/ai-workforce-curriculum/ai-augmented-team-manager-learning) and in Fournier / Larson / Google re:Work. The rest of this chapter specifies the five AI-specific rounds in detail.

## §1 — Prompt + trace + judge review

**The question the round answers:** can the candidate look at a real model interaction and tell you *what is wrong and how to fix it*, in the vocabulary of prompts, traces, and judges that the team ships in?

### Setup

- A redacted trace from a production feature (or a near-equivalent synthetic trace built from your own product) showing: a system prompt, the user turn, the tool-call history if any, the model's final response, a judge's verdict, and the ground-truth answer.
- Two or three variants of the same trace — one where the prompt is the problem, one where the judge is the problem, one where the retrieval is the problem — mixed in random order.

### Candidate task (60 minutes)

1. Read the trace. Say what the feature is trying to do.
2. Identify what went wrong, where, and why.
3. Propose a fix: a prompt change, a judge change, a retrieval change, a model change, or "this is the model's ceiling, change scope."
4. Say what eval case you would add so this specific failure cannot recur.

### Rubric

- **L20:** can find the model's response is wrong; may not locate the cause; relies on the interviewer to walk through the fix.
- **L30:** finds the cause in the trace; proposes a specific fix; writes down the eval case that would prevent recurrence.
- **L40:** finds multiple possible causes; reasons about which is load-bearing; proposes a fix at the right level (prompt vs. judge vs. retrieval vs. scope); adds **two** eval cases — the specific one and the generalisation.
- **L48:** moves to methodology — "this failure shape tells me our judge is misspecified; here is how I would redesign it," and connects to the broader eval taxonomy.

### Anti-patterns

- **Running this on whiteboard with no real trace.** The whole point is that the signal is *in the trace*. If the candidate cannot see the raw interaction, you are testing their fluency talking about traces, not reviewing them.
- **Scoring on tool-name vocabulary.** "They didn't mention LangSmith / Langfuse / Phoenix" is not a signal. The vocabulary is specific to whatever your team uses. Grade the review, not the brand.
- **Rewarding only pessimism.** "This whole feature should be rebuilt" is not a passing answer. Grade on the proposed *next move* being the right size.

## §2 — Model-selection reasoning

**The question the round answers:** can the candidate reason about which model and provider to use for a given feature, under real-world constraints (cost, latency, privacy, residency, deprecation risk), and defend the call?

### Setup

A one-page product brief: "We need to build feature X. It has these constraints: [latency, cost ceiling, privacy posture, residency, SLA floor, expected QPS, context-window demand]. Here are the current provider options as of today [a short list with real capabilities]. Which model do you propose, and why?"

The brief should *not* pre-pick the answer. Good briefs have at least two defensible answers at tension.

### Candidate task (45 minutes)

1. Clarify the brief (good candidates ask about traffic, failure cost, fallback expectations, SLA, DPA, data-training-opt-out posture).
2. Propose a primary model and a fallback model.
3. Walk the trade-offs: input/output pricing, cached pricing, batch pricing, context window, latency p95, deprecation risk, DPA / residency posture, vendor-lock-in exposure.
4. State a policy for when to re-evaluate (e.g., "re-evaluate every quarter; immediately on provider deprecation notice").

### Rubric

- **L20:** picks a model by name recognition; shallow on trade-offs.
- **L30:** picks a model and a fallback; names the three or four constraints that actually drive the call; references at least one concrete trade-off (price/perf, context window, deprecation).
- **L40:** adds **routing** and **multi-model** reasoning; names the DPA / data-training-opt-out vocabulary without being prompted; proposes a measurement plan to validate the choice in production.
- **L48:** moves to portfolio-level reasoning — "how does this fit the team's model portfolio, what are we locked into after this call, what does the two-year posture look like."

### Anti-patterns

- **Grading by whether the candidate picked your current model.** They are being hired to **reason**, not to psychic-read your production stack.
- **Rewarding "frontier labs always win."** Open-weights, smaller models, and specialists have real trade-offs that the right candidate names. Grade the reasoning.
- **Not naming the DPA vocabulary at any level.** For L30+, "does the data get used to train the provider's model, and what's our DPA position" must come up — either the candidate raises it, or the interviewer does and sees whether the candidate can engage.

Primary references the round assumes exist in the reader's world: public DPA exemplars like the [Anthropic DPA](https://www.anthropic.com/legal/dpa) (and equivalent OpenAI / Google / Mistral public versions), [Anthropic RSP](https://www.anthropic.com/rsp), [OpenAI Preparedness Framework](https://openai.com/safety/preparedness/), [Google DeepMind Frontier Safety Framework](https://deepmind.google/discover/blog/introducing-the-frontier-safety-framework/). Candidates do not need to quote them; they need to know they exist.

## §3 — Eval-design exercise

**The question the round answers:** can the candidate design an evaluation for a non-trivial LLM feature, defend the methodology, and spot the mistakes the design would otherwise make?

### Setup

A product brief for a feature that is genuinely hard to evaluate — e.g., an agent that summarises customer meeting notes, a RAG system over a legal corpus, a tool-using agent that files helpdesk tickets. The brief should include: what "good" looks like roughly, what a wrong answer *costs*, and what data is available.

### Candidate task (60 minutes)

1. Propose a **measurement contract**: what the eval is measuring, what "passing" means, and what passing *does not* mean.
2. Design a **golden dataset**: how large, how it is sourced, how it is refreshed, what biases it may have.
3. Design the **judges**: LLM-as-judge vs. rubric judges vs. human review; where each is used; how judge quality is itself measured.
4. Describe **where in the pipeline the eval runs** (PR gate, canary, GA, production monitoring) and what the delta thresholds are.
5. Name the **failure modes of the eval itself**: judge drift, dataset staleness, over-fitting to the golden set, metric-gaming by the prompt.

### Rubric

- **L20:** writes individual eval cases; cannot defend a methodology.
- **L30:** designs a passable suite with a golden set and a judge; names at least two failure modes of the eval itself.
- **L30 eval specialist:** designs a defensible suite; has a crisp position on LLM-as-judge vs. rubric vs. human; proposes a judge-quality measurement; references continuous eval.
- **L40:** moves to **governance** — audit trail, release-assurance evidence, how the eval evidence lands with governance peers (`ai-evaluation-engineer`, L35); references NIST AI RMF GenAI Profile or OWASP Top 10 for LLM Applications vocabulary.
- **L48:** sets methodology direction — how this eval fits the team's eval architecture, what the reusable primitives are.

### Anti-patterns

- **Grading by tool familiarity.** Promptfoo, Ragas, DeepEval, LangSmith, Langfuse, Braintrust, Arize Phoenix, OpenAI Evals, Inspect are all reasonable; grade on **methodology**, not whether they picked the tool your team happens to use.
- **Letting LLM-as-judge vs. rubric vs. human become a dogma question.** The right answer is almost always a mix. Grade on the candidate's position on *when each is used*.
- **Skipping the "failure modes of the eval itself" step.** Candidates who cannot name how their own eval can lie are not ready to own measurement at L30 eval specialist.

Primary references the round assumes: [NIST AI RMF](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf) + [GenAI Profile](https://airc.nist.gov/AI_RMF_Knowledge_Base/AI_RMF/Uses/GenAI-Profile), [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/), Eugene Yan's [Patterns for Building LLM-based Systems](https://eugeneyan.com/writing/llm-patterns/).

## §4 — Non-deterministic debugging

**The question the round answers:** can the candidate debug a failure whose cause is a probability distribution, not a value — and produce an action plan that *converges*?

### Setup

A scenario, delivered verbally with supporting artefacts (traces, eval reports, a dashboard screenshot):

> "A production feature was shipping at 91% eval pass rate. Over the last two weeks it drifted to 78%. Nothing was deployed — no prompt changes, no judge changes, no code changes, no retrieval changes. Walk me through how you figure out what happened."

Deliberately ambiguous. Possible causes include: vendor model silently updated under a pinned name, embedding store drift, corpus added new content that changed retrieval distribution, judge-corpus staleness, traffic shift exposing a long-tail failure mode, moderation classifier update at the provider, or some combination.

### Candidate task (60 minutes)

1. Ask clarifying questions.
2. Propose an investigation plan ordered by **evidence yield**.
3. Walk through execution: what to measure first, what hypothesis each measurement rules in/out.
4. Decide a **mitigation** path (route to fallback, prompt patch, judge update, kill-switch) *before* the root-cause investigation completes.
5. Name what **eval fixture** gets added as the permanent artefact of this incident.

### Rubric

- **L20:** picks one hypothesis and runs it to the ground; does not triage across hypotheses.
- **L30:** triages across two or three hypotheses; sequences by evidence yield; separates **mitigation from root cause** (you don't wait for the RCA to stop the bleeding).
- **L40:** adds **incident command** moves — who is pulled in, who owns kill-switch, what the stakeholder update looks like; references AI-specific incident types (post-vendor-upgrade regression, judge drift, retrieval degradation, moderation-classifier change) without being prompted; references the blameless postmortem pattern.
- **L48:** moves to **detection** — "this failure should have paged two weeks earlier; here is the observability gap."

### Anti-patterns

- **Grading on "they got the right answer."** The scenario has multiple plausible causes. Grade the *reasoning*, the *sequencing*, and the *separation of mitigation from root cause*.
- **Letting candidates skip the mitigation step.** A candidate who says "we need a week to investigate before we do anything" at L30+ is miscasting the role.
- **Not testing the eval-fixture step.** The permanent artefact of every AI incident is the eval fixture that catches recurrence. Candidates who do not propose one are missing a load-bearing mod-107 concept.

Primary references: [Google SRE Book](https://sre.google/sre-book/table-of-contents/) and [SRE Workbook](https://sre.google/workbook/table-of-contents/), [Etsy blameless-postmortems essay](https://www.etsy.com/codeascraft/blameless-postmortems/), the public [AI Incident Database](https://incidentdatabase.ai/).

## §5 — LLM + retrieval + tool-use system design

**The question the round answers:** can the candidate design a system that handles the real LLM-era stack — model(s), retrieval, tools, state, evals — at the right level of ambition for their level?

### Setup

A system-design prompt that cannot be solved by one model call. A good prompt forces the candidate to make calls on:

- **Model(s)** — which, with fallback, with cost/latency reasoning.
- **Retrieval** — corpus contract, chunking strategy, embeddings, re-ranker, retrieval evals.
- **Tools** — what tools, what the contract looks like, what the authorisation surface is, what happens on failure.
- **State** — what the agent remembers across turns, what it does not, how state is persisted.
- **Evals** — what measures this system's health, where it runs, what gates what.
- **Incidents** — what the kill-switch surface looks like, what the top three incident types are.
- **Cost and capacity** — rough back-of-envelope on cost per interaction and throughput.

Example prompt: "Design a customer-support agent that can read a user's recent orders, look up warranty policy in a documentation corpus, and open a return ticket on their behalf. The company has 10M customers, averages 200k support contacts a week, and a typical interaction has 2-5 user turns. Design the system."

### Candidate task (60 minutes)

1. Clarify the prompt (good candidates ask about failure cost, fraud posture, latency target, regulatory posture).
2. Sketch the system at a block-diagram level.
3. Walk each layer — model, retrieval, tools, state, evals, incidents — with the trade-offs each call carries.
4. Name the hardest two or three problems and how they would be addressed.
5. Describe the **release readiness** — eval-gated SDLC posture, release triad evidence, incident runbook outline.

### Rubric

- **L30:** draws a reasonable block diagram; makes defensible calls at each layer; names trade-offs when asked; has a shaky but present position on evals and incidents.
- **L40 senior-agentic:** drives the trade-offs without prompting; crisp on tool contracts, HITL surface, kill-switch; references the release triad and the eval-gated SDLC fluently; proposes a specific observability posture.
- **L48 architect:** moves to cross-team reasoning — "this needs a platform primitive we should build once," "this is going to fight the agent platform's assumptions, here is the conversation to have with that team"; references technical direction and standardisation.

### Anti-patterns

- **Letting the round collapse to a prompt-engineering conversation.** The candidate must address retrieval, tools, state, evals, and incidents, not just the prompt.
- **Grading on tool-name coverage.** "They didn't say LangGraph" is not a signal. Grade the design.
- **Not asking about cost and incidents.** For a production-scale brief, candidates who do not reach cost and incident reasoning on their own are missing the production muscle.

Primary references the round leans on: Chip Huyen's [*AI Engineering*](https://www.oreilly.com/library/view/ai-engineering/9781098166298/) and [*Designing Machine Learning Systems*](https://www.oreilly.com/library/view/designing-machine-learning/9781098107956/), Eugene Yan's [LLM patterns essay](https://eugeneyan.com/writing/llm-patterns/), the [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/), and provider primary docs ([Anthropic build-with-Claude](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview), [OpenAI Cookbook](https://cookbook.openai.com/)).

## §6 — Which rounds for which shape

Not every role needs all five AI-specific rounds. A reasonable default mapping, keyed to the matrix in chapter 01:

| Shape / level | §1 Prompt+trace+judge | §2 Model selection | §3 Eval design | §4 Non-det debugging | §5 System design |
|---|---|---|---|---|---|
| `llm-application-developer` (L20) | yes, light | optional | optional | optional, short | optional, short |
| `applied-ai-engineer` (L30) | yes | yes | yes, light | yes | yes |
| `agentic-ai-engineer` (L30) | yes | yes | yes, light | yes | yes, agent-weighted |
| `senior-agentic-ai-engineer` (L40) | yes | yes | yes | yes | yes, with architect |
| `ai-eval-engineer` (L30) | yes, judge-weighted | yes, light | yes, deep | yes | yes, eval-weighted |
| `rag-engineer` (L30) | yes, retrieval-weighted | yes | yes, retrieval-weighted | yes | yes, retrieval-weighted |
| `fine-tuning-engineer` (L30) | yes | yes, fine-tune-weighted | yes, deep | yes | yes, training-weighted |
| staff-plus (L48) | sampled | portfolio-level | methodology-level | yes, cross-team | yes, cross-team |

"Light" means 30 minutes rather than 60 and a narrower prompt. "Weighted" means the round shifts emphasis toward that shape's specialty.

Pair the AI-specific rounds with the general-SWE loop sized to level — typically a phone screen plus one onsite coding pairing, plus the behavioural / closing round. The general-SWE loop never disappears; the AI-specific rounds add on top.

## Rubrics are written, read aloud, and public

Three operating rules that keep the loop honest:

1. **Write every rubric down.** A round whose rubric exists only in the interviewer's head grades inconsistently and biases offers. Publish the rubric to the loop; if a round's rubric changes, publish the change.
2. **Read the rubric aloud in debrief.** When the debrief starts, the round owner says what the rubric was before saying what the candidate did. This forces grading against the spec, not against vibe.
3. **Make the rubric visible to candidates, at least in shape.** Candidates deserve to know what each round is testing. You do not share the exact prompt, but you share the round's purpose and the shape of the signal. This is the GitLab-handbook move adapted to interviewing.

## Calibration starts in the loop

The debrief after every onsite is **calibration in miniature**: for each round, the interviewer names the behaviours they saw and places them at level against the published rubric. The hiring manager's job in debrief is to ask "what evidence is that?" every time someone says a level — "L30" is not a fact, it is a claim about artefacts and behaviours. Chapter 04 formalises this across the pipeline; the habit starts here.

## Summary

- An AI-engineering loop is a general-SWE loop **plus** five AI-specific rounds: prompt + trace + judge review, model-selection reasoning, eval-design exercise, non-deterministic debugging, LLM + retrieval + tool-use system design.
- Each round has a written rubric with L20 → L48 behaviours. Rubrics are published, read aloud in debrief, and visible in shape to candidates.
- The general-SWE rounds are assumed and defer to the general-EM canon. They never get deleted to make room.
- Round selection is keyed to the shape in chapter 01: every shape gets §1 and §4; eval specialists lean on §3; agentic specialists lean on §5; model-selection (§2) applies to L30 and up.
- The debrief is the first act of calibration. The hiring-manager question "what evidence is that?" is the only question that keeps the rubric honest.

The next chapter takes the evidence the loop produces and runs calibration — across the L20 → L40 IC ladder and the staff-plus altitude — with a public-style rubric and without a secret internal language.
