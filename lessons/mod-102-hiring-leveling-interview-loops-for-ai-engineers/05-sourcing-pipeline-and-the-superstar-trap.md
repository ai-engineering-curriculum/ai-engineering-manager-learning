# Sourcing, Pipeline, and the Superstar Trap

The best interview loop in the world grades the candidates it sees. If the top of the funnel is one source, the loop grades one slice of the market. This chapter builds a four-source pipeline for a new AI engineering team, names the screening pattern that keeps each source honest, and sets the operating rule that prevents the most common and most expensive hiring mistake in this market — the "AI-first-ever" superstar who refuses to work the eval and incident surface the role actually demands.

## Motivation

Three failure modes drive bad AI-engineering pipelines:

- **The single-source funnel.** All candidates arrive from one place — an elite ML lab network, or an alumni channel, or a single recruiter. The team converges on one shape, one worldview, and (over a year) one blind spot. Diversity-of-experience collapses; onboarding ramp lengthens; the team is fragile when the source dries up.
- **The "AI title" filter.** The ATS filter is "must have `AI Engineer` on current résumé." Adjacent SWEs with excellent production fluency are rejected at keyword stage; research-to-production converts are rejected because their title says Research Scientist. The pipeline narrows to candidates who have had the title longest, which — in a field three years old at commercial scale — is a very thin slice.
- **The superstar hire.** Someone with a strong public profile in AI arrives, is hired at a staff-plus level over the heads of the existing team, and refuses on principle to review traces, write judges, lead postmortems, or own an eval delta. They treat the AI-engineering operating system as beneath them. The eval surface rots; the incident surface becomes the problem of whoever is not them; the team stops being able to defend its release gate.

All three are preventable. The pipeline design below prevents the first two; the operating rule in the second half prevents the third.

## The four sources

A healthy AI engineering pipeline for a new team draws from at least four distinct sources. Any one source should produce no more than roughly half of hires in the team's first year. The sources, in rough order of volume for most teams:

### 1. Adjacent software engineering — the main artery

Engineers from strong backend / full-stack / platform / data-engineering backgrounds who have been shipping LLM features for the last 12–24 months, with or without the formal "AI Engineer" title. This is the deepest source in the market. The pipeline moves:

- **Internal mobility.** Engineers in your own org who have been doing AI-shaped side-of-desk work. Internal mobility candidates arrive with context (codebase, incident surface, team dynamics) that cuts ramp by months. Treat internal mobility as source number one before external posting.
- **Open-pipeline sourcing.** External engineers at companies known to be shipping LLM product. The signal to look for in a résumé is not the job title; it is specific artefacts: prompts, judges, retrieval work, tool-use work, incidents led, eval suites authored.
- **Community visibility.** Blog posts, conference talks, open-source contributions to eval / RAG / agent frameworks. The artefact-based JD (chapter 02) tells the sourcer what to look for; the loop (chapter 03) grades it honestly.

Screen posture for this source: artefacts over title. Lean on §1 (prompt + trace + judge) and §4 (non-deterministic debugging) early to filter out the "I worked at an AI company for a year but my scope was backend infra" résumé.

### 2. ML-adjacent — classical MLOps and MLE backgrounds

Engineers from classical ML-engineering roles (recommender systems, fraud models, forecasting, computer vision, speech) who have made or are making the conversion to LLM-first work. These candidates bring the measurement muscle most AI-engineering teams underweight: production ML experience, offline eval discipline, A/B testing fluency, and a native respect for the measurement-is-the-system mindset.

- **The conversion check.** Can they talk about LLM-specific evaluation (LLM-as-judge, human-review workflows, trace review) and not just "accuracy on a held-out set"? If yes, they are a strong eval or applied hire. If no, they are a classical MLE and the hire should route to the ML Engineering ladder, not this one.
- **Common miscast.** Hiring a classical MLE into an applied-AI role with no conversion plan. Six months in, they are still looking for the training loop that does not exist.

Screen posture: lean on §3 (eval design) to surface the measurement muscle, and on §2 (model selection) to see whether they have absorbed the vendor-and-portfolio reality of modern AI engineering.

### 3. Research-to-production converts

Candidates from research-leaning backgrounds — academic labs, applied research groups, early-career research scientists — who want to leave research and build product. This source is small and high-variance but produces some of the strongest hires when the match is right. The screen that matters most:

- **Can they ship in a real codebase under a real release gate?** Not "have they published?" but "have they ever operated on a real production rotation, closed a bug under time pressure, or defended an eval delta to a release-gate chair?"
- **Are they comfortable owning the eval and incident surface,** or do they treat it as "production work" they expect someone else to do? This is the superstar-trap question in miniature; see the next section.

Screen posture: §1 (prompt + trace + judge) and §4 (non-deterministic debugging) heavily, in a *real-codebase* flavour. The system-design round (§5) is useful for level placement but is not where this source succeeds or fails.

### 4. The agent / LLM community

A real pipeline has emerged in the last 24 months around open-source agent frameworks, eval tooling, LLM application patterns, and open-weights model work. Signals from this source:

- Contributors to open-source agent frameworks (LangChain, LangGraph, LlamaIndex, Instructor, and the equivalents on the eval side — Ragas, DeepEval, promptfoo, Inspect).
- Authors of LLM-pattern writing that your team reads (the [Chip Huyen](https://www.oreilly.com/library/view/ai-engineering/9781098166298/) / [Eugene Yan](https://eugeneyan.com/writing/llm-patterns/) style — specific, measured, incident-aware).
- Active contributors in the public LLM community who do more than social-media commentary — they ship code, review others' code, and demonstrate judgment.

Screen posture: lean on §5 (system design) and §3 (eval design) because this source often has strong vocabulary but under-tested production muscle; §1 and §4 keep the loop honest.

### No fifth source — but a cross-cutting pattern

The four above are the volume sources. Two cross-cutting moves apply across all of them:

- **Referrals from your existing team.** Referrals are a *distribution channel* that cross-cuts the sources above, not a fifth source. A great referral pipeline accelerates all four sources; a referral-only pipeline concentrates the team's blind spots. Keep referrals as a channel, not as a worldview.
- **Boomerangs.** Former employees returning. Usually faster to productive. Grade on the same loop; do not level up for pedigree.

## A four-week sourcing plan for a new team

A new AI engineering team with four open roles in the first quarter follows a four-week pattern to stand up the pipeline.

### Week 1 — define the shapes and publish

Using chapters 01, 02, and 04:

- Place each opening on the role matrix (chapter 01).
- Write the four JDs from the artefact template (chapter 02). Publish them with the "what we do not expect" section intact.
- Draft the loop rubric for each shape (chapter 03). Publish the rubric shape to candidates.
- Brief recruiting on the matrix, the JDs, and the sourcing sources above. The anti-pattern to prevent in week 1 is a recruiter running keyword search against "AI Engineer" résumés.

### Week 2 — open the four sources

- **Adjacent SWE:** post internally and externally; brief recruiters on artefact-based sourcing.
- **ML-adjacent:** target lists of specific companies / teams with classical-ML pedigree and known conversion to LLM; reach out directly through referrals and engineering networks.
- **Research-to-production converts:** reach into academic / applied-research networks, specifically for candidates who have *already* been looking to leave research; do not try to convert researchers who are not pre-disposed.
- **Agent / LLM community:** identify three to five public contributors to tools your team uses, and reach out with a shape-and-level-specific pitch.

### Week 3 — run the loops

- Start loops on arriving candidates. Debrief immediately after every loop (chapter 04).
- Keep the per-shape loop consistent; variation in loops at this stage is the single largest driver of leveling drift across the pipeline.

### Week 4 — first rolling calibration

- Pull the loops run so far and run a first rolling calibration (chapter 04).
- If one source is over-represented in offers — more than roughly half — adjust the top of the funnel in the next week. Rebalance.
- If one source is under-represented despite effort, say so in writing; it might be a targeting problem, a brand problem, or a loop problem. Diagnose before next quarter.

## Screening without filtering out the pipeline you want

The ATS filter and the recruiter screen are where most pipelines silently narrow to one source. Three rules keep the top of the funnel open:

- **Filter on artefacts, not titles.** A résumé that lists "Backend Engineer at [company], 2023–present" with bullets about shipping a RAG system, writing judges, and leading an eval-regression incident is a stronger L30 candidate than a résumé titled "AI Engineer" with vague bullets.
- **Read the "what we do not expect" clause in the JD at every screen.** Recruiters routinely re-filter in the "years of experience" and "PhD required" clauses that chapter 02 deleted. The screen briefing has to make this explicit, and every rejected résumé should be spot-checked against the artefact bar.
- **Keep a visible rejected-at-screen pile.** Once a week, scan the pile and ask whether the rejections match the artefact bar or the title bar. If the latter, the screen is drifting; course-correct in the next recruiter sync.

## The superstar trap

The most expensive single hiring mistake an AI Engineering Manager can make is hiring one "AI-first-ever" superstar at a staff-plus level, over the heads of the team, who refuses to work the eval and incident surface the role actually demands.

### What the trap looks like

A candidate arrives with a strong public profile — perhaps a research pedigree, perhaps a prominent position at a well-known AI company, perhaps a popular blog or Twitter presence. The pressure to close them runs hot: the business wants an "AI heavyweight," the CAIO wants a splashy hire, recruiting wants the win. The loop sees the gaps — the §3 eval-design round goes badly, the §4 non-deterministic-debugging round is skipped because "everyone knows they know AI" — but the offer goes out anyway.

In the first quarter, three things happen in sequence:

1. The superstar declines to review traces. "I don't look at individual outputs; I look at systems."
2. The superstar declines to write judges. "That's a QA problem."
3. The superstar declines on-call. "I'm at architect altitude. I'm not on an on-call rotation."

The rest of the team internalises the message. The eval surface rots. The incident surface becomes the problem of whichever senior engineer is not the superstar. Within two quarters either the superstar leaves (having not shipped much) or a senior engineer leaves (having absorbed all the floor work). The team's measurement-and-incident muscle has not grown; the team's shape is worse than before the hire.

### Why the trap is specifically AI-shaped

In classical backend engineering the trap is less common because the operating system is older. A staff-plus backend engineer who refused to debug a production issue would be called out at the senior-IC altitude within a week. In AI engineering the operating system — eval-gated SDLC, trace-level review, AI-specific incident response, kill-switch discipline — is new, and the field has rewarded public visibility over operational muscle for most of the last three years. A superstar can arrive with real prominence and *no* operational experience, and a hiring manager under pressure has few public artefacts to point at to defend the operational bar.

### The operating rule

The rule is simple and absolute:

> Every AI engineering hire, at every level including staff-plus, works the eval and incident surface as part of the job.

In practice:

- **Every level in the ladder has an incidents-led artefact.** From L20 (participated in response) to L48 (sets incident methodology, participates in response when the incident type is novel). No exceptions for pedigree.
- **Every level in the ladder has an eval-fixture artefact.** Every confirmed incident becomes a permanent eval fixture; every level participates in that production. The architect does not get out of it because they are the architect.
- **Loops never skip §3 or §4 for a well-known candidate.** The rounds that most often get skipped under close-the-superstar pressure are eval design and non-deterministic debugging. The hiring manager's job is to refuse the skip. "We'll waive the eval round for them" is the sentence that lands you in the trap.
- **The JD's "what you'll own" section includes the eval and incident bullets at staff-plus altitude.** If a staff-plus candidate declines those bullets, the role is not a fit, regardless of brand.

### The sanity check

Before extending any staff-plus offer, ask three questions:

1. Has this candidate ever led an incident to postmortem, not just designed a system that had one?
2. Has this candidate ever written or defended a judge?
3. Will this candidate carry an on-call rotation, scoped to their altitude?

If the answer to any of the three is "no," and you cannot change it to "yes" in the offer conversation, do not extend. The cost of a staff-plus hire who refuses the operating system is higher than the cost of a slower hire.

## Diversity of experience and the four-source discipline

The four sources are a diversity-of-experience strategy by construction. A team sourced entirely from research produces a team that defers eval to future-us; a team sourced entirely from backend produces a team that under-weights measurement; a team sourced entirely from the LLM community produces a team that chases frameworks over production reality. Mixing the sources produces a team that catches each other's blind spots.

This is specifically **diversity of experience**; it is not a substitute for broader diversity work (demographic, geographic, non-traditional pedigree, career-returner, etc.) which the general EM canon covers and which this track assumes. Both matter. The four-source sourcing plan does not replace the broader diversity commitment; it attaches to it.

## A concrete read: the first eight hires

Eight hires over the first year on a new AI engineering team, distributed across the four sources and the role matrix:

| # | Shape / level | Source |
|---|---|---|
| 1 | `applied-ai-engineer` (L30) | adjacent SWE, internal mobility |
| 2 | `ai-eval-engineer` (L30) | ML-adjacent, classical MLE conversion |
| 3 | `rag-engineer` (L30) | adjacent SWE, external |
| 4 | `agentic-ai-engineer` (L30) | agent/LLM community |
| 5 | `applied-ai-engineer` (L30) | adjacent SWE, referral |
| 6 | `senior-agentic-ai-engineer` (L40) | adjacent SWE, external |
| 7 | `fine-tuning-engineer` (L30) | research-to-production convert |
| 8 | `ai-eval-engineer` (L30) | adjacent SWE, external |

No one source dominates. The L40 tech-lead is not the staff-plus hire; staff-plus comes after the team's operating system is bedded in, and only if the architect / principal MLE role is genuinely load-bearing against the roadmap. The research-to-production convert is placed into a fine-tune role where their research muscle is leveraged *and* their conversion to production-shaped work is explicit.

## Summary

- A healthy AI engineering pipeline draws from four sources: adjacent SWE, ML-adjacent, research-to-production, agent/LLM community. No source should produce more than roughly half of hires in the team's first year.
- Referrals are a distribution channel across sources, not a fifth source.
- Screening filters on artefacts, not titles. The "what we do not expect" section of the JD is defended at every screen.
- The four-week sourcing plan: define shapes, open sources, run loops, first rolling calibration. Rebalance on week 4.
- The superstar trap — hiring a prominent AI candidate who refuses to work the eval and incident surface — is the most expensive mistake in this market. The operating rule is that every hire at every level works that surface, and the three sanity-check questions (led an incident, written a judge, will carry on-call) are asked before any staff-plus offer.
- Diversity of experience across sources catches blind spots; broader diversity commitments remain, and this plan attaches to them rather than replacing them.

This closes the module's five chapters. The exercises walk you through producing the five artefacts the module teaches: the role matrix, a JD for one concrete opening, an interview loop and rubric, a mock calibration, and a sourcing and pipeline plan.
