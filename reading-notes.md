# Reading Notes: Agentic Search and LLM Tool Use

**Purpose:** Team literature brief for Thursday's team sync on **agentic search** — assistants where an LLM-driven agent searches, calls tools, and hands answers to users. This note summarizes recent arXiv papers on three connected themes: (1) building and evaluating open-domain conversational search agents and tool use in LLM agents (arXiv:2602.20426, arXiv:2511.01854), (2) how AI assistants should adapt to user trust (arXiv:2502.13321), and (3) how agentic search is now measurable via benchmarks and claim-level evaluation (retained: arXiv:2506.21506 Mind2Web 2, arXiv:2507.16725 RAVine, arXiv:2409.18475, arXiv:2609.34930). Each entry includes arXiv ID, title, full author list, subject category (verified on the paper's public arXiv abs page), submission date, a 2–3 sentence abstract summary, and at least two concrete findings extracted from the full text with their source sections. Team members are encouraged to comment on this file or the pull request before Thursday's sync.

**Papers covered (3 primary):** 1. arXiv:2602.20426 (v2) — Learning to Rewrite Tool Descriptions for Reliable LLM-Agent Tool Use; 2. arXiv:2511.01854 (v2) — Tool-to-Agent Retrieval; 3. arXiv:2502.13321 (v2) — Adjust for Trust (retained from prior curation; full text re-read and findings re-verified this session). Also retained from prior curation: arXiv:2506.21506 (Mind2Web 2), arXiv:2507.16725 (RAVine), arXiv:2409.18475, arXiv:2609.34930 (PDEU-Bench).

**Method note:** Papers located via arXiv search on phrasings such as "conversational search agents", "tool use in LLM agents", and "user trust in AI assistants". Full texts were read via the arXiv HTML endpoint (2602.20426, 2511.01854, 2502.13321), and each paper's public abs page at arxiv.org/abs/{ID} was fetched directly to confirm the primary subject category and submission history. All three new abs pages loaded successfully; abs-page metadata for retained papers was verified in a prior session.

---

## Paper 1: arXiv:2602.20426 (v2) — Learning to Rewrite Tool Descriptions for Reliable LLM-Agent Tool Use

**Authors:** Ruocheng Guo, Kaiwen Dong, Xiang Gao, Kamalika Das

**Subject category (from abs page):** Artificial Intelligence (cs.AI) — primary
**Submission date (from abs page):** v1 Mon, 23 Feb 2026 23:50:24 UTC; current version v2 Wed, 29 Apr 2026

**Abstract summary:** While most effort on LLM tool-using agents goes into the agent itself (larger models, better prompting, fine-tuning), performance plateaus because of the quality of the tool interfaces agents consume. Tool descriptions are written for human developers, tolerate ambiguity agents cannot resolve, and get worse as candidate tool catalogs grow into the hundreds. The authors propose **Trace-Free+**, a curriculum learning framework that progressively transfers supervision from trace-rich settings to trace-free deployment so a description generator internalizes reusable interface patterns. On StableToolBench and RestBench with all test tools unseen at training time, it reduces accuracy degradation by 29.23% and improves average query-level success by 60.89% at 150+ candidate tools, transfers to BFCLv2 (lifting Gemini-3-pro-preview by up to 1.4 points purely via better interfaces), and gives complementary gains on top of agent fine-tuning.

**Extracted findings:**
- **Finding 1 (Section 6.3, "Scaling Experiments"):** In scaling experiments with candidate sets augmented to 150+ tools, Trace-Free+ reduces performance degradation of the original descriptions (D0) by 29.23% and improves over D0 by 60.89% on average on StableToolBench. The authors attribute this to a structural advantage: well-specified scope boundaries and parameter constraints help agents filter signal from noise as the candidate pool grows — an advantage per-tool rewriting methods (D1, fixed heuristics) cannot provide. They note these are the largest gains in their evaluation, suggesting interface optimization is most valuable when catalogs are large, and that this scalability advantage is unique to learned optimization.
- **Finding 2 (Section 6, "Limitations"):** Trace-Free+ does **not** outperform D1 on the single-step G1 subset of StableToolBench, because their synthesized queries average 3 tools and prioritize the practical multi-step agentic system at the cost of underrepresenting single-step patterns; D1's data-independent guidelines capture most of the headroom on G1 while D0 is already near the ceiling. The authors also acknowledge hallucination risk: the model may occasionally invent constraints (e.g., value ranges) not enforced by the server, and that the base description generator is Qwen3-4B-Instruct, with larger models left as future work.
- **Finding 3 (Section 1, "Introduction"):** Empirically, Trace-Free+ achieves up to **97.2% pattern coverage** across five categories on 4,585 unseen tools, while the original descriptions cover fewer than 12% in any category (Table 8) — evidence for the bounded reused-patterns hypothesis. Design decision: instead of re-running a costly per-tool pipeline (query synthesis → trace collection → annotation → strong-LLM prompting) for every new API, onboarding a new tool requires only its schema as input, with no query synthesis, trace collection, annotation, or rule extraction.

**Two-sentence takeaway:** The headline result is that fixing tool *interfaces* (not just models) is where the agentic-search quality lever lives: rewriting descriptions via curriculum learning cuts catalog-scale degradation by 29.23% and lifts query-level success by 60.89% at 150+ tools, and lifts a frontier model on BFCLv2 by 1.4 points with no retraining. Design implication for us: invest in tool-registry hygiene and description quality as a first-class engineering concern, and expect single-step tasks to be a weaker regime than multi-step agentic use.

---

## Paper 2: arXiv:2511.01854 (v2) — Tool-to-Agent Retrieval: Bridging Tools and Agents for Scalable LLM Multi-Agent Systems

**Authors:** Elias Lumer, Faheem Nizar, Anmol Gulati, Pradeep Honaganahalli Basavaraju, Vamse Kumar Subbiah

**Subject category (from abs page):** Computation and Language (cs.CL) — primary
**Submission date (from abs page):** v1 Mon, 3 Nov 2025 18:58:28 UTC; current version v2 Tue, 4 Nov 2025

**Abstract summary:** LLM multi-agent systems let sub-agents coordinate hundreds or thousands of tools or MCP servers, but existing retrieval matches queries only against coarse agent-level descriptions before routing, which obscures fine-grained tool functionality and yields suboptimal agent selection. The authors introduce **Tool-to-Agent Retrieval**, a unified framework that embeds both tools and their parent agents in a shared vector space connected by metadata relationships, enabling granular tool-level or agent-level retrieval without context dilution from chunking many tools together. Across eight embedding models it achieves consistent +19.4% Recall@5 and +17.7% nDCG@5 over previous state-of-the-art agent retrievers on LiveMCPBench.

**Extracted findings:**
- **Finding 1 (Section 3, "Experiments and Evaluation" / Table 1):** On LiveMCPBench, Tool-to-Agent Retrieval reaches Recall@1 0.61 / Recall@5 0.83 / nDCG@1 0.61 / nDCG@5 0.46, vs. ScaleMCP at 0.49 / 0.74 / 0.49 / 0.40 — i.e., +19.4% Recall@5 and +17.7% nDCG@5 over the previous SOTA across eight embedding models, and it beats BM25 (0.20/0.20) and Q.Retrieval (0.31/0.56).
- **Finding 2 (Section 2, "Approach")** — a concrete design decision with a concrete number: forwarding all tool descriptions to the model is impractical, e.g. **one MCP server with 26 tools can consume over 4,600 tokens**, so efficient retrieval is essential for scalability. The framework builds a unified tool–agent catalog with a tool corpus (tool names/descriptions + owner metadata) and agent corpus, uses BM25 + dense vector retrieval over top-N ≫ K entities, then aggregates parent agents (skipping entries with undefined owner) to return K unique executable agents.
- **Finding 3 (Section 2, "Query")** — two query paradigms: **Direct Querying** (use the user's high-level question as-is) and **Step-wise Querying** (decompose into sub-tasks, query each independently); the authors state step-wise querying is the primary setting used in evaluations, aligning with decomposition-for-planning methods.

**Two-sentence takeaway:** Routing is the make-or-break scaling detail for agentic search: a single MCP server with 26 tools costs 4,600+ tokens in flat descriptions, and joint tool+agent retrieval with metadata traversal beats coarse agent-first routing by ~20% on Recall@5 / nDCG@5 on LiveMCPBench. Practical takeaway: invest in tool-level indexing with BM25+dense hybrid retrieval and define clear owner mappings so tools resolve back to executable agents — this is table-stakes infra for any multi-tool assistant we build.

---

## Paper 3: arXiv:2502.13321 (v2) — Adjust for Trust: Mitigating Trust-Induced Inappropriate Reliance on AI Assistance

**Authors:** Tejas Srinivasan, Jesse Thomason

**Subject category (from abs page):** Human-Computer Interaction (cs.HC, primary); Artificial Intelligence (cs.AI); Computation and Language (cs.CL)
**Submission date (from abs page):** v1 Tue, 18 Feb 2025 22:42:39 UTC; current version v2 Mon, 26 Jan 2026

**Abstract summary:** User trust biases reliance on AI assistance, with low and high trust producing under- and over-reliance respectively. The authors propose that AI assistants adapt behavior via **trust-adaptive interventions**: supporting explanations when trust is low, counter-explanations when trust is high, and forced pauses to slow interactions. In two scenarios (laypeople answering science questions; doctors making diagnoses), supporting and counter-explanations at trust moments yield up to 38% reduction in inappropriate reliance and 20% improvement in decision accuracy, and adaptively inserted pauses reduce over-reliance; they also show purely interaction-level trust features are insufficient to predict trust moments.

**Extracted findings:**
- **Finding 1 (Section 10, "Experiment #5: Trust-Adaptive Interventions with an LLM Assistant") — validates on a real LLM:** With LLaMA-3.1-8B on the ARC task (30 participants per condition), trust-adaptive supporting/counter-explanations improved decision accuracy from 58.4% → 67.8% and reduced total inappropriate reliance from 83.5% → 68.2% for low-trust users; under-reliance fell when trust was low (65.1% → 44.4%) and over-reliance fell when trust was high (56.4% → 42.4%), mirroring the simulated-assistant findings and validating H11 that access to a trust signal enables trust-aware adaptation in human–LLM collaboration.
- **Finding 2 (Section 1, "Introduction") — the human-side numbers:** In studying doctors, they found clinicians mistakenly accept **26% of AI misdiagnoses when trust is high vs 8% when trust is lower** (over-reliance), and when trust is low they reject correct AI diagnoses **68% of the time vs 40% otherwise** (under-reliance). This suggested design rule: surface explanations when trust is low and counter-explanations when trust is high, and use forced pauses to promote deliberation when trust is excessive.
- **Finding 3 (Section 12, "Analysis: Can We Model User Trust?": purely interaction-level trust features (AIAcc5, CapabilityDiff, SmoothOutcomes, SmoothConfs, and a linear TrustModel) only reach ~0.43 correlation with self-reported trust and F1 ≤ 0.5 at detecting low/high trust moments — you cannot reliably infer trust from interaction signals alone, so measure it explicitly (they use an 11-point post-interaction scale). Section 14 "Limitations" flags: most studies used a simulated assistant rather than a real LLM, samples were limited to UK/US English speakers, and ethically the assistant must disclose to users that it is modeling their trust.

**Two-sentence takeaway:** Trust-adaptive design is a real lever — surfacing explanations when trust is too low (13–31% less under-reliance) and counter-explanations/pauses when trust is too high (up to 38% less inappropriate reliance, +20% decision accuracy on a real LLM assistant) materially improves outcomes, but "always-on" explanations backfire (they can worsen diagnosis accuracy), and you cannot infer trust from interaction signals (F1 ≤ 0.5), so measure trust explicitly and disclose that the assistant is modeling it. Pair with RAVine's attribution warning: calibrated, honest communication about confidence is a trust/safety prerequisite, not a UX add-on.

---

## Retained papers (prior curation, included for completeness)

### Paper 4: arXiv:2506.21506 (v2) — Mind2Web 2: Evaluating Agentic Search with Agent-as-a-Judge
**Authors:** Boyu Gou, Zanming Huang, Yuting Ning, Yu Gu, Michael Lin, Weijian Qi, Andrei Kopanev, Botao Yu, Bernal Jiménez Gutiérrez, Yiheng Shu, Chan Hee Song, et al.
**Subject category (from abs page):** Artificial Intelligence (cs.AI) primary; Computation and Language (cs.CL)
**Submission date (from abs page):** v1 Thu, 26 Jun 2025; v2 Thu, 3 Jul 2025

**Abstract summary:** Mind2Web 2 is a benchmark of 130 realistic, long-horizon web-search tasks requiring real-time browsing and information synthesis, built with 1,000+ hours of human labor. It proposes an **Agent-as-a-Judge** framework with tree-structured rubrics (avg 50 nodes, max 603) to evaluate time-varying, citation-backed answers. OpenAI Deep Research already reaches 50–70% of human performance at less than half the time, and agent-as-a-judge validation against humans was 99.03% correct (7/720 actual verifier errors).

**Extracted findings:** (§10.2 Main Results) Deep Research gets 0.54 partial completion vs 0.79 humans; ChatGPT Search and Perplexity Pro Search are weakest (success ≤ 0.11) due to short search horizons and shallow synthesis; 57 explicitly time-varying tasks expose the need for real-time browsing. (§10.4) Judge agents validated on 15 sampled tasks (720 leaf-node verifications): only 7/720 nodes reflected actual verifier errors (99.03% correctness), notably above prior automated evals (<90%).

**Two-sentence takeaway:** Agentic search is measurable now — Deep Research-style systems hit 50–70% of human performance at half the cost, and an Agent-as-a-Judge with tree rubrics is a repeatable, 99%-reliable way to evaluate long-horizon, citation-backed answers. Design lessons: budget for real-time browsing or you lose on time-varying tasks, and answer length does not correlate with quality.

### Paper 5: arXiv:2507.16725 (v2) — RAVine: Reality-Aligned Evaluation for Agentic Search
**Authors:** Yilong Xu, Xiang Long, Zhi Zheng, Jinhua Gao
**Subject category (from abs page):** Computation and Language (cs.CL) primary
**Submission date (from abs page):** v1 Tue, 22 Jul 2025; v2 Thu, 31 Jul 2025

**Abstract summary:** RAVine is a Reality-Aligned evaluation framework for agentic LLMs with search targeting multi-point queries and long-form answers reflecting realistic user intent, with nugget-centered ground-truth construction plus process-oriented metrics for tool use and efficiency. Built on MS MARCO V2.1 (10.96M docs) with search and fetch tools, it finds current models are limited in task completeness, faithfulness, and search performance; strong intermediate process performance does not guarantee high-quality final answers; and models rely on internal knowledge in unattributable ways.

**Extracted findings:** (§6.3) Qwen3-8B without thinking recalls only 22.3% of nuggets; Qwen3-32B achieves max citation recall of just 13.2% / precision 11.9%; a new Comp_in metric shows >50% of performance is traceable to unattributable internal knowledge; "illusion of progress" — no significant positive correlation between final task-completeness and search-precision scores.

**Two-sentence takeaway:** Evaluation of agentic search needs three axes, not just answer quality: (1) claim-level nugget attribution so unattributable internal-knowledge generation is penalized (a real trust/safety gap), (2) process metrics on tool use and efficiency, and (3) grade the output, not just the trajectory. Budget for multi-turn context management from day one.

### Paper 6: arXiv:2409.18475 (v1) — Data Analysis in the Era of Generative AI (retained)
**Authors:** Jeevana Priya Inala, Chenglong Wang, Steven Drucker, Gonzalo Ramos, Victor Dibia, Nathalie Riche, Dave Brown, Dan Marshall, Jianfeng Gao
**Subject category (from abs page):** Artificial Intelligence (cs.AI) primary; Human-Computer Interaction (cs.HC)
**Submission date (from abs page):** v1 Fri, 27 Sep 2024 06:31:03 UTC

**Abstract summary:** Industry-authored survey on how AI-powered tools reshape data analysis workflows, covering design considerations and challenges: translating high-level user intentions into executable code, charts, and insights; human-centered design principles for intuitive interactions and trust; and research challenges in capabilities, benchmarking, and end-user needs.

**Extracted findings:** (§6.1) Relying solely on natural language interfaces poses challenges — complex intents (e.g., chart legend placement) require users to acquire new "literacy"; design position is natural, multimodal, example-based intent input (mouse, pen, touch, GUIs, ONYX, sketching, programming-by-example). (§7.1) Reliability/trust design decisions: RAG grounding, input–output example verification of generated code, self-repair/self-rank mechanisms, fallback options, and the PCS (predictability, computability, stability) framework. (§7.2) Call for benchmarks reflecting multi-iteration analysis, evolving intent, and a centralized taxonomy of data tables/sources with high-level questions and low-level concrete tasks.

**Two-sentence takeaway:** If we ship an agentic search/analysis surface, design for multi-modal and example-based intent input, verify produced artifacts against examples, and budget for evaluation benchmarks that capture multi-step, intent-evolving workflows instead of single-turn accuracy.

### Paper 7: arXiv:2609.34930 (v1) — PDEU-Bench: Benchmarking the Personalized Planning Lifecycle of Tool-Calling LLM Agents (retained)
**Authors:** Huayi Lai, Shichao Song, Qingchen Yu, Simin Niu, Mengwei Wang, Hanyu Wang, Xun Liang (Renmin University of China; Beihang University)
**Subject category (from arXiv listing):** Artificial Intelligence (cs.AI) primary
**Submission date (from arXiv listing):** 28 Sep 2026 (v1); verified on the paper listing as noted in the prior session's method note.

**Abstract summary:** LLM agents are evolving from isolated tool-calling systems into task-oriented agents that pursue user goals through sustained, multi-step interactions. PDEU-Bench comprises 214 long-horizon interaction tasks across 12 everyday domains and 94 tools, with stage-specific assessments of preference adherence and plan quality. Evaluating 15 mainstream open-source and closed-source LLMs reveals a pronounced gap between local tool execution and dynamic planning: LLMs can often instantiate preferences in individual calls, yet struggle to construct coherent plan definition and plan update. No evaluated personalization/memory method reliably propagates user preferences throughout the complete lifecycle.

**Extracted findings:** (§1/§3) The benchmark decomposes the planning lifecycle into three independently measurable stages — Plan Definition, Plan Execution, and Plan Update — with 214 personalized scenarios across 12 domains and 94 tools on an extensible offline sandbox (85.7% of enriched data based on realistic scenarios). Evaluations of 15 LLMs show a pronounced gap between individual-call preference instantiation and coherent plan definition/update; six preference-enhancement methods improve particular stages but none reliably propagates preferences end-to-end, and fine-grained error analysis reveals preference omissions and conflicts persist throughout the lifecycle. Compared against existing personalized tool-use benchmarks, PDEU-Bench is the only one marked ✓ across all three capabilities (plan definition, execution, update).

**Two-sentence takeaway:** The key bottleneck for task-oriented agentic assistants is not raw tool execution but lifecycle-level planning: 214 tasks across 12 domains and 94 tools reveal that 15 LLMs can hit individual-call preferences but struggle with coherent plan definition and feedback-driven plan update, and no evaluated personalization/memory method reliably propagates preferences end-to-end. When benchmarking our agentic search prototype, measure plan-level outcomes (did it define a plan, execute it, and update it after tool outputs) with stage-specific preference-adherence checks.

---

## Cross-cutting takeaways for the team sync

1. **Three pillars of agentic search quality:** (a) Agentic search evaluation is now measurable (Mind2Web 2 §10.2: Deep Research hits 50–70% of human performance at half the time with a 99%-reliable Agent-as-a-Judge; PDEU-Bench shows lifecycle-level planning metrics are table-stakes); (b) evaluation must be claim-level, process-aware, and trust-aware (2409.18475 §7.1 reliability/trust design; RAVine §6.3 internal-knowledge attribution); (c) the human side is the hardest problem (Adjust for Trust §10/§12: trust-adaptive explanations up to 38% better, but trust is hard to model, F1 ≤ 0.5; disclose trust modeling to users).
2. **Design checklist we should adopt:** budget for real-time browsing and multi-step planning (2609.34930: plan definition/update are key bottlenecks; 2409.18475: iterative workflows across apps); grade outputs, not just trajectories, and penalize unattributable internal-knowledge generation (RAVine §6.3); build tool-registry hygiene and description-quality pipelines (2602.20426 §6.3); use joint tool+agent retrieval indexing with owner mappings (2511.01854 §2); use trust-adaptive rather than always-on explanations (2502.13321 §10); measure trust explicitly via an 11-point scale and disclose trust modeling to users (§12, §14); design for multi-modal and example-based intent input and verify artifacts against examples (§6.1, §7.1); and budget for long-horizon eval cost and multi-turn context management (2609.34930; 2409.18475 §7.2).
3. **Known limitations to carry into our design:** judge-agent validation needs human eyeballs (§10.4: 99.03% but with attribution-loss edge cases); process-oriented metrics and token/latency budgets are table-stakes (§5.1–5.2); simulated-AI study results may not transfer to real LLMs (§14); real-world deployment makes isolating component effects hard (§7); single-step task regimes are weaker than multi-step agentic use (2602.20426 §6 Limitations); and trust cannot be reliably inferred from interaction signals alone (§12).

---

## Sources

- arXiv abs pages (primary category + submission history, fetched directly at arxiv.org/abs/{ID}; all loaded successfully): arxiv.org/abs/2602.20426 (cs.AI, 23 Feb 2026), arxiv.org/abs/2511.01854 (cs.CL, 3 Nov 2025), arxiv.org/abs/2502.13321 (cs.HC primary, 18 Feb 2025); retained: arxiv.org/abs/2506.21506 (cs.AI), arxiv.org/abs/2507.16725 (cs.CL), arxiv.org/abs/2409.18475 (cs.AI), arxiv.org/abs/2609.34930 (cs.AI per paper listing).
- Full texts read via arXiv HTML endpoints: 2602.20426 (v2), 2511.01854 (v2), 2502.13321 (v2). Sections cited: 2602.20426 §1 Introduction, §6 Limitations, §6.3 Scaling Experiments; 2511.01854 §2 Approach, §3 Experiments and Evaluation, §2 Query; 2502.13321 §1 Introduction, §10 Experiment #5, §12 Analysis, §14 Limitations; Mind2Web 2 §10.2, §10.4; RAVine §6.3; Data Analysis §6.1, §7.1, §7.2; PDEU-Bench §1, §3, benchmark comparison.
- Repository: `mcpmark-eval-liuhezi/arxiv-agentic-search-notes`, branch `add-reading-notes` (off `main`, the default branch), file `reading-notes.md` at repo root, via pull request #1 "Add reading notes: agentic search and tool use" (https://github.com/mcpmark-eval-liuhezi/arxiv-agentic-search-notes/pull/1). Team members: please comment on the PR or the file to add discussion points before Thursday's sync.
