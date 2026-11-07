# Writing JDs from Artefacts, Not Years of Experience

A job description is a contract with two audiences at once: the candidate reading whether to apply, and the interview loop reading what to assess. If the JD is written as a years-of-experience list — "5+ years ML, 3+ years LLMs, experience with RAG" — both audiences fail. The candidate cannot tell whether their shape fits; the loop cannot tell what to measure. This chapter builds JDs from the **concrete artefacts** each role owns, keyed to the matrix from chapter 01 and the leveling anchor from chapter 04.

## Motivation

Two symmetric failures drive bad AI-engineering JDs:

- **Resume-bingo JDs.** Fifteen bullet points of tools and acronyms (LangChain, LangGraph, LlamaIndex, pgvector, Pinecone, Weaviate, Ragas, Promptfoo, SFT, LoRA, DPO, RLHF, ...) and a "5+ years AI experience" clause. Candidates with strong adjacent backgrounds self-reject. Candidates who have *used* the tools once self-select in. The loop then runs against an unwritten rubric and offers go out inconsistently.
- **Fluff JDs.** "We're looking for passionate AI engineers who want to work on cutting-edge problems and ship customer value fast." Nothing in there is measurable. The loop has no spec; the offer is a vibe check.

The AI-engineering-specific twist is that **years of experience is a nearly useless signal** for most of this work. Transformers-on-GPUs-in-production is three years old at commercial scale. The "10+ years of LLM experience" candidate does not exist and should not be asked for. What exists is evidence of **artefacts produced** — a shipped agent, a registered eval suite, an incident led, an eval fixture added, a prompt refactored, a judge designed. The JD that names those artefacts is the JD that attracts the right shape and tells the loop what to measure.

Relevant canon: GitLab's [public engineering handbook](https://handbook.gitlab.com/handbook/engineering/) treats role expectations as a living, linkable artefact; Dropbox's [career framework](https://dropbox.github.io/dbx-career-framework/) writes competency as behaviours by dimension; Google [re:Work](https://rework.withgoogle.com/en/guides) and [Project Oxygen](https://rework.withgoogle.com/en/guides/managers-identify-what-makes-a-great-manager) write manager and IC expectations in behavioural, not biographical, language. The JD pattern below inherits from all three.

## The five artefact classes

Every AI-engineering role, at every level in the matrix, is best described by its posture across five artefact classes. The classes are stable; the specifics change by shape and level.

### 1. Traces reviewed

How much raw model interaction does this role look at, in what detail, with what frequency? The number is a shape *and* level signal:

- **L20 — llm-application-developer:** reviews traces to debug their own feature; counts in dozens per week.
- **L30 — applied/agentic/eval/RAG/fine-tune:** reviews traces as part of every PR touching a prompt, retrieval, or judge; counts in hundreds per week; writes traces into bug reports and postmortems.
- **L40 — senior-agentic:** reviews traces at the taxonomy level — "what failure modes are we seeing this week," not "what did turn 3 return"; on-call escalations involve them.
- **L48 — architect / principal MLE:** samples traces to inform technical direction and eval methodology; does not run point on incident-level trace review.

A JD that names this number is a JD that the loop can test against.

### 2. Eval suites commissioned or owned

Evals are the measurement contract. What does the role do with them?

- **L20:** writes individual eval cases under guidance.
- **L30 applied / agentic / RAG / fine-tune:** adds eval cases on every feature PR; cannot get a PR merged without them.
- **L30 eval:** commissions new eval suites; owns the methodology defending judge choice; owns the eval-delta report at release.
- **L40:** leads the eval design conversation across the team; mentors L30s into commissioning.
- **L48:** sets methodology direction across teams.

### 3. Prompts (and judges, tools, corpora) registered

What first-class artefacts do they create that outlive their current feature?

- **L20:** uses the prompt and judge registry; does not yet add to it independently.
- **L30:** adds and versions prompts, judges, tools, retrieval configs, or fine-tune recipes in the registry; attaches a trace sample and an eval delta to the PR that registers them.
- **L40:** owns the **conventions** — what belongs in the registry, what versioning looks like, what the PR-review checklist is for the registry.
- **L48:** sets registry architecture across teams.

### 4. Incidents led

AI-specific incident surface (per mod-107): jailbreak, prompt injection, hallucination storm, post-vendor-upgrade regression, judge drift, tool-use failure, retrieval degradation, moderation false-positive storm.

- **L20:** participates in incident response; writes the trace timeline.
- **L30:** leads a response when the incident is in their area of ownership; contributes to the postmortem; adds the permanent eval fixture.
- **L40:** runs incident command across the team's AI surface; owns the kill-switch surface; chairs the postmortem.
- **L48:** sets incident methodology; is consulted on novel incident types.

### 5. Mentorship scope

Who do they grow, and how?

- **L20:** mentored by an L30+; no mentorship responsibility.
- **L30:** mentors L20s in their shape; pairs across shapes informally.
- **L40:** formally mentors multiple L20/L30s across shapes; is the go-to for "I'm stuck on this agent/eval/RAG problem."
- **L48:** mentorship scope reaches across teams and into the staff-plus pipeline.

These five classes are the spine of every JD in the rest of the chapter. Everything else — tools, frameworks, degrees, years — is at most context.

## JD template: five sections, one page

Every JD this chapter writes fits in a single page and has five sections. The template is identical across all seven IC shapes in chapter 01; the content specialises to shape and level.

```
# [Shape at Level] — [Team / Product]

## Who you are
  One paragraph. Shape and level in plain English, with the primary artefact
  from chapter 01 named explicitly.

## What you'll own in year one (artefacts, not responsibilities)
  Five bullets, one per artefact class — traces reviewed, eval suites,
  registered artefacts, incidents led, mentorship scope.

## What a strong first 90 days looks like
  Three bullets. Specific. What a candidate at this level would produce
  that would make us say "yes, this was the right hire."

## Who you work with
  One paragraph. Point at the handoff contracts (mod-101 chapter 03) so
  the candidate can see the role in the org, not floating in space.

## What we do NOT expect
  Three bullets. The years, degrees, and pedigree we are not requiring.
  This section is load-bearing for pipeline diversity (chapter 05).
```

The template deliberately has no "requirements" / "nice-to-have" split. That split invites the resume-bingo failure mode. It also has no years clause. Years can show up in the "who we do not require" section (see below).

## Worked example 1 — `applied-ai-engineer` (L30), product team

```
# Applied AI Engineer (L30) — Documents Product

## Who you are
  You ship end-to-end AI features inside a real product codebase, from
  product code through prompt, context, model, output, and back to product
  code. You defer to agent, eval, RAG, and fine-tune specialists on depth,
  but you can carry a feature across all four surfaces when it needs it.

## What you'll own in year one
  - **Traces reviewed:** on the order of hundreds per week, routinely attached
    to PRs and bug reports; you run a trace review as part of every prompt
    change.
  - **Eval suites:** you add eval cases to the suite on every feature PR; you
    do not commission new suites, you extend the ones the eval engineer owns.
  - **Prompts registered:** you register the prompts your features use, with
    versions, trace samples, and eval deltas attached to the PR.
  - **Incidents led:** you lead response when an incident is in your feature
    area; you contribute postmortems; you add the eval fixture that prevents
    recurrence.
  - **Mentorship scope:** you pair with L20 LLM application developers on
    the team; you are not formally mentoring yet.

## What a strong first 90 days looks like
  - Shipped one feature end-to-end to GA with an eval-delta report that
    cleared the release gate on first pass.
  - Added two permanent eval fixtures to the regression suite — one from a
    bug, one from a production trace surprise.
  - Carried one on-call shift with at least one incident you ran to root cause.

## Who you work with
  Reports into the AI Engineering Manager. Pairs daily with the agent,
  eval, and RAG specialists on the team. Consumes retrieval and tool
  contracts from them. Hands eval results and incident evidence to the
  release-gate chair and, by escalation, to governance peers.

## What we do NOT expect
  - A specific number of years in "AI" — the industry is too young for the
    number to carry signal. We care about the artefacts above.
  - A PhD or research background. Adjacent SWE backgrounds with real LLM
    production shipping are welcomed and routed the same way.
  - Expertise in every one of agent, eval, RAG, and fine-tune. Deep in one,
    competent in two more, aware of the fourth is plenty at L30.
```

That page is enough for a loop to measure against. A reader can see the shape; a reader can self-select in or out; the loop knows what to test.

## Worked example 2 — `ai-eval-engineer` (L30), platform team

```
# AI Eval Engineer (L30) — AI Platform Team

## Who you are
  You own measurement. You design golden datasets, you design LLM-as-judge
  prompts and rubric judges, you build and maintain the eval harness that
  runs in CI, and you defend the methodology that supports every release-gate
  decision this team makes.

## What you'll own in year one
  - **Traces reviewed:** sampled across the whole team's surface, not just
    your own features; you spot judge drift and label-corpus drift from the
    sample.
  - **Eval suites:** you commission new suites when the team ships a new
    capability; you own the judge-case corpus with change history; you
    publish the eval-delta report at every release gate.
  - **Prompts / judges registered:** every judge in the registry has your
    methodology note attached.
  - **Incidents led:** you lead response for judge-drift and eval-regression
    incidents; you co-lead with applied engineers when evals mispredict
    production.
  - **Mentorship scope:** you grow L20 and L30 engineers into eval
    discipline — you teach PR-level "where is your trace, where is your
    eval case" habits.

## What a strong first 90 days looks like
  - Commissioned one new eval suite with a defended judge methodology and
    it is running in CI.
  - Published an eval-delta report that changed a release-gate decision
    (promoted, held, or rolled back) — with the team crediting the report.
  - Refactored the judge-case corpus into a versioned registry; every
    production judge has a visible change history.

## Who you work with
  Reports into the AI Engineering Manager. Partners with applied, agentic,
  RAG, and fine-tuning engineers across the team. Hands release-gate
  evidence to the release-gate chair and to the governance peers
  (`ai-evaluation-engineer`, L35). Consumes red-team findings from the
  `ai-risk-engineer` (L25) and routes them back as eval fixtures.

## What we do NOT expect
  - Classical ML / statistics PhD. Strong statistical thinking is useful;
    credentials are not required.
  - Experience with every eval tool in the market. We use one or two; you
    will become expert in those. The methodology travels.
  - Prior "AI eval" title. This role is new enough that most strong hires
    come from applied engineering with a measurement bent, or from QA /
    test-infrastructure backgrounds with LLM exposure.
```

## Level differentiation: the same shape, L30 vs. L40

The JD template collapses cleanly across levels. A `senior-agentic-ai-engineer` (L40) JD built from the same template reads differently in the artefact bullets — not longer, just *different*. Compare:

| Artefact class | `agentic-ai-engineer` (L30) | `senior-agentic-ai-engineer` (L40) |
|---|---|---|
| Traces reviewed | Hundreds per week; on your own agent | Taxonomy-level reviews across the team's agent surface; escalates to on-call |
| Eval suites | Add cases to agent eval suite | Lead agent-eval design, mentor L30s commissioning new ones |
| Prompts / tools registered | Register new tools and agent prompts with trace + eval delta | Own registry **conventions** for the agent side |
| Incidents led | Lead agent incidents in your scope | Run incident command across the team's agent surface; own the kill-switch surface |
| Mentorship | Pair with L20s on the agent team | Formally mentor multiple L20s and L30s across agent + applied |

The same template produces two distinct, defensible JDs. The loop (chapter 03) runs against the artefacts; the calibration (chapter 04) reads the artefact posture to place a candidate at level.

## Three JD anti-patterns to delete on sight

Even after the template is in place, three phrases sneak back in and should be deleted every time:

1. **"5+ years of LLM / AI experience."** The field is too young and "experience with" does not measure shape. Replace with the artefact bullets. If an artefact bullet implicitly needs tenure to produce, that is fine — a candidate without any of those artefacts self-selects out without you needing a years clause.
2. **"PhD or equivalent."** Unless the role is specifically research-adjacent (which is likely out-of-scope for this track and routes to `principal-ml-engineer` or research), this filters without measuring. Delete.
3. **"Rockstar / 10x / passionate AI builder."** Deletes your pipeline-diversity options (chapter 05) and signals nothing. Delete.

The GitLab handbook culture is instructive here: when expectations are **public**, you cannot hide fluff clauses behind "that's just how we write JDs." You write them so an external candidate can tell whether they fit, and so your own loop can grade against them.

## The "what we do not expect" section earns its keep

New managers often cut the "what we do not expect" section to save space. It does not work and it should not be cut. In practice, this section:

- **Prevents self-rejection** from candidates with adjacent-but-strong backgrounds (chapter 05 pipeline move).
- **Tells the loop what is out of scope to assess.** If "PhD" is not expected, the loop does not test for research methodology.
- **Protects calibration.** If "10+ years" is not expected, calibrators do not accidentally use tenure as a level signal.

It is one of the most leverage-positive paragraphs in the entire JD.

## Summary

- Build every JD around five artefact classes: traces reviewed, eval suites commissioned/owned, prompts and judges registered, incidents led, mentorship scope.
- Use a single-page template with five sections: who you are, year-one artefacts, strong-90-days signals, who you work with, what we do not expect.
- Specialise the artefact bullets by shape (chapter 01) and by level (chapter 04). The template is stable; the bullets move.
- Delete "years of experience," "PhD required," and "rockstar" phrasing. None measure the thing you want to measure.
- Keep expectations **public** — Dropbox-framework-style dimensions, GitLab-handbook-style linkability. Secret internal levels fail the hiring loop before it starts.

The next chapter turns the JD's artefact bullets into the interview loop that measures them.
