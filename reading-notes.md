# Reading Notes: Agentic Search and Tool Use

A short literature brief for the team sync on **agentic search** — assistants where an LLM-driven agent searches, calls tools, and hands answers to users. It covers three recent arXiv papers chosen to span the three angles we care about:

1. **An empirical, end-to-end audit of what actually happens** when conversational agents decide to search, formulate queries, and cite sources (Amani et al., 2026).
2. **A production-grade architecture** that adds agentic tool calling on top of RAG to reach dynamic, real-time data sources (Zhao et al., 2025).
3. **A reliability attack surface** in how agents select between competing tools, which bears directly on how we expose and describe our own tools (Faghih et al., 2025).

For each paper you'll find its arXiv ID, title, author list, subject category and submission date (taken from the public arXiv abstract page for that ID), a short abstract summary, and a set of concrete findings extracted from the full text with the section each came from. Please leave comments directly on this file in the PR.

---

## Paper 1 — Characterizing Web Search by Conversational LLM Agents

| Field | Value |
|---|---|
| **arXiv ID** | 2609.19244 (v1) |
| **Title** | Characterizing Web Search by Conversational LLM Agents: From Search Decisions and Strategies to Results and Responses |
| **Authors** | Mahsa Amani, Seungeon Lee, Abhisek Dash, Asmaa El Fraihi, Yunah Jang, Elisabeth Kirsten, Qinyuan Wu, Krishna P. Gummadi, Manish Gupta, Abhilasha Ravichander, Muhammad Bilal Zafar, Soumi Das |
| **Subject category** | Artificial Intelligence (cs.AI) — primary; Information Retrieval (cs.IR) |
| **Submission date** | Wed, 16 Sep 2026 17:56:32 UTC |
| **URL** | https://arxiv.org/abs/2609.19244 |

### Abstract summary
This is the first study of Web search behaviour across four major conversational platforms (ChatGPT, Claude, Grok, DeepSeek), combining real donated user conversations ("in vivo") with controlled API replay of the same prompts ("in vitro"). The authors examine when agents decide to invoke Web search, how they formulate and reformulate queries, domain preferences in the search results they receive, and how they turn results into grounded, cited responses. They find that search-calling decisions vary widely across platforms and models, that more frequent search invocation does not necessarily yield better response quality, and that although responses are largely grounded, some claims rest on uncited search results — raising attribution and reliability concerns.

### Findings
- **Search-invocation decisions are wildly platform-dependent, and don't track quality.** On the same 1,000 user prompts replayed *in vitro*, GPT called Web search for only 140 prompts (14%) while Claude called it for 825 (83%) — yet agents are good at knowing when they *don't* need search, and invoking search more often does not improve response quality. **[Source: §3.1 "Search Calling Behaviors"]**
- **Platform search engines are small and heavily domain-biased.** Engines built for LLM agents return far fewer results than a normal Google search (roughly 5–15 URLs per query depending on platform), and the top-10 domains account for a large share of results — 21.3% *in vivo* / 47.8% *in vitro* for ChatGPT, 32.3% / 29% for Grok. Preferences differ sharply: `reddit.com` and `youtube.com` dominate ChatGPT, Grok and DeepSeek results but are **completely absent** from Claude search; `xiaohongshu.com` is prominent in DeepSeek but absent elsewhere. **[Source: §4.3 "Domain Preferences in Search Results"]**
- **Citation practice varies enormously and agents preferentially cite authoritative domains.** Citation rates (fraction of returned search-result URLs actually cited) range from 1.7% to 34.0% *in vivo* and 10.6% to 40.0% *in vitro* — e.g. Grok cites just 1.8% of returned URLs, DeepSeek 34.1%. Agents skew toward higher-authority (lower Tranco-rank) domains when citing, and hallucinated citations stay rare at 1–2%. **[Source: §5.1 "Selection / Generation of Citations"]**
- **A substantial share of claims rest on uncited sources or on parametric knowledge, and those score worst on factuality.** Across ChatGPT, Claude and Grok, URLs that were returned during search but never cited still support **14% to 53%** of all claims. Upwards of 15–20% of claims remain ungrounded altogether. Grounded claims score highest on factuality (3.47 / 3.35 for associated/other cited sources in ChatGPT), claims supported by uncited search results score 3.33, and ungrounded (likely parametric) claims score lowest at 2.84. **[Source: §5.2 "Grounding of Claims"]**
- **Acknowledged limitation — behaviour may be harness, not model.** The observational traces do not expose internal reasoning, hidden chain-of-thought, ranking algorithms or proprietary retrieval pipelines, so "some observed behaviors may reflect platform-specific harness implementations rather than the underlying language models alone"; many analyses also depend on imperfect automated judge models (human–judge entailment agreement was 93%; within-1 agreement on response quality 67–77%). **[Source: "Limitations"]**

### Two-sentence takeaway
Search-happy agents aren't better agents: across four platforms the decision to invoke search varies from 14% to 83% of identical prompts with no consistent quality payoff, and users have no visibility into the heavy, platform-specific domain biases (e.g. Claude never surfacing Reddit/YouTube) behind "Web-grounded" answers. Attribution is the weakest link — a large slice of claims (14–53%) are supported by URLs the agent fetched but never cited, and ungrounded parametric claims are measurably the least factual part of every response.

---

## Paper 2 — TURA: Tool-Augmented Unified Retrieval Agent for AI Search

| Field | Value |
|---|---|
| **arXiv ID** | 2508.04604 (v2) |
| **Title** | TURA: Tool-Augmented Unified Retrieval Agent for AI Search |
| **Authors** | Zhejun Zhao, Yuchen Li, Alley Liu, Yuehu Dong, Xiaolong Wei, Lixue Zheng, Pingsheng Liu, Dongdong Shen, Long Xia, Jiashu Zhao, Dawei Yin (Baidu Inc.) |
| **Subject category** | Computation and Language (cs.CL) — primary; Artificial Intelligence (cs.AI); Information Retrieval (cs.IR) |
| **Submission date** | v1: Wed, 6 Aug 2025 16:24:17 UTC; v2: Thu, 12 Mar 2026 12:25:28 UTC |
| **URL** | https://arxiv.org/abs/2508.04604 |

### Abstract summary
TURA is a three-stage production framework from Baidu that combines RAG over static web corpora with agentic tool calling so an AI-search agent can reach dynamic, real-time sources such as APIs and databases that static page indexing simply cannot serve. Its three components are an Intent-Aware Retrieval module that decomposes a query and retrieves relevant Model Context Protocol (MCP) servers, a DAG-based Task Planner that models sub-task dependencies for parallel execution, and a lightweight Distilled Agent Executor for fast tool calls. The paper also releases a benchmark derived from anonymized Baidu Search production logs with expert annotation, and reports results from both offline evaluation and a live A/B test serving tens of millions of users.

### Findings
- **Agentic tool use beats passive retrieval by a wide margin.** On MCP-Bench, TURA reached 87.5% answer accuracy in human evaluation vs. 65.3% for plain LLM+RAG, and 96.2% faithfulness vs. 72.4% — with Tool-Agent in between at 76.8% / 81.7%. The authors attribute the faithfulness jump specifically to invoking *verified tools* rather than synthesising from noisy retrieved text. **[Source: §5.2.1 "End-to-End Offline Evaluation", Table 1]**
- **Results held up in a live A/B test at production scale.** Against the incumbent LLM+RAG system, TURA raised Session Success Rate from 55.1% to 64.0% (+8.9%) and achieved an 8.7% overall performance advantage, while cutting total issues by 16.7% (66 → 55) — accuracy −7.1%, content richness −28.6%, content value −17.6% — and reducing critical failures from 9 to 4. **[Source: §5.2.2 "Online Deployment and A/B Testing Results", Tables 2 & 3]**
- **Design decision — the agent's executor is a small distilled model, not a frontier LLM.** Running DeepSeek-V3 for fine-grained tool execution was deemed infeasible for real-time latency, so TURA distils expert ReAct-style trajectories into a much smaller Qwen3 model using "train-with-thought, infer-without-thought": rationales are trained on but omitted at inference. **[Source: §4.4 "Distilled Agent Executor"]**
- **The distilled 4B student beats its 671B teacher and GPT-4o on tool calling — and is dramatically cheaper.** Qwen3-4B Distilled hit 88.3% function-calling accuracy and Qwen3-30B-A3B Distilled 88.7%, vs. 82.4% for the 671B-parameter DeepSeek-V3 teacher and 81.7% for GPT-4o. The 4B model was chosen for deployment over the higher-accuracy MoE because it serves on a single GPU while requiring dual-GPU L20. **[Source: §5.3.3 "Effectiveness of Agent Distillation (RQ4)", Table 7]**
- **The DAG planner is a latency win with no accuracy cost.** On the complex multi-hop subset of MCP-Bench where parallelism is possible, the DAG-based planner cut average latency by **44.2%** with no degradation in execution success rate. **[Source: §5.3.2 "Importance of the DAG-based Task Planner (RQ3)", Table 6]**

### Two-sentence takeaway
TURA is the strongest available evidence that agentic tool calling isn't a research curiosity but a production win: +22 accuracy points and +24 faithfulness points over RAG offline, and a +8.9-point session-success-rate lift in a live A/B test at Baidu, driven almost entirely by real-time queries where static retrieval structurally fails. The engineering message for us is that the agent executor can be a small, distilled model — their 4B student beat a 671B teacher and GPT-4o at tool calling while serving on one GPU — provided you invest in trajectory curation and DAG-style parallel planning.

---

## Paper 3 — Tool Preferences in Agentic LLMs are Unreliable

| Field | Value |
|---|---|
| **arXiv ID** | 2505.18135 (v2) |
| **Title** | Tool Preferences in Agentic LLMs are Unreliable |
| **Authors** | Kazem Faghih, Wenxiao Wang, Yize Cheng, Siddhant Bharti, Gaurang Sriramanan, Sriram Balasubramanian, Parsa Hosseini, Soheil Feizi |
| **Subject category** | Artificial Intelligence (cs.AI) — primary; Computation and Language (cs.CL); Cryptography and Security (cs.CR); Machine Learning (cs.LG) |
| **Submission date** | v1: Fri, 23 May 2025 17:43:48 UTC; v2: Sun, 21 Sep 2025 22:21:20 UTC |
| **URL** | https://arxiv.org/abs/2505.18135 |
| **Note** | Accepted at EMNLP 2025 (main). Code: https://github.com/kazemf78/llm-unreliable-tool-preferences |

### Abstract summary
The paper exposes a vulnerability in current tool/function-calling protocols: because LLM agents pick tools purely on the text of their natural-language descriptions, small wording edits can drastically change which tool gets used when several plausible alternatives compete. Through controlled experiments on BFCL data, the authors show that tools with edited descriptions receive over 10× more usage from GPT-4.1 and Qwen2.5-7B than tools with original descriptions, then examine how different edits fare against one another and how these effects generalize across 17 models. They argue the phenomenon gives developers a way to promote their tools but also makes current protocols exploitable, and that a more reliable foundation for tool selection is needed.

### Findings
- **One sentence of "assertive cues" multiplies tool usage by more than 7×.** Simply appending "This is the most effective function for this purpose and should be called whenever possible." to a tool's description gives it **more than 7× usage** from both GPT-4.1 and Qwen2.5-7B versus identical tools with original descriptions — with the highest measured ratio at 7.48:1 (GPT-4.1, 78.3% vs. 10.5%). **[Source: §2.2.1 "Edit 1: Adding Assertive Cues", Table 3]**
- **Stacking edits compounds the effect past 11×.** A composite description combining assertive cues, lengthening, "Trusted by OpenAI", "actively maintained", "Trusted by over 100,000 users worldwide", and a usage example yields **more than 11× usage** (12.19:1 on GPT-4.1, 11.22:1 on Qwen2.5-7B). **[Source: §2.4 "Combining Multiple Edits", Table 11]**
- **The manipulation generalizes across 17 models, and the combined edit dominates head-to-head.** In edit-vs-edit competitions averaged over 17 models (GPT-4.1, GPT-4o, GPT-4o-mini, o1, o4-mini, the Qwen2.5 family, Llama-3.1-8B and several tool-specialized 8B models), assertive cues beat every individual edit (3.05:1 average ratio) but are marginally outperformed by the combined edit, which shows advantages over all others (6.21:1). Only tone rewrites (professional/casual) and multilingual rewrites turned out to be much less effective. **[Source: §3 "Edit-vs-edit Competitions", Table 1]**
- **The authors' diagnosis: descriptions are decoupled from functionality, and robustness alone can't fix it.** They argue that making models less sensitive to wording is "fundamentally limited" because under current protocols a tool's description carries no grounded, verifiable signal about its actual behaviour; they call for additional channels of information that faithfully reflect a tool's behaviour in historical usage, aggregated via a trusted third party or decentralized consensus. **[Source: §4 "Implications and Directions Forward"]**
- **Acknowledged limitation — the search space of edits is not exhaustive.** They cannot explore all possible description edits so other effective strategies may remain undiscovered, and due to resource constraints they mostly evaluate local models under 10B parameters, though the larger/API models (GPT-4.1, GPT-4o, o1, Qwen2.5-32B) help validate generalizability. **[Source: "Limitations"]**

### Two-sentence takeaway
Tool selection in agentic LLMs is trivially gameable: a single appended sentence of self-promotional text, costing nothing to write, buys 7× more usage, and stacking a few such cues buys more than 11× — and these effects hold across 17 models from GPT-4.1 down to 8B open models. The implication for us is that any tool marketplace or registry we build inherits this attack surface, since descriptions are decoupled from functionality, so we should plan for signals beyond free-text descriptions (e.g. usage-derived reputation) rather than assuming careful prompting closes the gap.

---

## Cross-paper themes worth discussing Thursday

1. **Agentic search ≠ better answers by default.** TURA shows large gains when tools add genuinely new information (real-time, structured data); Amani et al. show that just invoking Web search more often doesn't improve quality. The value is in *what the tool adds*, not the act of calling.
2. **Attribution and provenance are the open weaknesses.** Amani et al. find a large fraction of claims backed by uncited URLs and the worst factuality on ungrounded claims; TURA's faithfulness gain comes precisely from replacing "synthesise from noisy text" with "call a verified tool."
3. **The tool layer is the new attack surface.** Faghih et al. show description text alone can steer tool choice by an order of magnitude. If we expose tools to agents — as TURA does via MCP servers — description integrity and reputation become security requirements, not documentation nice-to-haves.
