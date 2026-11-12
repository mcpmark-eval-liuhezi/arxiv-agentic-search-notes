# Reading Notes: Agentic Search and Tool Use

## Introduction

This brief covers recent arXiv literature on **agentic search** — assistants where an LLM-driven agent searches, calls tools, and hands answers to users. It was prepared for Thursday's team sync. We searched arXiv with phrasings such as "conversational search agents", "tool use in LLM agents", and "user trust in AI assistants", and selected the two or three most relevant hits. For each paper we read beyond the abstract (full text downloaded and reviewed) and extracted at least two concrete findings, each tagged with the section it came from. We also verified each paper's public arXiv abstract page for its primary subject category and submission date.

## Papers Covered

1. **arXiv:2610.41738** — Search When Needed, Proceed When Reliable: Metacognitive Reliability Supervision for Efficient Agentic Search (Sun, Wu, Mao — Oct 2026)
2. **arXiv:2610.37080** — Tessera: Structure-Completed Evidence Units for Web Retrieval-Augmented Generation (Hua, Sun, Zhuang, Qi, Qiu, Liu, Xiao — Oct 2026)
3. **arXiv:2401.01330** — TREC iKAT 2023: The Interactive Knowledge Assistance Track Overview (Aliannejadi, Abbasiantaeb, Chatterjee, Dalton, Azzopardi — Jan 2024)

---

## Paper 1: arXiv:2610.41738

**Title:** Search When Needed, Proceed When Reliable: Metacognitive Reliability Supervision for Efficient Agentic Search
**Authors:** Minhao Sun, Zhijing Wu, Xian-Ling Mao
**Subject category (from arXiv abs page):** Information Retrieval (cs.IR)
**Submission date (from arXiv abs page):** v1, Fri, 30 Oct 2026 13:44:30 UTC
**arXiv ID:** 2610.41738 (v1)

### Abstract Summary

This paper tackles the core decision in efficient agentic search: at each intermediate state, should the LLM search for external evidence or proceed using its current parametric knowledge? The authors argue that supervising search decisions with final-answer correctness or trajectory-level feedback is ambiguous, because a correct outcome can arise from either reliable knowledge or an unsupported lucky guess, and informative retrieval can still yield an incorrect final answer. They propose a metacognitive framework that scores each local conclusion by current-state reliability (can we proceed without search?) and by search-action reliability (how much does retrieved evidence improve reliability?), combining these into a process reward for GRPO. Experiments across seven question-answering benchmarks show a better accuracy-efficiency trade-off than strong agentic-search baselines.

### Extracted Findings (with source sections)

- **Finding 1 (Evaluation result, Section 1 Introduction):** Their 7B model achieves the highest average CEM across seven QA benchmarks (69.3), exceeding HiPRAG (the strongest baseline) by 2.1 points and improving on it across all seven datasets (as reported in Table 1). In controlled experiments, the policy reallocates search across task types: relative to a HiPRAG reimplementation, mean search calls decrease by approximately 20.9% on single-hop QA and increase by 6.6% on multi-hop QA, with 6.8% fewer searches overall (Table 5).
- **Finding 2 (Design decision, Section 4 Methodology):** They define two scores: the Current-State Reliability Score, which evaluates whether the model can proceed without search by comparing its estimated reliability against a calibration reference, and the Search-Action Reliability Score, which measures the change in reliability between matched contexts with and without retrieved evidence. Averaging these action scores within each rollout yields a process reward that is added to answer and format rewards in GRPO, encouraging the policy to proceed when current information is reliable and to favor searches that improve reliability.
- **Finding 3 (Limitation acknowledged, Section 1 Introduction):** The authors explicitly note the performance-reliability mismatch: overestimating current reliability leads to under-search, while underestimating it leads to over-search. They distinguish whether a local conclusion is correct from how reliably it can be reached with available information, and how much retrieval improved that reliability — correctness alone is an ambiguous signal for search supervision.
- **Finding 4 (Empirical basis, Section 3.2 Empirical Uncertainty-Accuracy Relationship):** For Qwen2.5-7B, local accuracy decreases with mean conclusion uncertainty across 50,000 states (32 conclusions sampled per search state without current retrieval evidence), providing an empirical basis for estimating the reliability of local conclusions via sequence-level uncertainty.

## Paper 2: arXiv:2610.37080

**Title:** Tessera: Structure-Completed Evidence Units for Web Retrieval-Augmented Generation
**Authors:** Peichun Hua, He Sun, Zirui Zhuang, Qi Qi, Yiming Qiu, Shinan Liu, Yunming Xiao
**Subject category (from arXiv abs page):** Information Retrieval (cs.IR)
**Submission date (from arXiv abs page):** v1, Tue, 27 Oct 2026 11:41:33 UTC
**arXiv ID:** 2610.37080 (v1)

### Abstract Summary

Web RAG traditionally splits a page into fixed-size text chunks (or screenshot tiles) that serve simultaneously as units of indexing, ranking, and reading — and this coupling loses evidence, especially for structured content like tables and infoboxes. The authors trace answer evidence from source HTML to reader input over full English Wikipedia and find that stronger extraction, context expansion, and larger reader budgets do not naturally recover the loss. They introduce Tessera, a training-free evidence delivery layer that rebuilds ranked pages into fine-grained evidence units (completed with page title, section path, and table headers), selects query-relevant units, and packs them into the reader context at an unchanged token budget — leaving the index, retriever, and reader unchanged. Across three QA benchmarks, Tessera improves answer accuracy by 5-6 points on text and screenshot pipelines and carries gains over to multi-step agentic search.

### Extracted Findings (with source sections)

- **Finding 1 (Evaluation result, Section 2.1 "The Modality Gap Is a Ranking Gap"):** Once both text and screenshot pipelines rank their candidates well, the two modalities converge — with reranking the difference is -0.4 points on NQ, +1.4 on NQ-Tables, and +2.6 on SimpleQA. A text pipeline with the authors' own hybrid index (fusing dense and BM25 candidates) is the most accurate on every dataset, and both modalities share a ceiling (best pipeline retrieves the gold article in top 3 for 61.8% of NQ-Tables questions and answers 48.4% of them).
- **Finding 2 (Evaluation result, Section 2.2 "Tracing Evidence from Source to Reader"):** For 9.7% of questions, a single stored chunk contains the complete evidence but is missing from the top three after ranking; placing that chunk in the third slot lifts accuracy on these questions from 49.1% to 78.5%. For 7.6% of source-supported questions, the evidence never enters the index intact, and 81% of these losses are table cells, table headers, or captions.
- **Finding 3 (Design decision, Section 1 Introduction):** Tessera is a training-free, query-time layer that treats the ranked list as a pointer to anchor pages, parses their HTML into small evidence units (table rows, list items, paragraphs), completes each unit with structure (page title, section path, column headers via merged cells), and packs them into the reader's token budget. It plugs into text, screenshot, and agentic pipelines without retraining, and 3K tokens with Tessera beat 12K tokens of chunks or tiles (Section 4).
- **Finding 4 (Limitation acknowledged, Section 1 Introduction):** The pipeline uses a single object (chunk or tile) as its unit of indexing, ranking, and reading — "it finds the right page and delivers the wrong part." Stronger extraction, context expansion, structure-aware reranking, and larger budgets leave the evidence loss in place, which is why Tessera instead decouples delivery from retrieval.

## Paper 3: arXiv:2401.01330

**Title:** TREC iKAT 2023: The Interactive Knowledge Assistance Track Overview
**Authors:** Mohammad Aliannejadi, Zahra Abbasiantaeb, Shubham Chatterjee, Jeffery Dalton, Leif Azzopardi
**Subject category (from arXiv abs page):** Information Retrieval (cs.IR); Artificial Intelligence (cs.AI); Computation and Language (cs.CL)
**Submission date (from arXiv abs page):** v1, Tue, 2 Jan 2024 18:40:03 UTC (revised v2, Thu, 22 Feb 2024)
**arXiv ID:** 2401.01330 (v1/v2)

### Abstract Summary

This is an overview of the TREC Interactive Knowledge Assistance Track (iKAT) 2023, which builds on the earlier TREC Conversational Assistance Track (CAsT). iKAT emphasizes creating and researching conversational search agents that adapt responses based on user prior interactions and present context, so the same question can yield varied answers contingent on the user's profile and preferences. The track attracted seven teams and 24 runs, most of which leveraged LLMs (with a few using a generate-then-retrieve approach). The track defines passage retrieval, statement ranking, and response generation tasks over a Personal Text Knowledge Base (PTKB), using a ClueWeb22-B subset of ~116.8M passages.

### Extracted Findings (with source sections)

- **Finding 1 (Context and scale, Section 1 Introduction):** iKAT distinctively emphasizes conversational search agents that adapt responses to user prior interactions and present context, so the same question yields varied answers based on user profile and preferences (illustrated with personas Alice (vegan) and Bob (diabetes, nut allergy, lactose intolerant)). The track's first year attracted seven teams and 24 runs, most leveraging LLMs, with a few focusing on generate-then-retrieve approaches.
- **Finding 2 (Design decision, Section 2 "Track, Tasks, Data, and Resources"):** The track includes three tasks — Statement Ranking (ranking relevant PTKB statements for the current utterance), Passage Ranking (retrieving and ranking relevant passages given conversation context and PTKB), and Response Generation (fluent, informative responses that cite at least one provenance passage). They used a 116,838,987-passage subset of ClueWeb22-B, segmenting documents with a sliding window (10 consecutive sentences per passage, 5-sentence stride), and provided four baselines including a LLaMA-2 7B pipeline with task-specific prompts.
- **Finding 3 (Limitation acknowledged, Section 1 Introduction):** Every response should cite at least one provenance passage, maintaining a focus on passage/provenance ranking; the track also emphasizes decisional search tasks where users sift through data to weigh options. The personal information provided (PTKB) is assumed to be high-level narrative sentences collected from prior conversations, with care taken to exclude personally identifiable information to ensure contributor privacy.

### How to Read These Notes

- Paper 1 (2610.41738) is the most directly on-topic for our sync: it is about training LLM agents to decide when to search vs. proceed, with concrete benchmark results and a GRPO process-reward design.
- Paper 2 (2610.37080) is highly relevant to the retrieval quality side of agentic search: it shows that chunk/tile-based retrieval loses structured evidence and that a lightweight, training-free delivery layer (Tessera) recovers 5-6 points of accuracy.
- Paper 3 (2401.01330) provides the evaluation-background perspective: a shared TREC-style benchmark (iKAT) with LLM-era baselines for conversational search agents, useful for comparing assistant designs.
