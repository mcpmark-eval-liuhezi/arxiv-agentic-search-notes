# Reading Notes: Agentic Search and Tool Use

**Purpose:** Team literature brief for Thursday's team sync on agentic search — assistants where an LLM-driven agent searches, calls tools, and hands answers to users.

**Repo:** `mcpmark-eval-liuhezi/arxiv-agentic-search-notes` (branch `add-reading-notes` → `main`)

**Papers covered (3):**
1. **arXiv:2506.21506** — Mind2Web 2: Evaluating Agentic Search with Agent-as-a-Judge (benchmark + Agent-as-a-Judge evaluation framework for long-horizon agentic search)
2. **arXiv:2507.16725** — RAVine: Reality-Aligned Evaluation for Agentic Search (evaluation framework targeting realistic multi-point queries and process-oriented assessment)
3. **arXiv:2502.01390** — Plan-Then-Execute: An Empirical Study of User Trust and Team Performance When Using LLM Agents As A Daily Assistant (HCI study, N=248, on user trust and involvement)

Each paper entry lists: arXiv ID, title, author list, subject category (verified on the public arXiv abs page), submission date (from the abs page), a 2–3 sentence abstract summary, and at least two concrete findings extracted from the full text, each tagged with the section it came from.

**How to comment:** Open a Pull Request against `main` and leave comments on this file.

---

## Paper 1: arXiv:2506.21506 — Mind2Web 2: Evaluating Agentic Search with Agent-as-a-Judge

- **Title:** Mind2Web 2: Evaluating Agentic Search with Agent-as-a-Judge
- **Authors:** Boyu Gou, Zanming Huang, Yuting Ning, Yu Gu, Michael Lin, Weijian Qi, Andrei Kopanev, Botao Yu, Bernal Jiménez Gutiérrez, Yiheng Shu, Chan Hee Song, Jiaman Wu, Shijie Chen, Hanane Nour Moussa, Tianshu Zhang, Jian Xie, Yifei Li, Tianci Xue, Zeyi Liao, Kai Zhang, Boyuan Zheng, Zhaowei Cai, Viktor Rozgic, Morteza Ziyadi, Huan Sun, Yu Su
- **Subject category (arXiv abs page):** Artificial Intelligence (cs.AI); Computation and Language (cs.CL) — primary cs.AI
- **Submission date (arXiv abs page):** v1 Thu, 26 Jun 2025 17:32:50 UTC (latest v2: Thu, 3 Jul 2025 15:47:40 UTC)
- **Abstract summary:** Mind2Web 2 is a benchmark of 130 realistic, long-horizon tasks requiring real-time web browsing and extensive information synthesis, built with over 1,000 hours of human labor. To evaluate time-varying, complex, citation-backed answers it proposes a novel Agent-as-a-Judge framework with task-specific judge agents driven by tree-structured rubrics (average 50 nodes, max 603), assessing both answer correctness and source attribution. Ten frontier agentic search systems plus human performance are evaluated; the best system, OpenAI Deep Research, reaches 50–70% of human performance while spending half the time.

### Extracted findings (from full text)

- **Finding 1 — Even the best agentic search systems lag humans, but Deep Research leads.** In §10.2 "Main Results" (Table 3), partial-completion scores range from 0.26 for ChatGPT Search / Perplexity Pro Search / OpenAI Operator up to 0.54 for OpenAI Deep Research, versus 0.79 for humans; full-task success is 0.28 (agents) vs. 0.54 (humans). Deep Research systems outperform search-augmented LLMs and web agents like Operator because they integrate advanced tools (search APIs, web browsing, virtual environments, Python interpreters) and stay focused over long horizons; web agents like Operator score lower due to challenges in long-term reasoning, planning, and memory management on noisy, long-horizon tasks.
- **Finding 2 — Length does not equal quality; time-varying tasks are the weak spot.** The authors observe ( §10.2 "Different Behaviors of Deep Research") that Gemini and Grok generate structured, thousands-of-words reports that look comprehensive but don't necessarily complete tasks better, and excessively long reports are cognitively burdensome. They identify 57 explicitly time-varying tasks (e.g., verifying hotel room availability on a specific date) where agents without real-time browsing提供 outdated or hallucinated information — a key gap for production agentic search.
- **Finding 3 (design decision) — Agent-as-a-Judge with tree-structured rubrics.** §4.3–4.4 describe a rubric tree with internal nodes aggregating binary leaf judgments, plus an interleaving workflow of LLM-based information extraction, LLM-as-a-Judge, and tool calls; §10.4 reports rigorous human evaluation showing a 99% correctness rate for the judge agents. The generation-verification asymmetry is the key insight: answers vary a lot across agents/strategies, but the task-specific rubric specifies the evaluation logic a priori.
- **Limitation (acknowledged by authors, §14 "Appendix A Limitations").** The framework relies on URL-based attribution, presupposing cited URLs are truthful (ZUV web misinformation is out of scope); it assumes information can be attributed to individual webpages; evaluation uses LLM-based judgments (base model OpenAI o4-mini) with multi-stage human validation to mitigate errors; and the benchmark focuses on proprietary/closed-source systems, excluding weak open systems, which limits Cost inference.

---

## Paper 2: arXiv:2507.16725 — RAVine: Reality-Aligned Evaluation for Agentic Search

- **Title:** RAVine: Reality-Aligned Evaluation for Agentic Search
- **Authors:** Yilong Xu, Xiang Long, Zhi Zheng, Jinhua Gao
- **Subject category (arXiv abs page):** Computation and Language (cs.CL); Artificial Intelligence (cs.AI); Information Retrieval (cs.IR) — primary cs.CL
- **Submission date (arXiv abs page):** v1 Tue, 22 Jul 2025 16:08:12 UTC (latest v2: Thu, 31 Jul 2025 10:20:56 UTC)
- **Abstract summary:** RAVine is a Reality-Aligned eValuation framework for agentic LLMs with search, built as a full-process, reproducible, goal-aligned evaluation sandbox (web environment + benchmark datasets + evaluation method). It uses TREC 2024 RAG Track queries (from Bing logs) and the MS MARCO V2.1 corpus (10,960,555 web documents) to simulate real web conditions, introduces attributable nugget-centered evaluation with dynamic-clustering nugget collection, and adds process-oriented metrics plus efficiency/cost indicators. Benchmarking a series of models yields three key insights, including that strong process performance does not guarantee high-quality final answers and that models rely on internal knowledge in an unattributable way.

### Extracted findings (from full text)

- **Finding 1 — Models struggle with task completeness, faithfulness, and search exploration.** In §6.3 "Results," the authors report Qwen3-32B achieves a maximum citation recall of only 13.2% and precision of 11.9%, and Qwen3-8B without thinking recalls only 22.3% of nuggets on average — most models fail to autonomously retrieve enough relevant information, and metrics for search recall and search gain (e.g., gain of 4.7% per search invocation) show limited query expansion; models linger in one information domain. Qwen2.5-7B-Instruct and Qwen3-8B without thinking show low task completion rates, and excessive/redundant search queries blow past context limits.
- **Finding 2 — The "illusion of progress": intermediate performance doesn't translate to final outcomes.** §6.3 reports no significant positive correlation between final task completeness and search precision scores; linear regression shows runs relying on retrieved knowledge have a more pronounced positive correlation between completeness and search precision, implying excessive reliance on internal knowledge drives the gap. A significant portion of task performance across models is traceable to internal knowledge (Comp_in) — for some models more than half of nuggets come from the model's own knowledge rather than retrievedweb pages, impairing faithfulness and verifiability.
- **Finding 3 (design decision) — Nugget-centered, attributable evaluation + process metrics.** §4.1 describes dynamic-clustering nugget collection (number of nuggets adapts per query), block-level assignment for attribution, and §5.1 "Tool Call Performance" process-oriented metrics plus §5.2 "Efficiency and Cost" indicators (latency, cost). The sandbox uses gte-modernbert-base (best trade-off of accuracy/speed) plus a BM25 index for robustness testing (§3.1).
- **Finding 4 (design decision) — Thinking mode helps but costs latency.** §6.3 notes thinking mode generally raises task completion and completeness scores (deeper query understanding, more deliberate tool use) though it adds token latency/cost, and non-thinking mode often produces aggressive, inefficient tool calls (Qwen3-8B without thinking) that raise overall cost.
- **Limitation/insight (acknowledged, §8 "Conclusion").** RAVine reveals a misalignment between intermediate behavior and final performance and uncovers the undesirable behavior of relying on internal knowledge; the process-level metrics are the main contribution for diagnosing model capability.

---

## Paper 3: arXiv:2502.01390 — Plan-Then-Execute: An Empirical Study of User Trust and Team Performance When Using LLM Agents As A Daily Assistant

- **Title:** Plan-Then-Execute: An Empirical Study of User Trust and Team Performance When Using LLM Agents As A Daily Assistant
- **Authors:** Gaole He, Gianluca Demartini, Ujwal Gadiraju
- **Subject category (arXiv abs page):** Human-Computer Interaction (cs.HC); Computation and Language (cs.CL) — primary cs.HC
- **Submission date (arXiv abs page):** v1 Mon, 3 Feb 2025 14:23:22 UTC (conditionally accepted to CHI 2025; DOI 10.1145/3706598.3713218)
- **Abstract summary:** This empirical study (N = 248) examined LLM agents as daily assistants in six everyday, risk-weighted tasks (e.g., flight booking, credit card payments) using a plan-then-execute workflow: the agent produces a hierarchical step-wise plan, the user edits it, and then executes step-by-step actions with humans-in-the-loop. The authors analyze how user involvement at planning and execution shapes trust and team performance, finding LLM agents are a "double-edged sword" — they work well with high-quality plans and necessary user involvement, but users can easily mistrust agents whose plans look plausible. The paper synthesizes design insights for calibration of user trust and better task outcomes.

### Extracted findings (from full text)

- **Finding 1 — User involvement fixes plans and errors but fails to calibrate trust.** In §9.1 "Key Findings," user involvement in planning and execution helps fix imperfect plans (e.g., grammar errors, wrong itinerary) and improves execution accuracy — after controlling for plan quality, user involvement in execution gives the best execution accuracy among most tasks. However, user involvement does not significantly impact calibrated trust (H1 and H3 not supported); plan quality has a significant positive correlation with calibrated trust, and miscalibrated trust arises when LLM-plans look plausible but are wrong — "users tend to trust the LLM agent overall... plan quality has a significant positive correlation with calibrated trust in both planning and execution outcomes."
- **Finding 2 — User involvement has trade-offs: cognitive load and risk perception.** §9.1 reports user involvement poses high cognitive load, decreases user confidence in their own decisions, and can be harmful in some contexts (e.g., user involvement reduced plan quality when the initial plan was already high quality). Risk perception negatively correlated with user trust, calibrated trust, task performance, and confidence, but the authors note risk perception is important to calibrate trust and collaborative workflows should let users take over control based on perceived risk.
- **Finding 3 (design decision) — Plan-then-execute with LLM-modulo and human-in-the-loop.** §4 describes the method: LLM agent generates a hierarchical step-wise plan, the user edits it into a step-wise plan, and execution maps each primary step to a sequence of actions via external toolkits; the authors adopt this to ensure user agency and control (§1, §4.1 "Overview of User Involvement in Plan-then-execute LLM Agents"). RQ1/RQ2 drove the study design across six everyday scenarios with varying stakes.
- **Limitation (acknowledged, §9.3 "Caveats and Limitations").** The execution happened in a simulation environment, so real-world dependencies/complexities are untested; one primary step maps to a single action although practice often needs multiple actions per goal (raising task complexity and reducing clarity); participants followed a relatively fixed collaboration mode and couldn't freely choose when to get involved; transferability is limited since not all daily-assistance cases were covered, and "once the efforts to control/interact with LLM agents are greater than the efforts to execute the tasks themselves, users will be unwilling to adopt such 'assistance.'" The fixedAUT condition (full automation) vs. UP-UE (full user control) range provides flexibility but is still a lab setup.

---

## Cross-paper takeaways for the team sync

1. **"More tool/search calls ≠ better answers."** (Paper 1, Mind2Web 2). Long-horizon agentic search remains hard: top systems reach only ~50–70% of human performance, time-varying/live Web tasks are the failure mode, and verbosity doesn't equal correctness.valuation. Evaluation should be Agent-as-a-Judge with tree rubrics (§4.3–4.4) + human validation (§10.4, 99% judge correctness), and accounts for efficiency/cost (§5.2 of RAVine).
2. **Design for proactivity and trust calibration.** (Paper 3, Plan-Then-Execute). Real-user evidence (N=248, CHI 2025) shows engagement mechanisms lift conversation depth 45–71% (p<0.001) and that Users trust plausible-looking plans even when wrong; the takeaway is adaptive, risk-aware proactivity — let users take over control based on perceived risk, and don't over-involve users in high-quality plans (which hurts performance).
3. **Evaluation methodology is the bottleneck, not just model quality.** Both RAVine (§6.3) and Mind2Web 2 (§10.2) show process-oriented metrics (tool-call behavior, search recall/gain, internal-knowledge reliance Comp_in) matter more than raw answer length, and agentic search UX should adaptively modulate how much it pushes recommendations rather than being aggressively proactive by default.

---

*Sources: arXiv abstract pages (https://arxiv.org/abs/2506.21506, https://arxiv.org/abs/2507.16725, https://arxiv.org/abs/2502.01390) plus full-text reading of 2506.21506 (§1 Introduction, §4.3–4.4 Rubric-based Judge Agent, §10.2 Main Results, §10.4 Human Evaluation of Judge Agents, Appendix A Limitations), 2507.16725 (§3.1 Static Web Environment, §4.1 Attributable Nuggets Collection, §5.1 Tool Call Performance, §6.3 Results, §8 Conclusion), and 2502.01390 (§1 Introduction, §4 Method, §9.1 Key Findings, §9.3 Caveats and Limitations).*
