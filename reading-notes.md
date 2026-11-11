# Reading Notes: Agentic Search and Tool Use

**Purpose:** Team literature brief for Thursday's team sync on agentic search — assistants where an LLM-driven agent searches, calls tools, and hands answers to users.

**Repo:** `mcpmark-eval-liuhezi/arxiv-agentic-search-notes` (branch `add-reading-notes` → `main`)

**Papers covered (3):**
1. **arXiv:2406.12276** — CodeNav: Beyond tool-use to using real-world codebases with LLM agents (code-use paradigm, agentic search over codebases)
2. **arXiv:2512.13278** — AutoTool: Dynamic Tool Selection and Integration for Agentic Reasoning (dynamic tool selection with RL, out-of-distribution generalization)
3. **arXiv:2304.02233** — Ericson: An Interactive Open-Domain Conversational Search Agent (deployed conversational search agent with real-user analysis)

Each paper entry lists: arXiv ID, title, author list, subject category (verified on the public arXiv abs page), submission date (from the abs page), a 2–3 sentence abstract summary, and at least two concrete findings extracted from the full text, each tagged with the section it came from.

**How to comment:** Open a Pull Request against `main` and leave comments on this file.

---

## Paper 1: arXiv:2406.12276 — CodeNav: Beyond tool-use to using real-world codebases with LLM agents

- **Title:** CodeNav: Beyond tool-use to using real-world codebases with LLM agents
- **Authors:** Tanmay Gupta, Luca Weihs, Aniruddha Kembhavi
- **Subject category (arXiv abs page):** cs.AI (primary); Computation and Language (cs.CL); Software Engineering (cs.SE)
- **Submission date (arXiv abs page):** Tue, 18 Jun 2024 05:10:38 UTC (v1)
- **Abstract summary:** CodeNav is an LLM agent that navigates and leverages previously unseen code repositories to solve user queries, moving beyond the tool-use paradigm that requires "registering" every tool via manual descriptions in the LLM context. Instead, CodeNav automatically indexes and searches over code blocks in the target codebase, finds relevant snippets, imports them, and iteratively generates a solution with execution feedback. On three tool-use benchmarks (m&m's, M3 ToolEval, API-Bank) CodeNav is competitive with tool-use (which has privileged access to tool names and descriptions), and the authors argue source code's inherent structure (README-style library descriptions, meaningful names, organization) makes free-form code search sufficient — "a codebase is all you need."

### Extracted findings (from full text)

- **Finding 1 — Code-use is competitive with tool-use despite no tool registration.** On m&m's and API-Bank, code-use achieves similar or slightly lower tool-f1 than tool-use (e.g., m&m's tool-f1: tool-use 79.6 ± 2.3 vs. code-use 80.6 ± 5.1; API-Bank F1: tool-use 84.0 vs. code-use 85.3), and on M3 ToolEval (final-answer correctness) code-use is within two points of tool-use. Code-use costs ~2 more interaction steps per query on average and only a minor increase in performance variance. *(Source: §6.1 "How does code-use compare to tool-use on tool-use benchmarks?", Table 1)*
- **Finding 2 — A high-level library description suffices; per-tool descriptions are unnecessary.** Ablations on m&m's show F1 rises from 74.1 ± 1.9 (no description) to 78.1 ± 3.5 (tool names only) to 80.8 ± 0.4 (full tool descriptions, 3,680 chars) — yet a library description (2,061 chars, CodeNav's approach) reaches 80.6 ± 5.1, i.e. similar performance at less than half the prompt length. The trade-off is a minor step penalty (~7.2 vs. ~6.9 steps) since the agent must search to discover tools rather than recall from context. *(Source: §6.2 "Is a library description sufficient for tool-use?", Table 2)*
- **Design decision — single-agent, multi-environment loop with Elasticsearch retrieval.** Code-nav针对性 designs a ReAct-style loop (thought → action type → action content, in structured XML) over a retrieval environment (Elasticsearch index of functions/classes/imports with rule-based re-ranking and persistent memory to avoid re-surfaces) and a Python execution environment (exec with optional flake8/mypy/black linting, capturing stdout, global namespace changes, and errors as feedback). It also adds Done and Code Summary helper environments. *(Source: §3.2 "Environments", §3.3 "Agent Actions", §3.4 "Environment Responses")*
- **Limitation (acknowledged by authors):** Code-nav targets Python only in this work (no stated limitation on other languages), and the benchmarks (m&m's, M3 ToolEval, API-Bank) are code-task benchmarks, so transfer to general web/search assistant behavior is not directly tested. Code is released under a permissive license. *(Source: §1 Introduction; Appendix A experiment details; Appendix E Limitations)*

---

## Paper 2: arXiv:2512.13278 — AutoTool: Dynamic Tool Selection and Integration for Agentic Reasoning

- **Title:** AutoTool: Dynamic Tool Selection and Integration for Agentic Reasoning
- **Authors:** Jiaru Zou, Ling Yang, Yunzhe Qi, Sirui Chen, Mengting Ai, Ke Shen, Jingrui He, Mengdi Wang
- **Subject category (arXiv abs page):** cs.CL (primary); Computation and Language (cs.CL); Machine Learning (cs.LG)
- **Submission date (arXiv abs page):** v1 Mon, 15 Dec 2025 12:38:04 UTC (v2 Fri, 5 Jun 2026 03:56:56 UTC)
- **Comments (abs page):** ICML 2026; Best Paper Award at ICCV 2025 Workshop on Multi-Modal Reasoning for Agentic Intelligence
- **Abstract summary:** AutoTool is a training framework that equips LLM agents with dynamic tool-selection capabilities throughout reasoning trajectories, addressing the limitation that existing agentic RL methods assume a fixed inventory of tools. It uses a dual-phase pipeline: Phase I does SFT + RL for stable long CoT, and Phase II applies KL-regularized Plackett–Luce ranking optimization over an evolving toolset. Built on a 200k dataset with explicit tool-selection rationales across 1,000+ tools and 100+ tasks, it trains Qwen3-8B and Qwen2.5-VL-7B, beating advanced LLM agents and tool-integration methods with average gains of 6.4% (math & science), 4.5% (search-based QA), 7.7% (code generation), and 6.9% (multimodal understanding), with stronger generalization to unseen tools.

### Extracted findings (from full text)

- **Finding 1 — AutoTool (Qwen3-8B) beats bigger closed models on math and code.** On AIME24, AutoTool (Qwen3-8B) reaches 68.8 vs. DeepSeek-R1-Distill-Qwen-14B's 65.9, QwQ-32B's 47.3, and ReTool (32B)'s 70.1 on AIME25 (51.2) — and on V-Code (code generation in image form) it reaches 56.1 vs. 44.5 for Search-R1 and 45.3 for AutoTool(Qwen2.5-VL-7B), while 2WikiMultihopQA is 48.8 vs. Search-R1's 43.3 and GPT-4o's 18.8. Gains are stated as average +6.4% math/science, +4.5% search QA, +7.7% code gen, +6.9% multimodal. *(Source: §5 "Experiments", Table 1)*
- **Finding 2 — It generalizes to tools it never saw at train time.** In a fully unseen-tool setting (inference pool restricted to 886 unseen tools out of 1,346), AutoTool (Qwen3-8B) scores 66.7 on AIME24 and 43.8 on HotpotQA vs. ReTool's 21.4 / 13.4, with only <2% drop versus the full-toolset setting (84.3 / 77.9 on LiveCodeBench / MedQA out-of-domain). Trajectories show AutoTool relies largely on unseen tools, especially on search and multimodal tasks (Figure 4), and Phase-II refinement substantially increases tool-selection HitRate, which correlates with downstream accuracy. *(Source: §6.1 "In-depth Analyses on AutoTool", Table 3, Figure 4)*
- **Finding 3 — Tool-selection rationales in the training data pay off.** An ablation shows models trained with explicit tool-selection rationales consistently outperform those trained without, and rationales increase successful tool executions by 13.5%, indicating more reliable intermediate tool use. The framework casts tool selection as a per-step Plackett–Luce ranking problem over the evolving toolset, selecting tools by sampling from a softmax-normalized distance distribution between a predicted embedding and candidate tool embeddings in the model's own hidden-state space. *(Source: §3.2–3.4 "Tool-Aware Trajectory Generation / Dual-phase Policy Training Pipeline / PL Ranking & Optimization", §6.1 "Effectiveness of Tool-Selection Rationales")*
- **Design decision / training setup:** Phase I uses LLaMa-Factory for SFT and VeRL for RL (rollout size N=8 per question, 3 epochs for Phase II); all experiments on 8×A100-80G. The 200k dataset was curated via a three-stage pipeline with explicit tool-selection rationales across 1,000+ tools and 100+ tasks (math, science, code generation, multimodal reasoning). *(Source: §5 "Experiments", Implementation Details; §4 "Data Curation for Tool-Selection")*

---

## Paper 3: arXiv:2304.02233 — Ericson: An Interactive Open-Domain Conversational Search Agent

- **Title:** Ericson: An Interactive Open-Domain Conversational Search Agent
- **Authors:** Zihao Wang, Ali Ahmadvand, Jason Ingyu Choi, Payam Karisani, Eugene Agichtein
- **Subject category (arXiv abs page):** cs.CL (primary); Computation and Language (cs.CL); Artificial Intelligence (cs.AI); MSC classes 68T50, 68T07
- **Submission date (arXiv abs page):** Wed, 5 Apr 2023 05:28:31 UTC (v1)
- **Abstract summary:** Ericson is a fully functional open-domain conversational search (ODCS) agent combining state-of-the-art QA and IR components with intent inference and dialogue management for proactive question refinement and recommendations. It was stress-tested in the Amazon Alexa Prize by engaging in live conversations with thousands of real Alexa users, giving an empirical basis for analyzing an ODCS system in the real world. Interaction data analysis showed accurate intent classification, encouraging user engagement, and careful proactive recommendations contribute most to user satisfaction, and the study identifies limitations of existing search techniques as a building block for next-generation ODCS agents.

### Extracted findings (from full text)

- **Finding 1 — Intent classification accuracy and proactive recommendations drive satisfaction most.** The authors report that accurate intent classification, encouraging user engagement, and careful proactive recommendations contribute most to users' satisfaction, and the system was the "first fully-functional open-domain conversational search prototype" with extensive real-user analysis. The system uses a hierarchical intent classifier (topic-level plus entity-level refinement via Google Knowledge Graph API, e.g. "let's talk about Bruno Mars" → entity "American singer-songwriter" → music-search module), a Dialogue State Manager (DSM) that controls conversation flow, IR components (general/domain-specific QA + search), a conversational transition component for proactive topic suggestions, and a dialogue logger. *(Source: Abstract; §4.3 "Hierarchical Intent Classifier"; §4.4 "Dialogue State Manager (DSM)"; §5 "Results and Discussion")*
- **Finding 2 — The engagement mechanism significantly increases conversation depth.** Adding in-depth engagement to the "Movies" component raised the average number of conversation turns from 1.554 to 2.667 (~+71.0%, p < 0.001 via two-tailed t-test); Food component from 1.280 to 1.850 (+45.0%, p < 0.001); and News component from 1.830 to 2.745 (+50.0%, p < 0.001), confirming the engagement mechanism is crucial to overall system performance and that users are willing to continue conversations. *(Source: §6.1.2 "Engagement")*
- **Finding 3 — Limitation acknowledged by authors.** The main limitation is that the study ran in a real-time setting, so component changes may affect other components, and because the user study time frame was limited, some component updates (classifiers, engagement, proactivity) were completed within a short window, making it hard to exclusively attribute ratings to individual component updates. Still, the improvements from the intent classifier, engagement, and proactive topic recommendation were significant enough to credit for the overall user-experience gains. *(Source: §7 "Limitations")*
- **Design decision — deployed as an Alexa skill on EC2 via Lambda.** Ericson was deployed on Amazon EC2 servers connected to the Alexa Skills Kit through a Lambda function, activated when assigned to a random Alexa user, with a five-component architecture (hierarchical intent classifier, DSM, IR components, conversational transition, dialogue logger) and is stress-tested with thousands of live users plus explicit ratings and free-form feedback. *(Source: §3.1 "Ericson Implementation as an Alexa skill"; §3.2 "Ericson System Overview"; Abstract)*

---

## Cross-paper takeaways for the team sync

1. **"More tool/search calls ≠ better answers" (Paper 1, CodeNav).** Free-form code search over a repository ("a codebase is all you need") matches pre-registered tool-use on official benchmarks at a fraction of the prompt length, so agentic search Value is in task coverage, not call volume. Pair every agentic-search prototype with a retrieval+execution loop (like CodeNav's) rather than a fixed tool registry.
2. **Dynamic tool selection beats fixed inventories (Paper 2, AutoTool).** With <2% drop on unseen tools and +13.5% more successful tool executions with rationales in the dataset, train your agent to *choose* from a large evolving toolset via RL + Plackett–Luce ranking, and bake explicit selection rationales into training trajectories.
3. **Design for proactivity and trustCalibration (Paper 3, Ericson).** Real-user data shows intent accuracy, engagement mechanisms, and cautious proactivity drive satisfaction ( turn counts up 45–71% with engagement, p < 0.001); and agentic search UX should adaptively modulate how much it pushes recommendations rather than being aggressively proactive by default.

---

*Sources: arXiv abstracts (https://arxiv.org/abs/2406.12276, https://arxiv.org/abs/2512.13278, https://arxiv.org/abs/2304.02233) plus full-text reading of 2406.12276 (§1, §3.2–3.4, §6.1, §6.2, App. E), 2512.13278 (§1, §3.2–3.4, §4, §5, §6.1, §8), and 2304.02233 (§3.1–3.2, §4.3, §5, §6.1.2, §7).*
