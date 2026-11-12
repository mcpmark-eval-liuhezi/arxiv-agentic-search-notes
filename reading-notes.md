# Reading Notes: Agentic Search and Tool Use

## Overview

This brief summarizes recent arXiv papers relevant to **agentic search** — assistants where an LLM-driven agent searches, calls tools, and hands answers to users. It was prepared for Thursday's team sync. We covered three papers along these themes: (1) **Search When Needed, Proceed When Reliable** (arXiv:2610.41738), a metacognitive reliability-supervised method for efficient agentic search; (2) **TREC iKAT 2023: A Test Collection for Evaluating Conversational and Interactive Knowledge Assistants** (arXiv:2405.02637), a benchmark and evaluation resource for conversational search agents (CSAs) built on LLMs; and (3) **A Conceptual Framework for Conversational Search and Recommendation** (arXiv:2404.08630), which formalizes the actions and intents of agent-human interaction during conversational search. For each paper we record the arXiv ID, title, full author list, primary subject category (verified on the public arXiv abs page), submission date (as shown on the abs page), a 2–3 sentence abstract summary, and at least two concrete findings extracted from the full text with their source sections. The team can comment on this file via pull request review.

All abs pages were checked publicly (https://arxiv.org/abs/<ID>); primary category and submission date are recorded per the abs page for each paper.

---

## Paper 1: Search When Needed, Proceed When Reliable: Metacognitive Reliability Supervision for Efficient Agentic Search

- **arXiv ID:** 2610.41738 (v1)
- **Title:** Search When Needed, Proceed When Reliable: Metacognitive Reliability Supervision for Efficient Agentic Search
- **Authors:** Minhao Sun, Zhijing Wu, Xian-Ling Mao
- **Primary subject category (from abs page):** Information Retrieval (cs.IR)
- **Submission date (from abs page):** [v1] Fri, 30 Oct 2026 13:44:30 UTC (1,464 KB). Abs page: https://arxiv.org/abs/2610.41738
- **Abs page category line:** Subjects: Information Retrieval (cs.IR). Cite as: arXiv:2610.41738 [cs.IR]

### Abstract summary

Efficient agentic search requires LLMs to decide at each intermediate state whether to search for external evidence or proceed with parametric knowledge. Supervising this choice with final-answer correctness or trajectory-level feedback is indirect, since realized performance is jointly shaped by stochastic generation, retrieval quality, and downstream reasoning. The paper proposes a metacognitive framework that scores the reliability of local conclusions: a Current-State Reliability Score whether the model can proceed without search, and a Search-Action Reliability Score measuring how much retrieved evidence improves estimated reliability. Together these define a reliability-aware process reward for GRPO, and experiments across seven QA benchmarks show a better accuracy–efficiency trade-off than strong agentic-search baselines, with code and checkpoints open-sourced.

### Extracted findings (with source sections)

1. **Evaluation result (Section 7.1 "Main Results"):** The 7B model achieves a 69.3 average CEM across seven QA benchmarks, exceeding HiPRAG (the strongest baseline) by 2.1 points, with improvements covering all seven datasets (multi-hop gains from 0.1 point on 2Wiki to 5.6 points on Bamboogle). In the controlled 7B comparison, the policy uses ~1.64 search calls per question versus 1.76 for the HiPRAG reimplementation and 1.83 for Search Wisely (a 10.4% reduction vs. Search Wisely), with ~20.9% fewer searches on single-hop QA and 6.6% more on multi-hop QA than HiPRAG — a task-dependent allocation of retrieval toward multi-step reasoning. Gains also transfer across model scales: +1.5 CEM on Qwen2.5-3B, +2.1 on Qwen2.5-7B, and +1.3 on Llama-3.2-3B over HiPRAG.

2. **Design decision (Sections 4–5, "Methodology" and "Estimating Local Reliability" / "Scoring Search and No-Search Decisions"):** The framework uses sequence-level uncertainty (mean negative log-likelihood of conclusion tokens) as a proxy for local-conclusion reliability, fit empirically per backbone (e.g., Qwen2.5-7B, 50,000 states, 32 conclusions per state). Search decisions are scored by the reliability change between matched contexts with and without retrieved evidence, and these action-specific scores are averaged within each rollout to form a process reward added independently of answer and format rewards in GRPO — distinguishing whether a conclusion is correct from how reliably it could be reached.

3. **Acknowledged limitation / behavior (Section 8.2 "Reliability-Aware Search Behavior"):** Dataset-level search counts do not reveal whether the policy responds to uncertainty at individual decisions; examining search frequency across local uncertainty levels shows the policy searches less often than HiPRAG at low no-evidence counterfactual uncertainty while both search frequently at high uncertainty, and selected searches produce larger mapped reliability gains ΔR than HiPRAG's, especially at higher uncertainty.

---

## Paper 2: TREC iKAT 2023: A Test Collection for Evaluating Conversational and Interactive Knowledge Assistants

- **arXiv ID:** 2405.02637 (v1)
- **Title:** TREC iKAT 2023: A Test Collection for Evaluating Conversational and Interactive Knowledge Assistants
- **Authors:** Mohammad Aliannejadi, Zahra Abbasiantaeb, Shubham Chatterjee, Jeffery Dalton, Leif Azzopardi
- **Primary subject category (from abs page):** Information Retrieval (cs.IR); cross-listed with Artificial Intelligence (cs.AI) and Computation and Language (cs.CL)
- **Submission date (from abs page):** [v1] Sat, 4 May 2024 11:22:16 UTC (199 KB). Abs page: https://arxiv.org/abs/2405.02637
- **Abs page category line:** Subjects: Information Retrieval (cs.IR); Artificial Intelligence (cs.AI); Computation and Language (cs.CL). Comments: To appear in SIGIR 2024.

### Abstract summary

Conversational information seeking has evolved rapidly with LLMs, providing the basis for interpreting and responding to user requests in a naturalistic manner. The extended TREC Interactive Knowledge Assistance Track (iKAT) collection enables researchers to test and evaluate Conversational Search Agents (CSAs) with 36 personalized dialogues over 20 topics, each coupled with a Personal Text Knowledge Base (PTKB) defining bespoke user personas. The collection provides 344 turns and ~26,000 passages for relevance assessments plus response evaluations over four dimensions (relevance, completeness, groundedness, and naturalness), challenging CSAs to navigate diverse personal contexts, elicit pertinent persona information, and employ context for relevant conversations, with the PTKB integration and emphasis on decisional search tasks making it an essential benchmark for conversational and interactive knowledge assistants.

### Extracted findings (with source sections)

1. **Evaluation result (Section 5.1 "Passage Ranking"):** Automatic runs generally perform better with a G→R→G pipeline (leveraging LLM learned knowledge, e.g., GPT-4 and Llama) than R→G runs, since learned LLM knowledge gives a better starting point for retrieval and response generation. Performance varies by topic (topics 12-1 and 21-1 are easiest; 19-1 and 20-2 are most difficult), and although PTKB statement ranking(introduction) was expected to add difficulty, in most cases PTKB dependence leads to lower performance; the highest difference appears in the first turn, suggesting predicting the right PTKB statements early is essential, and performance is best in the first turn and degrades as the conversation progresses.

2. **Design decision (Section 3 "The iKAT Resources"):** The track defines main evaluation tasks where responses must cite at least one "provenance" passage from the collection (maintaining passage/provenance ranking similar to TREC CAsT), must consider previous conversational turns as context (equivalent to taking parents in the conversational topic tree), and must utilize the personal information in the PTKB (narrative sentences assumed collected from the user's previous conversations capturing preferences). The collection contains twenty topics with one to three distinct personas each, providing 36 personalized conversational dialogues, and submissions were analyzed with both automatic and human (Prolific) annotations.

3. **Acknowledged limitation (Sections 1 "Introduction" and 2 "Related Work"):** The authors highlight that conversational search agent evaluation remains challenging due to the dynamic, context-dependent, and personalized nature of user-system interactions, and that the same request may yield considerably different responses based on prior conversation and user preferences. They present performance results of TREC submissions with additional baseline comparisons analyzed from multiple perspectives to illustrate the usability of the resources and highlight their limitations, noting that resources to evaluate CSAs are currently limited despite numerous existing LLM evaluation test collections.

---

## Paper 3: A Conceptual Framework for Conversational Search and Recommendation: Conceptualizing Agent-Human Interactions During the Conversational Search Process

- **arXiv ID:** 2404.08630 (v1)
- **Title:** A Conceptual Framework for Conversational Search and Recommendation: Conceptualizing Agent-Human Interactions During the Conversational Search Process
- **Authors:** Leif Azzopardi, Mateusz Dubiel, Martin Halvey, Jeffery Dalton
- **Primary subject category (from abs page):** Information Retrieval (cs.IR); cross-listed with Artificial Intelligence (cs.AI)
- **Submission date (from abs page):** [v1] Fri, 12 Apr 2024 17:48:18 UTC (149 KB). Abs page: https://arxiv.org/abs/2404.08630 (Journal reference: The Second International Workshop on Conversational Approaches to Information Retrieval (CAIR 2018) at ACM SIGIR)

### Abstract summary

The conversational search task aims to enable a user to resolve information needs via natural language dialogue with an agent. This paper develops a conceptual framework of the actions and intents of users and agents, explaining how these actions enable the user to explore the search space and resolve their information need. It outlines the different actions and intents, then discusses key decision points in the conversation where the agent needs to decide how to steer the conversational search process to a successful and/or satisfactory conclusion. The paper provides a formalization of the conversational search process between an agent and user, offering a framework and starting point for research, development, and evaluation of conversational search agents.

### Extracted findings (with source sections)

1. **User action taxonomy (Section 2 "Conversational Actions and Agent Decisions," Section 2.1 "User Actions"):** The paper defines a non-exhaustive list of user actions grouped into Reveal Actions (Disclose-volunteer/inquire/unsure/not, Revise, Expand), Inquire Actions (List, Summarize, Subset, Compare, Similar), Navigate Actions (Repeat, Back, More), the Interrupt Action (to stop irrelevant agent behavior), Interrogate Actions (Understand, Explain), and Closing Actions (Complete, or graceful closure such as "I'll get back to you"). These are illustrated with holiday-planning and current-affairs examples and grounded in prior work on conversational agent abilities (filtering fillers, decision-making, answer aggregation, conversation management, general knowledge, and personality/moral responsibility).

2. **Agent actions and dialog policy (Section 3.2 "Agent Actions" and Section 3.3 "Agent Decisions and Tasks"):** Agent actions include Elicit (criteria and constraints), Clarify (resolving ambiguity, e.g., "On the 4th" → May 4th vs. June 4th), Reveal (Show, Summarize, Compare, Subset, Similar), Traverse (Repeat, Back, Record), Suggest (Recommend, Hypothesize alternative information needs/AINs), Explain (Report, Reason), and Error/Finalization recovery actions. The paper's central design challenge is the **Agent Dialog Policy**: given preceding conversation and the user's request, the agent must decide whether to inquire-elicit, reveal-list, reveal-summarize, or explain-report, and crafting/inferring this policy is described as "an open challenge and core task of a conversational search agent." Lower-level tasks include Intent Identification and Extraction, Inquire-Elicit Question Selection, Inquire-Clarify Requirement Clarification (trade-off between conversation efficiency and accuracy of the represented information need), and the Elicit-Reveal Decision (when to continue eliciting requirements vs. revealing options).

3. **Acknowledged limitation (Section 2 "Assumptions"):** The authors explicitly acknowledge that their framework rests on focused assumptions: the agent is assumed cooperative (not adversarial), aims to minimize conversational effort (avoiding excessive questions that burden the user's cognitive capacity), and aims to maximize the range/number of relevant options provided. They note that a "meaningful response" requiring the agent to decide what action to take at any point is "both very much open challenges that have yet to be fully addressed," and that their simulated conversations assume the agent can perfectly understand user utterances and provide meaningful responses — capabilities they flag as ongoing research problems.

---

## Key takeaways for the team

- **Reliable search decisions beat brute-force retrieval (arXiv:2610.41738):** Scoring search vs. no-search decisions with action-specific reliability signals (current-state reliability + search-action reliability) yields a 69.3 average CEM (+2.1 over HiPRAG) with ~6.8% fewer searches overall and task-dependent allocation (fewer on single-hop, more on multi-hop QA). For our agentic search assistant, Build in uncertainty-based search控制 and report per-task search budgets.
- **Evaluation infrastructure is the bottleneck (arXiv:2405.02637):** Fair comparison for conversational search agents requires curated test collections, provenance-citing responses, and both automatic and human judgments; first-turn PTKB prediction quality matters most, and G→R→G LLM pipelines clearly beat R→G baselines on PTKB provenance (nDCG@3 0.7254 vs. 0.4515 in prior iKAT analysis) with performance degrading over conversation turns.
- **A shared vocabulary helps (arXiv:2404.08630):** The conceptual framework gives the team a common taxonomy of user actions (Reveal, Inquire, Elicit, Clarify, Navigate, Interrogate) and agent dialog-policy decisions (inquire-elicit vs. reveal), which is directly useful for specifying requirements for our own agentic search assistant and for writing better tool descriptions and action orderings.

## How to use this brief

This file is committed at the repo root on the `add-reading-notes` branch via pull request for team review. Team members can comment on `reading-notes.md` directly or on this PR before Thursday's sync. Each entry includes the arXiv ID, title, full author list, subject category (verified on the public abs page), submission date, a 2–3 sentence abstract summary, and at least two concrete findings from the full text tagged with their source sections, plus cross-paper takeaways for the sync.
