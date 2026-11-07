# Resources for mod-104: Eval-Driven Delivery and the Eval-Gated SDLC

Curated references for this module. Each entry is a primary or widely-cited source. Vendor and tool pages change frequently; always check publication date and current version against the content as it existed when this file was written.

## Risk-management and safety frameworks (chapters 02, 03, 05)

Primary governance pins the commissioning brief and the release-gate policy depend on.

- NIST — *AI Risk Management Framework 1.0* (AI 100-1). [PDF](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf)
- NIST — *Generative AI Profile* (AI 600-1), AI RMF Knowledge Base. [Page](https://airc.nist.gov/AI_RMF_Knowledge_Base/AI_RMF/Uses/GenAI-Profile)
- OWASP — *Top 10 for Large Language Model Applications*. [Project page](https://owasp.org/www-project-top-10-for-large-language-model-applications/) (pin the version year when referencing in briefs).
- MLCommons — *AILuminate* AI Safety benchmark. [Project page](https://mlcommons.org/benchmarks/ailuminate/) — hazard taxonomy useful for seeding adversarial case families (chapter 02, chapter 07).
- EU AI Act — *Regulation (EU) 2024/1689.* [Official Journal consolidated text](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) — Articles 9 (risk management), 10 (data governance), 14 (human oversight), 15 (accuracy / robustness / cybersecurity), 72 (post-market monitoring), 73 (incident reporting).
- ISO/IEC — *ISO/IEC 42001:2023 — AI Management Systems.* [Standard page](https://www.iso.org/standard/81230.html) — audit-shaped management-system vocabulary your governance peer may invoke.

## Eval harness and observability tooling (chapter 02, chapter 06)

The landscape the manager commissions against. Pin versions and deployment mode (SaaS / self-host) when adopting.

Eval harnesses:

- *OpenAI Evals.* [GitHub](https://github.com/openai/evals) — MIT-licensed reference harness; YAML-first; good for structured-task evals and as a format baseline.
- *Inspect AI* (UK AI Safety Institute). [Docs](https://inspect.aisi.org.uk/) · [GitHub](https://github.com/UKGovernmentBEIS/inspect_ai) — open-source; strong agent + tool-use eval support; solver-based architecture; graders align well with behavioural + safety suites.
- *promptfoo.* [Docs](https://www.promptfoo.dev/docs/intro/) · [GitHub](https://github.com/promptfoo/promptfoo) — fast CI-oriented eval runner; strong caching story; LLM-as-judge support; red-team tooling.
- *Ragas.* [Docs](https://docs.ragas.io/) · [GitHub](https://github.com/explodinggradients/ragas) — RAG-specific; canonical retrieval-eval metrics (faithfulness, answer relevance, context precision, context recall).
- *DeepEval.* [Docs](https://docs.confident-ai.com/) · [GitHub](https://github.com/confident-ai/deepeval) — pytest-integrated eval framework; broad metric library; pairs with Confident AI (SaaS).

Observability / trace storage / online evals:

- *LangSmith.* [Docs](https://docs.smith.langchain.com/) — SaaS, tightly integrated with LangChain / LangGraph; strong on dataset + experiment tracking.
- *Langfuse.* [Docs](https://langfuse.com/docs) · [GitHub](https://github.com/langfuse/langfuse) — open-source, self-hostable; model-agnostic tracing + evals + prompt management.
- *Braintrust.* [Docs](https://www.braintrust.dev/docs) — SaaS; experiment comparison, human-review workflows, dataset management.
- *Arize AI* (and *Phoenix*, the OSS trace viewer / eval harness). [Arize docs](https://docs.arize.com/) · [Phoenix docs](https://docs.arize.com/phoenix) · [Phoenix GitHub](https://github.com/Arize-ai/phoenix) — ML-observability vendor; drift monitoring + online eval; Phoenix is a strong free entry point.

Open standards:

- *OpenTelemetry GenAI semantic conventions.* [Spec](https://opentelemetry.io/docs/specs/semconv/gen-ai/) — the field-naming convention for LLM traces; adopt even if you pick a proprietary store.

Guardrails (referenced across chapters, depth in `mod-106`):

- NVIDIA — *NeMo Guardrails.* [Docs](https://docs.nvidia.com/nemo/guardrails/)
- Meta — *Llama Guard / Purple Llama.* [Project](https://ai.meta.com/research/publications/llama-guard-llm-based-input-output-safeguard-for-human-ai-conversations/)

## Eval engineering practice — primary essays (chapters 02, 03, 04, 06)

Clearest short reads on the eval-gated SDLC as practiced.

- Chip Huyen — *AI Engineering* (O'Reilly 2024). [Publisher page](https://www.oreilly.com/library/view/ai-engineering/9781098166298/) — chapters on evaluation and deployment are the technical companion to this module.
- Hamel Husain — *Your AI product needs evals.* [Essay](https://hamel.dev/blog/posts/evals/) — the clearest short argument for eval-gated SDLC.
- Hamel Husain — *Creating a LLM-as-Judge That Drives Business Results.* [Essay](https://hamel.dev/blog/posts/llm-judge/)
- Eugene Yan — *Patterns for Building LLM-based Systems.* [Essay](https://eugeneyan.com/writing/llm-patterns/)
- Eugene Yan — *LLM-powered Autonomous Agents.* [Essay](https://eugeneyan.com/writing/llm-evaluators/) (companion piece on evaluators).
- Anthropic — *Build with Claude — Prompt Engineering.* [Docs](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview) — vendor reference for prompt discipline the registry will version.
- OpenAI — *Cookbook — evaluations.* [Repo](https://cookbook.openai.com/) — canonical worked examples for the OpenAI Evals format.

## Judge design, calibration, and validation (chapter 04, chapter 07)

Primary references for the judge-stability discipline and rater-calibration work.

- Shreya Shankar, J. D. Zamfirescu-Pereira, Björn Hartmann, Aditya G. Parameswaran, Eugene Wu — *Who Validates the Validators? Aligning LLM-Assisted Evaluation of LLM Outputs with Human Preferences* (UIST 2024). [arXiv:2404.12272](https://arxiv.org/abs/2404.12272)
- Liu, Iter, Xu, Wang, Xu, Zhu — *G-Eval: NLG Evaluation using GPT-4 with Better Human Alignment* (EMNLP 2023). [arXiv:2303.16634](https://arxiv.org/abs/2303.16634)
- Zheng, Chiang, Sheng, et al. — *Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena* (NeurIPS 2023). [arXiv:2306.05685](https://arxiv.org/abs/2306.05685)
- Kim, Shin, Cho, et al. — *Prometheus: Inducing Fine-grained Evaluation Capability in Language Models* (ICLR 2024). [arXiv:2310.08491](https://arxiv.org/abs/2310.08491)
- Joon Sung Park et al. — *Social Simulacra* — useful for synthetic-rater / persona grounding when human raters are scarce. [arXiv:2208.04024](https://arxiv.org/abs/2208.04024)

Inter-rater reliability background:

- Jacob Cohen — *A Coefficient of Agreement for Nominal Scales* (Educational and Psychological Measurement, 1960). [DOI](https://doi.org/10.1177/001316446002000104)
- Joseph Fleiss — *Measuring Nominal Scale Agreement Among Many Raters* (Psychological Bulletin, 1971). [DOI](https://doi.org/10.1037/h0031619)
- Klaus Krippendorff — *Content Analysis: An Introduction to Its Methodology.* [Publisher page](https://www.sagepub.com/books/Book258450) — standard reference for Krippendorff's α.

## Benchmarks and benchmark vocabulary (chapter 02)

Public benchmarks relevant as baselines and as examples of the kind of coverage an eval suite can echo. Avoid using these as the *only* measurement of your product — they measure the model, not your surface.

- Stanford HELM — *Holistic Evaluation of Language Models.* [Project page](https://crfm.stanford.edu/helm/) · [arXiv:2211.09110](https://arxiv.org/abs/2211.09110)
- Hendrycks et al. — *MMLU — Measuring Massive Multitask Language Understanding.* [arXiv:2009.03300](https://arxiv.org/abs/2009.03300)
- Chen et al. — *Evaluating Large Language Models Trained on Code (HumanEval).* [arXiv:2107.03374](https://arxiv.org/abs/2107.03374)
- Lin et al. — *TruthfulQA — Measuring How Models Mimic Human Falsehoods.* [arXiv:2109.07958](https://arxiv.org/abs/2109.07958)
- BIG-bench collaboration — *Beyond the Imitation Game.* [arXiv:2206.04615](https://arxiv.org/abs/2206.04615)
- Chatbot Arena — *LMSYS Chatbot Arena Leaderboard.* [Site](https://lmarena.ai/) — pairwise-preference leaderboard; useful as an external direction-of-travel signal.

RAG-specific benchmarks and references:

- Lewis et al. — *Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks.* [arXiv:2005.11401](https://arxiv.org/abs/2005.11401)
- Thakur et al. — *BEIR — A Heterogeneous Benchmark for Zero-shot Evaluation of IR Models.* [arXiv:2104.08663](https://arxiv.org/abs/2104.08663)

Agent / tool-use benchmarks and references:

- Yao et al. — *ReAct — Synergizing Reasoning and Acting.* [arXiv:2210.03629](https://arxiv.org/abs/2210.03629)
- Liu et al. — *AgentBench — Evaluating LLMs as Agents.* [arXiv:2308.03688](https://arxiv.org/abs/2308.03688)
- Yao et al. — *τ-bench — A Benchmark for Tool-Agent-User Interaction.* [arXiv:2406.12045](https://arxiv.org/abs/2406.12045)

## Data labelling and the flywheel (chapter 07)

Labelling platforms:

- *Argilla.* [Docs](https://docs.argilla.io/) · [GitHub](https://github.com/argilla-io/argilla) — open-source, flexible.
- *Label Studio.* [Docs](https://labelstud.io/) · [GitHub](https://github.com/heartexlabs/label-studio) — open-source, flexible.
- *Prodigy.* [Site](https://prodi.gy/) — commercial, developer-friendly; strong for in-house engineering labelling.
- *Scale AI — Scale Studio / Data Engine.* [Site](https://scale.com/) — vendor labelling; enterprise RLHF / preference pipelines.
- *Surge AI.* [Site](https://www.surgehq.ai/) — vendor labelling; high-skill labour pools.

Preference / fine-tune references (for the Consume station when fine-tune is on the roadmap):

- Christiano et al. — *Deep Reinforcement Learning from Human Preferences.* [arXiv:1706.03741](https://arxiv.org/abs/1706.03741) — origin of preference-data labelling for RL.
- Rafailov et al. — *Direct Preference Optimization.* [arXiv:2305.18290](https://arxiv.org/abs/2305.18290) — DPO; simpler alternative to RLHF for preference-based tuning.
- Bai et al. — *Constitutional AI — Harmlessness from AI Feedback* (Anthropic). [arXiv:2212.08073](https://arxiv.org/abs/2212.08073) — useful frame for safety-tuning data pipelines.

Dataset documentation and provenance:

- Gebru et al. — *Datasheets for Datasets.* [arXiv:1803.09010](https://arxiv.org/abs/1803.09010) — shape of a good dataset-registry entry.
- Mitchell et al. — *Model Cards for Model Reporting.* [arXiv:1810.03993](https://arxiv.org/abs/1810.03993) — model-side companion.
- Pushkarna, Zaldivar, Kjartansson — *Data Cards — Purposeful and Transparent Dataset Documentation for Responsible AI.* [arXiv:2204.01075](https://arxiv.org/abs/2204.01075)

## Release-gate / SRE discipline and testing (chapter 03, chapter 05, chapter 06)

The release gate composes AI-specific discipline with SRE discipline — SLIs, SLOs, error budgets, and canary / shadow / A/B testing.

- Google — *Site Reliability Engineering* (SRE Book). [Free online](https://sre.google/sre-book/table-of-contents/) — chapters 3 (Embracing Risk), 4 (Service Level Objectives), 8 (Release Engineering).
- Google — *The Site Reliability Workbook.* [Free online](https://sre.google/workbook/table-of-contents/) — chapter 2 (Implementing SLOs), chapter 5 (Alerting on SLOs), chapter 16 (Canarying Releases).
- Google — *Building Secure and Reliable Systems.* [Free online](https://sre.google/books/building-secure-reliable-systems/) — security + reliability cross-cut useful on the safety floor.
- Forsgren, Humble, Kim — *Accelerate — The Science of Lean Software and DevOps.* [Publisher page](https://itrevolution.com/product/accelerate/) — DORA metrics (deployment frequency, lead time, change failure rate, time to restore).
- Ron Kohavi, Diane Tang, Ya Xu — *Trustworthy Online Controlled Experiments* (A/B testing; Cambridge University Press 2020). [Publisher page](https://www.cambridge.org/core/books/trustworthy-online-controlled-experiments/D97B26382EB0EB2DC2019A7A7B518F59)
- Georgi Georgiev — *Statistical Methods in Online A/B Testing.* [Book page](https://www.analytics-toolkit.com/statistical-methods-online-ab-testing-ebook/) — accessible reference for A/B thresholds and peeking-is-bad discipline.

## Frontier-lab release-safety frameworks (chapter 02, chapter 05)

Read as planning inputs — these are the gating disciplines at your model providers that shape what lands on your release-gate surface.

- Anthropic — *Responsible Scaling Policy (RSP).* [Page](https://www.anthropic.com/rsp)
- OpenAI — *Preparedness Framework.* [Page](https://openai.com/safety/preparedness/)
- Google DeepMind — *Frontier Safety Framework (FSF).* [Announcement](https://deepmind.google/discover/blog/introducing-the-frontier-safety-framework/)

## Incident corpora (chapter 06)

Read on cadence to see what landed in production this quarter for other teams; feeds the red-team obligation (chapter 07).

- AI Incident Database. [incidentdatabase.ai](https://incidentdatabase.ai/)
- OECD.AI — AI Incidents Monitor. [oecd.ai/en/incidents](https://oecd.ai/en/incidents)
- MIT AI Risk Repository. [airisk.mit.edu](https://airisk.mit.edu/)

## Prompt-engineering, trace design, and related craft (chapter 04, chapter 05)

For the level of fluency a commissioning manager and a code reviewer need — the depth lives in the IC tracks.

- Anthropic — *Build with Claude — Prompt Engineering Overview.* [Docs](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- OpenAI — *GPT Best Practices.* [Docs](https://platform.openai.com/docs/guides/prompt-engineering)
- *Learn Prompting* (community). [learnprompting.org](https://learnprompting.org/)
- Lilian Weng — *Prompt Engineering.* [Essay](https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/)
- OpenTelemetry — *GenAI Semantic Conventions.* [Spec](https://opentelemetry.io/docs/specs/semconv/gen-ai/) (re-listed for chapter 05's trace-discipline references).

## Vendor contracts and DPA posture (chapter 02, chapter 04, chapter 07)

Enterprise vendor references for the "what leaves the network" question when picking an observability platform or a labelling vendor.

- Anthropic — *Data Processing Addendum.* [Page](https://www.anthropic.com/legal/dpa)
- OpenAI — *Data Processing Addendum.* [Page](https://openai.com/policies/data-processing-addendum/)
- Google Cloud / Vertex AI — *Data Processing and Security Terms.* [Page](https://cloud.google.com/terms/data-processing-terms)
- Microsoft Azure OpenAI — *Data, privacy, and security.* [Page](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/data-privacy)

## Where this module reads out to other modules

- `mod-105-vendor-and-model-portfolio-selection` — the model-change sign-off from chapter 05 reads the portfolio inventory you build in mod-105.
- `mod-106-safety-guardrails-and-the-release-triad` — the safety case families you commission in chapter 02 are the measurement side of mod-106's release triad.
- `mod-107-ai-incident-response-and-postmortems` — the pager-firing thresholds and drift-triage runbooks (chapter 06) are mod-107's day-one playbooks.
- `mod-109-team-shape-span-and-ai-operating-cadence` — the quarterly override review (chapter 03), the quarterly flywheel review (chapter 07), and the quarterly registry / cadence reviews (chapters 04 and 06) are beats on the operating-cadence calendar mod-109 designs.
- `mod-110-budget-finops-and-build-vs-buy` — the labelling budget (chapter 07), the release-gate run cost (chapter 03), and the online-eval cost (chapter 06) are line items mod-110 defends.
- `mod-111-cross-functional-partnership` — the release-note content required by R12 (chapter 05) is the evidence mod-111's governance, security, and legal partners ask for.
- `mod-112-executive-communication-and-stakeholder-reporting` — the quarterly review memos this module prescribes are inputs to mod-112's executive-communication artefacts.
