# Resources for mod-102: Hiring, Leveling, and Interview Loops for AI Engineering Roles

Curated references for this module. Each entry is a primary or widely-cited source. The module assumes the general engineering-management canon from mod-101 (Fournier, Grove, Larson, Lopp, Google re:Work, Project Oxygen); this file lists the sources specific to the hiring, leveling, and interview-loop surface.

## Public leveling and career-framework exemplars

Chapter 01 anchors the role matrix in these three; chapter 04 anchors the calibration rubric in them.

- Dropbox — Engineering Career Framework. [Framework site](https://dropbox.github.io/dbx-career-framework/) — the clearest public example of dimensional leveling (project management, technical skills, people, influence) across IC and management tracks.
- levels.fyi — *Google Software Engineering Levels Explained*. [Article](https://www.levels.fyi/blog/google-swe-levels-explained.html) — the public-facing view of the L3 → L8 shape that the L20 → L48 rubric in this module traces.
- GitLab — Engineering handbook. [Handbook — Engineering](https://handbook.gitlab.com/handbook/engineering/) — the handbook-culture discipline of publishing JDs, role expectations, and promotion criteria openly.

## Engineering-management canon on hiring

The general-EM moves for running a hiring loop (scorecards, calibration, references, closing) live here. This module specialises them for AI engineering; the fundamentals are inherited.

- Camille Fournier — *The Manager's Path*. [Publisher page](https://www.oreilly.com/library/view/the-managers-path/9781491973882/)
- Will Larson — *An Elegant Puzzle: Systems of Engineering Management*. [Author site](https://lethain.com/elegant-puzzle/)
- Will Larson — *Staff Engineer: Leadership Beyond the Management Track*. [Author site](https://staffeng.com/book) — essential for chapter 04's staff-plus altitude.
- Michael Lopp — *Rands in Repose* management archive. [Archive](https://randsinrepose.com/archives/category/management/)
- Google re:Work — *Hire*. [re:Work guides — Hiring](https://rework.withgoogle.com/en/guides/hiring-use-structured-interviewing) — the empirical case for structured interviewing that chapter 03's written rubrics inherit.
- Google — *Project Oxygen*. [Guide](https://rework.withgoogle.com/en/guides/managers-identify-what-makes-a-great-manager) — the behavioural framing that chapter 02's JD template inherits.

## AI engineering practice

Chapter 03's rounds (§3 eval design, §5 system design) and chapter 02's artefact bullets assume the reader is fluent in these.

- Chip Huyen — *AI Engineering*. [Publisher page](https://www.oreilly.com/library/view/ai-engineering/9781098166298/)
- Chip Huyen — *Designing Machine Learning Systems*. [Publisher page](https://www.oreilly.com/library/view/designing-machine-learning/9781098107956/)
- Eugene Yan — *Patterns for Building LLM-based Systems and Products*. [Essay](https://eugeneyan.com/writing/llm-patterns/)
- Anthropic — Build-with-Claude engineering docs. [Prompt engineering overview](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- OpenAI — OpenAI Cookbook. [Site](https://cookbook.openai.com/)

## Eval and observability tooling (named in chapter 03 §3)

The round grades on methodology, not tool familiarity. Any of these is a reasonable answer when the candidate speaks the methodology clearly.

- OpenAI Evals. [GitHub](https://github.com/openai/evals)
- Inspect (UK AI Safety Institute). [Docs](https://inspect.aisi.org.uk/)
- promptfoo. [Docs](https://www.promptfoo.dev/docs/intro/)
- Ragas. [Docs](https://docs.ragas.io/)
- DeepEval. [Docs](https://docs.confident-ai.com/)
- LangSmith. [Docs](https://docs.smith.langchain.com/)
- Langfuse. [Docs](https://langfuse.com/docs)
- Braintrust. [Docs](https://www.braintrust.dev/docs)
- Arize Phoenix. [Docs](https://docs.arize.com/)
- OpenTelemetry — GenAI semantic conventions. [Spec](https://opentelemetry.io/docs/specs/semconv/gen-ai/)

## AI risk and regulation vocabulary (§2 model selection, §3 eval design)

Candidates at L30+ should know the vocabulary. Owning depth belongs to the AI Governance family (mod-101 chapter 03).

- NIST — AI Risk Management Framework 1.0 (AI 100-1). [PDF](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf)
- NIST — Generative AI Profile (AI 600-1). [Knowledge base](https://airc.nist.gov/AI_RMF_Knowledge_Base/AI_RMF/Uses/GenAI-Profile)
- OWASP — Top 10 for Large Language Model Applications. [Project page](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- EU AI Act — Regulation (EU) 2024/1689. [Official Journal consolidated text](https://eur-lex.europa.eu/eli/reg/2024/1689/oj) — Articles 9, 10, 14, 15 are the ones the hiring-manager vocabulary meets.

## Vendor contracts and deployment frameworks (§2 model selection)

Chapter 03's model-selection round assumes candidates can engage with DPA-level reasoning. The hiring manager reads these; the depth belongs to counsel.

- Anthropic — Data Processing Addendum (public exemplar). [Legal — DPA](https://www.anthropic.com/legal/dpa)
- Anthropic — Responsible Scaling Policy. [Policy page](https://www.anthropic.com/rsp)
- OpenAI — Preparedness Framework. [Site](https://openai.com/safety/preparedness/)
- Google DeepMind — Frontier Safety Framework. [Announcement](https://deepmind.google/discover/blog/introducing-the-frontier-safety-framework/)

## Incident response canon (§4 non-deterministic debugging)

Chapter 03's debugging round borrows directly from SRE methodology; the AI-specific layer is in mod-107.

- Google — *Site Reliability Engineering* (the SRE Book). [Table of contents](https://sre.google/sre-book/table-of-contents/)
- Google — *The Site Reliability Workbook*. [Table of contents](https://sre.google/workbook/table-of-contents/)
- Etsy Code as Craft — *Blameless Postmortems and a Just Culture*. [Essay](https://www.etsy.com/codeascraft/blameless-postmortems/)

## Public AI incident corpora (§4 non-deterministic debugging)

Interviewers draw §4 scenarios from recent incidents; candidates earn signal by recognising patterns.

- AI Incident Database. [Site](https://incidentdatabase.ai/)
- OECD.AI — AI Incidents Monitor. [Site](https://oecd.ai/en/incidents)
- MIT — AI Risk Repository. [Site](https://airisk.mit.edu/)

## Model documentation (§3 eval design)

Chapter 03's eval-design round can ask candidates to produce a model-card-shaped artefact; this is the reference shape.

- Mitchell et al. — *Model Cards for Model Reporting* (2019). [arXiv](https://arxiv.org/abs/1810.03993)

## Productivity empirical evidence (chapter 02 anti-fluff posture)

Grounds the operating rule that behaviours beat biography when writing JDs and grading loops.

- Dell'Acqua et al. — *Navigating the Jagged Technological Frontier* (HBS Working Paper 24-013). [Working paper page](https://www.hbs.edu/faculty/Pages/item.aspx?num=64700)
- Peng et al. — *The Impact of AI on Developer Productivity: Evidence from GitHub Copilot* (arXiv:2302.06590). [arXiv](https://arxiv.org/abs/2302.06590)

## Paired IC curricula (chapter 01 defers depth here)

The role matrix in chapter 01 **places** shapes; the IC craft is taught in these linked tracks.

- [`applied-ai-engineer`](https://github.com/ai-engineering-curriculum/applied-ai-engineer-learning) — L30
- [`agentic-ai-engineer`](https://github.com/ai-engineering-curriculum/agentic-ai-engineer-learning) — L30
- [`senior-agentic-ai-engineer`](https://github.com/ai-engineering-curriculum/senior-agentic-ai-engineer-learning) — L40
- [`ai-eval-engineer`](https://github.com/ai-engineering-curriculum/ai-eval-engineer-learning) — L30
- [`rag-engineer`](https://github.com/ai-engineering-curriculum/rag-engineer-learning) — L30
- [`fine-tuning-engineer`](https://github.com/ai-engineering-curriculum/fine-tuning-engineer-learning) — L30
- [`llm-application-developer`](https://github.com/ai-engineering-curriculum/llm-application-developer-learning) — L20
- [`agentic-systems-architect`](https://github.com/ai-engineering-curriculum/agentic-systems-architect-learning) — L48, staff-plus
- [`principal-ml-engineer`](https://github.com/ml-engineering-curriculum/principal-ml-engineer-learning) — L48, staff-plus (ML family)
