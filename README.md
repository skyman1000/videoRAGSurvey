<h1 align="center">Awesome Video RAG</h1>

<p align="center">
  <strong>A curated companion to <em>A Comprehensive Survey on Video Retrieval-Augmented Generation</em></strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Survey-Video%20RAG-3568A8" alt="Video RAG Survey" />
  <img src="https://img.shields.io/badge/Papers-2018--2026-6A5ACD" alt="Papers from 2018 to 2026" />
  <img src="https://img.shields.io/badge/Last%20Update-September%202026-2E8B57" alt="Last update September 2026" />
  <img src="https://img.shields.io/badge/PRs-welcome-brightgreen" alt="PRs welcome" />
</p>

This repository accompanies our survey paper:

> **A Comprehensive Survey on Video Retrieval-Augmented Generation**  
> Working manuscript. The public paper link and final bibliographic metadata will be added after release.

Video Retrieval-Augmented Generation (Video RAG) turns long videos or video corpora into searchable multimodal knowledge, retrieves query-relevant evidence, and conditions a language or multimodal generator on that evidence. The retrieved evidence can include frames, clips, scenes, entire videos, ASR transcripts, OCR text, audio events, timestamps, entities, relations, and structured memories.

Our survey provides a unified system view of Video RAG across three core modules:

- **Video knowledge indexing and representation**: corpus construction, multimodal extraction, temporal alignment, knowledge-base construction, and hybrid indexing.
- **Retrieval strategies and mechanisms**: query processing, multi-granular retrieval, single-step/multi-step/adaptive retrieval, and post-retrieval filtering.
- **Retrieval augmentation and generation**: multimodal evidence fusion, generator integration, training strategies, and grounded response generation.

> [!NOTE]
> Paper and code links below were checked against primary paper pages, official proceedings, project pages, or author-identified repositories. A dash (—) means that we did not verify an official public implementation. Dates and venues follow the latest public records available in August 2026.

---

<a id="quick-index"></a>

## Quick Index

- [What Counts as Video RAG?](#what-counts-as-video-rag)
- [Evolution](#evolution)
- [Taxonomy](#taxonomy)
  - [Index and Retrieval Granularity](#index-and-retrieval-granularity)
  - [Knowledge Structure](#knowledge-structure)
  - [Retrieval Mechanism](#retrieval-mechanism)
  - [Multimodal Fusion](#multimodal-fusion)
- [Workflow](#workflow)
- [Paper Collection](#paper-collection)
- [Benchmarks and Datasets](#benchmarks-and-datasets)
- [Evaluation](#evaluation)
- [Applications](#applications)
- [Challenges and Future Directions](#challenges-and-future-directions)
- [Contributing and Citation](#contributing-and-citation)

---

## What Counts as Video RAG?

We use a system-level definition:

1. Video content is transformed into a **retrievable knowledge base** $\mathcal{K}$.
2. A retriever $\mathcal{R}$ selects query-relevant evidence $\mathcal{E}$ from $\mathcal{K}$.
3. A generator $\mathcal{G}$ produces a textual answer grounded in the query and retrieved evidence:

$$
r = \mathcal{G}\bigl(q,\mathcal{R}(q,\mathcal{K})\bigr).
$$

<p align="center">
  <img src="imgC/videorag-framework.png" width="96%" alt="Overall framework of Video RAG" />
</p>

This criterion separates Video RAG from adjacent tasks:

| Task | Retrieval output | Final output | Video RAG? |
|---|---|---|---|
| Short/long Video QA | Often implicit frame or context selection | Text answer | Only when evidence is explicitly indexed, retrieved, and used for generation |
| Video captioning | Usually no query-conditioned retrieval | Caption/summary | Only retrieval-augmented variants |
| Video moment retrieval | Temporal interval(s) | Retrieved timestamps | No; retrieval is the final output |
| Text-to-video retrieval | Ranked videos | Ranked list | No; retrieval is the final output |
| **Video RAG** | Frames, clips, scenes, videos, text, audio, or structured evidence | **Evidence-conditioned text answer/explanation** | **Yes** |

<p align="center">
  <img src="imgC/task-comparison.png" width="94%" alt="Comparison of Video RAG and related video understanding tasks" />
</p>

<p align="right"><a href="#quick-index">↑ Back to Index</a></p>

---

## Evolution

Video RAG has evolved from retrieving external background knowledge for captioning, through internal video memories and query-aware frame/segment selection, toward hybrid knowledge bases, graph reasoning, adaptive routing, multi-agent retrieval, and streaming systems.

<p align="center">
  <img src="imgC/Evolution.png" width="98%" alt="Evolution of Video RAG from 2018 to 2026" />
</p>

The timeline uses three non-exclusive labels:

- 🔎 **Optimized retrieval strategy**: query guidance, granularity design, multi-hop search, tool use, reranking, or adaptive routing.
- 🟢 **Internal knowledge base**: indices, memories, stores, or graphs built from the current video or task process.
- 🟤 **External knowledge base**: video/text corpora, web content, similar examples, demonstrations, documents, or external knowledge graphs.

<p align="right"><a href="#quick-index">↑ Back to Index</a></p>

---

## Taxonomy

We organize Video RAG around five core design dimensions: evidence granularity, knowledge structure, retrieval strategy, multimodal alignment, and generator model. These choices jointly determine the trade-offs among evidence coverage, context budget, retrieval accuracy, latency, and answer quality.

<p align="center">
  <img src="imgC/videorag_design_framework.png" width="98%" alt="Core design dimensions of Video RAG" />
</p>

### Index and Retrieval Granularity

| Granularity | Retrieved evidence | Strength | Typical limitation | Representative systems |
|---|---|---|---|---|
| **Frame** | Frames or frame-aligned ASR/OCR/object evidence | Precise visual and temporal localization | Limited event context | [Video-RAG](https://arxiv.org/abs/2411.13093), [DrVideo](https://arxiv.org/abs/2406.12846), [FRAG](https://arxiv.org/abs/2504.17447), [E-VRAG](https://arxiv.org/abs/2508.01546), [RAVU](https://arxiv.org/abs/2505.03173) |
| **Segment / clip** | Bounded temporal windows with aligned multimodal evidence | Preserves local motion, dialogue, and event context | Sensitive to segmentation boundaries | [Goldfish](https://arxiv.org/abs/2407.12679), [SALOVA](https://arxiv.org/abs/2411.16173), [Vgent](https://arxiv.org/abs/2510.14032), [VideoStir](https://arxiv.org/abs/2604.05418) |
| **Scene** | Semantically or narratively coherent scenes | Cross-shot continuity and event-level context | Scene-boundary and graph-construction errors | [SceneRAG](https://arxiv.org/abs/2506.07600) |
| **Video** | Complete videos or global video representations | Corpus-scale selection and global context | Coarse localization and high downstream context cost | [VideoRAG over Video Corpus](https://aclanthology.org/2025.findings-acl.1096/), [VideoRAG with Extreme Long-Context Videos](https://arxiv.org/abs/2502.01549) |
| **Hierarchical / multi-granular** | Evidence across several temporal or semantic levels | Balances global context and fine-grained access | More complex indexing and routing | [VideoTree](https://arxiv.org/abs/2405.19209), [AdaVideoRAG](https://proceedings.neurips.cc/paper_files/paper/2025/file/092359ce5cf60a80e882378944bf1be4-Paper-Conference.pdf), [WorldMM](https://arxiv.org/abs/2512.02425), [CARVE](https://arxiv.org/abs/2606.13141) |

<p align="center">
  <img src="imgC/multi_granular_retrieval.png" width="96%" alt="Frame, segment, scene, and video-level retrieval" />
</p>

### Knowledge Structure

| Structure | Evidence organization | Best suited to | Representative systems |
|---|---|---|---|
| **Vector** | Frame/clip embeddings and textual proxies retrieved by similarity search or reranking | Fast semantic matching and scalable local evidence access | [iRAG](https://arxiv.org/abs/2404.12309), [Video-RAG](https://arxiv.org/abs/2411.13093), [DrVideo](https://arxiv.org/abs/2406.12846), [SALOVA](https://arxiv.org/abs/2411.16173), [TV-RAG](https://doi.org/10.1145/3746027.3755873) |
| **Graph** | Entities, events, scenes, or clips connected by temporal, semantic, spatial, causal, or co-occurrence edges | Multi-hop, relational, and event-chain reasoning | [Vgent](https://arxiv.org/abs/2510.14032), [ViG-RAG](https://doi.org/10.1609/aaai.v40i1.36963), [RAVU](https://arxiv.org/abs/2505.03173), [VideoStir](https://arxiv.org/abs/2604.05418) |
| **Hybrid / memory** | Two or more of vectors, graphs, hierarchical event structures, and persistent memories | Long-horizon reasoning over heterogeneous evidence | [AdaVideoRAG](https://proceedings.neurips.cc/paper_files/paper/2025/file/092359ce5cf60a80e882378944bf1be4-Paper-Conference.pdf), [MemVid](https://arxiv.org/abs/2503.09149), [VideoRAG with Extreme Long-Context Videos](https://arxiv.org/abs/2502.01549), [WorldMM](https://arxiv.org/abs/2512.02425), [StreamRAG](https://openaccess.thecvf.com/content/CVPR2026/html/Xie_StreamRAG_Enhancing_Real-Time_Video_Understanding_with_Retrieval_Augmentation_CVPR_2026_paper.html) |

### Retrieval Mechanism

| Mechanism | Definition | Typical use | Representative systems |
|---|---|---|---|
| **Single-step** | A fixed, non-iterative retrieval-and-ranking pass | Low-latency factual or local-evidence queries | [Goldfish](https://arxiv.org/abs/2407.12679), [Video-RAG](https://arxiv.org/abs/2411.13093), [FRAG](https://arxiv.org/abs/2504.17447), [TV-RAG](https://doi.org/10.1145/3746027.3755873) |
| **Multi-step** | Evidence is refined, expanded, or composed over multiple retrieval stages | Long-range, multi-event, or compositional reasoning | [DrVideo](https://arxiv.org/abs/2406.12846), [MemVid](https://arxiv.org/abs/2503.09149), [Vgent](https://arxiv.org/abs/2510.14032), [RAVU](https://arxiv.org/abs/2505.03173), [VideoStir](https://arxiv.org/abs/2604.05418) |
| **Adaptive** | The system selects the retrieval scheme, modality, granularity, search depth, or stopping condition per query/evidence state | Accuracy-efficiency control across diverse queries | [VideoTree](https://arxiv.org/abs/2405.19209), [AdaVideoRAG](https://proceedings.neurips.cc/paper_files/paper/2025/file/092359ce5cf60a80e882378944bf1be4-Paper-Conference.pdf), [WorldMM](https://arxiv.org/abs/2512.02425), [CARVE](https://arxiv.org/abs/2606.13141), [StreamRAG](https://openaccess.thecvf.com/content/CVPR2026/html/Xie_StreamRAG_Enhancing_Real-Time_Video_Understanding_with_Retrieval_Augmentation_CVPR_2026_paper.html) |

<p align="center">
  <img src="imgC/retrieval_strategies.png" width="98%" alt="Single-step, multi-step, and adaptive retrieval" />
</p>

### Multimodal Fusion

| Fusion stage | Common operations | Representative systems |
|---|---|---|
| **Early fusion** | Textual evidence injection, prompt augmentation, cross-modal token concatenation, unified textual representations | [R2A](https://arxiv.org/abs/2306.11732), [ChatVideo](https://arxiv.org/abs/2304.14407), [Goldfish](https://arxiv.org/abs/2407.12679), [Video-RAG](https://arxiv.org/abs/2411.13093), [VideoRAG over Video Corpus](https://aclanthology.org/2025.findings-acl.1096/) |
| **Mid fusion** | Cross-modal attention, query-conditioned selection, learned retrieval, memory augmentation, spatio-temporal alignment | [MA-LMM](https://arxiv.org/abs/2404.05726), [SALOVA](https://arxiv.org/abs/2411.16173), [DrVideo](https://arxiv.org/abs/2406.12846), [MemVid](https://arxiv.org/abs/2503.09149), [VideoStir](https://arxiv.org/abs/2604.05418) |
| **Late fusion** | Multi-source consolidation, cascaded filtering, reranking/verification, agentic reasoning, graph reasoning | [iRAG](https://arxiv.org/abs/2404.12309), [VideoAgent](https://arxiv.org/abs/2403.11481), [Vgent](https://arxiv.org/abs/2510.14032), [ViG-RAG](https://doi.org/10.1609/aaai.v40i1.36963), [RAVU](https://arxiv.org/abs/2505.03173), [WorldMM](https://arxiv.org/abs/2512.02425) |

<p align="center">
  <img src="imgC/multimodal-content-extraction-alignment.png" width="96%" alt="Multimodal content extraction and temporal alignment in Video RAG" />
</p>

<p align="right"><a href="#quick-index">↑ Back to Index</a></p>

---

## Workflow

A practical Video RAG pipeline contains ten principal modules grouped into three stages.

| Stage | Modules | Key design questions |
|---|---|---|
| **1. Video knowledge indexing and representation** | Corpus construction; multimodal content extraction; knowledge-base construction; hybrid indexing | What should be extracted? At what temporal granularity? How are visual, audio, ASR, OCR, metadata, entities, and timestamps aligned and stored? |
| **2. Retrieval strategies and mechanisms** | Query processing; retrieval granularity; retrieval mechanism; post-processing | Should the system retrieve frames, clips, scenes, videos, text, or graph paths? Is one pass sufficient? When should it rerank, expand, or stop? |
| **3. Retrieval augmentation and generation** | Multimodal fusion; generator integration | How should evidence be formatted, fused, verified, and passed to an LLM, VLLM, or MLLM? Which components are trained or frozen? |

### Indexing and Representation

- **Corpus construction**: source collection, preprocessing, temporal synchronization, corpus organization, and incremental updates.
- **Multimodal extraction**: uniform/keyframe/adaptive sampling; visual encoding and captioning; ASR, diarization, audio events; OCR, subtitles, and metadata.
- **Knowledge-base construction**: temporal units, hierarchical organizations, knowledge graphs, and hybrid memory structures.
- **Hybrid indexing**: multimodal embeddings, vector databases, lexical indices, graph stores, and combined architectures.

### Retrieval

- **Query processing**: analysis, rewriting, decomposition, modality-aware expansion, and evidence-need prediction.
- **Multi-granular retrieval**: frame, segment, scene, video, graph, and hierarchical evidence access.
- **Mechanism selection**: single-step, multi-step, or adaptive/agentic retrieval.
- **Post-processing**: relevance filtering, reranking, deduplication, temporal expansion, and multi-channel result fusion.

### Augmentation and Generation

- **Evidence fusion**: early, mid, and late fusion over visual, audio, textual, temporal, and structured signals.
- **Generator families**: text-only LLMs over textualized video evidence; VLLMs over retrieved visual evidence; MLLMs over heterogeneous evidence.
- **Integration paradigms**: training-enhanced (SFT/PEFT, contrastive learning, preference optimization, RL) or training-free; agentic integration can be combined with either.
- **Quality control**: evidence conditioning, iterative reasoning, verification, grounded attribution, and hallucination reduction.

<p align="right"><a href="#quick-index">↑ Back to Index</a></p>

---

## Paper Collection

### 2018–2023: Foundations

| Venue | Paper | Main Video-RAG contribution | Code |
|---|---|---|---|
| EMNLP 2018 | [Incorporating Background Knowledge into Video Description Generation](https://aclanthology.org/D18-1433/) | Retrieves external news documents and injects entities/events into video descriptions | — |
| ACM TOMM 2023 | [Retrieval Augmented Convolutional Encoder-Decoder Networks for Video Captioning](https://doi.org/10.1145/3539225) | Retrieval-augmented memory for video caption generation | — |
| ICCV Workshops 2023 | [Retrieving-to-Answer: Zero-Shot Video Question Answering with Frozen Large Language Models](https://arxiv.org/abs/2306.11732) | Retrieves semantically similar text from a generic corpus for zero-shot Video QA | — |
| arXiv 2023 | [ChatVideo: A Tracklet-Centric Multimodal and Versatile Video Understanding System](https://arxiv.org/abs/2304.14407) | Tracklet-centric multimodal database and tool-mediated video interaction | [Code](https://github.com/yiwengxie/Chat-Video) |
| NeurIPS 2023 | [Self-Chained Image-Language Model for Video Localization and Question Answering (SeViLA)](https://arxiv.org/abs/2305.06988) | Query-aware keyframe localization chained with answer generation | [Code](https://github.com/Yui010206/SeViLA) |

### 2024: Memories, Agents, and Efficient Video Access

| Venue | Paper | Main Video-RAG contribution | Code |
|---|---|---|---|
| CVPR Workshops 2024 | [ViTA: An Efficient Video-to-Text Algorithm Using VLM for RAG-Based Video Analysis](https://openaccess.thecvf.com/content/CVPR2024W/MAR/html/Arefeen_ViTA_An_Efficient_Video-to-Text_Algorithm__using_VLM_for_RAG-based_CVPRW_2024_paper.html) | Accelerates video-to-text knowledge construction with lightweight/heavyweight VLM routing | — |
| ECCV 2024 | [VideoAgent: A Memory-Augmented Multimodal Agent for Video Understanding](https://arxiv.org/abs/2403.11481) | Structured temporal/object memory with LLM-directed retrieval tools | [Code](https://github.com/YueFan1014/VideoAgent) |
| CIKM 2024 | [iRAG: Advancing RAG for Videos with an Incremental Approach](https://arxiv.org/abs/2404.12309) | Defers expensive fine-grained processing until query time for faster ingestion | — |
| CVPR 2024 | [MA-LMM: Memory-Augmented Large Multimodal Model for Long-Term Video Understanding](https://arxiv.org/abs/2404.05726) | Online visual memory bank for long-term video reasoning | [Code](https://github.com/boheumd/MA-LMM) |
| CVPR 2024 | [Retrieval-Augmented Egocentric Video Captioning (EgoInstructor)](https://arxiv.org/abs/2401.00789) | Retrieves exocentric instructional videos to improve egocentric captioning | [Code](https://github.com/Jazzcharles/Egoinstructor) |
| ECCV 2024 | [Goldfish: Vision-Language Understanding of Arbitrarily Long Videos](https://arxiv.org/abs/2407.12679) | Retrieves query-relevant clips through textual descriptions for arbitrarily long videos | [Code](https://github.com/Vision-CAIR/MiniGPT4-video) |

### 2025: Dedicated Video RAG Systems

| Venue | Paper | Main Video-RAG contribution | Code |
|---|---|---|---|
| NeurIPS 2025 | [Video-RAG: Visually-Aligned Retrieval-Augmented Long Video Comprehension](https://arxiv.org/abs/2411.13093) | Training-free retrieval of timestamp-aligned ASR, OCR, and object evidence | [Code](https://github.com/Leon1207/Video-RAG-master) |
| Findings of ACL 2025 | [VideoRAG: Retrieval-Augmented Generation over Video Corpus](https://aclanthology.org/2025.findings-acl.1096/) | Coarse-to-fine corpus-level video retrieval with adaptive frame/text selection | [Code](https://github.com/starsuzi/VideoRAG) |
| CVPR 2025 | [DrVideo: Document Retrieval Based Long Video Understanding](https://arxiv.org/abs/2406.12846) | Iteratively augments a textual video document with retrieved keyframes | [Code](https://github.com/Upper9527/DrVideo) |
| CVPR 2025 | [SALOVA: Segment-Augmented Long Video Assistant for Targeted Retrieval and Routing](https://arxiv.org/abs/2411.16173) | Segment Retrieval Router with spatio-temporal segment representations | [Code](https://github.com/IVY-LVLM/SALOVA) |
| CVPR 2025 | [VideoTree: Adaptive Tree-Based Video Representation for LLM Reasoning on Long Videos](https://arxiv.org/abs/2405.19209) | Query-adaptive, coarse-to-fine hierarchical video representation | [Code](https://github.com/Ziyang412/VideoTree) |
| NeurIPS 2025 | [AdaVideoRAG: Omni-Contextual Adaptive Retrieval-Augmented Efficient Long Video Understanding](https://proceedings.neurips.cc/paper_files/paper/2025/file/092359ce5cf60a80e882378944bf1be4-Paper-Conference.pdf) | Routes queries among direct inference, multimodal retrieval, and graph retrieval | [Repository; full release pending](https://github.com/xzc-zju/AdaVideoRAG) |
| NeurIPS 2025 Spotlight | [Vgent: Graph-Based Retrieval-Reasoning-Augmented Generation for Long Video Understanding](https://arxiv.org/abs/2510.14032) | Clip graph retrieval plus structured evidence verification and aggregation | [Code](https://github.com/xiaoqian-shen/Vgent) |
| ACM MM 2025 | [TV-RAG: A Temporal-Aware and Semantic Entropy-Weighted Framework for Long Video Retrieval and Understanding](https://doi.org/10.1145/3746027.3755873) | Temporal decay retrieval and entropy-weighted keyframe selection | [Code](https://github.com/AI-Researcher-Team/TV-RAG) |
| arXiv 2025 | [Memory-Enhanced Retrieval Augmentation for Long Video Understanding (MemVid)](https://arxiv.org/abs/2503.09149) | Memory-guided retrieval clues with SFT and preference optimization | — |
| arXiv 2025 | [FRAG: Frame Selection Augmented Generation for Long Video and Long Document Understanding](https://arxiv.org/abs/2504.17447) | Query-aware top-frame selection with a frozen VLM | [Code](https://github.com/NVlabs/FRAG) |
| arXiv 2025 | [E-VRAG: Enhancing Long Video Understanding with Resource-Efficient Retrieval Augmented Generation](https://arxiv.org/abs/2508.01546) | Hierarchical pre-filtering, lightweight scoring, and multi-view answering | — |

### 2026: Graph, Memory, Streaming, and Chunk-Adaptive RAG

| Venue | Paper | Main Video-RAG contribution | Code |
|---|---|---|---|
| KDD 2026 | [VideoRAG: Retrieval-Augmented Generation with Extreme Long-Context Videos](https://arxiv.org/abs/2502.01549) | Graph-driven textual grounding plus hierarchical multimodal context encoding across videos | [Code](https://github.com/HKUDS/VideoRAG) |
| ICASSP 2026 | [SceneRAG: Scene-Level Retrieval-Augmented Generation for Video Understanding](https://arxiv.org/abs/2506.07600) | Narratively coherent scene units and scene-level multimodal graphs | — |
| AAAI 2026 | [ViG-RAG: Video-Aware Graph Retrieval-Augmented Generation via Temporal and Semantic Hybrid Reasoning](https://doi.org/10.1609/aaai.v40i1.36963) | Probabilistic temporal knowledge graph with semantic-temporal hybrid retrieval | — |
| WACV 2026 | [RAVU: Retrieval Augmented Video Understanding with Compositional Reasoning over Graph](https://openaccess.thecvf.com/content/WACV2026/html/Malik_RAVU_Retrieval_Augmented_Video_Understanding_with_Compositional_Reasoning_over_Graph_WACV_2026_paper.html) | Spatio-temporal entity graph and compositional query decomposition | — |
| CVPR 2026 | [StreamRAG: Enhancing Real-Time Video Understanding with Retrieval Augmentation](https://openaccess.thecvf.com/content/CVPR2026/html/Xie_StreamRAG_Enhancing_Real-Time_Video_Understanding_with_Retrieval_Augmentation_CVPR_2026_paper.html) | Online event segmentation, token reuse, and a query-aware dynamic retrieval gate | — |
| CVPR 2026 Highlight | [WorldMM: Dynamic Multimodal Memory Agent for Long Video Reasoning](https://arxiv.org/abs/2512.02425) | Adaptive retrieval over episodic, semantic, and visual memories at multiple scales | [Code](https://github.com/wgcyeo/WorldMM) |
| ACL 2026 | [VideoStir: Understanding Long Videos via Spatio-Temporally Structured and Intent-Aware RAG](https://aclanthology.org/2026.acl-long.1656/) | Clip-level spatio-temporal graph, multi-hop retrieval, and intent-aware frame scoring | [Code](https://github.com/RomGai/VideoStir) |
| arXiv 2026 | [Rethinking RAG in Long Videos: What to Retrieve and How to Use It? (V-RAGBench & CARVE)](https://arxiv.org/abs/2606.13141) | Decoupled Video-RAG evaluation and chunk-adaptive modality/granularity reranking | — |

<p align="right"><a href="#quick-index">↑ Back to Index</a></p>

---

## Benchmarks and Datasets

Video RAG evaluation spans answer generation, retrieval, temporal localization, long-context understanding, and agentic multi-hop evidence acquisition.

| Benchmark | Venue / year | Primary target | Paper | Data / code |
|---|---:|---|---|---|
| **NExT-QA** | CVPR 2021 | Causal and temporal Video QA | [Paper](https://openaccess.thecvf.com/content/CVPR2021/html/Xiao_NExT-QA_Next_Phase_of_Question-Answering_to_Explaining_Temporal_Actions_CVPR_2021_paper.html) | [Repository](https://github.com/doc-doc/NExT-QA) |
| **QVHighlights** | NeurIPS 2021 Datasets & Benchmarks | Query-based moment retrieval and highlight detection | [Paper](https://arxiv.org/abs/2107.09609) | [Repository](https://github.com/jayleicn/moment_detr) |
| **EgoSchema** | NeurIPS 2023 Datasets & Benchmarks | Very long-form egocentric Video QA | [Paper](https://arxiv.org/abs/2308.09126) | [Repository](https://github.com/egoschema/EgoSchema) |
| **TVQA-Long** | ECCV 2024 | Episode-length video understanding | [Goldfish paper](https://arxiv.org/abs/2407.12679) | [Repository](https://github.com/Vision-CAIR/MiniGPT4-video) |
| **LongVideoBench** | NeurIPS 2024 Datasets & Benchmarks | Interleaved video-language understanding up to one hour | [Paper](https://arxiv.org/abs/2407.15754) | [Repository](https://github.com/longvideobench/LongVideoBench) |
| **Video-MME** | CVPR 2025 | Multi-domain, multi-duration, multimodal video understanding | [Paper](https://arxiv.org/abs/2405.21075) | [Repository](https://github.com/MME-Benchmarks/Video-MME) |
| **MLVU** | CVPR 2025 | Multi-task long-video understanding | [Paper](https://openaccess.thecvf.com/content/CVPR2025/html/Zhou_MLVU_Benchmarking_Multi-task_Long_Video_Understanding_CVPR_2025_paper.html) | [Repository](https://github.com/JUNJIE99/MLVU) |
| **LVBench** | ICCV 2025 | Extreme long-video comprehension and information extraction | [Paper](https://arxiv.org/abs/2406.08035) | [Repository](https://github.com/zai-org/LVBench) |
| **HiVU** | NeurIPS 2025 | Hierarchical and adaptive Video RAG | [AdaVideoRAG paper](https://proceedings.neurips.cc/paper_files/paper/2025/file/092359ce5cf60a80e882378944bf1be4-Paper-Conference.pdf) | [Repository; release pending](https://github.com/xzc-zju/AdaVideoRAG) |
| **LongerVideos** | KDD 2026 | Extreme long-context and cross-video reasoning | [VideoRAG paper](https://arxiv.org/abs/2502.01549) | [Repository](https://github.com/HKUDS/VideoRAG) |
| **V-RAGBench** | 2026 | Decoupled retrieval and generation with uniquely sufficient evidence chunks | [CARVE paper](https://arxiv.org/abs/2606.13141) | — |
| **LongVidSearch** | 2026 | Agentic 2–4 hop evidence retrieval planning under a standardized interface | [Paper](https://arxiv.org/abs/2603.14468) | [Dataset](https://huggingface.co/datasets/Fishiing/LongVidSearch) |

### Task Coverage

| Evaluation task | Representative benchmarks |
|---|---|
| Video-to-text generation | MSVD, MSR-VTT, VATEX, ActivityNet Captions, YouCook2, BDD-X, WikiVideo |
| Video question answering | MSVD-QA, MSRVTT-QA, TGIF-QA, ActivityNet-QA, TVQA, How2QA, NExT-QA, STAR, IntentQA, MVBench |
| Text-video retrieval | MSR-VTT, MSVD, LSMDC, DiDeMo, YouCook2, EK100 MIR, Charades-Ego, EgoLearner |
| Temporal grounding and localization | Charades-STA, QVHighlights, DiDeMo, ActivityNet Captions, Ego4D, TempCompass |
| Long-video understanding | MovieQA, MovieChat-1K, TVQA-Long, EgoSchema, LongVideoBench, MLVU, LVBench, Video-MME |
| Agentic retrieval and multi-hop reasoning | LongVidSearch, V-RAGBench |

<p align="right"><a href="#quick-index">↑ Back to Index</a></p>

---

## Evaluation

Answer accuracy alone cannot diagnose a Video RAG pipeline. We recommend reporting retrieval, grounding, generation, and system-level metrics together.

| Dimension | Metrics / protocols |
|---|---|
| **Retrieval** | Recall@K, Precision@K, F1@K, mAP, nDCG, MRR, Median Rank, Mean Rank |
| **Localization and grounding** | tIoU, mIoU, IoU-thresholded Recall@K, frame/segment Precision, Recall, and F1 |
| **Generation** | Accuracy, Exact Match, F1, ANLS, BLEU, METEOR, ROUGE, CIDEr, BERTScore, LLM-as-a-judge, win rate, human evaluation |
| **Long-video robustness** | Length-stratified accuracy and performance degradation as duration increases |
| **Cross-video / long-range reasoning** | Multi-video retrieval, temporal grounding, multi-hop evidence coverage, citation/evidence F1 |
| **Efficiency** | Offline indexing cost, retrieval latency, end-to-end wall-clock time, memory, TFLOPs, processed frames, and token count |
| **Groundedness and factuality** | Evidence attribution, answer support, hallucination rate, factual consistency, and trustworthiness |

For reproducible comparisons, please report the video sampling budget, retrieved evidence count, modality set, temporal granularity, generator, prompt/template, and whether subtitles or external knowledge are used.

<p align="right"><a href="#quick-index">↑ Back to Index</a></p>

---

## Applications

- **Education and knowledge learning**: lecture-series QA, curriculum search, and personalized review.
- **Film and entertainment**: plot understanding, episode-level search, character/event tracking, and segment navigation.
- **Surveillance and public safety**: long-duration event retrieval, anomaly localization, cross-camera association, and forensic review.
- **Embodied AI and robotics**: historical experience retrieval, task planning, navigation, and first-person interaction understanding.
- **Healthcare**: surgical-video retrieval, patient monitoring, rehabilitation analysis, and case-grounded assistance.
- **Content creation and media production**: source localization, automatic narration, news generation, and retrieval-guided editing.
- **Enterprise knowledge management**: searchable training videos, procedural QA, and real-time assistance over operational streams.
- **Scientific research**: evidence retrieval from experiments, demonstrations, and long-form multimodal records.

---

## Challenges and Future Directions

1. **Hierarchical and long-term memory** — preserve global context, visual detail, entity identity, temporal dependencies, and causal relations while supporting updates and selective forgetting.
2. **Fine-grained retrieval** — retrieve sparse, temporally dispersed, and modality-specific evidence without losing event continuity.
3. **Adaptive and agentic retrieval** — select modalities, time spans, granularity, tools, and stopping conditions according to query difficulty and evidence sufficiency.
4. **Efficient and deployable systems** — reduce indexing, storage, retrieval, and generation costs for real-time, streaming, on-device, and continually updated video sources.
5. **Unified end-to-end evaluation** — jointly measure retrieval recall, grounding, reasoning, answer quality, factuality, latency, and memory cost.
6. **Cross-module coordination** — co-design retrievers, encoders, alignment modules, memories, and generators to reduce representation mismatch and error propagation.

<p align="right"><a href="#quick-index">↑ Back to Index</a></p>

---

## Contributing and Citation

This survey and reading list are a work in progress. Pull requests and issues are welcome for:

- newly published Video RAG papers;
- corrected venue, paper, project, dataset, or code links;
- new benchmarks and evaluation protocols;
- taxonomy corrections or missing application areas.

For a paper submission, please include:

```text
Title:
Authors:
Venue and year:
Paper URL:
Official code/project URL (if available):
Taxonomy tags (granularity / knowledge structure / retrieval / fusion):
One-sentence contribution:
```

The survey's final BibTeX entry will be updated when the manuscript is publicly released. Until then, a minimal provisional record is:

```bibtex
@misc{videorag_survey_2026,
  title = {A Comprehensive Survey on Video Retrieval-Augmented Generation},
  year  = {2026},
  note  = {Manuscript}
}
```

### Acknowledgement

The organization of this repository was inspired by [Awesome MM-RAG](https://github.com/INTREBID/Awesome-MM-RAG). All taxonomy descriptions, figures, paper selections, and Video-RAG-specific scope follow our survey manuscript.

<p align="right"><a href="#quick-index">↑ Back to Index</a></p>
