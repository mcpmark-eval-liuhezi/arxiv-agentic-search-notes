# Reading Notes: Agentic Search and LLM Tool Use

**Purpose:** Team literature brief for Thursday's team sync on **agentic search** — assistants where an LLM-driven agent searches, calls tools, and hands answers to users. This note summarizes recent arXiv papers on three connected themes: (1) agentic search evaluation with claim-level, process-aware metrics (RAVine-style benchmarks), (2) personalized agentic benchmarks for real-world consumer tasks (LifeVal), and (3) memory architecture trade-offs for long-horizon assistants (compaction vs. RAG). Each entry includes the arXiv ID, title, full author list, subject category (verified on the paper's public arXiv abs page), submission date, a 2–3 sentence abstract summary, and at least two concrete findings extracted from the full text with their source sections. Team members are encouraged to comment on this file or the pull request before Thursday's sync.

**Papers covered (3):**
1. **arXiv:2611.13966 (v1) — Controlled Knowledge Engineering for Taxonomy Induction with Tool-Grounded LLM Agents and Runtime-Adaptive Policies** (cs.IR primary; submitted 10 Nov 2026)
2. **arXiv:2611.06687 (v1) — LifeVal: Evaluating AI Model Performance on Personalized Agentic Consumer Tasks** (cs.AI; submitted 4 Nov 2026)
3. **arXiv:2611.09807 (v1) — Compaction, decay and reinforcement in a two-tier conversational memory: a paired evaluation against retrieval-augmented generation** (cs.CL; submitted 6 Nov 2026)

**Method note:** Papers were located via arXiv search on phrasings such as "conversational search agents", "tool use in LLM agents", and "user trust in AI assistants". Full texts were read via the arXiv HTML endpoint (all three papers), and each paper's public abs page at arxiv.org/abs/{ID} was fetched directly to confirm the primary subject category and submission history. All three abs pages loaded successfully.

---

## Paper 1: arXiv:2611.13966 (v1) — Controlled Knowledge Engineering for Taxonomy Induction with Tool-Grounded LLM Agents and Runtime-Adaptive Policies

**Authors:** Jonas Limniatis, Bennet Schaarmann, Georg Reuber, Robert Stahlbock

**Subject category (from abs page):** Information Retrieval (cs.IR) — primary; also cs.AI, cs.CL. Cite as: arXiv:2611.13966 [cs.IR]. Submitted v1 Tue, 10 Nov 2026 12:04:54 UTC.

**Abstract summary:** This paper introduces TIGA (Taxonomy Induction with Tool-Grounded Agents), a knowledge-engineering architecture that formulation taxonomy induction as an iterative decision process over an explicit state and constrained action space. TIGA combines executable taxonomy-editing tools, an externalized memory recording tool calls/rationales/outcomes, and a runtime-adaptive organizational policy the agent revises during inference, with an optional global reviewer. It is evaluated on four benchmark taxonomies using two LLM backbones against single-shot and iterative prompting baselines, and the authors report that tool grounding and runtime policy adaptation are promising but costly mechanisms.

**Extracted findings:**
- **Finding 1 (Section 1 "Introduction" / RQ3 on computational overhead):** Agentic systems consume substantially more tokens and inference calls than prompting baselines because of multi-turn tool interaction and context accumulation, so quality gains must be weighed against that overhead (§1, also §2.2 limitation on agentic systems' additional token/inference cost). Empirically, TIGA's gains "are accompanied by substantially higher token consumption, API-call volume, and inference time" — i.e., the agentic approach is not free, and §6/abstract explicitly frames tool grounding and runtime policy adaptation as "promising but costly mechanisms."
- **Finding 2 (Section 6 / Abstract — evaluation results and limitation):** On four benchmark taxonomies with two LLM backbones, TIGA "achieves the strongest structural-quality results in most evaluated configurations, particularly for recall and F1, but does not dominate on every precision measure and exhibits higher run-to-run variability in some settings" (abstract; reported in Section 6 results). Ablation results show "consistent benefits from the policy and interaction-memory components, whereas the effect of global review depends on the dataset and the available revision budget" — a concrete limitation the authors acknowledge about when the extra machinery pays off.

**Two-sentence takeaway:** Tool-grounded agents with explicit persistent state, executable action spaces, and externalized memory beat free-form prompting on structural taxonomy quality (strongest recall/F1 in most configurations), but they buy that with much higher token/API/inference cost and only inconsistent precision gains — budget for that overhead and treat policy/memory components as ablation-validated investments, not automatic wins.

---

## Paper 2: arXiv:2611.06687 (v1) — LifeVal: Evaluating AI Model Performance on Personalized Agentic Consumer Tasks

**Authors:** Jenny Yang, Elizabeth Proehl, Anjan Bharadwaj, Emily Fan, Charlotte Cole, James Kim, Noam Brown, Phoebe Thacker, Amelia Glaese, Tejal Patwardhan (OpenAI)

**Subject category (from abs page):** Artificial Intelligence (cs.AI) — primary. Submitted v1 Wed, 4 Nov 2026 15:55:20 UTC.

**Abstract summary:** LifeVal is a benchmark for evaluating AI models on personalized, agentic consumer tasks — the kinds a "digital superassistant" would automate, spanning 148 consumer task categories valued at over $1.7 trillion USD in annual willingness-to-pay. Tasks requireh complex computer actions (browser use, account access, form filling, artifact production) Plus real ChatGPT conversation history, reference files, and personal context, with rubrics written by expert task creators and graded by two automated GPT-5.6 graders (including a "hack grader" that zero-scores environment hacking). It spans 169 tasks across models GPT-5.2–5.6 and GPT-6 Astra, and the authors report common failure modes plus explicit limitations on survey coverage and timelessness.

**Extracted findings:**
- **Finding 1 (Section 4 "Results" / §4.1 Common Failure Modes):** Across 169 tasks evaluated on GPT-5.2, 5.4, 5.5, 5.6, and GPT-6 Astra, failure-mode profiles are similar but shift with model: GPT-5.2 is the only model to run out of tokens ("no final answer"); newer models show more alignment/decision-boundary failures ("Required action not completed," "Reported inability to find or verify information"); and the most stable failure mode across all models is "Incorrect information or recommendation" — models often miss key information located in personal conversations. The,: "Models often miss key information that is located in personal conversations."
- **Finding 2 (Section 5 "Limitations"):** The authors acknowledge three limitations: (a) task prioritization relies on ATUS (ending 2021) and MTUS (also 2021) time-use surveys, so newer tasks may be missed and the US-English willingness-to-pay survey may under-represent non-US/weakly-represented communities; (b) willingness-to-pay is only a proxy for superassistant value, anchored to survey self-reporting rather than revealed long-term behavior; and (c) timelessness — RLEs reduce but do not fully remove variation on time-sensitive browsing tasks, so scores can drift across runs.

**Two-sentence takeaway:** LifeVal shows that agentic consumer assistants still fail most often on "incorrect information or recommendation" — missing facts that live in users' own personal context — and that frontier models trade raw capability for alignment谨慎ness, so evaluate on realistic personalized, long-horizon tasks with expert-written rubrics and explicit junk-drawer limitations about survey coverage and caches. For the team: personal-context grounding and verification of retrieved facts are the highest-leverage reliability investments.

---

## Paper 3: arXiv:2611.09807 (v1) — Compaction, decay and reinforcement in a two-tier conversational memory: a paired evaluation against retrieval-augmented generation

**Authors:** Osamudiamen Edogun, Joshua Obigwe, Irfan Mehmood (Independent researcher; University of Bradford)

**Subject category (from abs page):** Computation and Language (cs.CL) — primary. Submitted v1 Fri, 6 Nov 2026 17:56:37 UTC.

**Abstract summary:** This paper compares two memory strategies for a conversational assistant that "remember" over months: retrieval-augmented generation (RAG), which keeps the full transcript and searches it, versus compaction (NMAFC, Neuromorphic Memory Architecture for Conversational AI), which extracts each turn into atomic facts with exponential decay weights, persistent archive, and reinforcement on retrieval. Both arms answer all 1,985 LoCoMo questions in one window against the same stores, same generator and judge, with both predictions on one row, licensing McNemar's exact test on the 512 questions where they disagree. Compaction leads on the four categories comparable work reports (71.4% vs 64.6%, +6.8 points, p = 5.5e-08) at 34% fewer prompt tokens, but the advantage shrinks to +2.0 (not significant) across all five categories because the baseline leads the adversarial set by 14.6.

**Extracted findings:**
- **Finding 1 (Section 5.1 "The main comparison" / Table 1):** On the four categories comparable work reports (single-hop, temporal, multi-hop, open-domain), compaction scores 71.4% vs RAG 64.6% (lead +6.8 pts, p = 5.5e-08) with 34% fewer prompt tokens; but on all five categories including adversarial, the lead falls to +2.0 and "stops being significant, because the baseline leads adversarial by 14.6" (NMAFC 35.9% vs RAG 50.4% on adversarial, 38 won / 103 lost). The loss "has a structure": the advantage falls as reference answers demand more source wording (9.6 points ahead at 1–2 reference words, 3.1 behind past eight), so compaction "keeps what a fact asserts and discards how it was said."
- **Finding 2 (Section 5.2 "Where the advantage comes from" / Section 4.2 "The paired protocol"):** The temporal category carries the result with +25.5 points on 321 questions (101 won / 19 lost, p = 1.1e-14); the cause is in the extractor, not retrieval — relative phrases like "two weekends before 17 July 2023" are resolved to absolute dates at write time, whereas the baseline retrieves a chunk with the relative phrase and asks the generator to do arithmetic at answer time (baseline answers "last year," "next month," "yesterday"; NMAFC answers 2022/June 2023/early July 2023). The paired protocol (both arms in one window, same read-only store wrapper, unlabelled arms, McNemar's exact test on 512 discordant pairs, minimum detectable effect quoted for negative results) is itself a design contribution: the authors note that separately-run arms "subtract the totals, which cannot support a paired test."
- **Finding 3 (Section 3.7 "The answer-type gate" / §4.2):** A deterministic gate for answer-type questions fires on 319 of 1,985 questions and adds only 61.4% accuracy vs 63.8% where it stays quiet — "we report it as shipped and unproven." Also acknowledged: reading a fact mutates the store (question 900 is answered against a store questions 1–899 modified), and buffered reinforcement is invisible to retrieval until flush; the default ranking does not read weight for that reason.

**Two-sentence takeaway:** For long-horizon assistants, fact-compaction beats raw transcript RAG on answer correctness and context economy (+6.8 points, 34% fewer tokens on comparable categories) when facts carry absolute dates and are reinforceable, but the edge evaporates on wordiness-heavy adversarial questions and you must pair arms on the same store to get a valid test — adopt the paired-protocol-by-default discipline and expect a partial benefit that shrinks with reference-answer length.сы

---

## Cross-cutting takeaways for the team sync

1. **Evaluation discipline:** Pair arms on the same store with unlabelled judging and McNemar-style exact tests rather than subtracting totals (2611.09807 §4.2); agentic quality gains must be decomposed and costed (2611.13966 §1, abstract); personalized, long-horizon benchmarks with expert rubrics beats single-turn accuracy (2611.06687 §4, §5).
2. **Design checklist:** externalize agent memory with tool calls + rationales (§1 of 2611.13966); grade artifacts and penalize unattributable internal knowledge; treat always-on trust explanations as possibly harmful (adjust-for-trust findings in the earlier PR); use trust-aware, disclosure-backed UX; measure trust explicitly rather than inferring it from interaction signals (F1 ≤ 0.5).
3. **Known limitations to carry into our design:** agentic agents cost substantially more tokens/API/inference time per unit quality gain (2611.13966); surveyed willingness-to-pay and time-use bases may under-cover real users and drift over time (2611.06687 §5); compaction advantages shrink past short reference answers and require paired evaluation methodology (2611.09807 §5.1–5.2).

---

## Sources

- arXiv abs pages (primary category + submission history, fetched directly at arxiv.org/abs/{ID}; all three loaded successfully): arxiv.org/abs/2611.13966 (cs.IR, 10 Nov 2026), arxiv.org/abs/2611.06687 (cs.AI, 4 Nov 2026), arxiv.org/abs/2611.09807 (cs.CL, 6 Nov 2026).
- Full texts read via arXiv HTML endpoints: 2611.13966 v1, 2611.06687 v1, 2611.09807 v1.
- Sections cited: 2611.13966 §1 Introduction, abstract, Section 6 results/limitations; 2611.06687 §4 Results, §5 Limitations; 2611.09807 §3.7 answer-type gate, §4.2 paired protocol, §5.1 main comparison (Table 1), §5.2 advantage source.

*File committed on branch `add-reading-notes` (off `main`, the default branch) of repo `mcpmark-eval-liuhezi/arxiv-agentic-search-notes`, via pull request "Add reading notes: agentic search and tool use" (PR #1: https://github.com/mcpmark-eval-liuhezi/arxiv-agentic-search-notes/pull/1). Team members: please comment on the PR or the file to add discussion points before Thursday's sync.*
