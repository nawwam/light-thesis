# RQ2 post-retrieval (judge, retry, abstain) and agentic RAG: audit of reading-list sections 5 and 7

Citation counts below are all from the Semantic Scholar Graph API batch endpoint (`/graph/v1/paper/batch`, looked up by arXiv ID), retrieved 2026-10-22. Google Scholar counts are usually higher (often 1.3 to 2 times), so cite the source and date next to any number. Semantic Scholar's `venue` field was used as a hint only. Venues were checked against ACL Anthology, OpenReview or the authors' pages where that was possible, and each entry says how it was checked.

## Key question 1: Are the venues correct, and what are the real citation counts?

### Takeaway
Most entries are correct. Three changes are needed:
- CRAG has no confirmed venue. Cite it as an arXiv preprint.
- Search-R1's COLM 2025 venue could not be confirmed.
- The "Citations" column understates several papers. Self-RAG has about 2.9k, FLARE about 0.9k, the Singh survey about 460 (not "New"), and Search-R1 about 1.7k (not "New").

### Cited Findings
Current list entries (Semantic Scholar counts, 2026-10-22):

| Paper (list section) | Venue in list | Verified venue | S2 citations | List says | Fix |
|---|---|---|---|---|---|
| CRAG, Yan, Gu, Zhu, Ling (sec. 5), arXiv:2401.15884 | arXiv | Semantic Scholar shows arXiv only. arXiv v3 is from 7 Oct 2024. A survey cites an OpenReview forum (id JnWJbrnaUE), but its status could not be read because OpenReview returned a bot-challenge page. One secondary wiki says it was submitted to ICLR 2025 and withdrawn (unverified). | 339 | 100+ | Keep "arXiv"; label "300+" |
| Sufficient Context, Joren et al. (sec. 5), arXiv:2411.06037 | ICLR 2025 | ICLR 2025 (S2 venue; official repo calls it the "ICLR 2025 paper") | 121 | 100+ | OK |
| Self-RAG, Asai et al. (sec. 5), arXiv:2310.11511 | ICLR 2024 oral | ICLR (S2). The oral status was not re-checked here. | 2,911 | 1k+ | OK (or "2.9k") |
| FLARE, Jiang et al. (sec. 5), arXiv:2305.06983 | EMNLP 2023 | EMNLP 2023 (S2) | 929 | 100+ | Change to "~900" |
| DRAGIN, Su et al. (sec. 5), arXiv:2403.10081 | ACL 2024 | ACL 2024 (S2). The full title says "Real-time Information Needs", not "Information Needs" as in the list. | 109 | 100+ | Fix the title |
| Agentic RAG survey, Singh et al. (sec. 7), arXiv:2501.09136 | arXiv | arXiv only (S2) | 463 | New | Change to "400+" |
| ReAct, Yao et al. (sec. 7), arXiv:2210.03629 | ICLR 2023 | ICLR 2023 (S2) | 12,909 | 1k+ | Change to "10k+" |
| IRCoT, Trivedi et al. (sec. 7), arXiv:2212.10509 | ACL 2023 | ACL 2023 long, Anthology 2023.acl-long.557 | 1,443 | 100+ | Change to "1k+" |
| Search-R1, Jin, Zeng, Yue, Wang, Zamani, Han (sec. 7), arXiv:2503.09516 | COLM 2025 | Not confirmed. S2 shows arXiv only. The arXiv comment field says just "31 pages". A UMass CIIR news item mentions an RL search-agent paper "presented at COLM 2025" but does not name it. | 1,676 | New | Change to "1k+"; check the COLM 2025 program on OpenReview before citing it as COLM |
| Garbled "g class" entry (sec. 7) | Findings ACL 2026 | Findings of ACL 2026, pp. 1570–1588 (see key question 2) | not found in S2 batch (no arXiv ID found) | New | Fix the title |

Sources:
- CRAG: [arXiv 2401.15884](https://arxiv.org/abs/2401.15884); [Semantic Scholar](https://www.semanticscholar.org/paper/Corrective-Retrieval-Augmented-Generation-Yan-Gu/5bbc2b5aa6c63c6a2cfccf095d6020b063ad47ac); [OpenReview forum JnWJbrnaUE](https://openreview.net/forum?id=JnWJbrnaUE) (status not readable); [aiwiki entry claiming the ICLR 2025 withdrawal](https://aiwiki.ai/wiki/corrective_rag) (secondary, unverified).
- CRAG is still cited as an arXiv preprint in a 2026 open-source reproduction study: [arXiv 2603.16169](https://arxiv.org/pdf/2603.16169).
- Sufficient Context ICLR 2025: [GitHub hljoren/sufficientcontext](https://github.com/hljoren/sufficientcontext). It reports that using sufficiency in selective generation raises the accuracy of answered questions by up to 10 percentage points.
- IRCoT Anthology ID 2023.acl-long.557: from the externalIds returned by the Semantic Scholar API.
- Search-R1 authors: [DBLP](https://dblp.org/pid/279/6463). arXiv comments: [arXiv 2503.09516](https://arxiv.org/abs/2503.09516). Unnamed COLM mention: [CIIR news](https://ciir.cs.umass.edu/node/854).

### Inferences
- The user ranks papers by venue and citations. On that basis, CRAG is the weakest "Core" paper in section 5: it has no confirmed venue, although 339 citations show strong influence. Keep it as Core, because it is the closest prior design to RQ2's "grade, then fall back", but write "arXiv preprint (2024)" in the thesis.
- Search-R1 and the Singh survey are no longer "New" by any measure. Both are among the most cited 2025 RAG papers.

### Gaps
- Two statuses remain unconfirmed: CRAG's OpenReview status (accepted, rejected or withdrawn) and Search-R1's COLM acceptance. OpenReview's API and forum pages returned a challenge (HTTP 403), and DBLP search returned no parsable response from this machine. Resolve both by opening the pages in a browser.
- The ICLR 2024 oral status of Self-RAG was not re-verified in this session.

## Key question 2: What is the garbled Findings of ACL 2026 agentic-RAG data/benchmark paper?

### Takeaway
It is almost certainly "Data-Centric Perspectives on Agentic Retrieval-Augmented Generation: A Survey" by Jingwen Deng, Jihao Huang, Zhen Hao Wong, Hao Liang, Quanqing Xu, Bin Cui and Wentao Zhang. It appeared in Findings of ACL 2026, pp. 1570–1588 (DOI 10.18653/v1/2026.findings-acl.78). Its abstract matches the list's "why read it" text: it catalogs training datasets and benchmarks for agentic RAG.

### Cited Findings
- Abstract summary: it gives a data-centric overview along the data lifecycle: collection, preprocessing and task formulation, task construction, evaluation data, and training enhancement. It "catalogs training datasets and benchmarks across domains such as question answering, web, and software engineering". The companion GitHub project is Awesome-AgenticRAG-Data. — [ACL Anthology 2026.findings-acl.78](https://aclanthology.org/2026.findings-acl.78/)
- The Anthology page lists no arXiv link, so no citation count was found. — [ACL Anthology](https://aclanthology.org/2026.findings-acl.78/)
- Another agentic-RAG data paper appeared in the same Findings volume: "AgenticRAGTracer: A Hop-Aware Benchmark for Diagnosing Multi-Step Retrieval Reasoning in Agentic RAG" (pp. 1301–1335). It is a benchmark, not a catalogue, so it fits the list's description less well. — [ACL Anthology 2026.findings-acl.66](https://aclanthology.org/2026.findings-acl.66/)

### Inferences
- "g class" is probably what is left of a broken text box or citation chip. Replace the entry with the Deng et al. title.
- Priority: keep "Ref" or drop it. A data catalogue for training agentic RAG models is far from a one-retry, training-free RQ2.

### Gaps
- The survey's citation count could not be retrieved because no arXiv ID or S2 record was found in this session.

## Key question 3: Which important abstention, sufficiency and retry papers are missing for RQ2?

### Takeaway
Section 5 covers the "judge" side (CRAG, Sufficient Context) and the "when to retrieve" side (Self-RAG, FLARE, DRAGIN). It lacks five things:
1. Abstention and unanswerability evaluation: UAEval4RAG (ACL 2025), Know Your Limits (TACL), AbstentionBench (NeurIPS 2025), Kamath et al. (ACL 2020), and Over-Searching (EACL 2026).
2. Robustness to bad retrieved context: Yoran et al. (ICLR 2024), Lost in the Middle (TACL), and Chain-of-Note (EMNLP 2024).
3. Fixing the query after a failed retrieval: Rewrite-Retrieve-Read (EMNLP 2023).
4. Peer-reviewed 2026 "sufficiency judge, then retrieve again or stop" work: S2G-RAG (ACL 2026), plus very recent arXiv papers RINSE and "When Should Multi-Round RAG Stop?".
5. A filtering or compression baseline for the "refine" action: RECOMP (ICLR 2024).

### Cited Findings
Candidate additions. Citations are from Semantic Scholar on 2026-10-22. "Venue source" says how the venue was checked.

**A. Abstention, answerability and selective prediction (section 5)**

| Paper | Venue (venue source) | S2 cites | Relevance to RQ2 | Priority, reason |
|---|---|---|---|---|
| Unanswerability Evaluation for Retrieval Augmented Generation (UAEval4RAG), arXiv:2412.12300 | ACL 2025 long ([Anthology PDF 2025.acl-long.415](https://aclanthology.org/2025.acl-long.415.pdf)) | 14 | Builds unanswerable requests for a given corpus and gives metrics for RAG rejection. It notes that rejections often come from failed retrieval, not from understanding that the request cannot be answered. Directly usable for building a "no answer in the documents" test set on humanitarian corpora. | **Core**: peer-reviewed and the closest evaluation protocol for the abstain branch, despite few citations |
| Know Your Limits: A Survey of Abstention in Large Language Models (Wen et al.), arXiv:2407.18418 | TACL (S2 venue) | 180 | Survey of abstention: definitions, methods, metrics. Gives vocabulary and metrics such as abstention precision and recall for the abstain branch. | **Skim**: framing and metrics |
| AbstentionBench: Reasoning LLMs Fail on Unanswerable Questions, arXiv:2506.09038 | NeurIPS 2025 (S2 venue) | 172 | Benchmark showing that reasoning models abstain poorly on unanswerable questions. Supports the motivation that the generator alone will not abstain reliably. | **Skim** |
| Selective Question Answering under Domain Shift (Kamath, Jia, Liang), arXiv:2006.09462 | ACL 2020 (Anthology 2020.acl-main.503) | 335 | Classic selective QA: a calibrator decides when to abstain, evaluated with risk-coverage curves and AUC. That is the standard way to report an answer/abstain trade-off. Older than 2023 but foundational for the abstention metric. | **Ref**: cite for the risk-coverage method |
| Over-Searching in Search-Augmented Large Language Models (Xie, Gopinath, Qiu, Lin, Sun, Potdar, Dhingra) | EACL 2026 long, pp. 7714–7739 ([Anthology 2026.eacl-long.361](https://aclanthology.org/2026.eacl-long.361/)) | not retrieved | A search snippet says it finds that search improves accuracy on answerable queries but harms abstention on unanswerable ones. That is directly relevant to "retry or abstain". | **Skim**: peer-reviewed, 2026, a direct warning about retrying when the answer is absent. The snippet claim should be confirmed in the PDF. |
| Measuring and Enhancing Trustworthiness of LLMs in RAG through Grounded Attributions and Learning to Refuse (Trust-Align / Trust-Score), arXiv:2409.11242 | ICLR 2025 (S2 venue) | 44 | Defines a metric that includes refusing when the documents do not support an answer, and trains models to refuse. | **Ref** |
| Abstain-R1: Calibrated Abstention and Post-Refusal Clarification via Verifiable RL, arXiv:2604.17073 | Venue unconfirmed. A search summary claimed Findings ACL 2026, but only the arXiv page was found. | not retrieved | A 3B model trained with RL to abstain and then say what is missing. Training-based, so further from RQ2. | **Optional / Ref** if the venue is confirmed |
| Prompt-Based Abstention Fails Under Misleading Context: A Controlled Study of Small Frozen RAG Models, arXiv:2608.22228 | arXiv 2026 | 3 | Its title says prompt-only abstention fails under misleading context in small frozen RAG models, which is relevant if mmore uses a small frozen generator. Only the reference list was seen, so the findings are unverified. | **Skip unless needed** (arXiv, 3 cites) |

**B. Robustness to irrelevant or noisy retrieved context, and filtering (section 5, or a new "refine" sub-group)**

| Paper | Venue | S2 cites | Relevance | Priority |
|---|---|---|---|---|
| Making Retrieval-Augmented Language Models Robust to Irrelevant Context (Yoran et al.), arXiv:2310.01558 | ICLR 2024 (S2) | 510 | Uses an NLI model to detect irrelevant retrieved context and fall back to no-retrieval. Essentially a binary post-retrieval judge with a fallback. | **Core**: same "judge, then fall back" pattern as CRAG, and peer-reviewed |
| Lost in the Middle: How Language Models Use Long Contexts (Liu et al.), arXiv:2307.03172 | TACL 2024 (Anthology 2024.tacl-1.9) | 5,613 | Answer quality depends on where the evidence sits in the context. Explains why "retrieve more" (one possible retry) can fail. | **Skim**: very widely cited. It may already belong in the foundations section, which is covered by another researcher; check for duplicates. |
| Large Language Models Can Be Easily Distracted by Irrelevant Context (Shi et al.), arXiv:2302.00093 | ICML 2023 (S2) | 1,322 | Irrelevant context lowers accuracy. Motivates judging context before generating. | **Ref** |
| Benchmarking Large Language Models in Retrieval-Augmented Generation (RGB; Chen et al.), arXiv:2309.01431 | AAAI 2024 (S2) | 717 | Includes a "negative rejection" test bed: does the model refuse when no retrieved document holds the answer? | **Skim**: ready-made abstention evaluation dimension. It may already be in the list's evaluation section; check. |
| Chain-of-Note (Yu et al.), arXiv:2311.09210 | EMNLP 2024 main (Anthology 2024.emnlp-main.813) | 256 | Writes reading notes per document, then answers "unknown" when the documents are irrelevant. A prompt-level judge-and-abstain baseline. | **Skim**: cheap baseline for RQ2 |
| RECOMP (Xu, Shi, Choi), arXiv:2310.04408 | ICLR 2024 (S2) | 337 | Compresses retrieved documents and can output an empty summary when they do not help ("selective augmentation"). | **Ref**: a "refine" action alternative |
| Learning to Filter Context for RAG (FILCO), arXiv:2311.08377 | arXiv (S2 shows arXiv only) | 94 | Filters context at the sentence level. | **Skip**: RECOMP covers this and has a venue |
| Astute RAG, arXiv:2410.07176 | ACL 2025 (S2 venue) | 84 | Resolves conflicts between retrieved passages and the model's internal knowledge. | **Skip / Ref**: about knowledge conflict, not retry |

**C. Retrying after failed retrieval, and query rewriting (section 5)**

| Paper | Venue | S2 cites | Relevance | Priority |
|---|---|---|---|---|
| Query Rewriting in Retrieval-Augmented LLMs, "Rewrite-Retrieve-Read" (Ma, Gong, He, Zhao, Duan), arXiv:2305.14283 | EMNLP 2023 main, Anthology 2023.emnlp-main.322 ([ACL Anthology](https://aclanthology.org/2023.emnlp-main.322)). The arXiv title says "for" where the Anthology title says "in". | 320 | Rewriting the query is the most common "other configuration" for a second retrieval. | **Core** if one RQ2 retry option is a reformulated query; otherwise Skim |
| Iter-RetGen: Enhancing RAG with Iterative Retrieval-Generation Synergy (Shao et al.), arXiv:2305.15294 | Findings of EMNLP 2023 (S2 says "EMNLP"; Findings not verified) | 605 | Uses the first draft answer as the query for a second retrieval. A natural one-retry baseline. | **Skim** |
| S2G-RAG: Structured Sufficiency and Gap Judging for Iterative Retrieval-Augmented QA (Li, Zou, Lv, Zhang, Zhou), arXiv:2604.23783 | ACL 2026 long, Anthology 2026.acl-long.1185, pp. 25846–25862 ([Anthology](https://aclanthology.org/2026.acl-long.1185/)) | 14 | A judge decides whether the gathered evidence suffices. If not, it outputs structured "gap items" that become the next query. On HotpotQA the judge declares evidence sufficient when it is not in 6.44% of cases, and rejects 31.60% of contexts that are in fact sufficient ([arXiv](https://arxiv.org/pdf/2604.23783), via search summary). | **Core**: the most recent peer-reviewed paper on the sufficiency-judge plus retry pattern |
| Relevance Is Not Sufficient Evidence: Detecting Evidence Gaps Before Generation in RAG (RINSE), arXiv:2609.37469 | arXiv, Sep 2026 | 1 | Treats the post-retrieval decision as a choice between answer, retrieve again, widen to web, or abstain. Search summary: sufficiency can be judged without generating an answer, but no single signal works on every dataset. | **Core to discuss as concurrent work**: nearly identical to RQ2's decision space, though arXiv-only with 1 citation. Place it in "Closest work". |
| When Should Multi-Round RAG Stop? Structured Stopping Judgments and Retrieval Reduction in Search-R1, arXiv:2608.13237 | arXiv, Aug 2026 | 1 | Applies an S2G-style stopping judge to a frozen Search-R1. It cuts retrieval calls by 3.70% (77 calls) for a 0.625-point drop in exact match ([arXiv](https://arxiv.org/abs/2608.13237), via search summary). | **Ref**: shows the small savings available from stopping early |
| SeaKR: Self-aware Knowledge Retrieval for Adaptive RAG, arXiv:2406.19215 | ACL 2024 (S2 venue) | 68 | Re-retrieves and re-ranks based on the LLM's internal uncertainty. | **Ref**: overlaps with DRAGIN |
| Self-Knowledge Guided Retrieval Augmentation (SKR), arXiv:2310.05002 | Findings of EMNLP 2023 (S2 says "EMNLP") | 112 | Decides whether to retrieve at all. | **Skip**: pre-retrieval, out of scope |
| Speculative RAG, arXiv:2407.08223 | ICLR 2025 (S2) | 123 | A small model drafts answers from document subsets and a large model verifies them. Verifying drafts is a form of judging. | **Ref**: about efficiency, only loosely tied to retry |

Sources: all citation counts and S2 venues come from the Semantic Scholar batch API on 2026-10-22, queried with the arXiv IDs above. Search summaries for the 2026 papers: [S2G-RAG arXiv](https://arxiv.org/abs/2604.23783); [RINSE arXiv](https://arxiv.org/html/2609.37469v1); [Multi-round stop arXiv](https://arxiv.org/abs/2608.13237); [Abstain-R1 arXiv](https://arxiv.org/pdf/2604.17073).

### Inferences
- The best additions for an RQ2 "judge, then retry or abstain" chapter are:
  - Yoran et al. (ICLR 2024): NLI judge with fallback.
  - S2G-RAG (ACL 2026): sufficiency judge with targeted re-query.
  - UAEval4RAG (ACL 2025): an unanswerability evaluation protocol for RAG.
  - Rewrite-Retrieve-Read (EMNLP 2023): the canonical retry action.
  - RINSE (arXiv 2026): flag it as concurrent work that the thesis must position against, ideally in "Closest work".
- For metrics, Kamath et al.'s risk-coverage reporting and Sufficient Context's selective-generation curves give a principled way to report the abstain trade-off. Inferred from the papers' known methods; check their evaluation sections.
- The Over-Searching (EACL 2026) finding, if it holds, is directly relevant: a retry can make abstention worse. Report how often the retry turns a correct abstention into a wrong answer.
- Possible removal: DRAGIN (Ref, 109 cites). Like FLARE, it decides retrieval during generation, which differs from RQ2's single post-retrieval check. Keep FLARE as the representative of that line (929 cites) and drop DRAGIN, or keep DRAGIN only if it serves as an experimental baseline.

### Gaps
- No peer-reviewed paper was found that switches between different retrieval configurations after a judge rejects the first result (for example sparse to dense, or text to page-image). CRAG falls back to web search, and S2G-RAG, Rewrite-Retrieve-Read and Iter-RetGen change the query, not the retriever. Only the arXiv "Closest work" papers already on the list (Li & Gai; Li et al.) come close. This gap supports the novelty claim, with the caveat that the search covered about ten queries, not a systematic review.
- The Over-Searching abstract's numerical results and the Prompt-Based Abstention findings were not read in full.
- Iter-RetGen and SKR: whether they appeared in the main EMNLP proceedings or in Findings was not verified.
- Self-Routing RAG and Probing-RAG were not checked.

## Key question 4: Is the agentic section the right size given that RQ2 is only one retry?

### Takeaway
Yes, if anything it is slightly too large. RQ2 needs the loop concept (ReAct, IRCoT), one survey, and one training-based reference point (Search-R1). The data-centric catalogue (the "g class" entry) can go. Deep-research benchmarks such as BrowseComp should be at most a one-line mention.

### Cited Findings
Agentic and deep-research candidates (S2, 2026-10-22):

| Paper | Venue | S2 cites | Relevance | Recommendation |
|---|---|---|---|---|
| Self-Ask / "Measuring and Narrowing the Compositionality Gap" (Press et al.), arXiv:2210.03350 | Findings of EMNLP 2023 (S2 says "EMNLP"; Findings not verified) | 1,557 | Decomposes a question into follow-up search queries. | **Ref**: IRCoT already covers this |
| Toolformer (Schick et al.), arXiv:2302.04761 | NeurIPS 2023 (S2) | 6,272 | The model learns when to call tools, including search. | **Ref** at most. It is foundational for tool use, but RQ2 does not train tool use. |
| Search-o1, arXiv:2501.05366 | EMNLP 2025 (S2) | 495 | A reasoning model that calls search during reasoning. Its "Reason-in-Documents" step refines the retrieved documents. | **Skip / Ref** |
| R1-Searcher, arXiv:2503.05592 | arXiv (S2) | 313 | An RL-trained search agent, similar to Search-R1. | **Skip**: redundant with Search-R1 |
| DeepResearcher, arXiv:2504.03160 | EMNLP 2025 (S2) | 304 | RL training of deep-research agents on the real web. | **Skip** |
| BrowseComp (OpenAI), arXiv:2504.12516 | arXiv (S2) | 674 | Benchmark for browsing agents. | **Ref, one line**: shows where heavy agentic search is needed, which is not RQ2's setting |
| BrowseComp-Plus, arXiv:2508.06600 | arXiv (S2) | 226 | The same idea with a fixed corpus, which makes it fairer and reproducible. | **Skip** |
| DeepResearch Bench, arXiv:2506.11763 | arXiv (S2) | 268 | Deep-research agent benchmark. | **Skip** |
| Data-Centric Perspectives on Agentic RAG: A Survey (Deng et al.) | Findings ACL 2026 | n/a | Data catalogue. | **Remove**, or keep as Ref |
| AgenticRAGTracer | Findings ACL 2026 | n/a | Hop-level diagnostic benchmark. | **Skip** |

- The list's own section 7 note says iterative RAG uses about 3.5 times the tokens of no-retrieval on RAGRouter-Bench, against 1.4 times for single-pass RAG. That supports keeping the agentic loop out of scope and using a one-retry budget. — reading-list PDF, section 7 (local file).
- In the S2G-style stopping study on Search-R1, early stopping saved only 3.70% of retrieval calls for a 0.625-point drop in exact match. Agentic loops are hard to make cheaper after the fact. — [arXiv 2608.13237](https://arxiv.org/abs/2608.13237)

### Inferences
- Suggested final section 7, five papers:
  - Singh survey (Core).
  - ReAct (Core).
  - IRCoT (Skim, upgrade the citation label to 1k+).
  - Search-R1 (Skim, venue to be verified).
  - One deep-research benchmark as Ref (BrowseComp), only if the thesis discusses where agentic search is warranted.
- Drop the Deng et al. data survey, or move it to Ref with the corrected title.
- Do not add R1-Searcher, DeepResearcher or Search-o1. They are redundant for a thesis whose RQ2 is training-free and allows one retry.
- S2G-RAG and "When Should Multi-Round RAG Stop?" act as the bridge between section 5 and section 7. Place S2G-RAG in section 5, since its judge is the RQ2 mechanism, and cross-reference it from section 7.

### Gaps
- Citation counts for the 2026 Findings and EACL papers were not available from Semantic Scholar's arXiv lookup.
- Google Scholar was not queried: no API, and scraping is unreliable. All counts come from Semantic Scholar only.
