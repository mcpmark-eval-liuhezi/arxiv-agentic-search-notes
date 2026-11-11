# Reading Notes: Agentic Search and LLM Tool Use

**Prepared for:** Thursday team sync — agentic search week
**Scope:** A short literature brief on assistants where an LLM-driven agent searches, calls tools (retrieval, generation, API-style components), and hands answers to users. Papers were selected via arXiv searches on "conversational search agents", "tool use in LLM agents", and "user trust in AI assistants". For each paper the public arXiv abstract page was checked for the primary subject category and submission date, and findings below were extracted from the full text (HTML version) with section pointers.

**Papers covered:**
1. **arXiv:2601.11888** — Agentic-R: Learning to Retrieve for Agentic Search (cs.IR, submitted 17 Jan 2026)
2. **arXiv:2606.02871** — Adaptive Latent Agentic Reasoning / ALAR (cs.CL, submitted 1 Jun 2026)
3. **arXiv:2402.08030** — Why and When LLM-Based Assistants Can Go Wrong (cs.HC, submitted 12 Feb 2024)

---

## Paper 1: arXiv:2601.11888 — Agentic-R: Learning to Retrieve for Agentic Search

**Authors:** Wenhan Liu, Xinyu Ma, Yutao Zhu, Yuchen Li, Daiting Shi, Dawei Yin, Zhicheng Dou
**Subject category (from arXiv abs page):** Information Retrieval (cs.IR); also cs.CL — primary category cs.IR
**Submission date (from arXiv abs page):** v1 submitted Sat, 17 Jan 2026 02:59:54 UTC (658 KB)
**Link:** https://arxiv.org/abs/2601.11888

**Abstract summary (2-3 sentences):** Agentic search interleaves multi-step reasoning with on-demand retrieval to solve complex questions, but most research optimizes the agent rather than the retriever. This paper proposes Agentic-R, a retriever training framework for agentic search that measures passage utility with both local query-passage relevance (via an LLM-based listwise scoring approach) and global answer correctness, and optimizes the search agent and retriever bidirectionally and iteratively. Extensive experiments on seven single-hop and multi-hop QA benchmarks show the retriever consistently outperforms strong baselines across different search agents, and also reduces the number of search queries the agent needs.

**Extracted findings:**

- **(Section 6.2 "Overall Performance") — Evaluation result:** Agentic-R consistently achieves the best average exact match (EM) across all three evaluated search agents (trained Agent-2, R1-Searcher, and SimpleDeepSearcher), beating the second-best baseline by ~3.2 points on the trained agent and ~2 points on R1-Searcher and SimpleDeepSearcher, demonstrating generalization beyond the in-domain agent. It yields larger gains on multi-hop QA (~3 pt gap over REPLUG) than single-hop QA (~2 pt), and RAG-specific retrievers (LLM-Embedder, SCARLet) do not consistently beat general-purpose E5 — likely because single-turn RAG utility (gold answers, fixed user questions) does not transfer to multi-turn agentic search with agent-generated queries.
- **(Section 5.1.1 "Training Data Construction") — Design decision:** Passage utility is modeled from two angles: local relevance via an LLM-based listwise scorer over 20 candidate passages per intermediate query, and global correctness by checking whether the agent can derive the correct final answer when using a specific passage. Training data comes from letting the search agent generate a full trajectory {reasoning, query, passages, answer} and scoring each candidate passage.
- **(Section 8 "Limitations") — Acknowledged limitation:** Evaluation is limited to widely used single-hop/multi-hop QA benchmarks that do not represent expert-level or scientific reasoning (e.g., GPQA), and experiments used moderately sized search agents and retriever backbones due to compute/memory constraints, so larger-scale validation is left as future work.

**Two-sentence takeaway:** Agentic-R shows the retriever is the underexplored component of agentic search: scoring passages on both local relevance and final-answer correctness, and co-optimizing agent and retriever iteratively, gives a ~3-point EM gain over the second-best baseline that generalizes across three search agents. The authors' own caveat is that gains are currently bounded by standard QA benchmarks and moderately sized models.

---

## Paper 2: arXiv:2606.02871 — Adaptive Latent Agentic Reasoning (ALAR)

**Authors:** Dongwon Jung, Peng Shi, Yi Zhang, Junshan Zhang, Muhao Chen
**Subject category (from arXiv abs page):** Computation and Language (cs.CL); also cs.AI — primary category cs.CL
**Submission date (from arXiv abs page):** v1 submitted Mon, 1 Jun 2026 20:36:06 UTC (373 KB)
**Link:** https://arxiv.org/abs/2606.02871

**Abstract summary (2-3 sentences):** Current LLM agents generate verbose explicit chain-of-thought (CoT) at every decision step, which is inefficient in multi-turn agentic trajectories. ALAR is a dual-mode framework that uses compact latent reasoning for routine turns and escalates to explicit CoT only when deeper deliberation is needed, learning latent reasoning with the agent's actions as supervision anchors (Action-Anchored Self-Distillation, AASD) and adaptive mode selection via AR-GRPO. On agentic search and tool-use benchmarks ALAR maintains comparable-or-better accuracy while cutting generated tokens by up to 43.6% in search and 84.6% in tool use.

**Extracted findings:**

- **(Section 6.1 "Main Results") — Evaluation result:** On the tool-use domain (Qwen3-4B-Thinking), Stage 1 improves average accuracy from 86.1 to 88.8 while reducing generated tokens by 88.6%, and Stage 2 further improves accuracy to 89.2 while reducing tokens by 84.6% — the best EM and strongest accuracy-efficiency (AE) score among all methods. In search (7B), Stage 2 reduces tokens by 43.6% while maintaining comparable EM, and Stage 1 achieves 52.5% token reduction. The paper notes text-based reasoning compression has limited headroom (~4–10% token reduction) because Search-R1 already produces compact explicit CoT, whereas ALAR changes the reasoning interface itself.
- **(Section 4.4 "Action-Anchored Self-Distillation") — Design decision:** Instead of aligning latent thoughts with textual CoT (which is nontrivial since continuous thoughts live in hidden-state space), AASD replaces each teacher CoT span with a latent block and trains the student to reproduce the teacher's subsequent environment-facing action — actions are the natural supervision anchor because they are where the agent interacts with the environment.
- **(Section 8 "Limitations") — Acknowledged limitations:** The work focuses on environment-interacting agentic tasks (search/tool use) and does not evaluate math/coding domains where more reasoning tokens may directly improve final accuracy; AASD relies on successful teacher trajectories so the latent policy may inherit teacher coverage and biases; it uses a fixed latent block length K and discrete latent/explicit mode choice; and latent reasoning reduces煮 Hence the ethics statement recommends monitoring agent actions/outputs and retaining explicit reasoning or extra logging in high-stakes settings.

**Two-sentence takeaway:** ALAR's core finding is that agentic reasoning is often unnecessarily verbose — many turns only need enough internal computation to pick the next tool call, so latent reasoning by default with selective explicit CoT escalation delivers ~44% token savings in search and ~85% in tool use without-em Agency accuracy loss. The trade-off to plan for is interpretability: latent reasoning makes part of the agent's computation less visible, which the authors flag as a design consideration for high-stakes settings.

---

## Paper 3: arXiv:2402.08030 — Why and When LLM-Based Assistants Can Go Wrong

**Authors:** Anjali Khurana, Hari Subramonyam, Parmit K Chilana
**Subject category (from arXiv abs page):** Human-Computer Interaction (cs.HC); also cs.AI, cs.LG — primary category cs.HC
**Submission date (from arXiv abs page):** v1 submitted Tue, 12 Feb 2024 19:49:58 UTC (accepted at IUI'24)
**Link:** https://arxiv.org/abs/2402.08030

**Abstract summary (2-3 sentences):** This paper reports a within-subject experiment (16 participants, no ML/NLP expertise) comparing a baseline LLM assistant (GPT-4 via ChatGPT Plus) with SoftAIBot, an LLM optimized for a software domain (PowerPoint/Excel help-seeking) with prompt guidelines and integrated domain context. Although SoftAIBot produced more accurate and relevant output (PowerPoint accuracy 64.4% vs 37.5%; relevance 74.7% vs 44.4%), users could not detect the difference, task completion was low (35% PowerPoint / 40% Excel with no significant difference between conditions), and users often followed LLM suggestions verbatim even when incorrect, indicating overtrust and a gap between users' lack of software expertise and their ability to evaluate the assistant.

**Extracted findings:**

- **(Section 5.1 "Task Completion, Accuracy and Relevance of LLM Assistance") — Evaluation result:** SoftAIBot was significantly more accurate (PowerPoint: 64.4% vs 37.5%, p=0.0006; Excel: 65.7% vs 45%, p=0.0011) and more relevant (PowerPoint: 74.7% vs 44.4%, p<0.0001; Excel: 78.2% vs 55.4%, p<0.0001), yet no participant fully completed either task; average completion was 35% (PowerPoint) and 40% (Excel) with no significant difference between SoftAIBot and baseline (p=0.28 / p=0.07), and expert-rated accuracy differences were not significant (p=0.10). Perceived accuracy (12/16 vs 8/16) and relevance (13/16 vs 9/16) did not differ significantly across conditions, but 14/16 users trusted SoftAIBot more than baseline on PowerPoint (p=0.006) — a trust gap between actual quality and user perception.
- **(Section 5.4 "Coherent LLM Output Leads to Blind Faith") — User behavior finding:** Most users struggled to map LLM instructions to the actual software UI, followed suggestions verbatim even when wrong, and remained unaware of inaccuracies — the authors attribute this to users lacking accurate mental models of LLMs and over-trusting output correctness withoutmuch contemplation.
- **(Section 7 "Limitations") — Acknowledged limitation:** Findings are constrained by the specific applications studied (PowerPoint/Excel), may not generalize to other LLM implementations given rapid evolution and variability, and the study did not control for individual differences such as expertise or familiarity with the software; the authors call for larger and more qualitative studies.

**Two-sentence takeaway:** The study's headline finding is a trust–quality gap: SoftAIBot's output was significantly more accurate and relevant than baseline, but users could not perceive the difference, completed tasks at only ~35–40%, and over-trusted coherent output even when inaccurate — calling for explainable, context-aware design cues so users can evaluate assistive output. Its documented limitation is that results are tied to two specific applications and non-expert participants, so teams should validate trust effects with larger studies before concl回忆.

---

## Quick reference table

| arXiv ID | Title | Primary category | Submitted (abs page) | Key sections for notes |
| --- | --- | --- | --- | --- |
| 2601.11888 | Agentic-R: Learning to Retrieve for Agentic Search | cs.IR (also cs.CL) | 17 Jan 2026 | §5.1.1 Training Data Construction; §6.2 Overall Performance; §8 Limitations |
| 2606.02871 | Adaptive Latent Agentic Reasoning (ALAR) | cs.CL (also cs.AI) | 1 Jun 2026 | §4.4 AASD; §6.1 Main Results; §8 Limitations |
| 2402.08030 | Why and When LLM-Based Assistants Can Go Wrong | cs.HC (also cs.AI, cs.LG) | 12 Feb 2024 | §5.1 Task Completion; §5.4 Coherent LLM Output; §7 Limitations |

## Suggested discussion prompts for Thursday's sync

1. Should we adopt provenance-cited responses (citations required) as a guardrail for our assistant's answers, per the iKAT-style benchmark design in Agentic-R and the trust study?
2. Is the "latent reasoning by default" Idea from ALAR applicable to our agent's tool-calling loop — when should we force explicit CoT for harder decisions?
3. Both the trust study and ALAR's limitations section warn about Interpretability and attribution: how do we validate our agent's groundedness independently and isolate design choices in evaluation?
4. The trust study found prompt guidelines and domain context did not significantly change user behavior despite improving output quality — what does that mean for our prompt-engineering investment?
