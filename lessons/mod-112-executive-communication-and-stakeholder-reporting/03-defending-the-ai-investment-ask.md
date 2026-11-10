# Defending the AI Investment Ask

Every quarter the AI engineering manager asks for something. Two more model-licenced seats for the team's fine-tuning pipeline. A doubled labelling-data budget. A new headcount — maybe a `rag-engineer` to cover a shape the team does not currently have. A tooling line for a prompt-registry or trace-evaluation platform. The ask lands in a budget cycle alongside every other engineering function's asks, and the CFO, the VP of engineering, and the CAIO read it against a limited envelope.

The manager's job is to make the ask readable, evidenced, and *honestly* evidenced. This chapter is about the last word. The industry has absorbed, over three years, a fashion of citing "AI productivity gains" in investment slides without reading the underlying research — a fashion that is already backfiring on teams whose asks are read against the fashion rather than against the specific study the ask implicitly invokes. The role of the AI engineering manager at this altitude is to cite the empirical work *honestly* — by name, with the population, with the condition, with the generalisability window — so the ask survives the scrutiny the second quarter of the fashion is already inviting.

## Motivation — four shapes of ask that lose

Four failure shapes recur across investment asks that come back deferred or denied.

- **The vibes ask.** "AI productivity gains in the industry are significant; we need two more headcount to keep up." No citation; no number; no condition; no population. The CFO cannot defend this ask to their counterpart in finance; it is deferred until the manager returns with evidence, and by then the quarter has moved and the slot is gone.
- **The over-cited ask.** "Research shows 55% productivity gains on coding tasks (Peng et al., 2023); we are asking for two engineers and will see 55% gain against our current throughput." The citation is real; the generalisability is wrong. *Peng et al.* measured HTTP-server task completion by moderately experienced developers given a specific stand-alone task under controlled conditions with GitHub Copilot in 2022. A team building a production AI agent with eval-gated releases and a full SDLC is not that population, is not doing that task, and is operating in a different year with different tooling. The cited gain does not transfer unexamined. The CFO's partner — a research-literate finance analyst — will spot the generalisability gap and the ask will come back with an apology attached.
- **The one-sided ROI.** "This fine-tune will save $X in inference cost against the vendor-API baseline; ROI is 3.4×; approve the ask." No band; no condition under which the saving does not materialise; no acknowledgement of the labelling-cost or the compute-cost the fine-tune is trading the inference against. The *mod-110 chapter 03* three-band ROI discipline is not optional at this altitude; a single-number ROI projected on an AI initiative is a projection that assumes the non-determinism of AI outputs will cooperate, which is not a defensible assumption.
- **The ask with no exit criterion.** "Give us the labelling-data budget for this quarter and we will see improved eval performance." No specific metric; no specific delta target; no specific condition under which the budget is pulled back. Every ask without a named exit criterion is an ask for permanent funding against an ambiguous outcome; the CFO is right to decline. The exit criterion is the manager's lever for making the ask reversible, which makes it approvable.

All four failures are solved by the same discipline: cite the evidence honestly against the team's specific context, project outcomes in confidence bands against named conditions, and name the exit criterion that lets the funder claw back if the conditions are not met. The rest of this chapter builds the discipline.

## Core concept 1 — the four AI-engineering ask shapes

The AI engineering manager's asks fall into four recurring shapes. Each has a characteristic evidence surface.

- **Model-spend ask.** Headroom for provider API costs, enterprise contract commitments, self-host inference capacity. The evidence surface is the team cost model (mod-110 chapter 02) and the ROI projection with confidence bands (mod-110 chapter 03); the honest ask reads the current spend trajectory, the levers already pulled (mod-110 chapter 06), the levers left to pull, and the asked-for headroom against the projected growth. The failure mode is asking for headroom without naming the levers already pulled — the CFO reads that as "they have not optimised yet, come back when they have."
- **Labelling-data budget ask.** Human-labelling spend for eval-fixture growth (mod-104 chapter 04 registry), for fine-tuning preference data, for red-team adversarial corpora, for golden-dataset expansion. The evidence surface is the eval-trajectory page from chapter 02's single source of truth; the honest ask reads what the labelled data is specifically for (which eval slice is under-covered; which agent behaviour the preference data is intended to shape), how much labelling is needed in units (not dollars), and what the exit criterion is (a target slice-coverage number; a target inter-annotator-agreement floor). The failure mode is asking for a dollar budget without the unit math underneath — the CFO reads that as a slush fund, correctly.
- **Headcount ask.** A new engineer at a specific level and shape (`applied-ai-engineer`, `rag-engineer`, `fine-tuning-engineer`, `ai-eval-engineer`, `agentic-ai-engineer`, `senior-agentic-ai-engineer` — the shapes from mod-101). The evidence surface is the team shape and span (mod-109) and the growth plan for the current team (mod-108); the honest ask reads the specific capability gap — "we have no `fine-tuning-engineer` on the team and the fine-tune decisions we made last quarter were made without that capability in the room" — against the alternative (contracting the work, growing an existing IC into the shape via mod-108 chapter 04 cross-shape rotation, deferring the capability). The failure mode is asking for a headcount without naming the capability gap it closes — the VP reads that as "they want more staff," which is a weaker ask.
- **Tooling ask.** A vendor-SaaS line for a prompt-registry, a trace-evaluation platform, an eval-management tool, a safety-eval suite. The evidence surface is the build-vs-buy matrix (mod-110 chapter 04); the honest ask reads the three axes (vendor-API / fine-tune-equivalent / self-host-equivalent — or in tooling language, buy / fine-tune-OSS-tool / build) and the six decision inputs. The failure mode is asking for the SaaS line without the build-vs-buy matrix — the VP of engineering, who has seen seventeen SaaS asks this quarter, defaults to "build on OSS" unless the matrix defends the buy.

Each shape carries its own evidence surface. A manager who writes a single boilerplate ask template for all four is writing a worse ask for three of them. The chapter template below is parameterised against the four shapes.

## Core concept 2 — the citation discipline for empirical productivity research

The two most-cited empirical studies on AI-engineering productivity, both published within the last three years, are the ones every AI investment ask touches whether it cites them or not. The citation discipline below names each by its primary reference and reads the generalisability window the ask has to argue within.

### Dell'Acqua et al. — "Navigating the Jagged Technological Frontier"

*Dell'Acqua, Fabrizio; McFowland III, Edward; Mollick, Ethan; Lifshitz-Assaf, Hila; Kellogg, Katherine; Rajendran, Saran; Krayer, Lisa; Candelon, François; Lakhani, Karim R. "Navigating the Jagged Technological Frontier: Field Experimental Evidence of the Effects of AI on Knowledge Worker Productivity and Quality." Harvard Business School Working Paper 24-013, September 2023.*

- **What the study measured.** A randomised controlled experiment with 758 BCG consultants performing 18 realistic business-consulting tasks, randomly assigned to one of three arms — no AI access, AI access (GPT-4), AI access with light prompt-engineering training. Outcomes measured: task-completion rate, quality of output rated by human and AI graders, time per task.
- **The headline finding people cite.** Consultants in the AI-access arms completed 12.2% more tasks, 25.1% more quickly, and produced results rated 40% higher in quality than the control arm — *on tasks inside the AI's capability frontier*.
- **The finding people under-cite.** On tasks *outside* the AI's capability frontier (the "jagged" edge of what the model could and could not do), consultants using AI were 19 percentage points *less* likely to produce correct solutions than the control arm. The paper's framing is that the frontier is irregular — AI is excellent on some tasks and fails on adjacent-looking ones — and knowledge workers need to learn to read the frontier, not to generalise from inside-frontier successes.
- **Generalisability window for an AI-engineering manager's ask.** The population was business consultants performing consulting tasks, not software engineers writing AI features. The task set was 18 pre-defined consulting scenarios, not an open-ended engineering roadmap. The model was GPT-4 as of mid-2023; model capabilities, and thus the frontier shape, have moved since.<!-- needs-research: verify most-recent replications or follow-up studies that extend the frontier reading to software engineering populations -->
- **How to cite honestly.** "Dell'Acqua et al. (2023) establish that AI access produces substantial productivity and quality gains on tasks *inside* the model's capability frontier, and substantial *losses* on tasks outside it; this ask is for investment in the capability to tell the difference for our team's specific task set" is the shape. The ask then explains how the investment produces the frontier-reading capability — a specific eval surface that distinguishes inside-frontier from outside-frontier performance; a specific training loop that teaches ICs to read it. Citing the study without the frontier-reading framing is citing against the paper's actual argument.

### Peng et al. — "The Impact of AI on Developer Productivity: Evidence from GitHub Copilot"

*Peng, Sida; Kalliamvakou, Eirini; Cihon, Peter; Demirer, Mert. "The Impact of AI on Developer Productivity: Evidence from GitHub Copilot." arXiv:2302.06590, February 2023.*

- **What the study measured.** A randomised controlled trial with 95 professional developers (recruited via freelance platforms) performing a specific, bounded task — implementing an HTTP server in JavaScript — with half the participants given access to GitHub Copilot.
- **The headline finding people cite.** The Copilot-access group completed the task 55.8% faster than the control group (median task-completion time, with the 95% confidence interval reported in the paper).
- **The finding people under-cite.** The task was a single, well-scoped engineering task with clear completion criteria — not representative of the open-ended, long-tail, cross-system software work that constitutes most engineering time. The population was moderately experienced developers (self-reported) in a freelance context. The study does not measure quality of the resulting code, maintenance burden, or long-run productivity; it measures completion time on one task.
- **Generalisability window for an AI-engineering manager's ask.** Software engineering, yes — but on a specific *kind* of software engineering task (implementation of a well-defined specification using a popular language with abundant training data). An ask that invokes this study to justify productivity gains on *AI-engineering* work — prompt engineering, eval-suite authoring, retrieval-pipeline debugging, agent-trace analysis — is citing outside the paper's measured scope. The direction of effect may hold; the magnitude is not transferable.<!-- needs-research: verify whether more-recent studies have replicated or extended Peng et al. to AI-engineering-specific tasks, e.g. prompt authoring, eval writing, trace analysis -->
- **How to cite honestly.** "Peng et al. (2023) show substantial completion-time gains for well-scoped implementation tasks using AI coding assistants in a freelance developer population; we expect the direction of effect to apply to parts of our team's work but not the full task set, and we read our own throughput against the specific tasks the study scoped" is the shape. The ask then reads the team's time-use surface and distinguishes the implementation-task portion (where the study's effect likely applies) from the open-ended engineering portion (where it does not).

### What other research the honest ask reads

Two further references worth having in hand, each cited for a different sub-claim:

- **Microsoft / GitHub survey on Copilot (self-reported developer productivity).** *Kalliamvakou, Eirini. "Research: Quantifying GitHub Copilot's Impact on Developer Productivity and Happiness," GitHub Research Blog, September 2022* (self-reported survey data). Useful for the engagement and satisfaction axis the Peng et al. RCT does not measure; weaker as productivity evidence because self-reported productivity does not equal measured productivity.<!-- needs-research: verify current URL and any superseding Microsoft research publications -->
- **The jagged-frontier framing in applied practice.** Mollick's Substack posts and *Co-Intelligence* (Mollick, 2024) extend the Dell'Acqua et al. framing into practitioner guidance; useful for the training-the-frontier-reading element of the ask. Cite Mollick as practitioner framing, not as empirical evidence.<!-- needs-research: confirm latest edition and any peer-reviewed follow-ups -->

The honest ask reads these four references together: *Dell'Acqua et al.* for the inside-vs-outside-frontier framing, *Peng et al.* for the measured gain on bounded implementation tasks, Microsoft/GitHub survey for engagement, Mollick for the practitioner frame. None of them says "AI gives teams 50% more capacity"; the ask that says that is citing against the literature, not from it.

## Core concept 3 — the three-band projection with named assumptions

Every investment ask at this altitude projects an outcome. The outcome under AI non-determinism is not a point estimate; it is a band. Mod-110 chapter 03 teaches the mechanics; this chapter composes them into an investment-ask slide.

- **Low band.** The outcome if the frontier for the team's task set is less favourable than the planning assumption — specifically, if the fraction of inside-frontier tasks in the team's roadmap is lower than assumed, or if a vendor regression degrades the model behind the ask, or if the labelling data does not close the eval-slice gap the ask claimed it would. The low band is often below the cost of the ask. The honest ask names the low band explicitly.
- **Mid band.** The planning assumption — the fraction of inside-frontier tasks is at the historical average, the vendor behaviour holds, the labelling data closes the gap at the industry-median rate. This is the number the ask is implicitly asking the funder to plan against.
- **High band.** The outcome if the frontier is more favourable, if the team ships two bets this quarter instead of one, if the labelling data closes a second slice-gap in addition to the target. Never the planning number; always the upside.

A worked example for a model-spend ask against a new fine-tune pipeline:

```
Ask: $240k Q1 2027 labelling + $85k Q1 2027 compute for the csrep-agent fine-tune.
Hypothesis: a domain-adapted fine-tune on billing-dispute and finance-template slices 
closes the inside-frontier gap on those slices (currently 68 and 71 against a 76 floor).

Low band (-30% vs. planning):
  Assumptions — the labelling data closes <50% of the slice gap; the fine-tune costs 
  12% more per inference than the vendor-API baseline at current volumes; fine-tune 
  is pulled back and vendor-API baseline is held.
  Outcome — labelling + compute spent ($325k); slice-level eval moves <2 points; 
  no inference saving; net cost ≈ $325k with no revenue-side offset.
  Exit criterion — if the slice-level delta after week 6 of labelling is <2 points, 
  fine-tune pipeline is wound down and the remaining budget is returned.

Mid band (planning assumption):
  Assumptions — the labelling closes ~70% of the slice gap (consistent with historical 
  slice-closure rates on this team's eval registry); fine-tune costs are offset by 
  caching and batch at projected post-launch volumes.
  Outcome — slice-level evals recover to 73–75 (above floor); inference cost per 
  resolution flat; projected first-year revenue lift from improved resolution quality 
  $1.1M (per product partner's measurement on resolved-ticket revenue delta).
  Confidence — moderate; the slice-closure rate and the revenue-per-resolution 
  conversion are the two assumptions most likely to move.

High band (+50% vs. planning):
  Assumptions — labelling closes ~90% of the gap and the preference-data corpus 
  generalises to the adjacent "subscription-change" slice (not in the planning scope); 
  inference cost falls 18% at post-launch volumes due to the smaller fine-tuned model.
  Outcome — two slices recovered, inference-cost delta -18%, projected first-year 
  revenue lift $1.8M.
  Confidence — lower; the generalisation to the adjacent slice is specifically where 
  Dell'Acqua et al.'s jagged-frontier reading cautions against planning on.

Citations:
  - Dell'Acqua et al. (2023), inside-vs-outside-frontier framing, applied to the slice 
    selection — we are investing on slices where the team's traces show inside-frontier 
    behaviour at prompt-only, not on slices currently outside the frontier.
  - Peng et al. (2023), not directly cited for throughput; the fine-tune's planning 
    does not assume engineer-throughput gains from the fine-tune pipeline itself.
  - Team's mod-104 eval registry — historical slice-closure rates after labelling 
    across the last six fine-tune cycles, as the base rate for the mid-band assumption.
```

Four moves in this shape worth naming. First: the low band is explicit about the loss; the exit criterion is specific about the wind-down trigger. The ask is *reversible*, which makes it approvable. Second: the mid band names which two assumptions are most likely to move, so the funder's follow-up conversation is scoped to those two, not to the whole plan. Third: the high band is small and labelled as such; the ask is not planned against it. Fourth: the citations are honest about the generalisability window — "Dell'Acqua et al.'s framing applied to the slice selection" is not the same as "Dell'Acqua et al. shows fine-tunes produce 40% quality gains," which would be an invented claim.

## Core concept 4 — the FinOps composition and the "already-pulled levers" discipline

A model-spend ask the CFO reads against has a mandatory section: *what levers have been pulled before the ask was written.* Mod-110 chapter 06 names the five manager-owned levers (caching rate, batch utilisation, fallback routing, prompt-size discipline, context-window waste). The ask reads them one by one, states the current status, and states whether further movement is available.

```
Pre-ask lever status (mod-110 ch 06):
  - Caching rate: 42% as of Nov 2026, up from 11% at start of the year. The 
    prompt-template refactor in v1.3 is credited; further gains projected to top out 
    at ~55% on current workload shape.
  - Batch utilisation: offline workflows (overnight eval runs, retrieval-corpus 
    rebuild) are on the batch endpoint; interactive workflows are not. No further 
    batch headroom without product-scope change.
  - Fallback routing: three-tier (primary / secondary / cache) is in production 
    since Q3; hit rate on the cache tier is 18%; secondary is used in <1% of 
    requests (primary-provider availability has been healthy).
  - Prompt-size discipline: median prompt size is 2.4k tokens on a workload the 
    team sized for 3k budget; within discipline.
  - Context-window waste: p95 context-window usage is 38% of allocated; the 
    allocation was set conservatively for the prior model generation and will 
    be tightened in the Q1 2027 prompt refactor.
```

The CFO reads this and sees a team that has pulled the levers already. The ask is not for a team that has not optimised; it is for a team at the end of the optimisation curve that needs new investment to unlock the next capability. That framing changes whether the ask is deferred or approved. A manager who skips this section is inviting the deferral.

## Core concept 5 — the labelling, headcount, and tooling asks — the parameter variations

The template above (planning assumption, three bands, cited evidence, pre-pulled levers) is the shape for all four asks. Each varies in the specific sections.

- **The labelling-data ask.** The pre-pulled-levers section becomes the pre-labelling-levers section: what fixture-synthesis moved, what auto-labelling was tried, what sampling discipline reduced the raw labelling need. The exit criterion is a slice-coverage number or an inter-annotator-agreement floor. The citation is to the eval-registry page, not to external productivity research — labelling-cost per fixture is measurable on the team's own data.
- **The headcount ask.** The pre-pulled-levers section becomes the pre-headcount-levers section: what cross-shape rotations were tried (mod-108 chapter 04), what contract work was tried, what growth plan was tested for the gap. The citation is to the team shape and span (mod-109) — the ask has to defend the shape gap against the alternative shapes already on the team. The exit criterion is the capability demonstrated (first `fine-tuning-engineer` artefact: the fine-tune decision document for the next initiative) within a named window.
- **The tooling ask.** The pre-pulled-levers section becomes the build-vs-buy matrix (mod-110 chapter 04). The citation is to the matrix and to the reversibility cost. The exit criterion is a renewal gate at the first anniversary — has the tool earned its place against the matrix at six-month and twelve-month reads?

Each variation is small; the core discipline is the same. The *shape* — bands, named assumptions, citations within generalisability, pre-pulled levers, exit criterion — is what the funder recognises as "a defensible ask."

## What good looks like

- Every ask is dated, versioned, and on file in the team handbook.
- Every ask carries three confidence bands with named assumptions.
- Every ask cites empirical productivity research by name, with the generalisability window stated, and reads the team's context against the window honestly.
- Every ask names the levers already pulled and the levers still available.
- Every ask names an exit criterion and the conditions under which the ask is reversed.
- The CFO and the VP can read any ask in under fifteen minutes and know what is being asked, what is being promised, and what happens if the promise does not land.

## What bad looks like

- Asks read as vibes: "industry productivity research shows gains; we need more staff."
- Asks cite a study without reading the generalisability window; the finance analyst spots the gap and the ask is deferred.
- Asks project a single-number ROI; the actual outcome lands in a band the ask did not disclose, and credibility is downgraded.
- Asks do not name pre-pulled levers; the CFO reads them as "they have not optimised yet."
- Asks have no exit criterion; the funder is correctly reluctant to approve.

## Summary

- The AI engineering manager's four ask shapes are model-spend, labelling-data budget, headcount, and tooling. Each has a characteristic evidence surface.
- The citation discipline is specific: *Dell'Acqua et al.* (jagged frontier) for the inside-vs-outside-frontier framing, *Peng et al.* (Copilot) for measured gains on bounded implementation tasks. Both are cited within their generalisability windows, not as blanket "AI productivity gains" claims.
- Every ask projects three confidence bands with named assumptions, names the levers already pulled, and names the exit criterion that makes the ask reversible.
- The next chapter turns to the other side of the compounding-credibility surface: narrating regressions honestly, in the same cadences and with the same discipline, so leadership learns the team as a trustworthy signal rather than a marketing channel.
