# Reading Notes: Agentic Search — Thursday Team Sync

**Prepared for:** Thursday's team sync on **agentic search** (assistants where an LLM-driven agent searches, calls tools, and hands answers to users).
**Status:** OPEN for team review — please leave comments directly on this file (or open an issue) before Thursday.

This brief was assembled end to end. arXiv searches were run on the three requested phrasings — "conversational search agents", "tool use in LLM agents", and "user trust in AI assistants" — and the three most relevant hits that surfaced are covered below. For each paper we read the full text (not just the abstract) and extracted at least two concrete findings (an evaluation result, a design decision, or an acknowledged limitation), each tagged with the section it came from. The primary subject category and submission date for each paper are taken from its public arXiv abstract page (https://arxiv.org/abs/<ID>).

---

## Paper 1 — Agentic search lifecycle across conversational platforms

- **arXiv ID:** 2609.19244 (v1) — https://arxiv.org/abs/2609.19244
- **Title:** Characterizing Web Search by Conversational LLM Agents: From Search Decisions and Strategies to Results and Responses
- **Authors:** Mahsa Amani, Seungeon Lee, Abhisek Dash, Asmaa El Fraihi, Yunah Jang, Elisabeth Kirsten, Qinyuan Wu, Krishna P. Gummadi, Manish Gupta, Abhilasha Ravichander, Muhammad Bilal Zafar, Soumi Das
- **Primary subject category (arXiv abs page):** cs.AI (Artificial Intelligence); co-listed: cs.IR (Information Retrieval)
- **Submission date (arXiv abs page):** v1 submitted Wed, 16 Sep 2026 17:56:32 UTC
- **Abstract summary:** The first study of Web search across four major conversational platforms (ChatGPT, Claude, Grok, DeepSeek), combining real-world user interactions ("invivo") with controlled API-based experiments ("invitro") using the same models. The authors analyze when a model decides to call Web search, how it formulates queries, the domain preferences of search engines it queries, and how it grounds responses in search results. Headline results: search-call decisions vary substantially across platforms and models, more frequent Web-search invocation does not necessarily yield better response quality, agents use complex iterative querying strategies, and while responses are largely grounded in search results, a non-trivial share relies on uncited search results.

**Extracted findings:**

1. **Agents are good at knowing when *not* to search, but invParadise triggering Web search does not improve responses.** LLM agents differ significantly in when they choose to call Web search; while they are good at deciding when they do *not* need Web search, their search-call decisions do not always improve responses. Controlled via API with 1,000 identical user prompts: Web search was invoked for only 14% of prompts by GPT-5.3-chat (OpenAI), 83% by Claude Sonnet 4.6 (Anthropic), 45% by Grok-4.3 (xAI), and 75% by DeepSeek-v4-flash — and critically, prompts that did *not* trigger search scored higher on all response-quality measures (factuality, completeness, relevance). **[Source: Section 3 "Analyzing Search Calling Decisions", esp. §3.1 and §3.3]**
2. **Retrieved results exhibit strong platform-specific domain preferences, and retrieved claims are often uncited.** Top-10 domains account for 21.3% (invivo) / 47.8% (invitro) of all returned search results for ChatGPT, and 32.3% (29%) for Grok; different engines return results heavily skewed to their preferred domains (e.g., reddit.com and youtube.com dominate on some). Uncited-yet-searched URLs account for 14% to 53% of all claims, and 15%–20% of all claims remain ungrounded (potentially parametric-knowledge-derived), with lowest factuality scores (2.84) vs. claims supported by citations (3.47). **[Source: Section 12.3 "Domain Preferences in Search Results"; Section 13 "Analyzing Search Responses" / §13.2 "Grounding of Claims"]**
3. **Acknowledged limitation: observational traces omit internal reasoning and rely on automated judges.** The study relies on observational conversational traces from four platforms (171,264 conversations from 613 users), which do not expose full internal reasoning/chain-of-thought or platform ranking algorithms; analysis depends on automated judge models whose evaluation "remains imperfect and may inherit biases," and the work focuses on English interactions without multimodal/non-English retrieval. **[Source: Section 15 "Limitations"; Section 2 "Datasets and Experimental Setups"; Section 14 "Conclusion"]**

---

## Paper 2 — Reliable tool descriptions for LLM agents at scale

- **arXiv ID:** 2602.20426 (v2) — https://arxiv.org/abs/2602.20426
- **Title:** Learning to Rewrite Tool Descriptions for Reliable LLM-Agent Tool Use
- **Authors:** Ruocheng Guo, Kaiwen Dong, Xiang Gao, Kamalika Das
- **Primary subject category (arXiv abs page):** cs.AI (Artificial Intelligence)
- **Submission date (arXiv abs page):** v1 Mon, 23 Feb 2026 23:50:24 UTC; current version v2 Wed, 29 Apr 2026 00:08:36 UTC
- **Abstract summary:** While most effort to improve LLM-based tool-using agents focuses on the agent itself (larger models, better prompting, fine-tuning), performance increasingly plateaus because of the quality of tool interfaces agents consume. Tool descriptions are written for human developers, tolerating ambiguity agents cannot resolve as tool catalogs grow into the hundreds. The authors propose **Trace-Free+**, a curriculum-learning framework that transfers supervision from trace-rich settings to trace-free deployment so a description generator internalizes reusable interface patterns and can onboard new tools from schema alone. On StableToolBench and RestBench it reduces accuracy degradation by 29.23% and improves average query-level success by 60.89% at 150+ candidate tools, generalizes across domains without retraining, and lifts the state-of-the-art Gemini-3-pro-preview on BFCLv2 by up to 1.4 points purely through better tool interfaces.

**Extracted findings:**

1. **Design decision — curriculum learning from trace-rich to trace-free deployment (§5.1 "Trace-Free+: Curriculum Learning for Transferable Interface Optimization").** Execution traces are the richest supervision signal but are unavailable for new tools at deployment time due to cold-start, privacy, or safety constraints. Trace-Free+ fine-tunes an open-weight LLM as a description generator on a curriculum: training starts with trace-based examples (trace summaries) and progressively transitions to trace-free examples where descriptions must be generated from the tool schema alone, so the model conditions on information absent at deployment time.
2. **Evaluation result — up to 97.2% pattern coverage on 4,585 unseen tools vs. <12% for original descriptions (§2 "Introduction"; validated in §4.2, Table 8).** This supports the authors' bounded-pattern hypothesis: effective tool descriptions follow a reusable set of interface patterns (scope definition, parameter constraints, output formats, dependency structure). Trace-Free+ onboarding a new tool requires only its schema as input — no query synthesis, trace collection, annotation, or rule extraction — matching trained description quality (~2.93 vs. 2.06) at inference time without per-tool cost.
3. **Evaluation result — at 150+ candidate tools, Trace-Free+ cuts accuracy degradation by 29.23% and improves average query-level success by 60.89% on StableToolBench (§Abstract; §5 "Experiments"), with complementary gains on top of agent fine-tuning and boosts on RestBench and BFCLv2.** In scaling experiments with up to 150+ candidates, it reduces accuracy degradation by 29.23% and improves query-level success by 60.89% on average, transfers to the Berkeley Function Calling Leaderboard (BFCLv2) purely through better tool interfaces, lifting the state-of-the-art Gemini-3-pro-preview by up to 1.4 points.
4. **Design decision — schema-only onboarding at deployment (§5.2 "Data Synthesis for Tool Interface Improvement").** Settings D1/D2 are per-tool supervision targets (D2 adds RIMRULE-extracted rules: acceptable value formats, undocumented preconditions), but Trace-Free+ trained across hundreds of tools abstracts cross-tool patterns and applies them zero-shot to unseen tools; the authors deliberately do *not* aim to outperform D2 but to match its quality without the per-tool cost.

---

## Paper 3 — User trust in AI assistants and trust-adaptive interventions

- **arXiv ID:** 2502.13321 (v2) — https://arxiv.org/abs/2502.13321
- **Title:** Adjust for Trust: Mitigating Trust-Induced Inappropriate Reliance on AI Assistance
- **Authors:** Tejas Srinivasan, Jesse Thomason
- **Primary subject category (arXiv abs page):** cs.HC (Human-Computer Interaction); co-listed: cs.AI (Artificial Intelligence), cs.CL (Computation and Language)
- **Submission date (arXiv abs page):** v1 Tue, 18 Feb 2025 22:42:39 UTC; current version v2 Mon, 26 Jan 2026 16:35:39 UTC
- **Abstract summary:** User trust biases reliance on AI recommendations in AI-assisted decision-making: low and high trust produce increased under- and over-reliance respectively. The authors propose that AI assistants should adapt behavior through trust-adaptive interventions — when trust is low, supporting explanations elicit more careful consideration; when trust is high, counter-explanations and/or adaptively inserted forced pauses reduce over-reliance. In two scenarios (laypeople answering science questions; medical doctors making diagnoses) supporting + counter-explanations at the right trust moments yield up to a 38% reduction in inappropriate reliance and 20% improvement in decision accuracy; forced pauses reduce over-reliance. The findings generalize to a real LLM assistant, and trust can largely not be inferred from interaction features alone.

**Extracted findings:**

1. **Evaluation result — up to 38% reduction in inappropriate reliance and 20% decision-accuracy gain (§7.2 "Results"; also §5 / abstract).** Providing explanations when trust is low yields 13–31% reduction in under-reliance, 9–38% reduction in total inappropriate reliance, and 10–19% improvement in final decision accuracy, with a clustered bootstrap test showing most improvements of the trust-adaptive intervention over no intervention are statistically significant. Quantified in the Introduction: doctors mistakenly accept 26% of AI misdiagnoses when trust is high vs. 8% when trust is lower (over-reliance); when trust is low, doctors reject correct AI diagnoses 68% of the time vs. 40% otherwise (under-reliance).
2. **Design decision — timing and type of intervention matter (matching explanations to trust level; §7.2 "Results").** The "Supporting Explanation Always" condition does not yield similar improvements and *worsens* decision accuracy in the diagnosis task — persistent explanations hurt and are not as useful as trust-adaptive explanations (hypothesis H5); forcing pauses helps reduce over-reliance but not under-reliance. The design choice: low trust → supporting explanations (reduce disuse risk); high trust → counter-explanations and/or forced pauses (promote deliberation).
3. **Acknowledged limitation — simulated AI rather than a real LLM, and trust heuristics are insufficient (§14 "Limitations").** The majority of user studies used a simulated AI assistant (for controllability of accuracy and confidence calibration); findings come from UK/US English-speaking Prolific participants, and using purely interaction-level features (heuristic or learning-based methods) is insufficient to accurately predict moments of low/high trust, highlighting the challenging nature of trust estimation and the need to consider other human factors.
4. **Design decision — operationalization: an 11-point global trust scale measured after each interaction (§2 "Related Work"; §11 "Results").** The authors elect global trust scores ("belief in the AI's helpfulness after each decision-making problem") over local interaction-specific trust, and validate that trust-adaptive design generalizes to a real LLM assistant (§10), making trust-adaptive behavior a promising avenue for human-LLM collaboration.

---

## Cross-paper takeaways for Thursday

1. **Search behavior is heterogeneous and platform-biased (2609.19244).** Agents rarely (and not always correctly) decide when to search, and their retrieved evidence comes with strong platform-specific domain preferences; the fix is to evaluate the whole search lifecycle — when agents search, how they formulate queries, and citation quality — not just retrieval accuracy.
2. **Interfaces and prompts matter as much as models (2602.20426).** Better tool descriptions (schema-only onboarding via curriculum learning) cut accuracy degradation by ~29% and add complementary gains on top of fine-tuning; regenerating per-tool interfaces as catalogs grow is a recurring operational cost.
3. **Trust is a first-class design dial for AI assistants (2502.13321).** Supporting explanations at low-trust moments and counter-explanations at high-trust moments reduced inappropriate reliance by up to 38% and improved accuracy by 20% — adaptivity beats constant transparency.
