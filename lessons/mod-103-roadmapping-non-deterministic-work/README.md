# mod-103: Roadmapping and Planning Non-Deterministic Work

> Third module of the AI Engineering Manager track. Builds the planning operating system for a team whose stack moves underneath it: quarterly and annual roadmaps with explicit confidence bands and capability postures, the prototype → MVP → GA gate that governs what ships, capacity paired with provider release cadence so vendor announcements are weather rather than fire drills, the research-to-production handoff contract (R16) with its acceptance gate and decommissioning path, and the discovery / delivery / incident capacity mix with a defence of each bucket.

**Estimated effort:** 18 hours · **Role level:** 45 (AI Engineering family)

## Learning objectives

By the end of this module you should be able to:

- Build quarterly and annual roadmaps for AI product work where ETAs carry explicit confidence bands and the plan assumes model-capability shifts mid-quarter — with named capability postures, pre-booked re-plan rituals, and an annual roadmap shaped as themes-with-confidence rather than twelve months of dated commitments.
- Design the prototype → MVP → GA promotion gate for an AI feature — the five pieces of evidence that open prototype → MVP (eval delta, user-research signal, cost ceiling, safety floor, named owner-of-record and rollback) and the eight pieces that open MVP → GA (production-traffic eval, sustained outcome, GA-scale cost, in-production safety monitoring, CI-enforced release gate, rehearsed incident playbook, on-call readiness, published SLO plus error-budget policy).
- Run capacity planning paired with provider release cadence — pairing the team's capacity blocks with the model deprecations, capability unlocks, pricing resets, and framework updates you can see coming — and read the frontier-lab RSP / Preparedness / FSF as planning inputs rather than as policy this role owns.
- Design the research-to-production handoff (R16) with an acceptance gate across four dimensions (reproducibility, eval scope, documentation, on-call readiness), a formal owner-of-record transition, and a decommissioning path for stalled research tracks.
- Operate the discovery / delivery mix at a manager's altitude — delivery, discovery, incident / regression, and (when applicable) research-handoff-ramp — set percentages from a maturity read and an organisational posture read, defend each bucket against the pressures that recur, and show the mix on one slide leadership can budget against.

## Chapters

1. [Confidence-banded quarterly and annual roadmaps](01-confidence-banded-roadmaps.md) — P10/P50/P90 bands, capability posture, the re-plan ritual, and the annual view as themes with confidence.
2. [The prototype → MVP → GA promotion gate](02-prototype-mvp-ga-promotion-gate.md) — the five-piece MVP gate, the eight-piece GA gate, promotion-vote mechanics, and the anti-patterns that defeat them.
3. [Capacity planning paired with provider release cadence](03-capacity-paired-with-provider-cadence.md) — four shapes of vendor event, the quarterly capacity-and-cadence table, the re-plan ritual and delta memo, and what this role does not own.
4. [Designing the research-to-production handoff (R16)](04-research-to-production-handoff.md) — the four-dimension acceptance gate, owner-of-record transition, decommissioning path, and the right-sizing of the gate by handoff shape.
5. [Operating the discovery / delivery / incident capacity mix](05-discovery-delivery-incident-mix.md) — bucket definitions, setting percentages from two posture reads, defending the mix to leadership, and the quarterly capacity slide.

## Exercises

- [Exercise 01 — Confidence-banded quarterly roadmap](exercises/exercise-01-confidence-banded-quarterly-roadmap.md) *(3 h)*
- [Exercise 02 — Prototype → MVP → GA promotion criteria](exercises/exercise-02-prototype-mvp-ga-promotion-criteria.md) *(3 h)*
- [Exercise 03 — Capacity paired with provider release cadence](exercises/exercise-03-capacity-paired-with-provider-release-cadence.md) *(2 h)*
- [Exercise 04 — Research-to-production handoff contract](exercises/exercise-04-research-to-production-handoff-contract.md) *(3 h)*

Each exercise ships its prompt here; reference solutions live in the paired [`ai-engineering-manager-solutions`](https://github.com/ai-engineering-curriculum/ai-engineering-manager-solutions) repo.

## Supporting material

- `labs/` — long-form hands-on labs (planned).
- `quizzes/` — knowledge checks (planned).
- [`resources.md`](resources.md) — curated external references used across chapters.

## Where this module fits

- **Builds on mod-101 and mod-102.** The handoff-contract map from mod-101 chapter 03 names the roles the promotion vote (chapter 02) and the acceptance vote (chapter 04) draw participants from. The role matrix from mod-102 is where the single human owner-of-record for a research artefact or feature is picked.
- **Feeds mod-104 (eval-gated SDLC) directly.** The release-gate piece of this module's MVP → GA gate is enforced by the CI release gate mod-104 teaches you to build. Chapters 02 and 04 both depend on the eval vocabulary mod-104 formalises.
- **Pairs with mod-105 (vendor and model portfolio).** The capacity-and-cadence table in chapter 03 is the planning artefact mod-105's portfolio inventory feeds. If you have not read vendor release and deprecation pages for the quarter, the capacity table is theatre.
- **Pairs with mod-106 (safety and the release triad).** Chapter 02's safety floor and in-production safety monitoring are sized at the sketch level here; mod-106 is where the full triad vocabulary lives.
- **Pairs with mod-107 (AI incident response).** Chapter 05's incident / regression capacity bucket exists to absorb the incident surface mod-107 teaches you to run. The release gate's "rehearsed incident playbook" and "on-call readiness" items are mod-107's deliverables.
- **Pairs with mod-109 (team shape and operating cadence).** The re-plan ritual (chapter 03), the discovery close-out (chapter 05), and the end-of-quarter calibration review (chapter 01) are beats on the operating-cadence calendar mod-109 designs.
- **Pairs with mod-110 (budget, FinOps, build-vs-buy).** The cost ceiling and GA-scale cost projection (chapter 02), and the pricing-reset trigger (chapter 03), both land in mod-110's cost-review artefacts.
- **Pairs with mod-112 (executive communication).** The delta memo (chapter 03), the capacity-mix slide (chapter 05), and the band-calibration review (chapter 01) are executive-communication artefacts mod-112 teaches you to write and present.
- **Anchors project 102.** This module is the primary source for [`project-102-eval-gated-sdlc-for-one-ai-product`](../../projects/project-102-eval-gated-sdlc-for-one-ai-product), together with mod-104, mod-106, and mod-107.
