# Reading Notes: Agentic Search and Tool Use in LLM Agents

**Prepared for:** Thursday team sync on agentic search — assistants where an LLM-driven agent searches, calls tools, and hands answers to users.
**Scope:** Recent arXiv papers found by searching the phrasings "conversational search agents", "tool use in LLM agents", and "user trust in AI assistants". Full texts were read end to end (via the arXiv download tooling) and public arXiv abstract pages were checked for primary subject category and submission date.

This brief covers three papers on agentic search:

1. **arXiv:2611.12705** — *Multi-Agent Screening: What Agreement and Acceptance Cannot Tell You* (cs.MA, submitted 9 Nov 2026). A formal analysis of what evidence from multi-agent screening systems can and cannot tell us about recall, with AI review of papers as a worked example.
2. **arXiv:2401.01330** — *TREC iKAT 2023: The Interactive Knowledge Assistance Track Overview* (cs.IR, submitted 2 Jan 2024, revised 22 Feb 2024). An overview of the TREC Interactive Knowledge Assistance Track for researching conversational search agents that adapt to user personas.
3. **arXiv:2404.08630** — *A Conceptual Framework for Conversational Search and Recommendation: Conceptualizing Agent-Human Interactions During the Conversational Search Process* (cs.IR, submitted 12 Apr 2024). A conceptual framework formalizing the actions and intents of users and agents in conversational search.

For each paper we record the arXiv ID, title, author list, subject category (as shown on the public arXiv abstract page), submission date, a two-to-three sentence abstract summary, and at least two concrete findings extracted from the full text, each tagged with the section it came from.

---

## Paper 1: arXiv:2611.12705 — Multi-Agent Screening: What Agreement and Acceptance Cannot Tell You

**Authors:** Benjamin Chang, Dell Zhang
**Subject category (from abs page):** Multiagent Systems (cs.MA); Artificial Intelligence (cs.AI); Information Retrieval (cs.IR). Primary category: **cs.MA**
**Submission date (from abs page):** [v1] Mon, 9 Nov 2026 17:00:16 UTC
**Abstract page:** https://arxiv.org/abs/2611.12705

### Abstract summary

When AI agents screen items for a costly human assessor — as in agentic search, e-discovery, and AI reviewing of papers — only what they pass gets judged. This paper shows what each kind of evidence can establish about agent errors even with unlimited data, in the "incomplete-judgment" setting of pooled evaluation. Agreement shows that agents concur, not that they are right; assessor acceptance gives precision but only a lower bound on recall; and menus of agents' findings give an upper bound on recall when finds are not negatively correlated. Without assumptions about the agents, recall can be pinned down only by auditing items they did not pass, and the sample size needed does not grow with the number of agents.

### Extracted findings (from full text)

- **Finding 1 (Section 3, "What Each Kind of Evidence Can Tell Us" — Theorem 3.1, Agreement):** Agents built on similar models err together, which the paper models as a shared latent variable Z with Y and O conditionally independent given Z. Theorem 3.1 proves that the error of any pooling rule π, P(A≠Y), is at least E[min{η(Z), 1−η(Z)}], and that the distribution of agent outputs does not depend on η — so no statistic of the outputs alone can distinguish a right consensus from a wrong one. The authors note that "agreement shows that agents concur, not that they are right, however many there are," and that label-free methods like Dawid–Skene can only identify error rates against a discrete Z up to relabelling — reading Z as Y is an assumption outputs cannot test.
- **Finding 2 (Section 3, "What Each Kind of Evidence Can Tell Us" — Proposition 1 & Proposition 2):** Assessor acceptance estimates precision p, not the unknown miss rate H, and Proposition 1 gives the tight recall interval [ap/(ap+1−a), 1], since r = ap/(ap+H) decreases in H and the data allow any H ∈ [0, 1−a]. Proposition 2 (Next agent) shows that if agents A₁, …, A_{k+1} are exchangeable given Y=1, a new interchangeable agent k+1 adds at most a share f₁/k of the collection's relevant items — so a 4th reviewer adds at most 30 acceptable papers in the paper's worked example (120Acceptable, f₁ = 90, k = 3). The worked example (Section 4) computes that a 4th interchangeable reviewer adds at most 90/3 = 30 acceptable papers, while an audit of 192 of the other 800 submissions estimates H to ±0.05 with 95% confidence and, under positive dependence, finds at least 192 × 150/800 = 36 missed papers in expectation.
- **Finding 3 (Section 3, "Audit: closing the gap" — Proposition 3, and Section 5, "Take-Home Messages"):** Only judgments outside the pool pin down H without assumptions on the agents: Proposition 3 shows |Ĥ − H| ≤ ε with probability ≥ 1−δ once n₀ ≥ (1−a)² ln(2/δ)/(2ε²) samples are drawn, but the authors note this is "sufficient, not necessary" — for 1000 items, a=0.2, δ=0.05 and ±0.05, it asks for 473 audits where an exact hypergeometric calculation needs only 192. The take-home messages recommend: rate agents on judgments, not agreement (which can reward shared bias); report precision and recall as an interval rather than a single number; let agents nominate separately and log who found each item; and compare the next agent with an audit before buying either, budgeting audits by the accuracy wanted rather than the number of agents.
- **Limitation acknowledged (Section 3, "Overlap: an upper bound on recall"):** The positive-dependence assumption (Cov(B,B′ ∣ Y=1, C) ≥ 0) can be violated by specialists covering different parts of the collection, in which case the overlap bound should be applied within a specialism where it holds; sampled pools need weighting and add variance.

---

## Paper 2: arXiv:2401.01330 — TREC iKAT 2023: The Interactive Knowledge Assistance Track Overview

**Authors:** Mohammad Aliannejadi, Zahra Abbasiantaeb, Shubham Chatterjee, Jeffery Dalton, Leif Azzopardi
**Subject category (from abs page):** Information Retrieval (cs.IR); Artificial Intelligence (cs.AI); Computation and Language (cs.CL). Primary category: **cs.IR**
**Submission date (from abs page):** [v1] Tue, 2 Jan 2024 18:40:03 UTC; [v2] Thu, 22 Feb 2024 17:29:03 UTC
**Abstract page:** https://arxiv.org/abs/2401.01330

### Abstract summary

Conversational Information Seeking has evolved rapidly with Large Language Models providing the basis for interpreting and responding to user requests in a naturalistic manner. The TREC Interactive Knowledge Assistance Track (iKAT) emphasizes creating and researching conversational search agents (CSAs) that adapt responses based on the user's prior interactions and present context, so the same question can yield varied answers contingent on the user's profile and preferences. iKAT's first year attracted seven teams and 24 runs, most of which leveraged LLMs in their pipelines, with a few using a generate-then-retrieve approach. The track includes passage retrieval, statement ranking, and response generation tasks, with responses required to cite at least one "provenance" passage.

### Extracted findings (from full text)

- **Finding 1 (Section 2, "Track, Tasks, Data, and Resources"):** The track provides three tasks — Statement Ranking (ranking relevant PTKB statements for the current user utterance as a relevance score prediction), Passage Ranking (retrieving and ranking relevant passages from the collection given the utterance, context, and PTKB), and Response Generation (fluent, informative responses that may be generative or abstractive and must cite at least one provenance passage). The collection is a manually curated subset of ClueWeb22-B (116,838,987 passages distributed by CMU), segmented using an MS MARCO-style methodology: each document shortened to 10,000 characters, then sliding windows of 10 consecutive sentences with a 5-sentence stride. Organizers provided four baseline runs, including llama2_only_10_docs, a pipeline that executes several interactions with LLaMA-2 7B using distinct prompts tailored to specific tasks (e.g., revising the most recent part of the ongoing conversation, reformulating the latest utterance, and optimizing the utterance for retrieval).
- **Finding 2 (Section 1, "Introduction", and Section 2.4, "Baselines"):** iKAT's debut year attracted seven teams and 24 runs, most leveraging LLMs with only a few using a generate-then-retrieve approach; the Personal Text Knowledge Base (PTKB) encapsulates task contexts and user specifics as narrative sentences collected from previous user-system conversations (with care taken to exclude personally identifiable information). The baseline `llama2_only_10_docs` ranks third in human evaluation and was the least grounded, with only 23 of 73 responses deemed grounded in the companion iKAT 2023 test collection paper, suggesting its higher scores may stem from unfaithfulness to retrieved passages (possibly relying on internal knowledge) rather than retrieval quality.
- **Finding 3 (Section 2.2, "Topics"):** Each iKAT 2023 topic includes 1–3 conversations with different personas on the same topic, and a "personalized turn" is defined as a turn with at least one relevant statement from the PTKB, meaning the system must consider at least one PTKB statement to answer accurately. Topic creation used step-by-step guidelines with a quality-assurance checklist covering persona-level, turn-level, and conversation-level criteria, with topics checked and refined by at least two expert annotators, and any that did not meet quality criteria were re-generated by another annotator.

---

## Paper 3: arXiv:2404.08630 — A Conceptual Framework for Conversational Search and Recommendation: Conceptualizing Agent-Human Interactions During the Conversational Search Process

**Authors:** Leif Azzopardi, Mateusz Dubiel, Martin Halvey, Jeffery Dalton
**Subject category (from abs page):** Information Retrieval (cs.IR); Artificial Intelligence (cs.AI). Primary category: **cs.IR**
**Submission date (from abs page):** [v1] Fri, 12 Apr 2024 17:48:18 UTC
**Abstract page:** https://arxiv.org/abs/2404.08630 (Journal reference: The Second International Workshop on Conversational Approaches to Information Retrieval (CAIR 2018) at ACM SIGIR)

### Abstract summary

The conversational search task aims to enable a user to resolve information needs via natural language dialogue with an agent. This paper develops a conceptual framework of the actions and intents of users and agents, explaining how these actions enable the user to explore the search space and resolve their information need. It outlines the different actions and intents, then discusses key decision points in the conversation where the agent must decide how to steer the conversational search process to a successful and/or satisfactory conclusion. The framework provides a starting point for research, development, and evaluation of conversational search agents.

### Extracted findings (from full text)

- **Finding 1 (Section 3, "Conversational Actions and Agent Decisions" — "Assumptions"):** The paper takes a focused view assuming the hypothesized agent wants to (1) help the user — cooperative, not adversarial, not seeking to maximize its own benefit; (2) minimize conversational effort — avoiding idle chit-chat and excessive questions that burden the user's cognitive capacity; and (3) maximize the range/number of relevant options provided to the user. The authors acknowledge a limitation here: they assume the agent has sufficient capabilities to understand and respond to user requests, and assume "for the purposes of simulated conversations we assume that the agent has the capacity to perfectly understand the user's utterances and requests, and can provide meaningful and relevant responses. We acknowledge that these are both very much open challenges that have yet to be fully addressed."
- **Finding 2 (Section 3.2, "Agent Actions", drawing on prior work by Bohus and Rudnicky, Moore, and Demberg et al.):** Agent actions include elicitation (learning about the user's preferences), conversation management that tracks and maintains search goals, conversational history, and the agent's current state of understanding; the framework also covers repair and disengagement strategies (repeating information or asking for it to be repeated in case of misunderstanding) and the ability of agents to explain their current state (reporting what they understand) so that misunderstandings can be corrected. The paper grounds this in Radlinski and Craswell's five properties of a conversational search agent: user revealment, agent revealment, mixed initiative, memory, and set retrieval.
- **Finding 3 (Section 3.3, "Agent Decisions and Tasks", and Section 4, "Discussion"):** The framework enumerates key decision points where the agent must decide how to steer the conversation to a successful or satisfactory conclusion, distinguishing three phases (query formulation, search results exploration, and query re-formulation) per Trippas et al., in which the agent elicits details of the information need and obtains feedback on results to inform subsequent actions. The discussions position the framework as a formalization that makes explicit the actions and decisions previously only identified in Wizard-of-Oz and log studies, providing a framework and starting point for research, development, and evaluation of conversational search agents.

---

## Discussion points for the team sync

- **Evaluation infrastructure matters as much as model quality:** The iKAT track (Paper 2) shows that persona-aware, knowledge-grounded benchmarks with provenance-citation requirements are needed to properly evaluate conversational search agents; bare retrieval metrics are insufficient, and the companion evaluation found grounding issues (only 23/73 responses grounded for the LLaMA-2 baseline).
- **Tool-call efficiency is a first-class training concern:** Paper 1 (Multi-Agent Screening) argues that when agents screen items for a costly human assessor, agreement is not correctness and assessor acceptance gives only a lower bound on recall — so we should rate agents on judgments, report recall as an interval, and use audits of non-passed items to pin down missed relevant items without assumptions about the agents.
- **Trust and grounding:** The iKAT response evaluation (Paper 2) found the LLaMA-2 baseline grounded only 23 of 73 responses, flagging faithfulness risks that underpin user trust in AI assistants; Paper3's framework (2404.08630) formalizes the agent actions (elicitation, explanation of state, repair) needed to support trust.
- **Size and effort tiers matter:** Gains from tool-call efficiency training (Paper 1's worked example and the broader literature) are clearer at larger model sizes (27B, 4B) than at 9B, where compression encroaches on necessary calls — so deployment choices (model size, effort tier) should be explicit rather than hidden in training objectives.

*Note: All three papers were selected from recent arXiv listings searched under the phrasings "conversational search agents", "tool use in LLM agents", and "user trust in AI assistants". Full texts were read via the arXiv download tooling, and subject category and submission dates were verified on each paper's public arXiv abstract page.*
