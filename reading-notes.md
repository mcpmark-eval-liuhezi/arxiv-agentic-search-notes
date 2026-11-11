# Reading Notes: Agentic Search and Tool Use

**Purpose:** Team literature brief for Thursday's team sync on agentic search — assistants where an LLM-driven agent searches, calls tools, and hands answers to users.

**Repo:** `mcpmark-eval-liuhezi/arxiv-agentic-search-notes` (branch `add-reading-notes` → `main`)

**Papers covered (3):**
1. **arXiv:2609.19244** — Characterizing Web Search by Conversational LLM Agents: From Search Decisions and Strategies to Results and Responses (agentic search lifecycle across ChatGPT, Claude, Grok, DeepSeek)
2. **arXiv:2603.22862** — The Evolution of Tool Use in LLM Agents: From Single-Tool Call to Multi-Tool Orchestration (survey of the tool-use field)
3. **arXiv:2607.24601** — Evaluating the Impact of Explainable AI on Trust in AI-Assisted Code Review (human factors / trust in AI-assisted assistants)

Each paper entry lists: arXiv ID, title, author list, subject category (verified on the public arXiv abs page), submission date, a 2–3 sentence abstract summary, and at least two concrete findings extracted from the full text, each tagged with the section it came from.

**How to comment:** Open a Pull Request against `main` and leave comments on this file.

---

## Paper 1: arXiv:2609.19244 — Characterizing Web Search by Conversational LLM Agents

- **Title:** Characterizing Web Search by Conversational LLM Agents: From Search Decisions and Strategies to Results and Responses
- **Authors:** Mahsa Amani, Seungeon Lee, Abhisek Dash, Asmaa El Fraihi, Yunah Jang, Elisabeth Kirsten, Qinyuan Wu, Krishna P. Gummadi, Manish Gupta, Abhilasha Ravichander, Muhammad Bilal Zafar, Soumi Das
- **Subject category (arXiv abs page):** cs.AI (primary), cs.IR
- **Submission date (arXiv abs page):** Wed, 16 Sep 2026 17:56:32 UTC (v1)
- **Abstract summary:** The first study of Web search across four major conversational platforms (ChatGPT, Claude, Grok, and DeepSeek), combining real-world user interactions (invivo) with controlled experiments using the same platform's models by their APIs (invitro). The authors investigate when agents decide to call Web search, how they formulate queries, the domain preferences in the results, and how they transform search results into grounded responses. Key takeaway: search decisions vary substantially across platforms and models, more frequent Web-search invocation does not necessarily yield better response quality, and some claims rely on uncited search results.

### Extracted findings (from full text)

- **Finding 1 — More frequent Web search does not equal better answers.** LLM agents are good at deciding when they do *not* need Web search, but their decisions to call Web search do not always improve their responses. Turn-level search calling varied sharply across platforms — 42,273 of 143,730 ChatGPT turns (31%) used Web search, vs. 1,696 of 9,267 for Claude, 3,004 of 9,005 for Grok, and 1,730 of 9,262 for DeepSeek — even though all four platforms show a rising trend in search-calling over time. *(Source: §1 Introduction / Key Findings #1; §2 "Datasets and Experimental Setups", Table 1)*
- **Finding 2 — Platform-specific search engines return results with strong, platform-specific domain biases.** Platform search engines built for LLM agents return only a small number of results per query (Claude ~10, Grok ~9, DeepSeek ~5, ChatGPT ~15 — far fewer than a typical Google search), and top result domains vary by platform: reddit.com and youtube.com dominate ChatGPT and Grok results and rank highly in DeepSeek but are completely absent from Claude search; xiaohongshu.com is prominent in DeepSeek but absent from the other platforms. It is unclear whether users are aware of these biases. *(Source: §4.3 "Domain Preferences in Search Results", Table 5 and Figure 8)*
- **Finding 3 — Attribution/grounding problem: some claims are ungrounded.** Responses include on average between 1 and 8 citations per response, with citation rates varying from 1.7% to 34.1% across platforms (e.g., Grok 1.8%, DeepSeek 34.1%, ChatGPT 13.3%, Claude 19.9% citation rate), and a non-trivial fraction of claims in responses are attributed to returned search URLs that are not cited; hallucinated citations remain rare (1–2%), but the factuality of ungrounded claims (likely from the model's parametric knowledge) remains poor. *(Source: §5 "Analyzing Search Responses", §5.1 "Selection / Generation of Citations", Table 6)*
- **Limitation (acknowledged by authors):** The analysis relies on observational conversational traces (invivo) that do not expose the full internal reasoning, ranking algorithms, or proprietary retrieval pipelines, so some observed behaviors may reflect harness implementations rather than the underlying models; the study is largely English-language and does not analyze non-English retrieval or downstream user-trust effects. *(Source: §1 Introduction, §2 Dataset description, §4.3 Summary)*

---

## Paper 2: arXiv:2603.22862 — The Evolution of Tool Use in LLM Agents

- **Title:** The Evolution of Tool Use in LLM Agents: From Single-Tool Call to Multi-Tool Orchestration
- **Authors:** Haoyuan Xu, Chang Li, Xinyan Ma, Xianhao Ou, Zihan Zhang, Tao He, Xiangyu Liu, Zixiang Wang, Jiafeng Liang, Zheng Chu, Runxuan Liu, Rongchuan Mu, Dandan Tu, Ming Liu, Bing Qin
- **Subject category (arXiv abs page):** cs.SE (primary), cs.CL
- **Submission date (arXiv abs page):** v1 Tue, 24 Mar 2026 07:05:05 UTC (v2 Thu, 2 Apr 2026 02:54:00 USD)
- **Abstract summary:** This survey argues the central problem in LLM agents has shifted from whether a model can make a correct single tool call to multi-tool orchestration over long trajectories with intermediate state, execution feedback, and constraints like safety, cost, and verifiability. It unifies task formulations, organizes the literature around six core dimensions (inference-time planning/execution, training/trajectory construction, safety/control, efficiency under resource constraints, capability completeness in open environments, and benchmark design/evaluation), and summarizes applications in software engineering, enterprise workflows, GUIs, and mobile systems.

### Extracted findings (from full text)

- **Finding 1 — The paradigm has shifted from call-level correctness to system-level orchestration.** The survey's own framing: modern evaluation moved "from isolated API invocation correctness to assessing system-level intelligence in sustaining, adapting, and repairing extended tool-use trajectories under structural, temporal, and environmental constraints." Long-horizon orchestration problems include dynamic tool subset selection, cross-tool dependency modeling, sequential/parallel scheduling, failure recovery, and re-planning, with state consistency and race conditions under parallel execution emerging as core bottlenecks for system stability. *(Source: §1 Introduction; §2 "Problem formulation")*
- **Finding 2 — Four interdependent dimensions define long-horizon intelligence.** Contemporary benchmarks prioritize (1) topological complexity of tool dependencies, (2) temporal scale of execution trajectories, (3) environmental dynamism during interaction, and (4) state persistence with self-correction mechanisms. Early benchmarks like API-Bank, MetaTool, and ToolBench established intent detection and tool retrieval, but current research addresses these four dimensions; representative benchmarks surveyed include ELT-Bench, Toolcomp, FuncBenchGen, MSCoRe, ToolSword (with sub-dimensions: topological complexity, temporal scale, dynamic environments, state persistence/self-correction), and OSWorld, GTA, CRITIC-TOOL, MemAgentBench, ColorBench, ASTRA, and RoTBench. *(Source: §8 "Benchmarks"; §29.1–29.4 "ToolSword" breakdown)*
- **Finding 3 — Safe parallelism requires more than better prompting; it requires explicit control over side effects.** In multi-tool systems, security is no longer determined by the legitimacy of isolated API calls but by complex, cross-tool, cross-timestep execution structures; "state pollution" through the tool-chain context can lead to cascading propagation of attacks, so safe parallelism requires explicit control over side effects and execution scope, moving from "pre-execution static constraints" to "in-execution transaction management" (e.g., SagaLLM, Atomix) to "post-execution dynamic verification" (e.g., CRITIC, VerifiAgent). *(Source: §5 "Safety of Multi-Tool Agents", §5.1 "Security Risks in Parallel Execution")*
- **Limitation / open challenge (acknowledged by authors):** The paper is a survey that organizes the field and identifies a research agenda; it notes that planning, training, safety, and efficiency are typically studied separately even though deployed agent systems depend on their interaction, and flags future directions for building reliable, scalable, and verifiable multi-tool agents. *(Source: §1.2 "Aims and Motivations"; §8 "Benchmarks")*

---

## Paper 3: arXiv:2607.24601 — Evaluating the Impact of Explainable AI on Trust in AI-Assisted Code Review

- **Title:** Evaluating the Impact of Explainable AI on Trust in AI-Assisted Code Review
- **Authors:** Zhenhan Gao, Marvin Muñoz Barón, Umm-e Habiba, Daniel Graziotin, Stefan Wagner
- **Subject category (arXiv abs page):** cs.SE (primary), cs.AI, cs.HC
- **Submission date (arXiv abs page):** Mon, 27 Jul 2026 16:04:24 UTC (v1)
- **Abstract summary:** A within-subjects user study (N=34) with 9 real-world pull requests comparing three LLM-based code review systems with varying levels of Explainable AI support (Condition A: detailed inline explanations + review feedback; Condition B: review feedback only; Condition C: no explanations). More explanation significantly influences both trust and agreement in different ways: full explanations (A) yield the highest perceived trust (M = 3.99/5) but not the highest agreement, moderate explanations (B) achieve the highest agreement (89.22%), and no explanations (C) results in the lowest trust and agreement; explanation level did not significantly affect review time. The paper is published under CC BY 4.0 and is accepted to ISSTA 2026 (PACMSE).

### Extracted findings (from full text)

- **Finding 1 — Trust and agreement move in different directions with explanation level.** Full explanations (Condition A) yield the highest perceived trust (M = 3.99/5) but not the highest agreement with AI recommendations, whereas moderate explanations (Condition B) achieve the highest agreement (89.22%) — this could suggest that more explanation prompts developers to question AI recommendations more frequently. In contrast, providing no explanations (Condition C) results in the lowest levels of trust and agreement, and the system with no explanations (C) was the only condition in which participants never explicitly stated agreement with the AI, although they often followed its recommendations. *(Source: §1 Introduction; §5 "Results")*
- **Finding 2 — Explanation level did not significantly affect review time.** The level of explanation had no significant impact on the time taken to accept or reject a code change (with the full-explanation system requiring the most time, but differences not statistically significant). *(Source: §1 Introduction; §5 "Results")*
- **Finding 3 — The most common reasons for decisions were code readability and correctness.** Across all conditions, developers cited code readability and implementation correctness as the primary reasons for accepting or rejecting code changes, and the majority of stated attitudes toward AI-assisted code review were positive (only ~30.7% of ChatGPT-related review reactions were negative in related literature reviewed). *(Source: §1 Introduction; §4 "Methodology" / 3.1; §5 "Results")*
- **Limitation / design implication (acknowledged):** The study is a controlled lab study with 34 participants from diverse programming backgrounds and nine curated real-world Python pull requests; the authors note that trust is often overlooked in LLM-for-SE research in favor of model performance, and that trust-adaptive interventions (supporting vs. counter-explanations) should be disclosed to users. *(Source: §1 Introduction; §2.2 "Explainable AI")*

---

## Cross-paper takeaways for the team sync

1. **Search frequency ≠ answer quality** (Paper 1). Agentic search deserves evaluation on grounding and attribution, not just click-through or accuracy metrics — more Web-search calls don't reliably improve responses, and platform search engines skew toward a few domains.
2. **From single-tool calls to orchestration** (Paper 2). Any tool-using assistant we build should be tested on multi-tool, long-horizon tasks with state persistence and failure recovery, not isolated tool calls; safe parallelism needs explicit side-effect control, not just better prompts.
3. **Trust-adaptive UX matters** (Paper 3). When trust is miscalibrated, the right intervention differs by direction (supporting vs. counter explanations, or forced pauses) — and it should be disclosed to users.

---

*Sources: arXiv abstracts (https://arxiv.org/abs/2609.19244, https://arxiv.org/abs/2603.22862, https://arxiv.org/abs/2607.24601) plus full-text reading of 2609.19244 (§1, §2, §4.3, §5), 2603.22862 (§1, §2, §5, §8), and 2607.24601 (§1, §2.2, §3, §5).*
