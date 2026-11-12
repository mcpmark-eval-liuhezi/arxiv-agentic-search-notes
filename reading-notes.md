# Reading Notes: Agentic Search and Tool Use

**Purpose:** Literature brief for Thursday's team sync on **agentic search** — assistants where an LLM-driven agent searches, calls tools, and hands answers to users. This file is committed at the repo root on branch `add-reading-notes` (targeting `main`, the default branch) for team comment and review before the sync.

**What this brief covers:** We searched arXiv with multiple phrasings — "conversational search agents", "tool use in LLM agents", "user trust in AI assistants", and "agentic search" — and selected the papers below as the most relevant recent hits. For each paper we did not stop at the abstract: we worked through the full text and extracted at least two concrete findings (an evaluation result, a design decision, or an acknowledged limitation), each tagged with the section it came from. We also verified each paper's public arXiv abstract page for its primary subject category and submission date; where a page would not load we fall back to the paper listing's report and note it here (all three abs pages loaded successfully).

---

## Paper 1 — arXiv:2401.01330 (v2)

**Title:** TREC iKAT 2023: The Interactive Knowledge Assistance Track Overview
**Authors:** Mohammad Aliannejadi, Zahra Abbasiantaeb, Shubham Chatterjee, Jeffery Dalton, Leif Azzopardi
**Subject category (from abs page):** Information Retrieval (cs.IR) — primary; also cs.AI, cs.CL. **Submitted:** 2 Jan 2024 (v1); last revised 22 Feb 2024 (v2). Abs page loaded: https://arxiv.org/abs/2401.01330

**Summary:** This is the overview paper for the TREC Interactive Knowledge Assistance (iKAT) track, which builds on the earlier Conversational Assistance Track (CAsT) but emphasize conversational search agents that adapt responses to a user's prior interactions and present context, using a Personal Text Knowledge Base (PTKB) of narrative sentences collected from past conversations. The track defines passage retrieval, statement ranking over the PTKB, and response generation as its core tasks, and reports on its first year, which attracted seven teams and 24 runs, most of which leveraged LLMs in their pipelines. The paper outlines the track design, baselines, and NIST/organizer assessments for the 2023 evaluation.

**Extracted findings (with source sections):**

1. **Evaluation result (§6.1 Passage Ranking / Results):** Generated-then-retrieve (G→R→G) runs tend to perform better than retrieve-then-generate (R→G) models, suggesting that leveraging the learned knowledge of LLMs (GPT-4 and Llama) gives a better starting point for subsequent retrieval and response generation. Among topics, 12-1 and 21-1 were the easiest while 19-1 and 20-2 were the most difficult, and performance was highest on the first turn, degrading as the conversation progressed, with the largest PTKB-dependency gap appearing in early turns — i.e., predicting the right PTKB statements early is essential.

2. **Design decision (§3.4 Baselines):** The organizers supplied four baselines, including `llama2_only_10_docs`, a pipeline that executes multiple interactions with a LLaMA-2 7B model, each employing distinct prompts tailored to specific tasks (conversation revision,Utterance reformulation, response summarization with a fine-tuned T5). The track's design keeps previous conversation turns as context (equivalent to parents in the conversational topic tree) and requires every response to cite at least one "provenance" passage, maintaining a focus on passage/provenance ranking.

3. **Limitation / scale note (§3.3 Collection):** The evaluation corpus is a manually inspected subset of ClueWeb22-B (116,838,987 passages) segmented with a 10-sentence sliding window (5-sentence stride) and shipped with a Pyserini index; topic quality was checked by at least two experts, and no PII was included to protect contributor privacy.

---

## Paper 2 — arXiv:2611.14492 (v1)

**Title:** ReTrace: Distilling Tool-Using Agents into Planner-Free Workflows
**Authors:** Kaiyuan Hou, Yang Ping Kuo, Indranil Bhattacharya, Rohith Nama, Arushi Gupta, Carlos D. Castillo, Prathap Ramachandra, Xiaofan Jiang, Francesco Carbone
**Subject category (from abs page):** Artificial Intelligence (cs.AI) — primary; also cs.LG, cs.MA. **Submitted:** 10 Nov 2026 (v1). Abs page loaded: https://arxiv.org/abs/2611.14492

**Summary:** ReTrace addresses the latency and cost problem of LLM agents that call an LLM to plan, route, and transform data at every step of every execution at scale. The authors turn a natural-language Standard Operating Procedure (SOP) plus a few logged agent runs into a reusable workflow in which code — not an LLM — selects the next step, extracting the condition, tool, and input sources behind each step into a compact, evidence-cited rule document. On 79 production procedures the workflows reach 93.8% completion vs. 96.8% for the agent at 24% lower cost per task; on τ²-bench they reach 75.8% task success (agent: 80.6%) in 1.3s instead of 170s, and repair from labeled training failures lifts k=2 workflows from 66.7% to 81.2%.

**Extracted findings (with source sections):**

1. **Evaluation result (§6.1 Results and Analysis — Task outcomes and repair):** On 79 production procedures, ReTrace workflows reach 93.8% completion against 96.8% for the agent at 24% lower cost per task; on τ²-bench (k=4, chosen by five-fold cross-validation on 74 training inputs, tested on 40 inputs) they reach 75.8% task success vs. 80.6% for the agent, running in 1.3s instead of 170s; three rounds of repair on training failures lift k=2 workflows from 66.7% to 81.2%. On eight SOP-Bench procedures (k=16, tested on 744 inputs), unrepaired ReTrace trails few-shot generation (80.6% vs. 83.6%), but repair from labeled training failures lifts it to 87.8% at 0.40s per task vs. 85.5% in 46.6s for the agent. Removing the agent's reasoning costs 8.8 and 6.2 points on τ²-bench and SOP-Bench respectively, and gains 1.7 on SOP-Bench.

2. **Design decision (§4.2 Building the workflow):** Construction has three stages — extraction (an LLM reads each step's reasoning `r_t` for the condition `g_t` under which the action applies and the intended action `α_t`, plus the tool record supplying tool `a_t`, typed inputs `v_t`, and source `ω_t` of each input, with case-specific values like customer IDs becoming variables), aggregation (an LLM summarizes each trace against the SOP, merges summaries into an Intermediate Representation rule document, and flags alternatives the agent never took as "unwitnessed"), and synthesis (an LLM converts the IR into Python code or typed workflow graphs, with a completion check `χ(h)` that stops execution when satisfied). LLMs are used only in this one-time construction; at run time the workflow `π(h)` stops if the completion check succeeds, otherwise runs the first rule whose guard holds — code, not an LLM, selects the next step (what the authors call "planner-free"). At k=16 on τ²-bench the IR input is 24.5k tokens vs. 101.4k with full traces.

3. **Acknowledged limitation (§9 Limitations):** ReTrace relies on the agent's reasoning, which may be wrong or may not reflect why the agent acted; construction checks confirm generated code loads and contains no planning calls and, where re-execution is available, count how many logged steps it reproduces, but they do not show that the recovered conditions are correct. The evaluation covers new inputs to the same procedures, not transfer to new procedures, and a change to an SOP or its tools may require rebuilding the workflow. Enterprise completion is reported by the platform, which can mark failed runs as complete; the public evaluation covers only the telecom domain of τ²-bench and eight SOP-Bench domains, and results on a single SOP-Bench procedure vary by as much as 50 points across trace samples.

---

## Paper 3 — arXiv:2610.15439 (v1)

**Title:** AuthScribe: Supporting Agency and Authorship in LLM-Assisted Reflective Journaling
**Authors:** Lanping Hu, Yinsi Zhou, Zhenchang Xing, Xiaoyu Sun, Shidong Pan
**Subject category (from abs page):** Human-Computer Interaction (cs.HC). **Submitted:** 11 Oct 2026 (v1). Abs page loaded: https://arxiv.org/abs/2610.15439v1

**Summary:** AuthScribe is an LLM-assisted reflective journaling system (implemented as an Obsidian plugin) that combines contextual anchors, writer-steered Reflect and Expand actions, Autobiographical Citations (exposing the prior self-authored material used to ground a prompt), and read-only Provisional Drafts that require re-expression before AI-generated suggestions enter the journal. Retrieval and LLM inference run on-device, and the paper reports a 14-day field deployment with 14 participants using the Sense of Agency Scale at baseline, Week 1, and Week 2, plus semi-structured exit interviews. The work bears directly on user trust in AI assistants: participants trusted personalized prompts when they could trace them to prior writing, and authorship was described as a process of transforming AI-generated suggestions into personally meaningful expression.

**Extracted findings (with source sections):**

1. **Evaluation result (§6.2 Findings — Perceived Agency Shifted After Participants Established a Practice of Use):** Broad perceived agency remained stable through Week 1 and shifted during Week 2, after participants had established repeated patterns of steering, rewriting, and grounding in prior self-authored material. In the 14-day deployment, grounding shifted from situating current experiences to reconnecting them with prior self-authored interpretations, and participants described authorship through transforming AI-generated suggestions into personally meaningful expression before placing them in the journal.

2. **Design decision (§4.4 Provisional Drafts and Adoption Friction):** The system uses read-only Provisional Drafts that keep generated continuations visually separate from editable journal text and ask writers to re-express ideas they choose to carry forward, creating what the authors call "adoption friction" at the uptake boundary; Autobiographical Citations make personalized assistance more trustworthy when participants could trace a current prompt to recognizable prior reasoning, and the design makes AI contributions inspectable and actionable so interaction boundaries shape authorship, agency, and trust.

3. **Acknowledged limitation (§8 Limitations):** The study examined AuthScribe with 14 university students over two weeks; the cohort captures reflective journaling during academic and career transitions, and broader populations and longer deployments are needed to extend findings across different journaling practices, privacy expectations, and levels of technical familiarity. Exit interviews occurred at T2, so qualitative accounts of change across the two weeks reflect retrospective recall, and the Sense of Agency Scale provides a broad longitudinal measure while interview accounts are the primary evidence for authorship and ownership; the journal corpus remained on participants' devices, so semantic analysis drew on concrete examples participants recounted during interviews.

---

## Cross-Paper Takeaways

- **Evaluation matters, butChoose the right benchmark.** iKAT shows G→R→G LLM-assisted pipelines beat naive R→G baselines and that early-turn PTKB provenance ranking is the Hard part (§6.1); ReTrace shows repaired workflow compilation can match agent success (75.8% vs. 80.6% on τ²-bench) while cutting runtime from 170s to 1.3s (§6.1); and AuthScribe shows trust metrics ( Sense of Agency Scale) need longitudinal measurement rather than a one-shot survey (§6.2, §8).
- **Design for provenance and inspectability, not just accuracy.** AuthScribe's Autobiographical Citations and Provisional Drafts answered the trust question by making AI contributions traceable to prior user writing (§4.4); ReTrace's evidence-cited rule documents and "unwitnessed" flags for alternatives the agent never took (§4.2) echo the same principle at the systems-engineering level; and iKAT's requirement that every response cite at least one provenance passage (§3.4) keeps personalized context auditable.
- **Known limitations to keep in mind.** ReTrace explicitly notes its reliance on agent reasoning that may be wrong, that it does not evaluate transfer to new procedures or domains (§9), and that a platform-reported "complete" flag can mask failed runs; AuthScribe notes its small 14-participant, two-week cohort and retrospective recall bias (§8); and iKAT's corpus is a manually inspected ClueWeb22-B subset with expert-checked topics (§3.3). Treat each paper's claims with appropriate confidence and funder/privacy caveats (iAuthScribe ran entirely on-device; iKAT excluded PII).

---

## Notes on Methodology and Sources

- **Search phrasings used:** "conversational search agents", "tool use" AND "LLM agents", "user trust" AND "LLM assistants", "agentic search", and cat:cs.HC trust + LLM assistants, sorted by relevance and by submission date.
- **Full-text work:** All three papers were downloaded and read in full text (via arXiv's HTML endpoint); findings above are tagged with the section they came from (RQ1/RQ2-based section names for AuthScribe; numbered sections for the other two).
- **Abs-page verification:** All three public arXiv abstract pages loaded successfully (https://arxiv.org/abs/2401.01330, https://arxiv.org/abs/2611.14492, https://arxiv.org/abs/2610.15439v1); primary categories and submission dates recorded above are taken from those pages. Where a page would not load, we fall back to the paper listing's report and note it here — no fallback was needed for any of these three.
- **Repository/PR context:** This file lives at the repo root on branch `add-reading-notes`, targeting `main` (the default branch), and is proposed via pull request #1 ("Add reading notes: agentic search and tool use") so team members can comment on `reading-notes.md` directly or on the PR before Thursday's sync.
