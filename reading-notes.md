# Reading Notes: Agentic Search and LLM Tool Use

**Prepared for:** Thursday team sync — agentic search week
**Scope:** A short literature brief on assistants where an LLM-driven agent searches, calls tools (retrieval, generation, API-style components), and hands answers to users. Three papers are covered: two TREC iKAT 2023 tracks on Conversational Search Agents (CSAs) and one full-system study of an open-domain conversational search agent (Ericson).

## Papers covered

1. **arXiv:2405.02637** — TREC iKAT 2023: A Test Collection for Evaluating Conversational and Interactive Knowledge Assistants (cs.IR, submitted 4 May 2024)
2. **arXiv:2401.01330** — TREC iKAT 2023: The Interactive Knowledge Assistance Track Overview (cs.IR, submitted 2 Jan 2024, v2 22 Feb 2024)
3. **arXiv:2304.02233** — Ericson: An Interactive Open-Domain Conversational Search Agent (cs.CL, submitted 5 Apr 2023)

---

## Paper 1: arXiv:2405.02637 — TREC iKAT 2023: A Test Collection for Evaluating Conversational and Interactive Knowledge Assistants

**Authors:** Mohammad Aliannejadi, Zahra Abbasiantaeb, Shubham Chatterjee, Jeffery Dalton, Leif Azzopardi
**Subject category (from arXiv abs page):** Information Retrieval (cs.IR); also cs.AI, cs.CL — primary category cs.IR
**Submission date (from arXiv abs page):** v1 submitted Sat, 4 May 2024 11:22:16 UTC
**Note:** arXiv admin note: substantial text overlap with arXiv:2401.01330; annotation: to appear in SIGIR 2024.

**Abstract summary (2-3 sentences):** This paper describes the extended TREC iKAT 2023 test collection for evaluating Conversational Search Agents (CSAs) built on LLMs. It provides 36 personalized dialogues over 20 topics, each coupled with a Personal Text Knowledge Base (PTKB) defining user personas, totaling 344 turns and ~26,000 passages for relevance assessment plus response-quality assessment on four dimensions: relevance, completeness, groundedness, and naturalness. The collection emphasizes decisional search tasks (weighing options to reach a conclusion) and requires every response to cite at least one "provenance" passage, making it a reusable benchmark for the community.

**Extracted findings:**

- **(Section 5.1.1, "Results & Analysis" → "Passage Ranking")** G→R→G pipelines (LLM-aware: GPT-4 and Llama) tend to perform better than R→G pipelines on passage ranking, suggesting that leveraging LLMs' learned knowledge gives a better starting point for subsequent retrieval and response generation. *Evaluation result:* iKat's first year attracted seven teams and 24 runs, most leveraging LLMs with only a few using generate-then-retrieve.
- **(Section 5.3.1, "Results & Analysis" → "Response Evaluation" → "Human evaluation")** run-4-GPT-4 significantly outperformed all other methods in both human judged relevance (2.42) and completeness (2.54), mirroring its passage-ranking success; the organizers' baseline llama2_only_10_docs ranked third on human Relevance/Completeness, an unexpected ranking that did not align with the passage-ranking results (Table 4). *Evaluation result:* only 73 turns were assessed by NIST within budget constraints, limited to relevance and completeness labels.
- **(Section 5.3.2, "Results & Analysis" → "Response Evaluation" → "GPT-4 evaluation")** run-4-GPT-4 also topped the GPT-4-as-external-evaluator on relevance (2.92), completeness (2.85), naturalness (4.00) and groundedness (0.89, i.e. 65/73 responses deemed grounded) — while the baseline llama2_only_10_docs scored only 23/73 grounded, indicating it may rely on internal knowledge rather than retrieved passages, likely inflating raw scores. *Design decision / limitation:* the authors flag that GPT-4-as-evaluator may favor GPT-4-generated text (self-preference), so results should be corroborated with independent human evaluation.
- **(Section 5.1.2, "Passage Ranking" → "Performance per dialogue")** performance is topic-dependent: topics 12-1 and 21-1 are easiest while 19-1 and 20-2 are the most difficult; performance is highest in the first turn and degrades as the conversation progresses (with some mid-conversation peaks), and PTKB dependence is most costly in early turns. *Limitation acknowledged:* no clear correlation was found between PTKB statement ranking performance and passage retrieval performance, and there is no clear correlation between PTKB Statement Ranking and passage retrieval performance across runs.

**Two-sentence takeaway:** The iKAT test collection is now the de-facto benchmark for evaluating LLM-based conversational search agents, with the recipe that LLM priors (G→R→G) help generation but PTKB statement ranking remains harder than passage ranking. The top performer was a GPT-4-based pipeline (run-4-GPT-4, 65/73 grounded responses), and the authors correctly caution that LLM-as-judge metrics can bias toward GPT-4 outputs, so human assessment is still needed for independent evaluation.

---

## Paper 2: arXiv:2401.01330 — TREC iKAT 2023: The Interactive Knowledge Assistance Track Overview

**Authors:** Mohammad Aliannejadi, Zahra Abbasiantaeb, Shubham Chatterjee, Jeffery Dalton, Leif Azzopardi
**Subject category (from arXiv abs page):** Information Retrieval (cs.IR); also cs.AI, cs.CL — primary category cs.IR
**Submission date (from arXiv abs page):** v1 submitted Tue, 2 Jan 2024 18:40:03 UTC; this version v2 revised Thu, 22 Feb 2024 17:29:03 UTC.

**Abstract summary (2-3 sentences):** This is the overview paper for TREC's Interactive Knowledge Assistance Track (iKAT), which evolved from the TREC Conversational Assistance Track (CAsT) to support multi-path, multi-turn, multi-perspective conversations where direction depends not only on prior system responses but also on the user's persona. The track focuses on personalized conversational search agents that use a Personal Text Knowledge Base (PTKB) and provenance-cited responses, with iKAT's debut year attracting seven teams and 24 runs, most using LLMs. It lays out the track's tasks (Statement Ranking, Passage Ranking, Response Generation), the ClueWeb22-B collection subset (116,838,987 passages segmented with a 10-sentence sliding window and a pyserini index), and four baselines (BM25+RM3 with manual/automatic PTKB statement selection, and a LLaMA-2 7B pipeline).

**Extracted findings:**

- **(Section 3.3, "Collection")** The iKAT corpus is built from a manually curated subset of ClueWeb22-B (116,838,987 passages), segmented with the same methodology as the TREC Deep Learning track (documents shortened to 10,000 chars, 10-sentence sliding windows with a 5-sentence stride), distributed by CMU with Python segmentation scripts, MD5 hashes, and a pyserini index; PII was deliberately excluded from PTKB statements. *Design decision:* only high-level personal information (no PII) is stored in the PTKB to ensure contributor privacy.
- **(Section 6.1.1, "Results" → "Passage Ranking" → "Overall results", Tables 5–6)** On the PTKB provenance task (nDCG@3, NIST and organizers assessments), the best run was IRLab-Amsterdam run-1-llama-zero-shot (0.7254 / 0.6394 nDCG@3), while G→R→G models (e.g. run-4-GPT-4 at 0.6174 / 0.6288) were not the top runs despite their passage-ranking success — LLMs are good at利用 PTKB for generation, but less effective at ranking relevant PTKB statements. *Evaluation result:* topic 20-2 was the most challenging PTKB task; performance peaks at turn 15, showing weaker dependence on conversation depth than the passage-ranking task.
- **(Section 6.3, "Results" → "Response Evaluation")** The top submission from RALI (ConvGQR) did not include a generated response; among assessed turns, run-4-GPT-4 again led on human relevance/completeness, and the paper warns: "these results are likely to be somewhat biased towards GPT-4-based submissions. So we warn participants (and readers) not to take these results at face value, as (human) assessments are required to provide an independent evaluation." *Limitation acknowledged:* with only three manual runs and 73 assessed turns, the human annotation budget was constrained and limited to relevance and completeness.
- **(Section 1, "Introduction")** The track's four research questions are: (1) Personal Contexts — how efficiently can an agent navigate personal contexts; (2) Personalization — can agents modify conversational feedback based on user knowledge; (3) Elicitation — are agents proficient at drawing out pertinent persona information; (4) Dependent Relevance — can agents employ context and prior responses. *Design decision:* every response must cite at least one "provenance" passage, preserving a passage/provenance-ranking focus similar to TREC CAsT, and all prior conversation turns are usable as context (parents in the conversational topic tree).

**Two-sentence takeaway:** iKAT operationalizes what "agentic search with personalization" means seasonally: persona-conditioned queries, PTKB-linked responses with provenance citations, and evaluation across statement ranking, passage ranking, and multi-dimensional response quality. The deployable lesson is that LLM priors help generation but not PTKB statement ranking, and the authors' own caveat stands: LLM-as-judge numbers here should be read alongside independent human assessment.

---

## Paper 3: arXiv:2304.02233 — Ericson: An Interactive Open-Domain Conversational Search Agent

**Authors:** Zihao Wang, Ali Ahmadvand, Jason Choi, Payam Karisani, Eugene Agichtein
**Subject category (from arXiv abs page):** Computation and Language (cs.CL); also cs.AI — primary category cs.CL. MSC classes: 68T50, 68T07.
**Submission date (from arXiv abs page):** v1 submitted Wed, 5 Apr 2023 05:28:31 UTC.

**Abstract summary (2-3 sentences):** Ericson is a fully functional open-domain conversational search agent (ODCS) combining state-of-the-art QA and information-retrieval components with intent inference and dialogue-management models for proactive question refinement and recommendations. It was stress-tested in the Amazon Alexa Prize by engaging in live conversations with thousands of real Alexa users, giving an empirical basis for analyzing the ODCS system in real settings. The interaction-data analysis found that accurate intent classification, encouraging user engagement, and careful proactive recommendations contribute most to user satisfaction, and the study identifies limitations of existing search techniques as a building block for next-generation ODCS agents.

**Extracted findings:**

- **(Section 4.2, "System Description")** The Ericson system has five main parts: a Hierarchical Intent Classifier (top level picks the retrieval module by predicted topic; a second-level topic-specific classifier, enriched via the Google Knowledge Graph API — e.g. "Bruno Mars" is reclassified as "American singer-songwriter"), a Dialogue State Manager (DSM) that controls conversation flow and invokes components, information-retrieval components (general and domain-specific QA/search), a conversational transition component that generates topic recommendations, and a dialogue logger for later analysis; it was deployed as an Alexa skill on Amazon EC2 via the Lambda function. *Design decision:* a hierarchical (semantic + grammatical) intent classifier with entity enrichment via Google Knowledge Graph API to route utterances to the right retrieval module (e.g. small-talk vs. music-search).
- **(Section 6 "Results and Discussion" / Section 1, "Introduction")** The large-scale real-user deployment in Alexa Prize 2017 is reported as the first study of a fully functional open-domain conversational search system with real users: interaction data and explicit ratings/free-form feedback showed that accurate intent classification, encouraging user engagement, and careful proactive recommendations contribute most to user satisfaction. *Evaluation result:* RQ2 was answered via user ratings, conversation turn counts, engagement, and system proactivity metrics (Section 5.1); RQ1 via component-level design (Section 4).
- **(Section 7, "Limitations")** The main limitation was that the study ran in a real-time setting, so some component changes may have affected other components; and because the user-study timeframe was limited, component changes to classifiers, engagement, and proactivity were completed within a short period, making it hard to exclusively attribute ratings to individual updates. *Limitation acknowledged:* despite this, the effects of major improvements to the intent classifier, engagement, and proactive topic recommendation were significant enough to be credited for improvements in overall user experience.
- **(Section 4.3, "Hierarchical Intent Classifier")** The top-level classifier routes utterances to general or specific retrieval modules by predicted topic and conversation context, and a second-level entity-specific classifier (using Google Knowledge Graph API) further refines intent (e.g. "let's talk about Bruno Mars" is initially "Music" but reclassified as a music-related request after entity detection). *Design decision:* frictionless entity disambiguation is what makes proactive refinement usable in the wild.

**Two-sentence takeaway:** Ericson is the prove-out study for agentic search: a modular architecture (intent classifier → DSM → IR components → transition component → logger) deployed live to thousands of real users, with the empirical finding that intent accuracy, engagement design, and calibrated proactivity drive satisfaction more than raw retrieval quality. Its acknowledged limitation is that real-time deployment makes causal attribution to individual component changes difficult — a good reminder for our own evaluations to isolate design choices.

---

## Quick reference table

| arXiv ID | Title | Primary category | Submitted | Key Section(s) for notes |
| --- | --- | --- | --- | --- |
| 2405.02637 | TREC iKAT 2023: A Test Collection for Evaluating Conversational and Interactive Knowledge Assistants | cs.IR | 4 May 2024 | §5 Results & Analysis (passage ranking, PTKB provenance, response evaluation) |
| 2401.01330 | TREC iKAT 2023: The Interactive Knowledge Assistance Track Overview | cs.IR | 2 Jan 2024 (v2 22 Feb 2024) | §3 Collection, §6 Results, §1 Introduction |
| 2304.02233 | Ericson: An Interactive Open-Domain Conversational Search Agent | cs.CL | 5 Apr 2023 | §4 System Description, §6 Results & Discussion, §7 Limitations |

## Suggested discussion prompts for Thursday's sync

1. Should we adopt TREC iKAT-style provenance-cited responses (citations required) as a guardrail for our assistant's answers?
2. Is the "G→R→G helps generation but not PTKB ranking" gap a reason to keep strong retrieval-first (R→G) pipelines alongside LLM reranking?
3. Ericson's finding that proactivity must be "careful" rather than aggressive: how do we tune our tool-calling agent's verbosity vs. user engagement?
4. Both iKAT papers warn that LLM-as-judge evals bias toward GPT-4 outputs — how do we validate our agent's groundedness independently?
