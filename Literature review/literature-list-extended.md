# Audit reshapes the mmore thesis reading list

All 62 papers on the reading list exist. But five entries say something false or out of date that would mislead a reader or an examiner. **Adaptive Re-Ranking's "only 40% of queries gained from a reranker" is inverted**: the paper says about 40% gain *nothing*, so about 60% do gain, and it is peer reviewed at ICTIR 2026, not arXiv only ([arXiv HTML](https://arxiv.org/html/2606.25249v1), [Crossref](https://api.crossref.org/works/10.1145/3805713.3820436)). **ViDoRe V3, the list's main evidence that "page images win", actually gives the highest overall retrieval score to a text pipeline with a text reranker** ([ViDoRe V3](https://arxiv.org/html/2601.08620v2)). **mmore fuses sparse and dense scores with a weighted sum, not Reciprocal Rank Fusion (RRF), and its default embedding models are English-only.** That is a confound for French documents that the list never mentions. **The Chen routing-oracle paper was retitled and its v1 figure withdrawn** ([arXiv](https://arxiv.org/abs/2607.03436)). Two new papers belong in "Closest work": **Retriever Portfolios (ICML 2026, EPFL co-author Ola Svensson)**, which turns your oracle gap into an optimisation objective ([arXiv](https://arxiv.org/abs/2605.31176)), and **RINSE**, which makes almost exactly your RQ2 decision (answer, retrieve again, or abstain) ([arXiv](https://arxiv.org/html/2609.37469v1)). The citation column, which was written "from memory", is wrong in both directions. It undercounts Dense Passage Retrieval (DPR) by a factor of seven and BGE M3 by a factor of twenty, and it overcounts OHRBench (44 citations, listed as "100+"). This report keeps your nine categories. It recommends removing 4 papers, demoting 7, moving 3, promoting 1, and adding 54, for a total of 112: 36 Core, 55 Skim and 21 Ref. That total includes a proposed 10-paper "Domain: humanitarian and global health" section. Those additions are weighted toward RQ2 judges, multilingual retrieval and OCR-quality evidence, where the current list is thinnest.

**Conventions used throughout.** Citation counts are from Semantic Scholar's Graph API, retrieved on 22 October 2026, unless a cell says "OpenAlex". OpenAlex was used only where Semantic Scholar was rate-limited or did not index the paper, and it usually reports fewer citations for recent arXiv papers. **Google Scholar was not queried. Its counts are usually higher (often 1.3 to 2 times the Semantic Scholar count).** "n/a" means no count was retrieved. In the venue column, "**Verified**" means the venue was confirmed on a publisher, proceedings, ACL Anthology or Crossref/DOI page. "**Self-reported**" means the only evidence is the authors' arXiv comment or repository. "**Per Semantic Scholar**" means only the database's venue label was seen. "**Unverified**" means the claim could not be confirmed. Priorities use your definitions: Core = read fully, Skim = abstract, figures and conclusion (about 10 minutes), Ref = know it exists and cite it. Metrics are defined where they first appear.

## Five errors in the current list would mislead an examiner

**Adaptive Re-Ranking (Genc, Korukluoglu & Allan).** The list's Closest-work row says "only 40% of queries gained from a reranker, 11% from a heavier one". The paper says "approximately 40% of queries show **no** benefit from re-ranking at all, and only 11% benefit from using a heavy re-ranker" ([arXiv HTML](https://arxiv.org/html/2606.25249v1)). The router chooses among BM25 (classic keyword ranking), MiniLM-L6-v2 (a small cross-encoder reranker) and BGE-v2-m3 (a heavier reranker). It is trained on about 306,000 BEIR queries, labelled by a utility score that penalises latency. Compared with always using BGE, it gives 1.15 to 53 times lower median latency at between −17.5% and +4.0% nDCG@10. nDCG@10 is normalized discounted cumulative gain over the top 10 results: a 0–1 ranking score that rewards relevant documents placed near the top, divided by the score of the ideal ranking. The per-query oracle (the strategy chosen in hindsight to be best for each query) beats every fixed strategy at every latency budget. The venue is also wrong: the paper is in the *Proceedings of ICTIR 2026*, DOI 10.1145/3805713.3820436 ([Crossref](https://api.crossref.org/works/10.1145/3805713.3820436)). One internal inconsistency is worth knowing before you quote it. The text says 59.2% of labels are class 0, but Table 1 gives 59.6%.

**ViDoRe V3 and the Section 6 intro.** The list says "visual retrievers beat textual ones", and the section intro says the evidence "favours page-image retrieval". That holds only for first-stage retrieval. The best visual retriever, ColEmbed-3B-v2, scores 59.8 nDCG@10, and jina-embeddings-v4 scores 57.6 in visual mode against 50.4 in text mode. But **jina-v4 in text mode with the zerank-2 reranker reaches 63.6 nDCG@10, the highest retrieval score in the paper**, while the visual pipeline with its reranker reaches only 57.8. For answer generation outside the oracle setting, hybrid context (top-5 page images plus top-5 text chunks) scores 65.7, against 64.9 for text and 64.5 for images ([ViDoRe V3](https://arxiv.org/html/2601.08620v2)). Two newer results push in the same direction. MIRACL-VISION (18 languages) finds that visual retrievers trail text retrievers by up to 59.7% in multilingual retrieval and by 12.1% in English ([arXiv](https://arxiv.org/abs/2505.11651)). Most et al. (DocEng 2025) find that OCR-based retrieval "generalizes better to unseen documents of varying quality" than ColPali ([arXiv](https://arxiv.org/abs/2505.05666)). ViDoRe V3's text arm used NVIDIA's NeMo Retriever extraction, a modern parser. Its Limitations section notes that it contains no scanned handwritten notes, which are exactly your setting. "OCR quality is a bottleneck" still stands (OHRBench, MultiOCR-QA). "Page images win" does not stand in general.

**mmore's hybrid search is not RRF, and its defaults are English.** In the local code, hybrid fusion is pymilvus `WeightedRanker`, a weighted score sum with `hybrid_search_weight: float = 0.5` (`mmore/src/mmore/rag/retriever.py`, lines 35 and 247). The sparse model is `naver/splade-cocondenser-selfdistil` (`src/mmore/rag/model/sparse/base.py:8`), an English model trained on MS MARCO. The PubMedQA example uses dense `BAAI/bge-large-en-v1.5`, the WHO example uses dense `intfloat/e5-large-v2`, and the reranker is `BAAI/bge-reranker-base` (`retriever.py:39`). The RRF row ("the standard way to merge a sparse and a dense ranking") therefore describes an alternative to mmore, not what mmore does. The peer-reviewed comparison of the two fusion families is Bruch, Gai & Ingber, ACM TOIS 2023, 133 citations ([OpenAlex](https://api.openalex.org/works?search=An%20Analysis%20of%20Fusion%20Functions%20for%20Hybrid%20Retrieval)). The English-only defaults matter more. On French documents, a sparse-vs-dense comparison built from these defaults mixes up retrieval architecture with language coverage. This is an inference from the model names and training data; no one has tested these checkpoints on French here.

**Chen's routing-gap paper has changed underneath the list.** The list cites "How Much of the Routing Gap Is Real?" with the v1 conclusion that part of the router-to-oracle gap is single-run noise. Version 3 (27 September 2026) is retitled "What Does a Routing Oracle Measure Under Stochastic Decoding? Coupling, Scorer Choice, and Single-Commit Ceilings". Its arXiv comment says it "supersedes empirical summaries and interpretations in earlier versions". It now reports "premiums" (the gap between a single-commit ceiling and a product-coupling union of 0.37, 3.54 and 5.10 percentage points on GSM8K, MATH-500 and GPQA-Diamond). Their intervals start at zero, and the paper claims no deployment gain ([arXiv](https://arxiv.org/abs/2607.03436)). Do not quote the v1 "12–36% is label noise" figure.

**Closest work has two missing neighbours and one mis-described one.** *Retriever Portfolios* (Stouras, Cohen-Addad, Lattanzi, Svensson; ICML 2026 per the arXiv comment; 1 citation) chooses a small, diverse set of k retrievers to maximise expected best-of-k (the expected score of the best retriever in the set, per query), with near-optimal guarantees, and then routes each query to one of them. That is your virtual-best-vs-single-best gap written as an objective ([arXiv](https://arxiv.org/abs/2605.31176)). *RINSE* ("Relevance Is Not Sufficient Evidence", arXiv September 2026, 1 citation) treats the post-retrieval decision as answer, retrieve again, widen to web, or abstain. It reports that sufficiency can be judged before generation but that no single signal works on every dataset ([arXiv](https://arxiv.org/html/2609.37469v1)). On the mis-described side, RetrievalRouter evaluates 7 pipelines but **its router has only 5 arms**. The two late-interaction pipelines are static baselines only ([arXiv HTML](https://arxiv.org/html/2608.25625v1)). The list's sentence that "Modality Relevance" and "Beyond the Query" "disagree on whether post-retrieval signals help" also overstates how comparable they are. The first escalates modality *after generation* on MultiModalQA. The second makes a binary run-or-skip decision over document, audio and video inputs ([arXiv](https://arxiv.org/pdf/2607.05438), [arXiv](https://arxiv.org/pdf/2609.12437)).

The list's header also says each venue was "checked on the conference or ACL Anthology page" and that citations are a "rough lower bound from memory". Both should change. Five venues were wrong or outdated: Adaptive Re-Ranking, Tian et al. (now ECIR 2026), Lumer et al. (now ICAART 2026), Dado et al. (CIKM 2026, unconfirmed), and the garbled "g class" entry. The buckets put more than ten papers one bucket too low, among them DPR, BGE M3, RAGAS, Adaptive-RAG, Mallen et al., ReAct, IRCoT, and the two "New" entries Singh and Search-R1. Only OHRBench was overstated. Replace the column with dated Semantic Scholar numbers.

## Section-by-section audit of the 62 listed papers

The tables follow the list's order. "List said" quotes the venue, citation bucket and priority as printed, plus any claim that needs fixing. A "Keep" action with a note means the metadata or description needs a small fix.

### Closest work (6 listed)

| Paper | Venue (status) | Citations | List said | Action | Reason |
|---|---|---|---|---|---|
| RetrievalRouter: Joint Modality and Architecture Selection for Document Retrieval (Kuru, Keskin, Farahbakhsh, Crespi, 2026) | EMNLP 2026 (Self-reported in the arXiv comment; Anthology not yet checked) | 0 | "EMNLP 2026 (per the authors' page)"; "routes among 7 pipelines"; Core | **Keep Core; fix text** | Router has 5 arms (BM25, TD, TR, MD, MR). The 7 pipelines include 2 static late-interaction baselines. The 80,000+ labelled queries are confirmed, from REAL-MM-RAG, T2-RAGBench and MMDocRAG, mostly financial and scientific documents. Oracle numbers in the next section ([arXiv HTML](https://arxiv.org/html/2608.25625v1)) |
| Modality Relevance is not Modality Utility: Post-hoc Selective Modality Escalation for Cost-Aware Multimodal RAG (Li & Gai, 2026) | arXiv only | 2 | arXiv; RQ2+RQ3; Core | **Demote to Skim; use full title** | The method acts after generation, so it fits RQ2/RQ3, not RQ1. Its 61% "answerable from text and tables" figure is confirmed. arXiv only ([arXiv](https://arxiv.org/pdf/2607.05438)) |
| Beyond the Query: Do Retrieval Signals Improve Adaptive Multimodal RAG Routing? (Q. Li et al., 2026) | arXiv only | 0 | arXiv; RQ1 vs RQ2; Core | **Demote to Skim** | The decision is binary (run or skip an extra step) over document, audio and video inputs, not a choice among configurations. Keep it for its matched query-only control design. Researchers read only the abstract ([arXiv](https://arxiv.org/pdf/2609.12437)) |
| Adaptive Re-Ranking (Genc, Korukluoglu & Allan, 2026) | ICTIR 2026 (**Verified**, Crossref DOI) | 1 | arXiv; "only 40% of queries gained"; Core | **Keep Core; fix venue and claim** | About 40% gain nothing and 11% gain from the heavy reranker. See the first section ([Crossref](https://api.crossref.org/works/10.1145/3805713.3820436)) |
| How Much of the Routing Gap Is Real? → v3: What Does a Routing Oracle Measure Under Stochastic Decoding? (Chen, 2026) | arXiv only, single author | 6 | arXiv; "part of the gap is noise"; Skim | **Keep Skim; retitle; drop v1 claim** | v3 supersedes the earlier empirical claims. It is about LLM routing on maths and science QA, so use it only as a methodology reference ([arXiv](https://arxiv.org/abs/2607.03436)) |
| Evaluating Modern RAG: Textual, Multimodal, Dense, and Late Interaction Pipelines (Kuru & Keskin, 2026) | arXiv only | 0 | arXiv; Skim | **Demote to Ref** | Same authors and pipeline set as RetrievalRouter, which is peer reviewed and also reports static trade-offs ([arXiv](https://arxiv.org/pdf/2608.23176)) |

### Earlier and adjacent work (19 listed)

| Paper | Venue (status) | Citations | List said | Action | Reason |
|---|---|---|---|---|---|
| A Survey of Pre-retrieval Query Performance Predictors (Hauff, Hiemstra & de Jong, 2008) | CIKM 2008 (**Verified**, DOI 10.1145/1458082.1458311) | 58 (OpenAlex 161) | CIKM 2008; Core | **Keep Core** | The query-only predictor set for RQ1 |
| Predicting Efficiency/Effectiveness Trade-offs for Dense vs. Sparse Retrieval Strategy Selection (Arabzadeh, Yan & Clarke, 2021) | CIKM 2021 (**Verified**, DOI) | 62 (OpenAlex 40) | CIKM 2021; Core | **Keep Core** | It is the only adaptive baseline RetrievalRouter actually runs. Check in the PDF whether the classifier chooses among sparse, dense and hybrid, or only whether to add dense |
| Learning to Select a Ranking Function (Peng, Macdonald & Ounis, 2010) | ECIR 2010 (**Verified**, LNCS DOI) | 32 (OpenAlex 36) | ECIR 2010; Skim | **Keep Skim** | Peer reviewed and directly on topic |
| Predicting Query Performance (Cronen-Townsend, Zhou & Croft, 2002) | SIGIR 2002 (**Verified**, DOI) | 839 (OpenAlex 669) | SIGIR 2002; Ref | **Keep Ref** | The clarity score. It now sits next to the added NQC and WIG predictors |
| Automated Algorithm Selection: Survey and Perspectives (Kerschke, Hoos, Neumann & Trautmann, 2019) | Evolutionary Computation 27(1) (**Verified**; online 2018) | 508 (OpenAlex 439) | Journal, 2019; Skim | **Keep Skim; add authors** | Source of the "virtual best solver" (the best method chosen per instance in hindsight) vs "single best solver" (the one method best on average) vocabulary |
| DAT: Dynamic Alpha Tuning for Hybrid Retrieval in RAG (Hsu & Tzeng, 2025) | arXiv only | 20 (OpenAlex 5) | arXiv; Core | **Demote to Skim** | Still arXiv only. Bruch et al. (TOIS 2023, added) is the peer-reviewed fixed-weight baseline that DAT's per-query weight must beat ([arXiv](https://arxiv.org/abs/2503.23013)) |
| RouterRetriever: Routing over a Mixture of Expert Embedding Models (Lee, Soldaini, Cohan et al., 2025) | AAAI 2025 (**Verified**, DOI 10.1609/aaai.v39i11.33306) | 17 (OpenAlex 3) | AAAI 2025; Skim | **Keep Skim** | The "+2.1 nDCG@10" figure was not re-checked |
| MBA-RAG: a Bandit Approach for Adaptive RAG through Question Complexity (Tang et al., 2025) | COLING 2025 (**Verified**, Anthology 2025.coling-main.218) | 43 | COLING 2025; Skim | **Keep Skim** | Description confirmed: the reward penalises extra retrieval steps ([Anthology](https://preview.aclanthology.org/setup/2025.coling-main.218)) |
| Efficient Context Selection for Long-Context QA: No Tuning, No Iteration, Just Adaptive-k (Taguchi et al., 2025) | EMNLP 2025 main (**Verified**, DOI) | 29 (OpenAlex 8) | EMNLP 2025; Skim | **Keep Skim; full title** | The "10× fewer tokens" claim was not re-checked |
| METIS: Fast Quality-Aware RAG Systems with Configuration Adaptation (Ray et al., 2025) | SOSP 2025 (**Verified**, DOI) | 29 (OpenAlex 9) | SOSP 2025; Skim | **Keep Skim** | The 1.64–2.54× latency figure was not re-checked |
| An Analysis of Hyper-Parameter Optimization Methods for RAG (Orbach et al., 2025) | AAAI 2026 Workshop on New Frontiers in IR (**Self-reported**, arXiv comment only) | 3 (OpenAlex 0) | AAAI 2026 workshop; Skim | **Keep Skim; add finding** | Also finds that, for greedy search, optimising model choice first beats optimising in pipeline order ([arXiv](https://arxiv.org/abs/2505.03452v3)) |
| AutoRAG-HP: Automatic Online Hyper-Parameter Tuning for RAG (Fu et al., 2024) | Findings of EMNLP 2024 (**Verified**, DOI) | 21 (OpenAlex 8) | Findings EMNLP 2024; Ref | **Keep Ref** | Merge with Orbach into one "best single configuration" line |
| RAGSmith: Finding the Optimal Composition of RAG Methods Across Datasets (Kartal et al., 2025) | arXiv only | 6 (OpenAlex 0) | arXiv; Ref | **Remove** | Duplicates Orbach and AutoRAG-HP, and has no venue |
| Predicting the Benefit of Retrieval Augmentation in Open-Domain QA (Dado, Carmel & Kurland, 2026) | CIKM 2026 (**Unverified**: assigned DOI returns 404; claim comes from a co-author's post) | 1 (OpenAlex 0) | "ACM, 2026 (per its DOI)"; Skim | **Keep Skim; fix venue and text** | A secondary summary reports that pre-retrieval features were largely ineffective, which is direct evidence that RQ1 is hard (check in the PDF). Drop "Carmel co-wrote the QPP textbook", since the textbook is not on the list ([arXiv](https://arxiv.org/abs/2604.07985)) |
| Predicting Retrieval Utility and Answer Quality in RAG (Fangzheng Tian, Ganguly & Macdonald, 2026) | **ECIR 2026** (**Verified**, ECIR archive and LNCS DOI) | n/a (OpenAlex 3) | arXiv; Skim | **Keep Skim; fix venue** | Linear regression over retriever signals, reader perplexity and document-quality features, on Natural Questions only ([ECIR](https://ecir2026.dryfta.com/programme/abstract-archive/abstract/public/42/predicting-retrieval-utility-and-answer-quality-in-retrieval-augmented-generation)) |
| Adaptive Query Performance Prediction for Retrieval-Augmented Generation: Bridging Retrieval Quality and Generation Relevance (Sinha & Chakma, 2026) | ACM TOIS 2026 (**Verified**, DOI 10.1145/3827605) | 2 (OpenAlex 0) | TOIS 2026; Skim | **Keep Skim; full title** | Low citation count, but published in a top IR journal |
| Comparison of Text-Based and Image-Based Retrieval in **Modern** Multimodal RAG LLM Systems (Lumer et al., 2025) | **ICAART 2026** (**Verified**, DOI 10.5220/0014479300004052) | 6 (OpenAlex 1) | arXiv; Ref | **Fix venue and title; move to Section 6 (Ref)** | It is about text vs image (RQ3) and uses only 40 questions |
| R1-Router → retitled "Mixture-of-Retrieval Experts for Reasoning-Guided Multimodal Knowledge Exploitation" (MoRE; Peng et al., 2025) | SIGIR 2026 (**Unverified**, repository mirror only) | 8 | arXiv; Ref | **Remove** | Retitled. It routes each reasoning step with reinforcement learning across multimodal knowledge bases, which is tangential to all three RQs ([arXiv](https://arxiv.org/abs/2505.22095)) |
| RouteLLM: Learning to Route LLMs **from** Preference Data (Ong et al., 2025) | ICLR 2025 (**Verified**, proceedings) | 875 | ICLR 2025; Ref | **Keep Ref; fix title** | The proceedings title says "from", while arXiv says "with" ([ICLR](https://proceedings.iclr.cc/paper_files/paper/2025/hash/5503a7c69d48a2f86fc00b3dc09de686-Abstract-Conference.html)) |

### 1. Retrieval foundations (6 listed)

| Paper | Venue (status) | Citations | List said | Action | Reason |
|---|---|---|---|---|---|
| Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks (Lewis et al., 2020) | NeurIPS 2020 (**Verified**, DBLP) | 19,864 | 10k+; Core | **Keep Core** | Correct |
| Dense Passage Retrieval for Open-Domain Question Answering (Karpukhin et al., 2020) | EMNLP 2020 (**Verified**, DOI) | 7,416 | 1k+; Core | **Keep Core; fix count** | Undercounted about 7× |
| ColBERT: Efficient and Effective Passage Search via Contextualized Late Interaction **over BERT** (Khattab & Zaharia, 2020) | SIGIR 2020 (**Verified**, DOI) | 2,681 | 1k+; Core | **Keep Core; fix title** | The list drops "over BERT" |
| M3-Embedding: Multi-Linguality, Multi-Functionality, Multi-Granularity Text Embeddings Through Self-Knowledge Distillation (BGE M3; Jianlv Chen et al., 2024) | Findings of ACL 2024 (**Verified**, DOI) | 2,053 | 100+; Skim | **Keep Skim; fix count and text** | Undercounted 20×. Its Anthology title starts "M3-Embedding". mmore's default dense model is bge-large-en-v1.5, not M3, so "matches mmore's hybrid index" should become "a multilingual candidate for the configuration grid" |
| Reciprocal Rank Fusion outperforms Condorcet and Individual Rank Learning Methods (Cormack, Clarke & Büttcher, 2009) | SIGIR 2009 (**Verified**, DOI) | 1,496 | 1k+; Skim | **Keep Skim; reword** | mmore uses a weighted score sum, so RRF is the comparison arm, not mmore's method |
| Passage Re-ranking with BERT (Nogueira & Cho, 2019) | arXiv only | 1,700 | 1k+; Skim | **Demote to Ref** | Never peer reviewed. monoT5 (Findings of EMNLP 2020, added) is the peer-reviewed reranker reference from the same group |

### 2. Evaluation and benchmarks (5 listed)

| Paper | Venue (status) | Citations | List said | Action | Reason |
|---|---|---|---|---|---|
| BEIR: A **Heterogenous** Benchmark for Zero-shot Evaluation of Information Retrieval Models (Thakur et al., 2021) | NeurIPS 2021 Datasets & Benchmarks (**Verified**) | 2,178 | 1k+; Core | **Keep Core; fix spelling** | The official title spells it "Heterogenous" |
| Benchmarking Retrieval-Augmented Generation for Medicine (MIRAGE/MedRAG; Xiong, Jin, Lu & Zhang, 2024) | Findings of ACL 2024 (**Verified**, [Anthology](https://aclanthology.org/2024.findings-acl.372/)) | 675 | 100+; Core | **Keep Core; fix count** | Name clash with the multilingual MIRAGE-Bench (NAACL 2025), so give the full title |
| Healthier LLMs: Retrieval-Augmented Generation for Public Health Question Answering (Feldman et al., 2026) | arXiv only | 0 | New; Skim | **Promote to Core; move to the proposed Domain section** (stays in Section 2 if you do not adopt it) | The nearest precedent for your setting. Hybrid retrieval "consistently improves recall and ranking" on 7,929 UK public-health guidance questions, and its rubric LLM judge is validated against human annotators ([arXiv](https://arxiv.org/abs/2607.06641)) |
| RAGAs: Automated Evaluation of Retrieval Augmented Generation (Es et al., 2024) | EACL 2024 System Demonstrations (**Verified** via DBLP; page range from a search snippet) | 1,228 | 100+; Skim | **Keep Skim; fix count; say "demo/tool paper"** | Widely used, but it is a tool paper, not a validated metric study |
| ARES: An Automated Evaluation Framework for RAG Systems (Saad-Falcon et al., 2024) | NAACL 2024 (**Verified**, DBLP) | 467 | 100+; Skim | **Keep Skim; fix count** | Correct |

### 3. Surveys and best practices (4 listed)

| Paper | Venue (status) | Citations | List said | Action | Reason |
|---|---|---|---|---|---|
| Retrieval-Augmented Generation for Large Language Models: A Survey (Gao et al., 2023) | arXiv only | 4,328 | 1k+; Core | **Keep Core** | Not peer reviewed but very widely cited. Pair it with Fan et al. (KDD 2024, added) |
| Searching for Best Practices in Retrieval-Augmented Generation (Xiaohua Wang et al., 2024) | EMNLP 2024 main (**Verified**, DBLP) | 209 | 100+; Core | **Keep Core** | Correct |
| Ask in Any Modality: A Comprehensive Survey on Multimodal RAG (Abootorabi et al., 2025) | Findings of ACL 2025 (Anthology ID 2025.findings-acl.861 per the authors' README) | 85 | Findings ACL 2025; New; Skim | **Keep Skim; update count** | Correct |
| Unlocking Multimodal Document Intelligence: From Current Triumphs to Future Frontiers of Visual Document Retrieval (Yan et al., 2026) | arXiv only ("under review") | 9 | arXiv; New; Ref | **Keep Ref; full title** | Overlaps with Abootorabi. Kept as a map for RQ3 only |

### 4. RQ1: adaptive pre-retrieval (6 listed)

| Paper | Venue (status) | Citations | List said | Action | Reason |
|---|---|---|---|---|---|
| Adaptive-RAG: Learning to Adapt Retrieval-Augmented LLMs through Question Complexity (Jeong et al., 2024) | NAACL 2024 main (**Verified**) | 762 (OpenAlex 242) | 100+; Core | **Keep Core; fix count** | Heavily undercounted |
| Adaptive Retrieval Without Self-Knowledge? Bringing Uncertainty Back Home (Moskvoretskii et al., 2025) | ACL 2025 long (**Verified**, [Anthology 2025.acl-long.319](https://aclanthology.org/2025.acl-long.319/)) | 49 | New; Core | **Keep Core** | Confirmed: 35 methods (8 adaptive-retrieval, 27 uncertainty), 6 datasets, 10 metrics |
| LTRR: Learning To Rank Retrievers for LLMs (Kim & Diaz, 2025) | SIGIR 2025 LiveRAG workshop, spotlight (**Self-reported**, arXiv comment) | 8 | SIGIR 2025 LiveRAG workshop; Core | **Keep Core** | The list is already correct here. Semantic Scholar and OpenAlex both tag it "SIGIR", which misleads. Claims confirmed: "no retrieval" is an arm, and pairwise XGBoost beats the best single retriever ([arXiv](https://arxiv.org/pdf/2506.13743)) |
| When Not to Trust Language Models (Mallen et al., 2023) | ACL 2023 long (**Verified**) | 1,587 (OpenAlex 331) | 100+; Skim | **Demote to Ref; fix count** | Highly cited, but the thesis needs only its finding (retrieval helps mostly for rare entities) and the PopQA dataset |
| R³AG: Retriever Routing for Retrieval-Augmented Generation (Zhao, Zhu, Tian & Dou, 2026) | ACL 2026 long (**Verified**, [Anthology 2026.acl-long.939](https://aclanthology.org/2026.acl-long.939/)) | 1 | ACL 2026; Skim | **Keep Skim** | Two-dimensional scoring (retrieval quality and generation utility) confirmed |
| RAGRouter-Bench: A Dataset and Benchmark for Adaptive RAG Routing (Ziqi Wang et al., 2026) | arXiv only | 9 | arXiv; Skim | **Keep Skim** | Confirmed: 7,727 queries, 21,460 documents, 5 paradigms, no paradigm dominates. It already has a follow-up baseline paper ([arXiv](https://arxiv.org/pdf/2602.00296)) |

### 5. RQ2: adaptive post-retrieval (5 listed)

| Paper | Venue (status) | Citations | List said | Action | Reason |
|---|---|---|---|---|---|
| Corrective Retrieval Augmented Generation (CRAG; Yan, Gu, Zhu & Ling, 2024) | arXiv only (OpenReview status unreadable; a secondary wiki claims it was withdrawn from ICLR 2025, which is unverified) | 339 | arXiv; 100+; Core | **Keep Core; cite as an arXiv preprint** | The closest prior "grade, then fall back" design ([arXiv](https://arxiv.org/abs/2401.15884)). Not the same as the CRAG benchmark (NeurIPS 2024) |
| Sufficient Context: A New Lens on RAG Systems (Joren et al., 2025) | ICLR 2025 (Per Semantic Scholar, plus the official repository) | 121 | ICLR 2025; Core | **Keep Core** | Selective generation using sufficiency raises the accuracy of answered questions by up to 10 points ([GitHub](https://github.com/hljoren/sufficientcontext)) |
| Self-RAG: Learning to Retrieve, Generate, and Critique through Self-Reflection (Asai et al., 2024) | ICLR 2024 (oral status not re-checked) | 2,911 | 1k+; Core | **Demote to Skim** | Synthesis judgement: an examiner will expect it cited, but it trains reflection tokens, while your RQ2 judge is training-free. The new Core papers (S2G-RAG, Yoran et al.) model your design more closely |
| Active Retrieval Augmented Generation (FLARE; Jiang et al., 2023) | EMNLP 2023 (Per Semantic Scholar) | 929 | 100+; Skim | **Keep Skim; fix count** | Representative of generation-time retrieval triggers |
| DRAGIN: Dynamic Retrieval Augmented Generation based on the **Real-time** Information Needs of LLMs (Su et al., 2024) | ACL 2024 (Per Semantic Scholar) | 109 | ACL 2024; Ref | **Remove** (keep only if you run it as a baseline) | Same line as FLARE, which has 8× the citations. Neither is a post-retrieval check |

### 6. RQ3: multimodal and visual document retrieval (6 listed)

| Paper | Venue (status) | Citations | List said | Action | Reason |
|---|---|---|---|---|---|
| ColPali: Efficient Document Retrieval with Vision Language Models (Faysse et al., 2025) | ICLR 2025 (**Verified**, [proceedings](https://proceedings.iclr.cc/paper_files/paper/2025/hash/99e9e141aafc314f76b0ca3dd66898b3-Abstract-Conference.html)) | 255 | 100+; Core | **Keep Core; qualify "beats OCR pipelines"** | A search snippet of Most et al. says ColPali's OCR baseline used Tesseract, a weak OCR engine (not read in full) |
| OCR Hinders RAG: Evaluating the Cascading Impact of OCR on RAG (OHRBench; Zhang et al., 2025) | ICCV 2025 (**Verified**, [CVF](https://openaccess.thecvf.com/content/ICCV2025/papers/Zhang_OCR_Hinders_RAG_Evaluating_the_Cascading_Impact_of_OCR_on_ICCV_2025_paper.pdf)) | 44 | 100+; Core | **Keep Core; fix count (overstated); move to the parsing sub-group** | The only overcounted entry. Still the most direct peer-reviewed evidence that OCR noise hurts RAG |
| ViDoRe V3: A Comprehensive Evaluation of Retrieval Augmented Generation in Complex Real-World Scenarios (Loison et al., 2026) | ACL 2026 main, long (Anthology author page plus Semantic Scholar) | 36 | ACL 2026; Core | **Keep Core; full title; rewrite description** | 10 corpora, about 26,000 pages, 3,099 queries in 6 languages, 3 French corpora. The best retrieval score comes from text plus a text reranker, and the best non-oracle answers from hybrid context ([arXiv](https://arxiv.org/html/2601.08620v2)) |
| VisRAG: Vision-based RAG on Multi-modality Documents (Yu et al., 2025) | ICLR 2025 (Per Semantic Scholar) | 329 | 100+; Core | **Keep Core; check the "20–40%" claim** | Name the text baseline that the 20–40% gain is measured against |
| Unifying Multimodal Retrieval via Document Screenshot Embedding (DSE; Ma et al., 2024) | EMNLP 2024 (Per Semantic Scholar) | 156 | 100+; Skim | **Keep Skim** | jina-v4 now also covers the single-vector screenshot baseline |
| UniversalRAG: RAG over Corpora of Diverse Modalities and Granularities (Yeo et al.; arXiv 2025) | ACL 2026 long (Per Semantic Scholar plus an [Anthology preview, 2026.acl-long.177](https://preview.aclanthology.org/ingest-acl/2026.acl-long.177/)) | 27 | ACL 2026; Skim | **Move to Section 4 (Skim)** | It is a query-only router over modality-specific corpora (text, image, video), not page image vs OCR. Write "(Yeo et al., ACL 2026)" consistently |

### 7. Agentic RAG (5 listed)

| Paper | Venue (status) | Citations | List said | Action | Reason |
|---|---|---|---|---|---|
| Agentic Retrieval-Augmented Generation: A Survey on Agentic RAG (Singh et al., 2025) | arXiv only | 463 | arXiv; New; Core | **Keep Core; fix count** | No longer "New" |
| ReAct: Synergizing Reasoning and Acting in Language Models (Yao et al., 2023) | ICLR 2023 (Per Semantic Scholar) | 12,909 | 1k+; Core | **Keep Core; fix to 10k+** | Correct otherwise |
| Interleaving Retrieval with Chain-of-Thought Reasoning (IRCoT; Trivedi et al., 2023) | ACL 2023 long (**Verified**, Anthology 2023.acl-long.557) | 1,443 | 100+; Skim | **Keep Skim; fix count** | Correct otherwise |
| Search-R1: Training LLMs to Reason and Leverage Search Engines with RL (Jin et al., 2025) | COLM 2025 (**Unverified**: Semantic Scholar shows arXiv only; a UMass news item mentions an unnamed COLM paper) | 1,676 | COLM 2025; New; Skim | **Keep Skim; mark the venue unverified** | Not "New" by any measure ([arXiv](https://arxiv.org/abs/2503.09516)) |
| "g class" → Data-Centric Perspectives on Agentic Retrieval-Augmented Generation: A Survey (Deng et al., 2026) | Findings of ACL 2026 (**Verified**, [Anthology 2026.findings-acl.78](https://aclanthology.org/2026.findings-acl.78/)) | n/a (no arXiv ID found) | Findings ACL 2026; Ref | **Remove** | The title was garbled. It is a catalogue of training data for agentic RAG, which is far from a one-retry, training-free RQ2 |

## Fifty-four additions, concentrated in RQ2 judges, multilingual retrieval and OCR

The additions below are chosen on two tests: a peer-reviewed venue or high citations, *and* a concrete role in the thesis. Several researchers proposed the same papers with different priorities or placements. The reconciliations are stated here so you can override them.

**Bruch et al. (fusion functions).** Two researchers proposed it, one for Earlier and adjacent and one for Foundations. It goes in Section 1 as Core, next to the RRF row it reframes.

**Arabzadeh et al., SIGIR 2026.** One researcher said Core, another said Skim. It is placed as Skim. Its arms are query variants, not pipelines, and its two key results fit in the abstract: the "utility gap" (the variant with the best nDCG often does not give the best answer) and the finding that cheap pre-retrieval predictors often match post-retrieval ones.

**UniversalRAG.** One researcher wanted it demoted, another wanted it cross-listed. It moves to RQ1 as Skim.

**Healthier LLMs.** One researcher put it in Closest work, another made it Core in Section 2. It goes to the Domain section as Core. It overlaps your setting, not your method, and "Closest work" in your list means method overlap.

**Self-RAG, FLARE and DRAGIN.** One researcher proposed adding these to Section 4 without noticing they are already in Section 5. They are not duplicated.

**MIRACL and BERGEN.** For both, one researcher said Core and one said Ref or Skim. Both are Skim. MIRAGE stays the Core benchmarking template.

**LLM-Independent Adaptive RAG (Marina et al.).** One researcher audited it as if it were already listed, but it is not on the list. It is added below.

### Closest work (+3)

| Paper | Venue (status) | Citations | Priority | Reason |
|---|---|---|---|---|
| Retriever Portfolios: A Principled Approach to Adaptive RAG (Stouras, Cohen-Addad, Lattanzi, Svensson, 2026) | ICML 2026 (**Self-reported**, arXiv comment) | 1 | **Core** | Formalises your oracle experiment. It picks a portfolio of k retrievers to maximise expected best-of-k, then routes each query, and reports lower latency and token cost than inference-time tuning. Svensson is at EPFL ([arXiv](https://arxiv.org/abs/2605.31176)) |
| Relevance Is Not Sufficient Evidence: Detecting Evidence Gaps Before Generation in RAG (RINSE, 2026) | arXiv only | 1 | **Core** | Its decision space (answer, retrieve again, widen, abstain) is RQ2. Read as concurrent work to position against ([arXiv](https://arxiv.org/html/2609.37469v1)) |
| One Size Does Not Fit All! Dynamic Retriever and Generator Selection for RAG (DRAG; Anand, Santra, Basuchowdhuri, Ganguly, Bhatia, 2026) | arXiv only | 0 | Skim | A training-free router uses QPP (query performance prediction: estimating how well a query will be answered) to choose the retriever. Finds that stronger retrieval gains more than extra generation effort, with diminishing returns ([arXiv](https://arxiv.org/abs/2609.17709)) |

### Earlier and adjacent work (+5)

The QPP researcher found that the post-retrieval side has only the clarity score and nothing on dense retrievers. An IR examiner would notice that gap. Consider splitting this section into three sub-tables that keep your "Line of work" labels: Classic IR (QPP and per-query selection), RAG (per-query configuration and QPP for RAG, with best-single-configuration as one line), and Method (routing and cost-quality).

| Paper | Venue (status) | Citations | Priority | Reason |
|---|---|---|---|---|
| Predicting Query Performance by Query-Drift Estimation (NQC; Shtok, Kurland, Carmel, Raiber & Markovits, 2012) | ACM TOIS 2012 (DOI 10.1145/2180868.2180873) | 301 (OpenAlex 169) | **Core** | NQC (normalized query commitment) is the standard deviation of the top-k retrieval scores. It can be computed directly from mmore's dense and sparse score lists, and it is the classic analogue of Adaptive-k's "shape of the scores" |
| Query Performance Prediction for Neural IR: Are We There Yet? (Faggioli et al., 2023) | ECIR 2023 (DOI 10.1007/978-3-031-28244-7_15) | OpenAlex 31 | **Core** | Classic QPP works markedly worse on dense and learned-sparse retrievers than on BM25. This warns that classic predictors may not transfer to mmore's index |
| Query Performance Prediction in Web Search Environments (WIG and query feedback; Zhou & Croft, 2007) | SIGIR 2007 (DOI 10.1145/1277741.1277835) | 314 (OpenAlex 268) | Ref | WIG (weighted information gain) is the second standard post-retrieval baseline. One researcher said Core, but NQC is the one you will compute, so this one only needs citing |
| Can QPP Choose the Right Query Variant? Evaluating Query Variant Selection for RAG Pipelines (Arabzadeh, Drozdov, Bendersky, Zaharia, 2026) | SIGIR 2026 (**Verified**, [Crossref](https://api.crossref.org/works/10.1145/3805712.3808571)) | 2 | Skim | QPP used as a per-query selector in RAG, from the author of your "direct ancestor" paper. Pre-retrieval predictors often match post-retrieval ones ([arXiv](https://arxiv.org/abs/2604.22661)) |
| Query Performance Prediction Using Relevance Judgments Generated by LLMs (QPP-GenRE; Meng et al., 2025) | ACM TOIS 2025 (DOI 10.1145/3736402) | 42 | Skim | An LLM judges top-k relevance, and QPP is derived from those judgments. A peer-reviewed template for an RQ2 judge, better established than DAT's similar trick |

### 1. Retrieval foundations (+8)

| Paper | Venue (status) | Citations | Priority | Reason |
|---|---|---|---|---|
| The Probabilistic Relevance Framework: BM25 and Beyond (Robertson & Zaragoza, 2009) | Foundations and Trends in IR (DOI 10.1561/1500000019) | 6,665 | **Core** (BM25 sections only) | The canonical BM25 reference. Every "BM25 is hard to beat" claim needs it |
| An Analysis of Fusion Functions for Hybrid Retrieval (Bruch, Gai & Ingber, 2023) | ACM TOIS 2023 (DOI 10.1145/3596512) | 133 (OpenAlex 69) | **Core** | Compares convex combination (a weighted sum of normalised scores, which is what mmore's `WeightedRanker` does) with RRF. The researcher's summary that convex combination wins is from memory of the abstract, so check it |
| SPLADE: Sparse Lexical and Expansion Model for First Stage Ranking (Formal, Piwowarski & Clinchant, 2021) | SIGIR 2021 (DOI 10.1145/3404835.3463098) | 723 | Skim | The original learned-sparse model. SPLADE++ only makes sense after it |
| From Distillation to Hard Negative Sampling: Making Sparse Neural IR Models More Effective (SPLADE++; Formal et al., 2022) | SIGIR 2022 short (DOI 10.1145/3477495.3531857) | 245 | Ref | The paper behind mmore's `splade-cocondenser-selfdistil` checkpoint |
| C-Pack: Packed Resources For General Chinese Embeddings (Xiao et al., 2024) | SIGIR 2024 (DOI 10.1145/3626772.3657878) | 1,054 | Ref | The citation for the BGE models, including mmore's default `bge-large-en-v1.5` |
| Multilingual E5 Text Embeddings: A Technical Report (L. Wang et al., 2024) | arXiv only (technical report) | 669 | Skim | The multilingual replacement for the English `e5-large-v2` in the WHO example. MMTEB (ICLR 2025) names multilingual-e5-large-instruct the best public model ([ICLR](https://proceedings.iclr.cc/paper_files/paper/2025/hash/fc0e3f908a2116ba529ad0a1530a3675-Abstract-Conference.html)) |
| ColBERTv2: Effective and Efficient Retrieval via Lightweight Late Interaction (Santhanam et al., 2022) | NAACL 2022 (DOI) | 924 | Skim | The late-interaction model people actually run. Late interaction means one vector per token, with each query token scored against its best-matching document token |
| Document Ranking with a Pretrained Sequence-to-Sequence Model (monoT5; Nogueira, Jiang, Pradeep, Lin, 2020) | Findings of EMNLP 2020 (DOI) | 911 | Skim | Peer-reviewed reranker reference that replaces the arXiv-only Nogueira & Cho |

### 2. Evaluation and benchmarks (+6)

| Paper | Venue (status) | Citations | Priority | Reason |
|---|---|---|---|---|
| Large Language Models can Accurately Predict Searcher Preferences (Thomas, Spielman, Craswell, Mitra, 2024) | SIGIR 2024 | 313 | **Core** | LLM relevance labels agree with human labels. The oracle experiment needs per-query relevance labels on humanitarian documents, and this justifies generating them with an LLM |
| MIRACL: A Multilingual Retrieval Dataset Covering 18 Diverse Languages (Zhang, Thakur et al., 2023) | TACL vol. 11 (**Verified**, [Anthology](https://aclanthology.org/2023.tacl-1.63)) | 239 (plus 76 on a separate arXiv record) | Skim | The standard multilingual retrieval benchmark. It includes French, with 726,000 native-speaker relevance judgments |
| BERGEN: A Benchmarking Library for Retrieval-Augmented Generation (Rau et al., NAVER Labs Europe, 2024) | EMNLP 2024 per Semantic Scholar; probably Findings (**Unverified**) | 48 | Skim | The closest analogue to "benchmark a RAG toolkit's configurations", with a multilingual extension |
| Retrieval-augmented generation in multilingual settings (Chirkova et al., 2024) | KnowLLM workshop at ACL 2024 (**Verified**, [Anthology](https://aclanthology.org/2024.knowllm-1.15.pdf)) | 57 | Skim | 13 languages, French near the top of the non-English group. Retrieval, prompting and evaluation all need language-specific changes |
| Benchmarking Large Language Models in Retrieval-Augmented Generation (RGB; Chen et al., 2024) | AAAI 2024 (Per Semantic Scholar) | 717 | Skim | Two researchers proposed it. Its "negative rejection" test (does the model refuse when no retrieved document holds the answer?) is a ready-made RQ2 abstention dimension |
| PubMedQA: A Dataset for Biomedical Research Question Answering (Jin et al., 2019) | EMNLP-IJCNLP 2019 | 2,026 | Ref | The only benchmark mmore was evaluated on |

### 3. Surveys and best practices (+1)

| Paper | Venue (status) | Citations | Priority | Reason |
|---|---|---|---|---|
| A Survey on RAG Meeting LLMs: Towards Retrieval-Augmented Large Language Models (Fan et al., 2024) | KDD 2024 (DOI 10.1145/3637528.3671470) | 1,225 | Skim | A peer-reviewed general survey to sit beside the arXiv-only Gao et al. |

### 4. RQ1 (+4 new, +1 moved in)

| Paper | Venue (status) | Citations | Priority | Reason |
|---|---|---|---|---|
| UniversalRAG (moved from Section 6) | ACL 2026 long (see above) | 27 | Skim | Query-only routing over corpora, from the Adaptive-RAG group |
| MoR: Better Handling Diverse Queries with a Mixture of Sparse, Dense, and Human Retrievers (Kalra, Zhao, Kim, Cai, Diaz, Wu, 2025) | EMNLP 2025 main (Anthology 2025.emnlp-main.601) | 14 | Skim | A per-query weighted mixture of sparse and dense retrievers that matches mmore's hybrid index. It beats each single retriever by +10.8% NDCG@20 (nDCG over the top 20). Same group as LTRR ([arXiv](https://arxiv.org/pdf/2506.15862)) |
| LLM-Independent Adaptive RAG: Let the Question Speak for Itself (**Marina** et al., 2025) | EMNLP 2025 main (**Verified**, [Anthology 2025.emnlp-main.439](https://aclanthology.org/2025.emnlp-main.439/)) | OpenAlex 3 | Skim | Uses 27 non-LLM query features in 7 groups and matches LLM-uncertainty methods at much lower cost. This is the feature-engineering recipe for a cheap RQ1 router. Moskvoretskii is the last author, not the first |
| Retrieval Augmented Generation or Long-Context LLMs? A Comprehensive Study and Hybrid Approach (Self-Route; Z. Li et al., 2024) | EMNLP 2024 Industry Track (Per Semantic Scholar) | 196 | Ref | The standard cost-aware per-query router. One line is justified even though long-context was deliberately left out |
| Most of the LLM Routing Gap Is Task Type (Lee, 2026) | arXiv only | 4 | Ref | Claims much of the router-oracle gap is explained by task type, which argues for a "best configuration per query type" baseline. Full text not read ([arXiv](https://arxiv.org/pdf/2608.23023)) |

### 5. RQ2 (+7)

| Paper | Venue (status) | Citations | Priority | Reason |
|---|---|---|---|---|
| S2G-RAG: Structured Sufficiency and Gap Judging for Iterative Retrieval-Augmented QA (Li et al., 2026) | ACL 2026 long (**Verified**, [Anthology 2026.acl-long.1185](https://aclanthology.org/2026.acl-long.1185/)) | 14 | **Core** | A sufficiency judge plus a targeted re-query, the closest peer-reviewed match to RQ2. On HotpotQA the judge wrongly calls evidence sufficient in 6.44% of cases and wrongly rejects 31.60% of sufficient contexts (search summary of the [arXiv](https://arxiv.org/pdf/2604.23783) paper) |
| Making Retrieval-Augmented Language Models Robust to Irrelevant Context (Yoran et al., 2024) | ICLR 2024 (Per Semantic Scholar) | 510 | **Core** | An NLI (natural language inference) judge detects irrelevant context and falls back to no retrieval. Same "judge, then fall back" pattern as CRAG, but peer reviewed |
| Unanswerability Evaluation for Retrieval Augmented Generation (UAEval4RAG, 2025) | ACL 2025 long (**Verified**, [Anthology](https://aclanthology.org/2025.acl-long.415.pdf)) | 14 | **Core** | A protocol for building unanswerable queries for a given corpus. It notes that rejections often come from failed retrieval rather than real understanding. This is how to build the "no answer in the documents" test set |
| Query Rewriting in Retrieval-Augmented LLMs (Rewrite-Retrieve-Read; Ma et al., 2023) | EMNLP 2023 main (**Verified**, [Anthology](https://aclanthology.org/2023.emnlp-main.322)) | 320 | Skim (Core if one retry option is a rewritten query) | The most common "second try" in the literature |
| Over-Searching in Search-Augmented Large Language Models (Xie et al., 2026) | EACL 2026 long (**Verified**, [Anthology](https://aclanthology.org/2026.eacl-long.361/)) | n/a | Skim | A snippet says search helps answerable queries but harms abstention on unanswerable ones, which warns that a retry can cancel a correct abstention. Confirm in the PDF |
| Chain-of-Note (Yu et al., 2024) | EMNLP 2024 main (Anthology 2024.emnlp-main.813) | 256 | Skim | Writes per-document notes and answers "unknown" when the documents are irrelevant. A cheap prompt-only judge-and-abstain baseline |
| Selective Question Answering under Domain Shift (Kamath, Jia & Liang, 2020) | ACL 2020 (Anthology 2020.acl-main.503) | 335 | Ref | The risk-coverage curve (error rate among answered questions plotted against the fraction answered), the standard way to report an answer/abstain trade-off |

### 6. RQ3 (+11 new, +1 moved in), with a proposed parsing sub-group

The RQ3 researcher found that Section 6 treats "OCR text vs page image" as two fixed options. In practice, the text arm is itself a variable: parser choice changes text-pipeline quality, which is OHRBench's point. MMORE's own headline claim is 40% higher accuracy than Docling on scanned PDFs ([arXiv HTML](https://arxiv.org/html/2509.11937v1)). That makes parsers a natural comparison axis. The proposal is a "Document parsing and OCR quality" sub-group inside Section 6, not a new section. It is a recommendation: adopt it only if parser choice enters your configuration grid.

| Paper | Venue (status) | Citations | Priority | Reason |
|---|---|---|---|---|
| *Visual retrieval* | | | | |
| MIRACL-VISION: A Large, Multilingual, Visual Document Retrieval Benchmark (Osmulski et al., 2025) | arXiv only | 10 | **Core** | The main counter-evidence on multilingual documents: visual retrievers trail text retrievers by up to 59.7% ([arXiv](https://arxiv.org/abs/2505.11651)). Low citations, but irreplaceable for your framing |
| Lost in OCR Translation? Vision-Based Approaches to Robust Document Retrieval (Most et al., 2025) | ACM DocEng 2025 (Per Semantic Scholar) | 16 | **Core** | A direct test on degraded scans: OCR-based retrieval generalises better to unseen documents of varying quality ([arXiv](https://arxiv.org/abs/2505.05666)) |
| MMDocIR: Benchmarking Multi-Modal Retrieval for Long Documents (Dong et al., 2025) | EMNLP 2025 (Per Semantic Scholar) | 60 | Skim | A peer-reviewed benchmark that compares visual and OCR-text retrievers at page and layout level |
| jina-embeddings-v4: Universal Embeddings for Multimodal Multilingual Retrieval (Günther et al., 2025) | MRL 2025 workshop (Per Semantic Scholar) | 114 | Skim | One multilingual model with both a text mode and a visual mode. It gives a same-backbone RQ3 comparison, and ViDoRe V3 uses it this way |
| REAL-MM-RAG: A Real-World Multi-Modal Retrieval Benchmark (Wasserman et al., 2025) | ACL 2025 (Per Semantic Scholar) | 54 | Ref | One source of RetrievalRouter's per-query labels |
| Lumer et al. (moved from Earlier and adjacent) | ICAART 2026 (see above) | 6 | Ref | Small study of text vs image retrieval (40 questions) |
| *Document parsing and OCR quality (proposed sub-group)* | | | | |
| OHRBench (moved within the section) | ICCV 2025 | 44 | **Core** | See audit |
| Evaluating Robustness of LLMs in QA on Multilingual Noisy OCR Data (MultiOCR-QA; Piryani et al., 2025) | CIKM 2025 (Per [arXiv](https://arxiv.org/abs/2502.16781)) | 5 | Skim | 50,000 QA pairs over OCR'd historical documents in English, **French** and German. QA is "highly prone to OCR-induced errors". The one peer-reviewed French OCR result |
| OmniDocBench: Benchmarking Diverse PDF Document Parsing (Ouyang et al., 2025) | CVPR 2025 (**Verified**, DOI 10.1109/CVPR52734.2025.02313) | 192 | Skim | The standard benchmark for scoring parsers such as marker/Surya, Docling, MinerU and olmOCR. One researcher said Core |
| MinerU: An Open-Source Solution for Precise Document Content Extraction (Wang et al., 2024) | arXiv only | 358 | Skim | The most-cited open parsing pipeline and a direct comparator to mmore's ingestion |
| Docling: An Efficient Open-Source Toolkit for AI-driven Document Conversion (Livathinos et al., 2025) | AAAI 2025 Workshop on Open-Source AI for Mainstream Use (**Self-reported**, arXiv record) | 89 | Skim | The toolkit MMORE benchmarks itself against. One researcher wanted it in Closest work as Core. It is placed here because it competes with mmore's ingestion, not with your research questions |
| olmOCR: Unlocking Trillions of Tokens in PDFs with Vision Language Models (Poznanski et al., 2025) | arXiv only | 138 | Skim | A vision-language-model OCR alternative for scanned PDFs, ready to swap into mmore |
| Assessing the Impact of OCR Quality on Downstream NLP Tasks (van Strien et al., 2020) | ICAART 2020 ([SciTePress](https://www.scitepress.org/Papers/2020/91690/)) | not queried | Ref | The pre-2023 origin of the "OCR errors degrade retrieval" claim |

### 7. Agentic RAG (+0)

No additions. The researcher judged the section, if anything, too large for a one-retry RQ2. Removing the Deng et al. survey leaves the loop concept (ReAct, IRCoT), one survey and one training-based reference (Search-R1). The section intro's claim that iterative RAG costs about 3.5 times the tokens of no retrieval comes from the list itself and was not re-checked against RAGRouter-Bench.

### Proposed new section: Domain: humanitarian and global health (10)

This section is a recommendation, not a done deal. The domain researcher found **no peer-reviewed paper that does RAG over WHO, ICRC or MSF guidelines**. That absence is your novelty claim, and it needs sourced neighbours around it. A citable statistic anchors it: of 251 medical RAG studies reviewed, only 6 were conducted in low-resource settings ([arXiv](https://arxiv.org/pdf/2511.05901)). If you prefer not to add a section, Feldman et al. stays in Section 2 as Core, and the rest drop to the reserve pool at the end of the removals section.

| Paper | Venue (status) | Citations | Priority | Reason |
|---|---|---|---|---|
| Healthier LLMs (Feldman et al., 2026; moved from Section 2) | arXiv only | 0 | **Core** | Retrieval-configuration benchmark on official public-health guidance. It differs from your work in being UK-only, English-only, born-digital and without routing |
| A Large-Language-Model Framework for Automated Humanitarian Situation Reporting (Decostanzi, Mejova & Kalimeri, 2025) | Scientific Reports per Semantic Scholar (**Unverified**) | 2 | **Core** | The closest humanitarian RAG paper: RAG QA over more than 1,100 ReliefWeb and other documents covering 13 events, with 86.3% answer relevance and citation precision and recall above 76% ([arXiv](https://arxiv.org/abs/2512.19475)) |
| Retrieval augmented generation for 10 large language models and its generalizability in assessing medical fitness (Ke et al., 2025) | npj Digital Medicine (**Verified**, DOI 10.1038/s41746-025-01519-z) | 157 | Skim | Guideline-grounded RAG across 10 LLMs ([Nature](https://www.nature.com/articles/s41746-025-01519-z.pdf)) |
| HumSet: Dataset of Multilingual Information Extraction and Classification for Humanitarian Crisis Response (Fekih et al., 2022) | EMNLP 2022; Findings status **unverified** | 9 | Skim | Expert-annotated humanitarian documents in English, **French** and Spanish. A possible document source for a French test set ([arXiv](https://arxiv.org/abs/2210.04573)) |
| MedExpQA: Multilingual Benchmarking of LLMs for Medical QA (Alonso, Oronoz & Agerri, 2024) | Artificial Intelligence in Medicine (**Verified**, DOI) | 106 | Skim | English, Spanish, French and Italian medical QA with RAG baselines. Accuracy is markedly lower outside English ([ScienceDirect](https://www.sciencedirect.com/science/article/pii/S0933365724001805)) |
| Improving LLM applications in biomedicine with RAG: a systematic review, meta-analysis, and clinical development guidelines (Liu, McCoy & Wright, 2025) | JAMIA (**Verified**, DOI 10.1093/jamia/ocaf008) | 212 | Skim | The best-cited peer-reviewed medical RAG review |
| Healthy LLMs? Benchmarking LLM Knowledge of UK Government Public Health Information (PubHealthBench; Harris et al., 2025) | arXiv only | 5 | Ref | The benchmark that Healthier LLMs extends ([arXiv](https://arxiv.org/html/2505.06046v1)) |
| RAG in Medicine: A Scoping Review (Yang et al., 2025) | Cell Reports Medicine per Semantic Scholar (**Unverified**) | 16 | Ref | Source of the "6 of 251 studies in low-resource settings" statistic. One researcher said Core for this statistic, but citing it is enough |
| MEDITRON-70B: Scaling Medical Pretraining for Large Language Models (Chen et al., 2023) | arXiv only | 430 | Ref | The LiGHT lab's own medical LLM, built with the ICRC ([arXiv](https://arxiv.org/abs/2311.16079)) |
| Generative AI-enabled clinical decision support system in primary care: a pragmatic, cluster-randomized trial (Agweyu et al., 2026) | Nature Medicine (**Verified**, DOI) | 15 | Ref | In 16 Kenyan facilities, GPT-4o-assisted decision support did not significantly reduce 14-day treatment failure. A sober deployment reference ([Nature Medicine](https://www.nature.com/articles/s41591-026-04503-6)) |

## Four removals, seven demotions, and a list of 112

The consolidated changes to existing entries are as follows.

**Removed (4).**
- RAGSmith: arXiv only, 6 citations, duplicates two peer-reviewed tuning papers.
- R1-Router/MoRE: retitled, venue unverified, tangential.
- DRAGIN: FLARE represents the same line with 8 times the citations.
- The "g class" entry, Deng et al.'s data survey: off-scope for a training-free RQ2.

**Demoted (7).**
- Modality Relevance: Core to Skim.
- Beyond the Query: Core to Skim.
- DAT: Core to Skim.
- Self-RAG: Core to Skim.
- Evaluating Modern RAG: Skim to Ref.
- Nogueira & Cho: Skim to Ref.
- Mallen et al.: Skim to Ref.

None of the demotions questions quality; Mallen has 1,587 citations. Each reflects how much of the paper the thesis actually uses, or the existence of a better-placed peer-reviewed alternative.

**Moved (3).**
- Lumer et al.: to Section 6.
- UniversalRAG: to Section 4.
- Healthier LLMs: to the Domain section. It is also the one **promotion**, from Skim to Core.

| Section | Listed | Removed / moved out | Added / moved in | Result | Core | Skim | Ref |
|---|---|---|---|---|---|---|---|
| Closest work | 6 | 0 | +3 | 9 | 4 | 4 | 1 |
| Earlier and adjacent work | 19 | −3 | +5 | 21 | 4 | 13 | 4 |
| 1 Retrieval foundations | 6 | 0 | +8 | 14 | 5 | 6 | 3 |
| 2 Evaluation and benchmarks | 5 | −1 | +6 | 10 | 3 | 6 | 1 |
| 3 Surveys and best practices | 4 | 0 | +1 | 5 | 2 | 2 | 1 |
| 4 RQ1 | 6 | 0 | +5 | 11 | 3 | 5 | 3 |
| 5 RQ2 | 5 | −1 | +7 | 11 | 5 | 5 | 1 |
| 6 RQ3 (incl. parsing sub-group) | 6 | −1 | +12 | 17 | 6 | 8 | 3 |
| 7 Agentic RAG | 5 | −1 | 0 | 4 | 2 | 2 | 0 |
| Domain (proposed) | 0 | 0 | +10 | 10 | 2 | 4 | 4 |
| **Total** | **62** | **−7** | **+57** | **112** | **36** | **55** | **21** |

The reading load is the Core plus Skim count, 91 papers. Core grows from 26 to 36. If 112 is too many, three cuts bring the list to about 95 without losing an argument: drop the Domain section except Feldman and Decostanzi, drop all new Ref entries except PubMedQA and SPLADE++, and merge Orbach with AutoRAG-HP. "Earlier and adjacent work" is now the heaviest section, with 13 Skims. METIS and MBA-RAG are the first candidates to move to Ref if you want it lighter.

Several papers were considered and deliberately not added. They form a reserve pool to cite if a chapter needs them. Citations are Semantic Scholar unless marked. The first table covers QPP, routing and foundations.

| Paper | Venue | Citations | Why not added |
|---|---|---|---|
| Lost in the Middle (Liu et al.) | TACL 2024 | 5,613 | Well known; not needed for one retry |
| FrugalGPT (Chen, Zaharia & Zou) | TMLR per Semantic Scholar | 1,186 | LLM cascade; covered by RouteLLM |
| Hybrid LLM (Ding et al.) | ICLR 2024 per Semantic Scholar | 499 | LLM routing; same reason as FrugalGPT |
| RouterBench (Hu et al.) | arXiv | 288 | Borrow its oracle-router and AIQ vocabulary only if you plot cost-quality curves. AIQ is the area under the non-decreasing convex hull of the cost-quality points |
| Carmel & Yom-Tov, *Estimating the Query Difficulty* | Morgan & Claypool 2010 | OpenAlex 165 | Add only if you keep the Dado "QPP textbook" clause |
| Query-dependent ranking using k-nearest neighbor (Geng et al.) | SIGIR 2008 | 107 (OpenAlex 165) | Overlaps Peng et al. |
| Uncovering the limitations of QPP (Chifu et al.) | ACM TOIS 2025 | 5 | Caveat already made by Faggioli and Dado |
| E5; RankGPT; Sentence-BERT | arXiv; EMNLP 2023; EMNLP-IJCNLP 2019 | 1,789; 669; 21,503 | Cite only if the model is used |
| MMTEB / MTEB | ICLR 2025 / EACL 2023 | 195 / 1,128 | Model-selection leaderboards |
| SeaKR | ACL 2024 per Semantic Scholar vs ACL 2025 per OpenAlex (conflict) | 68 (OpenAlex 27) | Venue conflict unresolved; overlaps DRAGIN |
| RAGRouter (Zhang et al.) | NeurIPS 2025 per Semantic Scholar | 13 | Its arms are LLMs, not retrievers. Also beware the name clash with RAGRouter-Bench |

The second table covers RQ2, RQ3, evaluation and the domain.

| Paper | Venue | Citations | Why not added |
|---|---|---|---|
| Iter-RetGen; RECOMP; AbstentionBench; Know Your Limits | Findings EMNLP 2023 (unverified); ICLR 2024; NeurIPS 2025; TACL | 605; 337; 172; 180 | Secondary RQ2 baselines or surveys |
| When Should Multi-Round RAG Stop? | arXiv | 1 | Early stopping on Search-R1 saved only 3.70% of retrieval calls for a 0.625-point drop in exact match ([arXiv](https://arxiv.org/abs/2608.13237)). Cite it if you defend the one-retry budget |
| ViDoRe V2; M3DocRAG; VDocRAG; MMLongBench-Doc | arXiv; arXiv; CVPR 2025; NeurIPS 2024 D&B | 83; 140; 96; 245 | Visual RAG lineage, mostly on the generation side |
| Nougat; PaddleOCR-VL | ICLR 2024; arXiv | 340; 98 | Nougat handles academic PDFs only. PaddleOCR-VL is a multilingual parser to consider if you test non-Latin scripts |
| RAGChecker; MT-Bench LLM-as-judge; Kamalloo et al.; FlashRAG | NeurIPS 2024 D&B; NeurIPS 2023 D&B; ACL 2023; WWW 2025 companion | 108; 12,409; 207; 256 | Evaluation alternatives and toolkit positioning |
| MMedBench; AfriMed-QA; MMed-Bench-IR; i-MedRAG | Nature Communications 2024; ACL 2025; arXiv; PSB 2025 | 247; 49; 0; 143 | Domain extras. MMed-Bench-IR is the only multilingual medical retrieval benchmark that includes French |

## The oracle experiment now has published reference points and known traps

**Your oracle has a published yardstick.** RetrievalRouter reports a per-query oracle over 7 pipelines of about **0.90 nDCG@5**, against **0.737 for the best single pipeline** and **0.755 for the learned router** ([arXiv HTML](https://arxiv.org/html/2608.25625v1)). The oracle-minus-best-single gap of about 16 points is the headroom, and the router recovers about 1.8 of those points, roughly a ninth. If mmore's oracle on humanitarian documents shows a gap of similar size, you can say so directly. If the gap is much smaller, that is also a finding: routing is not worth it in this setting. Adaptive Re-Ranking adds a second reference point. There, the oracle beats every fixed strategy at every latency budget, and about 40% of queries gain nothing from reranking ([arXiv HTML](https://arxiv.org/html/2606.25249v1)).

**The oracle is biased upward, so measure it carefully.** Chen v3 shows that "best per query" over noisy single runs measures something different from deployable headroom ([arXiv](https://arxiv.org/abs/2607.03436)). Lee claims much of the router-oracle gap is task type ([arXiv](https://arxiv.org/pdf/2608.23023)). Three design choices follow. First, define the oracle on deterministic retrieval metrics (nDCG@k, recall@k), or repeat generation several times if the oracle is judged on answers. Second, report three numbers rather than two: best single configuration, best configuration per query type, and per-query oracle. The middle number shows how much a trivial router could capture. Third, use Kerschke's terms: virtual best solver for the per-query oracle and single best solver for the best fixed configuration. Retriever Portfolios gives a principled way to shrink the configuration grid before training any router: keep the k configurations whose best-of-k covers most queries ([arXiv](https://arxiv.org/abs/2605.31176)).

**Retrieval-optimal is not answer-optimal.** Arabzadeh et al. (SIGIR 2026) find that the query variant with the best nDCG often does not give the best generated answer ([arXiv](https://arxiv.org/abs/2604.22661)). Compute the oracle twice, once on a retrieval metric and once on an answer metric, and report how often the two disagree. Relevance labels for the retrieval oracle can be LLM-generated, citing Thomas et al. (SIGIR 2024), with a small human-checked sample.

**Expect RQ1 to be hard, and say why in advance.** Faggioli et al. show that classic QPP degrades on neural retrievers. A secondary summary of Dado et al. says pre-retrieval features barely predict whether retrieval helps. Marina et al. show that cheap external features can still match LLM-uncertainty methods. Together these make a learned query-only router an open question. That framing protects the thesis if the router captures little of the gap.

**Make the grid separate language from architecture.** Because the defaults are English (bge-large-en-v1.5, e5-large-v2, an English SPLADE), include at least one multilingual dense model (BGE-M3 or multilingual-E5). Otherwise "dense beats sparse on French" could just mean "this checkpoint saw French". Make fusion a dimension of the grid too: the weighted sum at several weights (mmore's `WeightedRanker`, currently fixed at 0.5) versus RRF, following Bruch et al. Then DAT's per-query weight becomes one more routing arm. For RQ3, add a hybrid arm (top page images plus top text chunks), since that is what ViDoRe V3 found best for non-oracle answers. If parsing enters the grid, use at least two parsers. jina-embeddings-v4 is the cleanest way to compare text and image retrieval with the same backbone.

**For RQ2, report what a retry costs, not only what it gains.** Over-Searching (EACL 2026) suggests a retry can turn a correct abstention into a wrong answer. Count those cases. Report the answer/abstain trade-off as a risk-coverage curve (Kamath et al.) and build the unanswerable query set with UAEval4RAG's protocol. The RQ2 researcher found no peer-reviewed paper that *switches retrieval configuration* (for example sparse to dense, or text to page image) after a judge rejects the first result. CRAG falls back to web search, and S2G-RAG, Rewrite-Retrieve-Read and Iter-RetGen change the query, not the retriever. That supports RQ2's novelty, with the caveat that the search covered about ten queries, not a systematic review.

### Revised "Where mmore stands"

**MMORE is an ICML 2025 CODEML workshop paper with 3 Semantic Scholar citations** ([arXiv](https://arxiv.org/abs/2509.11937), [ICML](https://icml.cc/virtual/2025/48192)). Cite it that way and do not treat its citation count as a quality signal. The list's three gaps should become five.

| # | Gap | Change from the current list |
|---|---|---|
| 1 | No page-image retrieval in the paper | Reframe. The claim to test is not "images win" but "which modality wins on scanned, multilingual documents". ViDoRe V3, MIRACL-VISION and Most et al. disagree, and none covers your setting. Whether the current mmore code has page-image retrieval is still unchecked |
| 2 | No evaluation on scanned, multilingual humanitarian documents | Drop "where OHRBench and ViDoRe V3 show text-only pipelines lose most". ViDoRe V3 excludes scanned handwritten notes, and the text pipeline wins its retrieval ranking with a reranker. OCR-quality evidence (OHRBench, MultiOCR-QA) still applies |
| 3 | No abstention when the corpus holds no answer | Keep. Add S2G-RAG, Yoran et al. and UAEval4RAG next to Sufficient Context and CRAG |
| 4 | **New:** English-only default retrieval models | bge-large-en-v1.5 / e5-large-v2 dense and English SPLADE sparse on French documents. Hybrid fusion is a fixed 0.5 weighted sum, not RRF |
| 5 | **New:** No comparison of parsers | marker/Surya are not benchmarked against OmniDocBench-scored alternatives (MinerU, Docling, olmOCR), even though MMORE's headline claim is about scanned-PDF accuracy relative to Docling |

The closing sentence of that section still holds, but it should change in one place. With Retriever Portfolios (ICML 2026) and R³AG (ACL 2026), per-query retriever routing on public QA benchmarks is now a top-venue topic. The contribution therefore rests even more on the setting (scanned, French, humanitarian), on the toolkit, and on RQ2's configuration-switching retry.

## Items to verify by hand before quoting

The researchers could not resolve the items below from scripts. Most need a browser, either because OpenReview and DBLP blocked automated access, or because the source is not yet published.

| Item | What to check | Where |
|---|---|---|
| RetrievalRouter at EMNLP 2026 | Listed in the accepted papers or the Anthology volume | EMNLP 2026 program or ACL Anthology |
| Retriever Portfolios at ICML 2026 | Listed in the ICML 2026 proceedings | proceedings.mlr.press / icml.cc |
| Dado et al. at CIKM 2026 | DOI 10.1145/3799682.3840689 resolves; pre-retrieval negative result is in the PDF | ACM DL; arXiv 2604.07985 |
| CRAG | OpenReview forum JnWJbrnaUE: accepted, rejected or withdrawn | openreview.net |
| Search-R1 at COLM 2025 | Present in the COLM 2025 accepted list | OpenReview COLM 2025 |
| Orbach et al. | AAAI 2026 New Frontiers in IR workshop page | Workshop website |
| Self-RAG | ICLR 2024 oral status | OpenReview |
| UniversalRAG | ACL 2026 main vs Findings | ACL Anthology 2026.acl-long.177 |
| BERGEN; HumSet; Iter-RetGen; SKR | Findings or main | ACL Anthology |
| Decostanzi et al.; Yang et al. | Scientific Reports; Cell Reports Medicine | Publisher pages |
| Docling; M3DocRAG; jina-v4 | Workshop or arXiv status | DBLP |
| Bruch et al. | That convex combination beats RRF, as summarised | TOIS paper, results section |
| Numeric claims kept from the list | RouterRetriever +2.1 nDCG@10; Adaptive-k "10× fewer tokens"; METIS 1.64–2.54×; Lumer +13 mAP@5 (mean average precision over the top 5); VisRAG 20–40% and its baseline; RAGRouter-Bench 3.5× tokens; Arabzadeh 2021's three-way choice | Each paper's results tables |
| Over-Searching; S2G-RAG numbers | Taken from search snippets, not full text | Anthology PDFs |
| MIRACL-VISION | Whether text retrievers received original Wikipedia text or OCR output. If original text, the 59.7% gap is an upper bound for OCR'd text | arXiv 2505.11651, method section |
| ViDoRe V3 French results | Per-language numbers (the appendix was truncated in the fetch) | arXiv 2601.08620, appendix |
| mmore code | Whether page-image retrieval exists now | `mmore/src/mmore/` |
| Google Scholar counts | If your supervisor expects Scholar numbers; usually higher than Semantic Scholar | scholar.google.com |
| "LiteGator" | Named in RetrievalRouter's related work; a web search found no such paper | RetrievalRouter references |

## Conclusion

Since the list was written, its framing has shifted in two directions at once. The idea of routing retrieval configurations per query has become mainstream at top venues (ICML, ACL, EMNLP 2026), so it can no longer carry a thesis on its own. Meanwhile, the evidence on page images versus OCR text has become more mixed, not less: modern parsers plus rerankers close the gap on clean documents, and text pipelines win on multilingual and degraded ones. Both shifts point to the same place. The defensible contribution is a measured oracle gap, and a test of whether a judge-then-switch retry closes it, on scanned French humanitarian documents where no published study has looked.

Two changes matter more than any single paper. First, the oracle experiment should report the per-query-type baseline and compute the oracle on both retrieval and answer metrics, because the 2026 routing-gap papers show that the raw per-query oracle overstates the headroom. Second, the configuration grid must include a multilingual embedding model. Otherwise mmore's English defaults will confound every sparse-vs-dense and text-vs-image comparison on French documents. That confound is the gap in "Where mmore stands" most likely to undermine a result after the fact.
