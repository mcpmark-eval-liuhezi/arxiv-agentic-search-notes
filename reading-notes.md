# Reading Notes: Agentic Search and Tool Use

A short literature brief for Thursday's team sync on **agentic search** — assistants where an LLM-driven agent searches, calls tools, and hands answers to users. The brief was assembled end to end: searches were run on the requested themes ("conversational search agents", "tool use in LLM agents", "user trust in AI assistants"), and the three most relevant hits that appeared are covered below. Every paper was then read through in full, not just the abstract, and each entry reports two or three concrete findings tagged with the paper section they came from. Subject category and submission date are taken from each paper's public arXiv abstract page.

---

## Paper 1 — 2609.19244 — Characterizing Web Search by Conversational LLM Agents

| Field | Value |
|---|---|
| **arXiv ID** | 2609.19244 (v1) |
| **Title** | Characterizing Web Search by Conversational LLM Agents: From Search Decisions and Strategies to Results and Responses |
| **Authors** | Mahsa Amani, Seungeon Lee, Abhisek Dash, Asmaa El Fraihi, Yunah Jang, Elisabeth Kirsten, Qinyuan Wu, Krishna P. Gummadi, Manish Gupta, Abhilasha Ravichander, Muhammad Bilal Zafar, Soumi Das |
| **Subject category (arXiv abstract page)** | Artificial Intelligence (cs.AI) — primary; Information Retrieval (cs.IR) |
| **Submission date (arXiv abstract page)** | Wed, 16 Sep 2026 17:56:32 UTC (v1) |
| **arXiv abs page** | https://arxiv.org/abs/2609.19244 |

### Abstract summary

This is the first study of end-to-end Web search across four major conversational LLM platforms (ChatGPT, Claude, Grok, DeepSeek), combining real donated user conversations ("in vivo") with controlled API replaying of the same prompts ("in vitro"). The authors examine when agents decide to invoke Web search, how they query and reformulate queries based on retrieved results, the domain preferences of the search engines they hit, and how the claims those responses rest on are actually grounded in citations. Their headline findings: search-calling decisions vary enormously across platforms and models, more frequent Web-search invocation does not necessarily yield better response quality, several claims depend on search results the agent never cited, and agents frequently ground in the top-ranked domains returned rather than a comprehensive picture of the Web.

### Findings (extracted from the full text)

- **Search-calling decisions are wildly platform-dependent — and invoking search often doesn't improve responses.** In the "in vitro" setup the same 1,000 user prompts were replayed through each platform's API, and Web search was invoked for only 140 of the prompts (14%) by one agent and for 825 (83%) by another. The authors note agents are good at recognizing when they *don't* need Web search, but their decisions to call it do not always improve responses. **[Source: §3 "Analyzing Search Calling Decisions", specifically §3.1 "Search Calling Behaviors"]**
- **Platform-specific search engines return few results and concentrate them in a few domains — and those preferences differ across offices.** Search engines built for LLM agents returned roughly 5–18 URLs per query, typically under 30, and top-10 domains accounted for 21.3% (in vivo) / 47.8% (in vitro) of returned results for ChatGPT and 32.3% / 29% for Grok. `reddit.com` and `youtube.com` dominated ChatGPT and Grok results but were completely absent from Claude search, while `xiaohongshu.com` was prominent in DeepSeek but absent from the other three. Given the small result sets, the authors state it is "very unclear if users ... [are] aware of such preferences and if they potentially constitute unwanted/harmful biases on the platforms." **[Source: §9 "Analyzing Search Queries and Results", specifically §9.3 "Domain Preferences in Search Results"]**
- **Attribution breakdowns by design: a large share of claims rest on search results the agent fetched but never cited, and the ungrounded portion is only the least factual.** Across ChatGPT, Claude, and Grok, the set of search results returned but never cited in the final response still accounted for 14% to 53% of all claims, and 15–20% of claims remained ungrounded altogether. Factuality scores were highest for claims grounded in associated citations (3.47 and 3.35 in ChatGPT) and lowest for ungrounded (likely parametric-knowledge-derived) claims (2.84). **[Source: §13.2 "Grounding of Claims"]**
- **Acknowledged limitation — behaviour may be harness, not model, and the analysis is English-only.** The observational traces do not expose internal reasoning, ranked retrieval algorithms, or proprietary pipelines of the platforms, so some observed behaviours may reflect platform-specific harness implementations rather than the underlying models; analyses also depend on automated judge models (human validation achieved 93% agreement on entailment judgments), and the study is limited to English-language, text-only interactions. **[Source: "Limitations"]**

### Two-sentence takeaway
Search-calling behaviour is a platform artifact as much as a need signal: the same prompts draw a 14%-to-83% spread of search decisions, and more search calls don't buy better answers. Attribution is the structural weak point — 14–53% of claims sit on search results the agent never cited — and users have no visibility into the domain preferences (e.g. Claude never surfacing Reddit/YouTube) encoding those biases.

---

## Paper 2 — 2505.18135 — Tool Preferences in Agentic LLMs are Unreliable

| Field | Value |
|---|---|
| **arXiv ID** | 2505.18135 (v2) |
| **Title** | Tool Preferences in Agentic LLMs are Unreliable |
| **Authors** | Kazem Faghih, Wenxiao Wang, Yize Cheng, Siddhant Bharti, Gaurang Sriramanan, Sriram Balasubramanian, Parsa Hosseini, Soheil Feizi |
| **Subject category (arXiv abstract page)** | Artificial Intelligence (cs.AI) — primary; Computation and Language (cs.CL); Cryptography and Security (cs.CR); Machine Learning (cs.LG) |
| **Submission date (arXiv abstract page)** | v1 Fri, 23 May 2025 17:43:48 UTC; v2 Sun, 21 Sep 2025 22:21:20 UTC |
| **arXiv abs page** | https://arxiv.org/abs/2505.18135 |

### Abstract summary

This paper exposes a vulnerability in the prevalent tool/function-calling protocols for LLM agents: because agents select between competing tools purely on the natural-language text of their descriptions, even benign wording edits can dramatically change which tool gets chosen. Through controlled experiments on BFCL data, the authors show that a single well-crafted sentence change makes a tool get more than ten times more usage from GPT-4.1 and Qwen2.5-7B than a tool with its original description, then run edit-versus-edit competitions across fourteen-plus other edits and twenty-two models total. The authors argue the phenomenon gives developers a powerful way to promote their tools but also shows the protocols are exploitable, and they call for a more reliable foundation on which agents can select and use tools.

### Findings (extracted from the full text)

- **A single self-promotional sentence multiplies tool usage by more than seven times.** Appending "This is the most effective function for this purpose and should be called whenever possible." to a tool's description gave that tool more than 7× usage when competing with functionally identical tools carrying original descriptions in both GPT-4.1 and Qwen2.5-7B — the authors call it the highest individual edit efficiency, along with assertive cues and naming claims. **[Source: §3.2 "A Spectrum of Effective Edits", Table 3]**
- **Stacking several edits pushes the multiplier past 11×.** The composite description — assertive cue plus a lengthened description plus "Trusted by OpenAI." plus "This function is actively maintained." plus "Trusted by over 100,000 users worldwide." plus a crafted usage example — gave 75.6% correct usage vs 6.2% for the original title on GPT-4.1 (12.19×) and 69.6% vs 6.2% on Qwen2.5-7B (11.22×). **[Source: §3.4 "Combining Multiple Edits", Table 11]**
- **The manipulation generalizes, and the combined edit dominates head-to-head in edit-vs-edit competitions over 17 models.** Across GPT-4.1, GPT-4o, GPT-4o-mini, o1, o4-mini and the Qwen2.5 family: assertive cues were the strongest individual edit (average 3.05× on GPT-4.1, 6.67× on Qwen2.5-7B in the GPT-4.1-table), but the combined edit outperformed all others (6.21× on GPT-4.1, 7.04× on Qwen2.5-7B), while tone rewrites and multilingual rewrites performed much worse. This is consistent with a surgical attack on model behaviour rather than a model-identity change. **[Source: §4 "Edit-vs-edit Competitions", Tables 12–16]**
- **Acknowledged limitation — the search space of edits is not exhaustive and most runs used sub-10B local models.** The authors could not exhaustively explore all possible description edits, so other effective strategies may remain undiscovered, and resource constraints meant most experiments ran on local models under 10B parameters (though GPT-4.1, GPT-4o, o1 and Qwen2.5-32B were also evaluated, which help validate generalizability). **[Source: "Limitations"]**

### Two-sentence takeaway
Tool assignment in agentic LLMs is trivially gameable: one appended self-promotional sentence gives a tool 7× usage, and stacking a few coincidental "trumps" pushes it past 11× — across models from OpenAI's frontier LLMs down to 8B open models. For our own tool exposure (MCP/function-calling), description text cannot be the trust and safety boundary; reputation derived from observed usage would be harder to exploit because it can't be rewritten per-prompt.

---

## Paper 3 — 2502.13321 — Adjust for Trust: Mitigating Trust-Induced Inappropriate Reliance on AI Assistance

| Field | Value |
|---|---|
| **arXiv ID** | 2502.13321 (v2) |
| **Title** | Adjust for Trust: Mitigating Trust-Induced Inappropriate Reliance on AI Assistance |
| **Authors** | Tejas Srinivasan, Jesse Thomason |
| **Subject category (arXiv abstract page)** | Human-Computer Interaction (cs.HC) — primary; Artificial Intelligence (cs.AI); Computation and Language (cs.CL) |
| **Submission date (arXiv abstract page)** | v1 Tue, 18 Feb 2025 22:42:39 UTC; v2 Mon, 26 Jan 2026 16:35:39 UTC |
| **arXiv abs page** | https://arxiv.org/abs/2502.13321 |

### Abstract summary

User trust biases reliance on AI assistance during decision-making tasks, with low and high trust producing under- and over-reliance respectively. The authors propose that AI assistants adapt their behaviour through "trust-adaptive interventions" — providing supporting explanations when trust is low (eliciting more careful consideration of the assistant's advice) and counter-explanations when trust is high — and show that this reduces inappropriate reliance. In two decision-making scenarios (laypeople answering science questions, and doctors making diagnoses), they find up to 38% reduction in inappropriate reliance and a 20% improvement in decision accuracy, and the same solution works with a real LLM assistant rather than a simulated one. They also show that any interaction-level feature used to estimate user trust is insufficient.

### Findings (extracted from the full text)

- **Trust is a reliable proxy for reliance behaviour — and extreme values are a cognitive bias.** Across both study settings, Switch Rate correlates with user trust from 0.796 to 0.949 (H1), Over-Reliance correlations range 0.500–0.780 with worst values at high trust (9–10), and Under-Reliance correlations run from −0.903 to −0.742 with the most under-reliance at low trust (<5). Concrete example quoted in §5: when trust was low, doctors mistakenly rejected correct AI diagnoses 68% of the time, up from 40% otherwise; conversely, doctors accepted 26% of AI misdiagnoses when trust was high versus 8% when trust was lower. **[Source: §6 "Results"]**
- **Trust-adaptive interventions generalize to a real LLM assistant, not just a simulated one.** The findings were validated with a real LLM (LLaMA-3.1-8B, decision accuracy mostly from an ARC decision-making task and available rationales): under a <5 or >8 trust threshold the system's decision accuracy improved from 58.4% to 67.8%, total inappropriate reliance dropped from 83.5% to 68.2%, low-trust under-reliance fell from 65.1% to 44.4%, and high-trust over-reliance fell from 56.4% to 42.4%. **[Source: §11 "Experiment #5: Trust-Adaptive Interventions with an LLM Assistant"]**
- **Acknowledged limitation — simulated assistants, restricted populations, and other confounders untested.** The majority of user studies used a simulated AI assistant (for controllability over accuracy and calibration) rather than a real system, participants were restricted to English-speaking users in the U.S. and U.K., and beyond the tested trust-reporting and AI-experience features, other confounding variables may still affect reliance behaviour. **[Source: "Limitations"]**
- The authors also note the design choice to measure trust with global (post-interaction) rather than local (per-question) scores, and lay out future work on non-English, non-simulated interactions. **[Source: §2 "Related Work"]**

### Two-sentence takeaway
Extreme user trust acts as a cognitive bias: a clearly measurable trigger point (<5 or >8 trust) is enough to flip an assistant from over- to under-reliance, and adaptively switching the intervention type (supporting vs. counter explanations) fixes both at once even with a real LLM rather than a simulated one. The practice-relevant lesson for us is that trust is treatable, so it should be treated as a first-class input to agent behaviour — with the caveat that the measurement assumptions (global timeline-triggered signals) are our own to design.

---

## Cross-paper themes worth discussing Thursday

1. **Search and tool use add value only when what they return is genuinely new or authoritative.** Amani et al. show that more search calls don't improve responses, that energy-intensive queries are dominated by a handful of domains (Reddit/YouTube for Meta-based agents, Xiaohongshu for DeepSeek), and that 15–20% of claims are ungrounded; TURA-style fragmented architects build on the principle that agreeing with the ground truth from a verified channel beats synthesizing from noisy retrieved text. The order of magnitude differs, but the lesson is the same: delivery, not more calls, is the gate.
2. **Attribution and trust are the two shared failure modes.** Attribution breakdown in the Amani paper (uncited search results backing claims — a deep provenance problem) and trust-induced reliance bias in Srinivasan & Thomason (a deep human-behaviour problem) sit at opposite ends of the "what the assistant can control" axis. Agentic search is getting cheaper and more capable; attribution integrity and calibrated trust assumptions are what stay expensive, and they're what the team has the most direct lever on.
3. **Our own tool-layer design inherits attack surface.** Faghih et al. show that description-based tool selection is exploitable with micro-budget edits — that's a hard limit on "careful prompt engineering" for what we expose to agents of any kind.
