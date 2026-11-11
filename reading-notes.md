# Reading Notes: Agentic Search and LLM Tool Use

**Purpose:** Team literature brief for Thursday's team sync on agentic search — assistants where an LLM-driven agent searches, calls tools, and hands answers to users. This note summarizes three recent arXiv papers, what each says, and the concrete findings we should take away. Team members are encouraged to comment on this file via pull request discussion.

**Papers covered:**
1. arXiv:2304.02233 — Ericson: An Interactive Open-Domain Conversational Search Agent
2. arXiv:2406.12276 — CodeNav: Beyond tool-use to using real-world codebases with LLM agents
3. arXiv:2402.07632 — Understanding the Effects of Miscalibrated AI Confidence on User Trust, Reliance, and Decision Efficacy

**Metadata sources:** arXiv listing/search results for IDs, categories, and submission dates; full text read via arXiv HTML endpoint; abs pages fetched directly at arxiv.org/abs/{ID} for primary subject category and submission history.

---

## Paper 1: arXiv:2304.02233 — Ericson: An Interactive Open-Domain Conversational Search Agent

**Authors:** Zihao Wang, Ali Ahmadvand, Jason Ingyu Choi, Payam Karisani, Eugene Agichtein
**Subject category (from abs page):** Computation and Language (cs.CL); Artificial Intelligence (cs.AI) — primary: cs.CL
**Submission date (from abs page):** Wed, 5 Apr 2023 05:28:31 UTC (v1)
**arXiv ID:** 2304.02233 (v1)

**Abstract summary:** Ericson is a fully functional open-domain conversational search (ODCS) agent built from state-of-the-art question answering and information retrieval components plus intent inference and dialogue management for proactive refinement and recommendations. It was stress-tested in the Amazon Alexa Prize by engaging in live conversations with thousands of Alexa users, giving empirical grounding for analyzing the ODCS system in real settings. Interaction data showed that accurate intent classification, user-engagement encouragement, and careful proactive recommendations drive user satisfaction most, and the authors identify limitations of existing search techniques to inform the next generation of ODCS agents.

**Extracted findings:**
- **Finding 1 (Section 6.1.1, "Hierarchical Intent Classification"):** The two-level (general intent + Google Knowledge Graph-backed entity) classifier was evaluated on 4,593 user utterances with two-fold cross-validation. Adding the entity classifier as a second level significantly improved accuracy and F1 for all Sentiment (S) and Information Retrieval (IR) classes — e.g., News (IR) went from 0.865 ACC / 0.915 F1 (one-level, before) to 0.971 ACC / 0.963 F1 (after), Transition (T) F1 rose from 0.871 to 0.975, and entity-classifier-after Accuracy rose for Music (0.904→0.968) and Joke (0.948→0.962). GBDT beat Naive Bayes / Maximum Entropy / CNN on the Sentiment and IR groups. Design takeaway: a hierarchical, entity-aware intent classifier is a core component of a conversational search agent, and accuracy here maps directly to user satisfaction.
- **Finding 2 (Section 6, "Results and Discussion", incl. 6.2 qualitative analysis):** Intersection with real users (Alexa Prize deployment) showed that accurate intent classification, encouraging user engagement, and careful proactive recommendations "contribute most to the users' satisfaction," while the authors identify limitations in existing search techniques. Design takeaway: live-user testing is the highest-value evidence for conversational search agents, and proactivity must be tuned ("aggressiveness") rather than maximized.
- **Finding 3 (Section 7, "Limitations"):** The main limitation is that the study ran in a real-time setting, so component changes could affect other components; the short study window meant classifier, engagement, and proactivity changes could not be exclusively attributed to individual updates. However, the major improvements to the intent classifier, engagement, and proactive topic recommendation were significant enough to be credited for overall user-experience gains. Honest caveat: results are confounded by synchronous deployment.

**Takeaway for the team:** Conversational search agents work best when intent classification is hierarchical (entity-aware), proactivity is carefully calibrated, and you validate with real user conversation data — not just offline benchmarks.

---

## Paper 2: arXiv:2406.12276 — CodeNav: Beyond tool-use to using real-world codebases with LLM agents

**Authors:** Tanmay Gupta, Luca Weihs, Aniruddha Kembhavi
**Subject category (from abs page):** Artificial Intelligence (cs.AI); Computation and Language (cs.CL); Software Engineering (cs.SE) — primary: cs.AI
**Submission date (from abs page):** Tue, 18 Jun 2024 05:10:38 UTC (v1)
**arXiv ID:** 2406.12276 (v1)

**Abstract summary:** CodeNav is a single-agent, multi-environment interaction framework where an LLM agent "navigates" an unseen code repository to find relevant snippets, imports them, and iteratively generates a solution with execution feedback — avoiding the need to "register" every tool via manual descriptions in the LLM context. On three tool-use benchmarks (m&m's, M3 ToolEval, API-Bank), CodeNav is competitive with tool-use despite not having privileged tool-name/description access (which forms an upper bound). The authors also study how library-description richness and source code vs. natural-language descriptions affect performance, and open-source the code under a permissive license.

**Extracted findings:**
- **Finding 1 (Section 6.1, "How does code-use compare to tool-use on tool-use benchmarks?"):** On m&m's and API-Bank, code-use achieves similar or slightly lower tool-F1 than tool-use (e.g., m&m's: tool-use F1 79.6 ± 2.3 vs code-use 80.6 ± 5.1; API-Bank: tool-use F1 88.5 vs code-use 85.3), with only a minor hit on tool-recall since tool-use gets tool names while code-use must search to discover them. On M3 ToolEval (final-answer correctness), code-use is within two points of tool-use (89.3 accuracy vs 93.6), at the cost of ~2 more interaction steps on average. Design takeaway: agents that read repositories directly can match registered-tool agents on standard benchmarks, removing the manual "tool registration" bottleneck.
- **Finding 2 (Section 6.3, "Does seeing the source code help code-use?"):** On m&m's, returning actual code for the top-3 retrieval matches yields higher F1 (81.2 ± 2.3) and fewer interaction steps (7.3 ± 0.2) than showing prototypes only (80.0 ± 5.2 / 7.8 ± 0.1); CodeNav's default (code + GPT-4-generated docstrings, shortest of the two) gets 80.6 ± 5.1 / 7.2 ± 0.2 — higher than prototypes but lower than raw code. The authors argue "well-written code is its own documentation" and that source code gives strictly more information than function signatures. Design takeaway: prefer returning real code blocks over signature-only retrieval responses.
- **Finding 3 (Appendix E, "Limitations"):** Three key limitations are acknowledged: (1) CodeNav assumes the agent produces Python and the codebase is Python; performance may degrade for less-represented languages; (2) the agent has no long-term memory across queries, so repeated queries repeat the same errors and searches; (3) performance is strongly dependent on the underlying LLM and degrades sharply with smaller LLMs, making most researchers depend on paid API access for larger-scale runs. Societal-impact note (Appendix F): because CodeNav can run arbitrary code on the user's machine, it has a wider attack surface than tool-use agents — a logic error could accidentally delete files, and intercepted/tainted inputs could enable malware-style abuse.

**Takeaway for the team:** "Code-use" (read the repo instead of registering tools) is competitive with today's tool-use paradigm, and real source code beat signature-only retrieval — but we still need long-term memory, multi-language support, and cost awareness, and we should weigh the wider security surface of agents that execute arbitrary code.

---

## Paper 3: arXiv:2402.07632 — Understanding the Effects of Miscalibrated AI Confidence on User Trust, Reliance, and Decision Efficacy

**Authors:** Jingshu Li, Yitian Yang, Renwen Zhang, Q. Vera Liao, Tianqi Song, Zhengtao Xu, Yi-chieh Lee
**Subject category (from abs page):** Artificial Intelligence (cs.AI); Human-Computer Interaction (cs.HC) — primary: cs.AI
**Submission date (from abs page):** v1 Mon, 12 Feb 2024 13:16:30 UTC (latest v4 Fri, 26 Sep 2025 07:48:29 UTC)
**arXiv ID:** 2402.07632 (current version v4)

**Abstract summary:** This HCI study ran two within-subjects experiments (EXP1 N=126, EXP2 N=126) on AI-assisted decision making. EXP1 found that miscalibrated AI confidence impairs users' appropriate reliance and reduces AI-assisted decision-making efficacy, and that users cannot easily detect miscalibration — they over-rely on overconfident AI and under-rely on underconfident AI. EXP2 tested whether communicating calibration levels helps: it improved users' ability to detect miscalibration but decreased trust in uncalibrated AI, causing high under-reliance and no improvement in decision efficacy. The authors offer design implications and discuss risks/ethics of uncalibrated AI confidence.

**Extracted findings:**
- **Finding 1 (Section 5.3, "AI Confidence Miscalibration Hinders AI-assisted Decision Making"):** Without communicating calibration, participants relied more on overconfident AI (over-reliance) and less on underconfident AI (under-reliance) — combining to decline in AI-assisted decision efficacy. Trust in AI did not significantly change in EXP1 because participants did not recognize the miscalibration. Communicating calibration reduced trust in uncalibrated AI, leading to under-reliance on both over- and underconfident AI and lower decision efficacy than well-calibrated conditions (Section 5.3). Design takeaway: merely displaying a confidence number doesn't fix miscalibration; uncalibrated confidence scores are actively harmful and hard for users to self-diagnose.
- **Finding 2 (Section 5.5, "Limitations and Future Work"):** The work has four acknowledged limitations: (1) only the numerical confidence-score form of uncertainty was used; discrete-class or LLM-style verbalized uncertainty representations were not tested; (2) real-world miscalibration is more complex/diversified than the experiment (e.g., mixed over/under-confident behavior per model/issue); (3) generalizability is limited to generic AI-assisted decision making (city image recognition) and high-risk domains like healthcare/military may produce different, more conservative behaviors under loss aversion; (4) participants were general online users, so AI-literacy differences (elderly, technophobes, students) may shift results. Future work called for better AI literacy, disclosure/monitoring of miscalibration, and research on how users perceive calibration across uncertainty representations.

**Takeaway for the team:** When we ship LLM agents that call tools and return confidence scores, we must treat confidence calibration as a trust/safety feature, not just a UX detail — miscalibrated scores erode appropriate reliance, and simply telling users "we calibrated it" can backfire by trimming trust too far. Invest in calibration metrics and user-facing interpretability beyond raw numbers.

---

## Cross-cutting takeaways for the team sync

1. **Agentic search vs. tool use is a spectrum:** Ericson (2304.02233) shows the human-centered, intent-driven side of conversational search agents; CodeNav (2406.12276) shows the engineering side where agents read repositories instead of calling pre-registered tools. Our own assistants should probably do both: retrieve + refine (Ericson) and use real-world artifacts (CodeNav).
2. **Benchmark parity is achievable without tool registration:** CodeNav's results (Section 6.1) show code-use within ~2 points of tool-use upper bound on standard benchmarks — a strong argument for letting agents explore their environment rather than hand-curate tool registries.
3. **User trust is the hardest part:** Paper 3 (2402.07632) is the HCI-side counterpoint: even a competent agent fails if users can't detect miscalibration and if confidence display backfires. Calibrated confidence + honest communication is a prerequisite, not an add-on.
4. **Known limitations to carry into our design:** No long-term memory across queries (CodeNav App. E), Python-only assumption, paid-API dependency for strong LLMs, and real-time-deployment confound (Ericson Section 7) — schedule budget and memory for these early.

---

## Sources

- arXiv abs pages (primary category + submission history): arxiv.org/abs/2304.02233, arxiv.org/abs/2406.12276, arxiv.org/abs/2402.07632
- Full texts read via arXiv HTML endpoints (v1 for 2304.02233 and 2406.12276; v4 for 2402.07632)
- Sections cited: Ericson Section 6.1.1 (Hierarchical Intent Classification), Section 6 (Results and Discussion), Section 7 (Limitations); CodeNav Section 6.1, Section 6.3, Appendix E (Limitations), Appendix F (Societal impact); 2402.07632 Section 5.3, Section 5.5 (Limitations and Future Work)

*File committed on branch `add-reading-notes` (off `main`) of repo `mcpmark-eval-liuhezi/arxiv-agentic-search-notes`, via pull request "Add reading notes: agentic search and tool use". Team members: please comment on the PR or the file to add discussion points before Thursday's sync.*
