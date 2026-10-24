<h1 align="center">A Comprehensive Survey on Video Retrieval-Augmented Generation: A System-Oriented Perspective</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Survey-Video%20RAG-3568A8" alt="Video RAG Survey" />
  <img src="https://img.shields.io/badge/Papers-2018--2026-6A5ACD" alt="Papers 2018–2026" />
  <img src="https://img.shields.io/badge/Updated-October%202026-2E8B57" alt="Updated October 2026" />
  <img src="https://img.shields.io/badge/PRs-welcome-brightgreen" alt="Pull requests welcome" />
</p>


This repository accompanies **A Comprehensive Survey on Video Retrieval-Augmented Generation: A System-Oriented Perspective**. It presents a system-oriented overview of **video knowledge organization, evidence acquisition, and retrieval-augmented generation**, along with a curated collection of research papers and evaluation resources.

Video RAG makes large-scale and long-form video content searchable and supports responses grounded in retrieved **visual, textual, acoustic, temporal, and structured evidence**.

<a id="quick-index"></a>

## Quick Index

- [Workflow](#workflow)
- [What Counts as Video RAG?](#what-counts-as-video-rag)
- [Evolution](#evolution)
- [Taxonomy and Key Design Choices](#taxonomy-and-key-design-choices)
- [Video Knowledge Indexing and Representation](#video-knowledge-indexing-and-representation)
- [Retrieval Strategies and Mechanisms](#retrieval-strategies-and-mechanisms)
  - [Retrieval Granularity](#retrieval-granularity)
  - [Retrieval Mechanism](#retrieval-mechanism)
- [Retrieval Augmentation and Generation](#retrieval-augmentation-and-generation)
  - [Multimodal Fusion](#multimodal-fusion)
  - [Integration Paradigms of Video RAG Systems](#integration-paradigms-of-video-rag-systems)
- [Benchmarks and Datasets](#benchmarks-and-datasets)
- [Evaluation](#evaluation)
- [Paper Collection](#paper-collection)
- [Applications](#applications)
- [Challenges and Future Directions](#challenges-and-future-directions)
- [Contributing and Citation](#contributing-and-citation)

---

## Workflow

The survey organizes Video RAG systems into three connected stages:

1. **Video knowledge indexing and representation** — prepare video corpora, extract and align multimodal content, construct retrievable knowledge bases, and build indices.
2. **Retrieval strategies and mechanisms** — interpret queries, select the retrieval target and temporal granularity, retrieve relevant evidence, and filter or rerank the results.
3. **Retrieval augmentation and generation** — fuse evidence across modalities and generate responses supported by retrieved video content.

The workflow below maps these stages to their constituent modules and associated methods.

<p align="center">
  <a href="imgC/Fig10_Workflow.png"><img src="imgC/Fig10_Workflow.png" alt="Fig. 10. Original Video RAG workflow from the survey, including paper citation numbers" /></a>
</p>


---

## What Counts as Video RAG?

In this survey, a system is counted as Video RAG when:

1. Video content is transformed into a **retrievable knowledge base** $\mathcal{K}$.
2. A retriever $\mathcal{R}$ selects query-relevant evidence $\mathcal{E}$ from $\mathcal{K}$.
3. A generator $\mathcal{G}$ produces a textual answer grounded in the query and retrieved evidence:

$$
r = \mathcal{G}\bigl(q,\mathcal{R}(q,\mathcal{K})\bigr).
$$

The evidence may consist of **frames, clips, scenes, full videos, transcripts, audio, timestamps, metadata, or graph-structured records**.

<p align="center">
  <a href="imgC/videorag-framework.png"><img src="imgC/videorag-framework.png" width="96%" alt="Overall framework of Video RAG: knowledge base, retrieval and generation" /></a>
</p>


**Scope.** Video RAG differs from direct Video QA and captioning in its **explicit evidence retrieval** step. It also differs from video moment retrieval and text-to-video retrieval, where the retrieved interval or ranked video list is usually the final output rather than context for answer generation.

<p align="center">
  <a href="imgC/task-comparison.png"><img src="imgC/task-comparison.png" width="94%" alt="Comparison of Video RAG and related video understanding tasks" /></a>
</p>


---

## Evolution

Video RAG research has progressed from **external knowledge-assisted video description** to **video-grounded retrieval, multimodal memories, graph-based reasoning, adaptive retrieval, and streaming systems**. The timeline distinguishes **optimized retrieval strategies**, **internal knowledge bases**, and **external knowledge bases**; **†** denotes a RAG-adjacent method.

<p align="center">
  <a href="imgC/Evolution.png"><img src="imgC/Evolution.png" width="98%" alt="Evolution of Video RAG methods from 2018 to 2026" /></a>
</p>


---

## Taxonomy and Key Design Choices

Five design choices connect **organization, acquisition, and utilization**: index-unit granularity, knowledge structure, retrieval strategy, modality alignment, and generator integration. Together, they balance evidence coverage, retrieval effectiveness, context budget, and system cost.

<p align="center">
  <a href="imgC/key_design_choices.png"><img src="imgC/key_design_choices.png" width="100%" alt="Video RAG key design choices across organization, acquisition, and utilization" /></a>
</p>


---

## Video Knowledge Indexing and Representation

Video knowledge organization determines **what can be retrieved**. It includes corpus construction, multimodal content extraction, knowledge-base construction, and hybrid indexing.

### Multimodal Content Extraction

Visual frames and scene descriptions, audio events and ASR, OCR and subtitles, and temporal metadata are extracted and aligned into retrievable evidence units. Frame sampling may be uniform, keyframe-based, or adaptive.

<p align="center">
  <a href="imgC/multimodal-content-extraction-alignment.png"><img src="imgC/multimodal-content-extraction-alignment.png" width="96%" alt="Multimodal evidence extraction and temporal alignment" /></a>
</p>


---

## Retrieval Strategies and Mechanisms

Retrieval determines **which evidence is needed** for a query, **where to search**, and **how much evidence to return**. Typical operations include query rewriting, decomposition, modality-aware expansion, reranking, and temporal context expansion.

### Retrieval Granularity

The retrieval unit may be a **frame, clip, scene, video**, or an adaptive combination of levels. Finer units improve moment localization, while coarser units retain more contextual information.

<p align="center">
  <a href="imgC/multi_granular_retrieval.png"><img src="imgC/multi_granular_retrieval.png" width="96%" alt="Frame, clip, scene and video granularity in retrieval" /></a>
</p>


### Retrieval Mechanism

- **Single-step retrieval:** selects relevant evidence in one retrieval pass.
- **Multi-step retrieval:** iteratively refines or expands evidence across retrieval steps.
- **Adaptive retrieval:** chooses the retrieval configuration, depth, modalities, or stopping conditions according to the query and evidence state.

<p align="center">
  <a href="imgC/retrieval_strategies.png"><img src="imgC/retrieval_strategies.png" width="96%" alt="Single-step, multi-step, and adaptive retrieval workflows" /></a>
</p>


---

## Retrieval Augmentation and Generation

After retrieval, evidence is aligned with the query, organized within the available context budget, and supplied to a generator to produce a **grounded response**.

### Multimodal Fusion

**Early fusion** combines evidence before the generator's main reasoning process; **mid fusion** integrates modalities through learned or intermediate representations; and **late fusion** consolidates outputs or evidence from multiple sources.

<p align="center">
  <a href="imgC/Table_Multimodal_Fusion.png"><img src="imgC/Table_Multimodal_Fusion.png" width="2000" alt="Early, intermediate, and late multimodal evidence fusion" /></a>
</p>


### Generator Models

Text-only LLMs can reason over textualized video records; LVLMs and MLLMs can incorporate retrieved visual evidence and additional modalities. Model selection affects context length, evidence format, computational cost, and grounding capabilities.

<p align="center">
  <a href="imgC/Table_Generator_Types_Systems.png"><img src="imgC/Table_Generator_Types_Systems.png" width="2000" alt="Generator types and Video RAG systems" /></a>
</p>



### Integration Paradigms of Video RAG Systems

The survey distinguishes **training-enhanced integration** (including supervised and preference-based optimization) from **training-free integration**. Agentic workflows can orchestrate retrieval and generation within either setting.

<p align="center">
  <a href="imgC/Table_Integration_Paradigms.png"><img src="imgC/Table_Integration_Paradigms.png" width="900" alt="Training-enhanced and training-free integration paradigms" /></a>
</p>


---

## Benchmarks and Datasets

| Benchmark                | Focus                                                        | Original paper                                               | Benchmark / data                                             |
| ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **MSVD**                 | Video captioning and video-text retrieval                    | [Paper](https://aclanthology.org/P11-1020/)                  | [Dataset](https://www.cs.utexas.edu/~ml/clamp/videoDescription/) |
| **MSR-VTT**              | Video captioning and video-text retrieval                    | [Paper](https://openaccess.thecvf.com/content_cvpr_2016/html/Xu_MSR-VTT_A_Large_CVPR_2016_paper.html) | [Dataset](https://cove.thecvf.com/datasets/839)              |
| **VATEX**                | Multilingual video captioning and retrieval                  | [Paper](https://openaccess.thecvf.com/content_ICCV_2019/html/Wang_VaTeX_A_Large-Scale_High-Quality_Multilingual_Dataset_for_Video-and-Language_Research_ICCV_2019_paper.html) | [Dataset](https://ericguo5513.github.io/vatex-website/)      |
| **ActivityNet Captions** | Dense video captioning                                       | [Paper](https://openaccess.thecvf.com/content_iccv_2017/html/Krishna_Dense-Captioning_Events_in_ICCV_2017_paper.html)                    | [Dataset](https://cs.stanford.edu/people/ranjaykrishna/densevid/) |
| **YouCook2**             | Instructional video captioning and procedure segmentation    | [Paper](https://ojs.aaai.org/index.php/AAAI/article/view/12342) | [Dataset / leaderboard](https://github.com/LuoweiZhou/YouCook2-Leaderboard) |
| **BDD-X**                | Driving video explanations and captioning                    | [Paper](https://openaccess.thecvf.com/content_ECCV_2018/html/Jinkyu_Kim_Textual_Explanations_for_ECCV_2018_paper.html) | [Project / data](https://github.com/JinkyuKimUCB/explainable-deep-driving) |
| **Spoken-SAX**           | Spoken driving-action explanations                           | [Paper](https://arxiv.org/abs/2402.10828)                    | —                                                            |
| **News Video Dataset**   | News video description with background knowledge             | [Paper](https://aclanthology.org/D18-1433/)                  | [Dataset](https://github.com/SpencerWhitehead/NewsVideoDataset) |
| **WikiVideo**            | Multi-video article generation                               | [Paper](https://aclanthology.org/2026.findings-acl.3/)       | [Dataset / code](https://github.com/alexmartin1722/wikivideo)         |
| **MSVD-QA**              | Open-ended video question answering                          | [Paper](https://doi.org/10.1145/3123266.3123427)             | [Dataset / code](https://github.com/xudejing/video-question-answering) |
| **MSRVTT-QA**            | Open-ended video question answering                          | [Paper](https://doi.org/10.1145/3123266.3123427)             | [Dataset / code](https://github.com/xudejing/video-question-answering) |
| **TGIF-QA**              | Video question answering over animated GIFs                  | [Paper](https://openaccess.thecvf.com/content_cvpr_2017/html/Jang_TGIF-QA_Toward_Spatio-Temporal_CVPR_2017_paper.html)                    | [Dataset](https://github.com/YunseokJANG/tgif-qa)            |
| **ActivityNet-QA**       | Video question answering                                     | [Paper](https://ojs.aaai.org/index.php/AAAI/article/view/4946)                    | [Dataset](https://github.com/MILVLG/activitynet-qa)          |
| **TVQA**                 | Compositional question answering over TV episodes            | [Paper](https://aclanthology.org/D18-1167/)                    | [Dataset](https://tvqa.cs.unc.edu/)                          |
| **How2QA**               | Instructional video question answering                       | [Paper](https://aclanthology.org/2020.emnlp-main.161/)       | [Dataset](https://github.com/ych133/How2R-and-How2QA)        |
| **NExT-QA**              | Causal and temporal video QA                                 | [Paper](https://openaccess.thecvf.com/content/CVPR2021/html/Xiao_NExT-QA_Next_Phase_of_Question-Answering_to_Explaining_Temporal_Actions_CVPR_2021_paper.html) | [Dataset](https://github.com/doc-doc/NExT-QA)                |
| **STAR**                 | Situated reasoning in real-world videos                      | [Paper](https://datasets-benchmarks-proceedings.neurips.cc/paper_files/paper/2021/hash/5ef059938ba799aaa845e1c2e8a762bd-Abstract-round2.html) | [Dataset / code](https://github.com/csbobby/STAR_Benchmark)  |
| **IntentQA**             | Intent-oriented video question answering                     | [Paper](https://openaccess.thecvf.com/content/ICCV2023/html/Li_IntentQA_Context-aware_Video_Intent_Reasoning_ICCV_2023_paper.html) | [Dataset / code](https://github.com/JoseponLee/IntentQA)     |
| **iVQA**                 | Open-ended video question answering                          | [Paper](https://openaccess.thecvf.com/content/ICCV2021/html/Yang_Just_Ask_Learning_To_Answer_Questions_From_Millions_of_Narrated_ICCV_2021_paper.html) | [Dataset / code](https://github.com/antoyang/just-ask)       |
| **MVBench**              | Multi-task multimodal video understanding                    | [Paper](https://openaccess.thecvf.com/content/CVPR2024/html/Li_MVBench_A_Comprehensive_Multi-modal_Video_Understanding_Benchmark_CVPR_2024_paper.html)                    | [Dataset / code](https://github.com/OpenGVLab/Ask-Anything)  |
| **EgoMCQ**               | Egocentric video multiple-choice video-text matching         | [Paper](https://proceedings.neurips.cc/paper_files/paper/2022/hash/31fb284a0aaaad837d2930a610cd5e50-Abstract-Conference.html)                    | [Dataset / code](https://github.com/showlab/EgoVLP)          |
| **CFVBench**             | Fine-grained video multimodal retrieval-augmented generation | [Paper](https://doi.org/10.1145/3774904.3792726)             | [Benchmark / code](https://github.com/CQU-MM-Intelligent-Lab/CFVBENCH) |
| **Video SimpleQA**       | Factual, multi-hop video question answering                  | [Paper](https://ojs.aaai.org/index.php/AAAI/article/view/37249) | [Project](https://videosimpleqa.github.io/)                  |
| **ForeSeaQA**            | Surveillance video QA with image-text queries and temporal grounding | [Paper](https://doi.org/10.1007/978-3-032-37461-5_10)                    | [Dataset](https://huggingface.co/datasets/qualcomm/ForeSeaQA) |
| **LSMDC**                | Movie description and text-video retrieval                   | [Paper](https://doi.org/10.1007/s11263-016-0987-1)           | [Dataset](https://sites.google.com/site/describingmovies/previous-years/lsmdc-2016/moviedescription) |
| **DiDeMo**               | Moment localization and text-video retrieval                 | [Paper](https://openaccess.thecvf.com/content_iccv_2017/html/Hendricks_Localizing_Moments_in_ICCV_2017_paper.html)                    | [Dataset / code](https://github.com/LisaAnne/LocalizingMoments) |
| **EK100 MIR**            | Egocentric multi-instance video-text retrieval               | [Paper](https://doi.org/10.1007/s11263-021-01531-2)          | [Dataset / benchmark](https://github.com/epic-kitchens/C5-Multi-Instance-Retrieval) |
| **Charades-Ego**         | Egocentric video understanding and retrieval                 | [Paper](https://arxiv.org/abs/1804.09626)                    | [Annotations](https://ai2-public-datasets.s3-us-west-2.amazonaws.com/charades/CharadesEgo.zip) |
| **Charades-STA**         | Temporal sentence grounding                                  | [Paper](https://arxiv.org/abs/1705.02101)                    | [Code](https://github.com/jiyanggao/TALL)                    |
| **QVHighlights**         | Query-focused moment retrieval and highlight detection       | [Paper](https://arxiv.org/abs/2107.09609)                    | [Dataset / code](https://github.com/jayleicn/moment_detr)    |
| **Ego4D**                | Egocentric video understanding and temporal grounding        | [Paper](https://openaccess.thecvf.com/content/CVPR2022/html/Grauman_Ego4D_Around_the_World_in_3000_Hours_of_Egocentric_Video_CVPR_2022_paper.html) | [Dataset](https://ego4d-data.org/)                           |
| **MovieQA**              | Movie story question answering                               | [Paper](https://openaccess.thecvf.com/content_cvpr_2016/html/Tapaswi_MovieQA_Understanding_Stories_CVPR_2016_paper.html) | [Dataset](http://movieqa.cs.toronto.edu/)                    |
| **MovieChat-1K**         | Long-video captioning and question answering                 | [Paper](https://openaccess.thecvf.com/content/CVPR2024/html/Song_MovieChat_From_Dense_Token_to_Sparse_Memory_for_Long_Video_CVPR_2024_paper.html)                    | [Dataset / code](https://github.com/wenhaochai/MovieChat)    |
| **TVQA-Long**            | Episode-length TV video QA                                   | [Paper](doi.org/10.1007/978-3-031-73397-0_15)                    | [Project](https://vision-cair.github.io/Goldfish_website/)   |
| **EgoSchema**            | Very long-form egocentric video QA                           | [Paper](https://proceedings.neurips.cc/paper_files/paper/2023/hash/90ce332aff156b910b002ce4e6880dec-Abstract-Datasets_and_Benchmarks.html) | [Dataset](https://github.com/egoschema/EgoSchema)            |
| **LVU**                  | Long-form video understanding                                | [Paper](https://openaccess.thecvf.com/content/CVPR2021/html/Wu_Towards_Long-Form_Video_Understanding_CVPR_2021_paper.html) | [Dataset / code](https://github.com/chaoyuaw/lvu)            |
| **LongVideoBench**       | Long-context interleaved video-language reasoning            | [Paper](https://proceedings.neurips.cc/paper_files/paper/2024/hash/329ad516cf7a6ac306f29882e9c77558-Abstract-Datasets_and_Benchmarks_Track.html)                    | [Dataset / code](https://github.com/longvideobench/LongVideoBench) |
| **MLVU**                 | Multi-task long-video understanding                          | [Paper](https://openaccess.thecvf.com/content/CVPR2025/html/Zhou_MLVU_Benchmarking_Multi-task_Long_Video_Understanding_CVPR_2025_paper.html) | [Dataset / code](https://github.com/JUNJIE99/MLVU)           |
| **LVBench**              | Extreme long-video understanding                             | [Paper](https://openaccess.thecvf.com/content/ICCV2025/html/Wang_LVBench_An_Extreme_Long_Video_Understanding_Benchmark_ICCV_2025_paper.html) | [Dataset / code](https://github.com/zai-org/LVBench)         |
| **VNBench**              | Needle-in-a-haystack video retrieval, ordering and counting  | [Paper](https://proceedings.iclr.cc/paper_files/paper/2025/hash/f7b77476d89d5fb58aeb77691d2f40f5-Abstract-Conference.html) | [Dataset / code](https://github.com/CASIA-IVA-Lab/VideoNIAH) |
| **Video-MME**            | Multimodal video understanding across durations              | [Paper](https://openaccess.thecvf.com/content/CVPR2025/html/Fu_Video-MME_The_First-Ever_Comprehensive_Evaluation_Benchmark_of_Multi-modal_LLMs_in_CVPR_2025_paper.html) | [Dataset / code](https://github.com/MME-Benchmarks/Video-MME) |
| **Video-MMMU**           | Learning from educational videos and multidisciplinary knowledge | [Paper](https://aclanthology.org/2026.acl-long.1281/)        | [Project](https://videommmu.github.io/)                      |
| **MMBench-Video**        | Long-form multi-shot video understanding                     | [Paper](https://proceedings.neurips.cc/paper_files/paper/2024/hash/a2326c9715a516c91174132e0170073a-Abstract-Datasets_and_Benchmarks_Track.html)                    | [Dataset](https://huggingface.co/datasets/opencompass/MMBench-Video) |
| **MMR-V**                | Multimodal deep reasoning in videos                          | [Paper](https://proceedings.iclr.cc/paper_files/paper/2026/hash/6f1989abe9562c5cd306e070725fe0a3-Abstract-Conference.html)                    | [Dataset / code](https://github.com/GaryStack/MMR-V)         |
| **Video-Holmes**         | Complex multi-clue causal video reasoning                    | [Paper](https://link.springer.com/chapter/10.1007/978-3-032-37531-5_30)                    | [Dataset / code](https://github.com/TencentARC/Video-Holmes) |
| **VSI-Bench**            | Visual-spatial reasoning in videos                           | [Paper](https://openaccess.thecvf.com/content/CVPR2025/html/Yang_Thinking_in_Space_How_Multimodal_Large_Language_Models_See_Remember_CVPR_2025_paper.html) | [Dataset / code](https://github.com/vision-x-nyu/thinking-in-space) |
| **HiVU**                 | Hierarchical video understanding                             | [Paper](https://proceedings.neurips.cc/paper_files/paper/2025/hash/092359ce5cf60a80e882378944bf1be4-Abstract-Conference.html) | [Project / code](https://github.com/xzc-zju/AdaVideoRAG)     |
| **LongerVideos**         | Cross-video and extremely long-context QA                    | [Paper](https://dl.acm.org/doi/10.1145/3770854.3783944)                    | [Dataset / code](https://github.com/HKUDS/VideoRAG)          |
| **EgoLifeQA**            | Lifelong egocentric video QA                                 | [Paper](https://openaccess.thecvf.com/content/CVPR2025/html/Yang_EgoLife_Towards_Egocentric_Life_Assistant_CVPR_2025_paper.html) | [Dataset](https://huggingface.co/datasets/lmms-lab/EgoLife/tree/main/EgoLifeQA) |
| **Ego-R1**               | Long-horizon egocentric video reasoning                      | [Paper](https://arxiv.org/abs/2506.13654)                    | [Dataset](https://huggingface.co/datasets/Ego-R1/Ego-R1-Bench) |
| **HippoVlog**            | Long-form audiovisual vlog associative memory                | [Paper](https://ieeexplore.ieee.org/document/11698447/)                    | [Project / code](https://github.com/linyueqian/HippoMM)      |
| **CinePile**             | Long-video question answering                                | [Paper](https://arxiv.org/abs/2405.08813)                    | [Dataset](https://huggingface.co/datasets/tomg-group-umd/cinepile) |
| **ScaleLong**            | Multi-timescale long-video understanding                     | [Paper](https://proceedings.iclr.cc/paper_files/paper/2026/hash/fa1cfe4e956d85e016b1f8f49b189a0b-Abstract-Conference.html) | [Dataset / code](https://github.com/multimodal-art-projection/ScaleLong) |
| **Event-Bench**          | Event-centric video understanding                            | [Paper](https://arxiv.org/abs/2406.14129)                    | [Dataset / code](https://github.com/RUCAIBox/Event-Bench)    |
| **CG-Bench**             | Clue-grounded long-video QA                                  | [Paper](https://proceedings.iclr.cc/paper_files/paper/2025/hash/70fa5df8e3300dc30bf19bee44a56155-Abstract-Conference.html) | [Benchmark](https://cg-bench.github.io/leaderboard/)         |
| **EC-Bench**             | Ultra-long video event enumeration and counting              | [Paper](https://arxiv.org/abs/2603.29943)                    | [Dataset](https://huggingface.co/datasets/vai-org/EC-Bench)  |
| **MM-Lifelong**          | Lifelong multimodal video understanding                      | [Paper](https://arxiv.org/abs/2603.05484)                    | [Dataset](https://huggingface.co/datasets/MM-Lifelong/MM-Lifelong) |
| **TempCompass**          | Temporal video understanding                                 | [Paper](https://aclanthology.org/2024.findings-acl.517/)                    | [Dataset / code](https://github.com/llyx97/TempCompass)      |
| **VRBench**              | Multi-step reasoning in long narrative videos                | [Paper](https://openaccess.thecvf.com/content/ICCV2025/html/Yu_VRBench_A_Benchmark_for_Multi-Step_Reasoning_in_Long_Narrative_Videos_ICCV_2025_paper.html) | [Dataset / code](https://github.com/OpenGVLab/VRBench)       |
| **LongVidSearch**        | Multi-hop evidence retrieval planning                        | [Paper](https://arxiv.org/abs/2603.14468)                    | [Dataset](https://huggingface.co/datasets/Fishiing/LongVidSearch) |
| **OVO-Bench**            | Streaming video understanding                                | [Paper](https://openaccess.thecvf.com/content/CVPR2025/html/Niu_OVO-Bench_How_Far_is_Your_Video-LLMs_from_Real-World_Online_Video_CVPR_2025_paper.html) | [Dataset / code](https://github.com/JoeLeelyf/OVO-Bench)     |
| **StreamingBench**       | Real-time streaming video understanding                      | [Paper](https://ieeexplore.ieee.org/document/11463959)                    | [Dataset / code](https://github.com/THUNLP-MT/StreamingBench) | - |



## Evaluation

Answer accuracy alone does not identify whether a Video RAG system has retrieved the right evidence, localized the correct moment, or generated a well-supported answer. Evaluation should jointly assess **retrieval, grounding, generation, robustness, and efficiency**.

<p align="center">
  <a href="imgC/Fig9_Evaluation.png"><img src="imgC/Fig9_Evaluation.png" alt="Fig. 9. Original evaluation taxonomy from the survey, including numbered references" /></a>
</p>


| Dimension                 | Representative metrics / protocols                           |
| ------------------------- | ------------------------------------------------------------ |
| **Evidence retrieval**    | Recall@K, Precision@K, F1@K, Hit Rate, mAP, nDCG, MRR, MedR, MnR |
| **Temporal grounding**    | tIoU, mIoU, IoU-thresholded Recall@K, MTGS                   |
| **Citation attribution**  | CiteP, CiteR, CiteF1; support of generated claims by cited video evidence |
| **Answer generation**     | Accuracy, Exact Match, F1, ANLS, BLEU, METEOR, ROUGE, CIDEr, BERTScore, STEM, InfoP, InfoR, InfoF1, LLM-as-a-judge |
| **Long-video robustness** | Accuracy, M-Avg, G-Avg, Overall score |
| **Cross-video reasoning** | O.Avg, C.Avg, T.Avg, M.Avg |
| **System efficiency**     | RLatency, Wall Speedup, Memory, Retained, TFLOPs, #Frames, #Tokens |

For reproducibility, report the **video duration**, **frame-sampling budget**, **retrieved evidence count and granularity**, **used modalities**, **indexing cost**, **generator**, and **inference settings** alongside answer metrics.

---

## Paper Collection

Video RAG and retrieval-based video methods are listed chronologically. **†** denotes a RAG-adjacent method.

### Video RAG Systems and Methods

| Year | Paper                                                        | Venue / source               |                         Code                          |
| :--: | ------------------------------------------------------------ | ---------------------------- | :---------------------------------------------------: |
| 2018 | [**Know-Aware VD** — Incorporating background knowledge into video description generation](https://aclanthology.org/D18-1433/) | EMNLP 2018                   |                           —                           |
| 2023 | [**R-ConvED** — Retrieval augmented convolutional encoder-decoder networks for video captioning](https://doi.org/10.1145/3539225) | ACM TOMM 2023                |                           —                           |
| 2023 | [**R2A** — Retrieving-to-answer: Zero-shot video question answering with frozen large language models](https://openaccess.thecvf.com/content/ICCV2023W/MMFM/html/Pan_Retrieving-to-Answer_Zero-Shot_Video_Question_Answering_with_Frozen_Large_Language_Models_ICCVW_2023_paper.html) | ICCV Workshops 2023          |                           —                           |
| 2023 | [**ChatVideo**: A tracklet-centric multimodal and versatile video understanding system](https://arxiv.org/abs/2304.14407) | arXiv 2023                   |    [Code](https://github.com/yiwengxie/Chat-Video)    |
| 2023 | [**SeViLA** — Self-chained image-language model for video localization and question answering](https://proceedings.neurips.cc/paper_files/paper/2023/hash/f22a9af8dbb348952b08bd58d4734b50-Abstract-Conference.html) | NeurIPS 2023                 |      [Code](https://github.com/Yui010206/SeViLA)      |
| 2024 | [**iRAG**: Advancing RAG for Videos with an Incremental Approach](https://doi.org/10.1145/3627673.3680088) | CIKM 2024                    |                           —                           |
| 2024 | [**ViTA**: An Efficient Video-to-Text Algorithm Using VLM for RAG-Based Video Analysis System](https://openaccess.thecvf.com/content/CVPR2024W/MAR/html/Arefeen_ViTA_An_Efficient_Video-to-Text_Algorithm__using_VLM_for_RAG-based_CVPRW_2024_paper.html) | CVPR Workshops 2024          |                           —                           |
| 2024 | [**Goldfish**: Vision-language understanding of arbitrarily long videos](https://www.ecva.net/papers/eccv_2024/papers_ECCV/html/4213_ECCV_2024_paper.php) | ECCV 2024                    | [Code](https://github.com/Vision-CAIR/MiniGPT4-video) |
| 2024 | [**RAG-Indoor** — Retrieval-Augmented-Generation-Enhanced Dense Video Caption for Human Indoor Activities: Disambiguating Caption Using Spatial Information Beyond Field of View Constraints](https://doi.org/10.18494/SAM5238) | Sensors and Materials 2024   |                           —                           |
| 2024 | [**VideoAgent**: A memory-augmented multimodal agent for video understanding](https://www.ecva.net/papers/eccv_2024/papers_ECCV/html/3241_ECCV_2024_paper.php) | ECCV 2024                    |   [Code](https://github.com/YueFan1014/VideoAgent)    |
| 2024 | [**MA-LMM†**: Memory-augmented large multimodal model for long-term video understanding](https://openaccess.thecvf.com/content/CVPR2024/html/He_MA-LMM_Memory-Augmented_Large_Multimodal_Model_for_Long-Term_Video_Understanding_CVPR_2024_paper.html) | CVPR 2024                    |       [Code](https://github.com/boheumd/MA-LMM)       |
| 2024 | [**EgoInstructor** — Retrieval-augmented egocentric video captioning](https://openaccess.thecvf.com/content/CVPR2024/html/Xu_Retrieval-Augmented_Egocentric_Video_Captioning_CVPR_2024_paper.html) | CVPR 2024                    | [Code](https://github.com/Jazzcharles/Egoinstructor)  |
| 2024 | [**RAG-Driver**: Generalisable driving explanations with retrieval-augmented in-context learning in multi-modal large language model](https://arxiv.org/abs/2402.10828) | arXiv 2024                   |                           —                           |
| 2025 | [**Enhance-RAG†** — Enhancing Video Retrieval via Synergized Image Embeddings and RAG](https://link.springer.com/chapter/10.1007/978-981-96-4291-5_3) | SOICT 2024 (Springer 2025)   |                           —                           |
| 2025 | [**TV-RAG**: A temporal-aware and semantic entropy-weighted framework for long video retrieval and understanding](https://doi.org/10.1145/3746027.3755873) | ACM MM 2025                  | [Code](https://github.com/AI-Researcher-Team/TV-RAG)  |
| 2025 | [**OneClip-RAG** — Towards Effective Long Video Understanding of Multimodal Large Language Models via One-shot Clip Retrieval](https://arxiv.org/abs/2512.08410) | arXiv 2025                   |                           —                           |
| 2025 | [**RAPO** — The Devil Is in the Prompts: Retrieval-Augmented Prompt Optimization for Text-to-Video Generation](https://openaccess.thecvf.com/content/CVPR2025/html/Gao_The_Devil_is_in_the_Prompts_Retrieval-Augmented_Prompt_Optimization_for_CVPR_2025_paper.html) | CVPR 2025                    |                           —                           |
| 2025 | [**VRAG**: Retrieval-Augmented Video Question Answering for Long-Form Videos](https://openaccess.thecvf.com/content/CVPR2025W/IViSE/html/Gia_VRAG_Retrieval-Augmented_Video_Question_Answering_for_Long-Form_Videos_CVPRW_2025_paper.html) | CVPR Workshops 2025          |                           —                           |
| 2025 | [**FRAG**: Frame selection augmented generation for long video and long document understanding](https://arxiv.org/abs/2504.17447) | arXiv 2025                   |        [Code](https://github.com/NVlabs/FRAG)         |
| 2025 | [**VideoRAG (KAIST)** — Videorag: Retrieval-augmented generation over video corpus](https://aclanthology.org/2025.findings-acl.1096/) | Findings of ACL 2025         |     [Code](https://github.com/starsuzi/VideoRAG)      |
| 2025 | [**Video Swagger** — Social Media Video Creation: Harnessing the Power of Large Language Models for Non-Linear Editing and RAG-Based Question-Answering](https://ieeexplore.ieee.org/document/11158765/) | ICCTDC 2025                  |                           —                           |
| 2025 | [**SALOVA**: Segment-augmented long video assistant for targeted retrieval and routing in long-form video analysis](https://openaccess.thecvf.com/content/CVPR2025/html/Kim_SALOVA_Segment-Augmented_Long_Video_Assistant_for_Targeted_Retrieval_and_Routing_CVPR_2025_paper.html) | CVPR 2025                    |      [Code](https://github.com/IVY-LVLM/SALOVA)       |
| 2025 | [**CadenceRAG**: Context-Aware and Dependency-Enhanced Retrieval Augmented Generation for Holistic Video Understanding](https://openaccess.thecvf.com/content/CVPR2025W/IViSE/html/Liu_CadenceRAG_Context-Aware_and_Dependency-Enhanced_Retrieval_Augmented_Generation_for_Holistic_Video_CVPRW_2025_paper.html) | CVPR Workshops 2025          |                           —                           |
| 2025 | [**DrVideo**: Document retrieval based long video understanding](https://openaccess.thecvf.com/content/CVPR2025/html/Ma_DrVideo_Document_Retrieval_Based_Long_Video_Understanding_CVPR_2025_paper.html) | CVPR 2025                    |     [Code](https://github.com/Upper9527/DrVideo)      |
| 2025 | [**X-CoT†**: Explainable Text-to-Video Retrieval via LLM-Based Chain-of-Thought Reasoning](https://aclanthology.org/2025.emnlp-main.1588/) | EMNLP 2025                   |  [Code](https://github.com/PrasannaPulakurthi/X-CoT)  |
| 2025 | [**FilterRAG**: zero-shot informed retrieval-augmented generation to mitigate hallucinations in VQA](https://arxiv.org/abs/2502.18536) | arXiv 2025                   |                           —                           |
| 2025 | [**VideoTree**: Adaptive tree-based video representation for llm reasoning on long videos](https://openaccess.thecvf.com/content/CVPR2025/html/Wang_VideoTree_Adaptive_Tree-based_Video_Representation_for_LLM_Reasoning_on_Long_CVPR_2025_paper.html) | CVPR 2025                    |    [Code](https://github.com/Ziyang412/VideoTree)     |
| 2025 | [**E-VRAG**: Enhancing long video understanding with resource-efficient retrieval augmented generation](https://arxiv.org/abs/2508.01546) | arXiv 2025                   |                           —                           |
| 2025 | [**MemVid** — Memory-enhanced retrieval augmentation for long video understanding](https://arxiv.org/abs/2503.09149) | arXiv 2025                   |                           —                           |
| 2025 | [**VideoExplorer**: Think with videos for agentic long-video understanding](https://arxiv.org/abs/2506.10821) | arXiv 2025                   |                           —                           |
| 2025 | [**Magnet**: A multi-agent framework for finding audio-visual needles by reasoning over multi-video haystacks](https://proceedings.neurips.cc/paper_files/paper/2025/hash/465c08c85e6f1015358f2e0c0a0cbaf9-Abstract-Conference.html) | NeurIPS 2025                 |                           —                           |
| 2025 | [**Video-RAG**: Visually-aligned retrieval-augmented long video comprehension](https://papers.nips.cc/paper_files/paper/2025/hash/f593c9c251d4d7cf14d4ab9861dfb7eb-Abstract-Conference.html) | NeurIPS 2025                 | [Code](https://github.com/Leon1207/Video-RAG-master)  |
| 2025 | [**Vgent**: Graph-based retrieval-reasoning-augmented generation for long video understanding](https://proceedings.neurips.cc/paper_files/paper/2025/hash/91b75715a2ae8f2542b78be31941eac3-Abstract-Conference.html) | NeurIPS 2025                 |    [Code](https://github.com/xiaoqian-shen/Vgent)     |
| 2025 | [**VimoRAG**: Video-Based Retrieval-Augmented 3D Motion Generation for Motion Language Models](https://proceedings.neurips.cc/paper_files/paper/2025/hash/312237ba5de457df7bc8f88d4de21c4c-Abstract-Conference.html) | NeurIPS 2025                 |    [Code](https://github.com/WalkerMitty/VimoRAG)     |
| 2025 | [**AdaVideoRAG**: Omni-Contextual Adaptive Retrieval-Augmented Efficient Long Video Understanding](https://proceedings.neurips.cc/paper_files/paper/2025/hash/092359ce5cf60a80e882378944bf1be4-Abstract-Conference.html) | NeurIPS 2025                 |    [Code](https://github.com/xzc-zju/AdaVideoRAG)     |
| 2025 | [**LightVideoRAG**: Low-Resource Long Video Question-Answering via Adaptive Sampling and Context-Aware Retrieval](https://link.springer.com/chapter/10.1007/978-3-032-11768-7_2) | MoMM 2025        |   [Code](https://github.com/linshys/lightvideoRAG)    |
| 2026 | [**VideoRAG (VIT)†** — Scalable Multilingual Retrieval of Lecture Moments Through Whisper-Based Transcripts and Vector Search](https://ieeexplore.ieee.org/document/11456535/) | IEEE Access 2026             |                           —                           |
| 2026 | [**QualiRAG**: Retrieval-augmented generation for visual quality understanding](https://arxiv.org/abs/2601.18195) | arXiv 2026                   |                           —                           |
| 2026 | [**ViG-RAG**: Video-aware Graph Retrieval-Augmented Generation via Temporal and Semantic Hybrid Reasoning](https://doi.org/10.1609/aaai.v40i1.36963) | AAAI 2026                    | [Code](https://github.com/AI-Researcher-Team/ViG-RAG) |
| 2026 | [**MARQUIS**: A Three-Stage Pipeline for Video Retrieval-Augmented Generation](https://aclanthology.org/2026.magmar-main.13/) | MAGMaR (ACL Workshop) 2026   |     [Code](https://github.com/debashishc/marquis)     |
| 2026 | [**Event-Anchored** frame selection for effective long-video understanding](https://arxiv.org/abs/2603.00983) | arXiv 2026                   |                           —                           |
| 2026 | [**Sprout**: Building Dynamic Memory While Reasoning for Agentic Video Understanding](https://arxiv.org/abs/2609.35497) | arXiv 2026                   |                           —                           |
| 2026 | [**VideoChat-M1**: Collaborative policy planning for video understanding via multi-agent reinforcement learning](https://openaccess.thecvf.com/content/CVPR2026/html/Chen_VideoChat-M1_Collaborative_Policy_Planning_for_Video_Understanding_via_Multi-Agent_Reinforcement_CVPR_2026_paper.html) | CVPR 2026                    |                           —                           |
| 2026 | [**MERIT** — Keep It Simple: Multi-Key Episodic Memory Retrieval for Ultra-Long Video Understanding](https://arxiv.org/abs/2608.07663) | ECCV 2026                    |                           —                           |
| 2026 | [**C2F-RAG** — Decoupling Semantics and Logic: A Training-Free Coarse-to-Fine Pipeline for Video Retrieval-Augmented Generation](https://aclanthology.org/2026.magmar-main.12/) | MAGMaR (ACL Workshop) 2026   |                           —                           |
| 2026 | [**VideoStir**: Understanding long videos via spatio-temporally structured and intent-aware rag](https://aclanthology.org/2026.acl-long.1656/) | ACL 2026                     |      [Code](https://github.com/RomGai/VideoStir)      |
| 2026 | [**MAVIS** — Multi-Agent Video Intelligent System for Educational and Archival Video Analysis](https://ieeexplore.ieee.org/document/11438286/) | ICIPCN 2026                  |                           —                           |
| 2026 | [**VideoRAG (ZJU)** — Rewind-Augmented Generation for Long Video Understanding](https://doi.org/10.1109/TMM.2026.3679059) | IEEE TMM 2026                |                           —                           |
| 2026 | [**DAGC** — Coarse Indexing, Fine Evidence: Decoupling Temporal Granularity in Long-Video RAG](https://arxiv.org/abs/2608.23011) | arXiv 2026                   |                           —                           |
| 2026 | [**NR-MV-RAG** — Noise-Resilient Multimodal Video RAG Enhancing Urdu Retrieval](https://ieeexplore.ieee.org/document/11591849/) | iCoMET 2026                  |                           —                           |
| 2026 | [**TrafficRAG**: Temporal Grounding for Traffic Violations via Retrieval-Augmented Generation](https://openaccess.thecvf.com/content/WACV2026W/RWS/html/Kim_TrafficRAG_Temporal_Grounding_for_Traffic_Violations_via_Retrieval-Augmented_Generation_WACVW_2026_paper.html) | WACV Workshops 2026          |                           —                           |
| 2026 | [**ReFoCUS**: Reinforcement-guided frame optimization for contextual understanding](https://openaccess.thecvf.com/content/CVPR2026F/html/Lee_ReFoCUS_Reinforcement-guided_Frame_Optimization_for_Contextual_Understanding_CVPRF_2026_paper.html) | CVPR Findings 2026           |   [Code](https://github.com/interlive-team/ReFoCUS)   |
| 2026 | [**FastV-RAG**: Towards Fast and Fine-Grained Video QA with Retrieval-Augmented Generation](https://aclanthology.org/2026.acl-long.38/) | ACL 2026                     |     [Code](https://github.com/FastVRAG/FastV-RAG)     |
| 2026 | [**OASIS**: On-Demand Hierarchical Event Memory for Streaming Video Reasoning](https://arxiv.org/abs/2604.17052) | arXiv 2026                   |                           —                           |
| 2026 | [**RAVU**: Retrieval augmented video understanding with compositional reasoning over graph](https://openaccess.thecvf.com/content/WACV2026/html/Malik_RAVU_Retrieval_Augmented_Video_Understanding_with_Compositional_Reasoning_over_Graph_WACV_2026_paper.html) | WACV 2026                    |                           —                           |
| 2026 | [**Multi-RAG**: A Multimodal Retrieval-Augmented Generation System for Adaptive Video Understanding](https://doi.org/10.1007/978-981-92-1947-6_30) | PAKDD 2026 (LNCS cite 2027)  |                           —                           |
| 2026 | [**WikiVideo**: Article generation from multiple videos](https://aclanthology.org/2026.findings-acl.3/) | Findings of ACL 2026         |  [Code](https://github.com/alexmartin1722/wikivideo)  |
| 2026 | [**ForeSea**: AI forensic search with multi-modal queries for video surveillance](https://arxiv.org/abs/2603.22872) | ECCV 2026                    |                           —                           |
| 2026 | [**UrbanClipAtlas**: A Visual Analytics Framework for Event and Scene Retrieval in Urban Videos](https://doi.org/10.1111/cgf.70431) | Computer Graphics Forum 2026 |                           —                           |
| 2026 | [**EGAgent** — Agentic Very Long Video Understanding](https://aclanthology.org/2026.acl-long.2161/) | ACL 2026                     |  [Code](https://github.com/facebookresearch/egagent)  |
| 2026 | [**GEB** — Beyond the Timeline: Augmenting Long-Video Memory with Grounded Entity Biographies](https://arxiv.org/abs/2609.38155) | arXiv 2026                   |                           —                           |
| 2026 | [**VideoRAG (HKU)** — Videorag: Retrieval-augmented generation with extreme long-context videos](https://doi.org/10.1145/3770854.3783944) | KDD 2026                     |       [Code](https://github.com/HKUDS/VideoRAG)       |
| 2026 | [**StreamRAG**: Enhancing Real-Time Video Understanding with Retrieval Augmentation](https://openaccess.thecvf.com/content/CVPR2026/html/Xie_StreamRAG_Enhancing_Real-Time_Video_Understanding_with_Retrieval_Augmentation_CVPR_2026_paper.html) | CVPR 2026                    |                           —                           |
| 2026 | [**R-VLM** — Long Video Understanding with Learnable Retrieval in Video-Language Models](https://doi.org/10.1109/TMM.2026.3664996) | IEEE TMM 2026                |                           —                           |
| 2026 | [**AVA**: Towards Agentic Video Analytics with Vision Language Models](https://www.usenix.org/conference/nsdi26/presentation/yan) | USENIX NSDI 2026             |     [Code](https://github.com/I-ESC/Project-Ava)      |
| 2026 | [**Event-Causal RAG**: A Retrieval-Augmented Generation Framework for Long Video Reasoning in Complex Scenarios](https://arxiv.org/abs/2605.06185) | arXiv 2026                   |                           —                           |
| 2026 | [**REVEAL**: A Rubric-Guided Agent for Explicit Evidence Sufficiency Verification in Long-Video Question Answering](https://arxiv.org/abs/2608.08612) | arXiv 2026                   |                           —                           |
| 2026 | [**TRACE**: Evidence Grounding-Guided Multi-Video Event Understanding and Claim Generation](https://aclanthology.org/2026.magmar-main.14/) | MAGMaR (ACL Workshop) 2026   |      [Code](https://github.com/pengyu965/TRACE)       |
| 2026 | [**Graph-to-Frame RAG**: Visual-Space Knowledge Fusion for Training-Free and Auditable Video Reasoning](https://arxiv.org/abs/2604.04372) | arXiv 2026                   |                           —                           |
| 2026 | [**WorldMM**: Dynamic multimodal memory agent for long video reasoning](https://openaccess.thecvf.com/content/CVPR2026/html/Yeo_WorldMM_Dynamic_Multimodal_Memory_Agent_for_Long_Video_Reasoning_CVPR_2026_paper.html) | CVPR 2026                    |       [Code](https://github.com/wgcyeo/WorldMM)       |
| 2026 | [**HAVEN** — Hierarchical Long Video Understanding with Audiovisual Entity Cohesion and Agentic Search](https://openaccess.thecvf.com/content/CVPR2026/html/Yin_Hierarchical_Long_Video_Understanding_with_Audiovisual_Entity_Cohesion_and_Agentic_CVPR_2026_paper.html) | CVPR 2026                    |                           —                           |
| 2026 | [**VideoARM**: Agentic reasoning over hierarchical memory for long-form video understanding](https://openaccess.thecvf.com/content/CVPR2026/html/Yin_VideoARM_Agentic_Reasoning_over_Hierarchical_Memory_for_Long-Form_Video_Understanding_CVPR_2026_paper.html) | CVPR 2026                    |      [Code](https://github.com/MILVLG/videoarm)       |
| 2026 | [**SceneRAG**: Scene-level retrieval-augmented generation for video understanding](https://ieeexplore.ieee.org/document/11462022/) | ICASSP 2026                  |                           —                           |
| 2026 | [**SafeScreen**: A Safety-First Screening Framework for Personalized Video Retrieval for Vulnerable Users](https://arxiv.org/abs/2604.03264) | arXiv 2026                   |                           —                           |
| 2026 | [**VideoTrace-R1**: Long Video-based Retrieval-Augmented Generation via Reinforcement Learning](https://proceedings.mlr.press/v306/cao26m.html) | ICML 2026                    |                           —                           |

---

## Applications

The survey discusses Video RAG in several domains:

- **Education and knowledge learning:** lecture search, instructional QA, and personalized review.
- **Film and entertainment:** story understanding, event and character retrieval, and episode navigation.
- **Surveillance and public safety:** long-duration event search, anomaly investigation, and forensic review.
- **Embodied AI and robotics:** episodic memory, action and navigation history, and task planning.
- **Healthcare:** surgical video retrieval, monitoring, and case-grounded assistance.
- **Content creation:** source localization, news/video production, and retrieval-guided editing.
- **Enterprise and scientific workflows:** procedural knowledge retrieval, experimental videos, and searchable multimodal records.

## Challenges and Future Directions

1. **Hierarchical and long-term memory:** maintain entities, events, temporal dependencies, and source provenance across lengthy videos.
2. **Fine-grained, multimodal retrieval:** locate sparse or distributed evidence without discarding the context required for reasoning.
3. **Adaptive and agentic acquisition:** select modalities, granularities, tools, and retrieval stopping conditions dynamically.
4. **Efficient and deployable systems:** reduce indexing, storage, retrieval, and inference costs for streaming and real-time settings.
5. **Evidence-grounded generation:** improve cross-modal alignment, factual consistency, and traceable citation of retrieved moments.
6. **Unified evaluation:** jointly assess retrieval, localization, reasoning, answer quality, robustness, and computational cost.

---

## Contributing and Citation

We welcome additions of Video RAG papers, corrected publication or code links, new datasets and benchmarks, and feedback on taxonomy or evaluation coverage. Please open an issue or pull request with the paper **title, authors, venue/year, paper URL, official code/project URL (if available), and a short contribution summary**.

The final publication details will be added when available.

```bibtex
@misc{videorag_survey_2026,
  title = {A Comprehensive Survey on Video Retrieval-Augmented Generation: A System-Oriented Perspective},
  year  = {2026},
  note  = {Survey manuscript}
}
```
