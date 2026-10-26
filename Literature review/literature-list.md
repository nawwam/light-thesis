# mmore thesis — reading list

Oct 22, 2026 · @Yassine

## How to use this list

44 papers, grouped by topic. The **#** column is the suggested reading order (1 = first). Ref papers have no number: know they exist and cite them.

- **Priority.** Core = read fully. Skim = abstract, figures, conclusion (about 10 minutes). Ref = cite only.
- **Venue.** Where the paper was accepted. "arXiv" = preprint, not peer reviewed. "(authors' claim)" = acceptance stated only in the arXiv comment, not yet on the proceedings page. "(not confirmed)" = taken from Semantic Scholar's label only.
- **Citations.** Semantic Scholar, 22 Oct 2026. Google Scholar is usually 1.3 to 2 times higher. "OA" = OpenAlex, used where Semantic Scholar had no record. Papers from 2026 have almost no citations yet, so for those trust the venue.
- **Metrics used below.** nDCG@k (normalized discounted cumulative gain over the top k results) is a 0–1 ranking score that rewards relevant documents placed near the top, divided by the score of a perfect ranking. Recall@k is the fraction of relevant documents found in the top k. "Oracle" means the configuration chosen in hindsight to be best for each query.

## Reading order

**Week 1: foundations.** 1 RAG (Lewis) · 2 Gao survey, sections 1 to 4 · 3 DPR · 4 ColBERT · 5 Bruch fusion functions · 6 Best Practices (Wang) · 7 BEIR · 8 MIRAGE · 9 SPLADE · 10 BGE-M3

**Week 2: closest work and the framing.** 11 RetrievalRouter · 12 Adaptive Re-Ranking · 13 Retriever Portfolios · 14 Hauff QPP survey · 15 Arabzadeh 2021 · 16 Healthier LLMs · 17 RINSE · 18 Modality Relevance

**Week 3 onwards: one block per research question.** Start with the block your oracle experiment points to.
- RQ1: 19 Adaptive-RAG · 20 Moskvoretskii · 21 LTRR · 22 Marina · 23 MoR · 24 Shtok (NQC) · 25 Faggioli · 26 Arabzadeh 2026
- RQ2: 27 Sufficient Context · 28 CRAG · 29 S2G-RAG · 30 UAEval4RAG
- RQ3: 31 ColPali · 32 OHRBench · 33 ViDoRe V3 · 34 VisRAG · 35 MIRACL-VISION · 36 Lost in OCR Translation

**Before building the test set.** 37 Thomas (LLM relevance labels) · 38 MIRACL · 39 Humanitarian situation reporting (Decostanzi)

**Background, any time.** 40 ReAct · 41 Agentic RAG survey (Singh)

## Closest work to your thesis

Per-query routing among retrieval pipelines is now published at top venues (EMNLP 2026, ICML 2026, ICTIR 2026). Your contribution therefore comes from the setting (scanned, multilingual humanitarian and global-health documents, inside mmore) and from RQ2's switch to a different configuration after a failed retrieval. No peer-reviewed paper found so far does that switch.

| # | Paper | Venue | Citations | What it shows | Priority |
|---|---|---|---|---|---|
| 11 | RetrievalRouter: Joint Modality and Architecture Selection for Document Retrieval (Kuru, Keskin, Farahbakhsh & Crespi, 2026) | EMNLP 2026 (authors' claim) | 0 | Evaluates 7 pipelines (text or page image × sparse, dense, late interaction, reranking) and routes each query among 5 of them from the query text alone. On per-query nDCG@5: oracle ≈ 0.90, best single pipeline 0.737, learned router 0.755. So the router recovers only about 2 of the 16 points of headroom. This is your reference point for the oracle experiment. It releases best-pipeline labels for 80,000+ queries (financial and scientific documents) | Core |
| 12 | Adaptive Re-Ranking (Genc, Korukluoglu & Allan, 2026) | ICTIR 2026 | 1 | Routes each query to no reranker, a small one (MiniLM) or a heavy one (BGE-v2-m3). About 40% of queries gain **nothing** from reranking, and only 11% gain from the heavy reranker. The per-query oracle beats every fixed strategy at every latency budget, and the learned router leaves a large gap to it | Core |
| 13 | Retriever Portfolios: A Principled Approach to Adaptive RAG (Stouras, Cohen-Addad, Lattanzi & Svensson, 2026) | ICML 2026 (authors' claim) | 1 | Picks a small set of k retrievers that maximises the expected score of the best one per query ("best-of-k"), then routes each query within that set. This is your oracle-vs-single-best gap written as an objective, and a principled way to shrink mmore's configuration grid. Svensson is at EPFL | Core |
| 17 | Relevance Is Not Sufficient Evidence: Detecting Evidence Gaps Before Generation in RAG (RINSE, Sep 2026) | arXiv | 1 | After retrieval, decides between answering, retrieving again, widening to the web, or abstaining: almost exactly RQ2. Sufficiency can be judged before generation, but no single signal works on every dataset. Concurrent work to position against | Core |
| 18 | Modality Relevance is not Modality Utility: Post-hoc Selective Modality Escalation for Cost-Aware Multimodal RAG (Li & Gai, 2026) | arXiv | 2 | On MultiModalQA, 61% of questions whose evidence is an image can be answered from text and tables alone. It answers cheaply first and only escalates to vision when a verifier rejects the answer. Headroom evidence for RQ3 and a post-generation variant of RQ2 | Skim |

## Earlier and adjacent work

Information retrieval has picked a strategy per query since the 2000s, under the name query performance prediction (QPP: estimating how well a query will be answered, either before retrieval from the query alone or after retrieval from the results). That pre/post split maps onto your RQ1 vs RQ2, so cite this line as the origin of your framing.

| # | Paper | Venue | Citations | What it shows | Priority |
|---|---|---|---|---|---|
| 14 | A Survey of Pre-retrieval Query Performance Predictors (Hauff, Hiemstra & de Jong, 2008) | CIKM 2008 | 58 (OA 161) | Compares predictors computed from the query alone (term rarity, specificity, ambiguity). The feature set an RQ1 router can start from | Core |
| 15 | Predicting Efficiency/Effectiveness Trade-offs for Dense vs. Sparse Retrieval Strategy Selection (Arabzadeh, Yan & Clarke, 2021) | CIKM 2021 | 62 | A query-only classifier decides per query whether to pay for dense retrieval, trading GPU cost against effectiveness on MS MARCO. The only adaptive baseline RetrievalRouter actually runs | Core |
| 24 | Predicting Query Performance by Query-Drift Estimation (Shtok, Kurland, Carmel, Raiber & Markovits, 2012) | ACM TOIS 2012 | 301 | Normalized query commitment (NQC): the standard deviation of the top-k retrieval scores, divided by a corpus score. The standard post-retrieval predictor, and you can compute it directly from mmore's dense and sparse score lists | Skim |
| 25 | Query Performance Prediction for Neural IR: Are We There Yet? (Faggioli et al., 2023) | ECIR 2023 | OA 31 | Classic QPP predictors work markedly worse on dense and learned-sparse retrievers than on BM25. A warning that the predictors above may not transfer to mmore's index, and a reason RQ1 may be hard | Skim |
| 26 | Can QPP Choose the Right Query Variant? Evaluating Query Variant Selection for RAG Pipelines (Arabzadeh, Drozdov, Bendersky & Zaharia, 2026) | SIGIR 2026 | 2 | QPP used to choose, per query, among query variants in a RAG pipeline. The variant with the best nDCG often does not give the best answer. Cheap pre-retrieval predictors often match post-retrieval ones (your RQ1 vs RQ2). Compute your oracle on both a retrieval metric and an answer metric | Skim |
| — | Automated Algorithm Selection: Survey and Perspectives (Kerschke, Hoos, Neumann & Trautmann, 2019) | Evolutionary Computation | 508 | Source of the vocabulary for your oracle gap: "virtual best solver" (best method per instance, in hindsight) vs "single best solver" (one method best on average) | Ref |

## 1. Retrieval foundations

These cover every retrieval component in mmore and your configuration grid: sparse, dense, late interaction, hybrid fusion and multilingual embeddings. Note: mmore's hybrid search is a weighted score sum (pymilvus `WeightedRanker`, weight 0.5), not Reciprocal Rank Fusion. Its default models (bge-large-en-v1.5, e5-large-v2, an English SPLADE) are English-only.

| # | Paper | Venue | Citations | Why read it | Priority |
|---|---|---|---|---|---|
| 1 | Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks (Lewis et al., 2020) | NeurIPS 2020 | 19,864 | The paper that named RAG: retrieve passages, then condition the generator on them | Core |
| 3 | Dense Passage Retrieval for Open-Domain Question Answering (Karpukhin et al., 2020) | EMNLP 2020 | 7,416 | Dense retrieval: query and passage each become one embedding, matched by nearest-neighbour search. Beats BM25 (keyword matching) on open-domain QA | Core |
| 4 | ColBERT: Efficient and Effective Passage Search via Contextualized Late Interaction over BERT (Khattab & Zaharia, 2020) | SIGIR 2020 | 2,681 | Late interaction: one vector per token, and each query token is scored against its best-matching document token. The idea ColPali applies to page images | Core |
| 5 | An Analysis of Fusion Functions for Hybrid Retrieval (Bruch, Gai & Ingber, 2023) | ACM TOIS 2023 | 133 | Compares the weighted sum of normalised scores (what mmore does) with Reciprocal Rank Fusion (sum of 1/(k + rank) across lists). Tells you whether fusion type and weight should be dimensions of your grid | Core |
| 9 | SPLADE: Sparse Lexical and Expansion Model for First Stage Ranking (Formal, Piwowarski & Clinchant, 2021) | SIGIR 2021 | 723 | Learned sparse retrieval: a model expands the query and document into weighted vocabulary terms. The family of mmore's sparse model | Skim |
| 10 | M3-Embedding (BGE-M3; Chen et al., 2024) | Findings of ACL 2024 | 2,053 | One multilingual model that outputs dense, sparse and multi-vector scores at once. The multilingual candidate you need so that "dense beats sparse on French" is not just "this checkpoint saw French" | Skim |
| — | The Probabilistic Relevance Framework: BM25 and Beyond (Robertson & Zaragoza, 2009) | Foundations and Trends in IR | 6,665 | The canonical BM25 reference | Ref |

## 2. Evaluation and benchmarks

| # | Paper | Venue | Citations | Why read it | Priority |
|---|---|---|---|---|---|
| 7 | BEIR: A Heterogenous Benchmark for Zero-shot Evaluation of Information Retrieval Models (Thakur et al., 2021) | NeurIPS 2021 Datasets & Benchmarks | 2,178 | 18 datasets. Shows BM25 stays hard to beat outside the training domain. Source of the nDCG@10 convention | Core |
| 8 | Benchmarking Retrieval-Augmented Generation for Medicine (MIRAGE / MedRAG; Xiong, Jin, Lu & Zhang, 2024) | Findings of ACL 2024 | 675 | 7,663 medical questions, 41 combinations of corpus, retriever and LLM. The template for benchmarking mmore configurations on health questions | Core |
| 16 | Healthier LLMs: Retrieval-Augmented Generation for Public Health Question Answering (Feldman et al., 2026) | arXiv | 0 | 7,929 questions on UK public-health guidance. Compares dense, sparse and hybrid retrieval: hybrid consistently improves recall and ranking. Its LLM judge is validated against human annotators. The nearest precedent for your setting (but English-only, born-digital, no routing) | Core |
| 37 | Large Language Models can Accurately Predict Searcher Preferences (Thomas, Spielman, Craswell & Mitra, 2024) | SIGIR 2024 | 313 | LLM relevance labels agree with human ones. Justifies generating the per-query relevance labels your oracle needs, with a small human-checked sample | Skim |
| 38 | MIRACL: A Multilingual Retrieval Dataset Covering 18 Diverse Languages (Zhang, Thakur et al., 2023) | TACL 2023 | 239 | The standard multilingual retrieval benchmark, French included, with native-speaker relevance judgments | Skim |
| 39 | A Large-Language-Model Framework for Automated Humanitarian Situation Reporting (Decostanzi, Mejova & Kalimeri, 2025) | Scientific Reports (not confirmed) | 2 | The closest humanitarian RAG paper: question answering over 1,100+ ReliefWeb documents covering 13 crises. The only prior RAG work on humanitarian documents found | Skim |

## 3. Surveys and best practices

| # | Paper | Venue | Citations | Why read it | Priority |
|---|---|---|---|---|---|
| 2 | Retrieval-Augmented Generation for Large Language Models: A Survey (Gao et al., 2023) | arXiv | 4,328 | The Naive / Advanced / Modular RAG taxonomy most papers reuse. Read sections 1 to 4 | Core |
| 6 | Searching for Best Practices in Retrieval-Augmented Generation (Wang et al., 2024) | EMNLP 2024 | 209 | Ablates each pipeline step one at a time. The hybrid-search-plus-reranking recommendation comes from here | Core |

## 4. RQ1: adaptive pre-retrieval (routing from the query)

| # | Paper | Venue | Citations | Why read it | Priority |
|---|---|---|---|---|---|
| 19 | Adaptive-RAG: Learning to Adapt Retrieval-Augmented LLMs through Question Complexity (Jeong et al., 2024) | NAACL 2024 | 762 | A small classifier predicts question complexity and routes to no retrieval, single-step or multi-step retrieval. The reference baseline for query routing | Core |
| 20 | Adaptive Retrieval Without Self-Knowledge? Bringing Uncertainty Back Home (Moskvoretskii et al., 2025) | ACL 2025 | 49 | Compares 35 methods on 6 datasets. Simple uncertainty estimates match complex adaptive pipelines on accuracy and beat them on cost | Core |
| 21 | LTRR: Learning To Rank Retrievers for LLMs (Kim & Diaz, 2025) | SIGIR 2025 LiveRAG workshop (spotlight) | 8 | Routing as ranking a pool of retrievers, "no retrieval" included, by expected answer quality. Routing beats the best single retriever, and a small XGBoost model works best | Core |
| 22 | LLM-Independent Adaptive RAG: Let the Question Speak for Itself (Marina et al., 2025) | EMNLP 2025 | OA 3 | 27 cheap query features, no LLM call, match LLM-uncertainty methods at much lower cost. The feature recipe for a cheap RQ1 router | Skim |
| 23 | MoR: Better Handling Diverse Queries with a Mixture of Sparse, Dense, and Human Retrievers (Kalra et al., 2025) | EMNLP 2025 | 14 | Weights sparse and dense retrievers per query, the per-query version of mmore's fixed hybrid weight. +10.8% nDCG@20 over each single retriever | Skim |

## 5. RQ2: adaptive post-retrieval (judge, retry, abstain)

| # | Paper | Venue | Citations | Why read it | Priority |
|---|---|---|---|---|---|
| 27 | Sufficient Context: A New Lens on RAG Systems (Joren et al., 2025) | ICLR 2025 | 121 | An LLM autorater decides whether the retrieved context is enough to answer, and the system abstains when it is not. Raises accuracy on answered questions by up to 10 points. The basis for "no answer in the documents" | Core |
| 28 | Corrective Retrieval Augmented Generation (CRAG; Yan, Gu, Zhu & Ling, 2024) | arXiv | 339 | A light T5 evaluator labels retrieved documents correct, ambiguous or incorrect, then keeps, refines or falls back to web search. The closest prior "grade, then fall back" design. Not the CRAG benchmark (NeurIPS 2024) | Core |
| 29 | S2G-RAG: Structured Sufficiency and Gap Judging for Iterative Retrieval-Augmented QA (Li et al., 2026) | ACL 2026 | 14 | A sufficiency judge that turns what is missing into the next query. Its judge's error rates show what to expect from yours: on HotpotQA it wrongly accepts 6.4% of insufficient contexts and wrongly rejects 31.6% of sufficient ones | Core |
| 30 | Unanswerability Evaluation for Retrieval Augmented Generation (UAEval4RAG, 2025) | ACL 2025 | 14 | A protocol for building unanswerable queries for a given corpus. Use it to build your "no answer in the documents" test set | Skim |
| — | Self-RAG: Learning to Retrieve, Generate, and Critique through Self-Reflection (Asai et al., 2024) | ICLR 2024 | 2,911 | Trains the model to emit tokens that decide when to retrieve and grade passages. Examiners expect it cited, but your judge is training-free | Ref |

## 6. RQ3: multimodal and visual document retrieval

Page-image retrieval leads on clean, visually rich documents at first-stage retrieval. Strong parsing plus a reranker closes that gap, and on multilingual or degraded documents text pipelines often win. Which modality wins therefore depends on the corpus, which is what RQ3 tests. No study covers scanned humanitarian documents.

| # | Paper | Venue | Citations | Why read it | Priority |
|---|---|---|---|---|---|
| 31 | ColPali: Efficient Document Retrieval with Vision Language Models (Faysse et al., 2025) | ICLR 2025 | 255 | Embeds page images with a vision-language model and scores them by late interaction. Introduced the ViDoRe benchmark. Its OCR baseline used Tesseract, a weak engine, so its margin over OCR is an upper bound | Core |
| 32 | OCR Hinders RAG: Evaluating the Cascading Impact of OCR on RAG (OHRBench; Zhang et al., 2025) | ICCV 2025 | 44 | 8,561 document images across 7 domains. No OCR tool tested was good enough to build a RAG knowledge base. Measures how OCR noise degrades retrieval and answers | Core |
| 33 | ViDoRe V3: A Comprehensive Evaluation of Retrieval Augmented Generation in Complex Real-World Scenarios (Loison et al., 2026) | ACL 2026 | 36 | 26,000 pages, 3,099 queries in 6 languages, 3 French corpora. First stage: best image retriever 59.8 nDCG@10 vs best text 51.0. With a reranker, text reaches 63.6 (best overall) vs 57.8 for images. Hybrid context (page images + text chunks) gives the best answers. No scanned handwritten notes | Core |
| 34 | VisRAG: Vision-based RAG on Multi-modality Documents (Yu et al., 2025) | ICLR 2025 | 329 | Retrieves and generates directly on page images, skipping parsing. Reports 20–40% end-to-end gains over text RAG; check which text baseline that is measured against | Skim |
| 35 | MIRACL-VISION: A Large, Multilingual, Visual Document Retrieval Benchmark (Osmulski et al., 2025) | arXiv | 10 | 18 languages. Image retrievers trail text retrievers by up to 59.7%, and by 12.1% even in English. The main counter-evidence on multilingual documents | Skim |
| 36 | Lost in OCR Translation? Vision-Based Approaches to Robust Document Retrieval (Most et al., 2025) | ACM DocEng 2025 (not confirmed) | 16 | On degraded scans, OCR-based retrieval generalises better than ColPali to unseen documents of varying quality. The closest test to your scanned documents | Skim |

## 7. Agentic RAG

Your RQ2 is a constrained, one-retry version of the agent loop (plan, search, read, decide). Two papers are enough for context.

| # | Paper | Venue | Citations | Why read it | Priority |
|---|---|---|---|---|---|
| 40 | ReAct: Synergizing Reasoning and Acting in Language Models (Yao et al., 2023) | ICLR 2023 | 12,909 | Interleaves reasoning steps with tool calls such as search. The loop most agent frameworks still use | Skim |
| 41 | Agentic Retrieval-Augmented Generation: A Survey on Agentic RAG (Singh et al., 2025) | arXiv | 463 | Taxonomy built on four agent patterns: reflection, planning, tool use, multi-agent collaboration. Includes healthcare use cases | Skim |

## Left out on purpose

- Chunking (semantic, adaptive): the semester-project students cover it.
- GraphRAG and knowledge-graph retrieval: a separate research line, not in your configuration grid.
- Training or fine-tuning retrievers and agents: heavy compute, against your plan of fast iterations.
- Long-context models vs RAG: an active debate, but it does not change your three research questions.
- Document parsers (Docling, MinerU, olmOCR, OmniDocBench): add them only if parser choice enters your configuration grid.

## Check before quoting

- EMNLP 2026 (RetrievalRouter) and ICML 2026 (Retriever Portfolios): confirm on the proceedings pages once published.
- Scientific Reports (Decostanzi) and ACM DocEng (Most et al.): venue taken from Semantic Scholar only.
- S2G-RAG's error rates and VisRAG's 20–40% figure: read from summaries, check against the PDF.
