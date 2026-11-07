# Choosing Team Shape: Mix, Alignment, and the Embedded PM Question

A team-shape decision is the quietest, highest-leverage decision an engineering manager makes. Once a team is standing — three applied engineers, one eval, one RAG, a half-time infra rotation, no embedded PM, vertically aligned to a product surface — everything else follows: the rituals that make sense, the triage the team can hold, the span the manager can carry, the kinds of work that ship on time, and the kinds that quietly starve. The decision is quiet because nobody announces it; it was made by the first three hires. It is high-leverage because changing it later is slow, expensive, and visible to everyone on the team.

This chapter is how the AI Engineering Manager makes the decision in writing, on a cadence, and specialised to AI engineering workloads. The general team-topology canon (Skelton & Pais, *Team Topologies*; Larson on team sizing) is the prerequisite; the specialisation is around the three distinctive properties of AI engineering work — the measurement discipline the team must carry (an eval harness, a prompt + judge + fixture registry, a trace corpus), the model-provider volatility the team absorbs from outside, and the incident surface non-determinism creates.

## Motivation — three team-shape failures that look like performance failures

Before the framework, three shape failures that read, on the surface, like the team is simply under-performing.

- **The shape with no measurement contract.** The team is five applied engineers plus a product manager. They ship an LLM-backed feature every two weeks. Quality on the feature is "the team's judgment" — nobody owns an eval suite; traces are read individually; regressions are noticed by customers. The team's velocity, on the surface, is excellent. Then a frontier-model release reshapes the prompt behaviour, three quietly regressed features reach the customer, and the executive sponsor asks, "how did you not see this coming?" The honest answer is: the shape did not have anyone whose job was to see it coming. This is not a performance failure; it is a shape failure. An eval-shaped seat was missing from the roster.
- **The horizontal capability team that quietly became a request-queue.** The team is a "platform" team — two agentic engineers, two infra engineers, a RAG engineer. Four product teams depend on them. The horizontal team is theoretically faster than each product team building the shared capability themselves; in practice, every two weeks the team runs a planning meeting trying to prioritise five demand sources against three ICs' capacity, the roadmap is a lossy round-robin, every product team feels under-served, and two of the ICs are quietly interviewing. This is not a motivation failure. It is a team-shape failure: a capability team with no interaction-mode discipline (collaboration vs. X-as-a-service vs. facilitating — *Team Topologies*) becomes a request queue, and request queues demoralise the people in them.
- **The vertical pod with no PM, where the engineers now run product.** The team is a vertical product-aligned pod — four AI engineers, no embedded PM, reporting through a product team whose PM covers six surfaces. The AI engineers end up making product decisions under the surface (what the summariser's quality floor should be, which user segments the eval harness measures, which feature gets the week's capacity) because no PM is close enough to make them in time. Their technical work slows. The PM reads the velocity as "the team is slow" and does not see that the engineers have been co-opted into product work the PM is not doing. This is not a slowness failure; it is a shape failure — the embedded-PM question was decided by default (no), and the default did not match the team's altitude.

All three failures look like performance problems. They are the team's shape producing the behaviour the manager is reading on the surface. The written, dated shape decision is the manager's instrument for catching the failure before it produces the signal.

## Core concept 1 — the three axes of the shape decision

The shape of an AI engineering team is, in writing, three axes. The decision is not one of them; it is the triple.

1. **Mix.** How many of each IC shape. Specifically: applied, agentic, eval, RAG, fine-tune (the five IC shapes from mod-101 chapter 01 that anchor at L30); senior agentic (L40); and whether a staff-plus IC partner (`agentic-systems-architect` or `principal-ml-engineer`) is part of the team or sits outside it. Also: whether the team carries half-time rotations (infra, security, on-call), and the numerical total.
2. **Alignment.** Vertical (the team is aligned to a product surface, owns the surface's full AI stack, ships to a product KPI) or horizontal (the team is aligned to a capability — eval, retrieval, serving infra, safety — serving multiple product teams). The *Team Topologies* vocabulary: stream-aligned vs. platform vs. enabling vs. complicated-subsystem. Most AI engineering teams are stream-aligned or complicated-subsystem; eval, RAG-at-scale, and serving-infra teams are often platform; a small number of teams are enabling (the "we help other teams onboard to AI" shape).
3. **Embedded PM.** Yes (a dedicated product manager, embedded in the team, co-authors the roadmap with the engineering manager, owns the user research and the product discovery, sits inside the team's cadence) or no (the team relies on a PM shared across multiple surfaces, or on the engineering manager + a staff-plus IC covering product-adjacent decisions, or on a technical-product-manager / AI-product-manager specialist).

A written team-shape decision reads on all three axes. If the manager cannot state the mix, the alignment, and the embedded-PM choice in two paragraphs, the decision has not been made in writing — it has been made by default.

## Core concept 2 — mix, specialised to AI engineering workloads

Larson's team-sizing framework (*An Elegant Puzzle*) reads a team's mix against the workload it is being asked to carry. For a generic feature-delivery engineering team, the workload breaks into: delivery of new features, maintenance of what shipped, on-call response, and infrastructure investment. The mix is read against those four. AI engineering adds three distinctive workloads the mix has to carry:

- **Measurement workload** — eval design, judge authorship, fixture curation, trace reading, score monitoring, regression investigation. On a generic team this work is implicit in "testing"; on an AI engineering team it is a specialist workload that demands at least one person whose job is to carry it, unless the team is explicitly deferring the entire measurement discipline to an adjacent eval team.
- **External-surface absorption** — provider release notes, pricing changes, model-version migrations, framework API shifts, deprecations. On a generic team, dependencies move on predictable cadences; on an AI engineering team, the surface moves weekly. Chapter 03 names the ritual that reads on it (model-watch, vendor release review); the mix has to include at least one person whose attention bandwidth can hold the absorption.
- **Non-deterministic incident surface** — the production incidents whose root cause is prompt + judge + retrieval + tool + model-vendor interaction, not deterministic code failure. Chapter 04 names the triage taxonomy; the mix has to carry the specialists whose craft the taxonomy reads on.

These three workloads map to the shapes in the mix.

| Workload the team carries | The shape(s) in the mix that carry it |
|---|---|
| New feature delivery (user-facing or capability-facing) | `applied-ai-engineer`, `agentic-ai-engineer`, `senior-agentic-ai-engineer` |
| Measurement — eval design, judge authorship, fixture curation, trace corpus, regression investigation | `ai-eval-engineer` (primary); applied and agentic ICs contribute on their scope |
| Retrieval depth — chunking, embedding, re-ranking, retrieval-side regression investigation | `rag-engineer` (where retrieval is a first-class product surface, not an afterthought) |
| Specialist fine-tune work — SFT, DPO, model customisation, cost-serving substitutions | `fine-tuning-engineer` (where the team's cost structure or latency structure justifies the investment) |
| External-surface absorption — provider releases, framework deprecations | The senior-most IC + the manager; split across the team via the model-watch ritual (chapter 03) |
| Non-deterministic incident command | Senior IC + on-call rotation across the team; the staff-plus IC partner testifies on hard incidents |
| Architectural direction for agentic / fine-tune programmes | The staff-plus IC partner (`agentic-systems-architect`, `principal-ml-engineer`), inside or outside the team |

The mix question is: *which workloads are you being asked to carry, and do you have the shapes to carry them?*

Three common mix mistakes.

- **Measurement is an unfilled seat.** The team is five applied + a PM and no eval ownership. Measurement discipline was treated as a quality of all engineers, not a specialist workload. The team ships fast and regresses silently. Unless the team can genuinely defer to a central eval team (and has a clear service contract with them), the measurement seat has to be in the mix.
- **The fine-tune hire comes before the fine-tune workload.** The manager hires a fine-tuning engineer because it feels prestigious; the team has no sustained fine-tune workload. The engineer either idles, drifts to work they were not hired for, or leaves. Fine-tune workload has to precede the fine-tune hire; chapter 05 of mod-105 is specific about reading the portfolio for whether fine-tune is justified.
- **The team has no staff-plus partner at all.** The team runs on senior ICs, but there is no `agentic-systems-architect` or `principal-ml-engineer` partner — inside the team, adjacent to it, or at the next-line-manager's altitude. The manager is now the only person reading technical direction, which collapses to the manager's current bandwidth. When the ambition of the work exceeds the current senior ICs' altitude, there is no appeal; the direction drifts, or the manager becomes the single point of judgment. Either outcome is a shape failure.

## Core concept 3 — alignment: vertical pod vs. horizontal capability team

The second axis of the shape decision is alignment. The canonical *Team Topologies* reading applies — stream-aligned (vertical, product-facing), platform (horizontal, capability-facing), enabling (temporary, teaches other teams), complicated-subsystem (deep expertise owning a specialist system). Three alignment patterns are common for AI engineering teams; a fourth is rare and often wrong.

### Vertical product-aligned pod (stream-aligned, *Team Topologies*)

The team is aligned to a product surface — a chat product, a summarisation product, a developer-tool surface, an agent product. The team owns the full AI stack for that surface: the applied code, the prompts, the evals that measure the surface, the retrieval pipeline that feeds it, the fine-tune if there is one, the production incidents on it. The team ships to a product KPI; product decisions are made with the embedded PM; measurement is internal to the team (the team owns its eval suite); technical direction is co-authored with the staff-plus IC partner.

Vertical pods are the default. They optimise for cognitive-load locality (the team can hold the surface in its head), for incident-response speed (the team that shipped the feature is the team that owns the pager), and for the feedback loop between measurement and shipping (the eval engineer sits next to the applied engineer whose feature they evaluate). A vertical pod on an AI product surface is usually three to seven ICs plus a PM plus a manager — sometimes with a staff-plus IC partner and sometimes without.

Vertical pods have weaknesses. Measurement discipline fragments across pods (every pod carries its own eval, judges, and fixtures; reuse is accidental). Retrieval and fine-tune work duplicate across pods that could have shared. The absorption of provider releases is done four times.

### Horizontal capability team (platform, *Team Topologies*)

The team is aligned to a capability — eval and measurement across the company, retrieval infrastructure, the serving substrate, the safety-and-guardrails layer, the AI-assist platform. The team serves multiple vertical pods; its product KPI is the product-team-experience of the capability (platform NPS, adoption, time-to-first-eval, time-to-onboard-a-new-product-team) rather than any particular customer-facing surface.

Horizontal capability teams optimise for reuse (the eval harness is built once, judged once, maintained once), for specialist depth (the team can hold craft at the altitude no vertical pod would justify), and for provider-landscape absorption centralised (chapter 03's vendor release review runs once, not four times). They succeed when the interaction mode (*Team Topologies*' three modes: collaboration, X-as-a-service, facilitating) is explicit and bounded — the eval platform team as X-as-a-service, with a defined API, a defined SLA, and a defined roadmap input mechanism.

They fail predictably when the interaction mode is implicit. The team becomes a request queue (see the second motivation example above); demand exceeds capacity; prioritisation becomes politics; the team is blamed for everything none of them individually broke. A horizontal team without an interaction-mode discipline is not a capability team — it is an intake form with headcount.

The AI engineering teams that commonly succeed as horizontal are: the eval platform team (owns the registry mod-104 builds, serves vertical pods as X-as-a-service); the retrieval-at-scale team (where retrieval is a shared substrate and no vertical pod would reasonably own the embedding + rerank + chunking depth); the serving infrastructure team (where serving-side model deployment and inference-side optimisation are shared); and the safety-and-guardrails team (where the triad mod-106 builds is shared across every shipping surface).

### Complicated-subsystem team (*Team Topologies*)

The team owns a specialist system whose depth is beyond any product pod's reasonable cognitive load — a fine-tune programme at scale, a research-grade agent reasoning subsystem, a specialist safety-classifier pipeline. The team is small (often three to five ICs plus a staff-plus IC), intensely specialised, and interacts with vertical pods either as X-as-a-service or as collaboration-for-a-bounded-period. Fine-tune programmes and specialist safety-classifier subsystems are the common AI engineering cases.

### Enabling team (*Team Topologies*)

The team's job is to help other teams onboard a new capability — AI-assist stack enablement, eval discipline onboarding, retrieval-pattern education. They are temporary by design: the team dissolves or re-roles once the host teams have internalised the capability. Enabling teams are rare in AI engineering today because the surface moves faster than enabling teams can stabilise against; where they exist, they often graduate into a platform team (horizontal capability) or an educational-resource-publishing centre of excellence.

### How to pick

The alignment follows the workload, not the team's preference. Three readings.

- **If the work is one product surface deep, and the measurement + retrieval + applied work is coupled to that surface, pick vertical.** The cognitive-load locality, the PR-latency-to-eval locality, and the incident-ownership locality dominate the duplication cost.
- **If the work is a capability that two or more vertical pods would have to re-build, pick horizontal — with the interaction mode written down and defended.** The duplication cost dominates the locality cost; the interaction-mode discipline is what prevents the request-queue failure.
- **If the depth of a specialist subsystem is beyond any one pod's cognitive load, pick complicated-subsystem.** Fine-tune programmes and specialist safety classifiers are the common cases.

A fourth reading is to combine shapes: a small vertical pod that is also running a time-bounded enabling engagement with an adjacent team; a horizontal platform team that temporarily collaborates with one vertical pod to co-design a shared retrieval API. These are legitimate; they require the manager to write down the shape and the time horizon in the decision.

## Core concept 4 — the embedded-PM question

The third axis of the shape decision is whether the team has a dedicated, embedded product manager. The default across most companies is "yes for product-facing teams, no for platform teams." For AI engineering teams that default is correct roughly half the time; the other half is where the shape failure lands.

The embedded PM covers four things a team cannot cleanly do without.

- **The quality floor and the user-research input.** For an AI product surface, the quality floor is often a product-side judgment (is this summary good enough to ship? what does "good enough" mean to the user segment we are serving?) that is partly empirical (an eval score) and partly a product-shaping conversation with real users. The PM is the owner of the user-research loop that produces the latter.
- **The roadmap that balances AI ambition against user need.** AI engineering work is seductive; the team will always want to push the capability frontier. The PM holds the line between "what is technically interesting" and "what the user will pay for," and translates between them in both directions.
- **The cross-functional surface — marketing, sales, support, legal.** The PM is the connective tissue to the functions outside engineering; without one, the engineering manager becomes the connective tissue by default and loses engineering-manager bandwidth to functions whose craft they do not own.
- **The release discipline on the customer side.** Which features ship to which segment, in which order, with what messaging. Mod-112 (executive communication) and mod-106 (the release triad) both touch this; the PM is the day-to-day owner.

Three readings on the embedded-PM question.

- **Vertical product-aligned pod, consumer- or developer-product-facing surface, clear user-research need: embedded PM is required.** Not negotiable; a pod of this shape without a PM is one of the three motivation failures. If the role cannot be filled, the manager has to carry the PM work temporarily and shrink the engineering ambition to the bandwidth left — which is usually about 40% of the original.
- **Horizontal platform or capability team, internal customers (the other engineering teams), capability KPIs that are mostly technical: embedded PM is optional, usually "no".** The platform's PM-shape work is usually done by a TPM or by the engineering manager with a strong product-adjacent staff-plus IC; a traditional PM on a platform team is often under-utilised and over-constrained.
- **Mid-state: the team ships a product surface, but the user research is thin, the segment is small, and the product-adjacent decisions are mostly "what does the eval harness say?": a shared PM is acceptable.** A PM shared across two teams, with 50% allocation to each, works if the PM is senior, if the team's cadence is predictable, and if the engineering manager is willing to cover PM decisions on PM-off days. It is a fragile shape; monitor it quarterly.

A fourth variant: the **AI product manager** specialisation. The role is increasingly common; the honest read is that most PMs can learn to work with an AI engineering team in six months, and a specialist AI PM is a strong preference not a hard requirement — more important than the specialism is whether the PM reads evals and traces as part of their craft or treats them as engineering-internal.

## Core concept 5 — writing the shape decision down

A shape decision that lives only in the manager's head is not a decision; it is a current preference. The written artefact has five sections.

1. **The context.** What is the team being asked to carry? Which product surface or capability? Which workload breakdown (new feature, measurement, external-surface absorption, incident response, specialist investment)? Which adjacent teams and dependencies?
2. **The mix.** Named shapes and their numeric counts. "Three applied, one eval, one RAG, 0.5 infra rotation, 0.5 agentic-systems-architect partnership (outside the team), one PM, one engineering manager." Justification against the workload breakdown.
3. **The alignment.** Vertical pod / horizontal capability / complicated-subsystem / enabling (and the *Team Topologies* term). If horizontal, the interaction mode with each dependent team, written down and agreed with the receiving manager.
4. **The embedded-PM choice.** Yes / no / shared, with the justification.
5. **The span read.** The manager's current direct-report count and a brief read against chapter 06's three signal patterns (partner-needed, split-needed, manager-to-grow). The shape decision has to fit inside the span the manager actually has.

Date the decision. Name the staff-plus IC partner who read it before publishing. Link it from the team handbook. Schedule the next re-read: at minimum quarterly, and immediately on any workload shift (a new product surface added, a frontier-model generation shift, a pod-size change, an incident class the current shape cannot carry).

## Two worked team-shape decisions

Short vignettes. Treat them as templates, not as recipes; your context is not these contexts.

### Vignette 1 — a vertical product-aligned pod with an embedded PM

Context. A chat product serving a specialist professional segment (legal research, medical imaging, developer tooling — pick your surface). The team is being asked to ship a new feature every three to four weeks, hold a quality bar the user segment is willing to pay for, and absorb the frontier-model surface without losing quality on each release.

- **Mix.** Three applied engineers, one eval engineer, one RAG engineer, 0.2 FTE agentic-systems-architect partnership (outside the team, reviewing architecture every two weeks and testifying on L30→L40 promotions). One PM, one engineering manager. No fine-tune hire (the portfolio reading from mod-105 says fine-tune is not currently justified against the frontier model's capability).
- **Alignment.** Vertical, stream-aligned. The pod owns the full stack for its product surface: applied code, prompts, eval harness for the surface, retrieval pipeline that feeds the chat, the on-call pager for the surface. Interactions with the central eval-platform team (X-as-a-service for registry tooling) and the central safety-and-guardrails team (collaboration quarterly on threat-model updates).
- **Embedded PM.** Yes. The user segment is specialised, the user research is non-trivial, and the quality-floor decisions are partly product-shaping conversations; a shared PM would not have enough context on the surface.
- **Span read.** Engineering manager has five direct reports (three applied + one eval + one RAG) plus a dotted-line into the shared infra rotation; the PM is a peer, not a report. Span is at the comfortable five-to-eight range; current direction is to stabilise, not to split or add a partner.

### Vignette 2 — a horizontal eval-and-measurement capability team, no embedded PM

Context. The company has four vertical product-aligned pods shipping AI-backed features. Measurement discipline fragmented: four eval harnesses, four judge sets, no shared registry, provider releases absorbed four times. The company is standing up a horizontal eval team to centralise the measurement discipline and serve the pods as X-as-a-service.

- **Mix.** Three eval engineers (one senior, two L30), one `principal-ml-engineer` partner for the methodology guidance (at the next-line-manager's altitude, not inside the team), 0.5 FTE TPM (programme-management for the onboarding schedule), one engineering manager. No PM in the conventional sense.
- **Alignment.** Horizontal, platform (*Team Topologies*). Interaction mode with each vertical pod: X-as-a-service for the eval registry tooling and the judge library; collaboration for the first six weeks per pod onboarding; facilitating for any pod that is building its own judge for the first time. Service contract: 48-hour turnaround on a new-judge review, weekly office hours, quarterly capacity-plan published.
- **Embedded PM.** No. The team's customers are internal; the TPM covers the onboarding programme; roadmap is a joint read with the four pod managers quarterly.
- **Span read.** Engineering manager has three direct reports (the eval engineers) plus the TPM dotted-line. Span is small; current direction is to grow the team to five by hiring two more eval engineers over the next two quarters, which will shift span into the five-to-seven range and may justify splitting a lead-eval-engineer role out of the current senior.

## What good looks like

- The team-shape decision is on file in the team handbook, dated, and signed by the engineering manager and the staff-plus IC partner who read it.
- The decision states the mix numerically, the alignment in *Team Topologies* vocabulary, the embedded-PM choice, and the span read against chapter 06's patterns.
- The decision was re-read within the last quarter, or there is a dated note explaining why no re-read is currently needed.
- If horizontal, every dependent-team interaction mode is written down and agreed with the receiving manager.
- The three workloads this chapter names (feature delivery, measurement, external-surface absorption) each have a named owner in the mix; the non-deterministic-incident-command surface has an on-call rotation.
- The last hire into the team was justified against a workload the shape decision names; the next planned hire is similarly justified in writing.

## What bad looks like

- The team's shape is "whatever it has been for the last eighteen months"; no written decision; the mix drifted hire by hire; nobody can say in two paragraphs what the team is.
- Measurement workload is "everyone's responsibility," which in practice means nobody's. No eval engineer in the mix; no clear deferral to an external eval team either.
- The horizontal team is a request queue; every pod feels under-served; the team is blamed for every delay; the interaction mode was never written down.
- The vertical pod has no PM and no plan to add one; the engineering manager has quietly been doing the PM work for three quarters; engineering ambition has been shrunk without anyone saying so out loud.
- The staff-plus IC partner was not consulted on the shape decision; the direction of the team is a single-point read from the engineering manager.
- The shape decision has not been re-read since the last frontier-model generation shift; the workload has moved but the roster has not.

## Summary

- The team-shape decision is the quietest, highest-leverage decision an engineering manager makes. It has three axes — mix, alignment, embedded PM — and a span read anchoring it.
- Mix is specialised to AI engineering workloads: new-feature delivery, measurement, external-surface absorption, non-deterministic incident command, and (sometimes) specialist fine-tune work. Each workload has to have a named owner in the mix or a written deferral to an adjacent team.
- Alignment uses the *Team Topologies* vocabulary. Vertical (stream-aligned) is the default for product-facing work; horizontal (platform) is right for capabilities two or more vertical pods would otherwise duplicate; complicated-subsystem is right for specialist depth beyond any one pod's cognitive load; enabling is temporary.
- The embedded-PM question is required for most vertical product-aligned pods on a customer-facing surface; usually "no" for horizontal platform teams; sometimes "shared" for small or thin-research surfaces.
- The decision is written down, dated, signed, re-read quarterly, and fits inside the manager's span.

The next chapter opens the second manager-owned artefact: the AI-specific ritual set — the four rituals that earn their place on a working AI engineering team, and the discipline of killing the rituals that do not.
