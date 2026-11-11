# Reading Notes: Agentic Search and Tool Use

**Purpose:** Team literature brief for Thursday's team sync on agentic search — assistants where an LLM-driven agent searches, calls tools, and hands answers to users.

**Repo:** mcpmark-eval-liuhezi/arxiv-agentic-search-notes (branch `add-reading-notes` → `main`)

**Papers covered (3):**
1. **arXiv:2609.19244** — Characterizing Web Search by Conversational LLM Agents: From Search Decisions and Strategies to Results and Responses
2. **arXiv:2603.22862** — The Evolution of Tool Use in LLM Agents: From Single-Tool Call to Multi-Tool Orchestration
3. **arXiv:2502.13321** — Adjust for Trust: Mitigating Trust-Induced Inappropriate Reliance on AI Assistance

Each paper lists: arXiv ID, title, authors, subject category (from the public abs page), submission date, a 2–3 sentence abstract summary, and extracted findings tagged with the section they came from.

**How to comment:** Open a Pull Request against `main` (currently implemented via branch `add-reading-notes`) and leave comments on this file.

---

## Paper 1: arXiv:2609.19244 — Characterizing Web Search by Conversational LLM Agents

- **Title:** Characterizing Web Search by Conversational LLM Agents: From Search Decisions and Strategies to Results and Responses
- **Authors:** Mahsa Amani, Seungeon Lee, Abhisek Dash, Asmaa El Fraihi, Yunah Jang, Elisabeth Kirsten, Qinyuan Wu, Krishna P. Gummadi, Manish Gupta, Abhilasha Ravichander, Muhammad Bilal Zafar, Soumi Das
- **Subject category (arXiv abs page):** cs.AI (primary), cs.IR
- **Submission date (arXiv abs page):** Wed, 16 Sep 2026 17:56:32 UTC (v1)
- **Abstract summary:** This is the first study of Web search across four major conversational platforms (ChatGPT, Claude, Grok, DeepSeek), combining real-world user interactions (invivo) with controlled API experiments (invitro). The authors investigate when agents decide to call Web search, how they formulate queries, the domain preferences in results, and how they transform results into grounded responses. They find that search decisions vary substantially across platforms and models, more frequent Web-search invocation does not guarantee better response quality, and a non-trivial fraction of claims rely on uncited search results.

### Extracted findings

- **Finding 1 — More frequent Web search does not equal better answers.** LLM agents are good at deciding when they do *not* need Web search, but the decisions to call Web search do not always improve responses. Search calling varied substantially across platforms (ChatGPT 42,273 of 143,730 turns had Web search vs. ~1,696 for Claude, 3,004 for Grok, 1,730 for DeepSeek) even though all four platforms show a rising trend in search-calling over time. *(Source: §1 Introduction / Key Findings #1)*
- **Finding 2 — Platform-specific search engines return results with strong domain biases.** The agents' querying strategies differ across platforms, but the underlying search engines return a limited number of results with strong preferences (biases) toward specific domains, meaning Web-grounded responses can reflect proprietary source preferences rather than a comprehensive view of the Web. *(Source: §1 Key Findings #2; §12.3 "Domain Preferences in Search Results")*
- **Finding 3 — Citation/attribution problem: some claims are ungrounded.** LLM agents exhibit strong preferences toward popular/cited URLs, but a non-trivial fraction of claims in responses are attributed to returned search URLs that are not cited; the factuality of ungrounded claims (likely from the model's parametric knowledge) remains poor. *(Source: §1 Key Findings #3; §13.2 "Grounding of Claims")*
- **Limitation (acknowledged by authors):** The analysis relies on observational conversational traces that do not expose the full internal reasoning (chain-of-thought), ranking algorithms, or proprietary retrieval pipelines, so some observed behaviors may reflect harness implementations rather than the underlying models. The study is also English-language only and does not analyze multimodal retrieval, non-English retrieval, or downstream user-trust effects. *(Source: §15 "Limitations")*
- **Design implication for our team:** When fine-tuning or prompting search agents, weighting search-call frequency alone is not a good quality proxy; evaluate grounding/citation rates and domain-preference behavior explicitly.

---

## Paper 2: arXiv:2603.22862 — The Evolution of Tool Use in LLM Agents

- **Title:** The Evolution of Tool Use in LLM Agents: From Single-Tool Call to Multi-Tool Orchestration
- **Authors:** Haoyuan Xu, Chang Li, Xinyan Ma, Xianhao Ou, Zihan Zhang, Tao He, Xiangyu Liu, Zixiang Wang, Jiafeng Liang, Zheng Chu, Runxuan Liu, Rongchuan Mu, Dandan Tu, Ming Liu, Bing Qin
- **Subject category (arXiv abs page):** cs.SE (primary), cs.CL
- **Submission date (arXiv abs page):** v1 Tue, 24 Mar 2026 07:05:05 UTC (v2 Thu, 2 Apr 2026 02:54:00 UTC)
- **Abstract summary:** This survey argues the central problem in LLM agents has shifted from whether a model can make a correct single tool call to multi-tool orchestration over long trajectories with intermediate state, execution feedback, and constraints like safety, cost, and verifiability. It unifies task formulations, organizes the literature around six core dimensions (inference-time planning/execution, training/trajectory construction, safety/control, efficiency under resource constraints, capability completeness in open environments, and benchmark design/evaluation), and summarizes applications in software engineering, enterprise workflows, GUIs, and mobile systems.

### Extracted findings

- **Finding 1 — The paradigm has shifted from call-level correctness to system-level orchestration.** The survey's own framing: modern evaluation moved "from isolated API invocation correctness to assessing system-level intelligence in sustaining, adapting, and repairing extended tool-use trajectories under structural, temporal, and environmental constraints." Long-horizon orchestration problems include dynamic tool subset selection, cross-tool dependency modeling, sequential/parallel scheduling, failure recovery, and re-planning. *(Source: §1 Introduction; §8 "Benchmarks")*
- **Finding 2 — Four interdependent dimensions define long-horizon intelligence.** Contemporary benchmarks prioritize (1) topological complexity of tool dependencies, (2) temporal scale of execution trajectories, (3) environmental dynamism during interaction, and (4) state persistence with self-correction mechanisms. Early benchmarks like API-Bank, MetaTool, and ToolBench established intent detection and tool retrieval, but current research addresses the four dimensions above. Representative benchmarks surveyed include ELT-Bench, Toolcomp, FuncBenchGen, MSCoRe, ToolSword (with sub-dimensions: topological complexity, temporal scale, dynamic environments, state persistence/self-correction), and OSWorld, GTA, CRITIC-TOOL, MemAgentBench, ColorBench, ASTRA, and RoTBench. *(Source: §8 "Benchmarks"; Appendix subsections §29.1–29.4 "ToolSword" breakdown)*
- **Limitation / open challenge (acknowledged by authors):** The paper is a survey that organizes the field and identifies a research agenda; it notes that planning, training, safety, and efficiency are typically studied separately even though deployed agent systems depend on their interaction, and flags future directions for reliable, scalable, and verifiable multi-tool agents. *(Source: §1.2 "Aims and Motivations"; §30.3 "Open Problems and Future Directions")*
- **Design implication for our team:** When we evaluate any tool-using assistant, report on orchestration quality (topology, temporal scale, state persistence) rather than per-call accuracy; mark failures and measure cost/latency.

---

## Paper 3: arXiv:2502.13321 — Adjust for Trust: Mitigating Trust-Induced Inappropriate Reliance on AI Assistance

- **Title:** Adjust for Trust: Mitigating Inappropriate Reliance on AI Assistance
- **Authors:** Tejas Srinivasan, Jesse Thomason
- **Subject category (arXiv abs page):** cs.HC (primary), cs.AI, cs.CL
- **Submission date (arXiv abs page):** v1 Tue, 18 Feb 2025 22:42:39 UTC (v2 Mon, 26 Jan 2026 16:35:39 UTC)
- **Abstract summary:** Trust biases users toward either under-reliance (low trust) or over-reliance (high trust) on AI advice. The authors propose that AI assistants should adapt their behavior via trust-adaptive interventions: supporting explanations when trust is low, counter-explanations when trust is high, and forced pauses to promote deliberation. In two scenarios (laypeople answering science questions and doctors making diagnoses), trust-adaptive interventions yielded up to a 38% reduction in inappropriate reliance and a 20% improvement in decision accuracy, and forced pauses reduced over-reliance but not under-reliance. A real-LLM generalization and interaction-feature trust estimation were also tested.

### Extracted findings

- **Finding 1 — Extreme trust causes measurable inappropriate reliance (quantified).** Medical doctors mistakenly accepted 26% of AI misdiagnoses when their trust in the assistant was high, compared to 8% when trust was lower (over-reliance); conversely, when trust was low, doctors rejected correct AI diagnoses 68% of the time vs. 40% otherwise (under-reliance). Trust at the start of an interaction strongly correlated with reliance behavior across both study tasks. *(Source: §1 Introduction / miscalibrated-trust examples)*
- **Finding 2 — Trust-adaptive interventions work: up to 38% reduction in inappropriate reliance and 20% accuracy gains.** Supporting explanations at low-trust moments and counter-explanations at high-trust moments reduced both under- and over-reliance in simulated AI assistance; forcing deliberation via inserted pauses reduced over-reliance but did *not* reduce under-reliance. *(Source: Abstract; §10 "Results"; §11 "Results" — LLM-assistant generalization)*
- **Finding 3 — Findings generalize to a real LLM assistant.** The authors report that the trust-adaptive strategy generalizes to a setting where users collaborate with a real LLM, indicating designing LLM assistants to be trust-aware is a promising avenue for human-LLM collaboration. *(Source: §1 Contribution iv; §11 "Experiment #5: Trust-Adaptive Interventions with an LLM Assistant" / §11.2 "Results")*
- **Limitation (acknowledged by authors):** The majority of studies used a simulated AI assistant rather than a real system; simulated answers can contradict each other across near-identical problems, and participants were U.K./U.S.-based English-speaking Prolific users, so attitudes toward AI may differ elsewhere. Trust-estimation from interaction-level features alone was insufficient to accurately predict moments of low/high trust. *(Source: §14 "Limitations"; §12 "Analysis: Can We Model User Trust?")*
- **Design implication for our team:** Make trust-adaptive behavior explicit and disclosed to users (per the authors' ethical framing: "disclosing to users that the AI assistant is modeling user trust"), and consider a forced pause/footstep for over-trusting users rather than more explanations alone.

---

## Cross-paper takeaways for the team sync

1. **Search frequency ≠ answer quality** (Paper 1). Agentic search deserves evaluation on grounding and attribution, not just click-through or accuracy metrics.
2. **From single-tool calls to orchestration** (Paper 2). Any tool-using assistant we build should be tested on multi-tool, long-horizon tasks with state persistence and failure recovery, not isolated tool calls.
3. **Trust-adaptive UX matters** (Paper 3). When trust is miscalibrated, the right intervention differs by direction (supporting vs. counter explanations, or forced pauses) — and it should be disclosed to users.

---

*Sources: arXiv abstracts (https://arxiv.org/abs/2609.19244, https://arxiv.org/abs/2603.22862, https://arxiv.org/abs/2502.13321) plus full-text reading of 2609.19244 (esp. §8, §13.2, §15), 2603.22862 (esp. §1, §5, §8, §30), and 2502.13321 (esp. §1, §10, §11, §14).*