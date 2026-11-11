# Reading Notes: Agentic Search and Tool Use

**Purpose:** Team literature brief for Thursday's team sync on agentic search — assistants where an LLM-driven agent searches, calls tools, and hands answers to users.

**Repo:** `mcpmark-eval-liuhezi/arxiv-agentic-search-notes` (branch `add-reading-notes` → default branch `main`)

**Papers covered (3):**
1. **arXiv:2609.19244** — Characterizing Web Search by Conversational LLM Agents (empirical study of agentic search across ChatGPT, Claude, Grok, DeepSeek)
2. **arXiv:2603.22862** — The Evolution of Tool Use in LLM Agents (survey of the tool-use field)
3. **arXiv:2502.11705** — LLM Agents Making Agent Tools / ToolMaker (agentic framework that autonomously builds LLM-compatible tools from papers-with-code; ACL 2025)

Each paper entry lists: arXiv ID, title, author list, subject category (verified on the public arXiv abs page), submission date, a 2–3 sentence abstract summary, and at least two concrete findings extracted from the full text, each tagged with the section it came from.

**How to comment:** Open a Pull Request against `main` and leave comments on this file.

---

## Paper 1: arXiv:2609.19244 — Characterizing Web Search by Conversational LLM Agents

- **Title:** Characterizing Web Search by Conversational LLM Agents: From Search Decisions and Strategies to Results and Responses
- **Authors:** Mahsa Amani, Seungeon Lee, Abhisek Dash, Asmaa El Fraihi, Yunah Jang, Elisabeth Kirsten, Qinyuan Wu, Krishna P. Gummadi, Manish Gupta, Abhilasha Ravichander, Muhammad Bilal Zafar, Soumi Das
- **Subject category (arXiv abs page):** cs.AI (primary), cs.IR
- **Submission date (arXiv abs page):** Wed, 16 Sep 2026 17:56:32 UTC (v1); submitted by Mahsa Amani
- **Abstract summary:** The first study of Web search across four major conversational platforms (ChatGPT, Claude, Grok, and DeepSeek), combining real-world user interactions (invivo) with controlled experiments using the same platform models by their APIs (invitro). The authors investigate when agents decide to call Web search, how they formulate queries, the domain preferences in the results they receive, and how they transform search results into grounded responses. Key takeaway: search decisions vary substantially across platforms and models, more frequent Web-search invocation does not necessarily yield better response quality, and some claims rely on uncited search results, raising concerns about attribution and reliability.

### Extracted findings (from full text)

- **Finding 1 — More frequent Web search does not equal better answers.** LLM agents are good at deciding when they do *not* need Web search, but their decisions to call Web search do not always improve their responses. Turn-level search-calling varied sharply across platforms — 42,273 of 143,730 ChatGPT turns (31%) used Web search, vs. 1,696 of 9,267 for Claude, 3,004 of 9,005 for Grok, and 1,730 of 9,262 for DeepSeek — even though all four platforms show a rising trend in search-calling over time. *(Source: §1 Introduction / Key Findings #1; §2 "Datasets and Experimental Setups", Table 1)*
- **Finding 2 — Platform-specific search engines return results with strong, platform-specific domain biases.** Platform search engines built for LLM agents return only a small number of results per query (Claude ~10, Grok ~9, DeepSeek ~5, ChatGPT ~15 — far fewer than a typical Google search), and top result domains vary by platform: reddit.com and youtube.com dominate ChatGPT and Grok results and rank highly in DeepSeek but are completely absent from Claude search; xiaohongshu.com is prominent in DeepSeek but absent from the other platforms. It is unclear whether users are aware of these biases. *(Source: §4.3 "Domain Preferences in Search Results", Table 5 and Figure 8)*
- **Finding 3 — Attribution/grounding problem: some claims are ungrounded.** Responses include on average between 1 and 8 citations per response, with citation rates varying from 1.7% to 34.1% across platforms (e.g., Grok 1.8%, DeepSeek 34.1%, ChatGPT 13.3%, Claude 19.9% citation rate), and a non-trivial fraction of claims in responses are attributed to returned search URLs that are not cited; hallucinated citations remain rare (1–2%), but the factuality of ungrounded claims (likely from the model's parametric knowledge) remains poor. *(Source: §5 "Analyzing Search Responses", §5.1 "Selection / Generation of Citations", Table 6)*
- **Limitation (acknowledged by authors):** The analysis relies on observational conversational traces (invivo) that do not expose the full internal reasoning, ranking algorithms, or proprietary retrieval pipelines, so some observed behaviors may reflect harness implementations rather than the underlying models; the study is largely English-language and does not analyze non-English retrieval or downstream user-trust effects. *(Source: §1 Introduction, §2 Dataset description, §4.3 Summary)*

---

## Paper 2: arXiv:2603.22862 — The Evolution of Tool Use in LLM Agents

- **Title:** The Evolution of Tool Use in LLM Agents: From Single-Tool Call to Multi-Tool Orchestration
- **Authors:** Haoyuan Xu, Chang Li, Xinyan Ma, Xianhao Ou, Zihan Zhang, Tao He, Xiangyu Liu, Zixiang Wang, Jiafeng Liang, Zheng Chu, Runxuan Liu, Rongchuan Mu, Dandan Tu, Ming Liu, Bing Qin
- **Subject category (arXiv abs page):** cs.SE (primary), cs.CL
- **Submission date (arXiv abs page):** v1 Tue, 24 Mar 2026 07:05:05 UTC (1,024 KB); v2 Thu, 2 Apr 2026 02:54:00 UTC (1,002 KB); submitted by Haoyuan Xu
- **Abstract summary:** This survey argues the central problem in LLM agents has shifted from whether a model can make a correct single tool call to multi-tool orchestration over long trajectories with intermediate state, execution feedback, changing environments, and practical constraints like safety, cost, and verifiability. It unifies task formulations, organizes the literature around six core dimensions (inference-time planning/execution, training/trajectory construction, safety/control, efficiency under resource constraints, capability completeness in open environments, and benchmark design/evaluation), and summarizes applications in software engineering, enterprise workflows, GUIs, and mobile systems.

### Extracted findings (from full text)

- **Finding 1 — The paradigm has shifted from call-level correctness to system-level orchestration.** The survey's own framing: progress depends not only on stronger base models but on advances in memory, orchestration, verification, and benchmark design. Long-horizon orchestration problems include dynamic tool subset selection, cross-tool dependency modeling, sequential/parallel scheduling, failure recovery, and re-planning, with state consistency and race conditions under parallel execution emerging as core bottlenecks for system stability. *(Source: §1 Introduction; §2 "Problem formulation")*
- **Finding 2 — Safe parallelism requires more than better prompting; it requires explicit control over side effects.** In multi-tool systems, security is no longer determined by the legitimacy of isolated API calls but by complex, cross-tool, cross-timestep execution structures; safe parallelism requires explicit control over side effects and execution scope, moving from "pre-execution static constraints" (e.g., AARM, AgentSpec) → "in-execution transaction management" (e.g., SagaLLM, Atomix) → "post-execution dynamic verification" (e.g., CRITIC, VerifiAgent, DVR). *(Source: §5 "Safety of Multi-Tool Agents", §5.1 "Security Risks in Parallel Execution")*
- **Finding 3 — Benchmarks now target long-horizon, multi-tool properties.** Contemporary benchmarks prioritize (1) topological complexity of tool dependencies, (2) temporal scale of execution trajectories, (3) environmental dynamism during interaction, and (4) state persistence with self-correction mechanisms. Representative benchmarks surveyed include ELT-Bench, Toolcomp, FuncBenchGen, MSCoRe, ToolSword (with sub-dimensions: topological complexity, temporal scale, dynamic environments, state persistence/self-correction), and OSWorld, GTA, CRITIC-TOOL, MemAgentBench, ColorBench, ASTRA, and RoTBench. *(Source: §8 "Benchmarks"; §29 "ToolSword" breakdown)*
- **Limitation / open challenge (acknowledged by authors):** The paper is a survey that organizes the field and proposes a research agenda; it notes that planning, training, safety, and efficiency are typically studied separately even though deployed agent systems depend on their interaction, and flags future directions for building reliable, scalable, and verifiable multi-tool agents. *(Source: §1.2 "Aims and Motivations"; §8 "Benchmarks")*

---

## Paper 3: arXiv:2502.11705 — LLM Agents Making Agent Tools (ToolMaker)

- **Title:** LLM Agents Making Agent Tools
- **Authors:** Georg Wölflein, Dyke Ferber, Daniel Truhn, Ognjen Arandjelović, Jakob Nikolas Kather
- **Subject category (arXiv abs page):** cs.CL (primary), cs.AI, cs.LG, cs.MA
- **Submission date (arXiv abs page):** v1 Mon, 17 Feb 2025 11:44:11 UTC (1,384 KB); v2 Thu, 29 May 2025 18:47:41 UTC; submitted by Georg Wölflein. **Comments: Accepted at ACL 2025.**
- **Abstract summary:** Tool use has turned large language models into powerful agents that perform complex multi-step tasks by dynamically utilising external software components. However, these tools must be implemented in advance by human developers, hindering applicability in domains demanding large numbers of highly specialised tools (e.g., life sciences and medicine). Motivated by scientific studies accompanied by public code repositories, the authors propose ToolMaker, an agentic framework that autonomously transforms papers-with-code into LLM-compatible tools. Given a GitHub URL and short task description, ToolMaker autonomously installs dependencies and generates code to perform the task, using a closed-loop self-correction mechanism for debugging. On a benchmark of 15 complex computational tasks with over 100 unit tests, the method correctly implements 80% of tasks, substantially outperforming current state-of-the-art software engineering agents. Code and benchmark are publicly available.

### Extracted findings (from full text)

- **Finding 1 — ToolMaker is a two-stage agentic workflow with environment snapshots and closed-loop self-correction.** Each tool requires only a minimal definition: (1) a concise textual description of the task, (2) a GitHub URL of the associated repository, and (3) a list of required input arguments including an example value for each. ToolMaker's workflow has two stages: environment setup (producing a reproducible "snapshot" Docker image where the tool will run, including dependency installation, model downloads, and hardware-aware configuration) and tool implementation (generating a Python function). It is built from LLM calls (e.g., gpt-4o-2024-08-06), read-only environment actions (run_bash_command, list_directory, read_file, write_file, browse, google_drive_*) versus write actions (terminal, write_file, google_drive, run_implementation), and agents that chain multiple LLM calls and environment interactions to accomplish a sub-task such as "install this repository and its dependencies." A closed-loop self-correction mechanism handles debugging. *(Source: Appendix A "ToolMaker", §3 "Workflow components", §3.1.1–3.1.3)*\n- **Finding 2 — ToolMaker correctly implements 80% of tasks on a 15-task, 100+ unit-test benchmark, beating prior SOTA software engineering agents.** The authors introduce TM-Bench, comprising 15 complex computational tasks spanning various domains with over 100 unit tests to assess correctness and robustness. ToolMaker's method correctly implements 80% of the tasks, substantially outperforming current state-of-the-art software engineering agents, and is a step towards fully autonomous agent-based scientific workflows. *(Source: Abstract; §4 "Appendix C TM-Bench")*\n- **Limitation / design implication (acknowledged):** The paper is structured with the main body consisting of a long reference list and the architectural contributions placed in appendices (Appendix A ToolMaker, Appendix C TM-Bench, Appendix D ToolMaker); evaluation is on benchmark tasks in scientific domains (pathology, radiology, omics, and other domains per the benchmark breakdown), and generalizability outside those domains is not yet established. *(Source: paper structure (Appendix A/C/D); §5.1–5.4 "C.1 Pathology", "C.2 Radiology", "C.3 Omics", "C.4 Other")*

---

## Cross-paper takeaways for the team sync

1. **Search frequency ≠ answer quality** (Paper 1, arXiv:2609.19244). Agentic search deserves evaluation on grounding and attribution, not just click-through or accuracy metrics — more Web-search calls don't reliably improve responses, and platform search engines skew toward a few domains, so tool/engine coverage needs auditing.
2. **From single-tool calls to orchestration** (Paper 2, arXiv:2603.22862). Any tool-using assistant we build should be tested on multi-tool, long-horizon tasks with state persistence and failure recovery, not isolated tool calls; safe parallelism needs explicit side-effect control (transactions, rollbacks, verification), not just better prompts.
3. **Autonomous tool acquisition is viable** (Paper 3, arXiv:2502.11705). ToolMaker shows agents can autonomously turn papers-with-code into LLM-compatible, dependency-resolving tools at 80% task correctness on a 15-task benchmark with 100+ unit tests — a promising pattern for domain-specific toolkits (e.g., medical/scientific assistants) where hand-built tools don't exist.

---

*Sources: arXiv abstracts (https://arxiv.org/abs/2609.19244, https://arxiv.org/abs/2603.22862, https://arxiv.org/abs/2502.11705) plus full-text reading of 2609.19244 (§1, §2, §4.3, §5), 2603.22862 (§1, §2, §5, §8), and 2502.11705 (Appendix A ToolMaker, Appendix C TM-Bench).
