# Sections 1–3 of the reading list: retrieval foundations, RAG evaluation and benchmarks, RAG surveys (audit and extension)

Method and provenance (applies to every citation count below):
- **Citation counts:** Semantic Scholar Graph API, `POST /graph/v1/paper/batch` with arXiv IDs or DOIs, fetched **2026-10-22**. Each "S2" link below is the single-paper endpoint for the same record. Google Scholar was **not** queried, and its counts are usually higher than Semantic Scholar's. Quote these numbers as "Semantic Scholar, 22 Oct 2026".
- **Venues:** taken from the ACL Anthology, ICLR proceedings, author pages, the arXiv abstract page, or the DBLP key and DOI that Semantic Scholar returns. The direct DBLP API was blocked from this machine, and the Semantic Scholar keyword search was rate-limited (HTTP 429), so lookups went by ID.
- **mmore defaults** were checked in the local code: dense `BAAI/bge-large-en-v1.5` (`/home/yassine/Desktop/light/mmore/examples/pubmedqa/index_bge.yaml`), dense `intfloat/e5-large-v2` in the WHO example (`examples/who/index/index_config_e5.yaml`), sparse `naver/splade-cocondenser-selfdistil` (`src/mmore/rag/model/sparse/base.py:8`), reranker `BAAI/bge-reranker-base` (`src/mmore/rag/retriever.py:39`). Hybrid fusion is pymilvus `WeightedRanker`, a weighted score combination, **not RRF** (`src/mmore/rag/retriever.py:247`, `hybrid_search_weight: float = 0.5` at line 35).

## Q1. Correct venues and real citation counts for the listed papers

### Takeaway
All 16 listed papers exist and their venues are correct. The "from memory" buckets were too low for several papers: DPR has about 7.4k citations (listed 1k+), BGE M3 about 2.1k (listed 100+), RAGAS about 1.2k (listed 100+), MIRAGE about 675 and ARES about 467 (listed 100+). Small fixes are needed to some titles (ColBERT, RRF, BGE M3, BEIR spelling). Three entries are arXiv-only: Nogueira & Cho, Gao et al., and the two 2026 papers.

### Cited Findings

**Section 1: Retrieval foundations**

| # | Exact title (as indexed) | Authors / year | Verified venue | Citations (S2, 2026-10-22) | List said | Flag |
|---|---|---|---|---|---|---|
| 1 | Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks | Lewis et al., 2020 | NeurIPS 2020 (DBLP conf/nips) | **19,864** | 10k+ | OK |
| 2 | Dense Passage Retrieval for Open-Domain Question Answering | Karpukhin et al., 2020 | EMNLP 2020 (DOI 10.18653/v1/2020.emnlp-main.550) | **7,416** | 1k+ | Bucket far too low |
| 3 | ColBERT: Efficient and Effective Passage Search via Contextualized Late Interaction **over BERT** | Khattab & Zaharia, 2020 | SIGIR 2020 (DOI 10.1145/3397271.3401075) | **2,681** | 1k+ | List drops "over BERT" from the title |
| 4 | M3-Embedding: Multi-Linguality, Multi-Functionality, Multi-Granularity Text Embeddings Through Self-Knowledge Distillation | Jianlv Chen et al., 2024 | Findings of ACL 2024 (DOI 10.18653/v1/2024.findings-acl.137) | **2,053** | 100+ | Bucket an order of magnitude too low. The arXiv title starts "BGE M3-Embedding", the Anthology title starts "M3-Embedding" |
| 5 | Reciprocal Rank Fusion outperforms Condorcet and Individual Rank Learning Methods | Cormack, Clarke & Büttcher, 2009 | SIGIR 2009 (DOI 10.1145/1571941.1572114) | **1,496** | 1k+ | List gives only the short name. mmore does **not** use RRF (see method note) |
| 6 | Passage Re-ranking with BERT | Nogueira & Cho, 2019 | **arXiv only** (DBLP journals/corr) | **1,700** | 1k+ | Not peer reviewed. See Q3 for a peer-reviewed companion |
| 6b | From Distillation to Hard Negative Sampling: Making Sparse Neural IR Models More Effective (SPLADE++) | Formal et al., 2022 | SIGIR 2022, short paper (DOI 10.1145/3477495.3531857) | **245** | — | Correct. This is the paper behind mmore's `splade-cocondenser-selfdistil` checkpoint |

Sources: [S2 RAG](https://api.semanticscholar.org/graph/v1/paper/arXiv:2005.11401?fields=title,venue,citationCount); [S2 DPR](https://api.semanticscholar.org/graph/v1/paper/arXiv:2004.04906?fields=title,venue,citationCount); [S2 ColBERT](https://api.semanticscholar.org/graph/v1/paper/arXiv:2004.12832?fields=title,venue,citationCount); [S2 M3](https://api.semanticscholar.org/graph/v1/paper/arXiv:2402.03216?fields=title,venue,citationCount); [S2 RRF](https://api.semanticscholar.org/graph/v1/paper/DOI:10.1145/1571941.1572114?fields=title,venue,citationCount); [S2 Nogueira & Cho](https://api.semanticscholar.org/graph/v1/paper/arXiv:1901.04085?fields=title,venue,citationCount); [S2 SPLADE++](https://api.semanticscholar.org/graph/v1/paper/arXiv:2205.04733?fields=title,venue,citationCount)

**Section 2: Evaluation and benchmarks**

| # | Exact title | Authors / year | Verified venue | Citations (S2) | List said | Flag |
|---|---|---|---|---|---|---|
| 7 | BEIR: A **Heterogenous** Benchmark for Zero-shot Evaluation of Information Retrieval Models | Thakur et al., 2021 | NeurIPS 2021 Datasets & Benchmarks | **2,178** | 1k+ | The official title spells it "Heterogenous". Copy it verbatim |
| 8 | Benchmarking Retrieval-Augmented Generation for Medicine | Guangzhi Xiong, Qiao Jin, Zhiyong Lu, Aidong Zhang, 2024 | Findings of ACL 2024, pp. 6233–6251 ([ACL Anthology](https://aclanthology.org/2024.findings-acl.372/)) | **675** | 100+ | Bucket too low |
| 9 | Healthier LLMs: Retrieval-Augmented Generation for Public Health Question Answering | Felix Feldman, Joshua Harris, Timothy Laurence et al. (10 authors), posted 7 Jul 2026 | **arXiv 2607.06641** only. The comments field names no venue ([arXiv](https://arxiv.org/abs/2607.06641)) | **0** | New | Correct as "New" |
| 10 | RAGAs: Automated Evaluation of Retrieval Augmented Generation | Shahul Es, Jithin James, Luis Espinosa Anke, Steven Schockaert | EACL 2024 System Demonstrations, pp. 150–158 (per DBLP, via [search](https://aclanthology.org/2024.eacl-demo.0)) | **1,228** | 100+ | Bucket too low. Semantic Scholar dates it 2023 (the arXiv year). Cite it as 2024 |
| 11 | ARES: An Automated Evaluation Framework for Retrieval-Augmented Generation Systems | Saad-Falcon, Khattab, Potts, Zaharia, 2024 | NAACL 2024 (DBLP conf/naacl/Saad-FalconKPZ24) | **467** | 100+ | Bucket too low |

Healthier LLMs content, from the abstract: it extends PubHealthBench (7,929 questions from UK Government public-health guidance) to a RAG setting and compares dense, sparse and hybrid retrieval across several embedding models and corpus variants. Hybrid retrieval "consistently improves recall and ranking quality". Retrieved context lets smaller open-weight models match or beat larger models used without retrieval. It also adds a rubric-based LLM-as-a-judge, which agrees best with human annotators on faithfulness and completeness and less well on factual consistency and clarity. — [arXiv 2607.06641](https://arxiv.org/abs/2607.06641)

Sources: [S2 BEIR](https://api.semanticscholar.org/graph/v1/paper/arXiv:2104.08663?fields=title,venue,citationCount); [S2 MIRAGE](https://api.semanticscholar.org/graph/v1/paper/arXiv:2402.13178?fields=title,venue,citationCount); [S2 Healthier LLMs](https://api.semanticscholar.org/graph/v1/paper/arXiv:2607.06641?fields=title,venue,citationCount); [S2 RAGAS](https://api.semanticscholar.org/graph/v1/paper/arXiv:2309.15217?fields=title,venue,citationCount); [S2 ARES](https://api.semanticscholar.org/graph/v1/paper/arXiv:2311.09476?fields=title,venue,citationCount)

**Section 3: Surveys**

| # | Exact title | Authors / year | Verified venue | Citations (S2) | List said | Flag |
|---|---|---|---|---|---|---|
| 12 | Retrieval-Augmented Generation for Large Language Models: A Survey | Yunfan Gao et al., 2023 | **arXiv only** (2312.10997) | **4,328** | 1k+ | Not peer reviewed but very widely cited. Keep, and pair it with Fan et al. (KDD 2024) |
| 13 | Searching for Best Practices in Retrieval-Augmented Generation | Xiaohua Wang et al., 2024 | EMNLP 2024 main (DBLP conf/emnlp) | **209** | 100+ | OK |
| 14 | Ask in Any Modality: A Comprehensive Survey on Multimodal Retrieval-Augmented Generation | Abootorabi et al. (8 authors), 2025 | Findings of ACL 2025 ([2025.findings-acl.861](https://aclanthology.org/2025.findings-acl.861/), per the [authors' README](https://github.com/llm-lab-org/Multimodal-RAG-Survey)). The arXiv comments field names no venue ([arXiv](https://arxiv.org/abs/2502.08826)) | **85** | New | OK. The list's earlier "ACL 2025" label should read "Findings of ACL 2025" |
| 15 | Unlocking Multimodal Document Intelligence: From Current Triumphs to Future Frontiers of Visual Document Retrieval | Yibo Yan, Jiahao Huo et al. (15 authors), Feb 2026 | **arXiv 2602.19961**, marked "under review" ([search summary of arXiv page](https://arxiv.org/abs/2602.19961v1)) | **9** | New | arXiv only |

Sources: [S2 Gao](https://api.semanticscholar.org/graph/v1/paper/arXiv:2312.10997?fields=title,venue,citationCount); [S2 Wang](https://api.semanticscholar.org/graph/v1/paper/arXiv:2407.01219?fields=title,venue,citationCount); [S2 Abootorabi](https://api.semanticscholar.org/graph/v1/paper/arXiv:2502.08826?fields=title,venue,citationCount); [S2 Yan](https://api.semanticscholar.org/graph/v1/paper/arXiv:2602.19961?fields=title,venue,citationCount)

- For context, MMORE itself (arXiv 2509.11937) has **3** Semantic Scholar citations. — [S2](https://api.semanticscholar.org/graph/v1/paper/arXiv:2509.11937?fields=title,venue,citationCount)

### Inferences
- Put an exact number with a date in the "Citations" column instead of buckets (for example "7.4k (S2, Oct 2026)"). The buckets understated most papers and, in a few cases, would mislead a reader who ranks by citations (BGE M3 at 100+ looks minor but is actually about 2k).
- Change the RRF "Why read it" text. mmore's hybrid search uses a weighted score sum (`WeightedRanker`), so RRF is the alternative fusion method the thesis should compare against, not a description of what mmore does. A fusion-function comparison paper (Bruch et al., Q2) becomes Core.
- Of the arXiv-only entries, Nogueira & Cho and Gao et al. are justified by citation count. The two 2026 papers are justified only because they are directly on topic.

### Gaps
- Google Scholar counts were not collected (no API, rate limits). The S2 numbers are lower bounds compared with GS.
- I did not open the RAGAS paper's own ACL Anthology page. The page range comes from DBLP via a search snippet.

## Q2. Missing foundational and benchmark papers (especially multilingual retrieval, embeddings and rerankers)

### Takeaway
The biggest gaps are: (a) the papers behind the models mmore actually runs (C-Pack/BGE, E5, SPLADE), (b) BM25 itself, (c) the fusion-function comparison that matches mmore's weighted hybrid, (d) multilingual retrieval benchmarks and models (MIRACL, MMTEB, multilingual E5, mMARCO), (e) a peer-reviewed reranker paper (monoT5) plus one LLM reranker (RankGPT), and (f) multilingual RAG evaluation (BERGEN, NoMIRACL, MIRAGE-Bench). The WHO example config uses `e5-large-v2` and the SPLADE checkpoint is English-only. Both are a concrete risk for French documents, and the thesis should cite multilingual alternatives.

### Cited Findings
All citation counts are from Semantic Scholar on 2026-10-22, via the batch endpoint ([S2 API docs](https://api.semanticscholar.org/api-docs/graph)). Priority: Core = read fully, Skim = abstract, figures and conclusion, Ref = cite only.

**Section 1 additions (retrieval foundations)**

| Paper | Venue | Cites | Relevance to thesis | Priority |
|---|---|---|---|---|
| The Probabilistic Relevance Framework: BM25 and Beyond (Robertson & Zaragoza, 2009) | Foundations and Trends in IR (DOI 10.1561/1500000019) | 6,665 | The canonical BM25 reference. The sparse baseline and every "BM25 is hard to beat" claim need it | **Core** (only the BM25 sections). It is the standard citation and nothing supersedes it |
| SPLADE: Sparse Lexical and Expansion Model for First Stage Ranking (Formal, Piwowarski, Clinchant, 2021) | SIGIR 2021 (DOI 10.1145/3404835.3463098) | 723 | The original learned-sparse model. SPLADE++ only makes sense after it | **Skim**. Cite with SPLADE++. SPLADE v2 (arXiv only, 277) can be dropped |
| C-Pack: Packed Resources For General Chinese Embeddings (Xiao et al.) | SIGIR 2024 (DOI 10.1145/3626772.3657878) | 1,054 | The paper behind the BGE models, including mmore's default `bge-large-en-v1.5` | **Skim**. Required to cite the default dense model properly |
| Text Embeddings by Weakly-Supervised Contrastive Pre-training (E5; L. Wang et al., 2022) | arXiv only | 1,789 | mmore's WHO example uses `e5-large-v2` | **Ref**. Cite because the model is used, despite being arXiv only |
| Multilingual E5 Text Embeddings: A Technical Report (L. Wang et al., 2024) | arXiv only (technical report) | 669 | A multilingual drop-in replacement for e5-large-v2 on French documents. MMTEB names multilingual-e5-large-instruct the best public model ([ICLR 2025](https://proceedings.iclr.cc/paper_files/paper/2025/hash/fc0e3f908a2116ba529ad0a1530a3675-Abstract-Conference.html)) | **Skim**. It is the main competitor to BGE-M3 in the config grid |
| Unsupervised Dense Information Retrieval with Contrastive Learning (Contriever; Izacard et al.) | TMLR 2022 | 1,796 | Showed unsupervised dense retrieval competitive with BM25, with a multilingual variant (mContriever) | **Ref**. Background only, since retriever training is out of scope |
| ColBERTv2: Effective and Efficient Retrieval via Lightweight Late Interaction (Santhanam et al.) | NAACL 2022 (DOI 10.18653/v1/2022.naacl-main.272) | 924 | The late-interaction version people actually run today. The late-interaction arm of the grid will use ColBERTv2-style models | **Skim** |
| PLAID: An Efficient Engine for Late Interaction Retrieval (Santhanam et al.) | CIKM 2022 | 210 | The efficiency engine for ColBERTv2. Useful only if the thesis reports latency or cost | **Ref** |
| Jina-ColBERT-v2: A General-Purpose Multilingual Late Interaction Retriever (Jha et al., 2024) | MRL workshop 2024 (per S2) | 46 | A multilingual late-interaction option for French. BGE-M3's multi-vector output is the other one | **Ref**. Workshop venue and low citations, so cite only if used |
| An Analysis of Fusion Functions for Hybrid Retrieval (Bruch, Gai, Ingber) | ACM TOIS (DOI 10.1145/3596512) | 133 | Compares convex (weighted) combination with RRF, exactly mmore's `WeightedRanker` against the list's RRF | **Core**. It decides how to frame the hybrid arm and its weight |
| Document Ranking with a Pretrained Sequence-to-Sequence Model (monoT5; Nogueira, Jiang, Pradeep, Lin) | Findings of EMNLP 2020 (DOI 10.18653/v1/2020.findings-emnlp.63) | 911 | A peer-reviewed cross-encoder or seq2seq reranker. Fixes the arXiv-only reranker citation | **Skim** |
| Is ChatGPT Good at Search? Investigating LLMs as Re-Ranking Agent (RankGPT; Sun et al.) | EMNLP 2023 | 669 | The standard reference for LLM listwise reranking, the heavy-reranker option | **Skim** |
| RankZephyr: Effective and Robust Zero-Shot Listwise Reranking is a Breeze! (Pradeep et al.) | arXiv only | 262 | An open-weight LLM reranker | **Ref**, optional |
| Qwen3 Embedding: Advancing Text Embedding and Reranking Through Foundation Models (Zhang et al., 2025) | arXiv only | 1,726 | Current multilingual open embeddings and rerankers. A strong 2025 baseline candidate | **Ref**, only if used in the grid |
| mGTE: Generalized Long-Context Text Representation and Reranking Models for Multilingual Text Retrieval (X. Zhang et al.) | EMNLP 2024 (S2 venue) | 399 | Multilingual embedding and reranker pair | **Ref**, optional |
| Sentence-BERT (Reimers & Gurevych) | EMNLP-IJCNLP 2019 | 21,503 | The bi-encoder and sentence-transformers library that mmore's dense and rerank models load through | **Ref** |

**Section 2 additions (evaluation and benchmarks)**

| Paper | Venue | Cites | Relevance | Priority |
|---|---|---|---|---|
| MS MARCO: A Human Generated MAchine Reading COmprehension Dataset (Nguyen/Campos et al., 2016) | CoCo@NIPS 2016 workshop | 3,725 | The training and evaluation source of almost every retriever and reranker in the grid, including SPLADE++ and bge-reranker | **Ref** |
| MIRACL: A Multilingual Retrieval Dataset Covering 18 Diverse Languages (X. Zhang, Thakur et al.) | TACL 2023, vol. 11, pp. 1114–1131 ([ACL Anthology](https://aclanthology.org/2023.tacl-1.63)) | 239 (TACL record; the arXiv record has another 76) | The standard multilingual retrieval benchmark (monolingual retrieval in 18 languages; it includes French). Shows how retrievers rank outside English | **Core** for the multilingual claim |
| MTEB: Massive Text Embedding Benchmark (Muennighoff et al.) | EACL 2023 (DOI 10.18653/v1/2023.eacl-main.148) | 1,128 | The leaderboard used to pick embedding models. Shows that no single embedding model wins every task | **Skim** |
| MMTEB: Massive Multilingual Text Embedding Benchmark (Enevoldsen et al.) | ICLR 2025 ([proceedings](https://proceedings.iclr.cc/paper_files/paper/2025/hash/fc0e3f908a2116ba529ad0a1530a3675-Abstract-Conference.html)) | 195 | The multilingual extension (500+ tasks) | **Skim**. Its finding that multilingual-e5-large-instruct is the best public model directly informs model choice |
| mMARCO: A Multilingual Version of the MS MARCO Passage Ranking Dataset (Bonifacio et al., 2021) | Venue not verified (S2 lists none) | 178 | MS MARCO machine-translated into several languages including French. Shows the effect of translated training data on multilingual rerankers | **Ref**. Venue still to verify |
| Mr. TyDi: A Multi-lingual Benchmark for Dense Retrieval (Zhang et al.) | MRL workshop 2021 | 184 | The predecessor of MIRACL | Do not add (superseded by MIRACL) |
| TREC-COVID (Voorhees et al.) | SIGIR Forum 2020 | 99 | A health retrieval collection, and part of BEIR | **Ref**. The health-domain retrieval precedent |
| MedCPT: Contrastive Pre-trained Transformers with Large-scale PubMed Search Logs for Zero-shot Biomedical IR (Jin et al.) | Bioinformatics 2023 | 365 | The biomedical retriever used in MIRAGE/MedRAG. A domain-specific baseline | **Ref** (Skim if a biomedical retriever enters the grid) |
| PubMedQA: A Dataset for Biomedical Research Question Answering (Jin et al.) | EMNLP-IJCNLP 2019 | 2,026 | The only benchmark MMORE was evaluated on. The thesis must cite it | **Ref** |
| Benchmarking Large Language Models in Retrieval-Augmented Generation (RGB; Chen et al.) | AAAI 2024 | 717 | Tests noise robustness, negative rejection and counterfactual robustness, with English and Chinese questions | **Skim** |
| CRAG – Comprehensive RAG Benchmark (Yang et al.) | NeurIPS 2024 (Datasets & Benchmarks per S2) | 158 | A large RAG benchmark with a multi-tier scoring scheme | **Ref** |
| KILT: a Benchmark for Knowledge Intensive Language Tasks (Petroni et al.) | NAACL 2021 | 898 | The first standard for joint retrieval and answer evaluation (provenance) | **Ref** |
| BERGEN: A Benchmarking Library for Retrieval-Augmented Generation (Rau et al., NAVER Labs Europe) | Findings of EMNLP 2024 (S2 venue EMNLP; DBLP key conf/emnlp) | 48 | The closest toolkit-benchmark analogue to "benchmark mmore's configurations", with a multilingual extension | **Core**. It is the methodological template for a RAG-toolkit benchmark |
| Retrieval-augmented generation in multilingual settings (Chirkova et al.) | KnowLLM workshop @ ACL 2024 | 57 | Builds multilingual RAG on BERGEN (MKQA and similar, includes French). Reports that retrieval, prompting and evaluation need language-specific changes | **Skim**. Directly on topic despite the workshop venue |
| "Knowing When You Don't Know": A Multilingual Relevance Assessment Dataset for Robust RAG (NoMIRACL; Thakur et al.) | Findings of EMNLP 2024 ([search summary](https://www.arxiv.org/abs/2312.11361v2)) | 16 (likely split across arXiv and Anthology records) | Multilingual RAG robustness when retrieved passages are non-relevant | **Ref** |
| MIRAGE-Bench: Automatic Multilingual Benchmark Arena for RAG Systems (Thakur et al.) | NAACL 2025 (DBLP conf/naacl/ThakurKLLA25) | 25 | Multilingual RAG evaluation with an LLM-judge arena. Note the name clash with Xiong's MIRAGE | **Ref** |
| MKQA (Longpre et al.) | TACL 2021 | 242 | A multilingual open-domain QA set (includes French) used by BERGEN | **Ref** |
| MedExpQA: Multilingual Benchmarking of LLMs for Medical QA (Alonso et al.) | Artificial Intelligence in Medicine 2024 | 106 | Multilingual medical QA (includes French) that tests RAG | **Skim**. The closest French and medical RAG evaluation found |
| Towards building multilingual language model for medicine (MMedBench; Qiu et al.) | Nature Communications 2024 | 247 | Multilingual medical QA benchmark (includes French) | **Ref** |
| Judging LLM-as-a-judge with MT-Bench and Chatbot Arena (Zheng et al.) | NeurIPS 2023 (D&B) | 12,409 | The canonical LLM-as-judge reference behind RAGAS, ARES and the Healthier LLMs rubric judge | **Skim** |
| Large Language Models can Accurately Predict Searcher Preferences (Thomas, Spielman, Craswell, Mitra) | SIGIR 2024 | 313 | LLM relevance labels agree with human labels. This justifies LLM-labelled qrels when building the humanitarian test set | **Core**. The oracle experiment needs relevance labels |
| UMBRELA (Upadhyay et al.) | arXiv only | 110 | Open-source reproduction of the Bing LLM assessor | **Ref**, only if used for qrels |
| Ragnarök: A Reusable RAG Framework and Baselines for TREC 2024 RAG Track (Pradeep et al.) | ECIR 2025 | 60 | The TREC RAG evaluation setup | **Ref** |
| RAGChecker: A Fine-grained Framework for Diagnosing RAG (Ru et al.) | NeurIPS 2024 (D&B per DBLP conf/nips) | 108 | Separates retriever errors from generator errors. Peer-reviewed alternative to RAGAS | **Skim** |
| Evaluating Open-Domain QA in the Era of LLMs (Kamalloo et al.) | ACL 2023 | 207 | Lexical-match metrics (EM, F1) underrate LLM answers. Argues for semantic or LLM judging of free-form answers | **Ref** |
| FlashRAG: A Modular Toolkit for Efficient RAG Research (Jin et al.) | WWW 2025 (DBLP conf/www) | 256 | A competing open RAG toolkit that mmore should be positioned against | **Ref** |

**Section 3 additions (surveys)**

| Paper | Venue | Cites | Relevance | Priority |
|---|---|---|---|---|
| A Survey on RAG Meeting LLMs: Towards Retrieval-Augmented Large Language Models (Fan et al.) | KDD 2024 (DOI 10.1145/3637528.3671470) | 1,225 | A peer-reviewed general RAG survey to sit beside the arXiv-only Gao et al. | **Skim** |
| Evaluation of Retrieval-Augmented Generation: A Survey (Yu et al.) | Springer chapter (DOI 10.1007/978-981-96-1024-2_8; the CCF Big Data 2024 proceedings by the DOI prefix, not verified) | 348 | Surveys RAG evaluation targets and metrics. Useful map for section 2 | **Skim** |

### Inferences
- **Which embedding model is a thesis-relevant gap, not just a citation gap.** mmore's WHO example (`examples/who/index/index_config_e5.yaml`) uses English `e5-large-v2`, and the sparse checkpoint `naver/splade-cocondenser-selfdistil` is an English MS MARCO model. On French humanitarian documents the grid should include a multilingual dense model (BGE-M3 and/or multilingual-E5) and evaluate on MIRACL-style multilingual retrieval. Otherwise the sparse-vs-dense comparison mixes up retrieval architecture with language coverage. This is my inference from model names and training data. I did not test the checkpoints on French.
- The most valuable additions, in order: Robertson & Zaragoza (BM25), Bruch et al. (fusion functions), MIRACL, BERGEN plus Chirkova et al., Thomas et al. (LLM relevance labels), C-Pack, multilingual E5, monoT5.
- The list has no French-specific RAG evaluation benchmark of high standing. MedExpQA and MMedBench (medical, multilingual including French) and Chirkova et al. (multilingual RAG) are the closest peer-reviewed items found.

### Gaps
- No well-cited French-only RAG or retrieval benchmark was found in this pass. MTEB-French (arXiv 2405.20468) has only 7 S2 citations and no confirmed venue, so I do not recommend it.
- mMARCO's peer-reviewed venue was not verified (S2 lists none). Check DBLP before citing it with a venue.
- No humanitarian-domain retrieval benchmark was found. That matches the list's "Closest work" claim, but it rests on absence of evidence.
- Whether `BAAI/bge-reranker-base` has its own paper was not verified. It is usually cited through C-Pack or the FlagEmbedding repository.

## Q3. Weak or redundant entries

### Takeaway
No entry should be removed outright. Nogueira & Cho (arXiv only) should be paired with or replaced by monoT5 (Findings of EMNLP 2020). Gao et al. (arXiv only) should be paired with Fan et al. (KDD 2024). The Yan et al. 2026 survey (arXiv, 9 citations, under review) duplicates Abootorabi for this thesis's scope and could drop to Ref. SPLADE v2 and Mr. TyDi should not be added because SPLADE++ and MIRACL supersede them.

### Cited Findings
- Nogueira & Cho 2019 has no peer-reviewed version (DBLP key journals/corr only, 1,700 citations). The same group's monoT5 paper is in Findings of EMNLP 2020 (911 citations). — [S2](https://api.semanticscholar.org/graph/v1/paper/arXiv:1901.04085?fields=title,venue,citationCount); [S2 monoT5](https://api.semanticscholar.org/graph/v1/paper/arXiv:2003.06713?fields=title,venue,citationCount)
- Gao et al. is arXiv only with 4,328 citations. Fan et al. covers the same ground at KDD 2024 with 1,225 citations. — [S2 Gao](https://api.semanticscholar.org/graph/v1/paper/arXiv:2312.10997?fields=title,venue,citationCount); [S2 Fan](https://api.semanticscholar.org/graph/v1/paper/arXiv:2405.06211?fields=title,venue,citationCount)
- Yan et al. 2026 is arXiv only, "under review", with 9 citations, and covers visual document retrieval, which is excluded from this thesis's own scope. Abootorabi et al. (Findings of ACL 2025, 85 citations) already covers multimodal RAG. — [S2 Yan](https://api.semanticscholar.org/graph/v1/paper/arXiv:2602.19961?fields=title,venue,citationCount); [ACL Anthology](https://aclanthology.org/2025.findings-acl.861/)
- Healthier LLMs is arXiv only with 0 citations, but it is the most on-topic paper in the section: it finds that hybrid retrieval consistently improves recall and ranking on public-health guidance. — [arXiv](https://arxiv.org/abs/2607.06641)
- RAGAS and ARES both cover reference-free automated RAG evaluation. RAGAS is a demo paper (1,228 citations) and ARES is a NAACL main paper (467). RAGChecker (NeurIPS 2024, 108) is a third option. — S2 links above

### Inferences
- Keep RAGAS and ARES, but reword the RAGAS "Why read it" text to make clear it is a tool or demo paper rather than a validated metric study. If one evaluation framework has to go, demote RAGAS to Ref.
- Keep Healthier LLMs as Core rather than Skim. Its hybrid-retrieval result and its rubric judge validated against humans are the nearest precedent for the thesis's health-domain setting, despite the arXiv-only status.
- Reword the RRF entry, since mmore uses weighted fusion, and promote Bruch et al. to Core.

### Gaps
- Citation trajectories for the 2026 arXiv papers cannot be judged yet. Recheck before submission.
