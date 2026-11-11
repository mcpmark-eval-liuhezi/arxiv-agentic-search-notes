# Reading Notes: Agentic Search and Tool Use

**Purpose:** Team literature brief for Thursday's team sync on agentic search — assistants where an LLM-driven agent searches, calls tools, and hands answers to users.

**Repo:** `mcpmark-eval-liuhezi/arxiv-agentic-search-notes` (branch `add-reading-notes` → `main`)

**Papers covered (3):**
1. **arXiv:2609.19244** — Characterizing Web Search by Conversational LLM Agents: From Search Decisions and Strategies to Results and Responses
2. **arXiv:2603.22862** — The Evolution of Tool Use in LLM Agents: From Single-Tool Call to Multi-Tool Orchestration
3. **arXiv:2608.21177** — From Search Agents to Dissemination Interfaces: Understanding Human Trust in Health Information from Conversational Search

Each paper lists: arXiv ID, title, author list, subject category (from the public arXiv abs page), submission date, a 2–3 sentence abstract summary, and extracted findings tagged with the section they came from.

**How to comment:** Open a Pull Request against `main` (this file is already on branch `add-reading-notes`) and leave comments on this file directly.

---

## Paper 1: arXiv:2609.19244 — Characterizing Web Search by Conversational LLM Agents

- **Title:** Characterizing Web Search by Conversational LLM Agents: From Search Decisions and Strategies to Results and Responses
- **Authors:** Mahsa Amani, Seungeon Lee, Abhisek Dash, Asmaa El Fraihi, Yunah Jang, Elisabeth Kirsten, Qinyuan Wu, Krishna P. Gummadi, Manish Gupta, Abhilasha Ravichander, Muhammad Bilal Zafar, Soumi Das
- **Subject category (arXiv abs page):** cs.AI (primary), cs.IR (cross-listed)
- **Submission date (arXiv abs page):** Wed, 16 Sep 2026 17:56:32 UTC (v1)
- **Abstract summary:** This is the first study of Web search across four major conversational platforms (ChatGPT, Claude, Grok, DeepSeek), combining real-world user interactions (invivo) with controlled experiments using the same platform models via their APIs (invitro). The authors investigate the quality of agentic decisions to invoke Web search, their query strategies, domain preferences in results, and how they transform results into grounded responses. Key findings: search decisions vary substantially across platforms and models, more frequent Web-search invocation does not guarantee better response quality, and some claims rely on uncited search results.

### Extracted findings

- **Finding 1 — Search-calling decisions vary enormously across agents.** In the controlled `invitro` comparison on the same 1,000 user prompts, GPT-5.3-chat calls Web search for only 140 of 1,000 prompts (~14%), while Claude Sonnet 4.6 calls Web search for 825 of 1,000 (~83%). The authors note agents are good at deciding when they don't need Web search, but the decisions to call Web search do not always improve responses. *(Source: §3.1 "Search Calling Behaviors")*
- **Finding 2 — Citation/grounding problem: a non-trivial fraction of claims are ungrounded.** Responses average between 1 and 8 citations, with citation rates varying widely by platform (ChatGPT ~13.3%, Claude ~19.9%, Grok ~1.8%, DeepSeek ~34.1% in the invivo dataset) and model (GPT-5.3-chat ~10.6%, Claude Sonnet 4.6 ~39.7%). While most claims are well grounded in cited URLs, some claims rely on uncited search results, raising attribution and reliability concerns — the factuality of ungrounded claims (likely from parametric knowledge) remains poor. *(Source: §5.1 "Selection / Generation of Citations" and §5.2 "Grounding of Claims")*
- **Finding 3 — Platform-specific search engines return results with strong domain biases.** The paper documents that platform-specific search engines return a limited number of search results with strong preferences (biases) toward specific domains, so Web-grounded responses may reflect proprietary source preferences rather than a comprehensive view of the Web. *(Source: §1 Key Findings #2; §12.3 "Domain Preferences in Search Results")*
- **Design implication:** When steering our team's agentic-search work, weight search-call frequency alone, evaluate grounding/citation rates and domain-preference behavior explicitly, and plan attribution auditing into the response pipeline.

---

## Paper 2: arXiv:2603.22862 — The Evolution of Tool Use in LLM Agents

- **Title:** The Evolution of Tool Use in LLM Agents: From Single-Tool Call to Multi-Tool Orchestration
- **Authors:** Haoyuan Xu, Chang Li, Xinyan Ma, Xianhao Ou, Zihan Zhang, Tao He, Xiangyu Liu, Zixiang Wang, Jiafeng Liang, Zheng Chu, Runxuan Liu, Rongchuan Mu, Dandan Tu, Ming Liu, Bing Qin
- **Subject category (arXiv abs page):** cs.SE (primary), cs.CL (cross-listed)
- **Submission date (arXiv abs page):** v1 Tue, 24 Mar 2026 07:05:05 UTC (v2 Thu, 2 Apr 2026 02:54:00 UTC)
- **Abstract summary:** This comprehensive survey argues the central problem in LLM agents has shifted from whether a model can select and execute a correct single tool call to multi-tool orchestration over long trajectories with intermediate state, execution feedback, changing environments, and constraints like safety, cost, and verifiability. It unifies task formulations, organizes the literature around six core dimensions (inference-time planning and execution, training and trajectory construction, safety and control, efficiency under resource constraints, capability completeness in open environments, and benchmark design/evaluation), and summarizes applications in software engineering, enterprise workflows, GUIs, and mobile systems.

### Extracted findings

- **Finding 1 — The paradigm has shifted from call-level correctness to system-level orchestration.** The survey's core argument: modern evaluation moves "from isolated API invocation correctness to assessing system-level intelligence in sustaining, adapting, and repairing extended tool-use trajectories under structural, temporal, and environmental constraints." Long-horizon orchestration requires dynamic tool subset selection, cross-tool dependency modeling, sequential/parallel scheduling, failure recovery, and re-planning. Traditional ReAct-style sequential traces "implicitly assume that tasks are linearly decomposable" and fail on nested calls, parallel I/O, or long-horizon goals. *(Source: §1 Introduction; §3.1 "Topological Planning"; §3.2 "Long-Horizon Orchestration")*
- **Finding 2 — Four dimensions now define long-horizon intelligence.** Contemporary benchmarks prioritize (1) topological complexity of tool dependencies, (2) temporal scale of execution trajectories, (3) environmental dynamism during interaction, and (4) state persistence with self-correction mechanisms. Representative benchmarks surveyed include ELT-Bench, Toolcomp, FuncBenchGen, MSCoRe, ToolSword (with sub-dimensions: topological complexity, temporal scale, dynamic environments, state persistence/self-correction), and OSWorld, GTA, CRITIC-TOOL, MemAgentBench, ColorBench, ASTRA, and RoTBench. *(Source: §8 "Benchmarks"; Appendix §29.1–29.4 "ToolSword" breakdown)*
- **Finding 3 — Planning, training, safety, and efficiency are tightly coupled.** The survey's central observation is that these dimensions interact: planning interacts with state management and verifiability; safety depends on execution control and isolation; efficiency depends on decomposition, routing, and reuse. *(Source: §10 Conclusion)*
- **Limitation / open challenge (acknowledged by authors):** The paper is a survey that organizes the field and identifies a research agenda; it notes that planning, training, safety, and efficiency are typically studied separately even though deployed agent systems depend on their interaction, and flags future directions for reliable, scalable, and verifiable multi-tool agents. Better abstractions for stateful orchestration and stronger evaluation protocols for dynamic, long-horizon settings remain open. *(Source: §1.2 "Aims and Motivations"; §30.3 "Open Problems and Future Directions")*
- **Design implication:** Any tool-using assistant we build should be tested on multi-tool, long-horizon tasks with state persistence and failure recovery rather than isolated tool calls; report on orchestration quality (topology, temporal scale, state persistence) and measure cost/latency.

---

## Paper 3: arXiv:2608.21177 — From Search Agents to Dissemination Interfaces: Understanding Human Trust in Health Information from Conversational Search

- **Title:** From Search Agents to Dissemination Interfaces: Understanding Human Trust in Health Information from Conversational Search
- **Authors:** Xin Sun, Rongjun Ma, Xiaochang Zhao, Janne Lindqvist, Jan de Wit, Zhuying Li, Abdallah El Ali, Jos A. Bosch
- **Subject category (arXiv abs page):** cs.HC (primary, Human-Computer Interaction)
- **Submission date (arXiv abs page):** Fri, 21 Aug 2026 14:46:53 UTC (v1)
- **Abstract summary:** This research integrates two mixed-methods studies (lab sessions and interviews) to explore trust perceptions in health information across different search agents and dissemination interfaces. Study 1 (N=21) compared trust in health information from ChatGPT vs. Google across three types of health-related search tasks, finding significantly higher trust in ChatGPT. Study 2 (N=20) compared three dissemination interfaces (text-based, speech-based, embodied), all sourcing from the same LLM, and found significant trust variations across interfaces. Interviews identified key trust factors: source credibility, search autonomy, prior knowledge, and interaction style/modality. The paper is accepted at the journal *Behaviour & Information Technology (BIT)*.

### Extracted findings

- **Finding 1 — People trust LLM-powered conversational search more than traditional search engines.** In Study 1 (N=21), participants showed significantly higher trust in health information sourced from ChatGPT than from Google, attributing it to ChatGPT's conversational style, rapid personalized responses, and an intuitive interface that fosters perceived expertise and relevance. Qualitatively, users evaluated synthesized responses primarily on intrinsic presentation (logical flow, professional language, familiarity) rather than the agent's reputation. *(Source: §5.1 "Divergent Trust Patterns")*
- **Finding 2 — Trust does not transfer linearly from agent to information ("source dissociation").** For Google, trust in the health information was significantly correlated with trust in Google as a search engine. In contrast, no significant correlation was found between trust in ChatGPT and trust in the health information it generated — the authors call this "source dissociation," where conversational search collapses the structural separation between the search agent and the external source, so trust in the agent does not automatically translate into trust in the content. Qualitatively, participants relied on trust heuristics (e.g., MATCH model) as a proxy for trustworthiness when no explicit source was given. *(Source: §5.1 "Divergent Trust Patterns: From Search Agents to Dissemination Interfaces")*
- **Finding 3 — Modality richness does not linearly boost trust: the "modality-context mismatch."** Study 2 found significantly higher trust in the text-based interface over speech-based or embodied ones, even though speech and embodied interfaces provide more natural interaction and social cues. Users favored text for easier and more precise cross-referencing in high-stakes health contexts, citing familiarity and ease of use as primary drivers; speech/embodied interfaces triggered privacy concerns and skepticism about authenticity. Across both studies, there was no significant main effect of task type on trust, suggesting these patterns are generalized rather than task-specific. *(Source: §5.1 "Divergent Trust Patterns"; §5.2 "Factors of Trust in LLM-powered Conversational Search")*
- **Limitation / design implication (acknowledged by authors):** The manuscript is accepted at *Behaviour & Information Technology (BIT)*; trust patterns were measured in a lab setting with N=21 and N=20, so results should be treated as indicative rather than population-representative. The practical design implication: build trustworthy eHealth tools that balance engagement with perceived safety and usability, and consider that human-like features do not always increase trust in high-sensitivity contexts.

---

## Cross-paper takeaways for the team sync

1. **Search frequency ≠ answer quality** (Paper 1). Agentic search deserves evaluation on grounding and attribution, not just click-through or accuracy metrics. Design for citation auditability.
2. **From single-tool calls to orchestration** (Paper 2). Any tool-using assistant we build should be tested on multi-tool, long-horizon tasks with state persistence and failure recovery, not isolated tool calls. Evaluate on orchestration quality (topology, temporal scale, state persistence).
3. **Trust-adaptive UX matters** (Paper 3). When trust is miscalibrated, the right intervention differs by direction (supporting vs. counter explanations, or forced pauses for over-reliance) — and per the authors' ethical framing, disclose to users that the assistant is modeling their trust.

---

*Sources: arXiv public abstract pages (https://arxiv.org/abs/2609.19244, https://arxiv.org/abs/2603.22862, https://arxiv.org/abs/2608.21177) plus full-text reading of 2609.19244 (§3.1, §5.1, §5.2, §12.3, §15), 2603.22862 (§1, §3.1, §3.2, §8, §10, §30), and 2608.21177 (§1, §5.1, §5.2).*
