# Reading Notes: Agentic Search and Tool Use in LLM Agents

**Prepared for:** Thursday team sync on agentic search (LLM-driven assistants that search, call tools, and hand answers to users)
**Source:** arXiv searches for "conversational search agents", "tool use in LLM agents", and "user trust in AI assistants"; full texts read end to end; public arXiv abstract pages checked for primary category and submission date.

## Overview

This brief covers recent arXiv literature on **agentic search** — assistants where an LLM-driven agent searches, calls tools, and hands answers to users. It includes two papers:

1. **arXiv:2405.02637** — *TREC iKAT 2023: A Test Collection for Evaluating Conversational and Interactive Knowledge Assistants* (cs.IR, submitted 4 May 2024). A benchmark paper on evaluating conversational search agents (CSAs) with persona-aware dialogues and a personal text knowledge base.
2. **arXiv:2611.12125** — *CARE-PO: Controllable Tool-Call Efficiency for LLM Agents* (cs.LG, submitted 9 Nov 2026). An experimental paper on making LLM agent tool calls cheaper and more controllable during training.

For each paper we record the arXiv ID, title, author list, subject category, submission date (as shown on the public arXiv abstract page), a 2-3 sentence abstract summary, and at least two concrete findings extracted from the full text, each tagged with the section it came from.

---

## Paper 1: arXiv:2405.02637 — TREC iKAT 2023: A Test Collection for Evaluating Conversational and Interactive Knowledge Assistants

**Authors:** Mohammad Aliannejadi, Zahra Abbasiantaeb, Shubham Chatterjee, Jeffery Dalton, Leif Azzopardi
**Subject category (from abs page):** Information Retrieval (cs.IR); also cs.AI and cs.CL. Primary category: **cs.IR**
**Submission date (from abs page):** [v1] Sat, 4 May 2024 11:22:16 UTC
**Abstract page:** https://arxiv.org/abs/2405.02637 (to appear in SIGIR 2024)

### Abstract summary

This paper extends the TREC Interactive Knowledge Assistance Track (iKAT) test collection to enable researchers to test and evaluate their Conversational Search Agents (CSAs). The collection provides 36 personalized dialogues over 20 different topics, each coupled with a Personal Text Knowledge Base (PTKB) that defines bespoke user personas, with 344 turns and approximately 26,000 passages available for assessing relevance, plus response evaluations on four key dimensions: relevance, completeness, groundedness, and naturalness. The integration of a personal knowledge base and emphasis on decisional search tasks make it an essential benchmark for advancing research in conversational and interactive knowledge assistants.

### Extracted findings (from full text)

- **Finding 1 (Section 3, "The iKAT Resources"):** The iKAT resources extend the TREC iKAT 2023 test collection by providing human and automatic annotations of generated responses, plus further analysis of submissions based on these annotations. The collection contains 36 personalized dialogues over 20 topics, each with a Personal Text Knowledge Base (PTKB) of narrative sentences assumed collected from previous user-system conversations (capturing preferences like "prefers pizza to pasta"); responses must cite at least one "provenance" passage, keeping passage/provenance ranking similar to TREC CAsT, and agents should utilize the personal information in the PTKB when forming responses. Main evaluation tasks include a PTKB Statement Ranking Task and response quality assessed on relevance, completeness, groundedness, and naturalness.
- **Finding 2 (Section 5.3, "Response Evaluation"):** In human evaluation (Table 4), run-4-GPT-4 significantly outperformed all other methods in both relevance and completeness, mirroring its success in passage ranking, while the Organizers' baseline model `llama2_only_10_docs` ranked third and was the least grounded, with only 23 of 73 responses deemed grounded — suggesting its high scores may stem from unfaithfulness to retrieved passages (possibly relying on internal knowledge). The authors also note that GPT-4-based labels can be relied on to rank models even when GPT-4 itself is among the runs, though absolute differences favor self-generated text, consistent with prior findings that LLMs tend to favor text they generated themselves when acting as evaluators.
- **Finding 3 (Section 2, "Related Work"):** TREC iKAT evolved TREC CAsT into a new track to support multi-path, multi-turn, and multi-perspective conversations, where for a given topic the direction and conversation that evolves depends not only on prior responses but also on the persona of the user — addressing limitations of earlier tracks that focused on document retrieval over feedback rounds rather than conversation.

---

## Paper 2: arXiv:2611.12125 — CARE-PO: Controllable Tool-Call Efficiency for LLM Agents

**Authors:** Xiaoyang Ye, Xiao Yao, Hairui Zhu, Yiying Yang, Bei Gan, Jinliang Lu, Tengjin Weng, Yuan Yao, Wenhao Jiang
**Subject category (from abs page):** Machine Learning (cs.LG); also cs.AI. Primary category: **cs.LG**
**Submission date (from abs page):** [v1] Mon, 9 Nov 2026 09:41:37 UTC
**Abstract page:** https://arxiv.org/abs/2611.12125 (under review at ICLR 2027)

### Abstract summary

External tool calls are a major running cost of LLM agents, while their training objectives align almost exclusively with task completion. The paper presents CARE-PO (Controllable Anchored Relative Efficiency Policy Optimization), which prices calls against a reference budget anchored to each task's own difficulty and fixed before training, injects a successful trajectory from training data into each online GRPO rollout group, and uses an effort tag in the prompt to yield both an efficiency and a capability operating point from a single training run. On GAIA, five of the six operating points across three model sizes improve on their base model in success rate and call count at once; on four held-out multi-hop benchmarks the result is size-dependent (success rate held or raised at 27B and 4B, while at 9B both points trade success rate for fewer calls). The call count remains well above zero throughout training at all three sizes.

### Extracted findings (from full text)

- **Finding 1 (Section 4.1, "Diagnosis of Redundant Calls"):** In a step-level diagnosis of 221 GAIA trajectories from Qwen3.5-9B and 27B, 82.8% contain a removable (deletable or mergeable) call; those trajectories average 5.7 removable calls, and 68.0% of all calls are removable without breaking the answer's evidence chain. The authors group redundant calls into three forms: (i) calls with no increment (aimless search, re-verification of a confirmed result, one acquisition split into several), (ii) repeated calls (tail repetition loops that persist to the call cap and leave the trajectory unable to answer), and (iii) evidence gaps (a cue that could advance the task appears and goes unused). Forms (ii) and (iii) are most associated with wrong answers even though the call count misses their harm, so Section 4.4 prices each with a term of its own.
- **Finding 2 (Section 6, "Main Results"):** On GAIA, five of the six operating points across three model sizes improve on their base model in success rate and call count at once, raising success rate by 3.4 to 6.1 points (p=0.01 for the largest) and reducing calls by 8.1% to 51.2%; the sixth (the efficiency point at 9B) is statistically indistinguishable in success rate (p=0.89) while calling 58.6% less, and over half the reduction comes from base-model trajectories that exhaust the call cap, almost all of them failures. On four held-out multi-hop benchmarks the effect is size-dependent: at 27B neither point differs significantly from the base model, at 4B neither falls significantly below it and the capability point rises significantly on three of four, while at 9B both points fall below it on all four (9B efficiency point calls 23% as often as base, vs 35% at 27B and 48% at 4B).
- **Finding 3 (Section 8, "Conclusion and Limitations"):** At all three sizes the group-mean call count remains well above zero throughout training, whereas the OTC-PO baseline collapses toward issuing no call at 27B (its call count falls to 0.01 per rollout after step 300). The authors acknowledge limitations: on held-out two-hop benchmarks, compression readily encroaches on necessary calls (9B compressed most, trading success rate at both points); the reward presupposes automatically gradable goals and a successful source trajectory for each training task; and due to lack of compute, training and evaluation cover only question answering (GAIA and four short-answer multi-hop benchmarks) on one model family (Qwen3.5).

---

## Discussion points for the team sync

- **Evaluation infrastructure matters as much as model quality:** iKAT shows that persona-aware, knowledge-grounded benchmarks (with provenance citation requirements) are needed to properly evaluate conversational search agents (CSAs); bare retrieval metrics are insufficient.
- **Tool-call efficiency is a first-class training concern:** CARE-PO shows that pricing tool calls against a task-anchored reference budget (fixed before training), plus a source-trajectory floor in GRPO rollout groups, can cut call counts by up to 51.2% on GAIA without sacrificing success rate, and keeps call counts from collapsing to zero.
- **Size matters:** gains are clearer at 27B and 4B; at 9B compressed models trade success rate for fewer calls, so deployment choices (model size, effort tier) should be explicit rather than hidden in training objectives.
- **Trust and grounding:** the iKAT response evaluation (Section 5.3) found `llama2_only_10_docs` grounded only 23/73 responses, flagging faithfulness risks that underpin user trust in AI assistants.

*Note: Both papers were selected from recent arXiv listings searched under the phrasings "conversational search agents", "tool use in LLM agents", and "user trust in AI assistants". Full texts were read via the arXiv download tooling, and subject category and submission dates were verified on each paper's public arXiv abstract page.*
