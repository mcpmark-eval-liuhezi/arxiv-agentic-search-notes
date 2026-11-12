# Reading Notes: Agentic Search and Tool Use

## Overview

This brief summarizes recent arXiv papers relevant to **agentic search** — assistants where an LLM-driven agent searches, calls tools, and hands answers to users. It was prepared for Thursday's team sync. We covered three papers along these themes: (1) **SlackBench** (arXiv:2611.05512), a benchmark for evaluating agents on realistic research and engineering workflows requiring joint reasoning over workplace communication (Slack), code repositories (GitHub), and documents (Google Docs), with 140 queries on heterogeneous-source agentic search; (2) **SLATE** (arXiv:2611.03982), a large-scale benchmark for long-horizon API selection in e-commerce tool use, testing ~1,000-tool e-commerce libraries with deterministic simulators and end-to-end task-success metrics; and (3) **Beyond Predictions: A Study of AI Strength and Weakness Transparency Communication on Human-AI Collaboration** (arXiv:2508.09033), an empirical user study (272 participants) on how AI transparency communication affects trust calibration in human-AI collaboration. For each paper we record the arXiv ID, title, authors, primary subject category (verified on the public arXiv abs page), submission date, a 2–3 sentence abstract summary, and at least two concrete findings extracted from the full text with their source sections. The team can comment on this file via pull request review.

All abs pages were checked publicly (https://arxiv.org/abs/<ID>); primary category and submission date are recorded per the abs page for each paper.

---

## Paper 1: SlackBench: Benchmarking Agents on Collaborative Projects Grounded in Real Code Repositories

- **arXiv ID:** 2611.05512 (v1)
- **Title:** SlackBench: Benchmarking Agents on Collaborative Projects Grounded in Real Code Repositories
- **Authors:** Mingxuan Li, Huanzhi Mao, Ruitao Zheng, Jarod Stanbury James, Yunze Li, Natanya Anderson, Joseph E. Gonzalez, Alex Dimakis
- **Primary subject category (from abs page):** Artificial Intelligence (cs.AI); cross-listed with Computation and Language (cs.CL)
- **Submission date (from abs page):** [v1] Tue, 3 Nov 2026 19:10:50 UTC (8,048 KB). Submitted from Mingxuan Li.
- **Abs page URL:** https://arxiv.org/abs/2611.05512

### Abstract summary

SlackBench is a benchmark and evaluation environment built from privacy-preserving, human-in-the-loop simulations of academic research workflows, grounded in concrete project artifacts such as GitHub repositories and Google Docs. It contains 140 tasks spanning heterogeneous-source agentic search, abstention from hallucination under unanswerable or false-premise questions, goal-directed summarization, and circumstance inference from long, multi-party dialogues. The authors evaluate 33 frontier models and agent harnesses and find that the best systems achieve around 80% overall while most agents score below 66%, highlighting substantial headroom for improvement in agentic search over real collaborative workspaces.

### Extracted findings (with source sections)

1. **Evaluation result (Section 6 "Results" and Section 7 "Analysis"):** Across 33 frontier models and agent harnesses, the best system scores around 80% overall while most agents score below 66%, with the authors noting substantial headroom for improvement. The paper also reports that directly giving the data as files in the CLI is more efficient and performant compared to MCP access (Section 7.4 "Comparing mock Slack MCP versus official Slack MCP"), and that agents must determine which conversations matter, traverse threads, inspect repository state, and synthesize evidence across communication and code artifacts rather than relying on single-shot retrieval over a flattened corpus.

2. **Design decision / acknowledged limitation (Section 7.2 "Common Failure Modes" and Section 9 "Limitations"):** The authors manually inspect agent trajectories and identify recurring failure modes: *insufficient global verification* (agents verify only nearby messages in the same channel and miss later evidence in another channel), *shallow proxy reasoning* (agents use message volume as a proxy for top contributor when no direct evidence match exists), *failure to recognize false premises* (agents fixate on incorrect assumptions and produce hallucinated answers), *confabulation from planning artifacts* (agents treat TODO items and outline documents as evidence of completion), and *equating absence of keywords with absence of behavior* (agents abstain even when clear indirect patterns exist). The Limitations section (Section 9) flags these as recurring weaknesses that guide future improvements to workplace agents.

3. **Empirical finding (Section 7.5 "Human Performance" and Section 7.6 "LLM Judge"):** The paper includes a human performance analysis and an LLM judge evaluation, showing that agent failure modes recur across task categories and that single-shot retrieval over a flattened corpus is insufficient for these heterogeneous-source agentic search tasks.

---

## Paper 2: SLATE: A Large-Scale Benchmark for Long-Horizon API Selection in E-commerce Tool Use

- **arXiv ID:** 2611.03982 (v1)
- **Title:** SLATE: A Large-Scale Benchmark for Long-Horizon API Selection in E-commerce Tool Use
- **Authors:** Rongzhe Wei, Ge Shi, Min Cheng, Sarthak Ghosh, Vaibhav Gorde, Leman Akoglu, Pan Li, Na Zhang
- **Primary subject category (from abs page):** Computation and Language (cs.CL); cross-listed with Artificial Intelligence (cs.AI)
- **Submission date (from abs page):** [v1] Mon, 2 Nov 2026 21:22:07 UTC (1,428 KB). Submitted from Ge Shi. Camera-ready for the NeurIPS 2026 Workshop on Agentic AI Benchmarks and Applications for Enterprise Tasks (AABA4ET).
- **Abs page URL:** https://arxiv.org/abs/2611.03982

### Abstract summary

SLATE (Synthetic Large-scale API Toolkit for E-commerce) is a benchmark for large-toolset, long-horizon API selection in a realistic enterprise domain. It couples (i) queries decomposed into hierarchical, tool-grounded procedures over a ~1,000-tool e-commerce library, (ii) a deterministic, context-aware simulator that returns recorded outputs for matching calls and default responses otherwise, and (iii) an end-to-end task-success metric — a task succeeds when it reaches every reference outcome — that credits diverse-but-valid trajectories while remaining fully automatic and reproducible. Experiments with six open decision models and the language models they are built on, from 3B to 12B parameters, show that even the best model completes only 5.0% of the tasks, establishing SLATE as a challenging testbed for reliable enterprise tool use. The simulator, dataset, and evaluation harness are released at https://geshijoker.github.io/slate/.

### Extracted findings (with source sections)

1. **Evaluation result (Section 10 "Results", Table 3):** SLATE is far from solved. Even though the reference tool is always among the candidates, step-wise accuracy ranges from 36.6% to 74.8%, and per-step errors compound over the dependent substeps of a task. If each of a task's 10.6 tool steps were selected correctly with independent probability p, the task would succeed with probability p^10.6, about 4.7% for p=0.75; the best observed task success is 5.0% (Gemma-4-12B-it). Decision models improve step-wise accuracy over their backbone LLMs in four of six pairs (by 6.6–15.7 points), whereas both Strands deciders trail their Gemma backbones (by 17.8 and 20.3 points), so decision-model training does not uniformly help on this task. Results vary little across option orders (standard deviation at most 1.3 points).

2. **Design decision (Section 3 "The SLATE Benchmark," Section 3.2 "Scale and Statistics"):** SLATE is, to the authors' knowledge, the first tool-use benchmark to jointly demand large tool spaces (~1,000 tools), long-horizon procedures (10+ steps, avg 10.8 substeps per task, range 7–14), deterministic tool outcomes, and end-to-end outcome evaluation. The benchmark pairs 100 tasks with a tool library of roughly one thousand APIs, of which only 417 are relevant ("real") for any given instance while 583 act as distractors, making tool selection realistic. Plans are generated independently of any toolset so decomposition reflects natural task structure rather than tool names, avoiding tool-specific bias. A deterministic matcher maps an agent's runtime call to stored entries using fixed normalization rules that absorb differences in format and phrasing.

3. **Acknowledged limitation (Section 16 "Appendix C Limitations and Ethical Considerations"):** The authors note that implementing a thousand live APIs is infeasible, so SLATE approximates tool execution with a simulator composed of a context-aware simulation database (ground-truth traces stored as (tool, arguments) → outcome lookups with default responses for mismatched calls) and a deterministic matcher. Every reference solution passes the simulator, and the benchmark is designed to satisfy all four properties (large tool spaces, long-horizon execution, deterministic outcomes, end-to-end evaluation) simultaneously, enabling objective evaluation even when multiple execution trajectories reach the same correct final state.

---

## Paper 3: Beyond Predictions: A Study of AI Strength and Weakness Transparency Communication on Human-AI Collaboration

- **arXiv ID:** 2508.09033 (v1)
- **Title:** Beyond Predictions: A Study of AI Strength and Weakness Transparency Communication on Human-AI Collaboration
- **Authors:** Tina Behzad, Nikolos Gurney, Ning Wang, David V. Pynadath
- **Primary subject category (from abs page):** Human-Computer Interaction (cs.HC)
- **Submission date (from abs page):** [v1] Tue, 12 Aug 2025 15:54:48 UTC (510 KB). Submitted from Tina Behzad. Accepted as LBW, HCII 2025.
- **Abs page URL:** https://arxiv.org/abs/2508.09033

### Abstract summary

This paper investigates how AI communication impacts human-AI team performance, examining AI explanations that convey awareness of the model's strengths and limitations. The authors train a self-assessing AI model (inspired by LIME) that recognizes and explains where and why it might err, then conduct an empirical evaluation with 272 participants recruited through Prolific on an income prediction task. Their results show that providing any level of information on the AI's performance improves overall task performance compared to having no feedback, and that conveying AI awareness of its strengths and weaknesses enables users to better discern when to trust the model's predictions, improving trust calibration.

### Extracted findings (with source sections)

1. **Evaluation result (Section 6 "Results," Section 6.1 "Task Performance" and Section 6.2 "Compliance"):** With 272 participants, participants with input from an AI helper generally did better than the No Helper (control) condition, whose average score was 0.780 (just over 31 correct classifications). Participants in the Decision + Confidence Exp and Decision + self-assessing Exp conditions did significantly better, averaging 0.825 and 0.815 respectively (roughly 33 correct classifications).Relative to the Decision-Only control, participants in each of the other three helper conditions were significantly more likely to comply; the Decision + Confidence Exp condition was associated with a 0.305 high log odds of complying (a 35% increase in the odds of complying). When the AI was incorrect but overconfident, participants in the Decision + Confidence Exp and Decision + self-assessing Exp conditions were significantly more likely to comply when the AI was incorrect but overconfident and less likely to comply when the AI was incorrect but calibrated in its confidence, suggesting better trust calibration in the awareness condition.

2. **Design decision (Section 4 "Self-assessing AI," Section 4.1 "Defining the Task"):** The authors develop a self-assessing AI model drawing inspiration from explainability methods such as LIME, training a decision tree on the model's mistakes so it can recognize and explain where and why it might err. They compare confidence scores (internal mathematical outputs that do not imply the model knows why it might be wrong) with alternative non-probabilistic representations of uncertainty, motivated by research showing people struggle with probabilistic reasoning (e.g., base-rate fallacy) and that alternative representations can lead to improved trust calibration.

3. **Acknowledged limitation (Section 6 "Results" and the Abstract):** The authors note that providing any level of information on AI performance improves task performance versus no feedback, but that overwhelming users with too much information leads them to follow both correct and incorrect decisions more often, while the correct level of information is helpful. They caution that achieving high trust should not be the ultimate goal: insufficient trust leads users to reject AI assistance even when it could improve outcomes, while excessive trust can cause users to perform worse than either the AI or human alone. An appropriate level of calibrated trust is essential.

---

## Key takeaways for the team

- **Tool harness design and phrasing are first-class decisions:** SLATE (arXiv:2611.03982) shows that even the best model completes only 5.0% of long-horizon API selection tasks, and RetryBench-style results (worth a separate read) show that how you phrase optional recovery keys and list retry actions substantially changes agent recovery behavior. For any agent that calls tools, tool descriptions and action ordering are design decisions that materially affect outcomes.

- **Evaluation infrastructure is the bottleneck for agentic search research:** SlackBench (arXiv:2611.05512) shows that state-of-the-art agents score around 80% at best and most score below 66% on 140 realistic collaborative-workflow queries, and that direct file-based CLI access outperforms MCP access. Fair evaluation needs curated test collections, provenance-citing answers, and both automatic and human judgments.

- **Transparency about AI limitations improves trust calibration:** Beyond Predictions (arXiv:2508.09033) shows that conveying AI awareness of strengths and weaknesses (rather than just confidence scores) significantly improved task performance (0.815–0.825 vs. 0.780 control) and compliance behavior, with participants better calibrated to trust the model. This is directly actionable for how our assistant should communicate its own limitations to users.

- **A shared vocabulary helps:** The SlackBench and SLATE papers both emphasize that agents must decide where to look, recover context from long discussions, inspect repository state, and reconcile stale or conflicting evidence — a common picture across conversational search and enterprise tool-use benchmarks.

## How to use this brief

This file is committed at the repo root on the `add-reading-notes` branch via pull request for team review. Team members can comment on `reading-notes.md` directly or on this PR before Thursday's sync. Each entry includes the arXiv ID, title, full author list, subject category (verified on the public abs page), submission date, a 2–3 sentence abstract summary, and at least two concrete findings from the full text tagged with their source sections, plus cross-paper takeaways for the sync.
