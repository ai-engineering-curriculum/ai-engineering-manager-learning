# Resources for mod-103: Roadmapping and Planning Non-Deterministic Work

Curated references for this module. Each entry is a primary or widely-cited source — if you reach for one source to go deeper on an idea introduced in the chapters, start with the entries below. Vendor pages change frequently; where links point to deprecation or release-notes pages, check the publication date and refresh your capacity-and-cadence table against the current content, not the content as it existed when this file was written.

## Planning under uncertainty (chapter 01)

The three-estimate / confidence-band approach has deep roots outside AI; chapter 01 borrows the vocabulary and specialises it.

- Steve McConnell — *Software Estimation: Demystifying the Black Art.* [Publisher page](https://www.microsoftpressstore.com/store/software-estimation-demystifying-the-black-art-9780735605350)
- Todd Little — *Schedule Estimation and Uncertainty Surrounding the Cone of Uncertainty* (IEEE Software, 2006). [IEEE Xplore](https://ieeexplore.ieee.org/document/1637693)
- Douglas Hubbard — *How to Measure Anything: Finding the Value of Intangibles in Business.* [Author site](https://hubbardresearch.com/publications/how-to-measure-anything-book/)
- PERT three-point estimation is the classical statement of P10/P50/P90-style estimation; a readable summary is in the PMI's *Project Management Body of Knowledge* (PMBOK) if your organisation uses it, or in [McConnell's book above].

Product-roadmap thinking that complements the planning-under-uncertainty view:

- Marty Cagan — *Inspired: How to Create Tech Products Customers Love* (2nd ed.) and *Empowered.* [Silicon Valley Product Group](https://www.svpg.com/books/)
- Shreyas Doshi — writing on product strategy, roadmapping, and prioritisation. [Blog / essays index](https://shreyas.co/)

## Release gates, SLOs, and error budgets (chapter 02)

The SLO / error-budget vocabulary is Google SRE canon. The release-gate concept composes SRE discipline with the eval-gated SDLC (`mod-104`).

- Google — *Site Reliability Engineering* (the SRE Book). [Free online — table of contents](https://sre.google/sre-book/table-of-contents/) — chapters 3 (Embracing Risk) and 4 (Service Level Objectives) are the primary source for SLIs, SLOs, and error-budget mechanics.
- Google — *The Site Reliability Workbook.* [Free online — table of contents](https://sre.google/workbook/table-of-contents/) — chapter 2 (Implementing SLOs) and chapter 5 (Alerting on SLOs) are the practical companions.
- Google — *Building Secure and Reliable Systems.* [Free online — table of contents](https://sre.google/books/building-secure-reliable-systems/) — useful for the security / safety cross-cut on release gates.
- Nicole Forsgren, Jez Humble, Gene Kim — *Accelerate: The Science of Lean Software and DevOps Research.* [Publisher page](https://itrevolution.com/product/accelerate/) — the DORA metrics (deployment frequency, lead time, change failure rate, time to restore) are the delivery-side signals that pair with the SLO-side signals.

## Prototype, MVP, and release-vocabulary origins (chapter 02)

- Eric Ries — *The Lean Startup.* [Publisher page](https://theleanstartup.com/book) — origin of the MVP vocabulary.
- Steve Blank — *The Four Steps to the Epiphany* and *The Startup Owner's Manual.* [Steve Blank — books](https://steveblank.com/books/)

## Model cards and documentation (chapter 04)

- Margaret Mitchell et al. — *Model Cards for Model Reporting* (2019). [arXiv:1810.03993](https://arxiv.org/abs/1810.03993)
- Timnit Gebru et al. — *Datasheets for Datasets.* [arXiv:1803.09010](https://arxiv.org/abs/1803.09010) — the companion artefact for training / eval data provenance.
- Inioluwa Deborah Raji et al. — *Closing the AI Accountability Gap: Defining an End-to-End Framework for Internal Algorithmic Auditing* (FAccT 2020). [ACM DL](https://dl.acm.org/doi/10.1145/3351095.3372873) — the broader accountability frame the acceptance gate sits inside.

## Frontier-lab release-safety frameworks (chapter 03)

Read these as planning inputs, not as policies this role owns. Each has been revised since initial publication; always check for the current version.

- Anthropic — Responsible Scaling Policy (RSP). [Policy page](https://www.anthropic.com/rsp)
- OpenAI — Preparedness Framework. [Site](https://openai.com/safety/preparedness/)
- Google DeepMind — Frontier Safety Framework (FSF). [Announcement](https://deepmind.google/discover/blog/introducing-the-frontier-safety-framework/) · [Current framework page](https://deepmind.google/discover/blog/updating-the-frontier-safety-framework/) (check for the latest version).
- METR — *A proposed model for measuring, limiting, and responding to AI capabilities* (research memo on responsible-scaling-policy structure). [metr.org](https://metr.org/)

## Vendor deprecation and release cadence (chapter 03)

Primary docs — refresh your capacity-and-cadence table against the current pages, not the content as it existed when this file was written.

- OpenAI — Deprecations page. [platform.openai.com/docs/deprecations](https://platform.openai.com/docs/deprecations)
- Anthropic — Model deprecations page. [docs.anthropic.com — model deprecations](https://docs.anthropic.com/en/docs/about-claude/model-deprecations)
- Google — Gemini / Vertex AI model lifecycle. [cloud.google.com — Gemini model lifecycle](https://cloud.google.com/vertex-ai/generative-ai/docs/learn/model-versioning)
- Azure OpenAI — Model retirement policy. [learn.microsoft.com — Azure OpenAI model retirements](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/model-retirements)
- AWS Bedrock — Model lifecycle / deprecation. [docs.aws.amazon.com — Bedrock model deprecation](https://docs.aws.amazon.com/bedrock/latest/userguide/model-lifecycle.html)

Release notes to read on cadence:

- Anthropic — Claude release notes. [docs.anthropic.com — release notes](https://docs.anthropic.com/en/release-notes)
- OpenAI — Changelog. [platform.openai.com/docs/changelog](https://platform.openai.com/docs/changelog)
- Google — Vertex AI / Gemini release notes. [cloud.google.com — Vertex AI release notes](https://cloud.google.com/vertex-ai/docs/release-notes)

Pricing pages (the pricing-reset trigger):

- Anthropic — API pricing. [anthropic.com/pricing](https://www.anthropic.com/pricing)
- OpenAI — API pricing. [openai.com/api/pricing](https://openai.com/api/pricing/)
- Google — Gemini API pricing. [ai.google.dev/pricing](https://ai.google.dev/pricing)

## Capacity, team-topology, and the discovery / delivery mix (chapter 05)

- Matthew Skelton and Manuel Pais — *Team Topologies.* [Publisher page](https://teamtopologies.com/book) — the four team types and three interaction modes vocabulary that underpins the discovery / delivery / incident split.
- Marty Cagan and Chris Jones — *Empowered: Ordinary People, Extraordinary Products.* [SVPG](https://www.svpg.com/books/) — the discovery-vs-delivery framing and the "discovery feeds delivery" argument chapter 05 echoes.
- Teresa Torres — *Continuous Discovery Habits.* [Publisher page](https://www.producttalk.org/continuous-discovery-habits/)
- Google — Project Aristotle (team effectiveness) and re:Work guides on psychological safety and team norms — context for how incident capacity and on-call quality affect team health. [re:Work — team effectiveness guide](https://rework.withgoogle.com/en/guides/understanding-team-effectiveness)
- Will Larson — *An Elegant Puzzle* (chapters on capacity planning and roadmap posture). [lethain.com/elegant-puzzle](https://lethain.com/elegant-puzzle/)

## Non-deterministic systems — the engineering frame

- Chip Huyen — *AI Engineering.* [Publisher page](https://www.oreilly.com/library/view/ai-engineering/9781098166298/) — chapters on evaluation and deployment are the technical companion to chapter 02's gate.
- Hamel Husain — *Your AI product needs evals.* [Essay](https://hamel.dev/blog/posts/evals/) — the clearest short argument for eval-gated SDLC that chapter 02's gate depends on.
- Eugene Yan — *Patterns for Building LLM-based Systems.* [Essay](https://eugeneyan.com/writing/llm-patterns/)
- Shreya Shankar et al. — *Who Validates the Validators? Aligning LLM-Assisted Evaluation of LLM Outputs with Human Preferences* (UIST 2024). [arXiv:2404.12272](https://arxiv.org/abs/2404.12272) — technical grounding for the judge-stability and eval-reproducibility criteria in chapter 02 and chapter 04.

## Incident response and postmortems (chapter 02, chapter 05)

Full ownership of this surface is in `mod-107`; these are the starting references.

- Google — *Site Reliability Engineering*, chapter 15 (Postmortem Culture). [sre.google/sre-book/postmortem-culture](https://sre.google/sre-book/postmortem-culture/)
- Etsy Code as Craft — *Blameless Postmortems and a Just Culture.* [Essay](https://www.etsy.com/codeascraft/blameless-postmortems/)
- AI Incident Database. [incidentdatabase.ai](https://incidentdatabase.ai/) — read on a weekly cadence for the incident-pattern landscape.

## Risk-management vocabulary used across chapters

- NIST — AI Risk Management Framework 1.0 (AI 100-1). [PDF](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.100-1.pdf)
- NIST — Generative AI Profile (AI 600-1). [Knowledge base](https://airc.nist.gov/AI_RMF_Knowledge_Base/AI_RMF/Uses/GenAI-Profile)
- OWASP — Top 10 for Large Language Model Applications. [Project page](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- EU AI Act — Regulation (EU) 2024/1689. [Official Journal consolidated text](https://eur-lex.europa.eu/eli/reg/2024/1689/oj)

## Executive-communication artefacts (chapter 01, chapter 03, chapter 05)

Full ownership of this surface is in `mod-112`; these are the starting references for the delta memo, capacity slide, and band-calibration review that chapter 03 and chapter 05 prescribe.

- Barbara Minto — *The Minto Pyramid Principle.* [Publisher page](https://www.pearson.com/en-us/subject-catalog/p/pyramid-principle-the-logic-of-writing-and-thinking/P200000003403) — the standard reference for top-down executive memos.
- Edward Tufte — *The Visual Display of Quantitative Information.* [Publisher page](https://www.edwardtufte.com/tufte/books_vdqi) — relevant to the capacity-mix slide and band-calibration visualisation.
