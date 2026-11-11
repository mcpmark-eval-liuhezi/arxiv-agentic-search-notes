# Reading Notes: Agentic Search and LLM Tool Use

**Purpose:** Team literature brief for Thursday's team sync on agentic search — assistants where an LLM-driven agent searches, calls tools, and hands answers to users. This note summarizes recent arXiv papers on three connected themes: (1) building and evaluating open-domain conversational search agents (Ericson), (2) moving beyond registered tool-use to code-use agents (CodeNav), and (3) how AI assistants should adapt to user trust (Adjust for Trust). It also retains two benchmarking papers already curated for the team (Mind2Web 2 and RAVine). Each entry includes arXiv ID, title, full author list, subject category (verified on the paper's public arXiv abs page), submission date, a 2–3 sentence abstract summary, and at least two concrete findings extracted from the full text with their source sections. Team members are encouraged to comment on this file or the pull request before Thursday's sync.

**Papers covered:**
1. arXiv:2304.02233 (v1) — Ericson: An Interactive Open-Domain Conversational Search Agent
2. arXiv:2406.12276 (v1) — CodeNav: Beyond tool-use to using real-world codebases with LLM agents
3. arXiv:2502.13321 (v2) — Adjust for Trust: Mitigating Trust-Induced Inappropriate Reliance on AI Assistance
4. arXiv:2506.21506 (v2) — Mind2Web 2: Evaluating Agentic Search with Agent-as-a-Judge (retained from prior curation)
5. arXiv:2507.16725 (v2) — RAVine: Reality-Aligned Evaluation for Agentic Search (retained from prior curation)

**Method note:** Papers were located via arXiv search on phrasings such as "conversational search agents", "agentic search", "tool use in LLM agents", and "user trust in AI assistants". Full texts were read via the arXiv HTML endpoint (Ericson, CodeNav, Adjust for Trust), and each paper's public abs page at arxiv.org/abs/{ID} was fetched directly to confirm the primary subject category and submission history. All three abs pages loaded successfully.

---

## Paper 1: arXiv:2304.02233 (v1) — Ericson: An Interactive Open-Domain Conversational Search Agent

**Authors:** Zihao Wang, Ali Ahmadvand, Jason Choi, Payam Karisani, Eugene Agichtein

**Subject category (from abs page):** Computation and Language (cs.CL); Artificial Intelligence (cs.AI) — primary: cs.CL (MSC: 68T50, 68T07)
**Submission date (from abs page):** Wed, 5 Apr 2023 05:28:31 UTC (v1)

**Abstract summary:** Ericson is a fully functional open-domain conversational search (ODCS) agent that combines state-of-the-art question answering and information retrieval components with intent inference and dialogue management for proactive question refinement and recommendations. It was stress-tested in the Amazon Alexa Prize by engaging in live conversations with thousands of Alexa users, giving an empirical basis for analyzing User satisfaction in real settings. Interaction-data analysis showed that accurate intent classification, encouraging user engagement, and careful proactive recommendations contribute most to user satisfaction, and the study identifies limitations of existing search techniques as a building block for the next generation of ODCS agents. The system is deployed as an Alexa Skill on Amazon EC2 via the Alexa Skills Kit Lambda function.

**Extracted findings:**
- **Finding 1 (Section 6.1.1 "Hierarchical Intent Classification"):** The hierarchical intent classifier (general intent classifier + Google Knowledge Graph-backed entity classifier) is crucial to system performance. On a dataset of 4,593 user utterances, GBDT outperformed Naive Bayes/Max-Entropy/CNN for the Sentiment (S) and Information Retrieval (IR) groups (e.g., News IR: NB acc 0.947/F1 0.768 vs GBDT acc 0.928/F1 0.887; Weather IR: GBDT acc 0.932/F1 0.905 vs NB acc 0.970/F1 0.899). Adding the second-level entity classifier significantly improved F1/accuracy for all S and IR classes (Table 5: e.g., News IR accuracy 0.865 → 0.971, F1 0.812 → 0.963), and the authors state "for all S and IR groups, the hierarchical classification system outperforms the one-level classification system."
- **Finding 2 (Section 6 "Results and Discussion" / §6.1.2 "Engagement" and §6.1.3 "Proactivity"):** Interaction data analysis revealed that accurate intent classification, encouraging user engagement, and careful proactive recommendations contribute most to user satisfaction. The system's Dialogue State Manager controls conversational flow based on intent classification and current dialogue state, and the conversational transition component suggests follow-up topics (e.g., after "hello" classified as Small-Talk, it suggests a popular follow-up topic like "Celebrity").
- **Finding 3 (Section 7 "Limitations"):** The main limitation was that the study was performed in a real-time setting, so some component changes had simultaneous effects on other components; and because the user study window was limited, changes to classifiers, engagement, and proactivity components were completed within a short time period, making it challenging to exclusively attribute ratings to individual component updates. The authors note the effects of major improvements to the intent classifier, engagement, and proactive topic recommendation were significant enough to be credited for improvements in overall user experience.

**Two-sentence takeaway:** Ericson shows that for a conversational search agent, the two highest-leverage design decisions are a hierarchical intent classifier (GBDT + entity refinement lifted News-IR F1 from 0.812 to 0.963) and engagement/proactivity tuning, both validated against thousands of live Alexa Prize conversations. The honest caveat for the team: real-world deployment makes isolating component effects hard, so report component-level ablations with care and expect attribution noise.

---

## Paper 2: arXiv:2406.12276 (v1) — CodeNav: Beyond tool-use to using real-world codebases with LLM agents

**Authors:** Tanmay Gupta, Luca Weihs, Aniruddha Kembhavi

**Subject category (from abs page):** Artificial Intelligence (cs.AI); Computation and Language (cs.CL); Software Engineering (cs.SE) — primary: cs.AI
**Submission date (from abs page):** Tue, 18 Jun 2024 05:10:38 UTC (v1)

**Abstract summary:** CodeNav is a single-agent, multi-environment interaction framework in which an LLM agent ("Nav") indexes and searches over code blocks in a target codebase, finds relevant snippets, imports them, and iteratively generates a solution with execution feedback — instead of requiring manual "registration" of tool names and descriptions in the LLM context. On three tool-use benchmarks (m&m's, M3 ToolEval, API-Bank) adapted for code-use, CodeNav is competitive with tool-use despite not having privileged access to hand-crafted tool descriptions (tool-use is the upper bound). The paper quantifies the gap, studies how library/description richness and LLM choice affect performance, and shows code blocks as retrieval results beat prototypes/docstrings alone. Code is open source under a permissive license.

**Extracted findings:**
- **Finding 1 (Section 5.3 / Table 3 — "Search response formatting ablation on m&m's"):** Returning actual code for the top-3 matches yields higher tool-f1 (81.2 ± 2.3) than showing function prototypes only (80.0 ± 5.2), and reduces the number of interaction steps the agent needs to solve a query. The authors state "returning the actual implementation details in the source code (which might also include docstrings) alleviates the need to manually register tools and provides strictly more information than just the function signatures or prototypes," and that real code blocks "span 100s of lines which quickly increases the context length" — CodeNav mitigates this by generating GPT-4 docstrings for top-3 retrievals and showing whichever is shorter.
- **Finding 2 (Section 5.4 / Table 4 — "What makes a good library description?"):** On M3 ToolEval (a 5-file codebase), a richer library description "file path + file desc" (641 tokens, one line per file summarizing contents) achieve accuracy 81.7 ± 4.9 and 7.8 ± 0.4 steps vs 76.8 ± 6.5 / 8.3 ± 0.3 for "file path" only. The paper gives three concrete recommendations: (i) provide context for the target domain so the LLM can generate useful search keywords; (ii) describe library structure (directory structure or key assumptions); (iii) give a brief natural-language description of available functionality. The authors also note tool-use forms an upper bound for code-use since the tool-use agent has comprehensive hand-crafted information.
- **Finding 3 (Section 5.5 / Table 5 — "Impact of LLM choice on performance"):** Performance is highly LLM-dependent: GPT-4-1106-preview reaches tool-f1 80.6 ± 5.1 in 7.2 ± 0.2 steps, while GPT-3.5-turbo-0125 collapses to 22.96 ± 0.8 f1 / 9.08 steps, and open-source Mixtral-8x22B-Instruct and Qwen1.5-110B-Chat get 67.91 and 48.84 f1 respectively (lagging mainly in recall, i.e., search failures). The authors attribute GPT-3.5's poor performance to the agent "failing to appropriately summarize its actions at the end of the episode," which "may be ameliorated by additional prompt tuning." They also note LLMs with context sizes below 16k tokens regularly fail by exceeding their context limit.
- **Finding 4 (Section 6 "Discussion"):** The core design position is "move from tool-use to code-use" — from meticulously registered tool sets to letting the agent directly read, import, and use source code of any repository, enabled by the observation that "well-designed libraries are written by humans for humans." CodeNav uses boolean Elasticsearch queries (e.g., `(type: CLASS) AND (text: ObjectDetection)`) to search the codebase, iterates between search and solution generation, inspects execution results, fixes errors, and resolves ambiguities iteratively.

**Two-sentence takeaway:** CodeNav's headline result is that agentic search over real codebases can match registered tool-use benchmarks without any tool registration, with source code as retrieval results beating prototypes/docstrings (81.2 vs 80.0 tool-f1 on m&m's) and a图书馆 description that gives domain context and file-level summaries adding ~5 points of accuracy. For the team: prioritize (a)surfer-friendly repo structure +signals like READMEs as "tool registries," (b) stronger LLMs or better prompt summarization for agent termination, and (c) context budget planning since long code blocks inflate context.

---

## Paper 3: arXiv:2502.13321 (v2) — Adjust for Trust: Mitigating Trust-Induced Inappropriate Reliance on AI Assistance

**Authors:** Tejas Srinivasan, Jesse Thomason

**Subject category (from abs page):** Human-Computer Interaction (cs.HC); Artificial Intelligence (cs.AI); Computation and Language (cs.CL) — primary: cs.HC
**Submission date (from abs page):** v1 Tue, 18 Feb 2025 22:42:39 UTC; current version v2 Mon, 26 Jan 2026 16:35:39 UTC

**Abstract summary:** This HCI study shows that user trust biases reliance on AI assistance, with low and high trust producing under- and over-reliance respectively. The authors propose that AI assistants adapt behavior via trust-adaptive interventions: supporting explanations when trust is low, counter-explanations when trust is high, and forced pauses to slow interactions. In two decision-making scenarios (laypeople answering science questions and medical doctors making diagnoses), supporting and counter-explanations at trust moments yield up to a 38% reduction in inappropriate reliance and a 20% improvement in decision accuracy, and adaptively inserted pauses reduce over-reliance; they also show purely interaction-level trust features are insufficient to predict trust moments, and trust can be elicited with an 11-point post-interaction scale.

**Extracted findings:**
- **Finding 1 (Section 7.2 "Results" / §7.1 experiment conditions):** Trust-adaptive explanations reduce inappropriate reliance: providing explanations when user trust is low yields 13–31% reduction in under-reliance, 9–38% reduction in total inappropriate reliance, and 10–19% improvement in final decision accuracy, with most Trust-Adaptive improvements over No Intervention statistically significant under clustered bootstrap tests. Critically, the "Supporting Explanation Always" condition does not help and can hurt — it worsens diagnosis accuracy and increases under-reliance in ArcO and DiagC, because always showing explanations exposes users to misleading explanations and erodes trust; the largest gains occur when the AI is over-confident (ArcO task), indicating explanations are most effective for mitigating miscalibrated AI disuse.
- **Finding 2 (Section 12 "Analysis: Can We Model User Trust?" / §12.1 "Trust Heuristics and Models" / Table 2):** The paper assumes the assistant can observe trust after each interaction, but in the real world it must predict trust from interaction history. Simple heuristics (AIAcc5 based on last-5 AI accuracies, CapabilityDiff, SmoothOutcomes, SmoothConfs) and a trained linear TrustModel achieve only moderate correlation with user-reported trust (test correlation ~0.43 for SmoothConfs/TrustEffectModel) and very low F1 (≤ 0.5) at detecting low/high trust moments (e.g., AIAcc5: test corr 0.345, high-trust F1 0.345, low-trust F1 0.458). The authors conclude this points to the challenging nature of modeling user trust and the inadequacy of surface-level interaction features.
- **Finding 3 (Section 14 "Limitations" / Ethical Considerations):** The majority of user studies used a simulated AI assistant rather than a real LLM (for controllability of accuracy and confidence calibration), but a simulated AI may be unrealistic since it can give contradicting predictions for near-identical problems (correctness is decided per problem independently). Findings would be more reliable with more participants, tasks, and interventions (limited by cost and a dearth of professional doctors on Prolific); the study only covers UK and US English-speaking users. Ethically, the authors stress trust-adaptive interventions must not manipulate users, and recommend disclosing to users that the AI assistant is modeling their trust, with compliance with local ethical standards.

**Two-sentence takeaway:** Trust-adaptive design is a real lever: surfacing explanations when users trust you too little (13–31% less under-reliance) and counter-explanations/pauses when trust is too high (up to 38% less inappropriate reliance, 20% better accuracy) materially improves decision quality — but "always on" explanations backfire, and you cannot reliably infer trust from interaction signals alone (F1 ≤ 0.5), so measure trust explicitly (e.g., an 11-point post-interaction scale) and disclose that the assistant is modeling trust. Pair this with RAVine's attribution warning: calibrated, honest communication about confidence is a trust/safety prerequisite, not a UX add-on.

---

## Paper 4 (retained): arXiv:2506.21506 (v2) — Mind2Web 2: Evaluating Agentic Search with Agent-as-a-Judge

**Authors:** Boyu Gou, Zanming Huang, Yuting Ning, Yu Gu, Michael Lin, Weijian Qi, Andrei Kopanev, Botao Yu, Bernal Jiménez Gutiérrez, Yiheng Shu, Chan Hee Song, Jiaman Wu, Shijie Chen, Hanane Nour Moussa, Tianshu Zhang, Jian Xie, Yifei Li, Tianci Xue, Zeyi Liao, Kai Zhang, Boyuan Zheng, Zhaowei Cai, Viktor Rozgic, Morteza Ziyadi, Huan Sun, Yu Su

**Subject category (from abs page):** Artificial Intelligence (cs.AI); Computation and Language (cs.CL) — primary: cs.AI
**Submission date (from abs page):** v1 Thu, 26 Jun 2025 17:32:50 UTC; current version v2 Thu, 3 Jul 2025

**Abstract summary:** Mind2Web 2 is a benchmark of 130 realistic, high-quality, long-horizon tasks requiring real-time web browsing and extensive information synthesis, built with over 1,000 hours of human labor. It proposes an Agent-as-a-Judge framework with tree-structured rubrics (average 50 nodes, max 603) to evaluate time-varying, citation-backed answers, and finds OpenAI Deep Research already reaches 50–70% of human performance while spending less than half the time, with the best systems beating search-augmented LLMs and web agents like Operator thanks to real-time browsing and test-time scaling.

**Extracted findings (retained from prior curation):**
- **Finding 1 (§10.2 "Main Results", Table 3):** OpenAI Deep Research reaches partial-completion 0.54 / full-task success 0.28 / Pass@3 0.40 vs. 0.79 / 0.54 / 0.83 for humans; ChatGPT Search and Perplexity Pro Search are the weakest (success ≤ 0.11) due to short search horizons and shallow synthesis; 57 explicitly time-varying tasks exist where most evaluated systems perform worse than on static tasks, supporting the need for real-time browsing; answer length does not correlate with task completion.
- **Finding 2 (§10.4 "Human Evaluation of Judge Agents"):** Judge agents were validated against a human evaluator on 15 sampled tasks (720 leaf-node verifications): only 7/720 nodes reflected actual verifier errors, a 99.03% correctness rate — notably above prior automated evaluation approaches (typically below 90%).

**Two-sentence takeaway:** Agentic search is real and measurable now — Deep Research-style systems already hit 50–70% of human performance at half the cost, and an Agent-as-a-Judge with tree rubrics is a repeatable, 99%-reliable way to evaluate long-horizon, citation-backed answers. Two design lessons: (a) budget for real-time web browsing capability or you'll lose on time-varying tasks, and (b) answer length is not quality — concise, well-attributed answers beat verbose ones.

---

## Paper 5 (retained): arXiv:2507.16725 (v2) — RAVine: Reality-Aligned Evaluation for Agentic Search

**Authors:** Yilong Xu, Xiang Long, Zhi Zheng, Jinhua Gao

**Subject category (from abs page):** Computation and Language (cs.CL); Artificial Intelligence (cs.AI); Information Retrieval (cs.IR) — primary: cs.CL
**Submission date (from abs page):** v1 Tue, 22 Jul 2025 16:08:12 UTC; current version v2 Thu, 31 Jul 2025

**Abstract summary:** RAVine is a Reality-Aligned eValuation framework for agentic LLMs with search targeting multi-point queries and long-form answers reflecting realistic user intent, with attributable nugget-centered ground-truth construction plus process-oriented metrics for tool use and efficiency. Built on MS MARCO V2.1 (10,960,555 documents) with search and fetch tools, it benchmarks models and derives three insights: current models are limited in task completeness, faithfulness, and search performance; strong intermediate process performance does not guarantee high-quality final answers; and models rely on internal knowledge to generate final reports in ways that are unattributable and undesirable.

**Extracted findings (retained from prior curation):**
- **Finding 1 (§6.3 "Results"):** Qwen3-8B without thinking recalls only 22.3% of nuggets on average, while Qwen3-32B achieves maximum citation recall of just 13.2% and precision of 11.9%; "thinking" mode generally improves task completion and completeness scores at added latency/cost, and non-thinking modes often make aggressive, inefficient tool calls.
- **Finding 2 (§6.3 — "Attribution is impacted by reliance on internal knowledge"):** A new metric Comp_in measures the share of task-completeness scores traceable to internal knowledge; a significant portion of task performance across models is traceable to internal knowledge — for some models more than half of nuggets are hits from internal knowledge — which "cannot be attributable through citations" and impairs faithfulness and verifiability.
- **Finding 3 (§6.3 — "The illusion of progress"):** There is no significant positive correlation between final task-completeness scores and search-precision scores; linear regression shows runs relying on retrieved knowledge exhibit a more pronounced positive correlation between completeness and search precision, meaning excessive internal-knowledge reliance is one factor in the gap between intermediate and final performance.

**Two-sentence takeaway:** Evaluation of agentic search needs three axes, not just answer quality: (1) claim-level nugget attribution so unattributable internal-knowledge generation is penalized (a real trust/safety gap), (2) process metrics on tool use and efficiency since agentic LLMs are process-oriented and runtime efficiency matters, and (3) the "illusion of progress" warning — strong search-tool performance does not guarantee a good final report, so grade the output, not just the trajectory. Budget for multi-turn context management from day one.

---

## Cross-cutting takeaways for the team sync

1. **Three pillars of agentic search quality:** (a) Agentic search evaluation is now measurable (Mind2Web 2, §10.2: Deep Research hits 50–70% of human performance at half the time with a 99%-reliable Agent-as-a-Judge); (b) evaluation must be claim-level and process-aware (RAVine §6.3: internal-knowledge generation is unattributable and undermines faithfulness); (c) the human side is the hardest problem (Adjust for Trust, §7.2/§12: trust-adaptive explanations move the needle up to 38%, but trust is hard to model, F1 ≤ 0.5).
2. **Design checklist we should adopt:** budget for real-time browsing (57 time-varying tasks hurt non-browsing agents, Mind2Web 2 §10.2); grade outputs, not just trajectories, and penalize unattributable internal-knowledge generation (RAVine §6.3); build codebase-aware retrieval + library-description quality (CodeNav §5.4 recommendations); use trust-adaptive explanations rather than always-on ones (Adjust for Trust §7.2); use elicited trust via an 11-point scale and disclose trust modeling to users (§12, §14); keep hierarchical intent classification strong and log conversation data for component attribution (Ericson §6.1.1, §7); and budget for long-horizon eval cost and multi-turn context management (Mind2Web 2 App. A; RAVine §5.1–5.2).
3. **Known limitations to carry into our design:** judge-agent validation needs human eyeballs (§10.4: 99.03% but 4 cases of collapsed-content attribution loss); process-oriented metrics and token/latency budgets are now table-stakes (§5.1–5.2); simulated-AI study results may not transfer to real LLMs (§14); and real-world deployment makes isolating component effects hard (Ericson §7).

---

## Sources

- arXiv abs pages (primary category + submission history, fetched directly at arxiv.org/abs/{ID}; all three loaded successfully): arxiv.org/abs/2304.02233, arxiv.org/abs/2406.12276, arxiv.org/abs/2502.13321 (retained: 2506.21506, 2507.16725)
- Full texts read via arXiv HTML endpoints: Ericson (2304.02233 v1), CodeNav (2406.12276 v1), Adjust for Trust (2502.13321 v2)
- Sections cited: Ericson §3.3 "Hierarchical Intent Classifier", §4 System Description, §6 "Results and Discussion" incl. §6.1.1 "Hierarchical Intent Classification", §6.1.2 "Engagement", §6.1.3 "Proactivity", §7 "Limitations"; CodeNav §2 "Related Work", §3 "CodeNav", §5.3–5.5 (Tables 3–5), §6 "Discussion"; Adjust for Trust §1 "Introduction", §7.2 "Results", §12 "Analysis: Can We Model User Trust?", §14 "Limitations"; Mind2Web 2 §4, §10.2, §10.4, Appendix A; RAVine §3, §5.1, §5.2, §6.3.

*File committed on branch `add-reading-notes` (off `main`, the default branch) of repo `mcpmark-eval-liuhezi/arxiv-agentic-search-notes`, via pull request "Add reading notes: agentic search and tool use" (PR #1: https://github.com/mcpmark-eval-liuhezi/arxiv-agentic-search-notes/pull/1). Team members: please comment on the PR or the file to add discussion points before Thursday's sync.*
