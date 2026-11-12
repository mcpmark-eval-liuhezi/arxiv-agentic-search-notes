# Reading Notes: Agentic Search and Tool Use

## Overview

This brief summarizes recent arXiv papers relevant to **agentic search** — assistants where an LLM-driven agent searches, calls tools, and hands answers to users. It was prepared for Thursday's team sync. We covered three papers along these themes: (1) a conceptual framework for conversational search agents and the actions/intents agents need to perform (arXiv:2404.08630); (2) a test collection and overview of TREC iKAT for evaluating conversational and interactive knowledge assistants built on LLMs (arXiv:2401.01330); and (3) RetryBench, an evaluation protocol for retry decisions and failure recovery in tool-using LLM agents (arXiv:2610.15884). For each paper we record the arXiv ID, title, authors, primary subject category, submission date (as shown on the arXiv abs page), a 2–3 sentence abstract summary, and at least two concrete findings extracted from the full text with their source sections. The team can comment on this file via pull request review.

All abs pages were checked publicly (https://arxiv.org/abs/<ID>); primary category and submission date are recorded per the abs page for each paper.

---

## Paper 1: TREC iKAT 2023: The Interactive Knowledge Assistance Track Overview

- **arXiv ID:** 2401.01330 (v2)
- **Title:** TREC iKAT 2023: The Interactive Knowledge Assistance Track Overview
- **Authors:** Mohammad Aliannejadi, Zahra Abbasiantaeb, Shubham Chatterjee, Jeffery Dalton, Leif Azzopardi
- **Primary subject category (from abs page):** Information Retrieval (cs.IR) (cross-listed with cs.AI, cs.CL)
- **Submission date (from abs page):** Submitted on 2 Jan 2024 (v1); last revised 22 Feb 2024 (this version, v2). v1: Tue, 2 Jan 2024 18:40:03 UTC; v2: Thu, 22 Feb 2024 17:29:03 UTC.
- **Abs page URL:** https://arxiv.org/abs/2401.01330

### Abstract summary

Conversational Information Seeking has evolved rapidly with Large Language Models providing the basis for interpreting and responding to user requests in a naturalistic manner. iKAT emphasizes creating and researching conversational search agents that adapt responses based on the user's prior interactions and present context, so the same question can yield varied answers contingent on the user's profile and preferences. The track's first year attracted seven teams and 24 runs, most of which leveraged LLMs in their pipelines, with a few using a generate-then-retrieve approach. The paper defines three core tasks — Statement Ranking, Passage Ranking, and Response Generation (where every response must cite at least one provenance passage) — over a manually curated ClueWeb22-B subset of ~116.8M passages, and reports evaluation results across participant and automatic runs.

### Extracted findings (with source sections)

1. **Evaluation result (Section 6.1 "Passage Ranking," Table 3/Table 5, NIST assessment):** G→R→G pipelines (e.g., LLaMA-2 7B with multiple task-specific prompts) tend to perform better than R→G models, since leveraging learned LLM knowledge gives a better starting point for retrieval and then response generation. On the PTKB provenance task (NIST assessment), the strongest automatic run (IRLab-Amsterdam run-1-llama-zero-shot) achieved nDCG@3 of 0.7254, P@3 of 0.4626, Recall@3 of 0.6964, and MRR of 0.7950, while participant runs generally scored lower (e.g., georgetown_infosense_ikat_run_1-3: nDCG@3 0.4515, MRR 0.5446). The authors note that performance is highest in the first turn and degrades as the conversation progresses, with the largest difference appearing in the first turn, suggesting predicting the right PTKB statements early is essential.

2. **Design decision (Section 3 "Track, Tasks, Data, and Resources" / Section 2 "Introduction"):** The track defines three tasks — Statement Ranking (ranking relevant PTKB statements), Passage Ranking (retrieving and ranking relevant passages), and Response Generation (fluent, non-redundant answers that cite at least one provenance passage). The collection is a manually curated subset of ClueWeb22-B (116,838,987 passages) distributed by CMU, segmented using a MS MARCO-like methodology (documents shortened to 10,000 characters, 10-sentence passages with a 5-sentence stride). Four baseline runs were provided, including a LLaMA-2 7B multi-prompt pipeline and BM25+RM3 with manual and automated PTKB statement selection.

3. **Acknowledged limitation (Section 6 "Results" and Section 1 "Introduction"):** The authors highlight that conversational search agent evaluation remains complex due to the large interaction space and trade-offs between costs and benefits of different interactions (e.g., query suggestions vs. query clarifications). Their analysis reveals there is no superior or dominant combination — query clarifications are better when asked first, while query suggestions are better when asked after presenting results — and that the best strategy depends on trade-offs between querying costs, initial query performance, assessments per query, and total gain required.

---

## Paper 2: RetryBench: Separating Retry Permission from Recovery Outcomes in GPT Tool Use

- **arXiv ID:** 2610.15884 (v4)
- **Title:** RetryBench: Separating Retry Permission from Recovery Outcomes in GPT Tool Use
- **Authors:** You Liu, Yue Liu, Quanchao Lu
- **Primary subject category (from abs page):** Artificial Intelligence (cs.AI) (cross-listed with cs.CR, cs.SE)
- **Submission date (from abs page):** Submitted on 11 Oct 2026 (v1); current version v4 dated Wed, 4 Nov 2026 07:00:04 UTC. Version history: v1 Sun, 11 Oct 2026; v2 Sun, 18 Oct 2026; v3 Thu, 22 Oct 2026; v4 Wed, 4 Nov 2026.
- **Abs page URL:** https://arxiv.org/abs/2610.15884

### Abstract summary

LLM agents that call external tools must decide what to do when a tool call times out: retrying risks duplicate side effects, while not retrying can leave the task incomplete. RetryBench is an evaluation protocol for retry decisions and failure recovery in tool-using LLM agents, scoring each decision on retry permission (is the retry justified by an idempotency-key guarantee or proof of non-execution?), settled effects (duplicates and omissions), and completion, by replaying decisions across all hidden execution worlds consistent with the observation. On four synthetic software-engineering tasks, two controlled prompt-sensitivity studies (1,728 decisions) show that GPT-5.6 Luna reuses the valid idempotency key in 10/40 decisions when an extra key is described as a "Candidate recovery key" but in 39/40 when it is an "Alternative key string" (27/120 vs. 113/120 across paraphrased families), and that retry rates are 17–27 points lower for GPT-5.6 Terra and GPT-5.4 when retry is listed first (option-order bias). The authors explicitly note findings are limited to researcher-chosen phrasings, four tasks, and unpinned model aliases, and that they make no claim about which behavior (retry vs. wait) is better.

### Extracted findings (with source sections)

1. **Evaluation result (Section 5.1 "Descriptions change safe recovery (Study 1)" and Section 5.2 "Supporting action-order sensitivity (Study 2)"):** With the contract fixed, GPT-5.6 Luna reuses the valid idempotency key in 10/40 decisions when an extra key is described as a "Candidate recovery key" but in 39/40 when described as an "Alternative key string"; across three further phrasings per family the gap is 27/120 versus 113/120. Sol and Terra are at ceiling (40/40 protected recovery in every original arm). In Study 2, under underspecified lookup guarantees, retry rates are 17–27 points lower for GPT-5.6 Terra and GPT-5.4 when retry is listed first (option-order bias), mainly on two tasks; the authors report this as position sensitivity with no claim about which behavior is better. Alternative and Unrelated both produce zero duplicates, but Alternative yields substantially more protected recovery (39/40 vs. 22/40), showing the main difference is safe progress, not just duplicate avoidance.

2. **Design decision / measurement approach (Section 4 "Method," Section 4.2 "Retry admissibility," Section 4.3 "Recovery worlds, actions, and outcomes"):** RetryBench separates retry permission from settled effects and recovery completion, replaying each decision across compatible hidden worlds rather than relying on one-dimensional recovery scores. A retry is justified in either of two situations: evidence can establish that the original attempt did not execute (proof of terminal non-execution), or the original valid idempotency key can make another dispatch safe by preventing duplicates. The protocol uses four software tasks (CI, issue, release, and review comment) with one recovery decision per episode, all main-study GPT requests use reasoning "none," a 256-token output cap, strict JSON output, and temperature omitted (provider default of 1.0); there are 1,728 main-study decisions total.

3. **Acknowledged limitation (Section 7 "Threats to validity" and the Abstract):** The authors state that RetryBench here denotes the scoring protocol and its four-task probe suite, not a leaderboard or a general estimate of agent reliability. Findings are limited to researcher-chosen phrasings, four tasks, and unpinned model aliases (Sol = gpt-5.6-sol, Terra = gpt-5.6-terra, Luna = gpt-5.6-luna, GPT-5.4 = gpt-5.4-2026-03-05). Requested and returned model strings match, but no backend fingerprint was supplied, so the logs cannot establish stable backend weights. The authors also note they make no claim about which behavior (retry vs. wait) is better, and that an unrelated operation's key is used only as a controlled stress case for keeping operation identity straight, not as an estimate of how often this occurs in deployment.

---

## Paper 3: A Conceptual Framework for Conversational Search and Recommendation: Conceptualizing Agent-Human Interactions During the Conversational Search Process

- **arXiv ID:** 2404.08630 (v1)
- **Title:** A Conceptual Framework for Conversational Search and Recommendation: Conceptualizing Agent-Human Interactions During the Conversational Search Process
- **Authors:** Leif Azzopardi, Mateusz Dubiel, Martin Halvey, Jeffery Dalton
- **Primary subject category (from abs page):** Information Retrieval (cs.IR) (cross-listed with cs.AI)
- **Submission date (from abs page):** Submitted on 12 Apr 2024 (v1, Fri, 12 Apr 2024 17:48:18 UTC).
- **Abs page URL:** https://arxiv.org/abs/2404.08630 (Journal reference: The Second International Workshop on Conversational Approaches to Information Retrieval (CAIR 2018) at ACM SIGIR)

### Abstract summary

The conversational search task aims to enable a user to resolve information needs via natural language dialogue with an agent. This paper develops a conceptual framework of the actions and intents of users and agents, explaining how these actions enable the user to explore the search space and resolve their information need. It outlines the different actions and intents, then discusses key decision points in the conversation where the agent needs to decide how to steer the conversational search process to a successful and/or satisfactory conclusion. The paper provides a formalization of conversational search between an agent and user, offering a framework and starting point for research, development, and evaluation of conversational search agents.

### Extracted findings (with source sections)

1. **User action taxonomy (Section 2.1 "User Actions," Table 1):** The paper defines a non-exhaustive list of user actions grouped into Reveal Actions (Disclose-volunteer/inquire/unsure/not, Revise, Expand), Inquire Actions (List, Summarize, Subset, Compare, Similar), Navigate Actions (Repeat, Back, More), the Interrupt Action (to stop irrelevant agent behavior), Interrogate Actions (Understand, Explain), and Closing Actions (Complete, or良好 closure such as "I'll get back to you"). These are illustrated with holiday-planning examples and generalized to other search scenarios, and the framework is grounded in prior work on conversational agent abilities (filtering fillers, decision-making, answer aggregation, conversation management, general knowledge, and personality/moral responsibility).

2. **Agent actions and dialog policy (Section 3.2 "Agent Actions" and Section 3.3 "Agent Decisions and Tasks"):** Agent actions include Elicit (criteria and constraints), Clarify (resolving ambiguity like "On the 4th" → May 4th vs. June 4th), Reveal (Show, Summarize, Compare, Subset, Similar), Traverse (Repeat, Back, Record), Suggest (Recommend, Hypothesize alternative information needs/AINs), Explain (Report, Reason), and Error/Finalization recovery actions. The paper's central design challenge is the **Agent Dialog Policy**: given preceding conversation and the user's request, the agent must decide whether to inquire-elicit, reveal-list, reveal-summarize, or explain-report, and crafting/inferring this policy is described as "an open challenge and core task of a conversational search agent." Lower-level tasks include Intent Identification and Extraction, Inquire-Elicit Question Selection (asking questions that reduce the search space), Inquire-Clarify Requirement Clarification (trade-off between conversation efficiency and accuracy of the represented information need), and Elicit-Reveal Decision (when to continue eliciting requirements vs. revealing options).

3. **Acknowledged limitation (Section 2 "Assumptions"):** The authors explicitly acknowledge that their framework rests on focused assumptions: the agent is assumed cooperative (not adversarial), aims to minimize conversational effort (avoiding excessive questions that burden the user's cognitive capacity), and aims to maximize the range/number of relevant options provided. They note that a "meaningful response" requiring the agent to decide what action to take at any point is "both very much open challenges that have yet to be fully addressed," and that their simulated conversations assume the agent can perfectly understand user utterances and provide meaningful responses — capabilities they flag as ongoing research problems.

---

## Key takeaways for the team

- **Tool harness design matters as much as policy:** RetryBench (arXiv:2610.15884) shows that how you phrase optional recovery keys and list retry actions substantially changes agent recovery behavior (e.g., 10/40 vs. 39/40 protected recovery for Luna depending on wording), so tool description and action ordering are first-class design decisions for any agent that calls tools.
- **Evaluation infrastructure is the bottleneck for conversational search research:** The iKAT overview (arXiv:2401.01330) shows that fair comparison requires curated test collections, provenance-citing responses, and both automatic and human (Prolific) judgments, since G→R→G LLM pipelines clearly beat R→G baselines on PTKB provenance (nDCG@3 0.7254 vs. 0.4515) and first-turn performance is critical.
- **A shared vocabulary helps:** The conceptual framework (arXiv:2404.08630) gives the team a common taxonomy of user actions (Reveal, Inquire, Elicit, Clarify, Navigate, Interrogate) and agent dialog-policy decisions (inquire-elicit vs. reveal), which is directly useful for specifying requirements for our own agentic search assistant.

## How to use this brief

This file is committed at the repo root on the `add-reading-notes` branch via pull request for team review. Team members can comment on `reading-notes.md` directly or on this PR before Thursday's sync. Each entry includes the arXiv ID, title, full author list, subject category (verified on the public abs page), submission date, a 2–3 sentence abstract summary, and at least two concrete findings from the full text tagged with their source sections, plus cross-paper takeaways for the sync.
