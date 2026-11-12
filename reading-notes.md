# Reading Notes: Agentic Search and Tool Use

**Purpose:** Literature brief for Thursday's team sync on agentic search — assistants where an LLM-driven agent searches, calls tools, and hands answers to users. This file records what the team covered, with arXiv IDs, author lists, subject categories, submission dates, abstract summaries, and extracted findings (each tagged with its source section) drawn from the full texts and public arXiv abstract pages.

**Papers covered (3):**

1. **arXiv:2611.12125 — CARE-PO: Controllable Tool-Call Efficiency for LLM Agents** (tool use in LLM agents)
2. **arXiv:2405.02637 — TREC iKAT 2023: A Test Collection for Evaluating Conversational and Interactive Knowledge Assistants** (conversational search agents)
3. **arXiv:2606.13854 — SpheriCity: Designing Trustworthy Conversational AI for Sustainability Decision Support** (user trust in AI assistants)

Each entry below includes: arXiv ID, title, full author list, subject category (verified on the public arXiv abs page), submission date, a 2–3 sentence abstract summary, and at least two concrete findings from the full text tagged with their source sections.

---

## Paper 1: arXiv:2611.12125 — CARE-PO: Controllable Tool-Call Efficiency for LLM Agents

**Authors:** Xiaoyang Ye, Xiao Yao, Hairui Zhu, Yiying Yang, Bei Gan, Jinliang Lu, Tengjin Weng, Yuan Yao, Wenhao Jiang

**Subject category (from abs page):** Machine Learning (cs.LG); Artificial Intelligence (cs.AI) — primary category cs.LG. Submitted: Mon, 9 Nov 2026 (v1). Comments: Under review at ICLR 2027.

**Abstract summary:** External tool calls are a major running cost of an LLM agent, but training objectives align almost exclusively with task completion. The paper presents CARE-PO (Controllable Anchored Relative Efficiency Policy Optimization), which prices calls against a reference budget anchored to each task's own difficulty and fixed before training, injects a successful source trajectory into each GRPO rollout group to avoid call-count collapse, and uses an effort tag in the prompt to yield both an efficiency and a capability operating point from a single set of weights. On GAIA, five of six operating points across three model sizes improve on the base model in success rate and call count at once; on held-out multi-hop benchmarks the trade-off is size-dependent (held or raised at 27B and 4B, while at 9B both points trade success rate for fewer calls).

### Extracted findings (from full text)

- **Finding 1 (evaluation result) — Section 4.1 "Diagnosis of Redundant Calls":** Across 221 GAIA trajectories collected from Qwen3.5-9B and 27B, 82.8% (183 trajectories) contained a call deletable outright or mergeable into its neighbour (85.7% at 9B, 80.2% at 27B); those trajectories averaged 5.7 removable calls, and 68.0% of all calls were removable without breaking the answer's evidence chain. The authors note these rates measure redundancy, not cost, and group behavioural patterns into three forms: calls with no increment, repeated calls (tail repetition loops that persist to the call cap), and evidence gaps — with forms (ii) and (iii) most associated with wrong answers even though call count misses their harm (Appendix A).

- **Finding 2 (design decision / limitation) — Section 7.3 "Call-Count Collapse in Training":** Unlike CARE-PO, the OTC-PO efficiency-reward baseline at 27B collapses toward zero tool calls: after step 300, OTC-PO's call count falls to 0.01 per rollout — the degenerate solution of issuing no call at all. CARE-PO remains well above zero at all three model sizes (9B, 27B, and 4B) with the same learning rate and number of training steps, which is presented as evidence that injecting the source trajectory as a group floor eases call-count collapse. The paper also acknowledges that at 9B both operating points trade success rate for fewer calls, so the efficiency–capability trade-off is not free.

- **Takeaway for the team:** Tool-call efficiency is a first-class training objective, but anchoring the efficiency reward to a pre-computed per-task budget (rather than the policy's own rollouts) and providing a source-trajectory floor both prevent the policy from collapsing to never calling tools.

---

## Paper 2: arXiv:2405.02637 — TREC iKAT 2023: A Test Collection for Evaluating Conversational and Interactive Knowledge Assistants

**Authors:** Mohammad Aliannejadi, Zahra Abbasiantaeb, Shubham Chatterjee, Jeffery Dalton, Leif Azzopardi

**Subject category (from abs page):** Information Retrieval (cs.IR); Artificial Intelligence (cs.AI); Computation and Language (cs.CL) — primary category cs.IR. Submitted: Sat, 4 May 2024 (v1). Comments: To appear in SIGIR 2024.

**Abstract summary:** Conversational information seeking has evolved rapidly with LLMs, providing the basis for naturalistic interpretation and response to user requests. The extended TREC Interactive Knowledge Assistance Track (iKAT) collection enables researchers to test and evaluate Conversational Search Agents (CSAs) using 36 personalized dialogues over 20 topics, each coupled with a Personal Text Knowledge Base (PTKB) defining bespoke user personas. The collection provides 344 turns with approximately 26,000 passages for relevance assessments, plus response evaluations over four dimensions: relevance, completeness, groundedness, and naturalness, challenging CSAs to navigate personal contexts, elicit persona information, and employ context in conversations.

### Extracted findings (from full text)

- **Finding 1 (evaluation result) — Section 5.2 "PTKB Provenance":** In the PTKB statement ranking task, G→R→G models were not the top runs despite their success in passage ranking, suggesting LLMs can leverage PTKB statements effectively in response generation but are less effective at ranking relevant PTKB statements in the G→R→G pipeline; LLaMA in the zero-shot setting achieved the best result in the PTKB statement ranking task on both NIST and organizer assessments. Using the organizers' assessments, topic 20-2 was the most challenging topic for this task while 16-1 was the easiest, and the authors did not notice a clear correlation between PTKB statement ranking and passage retrieval performance.

- **Finding 2 (design decision / limitation) — Section 5.3 "Response Evaluation" and Section 6 "Conclusion":** Response evaluations by human assessors and GPT-4 as an external evaluator showed run-4-GPT-4 leading on relevance (2.92), completeness (2.85), and naturalness (4.00), with groundedness at 0.89 (65/73 supported), while earlier systems like uot-yahoo_run_llmnoptkb scored much lower (relevance 0.35, completeness 0.11). The paper's design constraint is that responses must cite at least one "provenance" passage from the collection, and considers responses that amalgamate or summarize passages acceptable, highlighting that grounding-to-provenance-citation is now a standard requirement for CSA evaluation, and the track explicitly evolved from TREC iKAT 2023 into the CAsT track to support multi-path, multi-turn, persona-driven conversations.

- **Takeaway for the team:** When evaluating any conversational search agent, plan for persona-aware dialogues, mandatory provenance citation, and multi-dimensional response evaluation (relevance, completeness, groundedness, naturalness) rather than passage ranking alone.

---

## Paper 3: arXiv:2606.13854 — SpheriCity: Designing Trustworthy Conversational AI for Sustainability Decision Support

**Authors:** Ahmed Qayyum, Madison Werner, Kathryn Youngblood, Jenna R. Jambeck, Tahiya Chowdhury

**Subject category (from abs page):** Human-Computer Interaction (cs.HC); Artificial Intelligence (cs.AI) — primary category cs.HC. Submitted: Thu, 11 Jun 2026 (v1). Comments: Accepted to ACM SIGCAS/SIGCHI Conference on Computing and Sustainable Societies (COMPASS '26).

**Abstract summary:** SpheriCity is an expert-grounded conversational prototype designed to support trustworthy knowledge sensemaking from sustainability reports, using a provenance-first design that foregrounds evidence traceability through page-level source citations, structured response synthesis, and visible interaction cues for exploratory querying and cross-document synthesis. The authors conducted a formative expert review with six sustainability experts using 13 representative queries spanning cross-city comparison, policy summarization, and recommendation-oriented tasks, evaluating responses across relevance, contextual fit, accuracy, neutrality, and informational depth. Results revealed that transparent sourcing, contextual explanation, interpretability, and alignment with expert workflow strongly shape expert trust and judgments of system usefulness.

### Extracted findings (from full text)

- **Finding 1 (evaluation result / design insight) — Section 5.2 "Can Generic Chatbot Evaluation Metrics Work in Sustainability Contexts?":** Experts argued that generic chatbot evaluation dimensions (fluency, grammatical correctness, relevance, informativeness) are insufficient and sometimes misleading in sustainability contexts, identifying four value tensions: completeness vs. hallucination (more comprehensive answers were often perceived as less trustworthy if they introduced unsupported details), depth vs. speed (overly summarized responses risk flattening nuances and caveats), generalization vs. contextualization (responses too generalized across cities or policy contexts were judged misleading), and neutrality vs. persuasiveness (experts were wary of answers that appeared to advocate for particular policies without acknowledging tradeoffs). This is why the authors designed evaluation dimensions in collaboration with sustainability experts aligned to their workflow.

- **Finding 2 (limitation acknowledged) — Section 8.5 "Limitations and Future Work":** The study has several limitations: the expert sample was relatively small and domain-specific (appropriate for an exploratory design study but needing larger, more diverse populations); the study evaluated responses in a static review setting rather than through fully interactive multi-turn conversations, so trust and interpretability evolution during longer interactions remains untested; the corpus and queries were restricted to circularity assessment reports and may not generalize to climate adaptation, biodiversity conservation, or environmental justice domains; and the study did not observe experts using the system during real decision-making episodes. The authors also note they did not systematically measure hallucination rates, including potentially fabricated page numbers or misattributed report content, and call for systematic hallucination evaluation protocols for domain-specific corpora.

- **Takeaway for the team:** In high-stakes domains, generic chatbot metrics are insufficient — co-design evaluation dimensions with domain experts, and plan for static review protocols plus explicit hallucination auditing in the evaluation plan.

---

## Cross-Paper Takeaways for Thursday's Sync

1. **Evaluation matters as much as model capability.** iKAT (cs.IR) shows provenance-citation requirements and multi-dimensional response evaluation are now table stakes for conversational search agents; SpheriCity (cs.HC) shows generic metrics are insufficient in high-stakes domains and need co-design with experts.

2. **Agentic efficiency is a separate optimization problem.** CARE-PO (cs.LG) demonstrates that tool-call efficiency needs its own reward design (fixed reference budget, source-trajectory floor, effort tags) to avoid both over-calling and call-count collapse, with results verified on GAIA and held-out multi-hop benchmarks.

3. **Trust and grounding are design decisions, not aftermaths.** SpheriCity's provenance-first architecture and iKAT's mandatory citation requirement both point to evidence traceability (page-level citations, provenance passages) as a core design element of agentic search systems aimed at users who need to verify answers.

---

*Notes compiled from full-text reads of arXiv:2611.12125, arXiv:2405.02637, and arXiv:2606.13854, with subject category and submission date recorded from each paper's public arXiv abstract page (https://arxiv.org/abs/<ID>).*
