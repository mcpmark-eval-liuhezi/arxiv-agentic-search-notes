# Reading Notes: Agentic Search and LLM Tool Use

**Prepared for:** Team sync on agentic search (Thursday)
**Scope:** This brief covers recent arXiv research on agentic search — assistants where an LLM-driven agent searches, calls tools, and hands answers to users. It includes papers on conversational search agents, tool use in LLM agents, and evaluation methodology for AI assistants.

Three papers are covered below, selected from recent arXiv searches on "conversational search agents," "tool use in LLM agents," and "user trust in AI assistants." For each paper we record the arXiv ID, title, full author list, primary subject category and submission date (verified on the paper's public arXiv abs page), a 2–3 sentence abstract summary, and at least two concrete findings extracted from the full text with their source sections.

---

## Paper 1: ReTrace: Distilling Tool-Using Agents into Planner-Free Workflows

- **arXiv ID:** 2611.14492 (v1)
- **Title:** ReTrace: Distilling Tool-Using Agents into Planner-Free Workflows
- **Authors:** Kaiyuan Hou, Yang Ping Kuo, Indranil Bhattacharya, Rohith Nama, Arushi Gupta, Carlos D. Castillo, Prathap Ramachandra, Xiaofan Jiang, Francesco Carbone
- **Primary subject category (from arXiv abs page):** cs.AI (Artificial Intelligence); also cs.LG (Machine Learning) and cs.MA (Multiagent Systems). Cite as: arXiv:2611.14492 [cs.AI]
- **Submission date (from arXiv abs page):** [v1] Tue, 10 Nov 2026 18:49:47 UTC
- **Source:** https://arxiv.org/abs/2611.14492

### Abstract Summary

ReTrace addresses a cost problem in LLM-agent-driven business automation: agents that call an LLM to choose every step of every run are slow and expensive at scale. The paper presents a method that turns a natural-language Standard Operating Procedure (SOP) plus a few logged agent runs into a reusable workflow where code — not an LLM — selects the next step. Extracted conditions and input sources from the agent's reasoning are merged into a compact rule document that compiles into executable workflows, with only the final stage depending on the platform so rule documents can be reused across orchestration tools.

### Extracted Findings (from full text)

**Finding 1 — Production evaluation results (Section 1, Introduction):** On 79 procedures from the authors' production platform, ReTrace workflows reach **93.8% completion versus 96.8% for the agent, at 24% lower cost per task**. On τ²-bench, ReTrace workflows reach **75.8% task success (agent: 80.6%) and run in 1.3s instead of 170s**. Three rounds of repair on training failures lift k=2 workflows from 66.7% to 81.2%. On eight SOP-Bench procedures, unrepaired ReTrace trails few-shot generation (80.6% vs. 83.6%), but repair from labeled training failures lifts it to 87.8% at 0.40s per task, compared to 85.5% in 46.6s for the agent.

**Finding 2 — Design decision and cost analysis (Section 1, Introduction):** The authors report that of 5,115 automations customers created on their enterprise BPA platform between April and June 2026, **91.8% were agents that plan at run time, yet seven of the ten most frequently executed automations were fixed workflows**. For agents using three to five tools, choosing the next step took **37–43% of execution time on top of token cost**, and they estimate construction is recovered after about 75 executions (with the seven workflows having run 297k–528k times in three months). Removing the agent's reasoning costs **8.8 and 6.2 points** on τ²-bench and SOP-Bench respectively, and gains only 1.7 points on SOP-Bench — i.e., relying on raw logged traces alone is not enough, and with many long traces the workflow generator eventually writes worse code (Section 5.1).

**Finding 3 — Platform portability (Section 1, Contributions):** The paper's key design contribution is that **only the final stage depends on the target platform**, so the same pipeline yields Python code for public benchmarks and typed workflow graphs for the internal enterprise platform, allowing rule documents to be compiled for different orchestration platforms.

---

## Paper 2: TREC iKAT 2023: A Test Collection for Evaluating Conversational and Interactive Knowledge Assistants

- **arXiv ID:** 2405.02637 (v1)
- **Title:** TREC iKAT 2023: A Test Collection for Evaluating Conversational and Interactive Knowledge Assistants
- **Authors:** Mohammad Aliannejadi, Zahra Abbasiantaeb, Shubham Chatterjee, Jeffery Dalton, Leif Azzopardi
- **Primary subject category (from arXiv abs page):** cs.IR (Information Retrieval); also cs.AI (Artificial Intelligence) and cs.CL (Computation and Language). Cite as: arXiv:2405.02637 [cs.IR]
- **Submission date (from arXiv abs page):** [v1] Sat, 4 May 2024 11:22:16 UTC
- **Source:** https://arxiv.org/abs/2405.02637
- **Comments on abs page:** "To appear in SIGIR 2024. arXiv admin note: substantial text overlap with arXiv:2401.01330"

### Abstract Summary

This paper extend the TREC Interactive Knowledge Assistance Track (iKAT) test collection to enable researchers to test and evaluate Conversational Search Agents (CSA). The collection provides 36 personalized dialogues over 20 different topics, each coupled with a Personal Text Knowledge Base (PTKB) defining bespoke user personas, totaling 344 turns and approximately 26,000 passages assessed on relevance plus four response dimensions: relevance, completeness, groundedness, and naturalness. It challenges CSA to navigate diverse personal contexts, elicit pertinent persona information, and employ context for relevant conversations, with the PTKB integration and emphasis on decisional search tasks making it an essential benchmark for conversational and interactive knowledge assistants.

### Extracted Findings (from full text)

**Finding 1 — Benchmark structure and scale (Section 3, The iKAT Resources):** The extended iKAT collection contains **36 personalized conversational dialogues (11 for training and 25 for testing) over 20 different topics, with up to three personas per topic**; each dialogue averages 13 turns (427 turns total), and the underlying passage collection is a subset of ClueWeb22-B with 116,838,987 passages segmented into passages using a sliding-window approach (10 consecutive sentences per passage, 5-sentence stride). For the retrieval assessment, **NIST assessors manually judged a pool of 26,159 passages (a subset of 176 turns out of 326), with an average of 147 passages judged per turn**; response quality is assessed on four dimensions: relevance, completeness, groundedness, and naturalness.

**Finding 2 — Evaluation challenge and Limitations acknowledged (Section 1, Introduction, and Section 2, Related Work):** The paper explicitly notes that **"resources to evaluate CSA are currently limited"** and that conversational information-seeking evaluation faces four persistent challenges: conversations are context-dependent (past turns matter), personal (user preferences matter), evolvable in form (conversational flow matters), and mixed-initiative (user-agent interplay matters). The authors position iKAT as evolving the TREC CAsT track to support multi-path, multi-turn, and multi-perspective conversations where direction depends not only on prior responses but also on the user's persona, and they highlight the introduction of a personal knowledge base and complex, knowledge-intensive dialogues as novel challenges.

---

## Paper 3: Multi-Agent Screening: What Agreement and Acceptance Cannot Tell You

- **arXiv ID:** 2611.12705 (v1)
- **Title:** Multi-Agent Screening: What Agreement and Acceptance Cannot Tell You
- **Authors:** Benjamin Chang, Dell Zhang
- **Primary subject category (from arXiv abs page):** cs.MA (Multiagent Systems); also cs.AI (Artificial Intelligence) and cs.IR (Information Retrieval). Cite as: arXiv:2611.12705 [cs.MA]
- **Submission date (from arXiv abs page):** [v1] Mon, 9 Nov 2026 17:00:16 UTC
- **Source:** https://arxiv.org/abs/2611.12705

### Abstract Summary

This paper studies multi-agent screening systems — where AI agents screen items (such as documents, code changes, or papers) for a costly human assessor, as in agentic search, e-discovery, and AI reviewing of papers — and shows what each kind of evidence can establish about agent errors even with unlimited data. The central arguments are that agreement shows agents concur but not that they are right, acceptance by the assessor gives precision but only a lower bound on recall, and知道 which agents found which relevant items gives an upper bound on recall when agents' finds are not negatively correlated. The authors provide a recall interval, bounds on what the next agent can add, and rules for spending on agents and audits, with AI reviewing of papers as a worked example.

### Extracted Findings (from full text)

**Finding 1 — Core theoretical results (Section 3, What Each Kind of Evidence Can Tell Us):** The paper proves that **agreement (Theorem 3.1) shows agents concur, not that they are correct**: for every k and every rule, the agent's error is at least E[min{η(Z), 1−η(Z)}], and the distribution of agent outputs does not depend on the true acceptability — no statistic of the outputs can distinguish a right consensus from a wrong one. **Acceptance (Proposition 1) gives precision, not recall**: the tight interval for recall is [ap/(ap+1−a), 1], since recall r = ap/(ap+H) decreases in H and the data allow any H ∈ [0, 1−a]. **Overlap (Theorem 3.2)** shows that the Lincoln–Petersen capture-recapture quantity h₁₀h₀₁/h₁₁ gives a lower bound H ≥ L on recall, and an interchangeable next agent (Proposition 2) adds at most f₁/k of the collection's relevant items.

**Finding 2 — Worked example and practical guidance (Section 4, Worked Example: AI Reviewers and Human Experts, and Section 5, Take-Home Messages):** In the worked example with three AI reviewers nominateing 200 of 1000 submissions (a=0.2) and experts accepting 120 (p=0.6), acceptance alone gives r ∈ [13%, 100%], and with reviewers 1 and 2 the counts (30, 30, and 6 for pairs) imply at least 30×30/6 = 150 acceptable papers were missed under positive dependence, pinning recall at r ∈ [13%, 44%]. A 4th interchangeable reviewer adds at most 90/3 = 30 acceptable papers, and an audit of 192 of the other 800 submissions estimates H to ±0.05 with 95% confidence, finding at least 192×150/800 = 36 missed papers in expectation. The Take-Home Messages section concludes: rate agents on judgments, not agreement (which can reward shared bias); report precision and recall as an interval; let agents nominate separately and log who found each item; and compare the next agent with an audit before buying either, budgeting audits by accuracy wanted rather than the number of agents.

---

## Summary Table

| arXiv ID | Title | Primary Category | Submitted | Key Takeaway |
|---|---|---|---|---|
| 2611.14492 | ReTrace: Distilling Tool-Using Agents into Planner-Free Workflows | cs.AI | 10 Nov 2026 | Distilling agent reasoning into planner-free code workflows cuts per-task cost 24% and τ²-bench time from 170s to 1.3s, with repair closing the gap to agent performance. |
| 2405.02637 | TREC iKAT 2023: A Test Collection for Evaluating Conversational and Interactive Knowledge Assistants | cs.IR | 4 May 2024 | A benchmark of 36 personalized dialogues, 20 topics, PTKB-driven personas, and 4 response dimensions (relevance, completeness, groundedness, naturalness) for evaluating conversational search agents. |
| 2611.12705 | Multi-Agent Screening: What Agreement and Acceptance Cannot Tell You | cs.MA | 9 Nov 2026 | In multi-agent screening with costly assessors, agreement proves nothing about correctness and acceptance only gives a recall lower bound; audits on unjudged items are needed to estimate missed relevant items. |

## Notes on Methodology

- Papers were selected from arXiv searches using phrasings including "conversational search agents," "tool use in LLM agents," and "user trust in AI assistants."
- Full text was read via arXiv's HTML endpoint for each paper (downloaded and parsed locally).
- Primary subject category and submission date were verified on each paper's public arXiv abstract page (https://arxiv.org/abs/<ID>); all three pages loaded successfully.
- At least two concrete findings per paper were extracted from the full text, each tagged with the section it came from (Introduction, Method, Related Work, or dedicated results/discussion sections).
