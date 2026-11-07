# mod-105: Vendor and Model Portfolio Selection

> Fifth module of the AI Engineering Manager track. Builds the manager-owned operating system around model and vendor choice: a portfolio inventory and per-feature selection memos, a fallback and multi-model routing plan, the Data Processing Addendum posture the team defends on every renewal, an honest open-weights / self-host decision made in partnership with the Infra ladder, and a review cadence (quarterly + mid-quarter rapid-add + decommissioning) that keeps all of the above alive as the frontier moves. The IC craft of benchmarking defers to `ai-eval-engineer` (level 30); the craft of operating inference defers to `ai-infra-inference-engineer` and the peer `ai-infra-team-lead` (level 45, Infra ladder); this module teaches the system you require around both.

**Estimated effort:** 16 hours · **Role level:** 45 (AI Engineering family)

## Learning objectives

By the end of this module you should be able to:

- Run the model + provider portfolio across the current frontier (OpenAI, Anthropic, Google, Mistral, Cohere, Meta open-weights, specialist providers, inference platforms / clouds) along ten standard dimensions — context window, pricing (input / output / cached / batch / fine-tune), latency, throughput, SLA, DPA / SOC 2, data-use policy, residency, enterprise features, capability tags.
- Make and defend model-selection decisions per feature — cost / latency / quality / safety trade-offs, each with explicit weight / threshold / evidence — and commit the decision to a two-page selection memo.
- Design the fallback and multi-model routing plan so the product is not a single point of vendor failure — four fallback shapes, four routing policies, the abstraction layer, the triggers and restore conditions, the data-posture parity requirement, and the tabletop and restore drill that keep it real.
- Partner with procurement on enterprise contracts — read a DPA, know which clauses matter (training-on-data, data retention, residency, sub-processors, deletion rights, SLA, indemnity, deprecation notice, audit rights, term and termination), raise the right redlines, route legal opinion to counsel and contract execution to procurement.
- Position open-weights / self-host options honestly — the four narrow cases where self-host earns its place, the hidden costs that invalidate most self-host spreadsheets, the partnership with `ai-infra-team-lead` and `ai-infra-inference-engineer`, and the managed-open-weights intermediate step that most teams underweight.
- Build the portfolio review cadence — quarterly portfolio review on a six-item agenda, mid-quarter rapid-add path for new capabilities that cannot wait, a decommissioning calendar for deprecated models and vendors, and the discipline of reading Model Cards, system cards, vendor release notes, and deprecation schedules as part of the job.

## Chapters

1. [The Manager's Altitude for Vendor and Model Portfolio](01-the-managers-altitude-for-vendor-and-model-portfolio.md) — what the manager owns (inventory, selection memos, fallback plan, DPA posture, review cadence) vs. what the IC owns (eval craft, serving stack, contract execution); the four manager-owned artefacts; the deferrals to Infra and to Procurement / Counsel.
2. [The Frontier Portfolio Landscape](02-the-frontier-portfolio-landscape.md) — the four categories of provider (frontier closed, frontier open-weights, enterprise / specialist, inference platforms and clouds); the ten dimensions every inventory row carries; the current frontier at commissioning-manager depth.
3. [Per-Feature Model Selection and the Selection Memo](03-per-feature-model-selection.md) — the feature profile that drives selection; the four axes (cost / latency / quality / safety) with weights, thresholds, and evidence; the two-page memo shape; the three anti-patterns to name and defeat.
4. [Fallback and Multi-Model Routing](04-fallback-and-multi-model-routing.md) — the four fallback shapes; the four routing policies; the abstraction layer every call routes through; the triggers and restore conditions; the data-posture parity requirement; the tabletop and the restore drill.
5. [Reading a DPA and Partnering with Procurement](05-dpa-reading-and-procurement-partnership.md) — the clauses that matter; the one-page redline list the organisation defends; the escalation model (counsel / procurement / manager); the renewal and between-renewal cadence.
6. [Open-Weights and Self-Host, Honestly](06-open-weights-and-self-host-honestly.md) — the four narrow cases where self-host earns its place; the hidden costs that invalidate most spreadsheets; the partnership with the Infra ladder; the serving-stack landscape at manager depth; licence reading; the honest memo (which often lands on *prefer API*).
7. [Portfolio Review Cadence, Model Cards, and Release Notes](07-portfolio-review-cadence-model-cards-release-notes.md) — the quarterly review's six-item agenda; the mid-quarter rapid-add path; the decommissioning calendar; Model Cards and system cards; vendor release notes and deprecation schedules; the subscription log.

## Exercises

- [Exercise 01 — Model portfolio matrix for one product](exercises/exercise-01-model-portfolio-matrix-for-one-product.md) *(3 h)*
- [Exercise 02 — DPA reading and redline list](exercises/exercise-02-dpa-reading-and-redline-list.md) *(2 h)*
- [Exercise 03 — Fallback and multi-model routing plan](exercises/exercise-03-fallback-and-multi-model-routing-plan.md) *(2 h)*
- [Exercise 04 — Open-weights self-host build-vs-buy call](exercises/exercise-04-open-weights-self-host-build-vs-buy-call.md) *(3 h)*

Each exercise ships its prompt here; reference solutions live in the paired [`ai-engineering-manager-solutions`](https://github.com/ai-engineering-curriculum/ai-engineering-manager-solutions) repo.

## Supporting material

- `labs/` — long-form hands-on labs (planned).
- `quizzes/` — knowledge checks (planned).
- [`resources.md`](resources.md) — curated external references used across chapters.

## Where this module fits

- **Builds on mod-101 through mod-104.** The release-gate policy and the three registries from mod-104 are the surface the selection memo (chapter 03) and the capability brief (chapter 06) feed. The role matrix from mod-102 is where `ai-eval-engineer`, `applied-ai-engineer`, and the Infra ladder's `ai-infra-team-lead` / `ai-infra-inference-engineer` are defined.
- **Pairs with mod-104 (eval-gated SDLC).** Chapter 03's selection memo cites chapter 02's commissioning brief; chapter 04's online-eval trigger is one shape of chapter 06 of mod-104's cadence; chapter 07's quarterly review consumes the online-eval history mod-104 produces.
- **Feeds mod-106 (safety and the release triad).** Model-choice sign-off is one leg of the release triad; the vendor's Responsible Scaling / Preparedness / Frontier Safety framework is one input mod-106's safety case reads.
- **Feeds mod-107 (AI incident response).** The fallback plan and the tabletop / restore drill (chapter 04) are mod-107's day-one playbooks. A vendor outage is the incident mod-107's runbook responds to.
- **Pairs with mod-110 (budget, FinOps, build-vs-buy).** The cost model in chapter 03 and the self-host cost model in chapter 06 are line items mod-110 defends in the quarterly plan. The build-vs-buy discipline from this module is the AI-specific instance of mod-110's broader frame.
- **Pairs with mod-109 (team shape and operating cadence).** The quarterly portfolio review (chapter 07) is a beat on the operating cadence mod-109 designs. The partnership with the peer AI Infra manager is one of the cross-ladder spans mod-109 draws.
- **Pairs with mod-111 (cross-functional partnership).** The procurement partnership (chapter 05) and the counsel escalation path are two of the cross-functional surfaces mod-111 covers. The redline list is an artefact mod-111's procurement and legal partners read.
- **Pairs with mod-112 (executive communication).** The quarterly-review memo and the self-host capability brief are inputs to executive-communication artefacts mod-112 teaches you to write.
