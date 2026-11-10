# AI Infra as Partner, Not Vendor — Shared SLOs and Shared Capacity Planning

On the serving tier, the GPU capacity plan, and the inference platform, the AI Engineering Manager is the **customer** of `ai-infra-team-lead` (level 45, AI Infra ladder). The peer's team builds and operates the serving stack — vLLM, TensorRT-LLM, SGLang, the KV-cache implementation, the autoscaling policy, the quantisation strategy — against which this team's AI products run. The direction of trade is real; this chapter does not pretend it is symmetric. Serving-tier craft lives on the Infra ladder; the AI Engineering Manager does not do it.

But "customer" does not mean "vendor." The healthy pattern between a level-45 engineering-manager pair across the AI Engineering and AI Infra ladders is three joint artefacts — a shared SLO per user-facing feature, a shared quarterly capacity plan, and a shared incident path (R17 in this curriculum's routing registry) — co-authored by both managers and consumed by both teams. When the pattern is absent, the AI Engineering team treats Infra as a ticket queue, Infra treats the AI Engineering team as a noisy demand signal, and the user experience — which is the compound of both — accumulates error-budget burn that nobody owns.

## Motivation — four failure shapes when the Infra interface is a ticket queue

- **The Monday-morning GPU shortage.** The AI Engineering team pushes a change that doubles the per-query token output on a feature tied to a product campaign launching Tuesday. By Monday afternoon the Infra team's autoscaling has hit its GPU reservation ceiling and the serving tier is queueing. The manager escalates; the Infra lead points out — correctly — that the demand was never in the quarterly capacity plan because the team did not share a forecast. The campaign slips a week while a reservation is widened. The failure is the absence of a shared capacity ritual that mod-110 chapter 06 names as a cadence beat and mod-103 names as a roadmap input but that nothing in the team's operating model forces to happen.
- **The latency regression nobody owns.** The end-to-end latency on a flagship feature has crept up over three weeks. The AI Engineering on-call sees eval-online quality stable and retrieval latency flat; the Infra on-call sees serving-tier TTFT stable and inter-token latency flat. Each side's SLIs are green. The user experience, which is the sum, has slipped past the SLO. The failure is not the monitoring; it is the absence of an **end-to-end SLO** with a joint owner — the canonical anti-pattern in cross-team service decomposition. <!-- needs-research: cite the specific Google SRE Workbook chapter on service-decomposition SLO co-ownership. -->
- **The 70B-model roadmap row.** The quarterly roadmap (mod-103) carries a feature that depends on routing a non-trivial fraction of traffic to a 70B self-host candidate. Nobody put a capacity ask in front of Infra with the forecast QPS and serving-tier shape. Six weeks before launch, the Infra lead reads the row and surfaces that the reservation is not in the quarterly supply plan, and that the cloud lead-time on committed capacity is eight weeks. The feature slips a quarter not for a quality reason but for a procurement reason. The failure is treating the self-host call (mod-105 / mod-110) as a build-vs-buy artefact without also treating it as a capacity ask addressed to the Infra manager peer.
- **The 2am incident with no split owner.** At 2am, user-facing traces show retrieval timeouts on an agent workflow. The tool-call-to-inference round-trip has blown past its budget. Is the vector store degraded, or has serving-tier TTFT crept up such that the agent's reasoning step is now the bottleneck? Nobody on the call can tell. The Incident Commander (mod-107 chapter 04) has no pre-designated Subject-Matter Lead from each side because the pager split was never codified. Mean-time-to-mitigate drifts while the two teams argue root-cause ownership.

All four look like Infra problems or application problems in isolation. They are **cross-manager-interface** failures. Something that should have been a shared artefact was either not written down or was written down without a joint owner.

## Core concept 1 — what `ai-infra-team-lead` owns, what this role owns, and the shared row

The peer manager on the Infra ladder is `ai-infra-team-lead` (L45, AI Infra ladder). Their IC counterpart on the inference-platform surface is `ai-infra-inference-engineer`, who owns vLLM / TensorRT-LLM / SGLang tuning, the KV-cache implementation, and the quantisation strategy. Both roles have been referenced across mod-105, mod-107, and mod-110; this chapter pins the operating interface that makes the referencing concrete.

| What `ai-infra-team-lead` owns | What this role (`ai-engineering-manager`, L45, AI Engineering family) owns |
|---|---|
| Model-serving stack choice and tuning — vLLM, TensorRT-LLM, SGLang configuration, speculative decoding, continuous batching. | Application-side SLOs — end-to-end task quality, task-success rate, user-facing latency budget, the eval surface above the serving tier (mod-104). |
| GPU capacity planning — reservations, commitments, autoscaling policy, fallback capacity, cloud-provider negotiations alongside Procurement. | The per-feature capacity **ask** — forecast QPS, modal input and output sizes, model mix across the vendor portfolio (mod-105), confidence bands on the forecast (mod-103). |
| Serving-tier SLOs — TTFT, inter-token latency, throughput, availability — and the serving-tier SLIs that measure them. | Prompt, retrieval, and agent complexity that drives serving cost and shape — context size, output-length policy, tool-use fan-out, parallel-call patterns. |
| Training-platform infrastructure and the fine-tune runtime when the build-vs-buy call lands on fine-tune. | The **decision** to vendor-API / fine-tune / self-host (mod-105 and mod-110 chapter 04) and the product reason behind it. |
| KV-cache implementation, quantisation strategy (FP8 / FP4 / INT8 trade-offs), inference-side cost optimisation at platform depth (mod-110 chapter 06 deferral boundary). | The FinOps lever set at the manager's altitude (mod-110 chapter 06) — caching rate above the serving tier, prompt-size discipline, batch utilisation, fallback-routing hit rate, context-window waste. |
| The serving-tier observability — serving metrics, serving traces, serving-side log retention. | The application-tier observability — prompt / retrieval / tool-call / agent-step traces, eval-online signals, user-facing error traces. |

The **shared row** — co-owned, co-authored, co-signed by both managers:

- The **end-to-end SLO** per user-facing feature. This decomposes into application-side quality and infra-side performance, but the user experience is the compound and neither team's half can defend the whole alone.
- The **quarterly capacity plan.** Demand forecast (this team's input), supply plan (Infra's input), gap analysis (joint), price projection against the vendor-API baseline (joint, with mod-110 chapter 04 as the referee).
- The **shared incident path (R17).** Pager split, Incident Commander selection rule, Subject-Matter Lead expectation across both teams, joint postmortem format when both teams are involved, action-item split by team.

The pattern across all three shared artefacts is the same: *each team brings its half as an input, both managers co-sign the joint artefact, both teams operate against it.* A level-45-to-level-45 interface runs on co-authored artefacts; this chapter specifies the three that matter.

## Core concept 2 — shared SLOs, in the SRE-book sense

The Google SRE Book and the SRE Workbook established the canonical form of a service-level objective: a numerical target on a service-level indicator over a window, with an explicit error budget that acts as the policy lever when the target is burning faster than forecast. <!-- needs-research: cite the specific Google SRE Workbook chapter on SLO decomposition and shared ownership across teams. --> For a classical web service, the SLO sits on a single team because the service sits on a single team. For an AI product, the user-facing experience is the compound of the application tier (prompt, retrieval, agent orchestration, eval-measured quality) and the serving tier (TTFT, inter-token latency, availability) — and when neither tier's SLI alone describes the user's experience, neither team's SLO alone can sit on the user-facing surface.

The resolution is a **two-tier SLO structure** co-authored by both managers.

- The **user-facing SLO** is the manager-owned contract with the product surface, written in user-language and measured on a sample set that resembles production traffic. "95% of user queries on the doc-Q&A agent return a correct answer within 3 seconds, measured on sample set X, 28-day rolling window, error budget Y." This is the compound — not a serving-tier metric, not an application-tier metric.
- The **serving-side SLI** is Infra's reporting line: TTFT p95, inter-token-latency p95, availability, throughput headroom.
- The **application-side SLI** is this team's reporting line: eval-measured quality against the online judge (mod-104), retrieval recall at the top-k the agent actually calls, tool-call success rate.

The user-facing SLO is defended by both SLIs reading green. If the application-side SLI reads green and the user-facing SLO is burning, Infra's SLI is in question. If the serving-side SLI reads green and the user-facing SLO is burning, this team's SLI is in question. If both read green and the user-facing SLO is still burning, the decomposition is wrong and both managers re-open it together.

**A hypothetical worked example.** Consider a doc-Q&A agent with a user-facing SLO of "95% of queries return a correct answer within 3 seconds over a 28-day rolling window." The 5% error budget is co-signed by both managers in an allocation:

- **2% to Infra** — serving-side outages, inter-token-latency breaches that push total latency past 3 seconds, serving-tier queueing at capacity ceiling.
- **2% to this team** — eval-measured quality regressions on the correctness criterion, retrieval misses that return no grounding, tool-call failures that drop the agent into a fallback.
- **1% shared** — the long tail neither team cleanly owns (a vendor-upgrade regression that hits both serving latency and application quality; a traffic-pattern shift that reveals a seam neither SLI covered).

When the end-to-end SLO is burning, the error-budget conversation runs between the two managers: *we've burned 3% in the first 10 days — Infra's contribution is 0.5%, this team's is 1.8%, the shared bucket is 0.7%; where is the biggest lever?* The 1.8% on this team's side probably needs a mod-104 eval-fixture addition and a prompt or retrieval change; the 0.5% on Infra's side is a latency breach Infra is already triaging; the 0.7% in the shared bucket forces the two managers to ask whether the decomposition still describes the user experience. The point of the allocation is not the math — it is that the conversation has a structure and both managers walk in with the same numbers.

A shared SLO that never gets re-examined is usually a shared SLO that has quietly stopped describing the user. Re-opening the decomposition is the discipline working, not a failure.

## Core concept 3 — joint capacity planning

The second shared artefact is the **quarterly capacity plan**, co-owned by both managers, with four sections. It lives in a shared location, is updated on a quarterly cadence (or more frequently when the vendor-portfolio update of mod-105 chapter 06 names a near-term change), and feeds both the AI Engineering team's roadmap (mod-103) and the Infra team's own planning.

- **Demand forecast — this team's input.** For each feature on the roadmap: expected QPS (with a mod-103 confidence band), modal input size, expected output-token mix, model mix across the vendor portfolio (mod-105's active-set inventory), and mode of inference (interactive, batch, agent fan-out). Confidence bands come in mod-103 language — current-model floor, announced-upgrade dependency, speculative. A point estimate on a speculative row is a lie by omission; if the team cannot forecast a row, the row says so, and Infra carries it as "watchlist, not planned" rather than silently planning for zero.
- **Supply plan — Infra's input.** For each model in the active set: GPU reservation shapes (count, type, duration), cloud-commitment postures, autoscaling headroom, fallback capacity (to vendor-API when self-host is squeezed; to a lower-tier model when the primary is at capacity), and lead times on widening any of it. The AI Engineering Manager reads it and asks sharp questions but does not author the cells.
- **Gap analysis — joint.** Where will demand outstrip supply? Which features are at risk? Which get a deprioritisation call (routing to mod-103's capability-contingent plan)? Which get contracted out to vendor-API instead of self-host because the capacity ramp does not land in time? Neither team can decide the gap alone.
- **Price projection — joint, with mod-110 as the referee.** The self-hosted cost-per-token Infra is running against the planned mix, versus the vendor-API negotiated cost-per-token for the same mix. This is the number the manager reads when justifying the build-vs-buy call in mod-110 chapter 04. Infra owns the self-host number (they operate the platform); this team owns the vendor-API side (they ran the Procurement negotiation). The projection is dated and deltas from prior quarters are visible.

The AI Engineering Manager does **not** do the Infra planning *inside* the Infra side — they do not pick which H100 cluster to reserve, which autoscaling policy to run, or which cloud provider gets the commitment this quarter. They provide inputs (the demand forecast with honest confidence bands) and consume outputs (the supply plan and gap analysis). Infra mirrors: they do not pick which features make the AI Engineering roadmap or which model a feature routes to. The joint artefact is the surface where the two spheres connect.

Cadence varies by org shape (core concept 6). What matters is that the cadence is named and both teams operate against it.

## Core concept 4 — the shared incident path (R17)

The third shared artefact is the operating protocol when both teams are involved in an in-flight incident. Mod-107 chapter 04 teaches incident command for the AI-shaped surface; this chapter extends it across the two-team boundary. R17 is this curriculum's name for the shared incident path between the AI Engineering team and the AI Infra team, and the shape has four parts.

- **Pager split.** Serving-side incidents — serving-tier SLI breaches, GPU capacity issues, autoscaling failures, serving-stack regressions — page Infra on-call primary with a courtesy ping to this team. Application-side incidents — eval-online quality regressions, retrieval degradation, agent tool-call failures, moderation-classifier spikes (mod-107 chapter 02's catalogue) — page this team's on-call primary with a courtesy ping to Infra. Cross-boundary incidents — a serving-config change that lands as an agent's tool-call latency budget breach; a vendor upgrade that shifts both serving latency and application quality — are **joint from pager-fire**: both primaries are paged simultaneously.
- **Incident Commander selection.** The IC (mod-107 chapter 04) is drawn from the team whose on-call was paged first on a cross-boundary incident; the other team sends a Subject-Matter Lead. When the pager fires on both simultaneously, the IC is drawn from whichever team's SLI showed the leading indicator — not whichever manager happens to be awake. If the leading indicator is ambiguous, there is a pre-agreed default (usually the team whose surface is customer-visible, typically the AI Engineering side). The default is written down before the incident, not negotiated during it.
- **Postmortem ownership.** When both teams were involved, the postmortem is a **joint** write-up. Single-team postmortems for cross-boundary incidents are a tell for a broken interface. The template is mod-107 chapter 05's template extended with two role columns (Infra-side owner, application-side owner) on every action item; action items split by team at close. The mod-104 eval-fixture conversion (mod-107 chapter 06's incident-to-fixture loop) stays on this team's side when the incident surfaced a quality or safety signature; the SLI-monitor conversion stays on Infra's side when the incident surfaced a serving-tier signature neither SLI caught in time.
- **Blameless discipline extended across teams.** The SRE Workbook's blameless-postmortem discipline <!-- needs-research: cite the specific SRE Workbook chapter on blameless postmortem culture --> is an intra-team discipline by default, and the hardest place for it to survive is at the team boundary. Finger-pointing between teams is the dominant failure mode of cross-boundary response. The two managers hold the room against it: the IC frames the discussion around systems-not-people; the two managers co-sign the postmortem; both teams see the same document.

The pattern: *the IC runs the incident; the two managers co-own the system in which the incident runs.*

## Core concept 5 — what this role does NOT do inside Infra's scope

Chapter 07 of this module reinforces the deferral discipline across all partner functions; this chapter is specific to Infra. The AI Engineering Manager at L45 **does not** step into the following.

- **Do not tune vLLM / TensorRT-LLM / SGLang configs.** Serving-stack configuration — speculative decoding, continuous batching, KV-cache sizing, quantisation choice, attention-kernel selection — is `ai-infra-inference-engineer`'s craft. A manager who tunes the serving config themselves creates a single point of failure and crowds the Infra IC off the surface. If the serving tier is underperforming against the shared SLI, the response is a conversation with the Infra manager peer, not a solo-authored config change.
- **Do not plan GPU reservations.** Reservation shapes, cloud-commitment postures, autoscaling policy, and fallback capacity are Infra's craft, informed by this team's demand forecast. A manager who writes the reservation plan themselves misses the platform-depth considerations (cross-feature sharing, bursting economics, lead-times) the Infra peer naturally carries.
- **Do not negotiate directly with the GPU cloud provider.** Cloud-provider negotiation is Infra plus Procurement, running the same partnership shape mod-110 chapter 07 names for vendor contracts. The AI Engineering Manager contributes demand forecast and strategic context; Infra and Procurement run the commercial conversation.
- **Do not dictate the serving stack choice.** Whether the serving tier runs vLLM or TensorRT-LLM or SGLang or a mix is Infra's call, read against the vendor portfolio the AI Engineering team shapes in mod-105 chapter 06. The AI Engineering Manager names the models the team needs to serve (the portfolio); Infra names the stack that serves them.

The deferral is a **boundary, not a wall**. The AI Engineering Manager has a seat at the capacity-prioritisation table, raises sharp questions on Infra's gap analysis, and escalates to the Infra manager peer when a serving-tier posture is blocking a product commitment. What the manager does not do is substitute their judgement for the Infra ladder's craft. Chapter 07 reinforces this.

## Core concept 6 — org-shape variations

The three shared artefacts (SLO, capacity plan, incident path) exist in every org shape, but their cadence and co-authoring mechanics vary. Name which shape the reader's org has before applying the playbook literally.

- **Startup — no dedicated Infra function.** The AI Engineering Manager is also the Infra manager; the role runs **both** operating systems. The three shared artefacts collapse into a single set the manager co-authors with their own staff-plus ICs. This module still names the boundary explicitly so that when Infra is hired as a separate function, the manager knows which half delegates and which half stays. Do not skip the interface naming because you currently operate both sides; the interface is what makes the eventual delegation clean.
- **Central-platform shape.** Infra is a shared service across multiple AI engineering teams. The AI Engineering Manager contributes demand forecast and consumes capacity like any other customer, but has a **seat at the capacity-prioritisation table** — Infra's quarterly prioritisation across AI engineering teams needs each manager's forecast and strategic context. The joint SLO is still co-authored, but the manager's share of attention is proportional to their team's share of the capacity. The shared incident path runs across more than two teams; the Incident Commander selection rule has to name which AI engineering team is the customer-visible surface when multiple are affected.
- **Dedicated-pair shape.** One Infra team paired to one AI engineering team. This is the tightest shape; the cadence of the joint capacity plan is **weekly or fortnightly**, not quarterly. The joint SLO is live-reviewed; the shared incident path is practised regularly. The failure mode is over-coupling — the Infra team's roadmap becomes the AI Engineering team's capacity plan with no slack for Infra's own platform work. The manager's discipline is to respect the Infra team's standalone roadmap even while co-authoring the shared artefacts.

A startup-single-manager who runs the full dedicated-pair cadence against themselves will burn out. A central-platform manager who demands dedicated-pair cadence from a platform team serving six AI engineering teams will not get it. The playbook composes against the shape, not the reverse.

## What good looks like

A team has a healthy interface with `ai-infra-team-lead` and the Infra ladder when:

- A written, dated **end-to-end SLO** exists for every user-facing AI feature; the SLO decomposes into a named serving-side SLI and a named application-side SLI with an error-budget allocation co-signed by both managers.
- The **quarterly capacity plan** is a joint document with demand-forecast, supply-plan, gap-analysis, and price-projection sections, dated and current within the cadence the org-shape requires.
- Every roadmap row (mod-103) that depends on serving-tier capacity carries an explicit capacity ask that has been read into the Infra supply plan.
- The **shared incident path (R17)** is written down: pager split by incident shape, Incident Commander selection rule, Subject-Matter Lead expectation from the non-paged side, joint postmortem template extending mod-107 chapter 05.
- Both teams' on-call rotations have exercised the cross-boundary shape at least once in the last two quarters — a joint tabletop or a real cross-boundary incident with a dated postmortem.
- The error-budget conversation at each operating review names both sides' contributions and the shared bucket; both managers walk in with the same numbers.
- The deferral boundary is visible in the operating artefacts: the AI Engineering Manager has no line on serving-stack tuning, the GPU reservation plan, or the cloud-provider negotiation; the Infra Manager has no line on the AI Engineering team's model mix, prompt complexity, or eval surface.

## What bad looks like

- The team's interface to Infra is a Jira queue; the AI Engineering Manager has no running conversation with the Infra manager peer; "shared SLO" is a phrase nobody has used in six months.
- The serving-tier SLI reads green, the application-side SLI reads green, and the user-facing SLO has been burning for three weeks — no dashboard shows the compound.
- The quarterly roadmap contains a feature whose capacity ask was never filed with Infra; the manager discovers the gap six weeks before launch.
- Cross-boundary incidents are run by whichever team happened to be paged first; the Incident Commander has no Subject-Matter Lead from the other side; mean-time-to-mitigate drifts.
- Postmortems from cross-boundary incidents are single-team; the other team's action items are missing or added later by the first team guessing at the other team's workload.
- The AI Engineering Manager has personally tuned the vLLM config, written the GPU reservation plan, or negotiated directly with the cloud sales representative in the last quarter.
- Finger-pointing between the two teams on an incident call is accepted; neither manager has held the room against it.
- The org-shape was never named; the team is running a central-platform cadence inside a dedicated-pair shape (or vice versa) and the interface is friction.

## Summary

- The AI Engineering Manager is the **customer** of the Infra manager peer (`ai-infra-team-lead`, L45, AI Infra ladder) on serving, capacity, and inference-platform depth; "customer" does not mean "vendor," and the healthy pattern is three shared artefacts co-authored by both managers.
- The three shared artefacts are the **end-to-end SLO per user-facing feature** (decomposed into serving-side and application-side SLIs with a joint error-budget allocation), the **quarterly joint capacity plan** (demand forecast, supply plan, gap analysis, price projection), and the **shared incident path R17** (pager split, Incident Commander selection, Subject-Matter Lead expectation, joint postmortem).
- The deferral boundary is explicit: the AI Engineering Manager does not tune serving stacks, plan GPU reservations, negotiate with cloud providers, or dictate the serving-stack choice; these are Infra's craft, informed by this team's demand forecast and strategic context.
- Org-shape (startup-single-manager, central-platform, dedicated-pair) sets the cadence and co-authoring mechanics of the three shared artefacts; the artefacts exist in every shape, but the rhythm varies. Name the shape before applying the playbook.
- A team runs a healthy Infra interface when both managers walk into the operating review with the same numbers, the error-budget conversation names both sides, and the 2am cross-boundary incident has a named Incident Commander and a named Subject-Matter Lead from the other side before the pager fires.

The next chapter opens the interface with security, privacy, and data governance — the partners whose sign-off sits upstream of production launches and whose operating interface runs on a different cadence than the Infra partnership.
