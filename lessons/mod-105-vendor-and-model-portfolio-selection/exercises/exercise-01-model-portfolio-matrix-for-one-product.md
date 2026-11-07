# Exercise 01: Model Portfolio Matrix for One Product

**Estimated effort:** 3 hours · **Output:** (a) a 1–2 page portfolio inventory for one product with at least five rows, and (b) a 2-page selection memo for one specific feature of that product that commits it to a primary and a fallback.

## Objective

Produce the two artefacts chapters 02 and 03 commission — for **one real product** you ship or plan to ship:

1. The **portfolio inventory**: a table of every (model, provider, hosting-path) triple the product is permitted to call from production, filled along the ten dimensions chapter 02 names.
2. The **selection memo** for one specific feature: a two-page markdown document that commits that feature to a named primary model and a named fallback, with explicit weights, thresholds, and eval evidence against the four axes (cost, latency, quality, safety).

By the end of the exercise a peer AI Engineering Manager should be able to open the memo and either (a) ask three sharp questions about the trade-off, or (b) sign it as-is without editorialising.

## Prerequisites

- Chapter 01 (`The Manager's Altitude for Vendor and Model Portfolio`) for the manager/IC split and the four manager-owned artefacts.
- Chapter 02 (`The Frontier Portfolio Landscape`) for the four provider categories and the ten dimensions of an inventory row.
- Chapter 03 (`Per-Feature Model Selection and the Selection Memo`) for the two-page memo shape and the four-axis trade-off.
- Chapter 02 of mod-104 (`Commissioning an Eval Suite`) at a skim — the memo cites eval evidence, so you should know the shape of the eval suite you are citing.
- A real product you have enough context on to fill in the artefact. Three usable patterns:
  - A product your team currently ships.
  - A product at a prior employer with numbers abstracted where needed.
  - A hypothetical product for a composite team — in which case write the product charter yourself and mark numbers `[placeholder: measure before quoting]`.

## Problem statement

Two artefacts, written in markdown, filed in a location your team's governance surface would accept.

### Artefact A — the portfolio inventory (1–2 pages, 60 minutes)

A table with at least **five rows**, drawn from at least **two provider categories** (chapter 02 core concept 1). Each row carries the ten dimensions from chapter 02 core concept 2:

1. Identifier and version (specific snapshot, not family)
2. Context window (input / output separately)
3. Pricing (input / output / cached / batch / fine-tune where offered)
4. Latency profile (TTFT p50 / p95, TPS at modal input)
5. Throughput / rate limits
6. SLA posture
7. DPA posture (training-on-data, retention, residency, sub-processors)
8. Enterprise features (SSO, SOC 2, HIPAA BAA, audit logs, CMK, VPC)
9. Capability tags
10. Eval evidence (run id, date, headline numbers)

A cell whose value is not yet known is `[placeholder: source X by date Y]`, not blank. A cell whose value is "not applicable" says so explicitly.

The table should include at least one row where **the same model via a different hosting path** appears as a separate row (e.g. Anthropic direct and Anthropic via AWS Bedrock; OpenAI direct and OpenAI via Azure). This is the discipline from chapter 02 that most teams miss.

Date the inventory. Name the owner. Note the next review date.

### Artefact B — the selection memo (2 pages, 90 minutes)

Pick **one specific feature** of the product. Write the two-page selection memo from chapter 03, with the six sections:

1. **Feature profile** (one paragraph). What the feature is, who uses it, the shape (interactive / batch / agent / RAG / mixed), the volume projection, the latency target, the audience and the risk posture (one-line pin to the mod-104 commissioning brief or to the risk categories that would have been pinned).
2. **Candidates considered** (half a page). Three candidates minimum, from at least two providers or hosting paths. One-sentence note per candidate on fit; one-sentence exclusion for candidates ruled out on a non-negotiable (no EU residency, wrong capability tag, consumer-tier-only, etc.).
3. **Trade-off with weights, thresholds, evidence** (half a page). The four-axis table. For each axis, weight (low / medium / high / blocking), threshold (number), evidence (eval run id + date, or `[placeholder: run to be commissioned]` with an action). Weights not equal by default; justify in one sentence each.
4. **Recommendation** (two short paragraphs). Primary and fallback named; direct read of the trade-off table.
5. **Exit criteria** (one paragraph). Specific numbers against the eval surface or specific contract events; "if things change" fails.
6. **Ownership and review** (one paragraph). IC owner, manager owner (you), next review date.

### Remaining time (30 minutes)

Cross-reference the two artefacts. Every candidate in the memo's section 2 should be a row in the inventory; the DPA posture, pricing, and capability tag claims in the memo should match the inventory. If they don't match, update the inventory; the memo cites, it does not invent.

## Requirements

- Inventory has at least 5 rows and spans at least 2 of the 4 provider categories (frontier closed, frontier open-weights, enterprise / specialist, inference platforms and clouds).
- At least one case of the same model appearing as multiple rows via different hosting paths with different DPA / residency / SLA profiles.
- Every row carries all 10 dimensions, with `[placeholder: …]` or "not applicable" in any empty cell.
- Selection memo's section 2 names three candidates from at least two providers / hosting paths.
- Section 3's four axes carry numbers for thresholds, not adjectives. "High quality" fails; "Ragas faithfulness ≥ 0.85" passes.
- Section 5's exit criteria are specific numbers or specific contract events; "if things change" fails.
- Section 6 names humans by name (or `[placeholder: named at team formation]` with a date). "The team" is not an owner.
- The inventory row for primary and fallback matches what the memo cites (DPA posture, pricing, capability tags).

## Starter guidance

- Start with the inventory. The memo cites rows from it; building the memo before the inventory means inventing numbers twice.
- For a product whose eval suite does not yet exist (cold-start feature), use `[placeholder: run to be commissioned, due YYYY-MM-DD]` in the evidence cells and in the memo's section 3. This is the mod-104 handshake — you cannot defend a selection memo on a feature without an eval, but you can commit to one.
- For the same-model-different-path row, the easiest example is Anthropic direct vs. Anthropic via AWS Bedrock vs. Anthropic via Google Cloud Vertex AI; OpenAI direct vs. Azure OpenAI; or Meta Llama via self-host vs. via Together vs. via AWS Bedrock. Pick whichever is realistic for your product.
- Section 3 of the memo is the section where reviewers find the most mistakes. Write the weights first, then the thresholds, then pull the evidence. Doing it in the other order (writing what the evidence says then back-fitting the weights) is how cost tunnel-vision and latency tunnel-vision sneak in.
- The memo's section 2 is the second place reviewers push back. Excluded candidates with a one-sentence exclusion are stronger than candidates silently omitted — if you dropped Gemini because procurement cycle time killed it this quarter, say so.
- Pin specific snapshots in the inventory. If the current inventory practice at your organisation is to name families ("Claude", "GPT-4"), the exercise is to upgrade to snapshots ("claude-sonnet-[snapshot]", "gpt-4o-[snapshot]").
- Prices move; use `[placeholder: current price per provider's pricing page on YYYY-MM-DD]` liberally rather than quoting a number you can't verify.

## Acceptance criteria

- Portfolio inventory is 1–2 pages, ≥5 rows, ≥2 provider categories, same-model-different-path case included, all 10 dimensions present per row.
- Selection memo is 2 pages, all six sections present and filled.
- Memo's four-axis table carries weight / threshold / evidence per axis; thresholds are numbers (or `[placeholder: …]` with an action).
- Primary and fallback both appear in the inventory with matching DPA posture.
- Exit criteria are specific numbers or specific contract events.
- Named humans (or placeholders with dates) in the ownership section.
- Inventory and memo are internally consistent — numbers in the memo match rows in the inventory.
- A peer AI Engineering Manager can read both and either ask three sharp questions or sign the memo as-is.

## Stretch goals

- **Multi-feature portfolio.** Fill in selection memos for a second and third feature of the same product, with deliberately different shapes (one batch, one interactive; one RAG, one agent). The three memos should point to overlapping but not identical primary/fallback pairs. Write a one-page memo on which cross-feature standardisations are efficient and which would be counter-productive.
- **Routing policy prototype.** For the chosen feature, extend the selection memo with a routing-policy section: default (quality-first), and one specific sub-case where cost-first, latency-first, or capability-first routing applies (chapter 04 preview). Name the classifier (rule or model-based) and the routing-quality eval the classifier itself needs.
- **Inventory template.** After the specific inventory is done, extract a generic template version — headers, column definitions, format conventions — and publish it as the starting point for future product onboardings in your team's handbook. The exercise is to make sections 3 (pricing) and 7 (DPA) have placeholders that match your organisation's standard questions.
- **Inventory automation sketch.** Write a one-page description of how the inventory could partly auto-refresh (provider pricing APIs or docs scraping where available, eval-run-row populated from the eval runner, subscription log pulling deprecation-page RSS). Not an implementation — a specification for someone on the applied-engineering ladder who might later build it.
