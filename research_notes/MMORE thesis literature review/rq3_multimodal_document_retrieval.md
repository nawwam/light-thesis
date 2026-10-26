# RQ3: Multimodal and visual document retrieval, and document parsing/OCR quality

Citation counts: Semantic Scholar Graph API, `POST /graph/v1/paper/batch` looked up by arXiv ID, queried 2026-10-22 (fields title, year, venue, citationCount). Google Scholar was not queried, and its counts usually run higher. "S2 venue" means the venue string Semantic Scholar returned. Where I confirmed a venue from a second source, the source is linked.

## Q1. Are the venues and citation counts of the listed papers correct?

### Takeaway
All six papers exist and their titles and venues check out. The citation column is badly off in both directions. OHRBench is listed as "100+" but has 44 citations. ColPali (255), VisRAG (329) and DSE (156) are all well above the "100+" the list gives. UniversalRAG has an inconsistent year label in the list.

### Cited Findings
| Listed paper | Verified title / first author | Venue (verified) | S2 citations, 2026-10-22 | List says | Fix |
|---|---|---|---|---|---|
| ColPali (Faysse et al.) | "ColPali: Efficient Document Retrieval with Vision Language Models", Manuel Faysse, arXiv 2407.01449 | ICLR 2025 — [ICLR proceedings](https://proceedings.iclr.cc/paper_files/paper/2025/hash/99e9e141aafc314f76b0ca3dd66898b3-Abstract-Conference.html); [ICLR poster](https://iclr.cc/virtual/2025/poster/28336) | 255 | 100+ | Change to "250+" |
| OCR Hinders RAG / OHRBench (Zhang et al.) | "OCR Hinders RAG: Evaluating the Cascading Impact of OCR on Retrieval-Augmented Generation", Junyuan Zhang, arXiv 2412.02592 | ICCV 2025 — [CVF open access](https://openaccess.thecvf.com/content/ICCV2025/papers/Zhang_OCR_Hinders_RAG_Evaluating_the_Cascading_Impact_of_OCR_on_ICCV_2025_paper.pdf); [GitHub](https://github.com/opendatalab/OHR-Bench) | 44 | 100+ | **Overstated**: change to "40+" |
| ViDoRe V3 (Loison et al.) | "ViDoRe V3: A Comprehensive Evaluation of Retrieval Augmented Generation in Complex Real-World Scenarios", António Loison et al., arXiv 2601.08620 | ACL 2026 main conference, Volume 1 Long Papers. Listed on [ACL Anthology author page](https://aclanthology.org/people/antoine-edy/); S2 also says ACL | 36 | New | Fine; the list's title is shortened ("Evaluation of RAG in…"). Use the full title |
| VisRAG (Yu et al.) | "VisRAG: Vision-based Retrieval-augmented Generation on Multi-modality Documents", Shi Yu, arXiv 2410.10594 | ICLR 2025 (S2) | 329 | 100+ | Change to "300+" |
| DSE (Ma et al.) | "Unifying Multimodal Retrieval via Document Screenshot Embedding", Xueguang Ma, arXiv 2406.11251 | EMNLP 2024 (S2) | 156 | 100+ | Change to "150+" |
| UniversalRAG (Yeo et al.) | "UniversalRAG: Retrieval-Augmented Generation over Corpora of Diverse Modalities and Granularities", Woongyeong Yeo, arXiv 2504.20734 (Apr 2025) | S2 venue: ACL. The list says ACL 2026 | 27 | New | The list mixes "ACL 2026" in the venue column with "Yeo et al., 2025" in the citation. Pick one form, e.g. "(Yeo et al., ACL 2026; arXiv 2025)". I did not confirm the ACL year on the Anthology page itself |

- ViDoRe V3 contents, from the paper itself: 10 corpora, about 26,000 pages and 3,099 human-verified queries, each available in 6 languages. 7 corpora are English and 3 are French (French physics lectures, French public company annual reports, French governmental energy reports). The authors' Limitations section says scanned handwritten notes are not represented — [arXiv 2601.08620 HTML](https://arxiv.org/html/2601.08620v2)
- OHRBench, from the abstract: 8,561 document images and 8,498 Q&A pairs across 7 real-world RAG domains. It studies two kinds of OCR noise, semantic noise and formatting noise. Conclusion: "none is competent for constructing high-quality knowledge bases for RAG systems" — [arXiv 2412.02592](https://arxiv.org/abs/2412.02592). The list's description is accurate.

### Inferences
- If the user ranks papers by citations, OHRBench is weaker than the list implies (44 citations). It still belongs in Core: it is a peer-reviewed ICCV paper and the most direct evidence on OCR noise in RAG.
- The citation buckets should be refreshed from one source with one date. A footnote such as "Semantic Scholar, 2026-10-22" would let the supervisor reproduce them.

### Gaps
- I did not cross-check Google Scholar counts, because of rate limits and because Scholar has no API.
- I did not open the ACL Anthology entry for UniversalRAG directly to confirm "ACL 2026" rather than ACL 2025 or Findings.

## Q2. Which highly cited or top-venue visual document retrieval and document parsing papers are missing?

### Takeaway
The section has no document-parsing toolkits or benchmarks, even though mmore's own ingestion (marker/Surya) is an OCR parser. It also leaves out the multilingual visual-retrieval evidence that bears directly on French documents. The highest-value additions are ViDoRe V2, M3DocRAG, VDocRAG (CVPR), MMDocIR (EMNLP), REAL-MM-RAG (ACL), jina-embeddings-v4, OmniDocBench (CVPR), Nougat (ICLR), MinerU, olmOCR, MIRACL-VISION and MultiOCR-QA (CIKM).

### Cited Findings
All citation counts are from the Semantic Scholar batch API, 2026-10-22. Venue is the S2 venue string unless noted. The arXiv ID resolves each record.

**A. Visual document retrieval and visual RAG**
| Paper | Venue | Cites | Relevance to RQ3 | Priority (reason) |
|---|---|---|---|---|
| ViDoRe Benchmark V2: Raising the Bar for Visual Retrieval (Macé et al., 2025; arXiv 2505.17166) | arXiv | 83 | Harder, more realistic successor to the ColPali ViDoRe benchmark. Bridges V1 and V3 (which is already listed) | Skim (preprint; V3 supersedes it, but it is needed to cite the benchmark lineage) |
| M3DocRAG: Multi-modal Retrieval is What You Need for Multi-page Multi-document Understanding (Cho et al., 2024; 2411.04952) | arXiv (S2) | 140 | ColPali retrieval plus a vision-language model answering over many multi-page documents. Open-domain document question answering over pages, which is the setting mmore targets | Core (highly cited, the canonical multi-document visual RAG pipeline). Venue unverified |
| VDocRAG: Retrieval-Augmented Generation over Visually-Rich Documents (Tanaka et al., 2025; 2504.09795) | CVPR 2025 | 96 | Unified visual retriever and generator, with OpenDocVQA as the benchmark | Core (top venue, direct competitor to VisRAG) |
| MMDocIR: Benchmarking Multi-Modal Retrieval for Long Documents (Dong et al., 2025; 2501.08828) | EMNLP 2025 | 60 | Page-level and layout-level retrieval in long documents. Compares visual retrievers with OCR-text retrievers | Core (peer-reviewed benchmark that directly compares image and text retrieval) |
| REAL-MM-RAG: A Real-World Multi-Modal Retrieval Benchmark (Wasserman et al., 2025; 2502.12342) | ACL 2025 | 54 | Realistic multi-modal retrieval queries, including rephrased queries and table-heavy financial documents | Skim (peer-reviewed; a robustness angle) |
| Benchmarking Retrieval-Augmented Multimodal Generation for Document QA, "MMDocRAG" (Dong et al., 2025; 2505.16470) | NeurIPS 2025 (S2) | 54 | End-to-end multimodal RAG for document question answering with interleaved image and text answers | Skim |
| jina-embeddings-v4: Universal Embeddings for Multimodal Multilingual Retrieval (Günther et al., 2025; 2506.18902) | MRL 2025 workshop (S2) | 114 | A single model with both a text mode and a visual mode, and multilingual. Allows a controlled text-vs-image comparison with the same backbone. ViDoRe V3 uses it this way | Core (it is the experimental instrument for RQ3: same model, both modalities) |
| GME: Improving Universal Multimodal Retrieval by Multimodal LLMs (Zhang et al., 2024; 2412.16855) | arXiv | 208 | A widely used visual-document embedder (GME-Qwen2) and baseline | Ref (highly cited model card, but a preprint) |
| MIRACL-VISION: A Large, Multilingual, Visual Document Retrieval Benchmark (Osmulski et al., NVIDIA, 2025; 2505.11651) | arXiv | 10 | 18 languages, 7,898 queries and 338,734 Wikipedia page images. Finds visual retrievers trail text retrievers by up to 59.7% multilingually and 12.1% in English — [arXiv](https://arxiv.org/abs/2505.11651) | Core despite low citations (the main counter-evidence on multilingual documents; see Q3) |
| VisR-Bench (Chen et al., 2025; 2508.07493) | arXiv, "Under Review" | 16 | Over 35K QA pairs, 1.2K documents, 16 languages. Multimodal LLM retrievers beat text and encoder retrievers but "still struggle with structured tables and low-resource languages" — [arXiv](https://arxiv.org/abs/2508.07493) | Skim (multilingual, but a preprint) |
| VisRAG 2.0 (Sun et al., 2025; 2510.09733) | No venue in S2 | 8 | Follow-up to VisRAG aimed at visual hallucination during generation over multiple images | Ref or drop (generation side, which is out of RQ3's retrieval scope; low impact) |
| ColQwen2 / ColNomic / ColEmbed | No standalone papers found (model cards only) | n/a | Strongest open late-interaction retrievers on ViDoRe. ViDoRe V3's best visual retriever is ColEmbed-3B-v2 (NDCG@10 59.8) — [ViDoRe V3](https://arxiv.org/html/2601.08620v2) | Cite as software or model cards, not as papers |

**B. Document VQA benchmarks (the ground on which the visual-retrieval claims stand)**
| Paper | Venue | Cites | Relevance | Priority |
|---|---|---|---|---|
| DocVQA (Mathew et al., 2021; 2007.00398) | WACV 2021 | 1,820 | Origin benchmark for question answering on document images | Ref (foundational, single page, not retrieval) |
| MP-DocVQA: Hierarchical Multimodal Transformers for Multi-Page DocVQA (Tito et al.; 2212.05935) | Pattern Recognition (journal) | 150 | Multi-page extension, a precursor to page retrieval | Ref |
| SlideVQA (Tanaka et al., 2023; 2301.04883) | AAAI 2023 | 251 | Multi-image slide question answering; reused in ViDoRe and VisRAG evaluations | Ref |
| MMLongBench-Doc (Ma et al., 2024; 2407.01523) | NeurIPS 2024 (Datasets and Benchmarks) | 245 | Long-document understanding with visual evidence; shows how hard long PDFs are | Skim (top venue, highly cited; useful framing for long humanitarian reports) |

**C. Document parsing / OCR toolkits and benchmarks (the class mmore's ingestion belongs to)**
| Paper | Venue | Cites | Relevance | Priority |
|---|---|---|---|---|
| OmniDocBench (Ouyang et al., 2025; 2412.07626) | CVPR 2025 | 192 | Peer-reviewed benchmark for PDF parsing across diverse document types, with per-element annotations. The natural way to score mmore's marker/Surya parsing against alternatives | Core |
| Nougat (Blecher et al., 2024; 2308.13418) | ICLR 2024 | 340 | End-to-end vision-transformer OCR from page to markup | Skim (top venue; academic PDFs only) |
| MinerU (Wang et al., 2024; 2409.18839) | arXiv | 358 | Open-source parsing pipeline, the most-cited parsing toolkit. From the same lab as OHRBench and OmniDocBench | Core (a direct comparator to mmore's ingestion) |
| MinerU2.5 (Niu et al., 2025; 2509.22186) | arXiv | 127 | Decoupled vision-language model parser at high resolution | Skim |
| Docling Technical Report (Auer et al., 2024; 2408.09869) and Docling: An Efficient Open-Source Toolkit (Livathinos et al., 2025; 2501.17887) | arXiv (S2) | 59 / 89 | IBM's open-source converter, the closest toolkit analogue to mmore | Skim. Cite the 2025 paper. Venue unverified: it may have an AAAI 2025 venue, which I did not confirm |
| olmOCR (Poznanski et al., 2025; 2502.18443) / olmOCR 2 (2510.19817) | arXiv | 138 / 52 | Open vision-language OCR for PDFs. olmOCR-Bench uses unit-test-style checks | Skim (strong open baseline to swap into mmore) |
| GOT-OCR2.0: General OCR Theory (Wei et al., 2024; 2409.01704) | arXiv | 229 | Unified end-to-end "OCR-2.0" model | Ref |
| DeepSeek-OCR: Contexts Optical Compression (Wei et al., 2025; 2510.18234) | arXiv | 204 | Vision tokens as compressed text; fast-rising citations | Ref |
| PaddleOCR-VL (Cui et al., 2025; 2510.14528) | arXiv | 98 | 0.9B parser explicitly aimed at **multilingual** document parsing | Skim (multilingual angle for French and non-Latin scripts) |
| Marker, Surya (mmore's current OCR) | No papers; GitHub software | n/a | Cite as software | n/a |

**D. OCR noise on non-English and low-quality documents**
| Paper | Venue | Cites | Relevance | Priority |
|---|---|---|---|---|
| Evaluating Robustness of LLMs in QA on Multilingual Noisy OCR Data, "MultiOCR-QA" (Piryani et al., 2025; 2502.16781) | CIKM 2025 — [arXiv](https://arxiv.org/abs/2502.16781) | 5 | 50K QA pairs over OCR'd historical documents in **English, French, German**. Noise types are character insertion, deletion and substitution. QA is "highly prone to OCR-induced errors" | Core for the French angle (peer-reviewed, though low citations) |
| Lost in OCR Translation? Vision-Based Approaches to Robust Document Retrieval (Most et al., 2025; 2505.05666) | ACM DocEng 2025 (S2) | 16 | ColPali compared against OCR pipelines (Llama 3.2 90B, Nougat) across document quality. Finds "OCR-based RAG is better able to generalize to unseen documents of varying quality" — [arXiv](https://arxiv.org/abs/2505.05666) | Core (direct test on degraded scans; counter-evidence) |
| Assessing the Impact of OCR Quality on Downstream NLP Tasks (van Strien et al., ICAART 2020) | ICAART 2020 — [SciTePress](https://www.scitepress.org/Papers/2020/91690/) | not queried | Classic study: OCR errors consistently degrade information retrieval, named entity recognition and other downstream tasks | Ref (pre-2023 origin of the "OCR quality matters" claim) |

### Inferences
- Section 6 currently treats "OCR vs image" as two fixed options. Adding OmniDocBench plus MinerU, Docling and olmOCR shows the text arm is itself a variable: parser choice changes text-pipeline quality, which is exactly OHRBench's point. A thesis benchmark should therefore cover at least two parsers, not just marker.
- jina-embeddings-v4 is the cleanest tool for a controlled RQ3 experiment, because it has text and visual modes in one multilingual model.

### Gaps
- I could not confirm peer-reviewed venues for M3DocRAG, ViDoRe V2, Docling (2501.17887) or GOT-OCR2.0. S2 lists them as arXiv, but some may have since been accepted. Check DBLP before finalising.
- I found no paper on OCR-then-RAG for humanitarian or global-health documents, or for African languages. The low-resource OCR benchmarks the search surfaced (OCR4MT, GlotOCR Bench) do not evaluate end-to-end RAG, and I did not verify them in detail.

## Q3. Is there evidence on multilingual or scanned low-quality documents specifically, and does the list's claim still hold?

### Takeaway
The claim needs qualifying. On born-digital, visually rich English and French corpora, visual retrievers do lead at first-stage retrieval. But in ViDoRe V3 itself, a **textual retriever plus a textual reranker gets the highest retrieval score**, and hybrid image+text context gives the best answers outside the oracle setting. On multilingual (MIRACL-VISION) and degraded/unseen scans (Most et al., DocEng 2025), text pipelines match or beat visual retrievers. "OCR quality is a bottleneck" stands (OHRBench, MultiOCR-QA), but "page-image retrieval is favoured" does not hold in general.

### Cited Findings
- ViDoRe V3 first-stage retrieval (NDCG@10): best visual ColEmbed-3B-v2 59.8. Jina-v4 visual 57.6 vs Jina-v4 textual 50.4. Best textual Qwen3-8B 51.0. BM25S 20.3 — [ViDoRe V3](https://arxiv.org/html/2601.08620v2)
- ViDoRe V3 with rerankers: Jina-v4 textual + zerank-2 reaches 63.6 (+13.2), while Jina-v4 visual + jina-reranker-m0 reaches 57.8 (+0.2). The textual pipeline has the highest overall retrieval score — [ViDoRe V3](https://arxiv.org/html/2601.08620v2)
- ViDoRe V3 generation (Gemini 3 Pro, non-oracle, all queries): hybrid (top-5 visual + top-5 textual) 65.7, text 64.9, image 64.5. Image context helps most on hard queries (oracle 64.7 image vs 62.3 text). 48.6% of queries are "easy" — [ViDoRe V3](https://arxiv.org/html/2601.08620v2)
- ViDoRe V3 text inputs came from NVIDIA's NeMo Retriever extraction (Markdown per page), so the textual arm used a modern parser, not Tesseract. Cross-lingual retrieval is 2–3 points below monolingual. The corpora include no scanned handwritten notes — [ViDoRe V3](https://arxiv.org/html/2601.08620v2)
- MIRACL-VISION (18 languages): visual embedding models show "up to 59.7% lower retrieval accuracy" than text retrievers multilingually, and 12.1% lower even in English — [arXiv 2505.11651](https://arxiv.org/abs/2505.11651)
- VisR-Bench (16 languages): multimodal-LLM retrievers lead overall but "still struggle with … low-resource languages" — [arXiv 2508.07493](https://arxiv.org/abs/2508.07493)
- Most et al. (DocEng 2025): ColPali does well on the documents it was fine-tuned on, but OCR-based RAG "generalizes better to unseen documents of varying quality" — [arXiv 2505.05666](https://arxiv.org/abs/2505.05666). A search-result summary of that paper notes ColPali's original OCR baseline relied on Tesseract, which the paper calls not state-of-the-art — [arXiv HTML](https://arxiv.org/html/2505.05666v1) (seen via search snippet; I did not read the full text)
- OHRBench: no OCR solution tested was "competent for constructing high-quality knowledge bases for RAG systems" — [arXiv 2412.02592](https://arxiv.org/abs/2412.02592). A secondary summary says the authors flag scanned historical, multilingual and handwritten documents as under-studied (not confirmed in the abstract)
- MultiOCR-QA (CIKM 2025; English, French, German historical OCR): LLM QA is "highly prone to OCR-induced errors" — [arXiv 2502.16781](https://arxiv.org/abs/2502.16781)
- Older evidence: OCR errors consistently degrade information retrieval and other downstream tasks, some far more than others — [van Strien et al., ICAART 2020](https://dare.uva.nl/id/fd295310-eb9e-4271-9dad-862bd3f4eb57)

### Inferences
- Suggested rewrite of the section intro: "Page-image retrievers lead first-stage retrieval on visually rich, born-digital corpora (ColPali, ViDoRe V3), but a strong parser plus textual reranking matches or beats them (ViDoRe V3), and on multilingual or degraded documents text pipelines often win (MIRACL-VISION; Most et al. 2025). OCR quality caps text-pipeline accuracy (OHRBench, MultiOCR-QA). Which modality wins is therefore corpus- and query-dependent, which motivates RQ3."
- Two confounders cut across the image-vs-text comparisons. First, ColPali's original text baselines used weak OCR. Second, MIRACL-VISION uses Wikipedia, which is born-digital, so its text side may be cleaner than real scans (see Gaps). No published study isolates *scanned, multilingual, humanitarian* PDFs, so the thesis would fill a real gap.
- The ViDoRe V3 finding that hybrid context gives the best non-oracle answers supports a text+image fusion arm in mmore experiments, rather than a binary choice.

### Suggested removals or demotions
- **UniversalRAG**: demote to Ref or move it to the routing section. It routes across modality-specific corpora (text, image, video) and is not about page-image vs OCR on documents. Routing is covered by other researchers. It also has only 27 citations.
- **DSE**: keep it, but as Skim. Its role (a single-vector screenshot baseline) is now also covered by jina-embeddings-v4 and GME.
- **VisRAG**: keep as Core. Its "20–40% end-to-end gains" figure should be checked against the paper, and the comparison baseline should be named.

### Gaps
- I could not confirm whether MIRACL-VISION's text retrievers received the original Wikipedia text or OCR output. The abstract does not say. If it was original text, the 59.7% gap is an upper bound for OCR'd text.
- I did not extract Most et al.'s numeric results; the abstract gives only qualitative claims.
- I did not get per-language (French-only) ViDoRe V3 results: the appendix was truncated in the fetch.
- I found no study of visual vs OCR retrieval on low-resource African or Arabic-script humanitarian documents.
