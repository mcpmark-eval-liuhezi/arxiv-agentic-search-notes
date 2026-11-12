# Reading Notes: Agentic Search and Tool Use

## Overview

This brief summarizes recent arXiv papers relevant to **agentic search** — assistants where an LLM-driven agent searches, calls tools, and hands answers to users. It was prepared for Thursday's team sync. We covered three papers along these themes: (1) a conceptual framework for conversational search agents and the actions/intents agents need to perform; (2) a test collection (TREC iKAT) for evaluating conversational and interactive knowledge assistants; and (3) a recent reinforcement learning paper on co-evolving agent policies with executable harnesses (tools) for LLM agents. For each paper we record the arXiv ID, title, authors, primary subject category, submission date (as shown on the arXiv abs page), a 2–3 sentence abstract summary, and at least two concrete findings extracted from the full text with their source sections. The team can comment on this file via pull request review.

---

## Paper 1: CoHARL: Co-Evolving Agent Policies and Executable Harnesses with Reinforcement Learning

- **arXiv ID:** 2610.36469
- **Title:** CoHARL: Co-Evolving Agent Policies and Executable Harnesses with Reinforcement Learning
- **Authors:** Jianwen Chen, Peng Xia, Kaide Zeng, Siwei Han, Zhen-Yu Zhang, Charles Fleming, Xinyu Ye, Huaxiu Yao
- **Primary subject category (from abs page):** Computer Science > Machine Learning (cs.LG)
- **Submission date (from abs page):** Submitted on 27 Oct 2026 (v1, Tue, 27 Oct 2026 01:25:26 UTC)
- **Abs page URL:** https://arxiv.org/abs/2610.36469

### Abstract summary

CoHARL is a population-based reinforcement learning framework where an LLM agent's policy and its executable harnesses (the programs governing tool use and feedback) are co-evolved throughout training. A proposer model uses the policy's execution traces to revise harness code, while a scheduler allocates rollouts across an evolving population of up to eight harness versions so that one shared policy learns from diverse interfaces. On SWE-rebench, CoHARL achieves a 27.6% mean resolved rate, improving over harness-only search (Meta-Harness, 21.2%) by 6.4 percentage points and over policy-only training (Plain GRPO, 20.3%) by 7.3 points, with gains also shown on SWE-bench Verified and Terminal-Bench 2.0.

### Extracted findings (with source sections)

1. **Evaluation result (Section 4.2 Main Results / Table 1):** CoHARL achieves a **27.6% resolved rate** on the 110-task SWE-rebench evaluation split (avg@3, temperature 0.7), outperforming Meta-Harness (harness-only search, 21.2%) by **6.4 percentage points** and Plain GRPO (policy-only RL, 20.3%) by **7.3 points**. Both baselines also improved over the initial Base agent (17.3%), showing that interface optimization alone can help but neither harness-only nor policy-only approaches matches joint policy–harness adaptation. Cross-dataset generalization was verified by evaluating the SWE-rebench-trained agent on the 500-task SWE-bench Verified benchmark.

2. **Design decision / limitation (Section 3.4 Task Rewards and Evaluator Integrity):** The authors design strict evaluation integrity for SWE: success is strictly binary (r=1 iff all fail_to_pass tests pass and all pass_to_pass tests continue to pass), with forced submission at episode end and **no LLM judge**. They also implement an audit gate that rejects detected attempts to reintroduce hidden-test access; code inspection found no hidden-test access in five admitted harnesses, and a separate probe rejected six test-access proposals. The paper notes that a retained SWE group need not contain a fully resolved episode, since retention diagnostics use a separate filtering score rather than evaluation resolution.

3. **Training setup (Section 4.1 Setup):** They train **Qwen3.5-35B-A3B** directly with RL using rank-32 LoRA based on SkyRL, for 150 steps with a target of 48 rollouts per step distributed across a pool of up to eight harness versions. At each search invocation, Claude Opus 4.7 proposes up to two candidates, which are checked per Section 3.2. CoHARL uses approximately as many training episodes as the RL-only baseline, with additional computation for harness validation and proposer inference.

---

## Paper 2: TREC iKAT 2023: A Test Collection for Evaluating Conversational and Interactive Knowledge Assistants

- **arXiv ID:** 2405.02637
- **Title:** TREC iKAT 2023: A Test Collection for Evaluating Conversational and Interactive Knowledge Assistants
- **Authors:** Mohammad Aliannejadi, Zahra Abbasiantaeb, Shubham Chatterjee, Jeffery Dalton, Leif Azzopardi
- **Primary subject category (from abs page):** Information Retrieval (cs.IR) (cross-listed with cs.AI, cs.CL)
- **Submission date (from abs page):** Submitted on 4 May 2024 (v1, Sat, 4 May 2024 11:22:16 UTC)
- **Abs page URL:** https://arxiv.org/abs/2405.02637

### Abstract summary

This paper introduces the extended TREC Interactive Knowledge Assistance Track (iKAT) collection for evaluating Conversational Search Agents (CSAs) built on LLMs. The collection provides 36 personalized dialogues over 20 topics, each coupled with a Personal Text Knowledge Base (PTKB) defining bespoke user personas, plus 344 turns and approximately 26,000 passages for assessing relevance and generated-response quality across four dimensions: relevance, completeness, groundedness, and naturalness. It challenges CSA to navigate diverse personal contexts, elicit pertinent persona information, and employ context for relevant conversations, and it includes human (Prolific) and automatic (GPT-4) judgments for response generation.

### Extracted findings (with source sections)

1. **Dataset design (Section 3.1 Personalized Conversational Topics):** The iKAT 2023 collection contains **36 personalized conversational dialogues (11 for training and 25 for testing) over 20 different topics**, with up to three personas per topic (e.g., Topic 9 "Finding a Diet" has a vegetarian-with-medical-conditions persona and a middle-aged overweight man with a knee-injury persona). The average dialogue length is 13 turns (427 turns total), and only a subset of turns are classified as personalized turns — those that include at least one relevant PTKB statement. PTKBs were constructed with attention to including only high-level personal information while excluding personally identifiable information to safeguard contributor privacy.

2. **Evaluation methodology (Section 3.5–3.6 Assessments for Response Generation Task):** Response quality is assessed via a Prolific crowdsourcing task on relevance and completeness (grades -1 to 3), with workers compensated according to their country's minimum wage, and via GPT-4 judgments on **groundedness and naturalness**. The automated GPT-4 setup was validated by testing multiple prompts and comparing them to a subset of responses manually labeled by the organizers, using the configuration with the highest agreement with labeled data; the final prompt is released in the associated GitHub repository so others can obtain similar judgments.

3. **Baseline comparisons (Section 3.7 Runs):** The resource includes four author-developed baseline runs (two automatic, two manual) plus **24 participant runs** from TREC iKAT 2023 teams. Most teams used a retrieve-then-generate (R→G) or generate-retrieve-generate (G→R→G) pipeline: PTKB statement relevance prediction, conversational rewriting and query expansion, retrieval with traditional or dense IR models, and multi-stage neural re-ranking. On the PTKB provenance task (Table 3, NIST assessment), the strongest automatic run (run-1-llama-zero-shot) achieved nDCG@3 of 0.7254, P@3 of 0.4626, Recall@5 of 0.6964, and MRR of 0.7950, while participant runs generally scored lower (e.g., georgetown_infosense_ikat_run_1-3: nDCG@3 0.4515, MRR 0.5446).

---

## Paper 3: A Conceptual Framework for Conversational Search and Recommendation: Conceptualizing Agent-Human Interactions During the Conversational Search Process

- **arXiv ID:** 2404.08630
- **Title:** A Conceptual Framework for Conversational Search and Recommendation: Conceptualizing Agent-Human Interactions During the Conversational Search Process
- **Authors:** Leif Azzopardi, Mateusz Dubiel, Martin Halvey, Jeffery Dalton
- **Primary subject category (from abs page):** Computer Science > Information Retrieval (cs.IR) (cross-listed with cs.AI)
- **Submission date (from abs page):** Submitted on 12 Apr 2024 (v1, Fri, 12 Apr 2024 17:48:18 UTC)
- **Abs page URL:** https://arxiv.org/abs/2404.08630

### Abstract summary

This paper develops a conceptual framework of the actions and intents of users and agents in conversational search, providing a formalization of how agents should support users in finding, exploring, and understanding information objects via natural language dialogue. It enumerates the key actions users and agents perform (e.g., Reveal, Inquire, Elicit, Clarify, Navigate, Interrogate) and the decisions agents must make, such as choosing whether to inquire-elicit or reveal-list, and how to balance efficiency of conversation against accuracy of the represented information need. The framework is intended as a starting point for research, development, and evaluation of conversational search agents.

### Extracted findings (with source sections)

1. **User action taxonomy (Section 2.1 User Actions, Table 1):** The paper defines a non-exhaustive list of user actions grouped into: **Reveal Actions** (Disclose-volunteer/inquire/unsure/not, Revise, Expand), **Inquire Actions** (List, Summarize, Subset, Compare, Similar), **Navigate Actions** (Repeat, Back, More), the **Interrupt Action** (to stop irrelevant agent behavior), **Interrogate Actions** (Understand, Explain), and **Closing Actions** (Complete, or良好 closure such as "I'll get back to you"). These are illustrated with holiday-planning examples and generalized to other search scenarios.

2. **Agent actions and dialog policy (Section 2.2 Agent Actions and Section 2.3 Agent Decisions and Tasks):** Agent actions include Elicit (criteria and constraints), Clarify (resolving ambiguity like "On the 4th" → May 4th vs June 4th), Reveal (Show, Summarize, Compare, Subset, Similar), Traverse (Repeat, Back, Record), Suggest (Recommend, Hypothesize alternative information needs/AINs), Explain (Report, Reason), and Error/Finalization recovery actions. The paper's central design challenge is the **Agent Dialog Policy**: given preceding conversation and the user's request, the agent must decide whether to inquire-elicit, reveal-list, reveal-summarize, or explain-report — and crafts/infers this policy is described as "an open challenge and core task of a conversational search agent."

3. **Acknowledged limitation (Section 2 Assumptions):** The authors explicitly acknowledge that their framework rests on focused assumptions: the agent is assumed cooperative (not adversarial), aims to minimize conversational effort (avoiding excessive questions that burden the user's cognitive capacity), and aims to maximize the range/number of relevant options provided. They note that a "meaningful response" requiring the agent to decide what action to take at any point is "both very much open challenges that have yet to be fully addressed," and that their simulated conversations assume the agent can perfectly understand user utterances and provide meaningful responses — capabilities they flag as ongoing research problems.

---

## Key takeaways for the team

- **Tool harness design matters as much as policy:** CoHARL (arXiv:2610.36469) shows that co-evolving the agent's executable harness (tool-use interface) with its policy during RL beats either harness-only search or policy-only RL by meaningful margins (27.6% vs 21.2% vs 20.3% resolved rate on SWE-rebench). Worth considering when we design any agent that calls tools.
- **Evaluation infrastructure is the bottleneck for CSA research:** The iKAT test collection (arXiv:2405.02637) demonstrates that building proper benchmarks (36 personalized dialogues, PTKB provenance judgments, groundedness/naturalness via GPT-4) is essential before we can compare conversational search agents fairly.
- **A shared vocabulary helps:** The conceptual framework (arXiv:2404.08630) gives us a common taxonomy of user and agent actions (Reveal, Inquire, Elicit, Clarify, Navigate, Interrogate) and decisions (inquire-elicit vs. reveal), which is directly useful for specifying requirements for our own agentic search assistant.

## How to use this brief

This file is committed on the `add-reading-notes` branch via pull request for team review. Team members can comment on it through pull request reviews. Let us know if you'd like us to dive deeper into any of these papers before Thursday's sync.
