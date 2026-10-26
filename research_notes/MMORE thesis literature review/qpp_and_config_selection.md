# QPP, per-query configuration selection, algorithm selection and RAG hyper-parameter tuning ("Earlier and adjacent work" audit)

Method and provenance. Citation counts come from two sources, both queried on 2026-10-22:
- **S2**: Semantic Scholar Graph API, `POST /graph/v1/paper/batch` by DOI or arXiv ID (https://api.semanticscholar.org/graph/v1/paper/batch).
- **OA**: OpenAlex `/works?search=` (https://api.openalex.org/works).

Google Scholar was not queried, so its counts (usually higher) are a gap. S2 and OA sometimes disagree a lot (example: Hauff 2008 has S2 58 and OA 161). Both numbers are given where available. "Venue verified" means checked on a publisher, proceedings or anthology page, not just a database label.

## Are the venues and metadata correct? Current citation counts?

### Takeaway
All 19 papers exist and the authors match. Five entries need corrections:
- **Tian et al.:** now ECIR 2026, not arXiv-only.
- **Dado et al.:** to appear at CIKM 2026; the DOI does not resolve yet.
- **R1-Router:** retitled on arXiv and reported as accepted at SIGIR 2026.
- **Lumer et al.:** published at ICAART 2026 under a slightly different title.
- **RouteLLM:** the ICLR title says "from", not "with", Preference Data.

DAT and RAGSmith remain arXiv-only. The Orbach et al. workshop venue rests only on the arXiv comment.

### Cited Findings

| # | Paper (as listed) | Verified metadata / correction | S2 cites | OA cites |
|---|---|---|---|---|
| 1 | Hauff, Hiemstra & de Jong, A Survey of Pre-retrieval QPP, CIKM 2008 | Correct. DOI 10.1145/1458082.1458311 | 58 | 161 |
| 2 | Arabzadeh, Yan & Clarke, CIKM 2021 | Correct. DOI 10.1145/3459637.3482159 | 62 | 40 |
| 3 | Peng, Macdonald & Ounis, Learning to Select a Ranking Function, ECIR 2010 | Correct (LNCS). DOI 10.1007/978-3-642-12275-0_13 | 32 | 36 |
| 4 | Cronen-Townsend, Zhou & Croft, Predicting Query Performance, SIGIR 2002 | Correct. DOI 10.1145/564376.564429 | 839 | 669 |
| 5 | Kerschke et al., Automated Algorithm Selection: Survey and Perspectives | Evolutionary Computation 27(1). Online 2018, issue 2019; S2 and OA say 2018. Authors: Kerschke, Hoos, Neumann, Trautmann | 508 | 439 |
| 6 | Hsu & Tzeng, DAT | **Still arXiv-only** (2503.23013); no venue in S2 or OA | 20 | 5 |
| 7 | Lee et al., RouterRetriever, AAAI 2025 | Correct: AAAI 39(11). DOI 10.1609/aaai.v39i11.33306. Authors: Hyunji Lee, Luca Soldaini, Arman Cohan, et al. | 17 | 3 |
| 8 | Tang et al., MBA-RAG, COLING 2025 | Correct: ACL Anthology 2025.coling-main.218, pp. 3248–3254 | 43 | n/a (OA search missed it) |
| 9 | Taguchi et al., Just Adaptive-k, EMNLP 2025 | Correct: EMNLP 2025 main. DOI 10.18653/v1/2025.emnlp-main.1017. Full title: "Efficient Context Selection for Long-Context QA: No Tuning, No Iteration, Just Adaptive-k". A journal version also exists in J. Natural Language Processing (2026) | 29 | 8 |
| 10 | Ray et al., METIS, SOSP 2025 | Correct. DOI 10.1145/3731569.3764855 | 29 | 9 |
| 11 | Orbach et al., HPO for RAG | arXiv 2505.03452 v3. The comments field says AAAI 2026 Workshop on New Frontiers in IR. No workshop proceedings page found. 15 IBM authors | 3 | 0 |
| 12 | Fu et al., AutoRAG-HP, Findings EMNLP 2024 | Correct. DOI 10.18653/v1/2024.findings-emnlp.223 | 21 | 8 |
| 13 | Kartal et al., RAGSmith | **arXiv-only** (2511.01386) | 6 | 0 |
| 14 | Dado, Carmel & Kurland, Predicting the Benefit of Retrieval Augmentation in ODQA | arXiv 2604.07985. ACM DOI 10.1145/3799682.3840689 is assigned but **returns HTTP 404** (not registered yet). A post by co-author David Carmel says it will be presented at **CIKM 2026**. Venue is not verified on an ACM page | 1 | 0 |
| 15 | Tian, Ganguly & Macdonald, Predicting Retrieval Utility and Answer Quality in RAG | **Accepted at ECIR 2026** (LNCS). DOI 10.1007/978-3-032-21289-4_24. First author is **Fangzheng** Tian. The list says "arXiv", which is now outdated | n/a (DOI not indexed) | 3 |
| 16 | Sinha & Chakma, RAG-QPP, ACM TOIS 2026 | Correct. Full title: "Adaptive Query Performance Prediction for Retrieval-Augmented Generation: Bridging Retrieval Quality and Generation Relevance". DOI 10.1145/3827605 | 2 | 0 |
| 17 | Lumer et al., Text- vs Image-Based Retrieval in Multimodal RAG | Published at **ICAART 2026** as "...in **Modern** Multimodal RAG LLM Systems". DOI 10.5220/0014479300004052 | 6 | 1 |
| 18 | Peng et al., R1-Router | arXiv 2505.22095 **v2 (6 Apr 2026) was retitled** "Mixture-of-Retrieval Experts for Reasoning-Guided Multimodal Knowledge Exploitation" (MoRE). S2 tags it SIGIR. A repository mirror says it was accepted at SIGIR 2026. Not verified on the official SIGIR proceedings | 8 | n/a |
| 19 | Ong et al., RouteLLM, ICLR 2025 | ICLR 2025, but the proceedings title is "RouteLLM: Learning to Route LLMs **from** Preference Data" (arXiv uses "with") | 875 | 14 (arXiv record only) |

Sources for the venue corrections:
- **Dado et al.:** [arXiv 2604.07985](https://arxiv.org/abs/2604.07985); [Technion seminar page](https://dds.technion.ac.il/seminars/graduate-student-seminar/rag-performance-prediction-in-question-answering/). The CIKM 2026 claim comes from a LinkedIn post that surfaced in search; the DOI returned 404 on 2026-10-22.
- **Tian et al.:** [ECIR 2026 abstract archive](https://ecir2026.dryfta.com/programme/abstract-archive/abstract/public/42/predicting-retrieval-utility-and-answer-quality-in-retrieval-augmented-generation); [Glasgow eprints](https://eprints.gla.ac.uk/374630); [arXiv 2601.14546](https://arxiv.org/abs/2601.14546v1).
- **MBA-RAG:** [ACL Anthology 2025.coling-main.218](https://preview.aclanthology.org/setup/2025.coling-main.218); [arXiv 2412.01572](https://arxiv.org/abs/2412.01572v4).
- **R1-Router / MoRE:** [arXiv 2505.22095](https://arxiv.org/abs/2505.22095); SIGIR 2026 acceptance only via a [repo mirror](https://gh.mise.run.place/OpenBMB/MoRE). Treat it as unverified.
- **Orbach et al.:** [arXiv 2505.03452v3](https://arxiv.org/abs/2505.03452v3).
- **RouteLLM:** [ICLR 2025 proceedings](https://proceedings.iclr.cc/paper_files/paper/2025/hash/5503a7c69d48a2f86fc00b3dc09de686-Abstract-Conference.html).
- **DOIs and counts** for Lumer (ICAART), Adaptive-k, METIS, RouterRetriever and AutoRAG-HP: [OpenAlex API](https://api.openalex.org/works) and [Semantic Scholar batch API](https://api.semanticscholar.org/graph/v1/paper/batch), both 2026-10-22.

Descriptive points worth checking in the list:
- **Dado et al.** A summary of the paper reports that **pre-retrieval QPP-style features were largely ineffective** at predicting whether retrieval helps, and that supervised predictors beat unsupervised ones. This bears directly on RQ1, because it is evidence that query-only prediction is hard. The list's phrase "cheap pre-generation vs costly post-generation" is fine, but the negative result for pre-retrieval features should be stated. — [Emergent Mind summary](https://www.emergentmind.com/papers/2604.07985) (secondary; check against the PDF)
- **Tian et al.** The paper defines two tasks: retrieval performance prediction (RPP) and generation performance prediction (GPP). It uses linear regression over retriever signals, reader perplexity, and document quality and readability features, on Natural Questions only. — [ECIR 2026 abstract](https://ecir2026.dryfta.com/programme/abstract-archive/abstract/public/42/predicting-retrieval-utility-and-answer-quality-in-retrieval-augmented-generation)
- **Orbach et al.** The paper finds greedy or random search sufficient. It also finds that, for greedy search, **optimizing model choice first beats optimizing in pipeline order**. — [arXiv 2505.03452](https://arxiv.org/abs/2505.03452v3)
- **MBA-RAG** penalizes extra retrieval steps in its reward. The list's description is correct. — [arXiv 2412.01572](https://arxiv.org/abs/2412.01572v4)
- **RouteLLM** reports more than 2x cost reduction without quality loss, and routers that transfer to unseen model pairs. — [ICLR 2025 proceedings](https://proceedings.iclr.cc/paper_files/paper/2025/hash/5503a7c69d48a2f86fc00b3dc09de686-Abstract-Conference.html)

### Inferences
- Using S2 as the single source for the list's "Citations" column would be more consistent. Counts in that column should carry the date (2026-10-22), because OA undercounts recent arXiv items and S2 undercounts some older ACM items.
- The **Dado et al.** row ("Carmel co-wrote the QPP textbook") assumes the textbook is on the list, but it is not. Either add Carmel & Yom-Tov (2010) or drop that clause.

### Gaps
- Google Scholar counts were not collected (no API; scraping not attempted).
- CIKM 2026 for Dado et al. and SIGIR 2026 for R1-Router/MoRE are not confirmed on official proceedings pages.
- The Orbach et al. workshop acceptance is confirmed only by the arXiv comment.
- Not re-verified against the PDFs here:
  - Arabzadeh et al. (2021): that the classifier chooses among sparse, dense and hybrid, rather than only whether to add dense.
  - The headline numbers: RouterRetriever +2.1 nDCG@10, Adaptive-k "10x fewer tokens", METIS 1.64–2.54x, RAGSmith +3.8% and 46,080 pipelines, Lumer +13 mAP@5.

## Which seminal QPP / strategy-selection papers are missing?

### Takeaway
The section has no canonical post-retrieval predictors beyond clarity (missing: WIG, NQC, query feedback, ranking robustness), no QPP textbook, and no QPP-for-dense/neural-retriever work. Those are the gaps to close first. On the LLM-routing side, FrugalGPT (cascades) and Hybrid LLM are far better-cited cost-quality references than anything currently listed except RouteLLM. The most on-topic recent finds are Arabzadeh et al., SIGIR 2026 (QPP to choose query variants in RAG pipelines), and Bruch et al., TOIS 2023 (fusion functions for hybrid retrieval).

### Cited Findings

Proposed additions. Counts are S2 / OA as of 2026-10-22.

**Canonical QPP**
- **Zhou & Croft, "Query performance prediction in web search environments", SIGIR 2007**. 314 / 268 citations. DOI 10.1145/1277741.1277835. Introduces **WIG (weighted information gain)** and **query feedback**, two standard post-retrieval baselines. **Core**: these are the post-retrieval signals an RQ2 judge should beat. — [OpenAlex](https://api.openalex.org/works?search=Query%20performance%20prediction%20in%20web%20search%20environments)
- **Shtok, Kurland, Carmel, Raiber & Markovits, "Predicting Query Performance by Query-Drift Estimation", ACM TOIS 2012** (earlier ICTIR 2009 version, LNCS). 301 / 169 citations. DOI 10.1145/2180868.2180873. Introduces **NQC (normalized query commitment)**, the standard deviation of top-k retrieval scores. **Core**: it is computable from mmore's dense and sparse score lists directly, and is the closest classic analogue of Adaptive-k's "shape of the similarity scores". — [OpenAlex](https://api.openalex.org/works?search=Predicting%20query%20performance%20by%20query-drift%20estimation)
- **Carmel & Yom-Tov, "Estimating the Query Difficulty for Information Retrieval", Morgan & Claypool Synthesis Lectures, 2010**. OA 165 (S2 not indexed). DOI 10.2200/s00235ed1v01y201004icr015. The QPP textbook that the Dado row refers to. **Ref**: cite it for definitions of pre- and post-retrieval predictors and for the RQ1/RQ2 framing. — [OpenAlex](https://api.openalex.org/works?search=Estimating%20the%20Query%20Difficulty%20for%20Information%20Retrieval)
- **Yom-Tov, Fine, Carmel & Darlow, "Learning to estimate query difficulty", SIGIR 2005**. 239 / 199 citations. DOI 10.1145/1076034.1076121. A learned difficulty estimator, applied to missing-content detection and to distributed IR (choosing which collections to search). **Skim**: an early instance of predicting difficulty to drive a per-query decision. — [OpenAlex](https://api.openalex.org/works?search=Learning%20to%20estimate%20query%20difficulty)
- **Zhou & Croft, "Ranking robustness: a novel framework to predict query performance", CIKM 2006**. 160 citations (S2; Crossref 95). DOI 10.1145/1183614.1183696. **Ref**: a perturbation-based post-retrieval predictor and the ancestor of the dense-retriever perturbation work below. — [Crossref](https://api.crossref.org/works/10.1145/1183614.1183696)
- **Hauff, "Predicting the effectiveness of queries and retrieval systems", PhD thesis 2009** (abstract in SIGIR Forum 2010). 121 citations (S2). **Ref**, optional. It overlaps with the CIKM 2008 survey, so cite one. — [OpenAlex](https://api.openalex.org/works?search=Predicting%20the%20effectiveness%20of%20queries%20and%20retrieval%20systems)

**QPP for neural and dense retrievers** (the thesis's retriever setting)
- **Faggioli, Formal, Marchesin, Clinchant, Ferro & Piwowarski, "Query Performance Prediction for Neural IR: Are We There Yet?", ECIR 2023**. OA 31 (S2 not resolved by DOI). DOI 10.1007/978-3-031-28244-7_15. Finds that classic QPP works markedly worse on neural (dense and learned-sparse) retrievers than on BM25. **Core**: it warns that classic predictors may not transfer to mmore's dense index. — [OpenAlex](https://api.openalex.org/works?search=Query%20performance%20prediction%20for%20neural%20IR%20are%20we%20there%20yet)
- **Arabzadeh et al., "Noisy Perturbations for Estimating Query Difficulty in Dense Retrievers", CIKM 2023**. 30 / 25 citations. DOI 10.1145/3583780.3615270. **Skim**: a QPP method designed for dense retrievers. — [OpenAlex](https://api.openalex.org/works?search=Learning%20to%20estimate%20query%20difficulty)
- **Vlachou & Macdonald, "Coherence-based Query Performance Measures for Dense Retrieval", ICTIR 2024**. 8 citations (S2). DOI 10.1145/3664190.3672518. **Ref**: post-retrieval predictors built on embedding-space coherence of the top-k results, a cheap RQ2 signal. — [Crossref](https://api.crossref.org/works/10.1145/3664190.3672518)
- **Arabzadeh, Khodabakhsh & Bagheri, "BERT-QPP", CIKM 2021**. 74 / 57 citations. DOI 10.1145/3459637.3482063. **Skim**: a supervised cross-encoder predictor and the standard learned-QPP baseline. — [OpenAlex](https://api.openalex.org/works?search=BERT-QPP)
- **Zamani, Croft & Culpepper, "Neural Query Performance Prediction using Weak Supervision from Multiple Signals", SIGIR 2018**. 117 / 96 citations. DOI 10.1145/3209978.3210041. **Ref**: learns QPP without relevance labels, using classic predictors as weak labels. Useful if humanitarian queries lack labels. — [OpenAlex](https://api.openalex.org/works?search=Neural%20query%20performance%20prediction%20using%20weak%20supervision)

**QPP surveys and tutorials** (2023–2026)
- **Arabzadeh, Meng, Aliannejadi et al., "Query Performance Prediction: Techniques and Applications in Modern Information Retrieval", SIGIR-AP 2024 tutorial**. 12 citations (S2). DOI 10.1145/3673791.3698438. **Ref**: an up-to-date overview, and the nearest thing to a recent QPP survey I found. — [OpenAlex](https://api.openalex.org/works?search=Estimating%20the%20query%20difficulty%20for%20information%20retrieval%20Carmel%20Yom-Tov)

**QPP for RAG and LLM era** (2023–2026; not post-retrieval correction)
- **Arabzadeh, Drozdov, Bendersky & Zaharia, "Can QPP Choose the Right Query Variant? Evaluating Query Variant Selection for RAG Pipelines", SIGIR 2026**. 2 citations (S2). DOI 10.1145/3805712.3808571. Evaluates QPP as a *selector* among alternative query variants in RAG. **Core**: it is structurally the RQ1 problem (choose an option per query with QPP), in RAG, at a top venue. Read before finalizing the oracle-gap analysis. — [Crossref](https://api.crossref.org/works/10.1145/3805712.3808571)
- **Meng, Arabzadeh, Askari, Aliannejadi & de Rijke, "Query Performance Prediction Using Relevance Judgments Generated by LLMs" (QPP-GenRE), ACM TOIS 2025**. 42 citations (S2). DOI 10.1145/3736402. **Skim**: an LLM judges top-k relevance, and QPP is derived from those judgments. This is the direct template for an RQ2 LLM judge, and better-cited and peer-reviewed compared with DAT's similar trick. — [Crossref](https://api.crossref.org/works?query.bibliographic=QPP-GenRE)
- **Chifu et al., "Uncovering the Limitations of Query Performance Prediction: Failures, Insights, and Implications for Selective Query Processing", ACM TOIS 2025**. 5 citations (S2). DOI 10.1145/3774427. **Skim**: the most direct recent study of using QPP for *selective query processing*, which is per-query strategy choice. Its failures are a useful caveat for RQ1. — [OpenAlex](https://api.openalex.org/works?search=Selective%20Query%20Expansion%20predicting%20query%20performance)
- **Tian, Ganguly & Macdonald, "RAQG-QPP", ACM TOIS 2026**, 1 citation (OA), DOI 10.1145/3815198. **Ref only if** the authors' line of work is cited heavily; otherwise skip. — [OpenAlex](https://api.openalex.org/works?search=Adaptive%20query%20performance%20prediction%20for%20retrieval-augmented%20generation)

**Hybrid fusion weights and per-query ranker selection**
- **Bruch, Gai & Ingber, "An Analysis of Fusion Functions for Hybrid Retrieval", ACM TOIS 2023**. 133 / 69 citations. DOI 10.1145/3596512. Shows tuned convex combination (CC) of normalized lexical and semantic scores beats reciprocal rank fusion (RRF) in- and out-of-domain, and that CC is sample-efficient. **Core**: it is the peer-reviewed baseline that DAT's per-query alpha must beat, and it is directly about mmore's hybrid index. — [OpenAlex](https://api.openalex.org/works?search=An%20Analysis%20of%20Fusion%20Functions%20for%20Hybrid%20Retrieval). The claim about CC vs RRF comes from my knowledge of the abstract and is not re-fetched here; check it.
- **Geng, Liu, Qin, Arnold, Li & Shum, "Query Dependent Ranking Using K-Nearest Neighbor", SIGIR 2008**. 107 / 165 citations. DOI 10.1145/1390334.1390356. **Skim**: trains a ranker per query from its nearest training queries. This is the kNN baseline for a query-only router, and is better cited than Peng et al. 2010. — [OpenAlex](https://api.openalex.org/works?search=Query-dependent%20ranking%20using%20k-nearest%20neighbor)

**LLM routing and cascades** (cost-quality curves)
- **Chen, Zaharia & Zou, "FrugalGPT", TMLR** (S2 lists Trans. Mach. Learn. Res.; arXiv 2023). 1186 citations (S2). An LLM cascade with a learned answer scorer that decides whether to stop or escalate. **Skim**: this is the post-retrieval "judge then retry" pattern (RQ2), applied to models. — [OpenAlex](https://api.openalex.org/works?search=FrugalGPT); TMLR venue only per the S2 label, not verified on OpenReview.
- **Ding et al., "Hybrid LLM: Cost-Efficient and Quality-Aware Query Routing", ICLR 2024**. 499 citations (S2). Uses a query-only router with a tunable quality threshold that traces a cost-quality curve. **Skim**: it is the pre-retrieval analogue (RQ1) on the LLM side. — [OpenAlex](https://api.openalex.org/works?search=Hybrid%20LLM%20Cost-Efficient%20and%20Quality-Aware%20Query%20Routing); venue per the S2 label.
- **Hu et al., "RouterBench", arXiv 2024**. 288 citations (S2), no peer-reviewed venue found. Defines router evaluation via the **non-decreasing convex hull / AIQ** of cost-quality points, plus oracle routers. **Ref**: borrow its oracle-router and AIQ vocabulary for the virtual-best vs single-best plot. — [OpenAlex](https://api.openalex.org/works?search=RouterBench)

### Inferences
- The RQ1 vs RQ2 framing paragraph should cite WIG/NQC (post-retrieval) next to Hauff (pre-retrieval) and Cronen-Townsend (clarity). With only clarity, the post-retrieval side looks thin to an IR examiner.
- Faggioli et al. (2023) and Dado et al. (2026) together give a clear motivating tension:
  - classic QPP degrades on neural retrievers;
  - pre-retrieval predictors of retrieval benefit are weak;
  - so a learned query-only router (RQ1) is an open question rather than a solved one.
- Bruch et al. (TOIS 2023) should probably move DAT from Core to Skim. The fixed-alpha convex-combination baseline is the peer-reviewed reference, and DAT is the arXiv per-query extension.

### Gaps
- No dedicated *journal survey* of QPP in 2023–2026 was found; the SIGIR-AP 2024 tutorial is the closest.
- Not searched: "selective hybrid retrieval / when to use dense" papers beyond Arabzadeh 2021 (for example, query-dependent interpolation weights). One Crossref query was rate-limited and is unresolved.
- RouterBench's peer-reviewed status (some cite an ICML 2024 workshop) is not verified.

## Which entries are weak (arXiv-only, low citations, marginal relevance) and could be cut?

### Takeaway
Cut or move four entries. Lumer et al. and R1-Router are text-vs-image or multimodal and belong with RQ3 papers. RAGSmith is arXiv-only with 6 citations and duplicates Orbach et al. AutoRAG-HP could merge into a single "best single configuration" line. Split the section into three short tables of about 7 papers each, keeping the user's "Line of work" labels.

### Cited Findings
- **RAGSmith**: arXiv-only, 6 citations (S2). Covers the same "best single configuration" idea as Orbach et al. (3 citations, workshop) and AutoRAG-HP (21 citations, Findings EMNLP 2024). — [S2 batch API](https://api.semanticscholar.org/graph/v1/paper/batch)
- **Lumer et al.**: 6 citations (S2), ICAART 2026. The list itself notes it uses only 40 questions. It is about text vs image (RQ3), not per-query configuration selection. — [OpenAlex DOI 10.5220/0014479300004052](https://api.openalex.org/works?search=Comparison%20of%20Text-Based%20and%20Image-Based%20Retrieval)
- **R1-Router (now MoRE)**: 8 citations (S2). It is multimodal step-wise routing trained with RL (Step-GRPO), and the paper has been retitled. — [arXiv 2505.22095](https://arxiv.org/abs/2505.22095)
- **DAT**: still arXiv-only, 20 citations (S2). It remains the only paper on per-query hybrid alpha, so keep it, but as Skim with Bruch et al. as Core. — [arXiv 2503.23013](https://arxiv.org/abs/2503.23013)
- **Sinha & Chakma (TOIS 2026)**: 2 citations, but peer-reviewed at a top IR journal. Keep it as Skim. Its 12 post-retrieval features overlap the RQ2 judge. — [Crossref DOI 10.1145/3827605](https://api.crossref.org/works/10.1145/3827605)
- **Peng, Macdonald & Ounis (ECIR 2010)**: 32 citations, peer-reviewed and directly on-topic. Keep it. Geng et al. (SIGIR 2008, 107 citations) is a better-cited companion.
- **Strongest-cited entries now in the section** (S2): Cronen-Townsend (839), RouteLLM (875), Kerschke (508). Every 2025–2026 RAG paper has at most 43 (MBA-RAG).

### Inferences
Proposed restructuring into three tables that keep the existing "Line of work" labels:

1. **Classic IR: QPP and per-query selection.** Hauff 2008; Cronen-Townsend 2002; Zhou & Croft 2007 (WIG/QF, new); Shtok et al. 2012 (NQC, new); Carmel & Yom-Tov 2010 (book, new, Ref); Faggioli et al. 2023 (new); Arabzadeh et al. 2021; Peng et al. 2010; Kerschke et al. 2019.
2. **RAG: per-query configuration and QPP for RAG.**
   - Configuration: DAT (Skim), Bruch et al. 2023 (new, Core), RouterRetriever, MBA-RAG, Adaptive-k, METIS.
   - QPP for RAG: Dado et al. 2026, Tian et al. 2026 (ECIR), Sinha & Chakma 2026, Arabzadeh et al. SIGIR 2026 (new, Core).
   - Best single configuration: Orbach et al. and AutoRAG-HP, merged into one "best single configuration" line.
3. **Method: routing and cost-quality.** RouteLLM, Hybrid LLM (new), FrugalGPT (new), RouterBench (new, Ref).

Moves and cuts:
- Move Lumer et al. and R1-Router/MoRE to the RQ3 / multimodal section.
- Drop RAGSmith, or demote it to a footnote.

The net count goes from 19 to about 25 across three tables. If the user wants to stay near 19, also drop the Hauff PhD thesis, RAQG-QPP, Vlachou & Macdonald, and Zamani et al. These are already marked Ref or optional.

### Gaps
- Relevance judgments are mine and based on abstracts. None of the proposed additions was read in full.
- Counts for 2026 papers will change quickly; recheck them before submission.
