# Exercise 01: Commission an Eval Suite for One AI Feature

**Estimated effort:** 4 hours · **Output:** one commissioning brief (2–4 pages) for one specific AI feature, filled against the eight sections in chapter 02

## Objective

For **one specific AI feature** your team ships or plans to ship, produce the written commissioning brief from chapter 02 — not as a generic template, but filled in with this feature's product surface, baseline, risk posture (pinned to NIST GenAI Profile risk categories and OWASP LLM Top 10 categories), six eval-category plan, dataset plan, tool choice, thresholds, and ownership. By the end of the exercise an `ai-eval-engineer` on your team (or a counterpart at a peer organisation) should be able to start building the suite from your brief on Monday morning without needing to ask you follow-up questions.

## Prerequisites

- Chapter 01 (`The Manager's Altitude for Eval-Driven Delivery`) for the frame of what you own vs. what the IC owns.
- Chapter 02 (`Commissioning an Eval Suite`) for the eight sections of the brief.
- Chapter 04 (`The Three Registries`) at a skim — the brief names which registries and tool choices feed them.
- One feature in mind with enough detail that you can fill in the sections. Three usable patterns:
  - A feature your team currently ships or plans to ship in the next quarter.
  - A feature at a prior employer you can describe honestly, with the numbers abstracted where needed.
  - A hypothetical feature for a composite team — in which case write the feature charter yourself and mark numbers `[placeholder: measure before quoting]`.

## Problem statement

Write a commissioning brief, 2–4 pages of markdown, that answers each of the eight sections below in order. No section is optional; a section that is genuinely out of scope gets a one-sentence justification in place of the content.

### Section 1 — Product surface and audience (15 minutes)

One paragraph. What the feature does, who uses it, what shape of input it accepts, what shape of output it produces. If it is tool-using, name the tools. If it is retrieval-augmented, name the corpus and roughly its size. If it takes long inputs, name the modal and p99 input length. This paragraph is the root the rest of the brief refers back to — write it tightly.

### Section 2 — Baseline (15 minutes)

Name what the new system's quality is measured *against*. Pick explicitly from (or state that none of these apply and name your own):

- A production system being replaced.
- A no-X ablation (the same system with capability X removed).
- A published benchmark (name the benchmark version).

State the reason your baseline is defensible. A brief that skips this section produces a suite whose numbers are uninterpretable.

### Section 3 — Risk posture and the governance pins (30 minutes)

Pin specific risk categories from the published frameworks:

- **NIST AI RMF 1.0 Generative AI Profile (AI 600-1)** — list the GenAI Profile risk categories that apply to this feature (confabulation, content-integrity, data-leakage, privacy, dangerous recommendations, bias, etc.). For each, one-line justification.
- **OWASP Top 10 for LLM Applications** — list the categories that apply (prompt injection LLM01, sensitive-information disclosure, excessive agency, etc.). Pin the OWASP version you are measuring against (e.g., "2025 top ten"). For each listed category, one-line justification.
- If the feature is regulated — EU AI Act high-risk, HIPAA, GLBA, ITAR, PCI — name the regulation, the specific article or control if known, and who at the organisation holds the compliance call.

Produce a two-column table: left column, risk category; right column, case-family identifier (which you will fill in section 4) or "not meaningfully exposed because …".

### Section 4 — The six eval categories (45 minutes)

For each of the six categories, state: whether this feature needs coverage, and if so, the first-revision case count, the dataset source, and the IC owner (named human). The six:

1. Behavioural / task-success.
2. Judge-based quality (faithfulness, relevance, tone, hallucination, instruction-following — pick which apply).
3. Safety (the categories pinned in section 3).
4. Retrieval quality (if the system is RAG).
5. Tool-use correctness (if the system uses tools).
6. Cost and latency.

For any category you drop, write a one-sentence justification. Do not delete the heading.

### Section 5 — The dataset plan (30 minutes)

Name:

- The **golden dataset** — source (hand-authored, production-sampled, external corpus), size, who owns the refresh, PII posture.
- The **production trace sample** — when it populates (post-launch cadence), sampling policy, PII posture.
- The **adversarial / red-team corpus** — initial sources (OWASP reference examples, MLCommons AILuminate hazard categories, internal red-team), size, refresh cadence.
- The **holdout** — size, who holds access, the access policy (write-only during iteration).

Every dataset carries: provenance, PII / privacy posture, retention, refresh cadence, owner.

### Section 6 — The tool choice (20 minutes)

Pick **one** eval harness and **one** observability store. For each, write one paragraph explaining the choice against the alternatives, and name the exit criteria (what would cause you to re-evaluate the choice). Alternatives to be aware of:

- Eval harness: `OpenAI Evals`, `Inspect`, `promptfoo`, `Ragas`, `DeepEval`.
- Observability / online evals: `LangSmith`, `Langfuse`, `Braintrust`, `Arize` (and `Phoenix` as the OSS companion).

Pin the tool versions, and state whether you are using SaaS, self-host, or hybrid. Note the privacy posture (what leaves the network).

### Section 7 — Thresholds (30 minutes)

State thresholds as numbers with:

- The PR-gate thresholds (the subset — see exercise 02 for the depth).
- The release-gate thresholds per eval category.
- The regression allowance per metric (with a confidence posture — point estimate or CI bound).
- The absolute floors — safety, privacy, cost ceilings. Mark which are non-overridable.

Where a threshold is genuinely not yet known, use `[placeholder: measure before quoting]` and name the action that will produce the value.

### Section 8 — Ownership and the quarterly review (15 minutes)

Three named humans:

- The **suite owner (IC)** — usually an `ai-eval-engineer` or `applied-ai-engineer`.
- The **judge owner (IC)** — if an LLM-as-judge is in the suite.
- The **review owner (manager)** — you.

Plus the quarterly-review cadence (date / ritual), and the memo-shape the review produces.

## Requirements

- All eight sections present; no section is silently skipped.
- Section 3's risk-category table has at least one row from NIST GenAI Profile and at least one row from OWASP LLM Top 10 — or an explicit, justified "not applicable" entry for each pinned framework.
- Section 4 either names a case family (with owner and date) or justifies the category as out of scope. Generic "we will add safety evals later" fails the exercise.
- Section 6 picks exactly one eval harness and exactly one observability store. Teams running two overlapping tools fail; teams that have not yet picked fail.
- Section 7 carries numbers, not adjectives. Where a number is unknowable at commissioning, `[placeholder: measure before quoting]` + an action is the required substitute.
- Section 8 names three humans by name (or `[placeholder: named at team formation]`, with a date). "The team" is not an owner.
- The brief fits in 2–4 pages of markdown. Longer briefs are commonly less useful than shorter ones.

## Starter guidance

- Begin with sections 1 and 2 (product surface and baseline). These are the roots the rest grow from, and they tend to surface disagreements with your IC counterparts early.
- The risk-posture table (section 3) is the most-skipped section on teams new to governance frameworks. Open the NIST GenAI Profile document and the OWASP Top 10 for LLM Applications side-by-side and work the table row by row. For most generative features, confabulation (NIST) and LLM01 Prompt Injection (OWASP) will apply; the exercise is deciding what else does.
- When picking the tool (section 6), do not let "the current team is familiar with X" drive the choice alone. Familiarity matters, but so does fit — `Ragas` is a better default for retrieval-heavy surfaces regardless of familiarity; `Inspect` is a better default for agent surfaces regardless of familiarity. Pick fit; train on it.
- Thresholds (section 7) are easier to write if you borrow an initial floor from a comparable surface in your organisation and widen the regression allowance for the first quarter (your team will not be good at estimating at first). Chapter 02 of mod-103 covers calibration-across-time for similar artefacts.
- If your feature is pre-launch, the baseline (section 2) is often a no-X ablation or a published-benchmark proxy. Do not default to "no baseline — we're new"; that is how uninterpretable suites happen.
- The hardest conversation this brief forces is section 8. If no one on the team is available to own the suite long-term, that is a hiring or capacity signal. Surface it to leadership now rather than six months in.

## Acceptance criteria

- Brief is 2–4 pages of markdown, all eight sections present and filled.
- Risk-posture table (section 3) has pinned NIST GenAI Profile and OWASP LLM categories, each row either mapping to a planned case family or carrying an "N/A because…" justification.
- Each of the six eval categories (section 4) either has a plan (case count, data source, owner, date) or a justified exclusion.
- Dataset plan (section 5) names golden / trace-sample / adversarial / holdout, each with provenance, PII posture, and owner.
- Tool choice (section 6) picks one eval harness and one observability store; tools are version-pinned; exit criteria named.
- Thresholds (section 7) are numbers (or `[placeholder: …]` with an action), with confidence posture, regression allowance, and absolute floors marked.
- Ownership (section 8) names three humans and a quarterly review cadence.
- A peer AI Engineering Manager can read the brief and either (a) ask three sharp questions, or (b) hand it to their eval engineer as a start-of-week kickoff doc without editorialising.

## Stretch goals

- **Two-feature crosswalk.** Fill in a second brief for a feature with a different shape — e.g., a doc-Q&A agent and a code-generation assistant, or a RAG summariser and a tool-using scheduling agent. The two briefs should diverge in interesting ways: section-3 risk pins, section-4 category coverage, and section-6 tool choice should look different. Compare side-by-side; produce a one-page memo on what varies and why.
- **Hand it off.** Hand the brief to a willing eval-engineer colleague (or a peer manager playing the eval engineer) and run a 45-minute kickoff. Write a 10-line retrospective on which section was most-contested in the kickoff. Refine the brief with their feedback.
- **Template extraction.** After the specific brief is done, extract a generic template version — all the section headings, all the placeholders, none of the feature-specific content — and publish it in your team's handbook as the starting point for future features. Chapter 02's eight sections are what you already have; your value-add is the exact placeholders your organisation needs for sections 3, 6, 7, and 8.
