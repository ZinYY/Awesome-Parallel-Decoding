<p align="center">
  <img src="asset/imgs/Parallel_Decoding_Logo.jpg" width="90%" alt="Parallel Decoding Survey Logo" />
</p>

# Awesome-Parallel-Decoding

[![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/ZinYY/Awesome-Parallel-Decoding)
[![Stars](https://img.shields.io/github/stars/ZinYY/Awesome-Parallel-Decoding?style=social)](https://github.com/ZinYY/Awesome-Parallel-Decoding)
[![License: CC BY-NC-ND 4.0](https://img.shields.io/badge/License-CC--BY--NC--ND%204.0-blue.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
[![PRWelcome](https://img.shields.io/badge/PRs-Welcome-red)](https://github.com/ZinYY/Awesome-Parallel-Decoding/pulls)
[![Last Commit](https://img.shields.io/github/last-commit/ZinYY/Awesome-Parallel-Decoding.svg)](https://github.com/ZinYY/Awesome-Parallel-Decoding)

📒 A curated list of **Awesome Parallel Decoding** papers to accelerate the inference of (autoregressive) LLMs.

Autoregressive (AR) decoding generates one token per forward pass, leaving the strict token-by-token dependency as the dominant latency bottleneck of modern LLM serving. **Parallel decoding** refers to the broad family of techniques that shorten this critical path by introducing *parallel candidates*, *parallel verification*, *parallel reasoning steps*, or *parallel task orchestration*, while ideally preserving the quality (and, for lossless methods, the exact output distribution) of the original model. This repository organizes the literature along the **granularity of parallelism** — from (i) individual tokens, to (ii) reasoning sequences, to (iii) whole tasks — and additionally tracks the (iv) **model architecture-level** and (v) **system-level** improvements. We also introduce (vi) **hybrid** advances and (vii) **related benchmarks** that make parallel decoding practical.

<!-- Concretely, the collection is structured into seven parts: **(1) Token-level** methods (speculative decoding, self-speculation, multi-head / multi-token prediction, tree verification, retrieval, multi-draft theory, and resource-aware variants); **(2) Sequence-level** methods that speculate over reasoning steps and semantic units; **(3) Task-level** methods that parallelize agents, tools, and function calls; **(4) Architecture-level** methods that change the generation paradigm itself (diffusion LLMs, block-diffusion, semi-autoregressive models); **(5) System and infrastructure** support in production serving engines; **(6) Hybrid** methods that fuse multiple paradigms (e.g., speculative decoding × diffusion, model cascades × speculation); and **(7) Benchmarks and evaluation** resources. Contributions via pull requests are warmly welcomed. -->

> **Legend.** Venue is given as the publication venue (e.g., `ICML'24`, `NeurIPS'25`); preprints use `arXiv'YY`. The bracketed token at the start of a title is the method's alias/acronym (e.g., `[EAGLE]`). Author and affiliation fields are filled where reliably verifiable and marked `N/A` otherwise. A blank Code cell means no official public code repository is currently known.

<a id="contents"></a>
## 📖 Contents

- [🗺️ Overview: A Taxonomy of Parallel Decoding](#taxonomy-overview)
- [📙 1. Token-Level Parallel Decoding](#class-1)
- [📙 2. Sequence-Level Parallel Decoding](#class-2)
- [📙 3. Task-Level Parallel Decoding](#class-3)
- [📙 4. Architecture-Level Parallel Decoding](#class-4)
- [📙 5. System and Infrastructure](#class-5)
- [📙 6. Hybrid Methods](#class-6)
- [📙 7. Benchmarks and Evaluation](#class-7)

<!-- <a id="contents-with-subclasses"></a>
## 📖 Contents with Subclasses

The taxonomy contains **296 category placements covering 294 unique papers**. **Agent Forest** and **JetSpec** each belong to two categories and therefore appear twice. Counts below summarize the complete paper-by-paper placements shown in the overview and detailed tables.

- [🗺️ Overview: A Taxonomy of Parallel Decoding](#taxonomy-overview)
- [📙 1. Token-Level Parallel Decoding](#class-1)

  - [1.1 Drafting with the Draft Model](#subclass-1-1) — An independent draft model proposes candidate tokens that the target LLM verifies; this includes foundational speculative-decoding frameworks, drafter training and distillation, and cross-architecture draft adaptation.

    **27 papers.**

  - [1.2 Drafting within the Target Model](#subclass-1-2) — The target model produces its own drafts through layer skipping, early exit, attached prediction heads, or internal feature prediction, removing the need for a separate full draft model.

    **36 papers.**

  - [1.3 Retrieval-Based and Non-Parametric Drafting](#subclass-1-3) — Drafts come from retrieved text, n-gram or suffix caches, reusable context patterns, or parameter-free fixed-point/Jacobi procedures.

    **17 papers.**

  - [1.4 Draft Structure and Sampling Strategies](#subclass-1-4) — The main contribution concerns candidate-tree structure, multi-draft coupling and acceptance, adaptive draft length, verification order, or theoretical properties of speculative sampling.

    **42 papers.**

  - [1.5 Cross-Technique Integration](#subclass-1-5) — Speculative decoding is integrated with another technique or application domain, including multimodal generation, quality/alignment/safety controls, watermarking, and accelerated reinforcement-learning rollouts.

    **19 papers.**

- [📙 2. Sequence-Level Parallel Decoding](#class-2)

  - [2.1 Multi-Sequence Parallel Generation](#subclass-2-1) — Several reasoning continuations or complete answer sequences are generated in parallel and then verified, corrected, selected, or aggregated at the semantic level.

    **16 papers.**

  - [2.2 Intra-Sequence Parallel Generation](#subclass-2-2) — One output is decomposed into structurally independent sections, subproblems, or graph nodes that can be generated concurrently.

    **11 papers.**

  - [2.3 Path Exploration and Test-Time Scaling](#subclass-2-3) — Parallel test-time compute is allocated across branching reasoning paths, with explicit control of exploration, pruning, early termination, and answer aggregation.

    **12 papers.**

- [📙 3. Task-Level Parallel Decoding](#class-3)

  - [3.1 Multi-Agent Parallel Collaboration](#subclass-3-1) — Multiple LLM agents or teams solve and deliberate concurrently, then combine their results through voting, synthesis, or coordination.

    **4 papers.**

  - [3.2 Parallel and Speculative Agent Execution](#subclass-3-2) — Independent tools, functions, or predicted future actions are planned, dispatched, or speculatively executed in parallel.

    **10 papers.**

- [📙 4. Architecture-Level Parallel Decoding](#class-4)

  - [4.1 Model Architecture and Training](#subclass-4-1) — The work designs, trains, converts, or scales architectures that support non-left-to-right generation, including masked diffusion, block diffusion, and semi-autoregressive models.

    **26 papers.**

  - [4.2 Decoding and Sampling Algorithms](#subclass-4-2) — Inference algorithms accelerate trained non-autoregressive or diffusion language models through unmasking policies, confidence schedules, trajectory methods, or error correction.

    **24 papers.**

- [📙 5. System and Infrastructure](#class-5)

  - [5.1 Serving and Scheduling](#subclass-5-1) — Runtime and serving techniques optimize batching, GPU scheduling, pipelines, distributed execution, caching, or SLO-aware adaptation for parallel decoding.

    **16 papers.**

  - [5.2 Algorithm-Infrastructure Co-Design](#subclass-5-2) — The decoding algorithm is co-designed with deployment constraints or systems techniques such as quantization, KV-cache management, MoE execution, or hardware-aware verification.

    **10 papers.**

- [📙 6. Hybrid Methods](#class-6)

  - [6.1 Non-AR Drafter for AR Targets](#subclass-6-1) — A non-autoregressive or semi-autoregressive language model drafts multiple tokens in parallel for verification by an autoregressive target.

    **9 papers.**

  - [6.2 Speculation for Other Generative Paradigms](#subclass-6-2) — The draft-then-verify principle is transferred to diffusion language models, model cascades, continuous diffusion models, and visual autoregressive generation.

    **10 papers.**

- [📙 7. Benchmarks and Evaluation](#class-7) — **7 papers** directly under the layer: related surveys, benchmark suites, and diagnostic studies.

<a id="taxonomy-overview"></a>
## 🗺️ Overview: A Taxonomy of Parallel Decoding -->

<!-- Layers 1–6 contain 16 subclasses, while Layer 7 attaches evaluation resources directly to the layer.  -->
Within each section, papers run from older to newer; click a paper to jump to its full row.

<table>
<thead>
<tr><th>Layer</th><th>Subclass</th><th>Papers</th></tr>
</thead>
<tbody>
<tr>
<td rowspan="5"><b>1. Token-Level Parallel Decoding</b></td>
<td><a href="#subclass-1-1">1.1 Drafting with the Draft Model</a></td>
<td><a href="#Blockwise_NeurIPS_18">Blockwise (NIPS'18)</a> · <a href="#SpecDec_EMNLP_23">SpecDec (Findings-EMNLP'23)</a> · <a href="#Leviathan_ICML_23">Leviathan (ICML'23)</a> · <a href="#Chen_arXiv_23">Chen (arXiv'23)</a> · <a href="#BiLD_NeurIPS_23">BiLD (NeurIPS'23)</a> · <a href="#DistillSpec_ICLR_24">DistillSpec (ICLR'24)</a> · <a href="#Online_SD_ICML_24">OSD (ICML'24)</a> · <a href="#Decoding_SD_NAACL_25">Decoding SD (NAACL'25)</a> · <a href="#Multilingual_Drafters_EMNLP_24">Multilingual Drafters (EMNLP'24)</a> · <a href="#HASS_ICLR_25">HASS (ICLR'25)</a> · <a href="#Temp-Centric_KD_EMNLP_24">Temp-Centric KD (Findings-EMNLP'24)</a> · <a href="#ParallelSpec_arXiv_24">ParallelSpec (arXiv'24)</a> · <a href="#CTC_Drafter_NeurIPS_24">CTC-drafter (NeurIPS'24)</a> · <a href="#HeteroVocab_ICML_25">Heterogeneous Vocabularies (ICML'25)</a> · <a href="#Collaborative_Decoding_ICML_25">CoS (ICML'25)</a> · <a href="#GRIFFIN_NeurIPS_25">GRIFFIN (NeurIPS'25)</a> · <a href="#CORAL_ACL_25">CORAL (ACL'25)</a> · <a href="#FR-Spec_ACL_25">FR-Spec (ACL'25)</a> · <a href="#Domain_Drafters_ICLR_25">Domain Drafters (SCOPE @ ICLR'25)</a> · <a href="#Auto-Task_SD_ICPP_25">TaskSpec (arXiv'25)</a> · <a href="#Mamba_Drafters_EMNLP_25">Mamba Drafters (Findings-EMNLP'25)</a> · <a href="#OmniDraft_NeurIPS_25">OmniDraft (NeurIPS'25)</a> · <a href="#SPECTRA_ACL_25">SPECTRA (ACL'25)</a> · <a href="#RepSpec_ICLR_26">RepSpec (ICLR'26)</a> · <a href="#AdaSPEC_NeurIPS_25">AdaSPEC (NeurIPS'25)</a> · <a href="#Flatter_Tokens_ICLR_26">Flatter Tokens (ICLR'26)</a> · <a href="#OnlineSPEC_ICML_26">OnlineSPEC (ICML'26)</a></td>
</tr>
<tr>
<td><a href="#subclass-1-2">1.2 Drafting within the Target Model</a></td>
<td><a href="#Draft_Verify_ACL_24">Draft \& Verify (ACL'24)</a> · <a href="#PaSS_NeurIPS_23">PaSS (NeurIPSW'23)</a> · <a href="#Medusa_ICML_24">Medusa (ICML'24)</a> · <a href="#EAGLE_ICML_24">EAGLE (ICML'24)</a> · <a href="#GliDe_with_a_CaPE_ICML_24">GliDe with a CaPE (ICML'24)</a> · <a href="#Hydra_COLM_24">Hydra (COLM'24)</a> · <a href="#ReDrafter_arXiv_24">ReDrafter (arXiv'24)</a> · <a href="#LayerSkip_ACL_24">LayerSkip (ACL'24)</a> · <a href="#Kangaroo_NeurIPS_24">Kangaroo (NeurIPS'24)</a> · <a href="#MTP_ICML_24">MTP (ICML'24)</a> · <a href="#PPD_EMNLP_25">PPD (Findings-EMNLP'25)</a> · <a href="#S3D_arXiv_24">S3D (arXiv'24)</a> · <a href="#EESD_ACL_24">EESD (Findings-ACL'24)</a> · <a href="#EAGLE-2_EMNLP_24">EAGLE-2 (EMNLP'24)</a> · <a href="#Amphista_NAACL_25">Amphista (NAACL'25)</a> · <a href="#Clover-2_arXiv_24">Clover-2 (arXiv'24)</a> · <a href="#KOALA_CSCWD_25">KOALA (CSCWD'25)</a> · <a href="#Draft-on-the-Fly_EMNLP_24">Draft-on-the-Fly (Findings-EMNLP'24)</a> · <a href="#SWIFT_ICLR_25">SWIFT (ICLR'25)</a> · <a href="#Mixture_of_Attentions_ICLR_25">Mixture of Attentions (ICLR'25)</a> · <a href="#DeepSeek-V3_arXiv_24">DeepSeek-V3 (arXiv'24)</a> · <a href="#On_MTP_ICLR_25">On MTP (ICLRW'25)</a> · <a href="#EAGLE-3_NeurIPS_25">EAGLE-3 (NeurIPS'25)</a> · <a href="#Gumiho_ICML_25">Gumiho (ICML'25)</a> · <a href="#DEL_COLM_25">DEL (COLM'25)</a> · <a href="#CLaSp_ACL_25">CLaSp (ACL'25)</a> · <a href="#L-MTP_NeurIPS_25">L-MTP (NeurIPS'25)</a> · <a href="#Beagle_arXiv_25">Beagle (arXiv'25)</a> · <a href="#AdaDecode_ICML_25">AdaDecode (ICML'25)</a> · <a href="#Your_LLM_Knows_Future_arXiv_25">Your LLM Knows the Future (arXiv'25)</a> · <a href="#CAS-Spec_NeurIPS_25">CAS-Spec (NeurIPS'25)</a> · <a href="#FractalLLM_EMNLP_25">FractalLLM (Findings-EMNLP'25)</a> · <a href="#Speculative_Streaming_EMNLP_25">Speculative Streaming (EMNLP'25)</a> · <a href="#SparseSpec_arXiv_25">SparseSpec (MLSys'26)</a> · <a href="#mtp-lm_arXiv_26">mtp-lm (ICMLW'26)</a> · <a href="#ESP_ICML_26">ESP (ICML'26)</a></td>
</tr>
<tr>
<td><a href="#subclass-1-3">1.3 Retrieval-Based and Non-Parametric Drafting</a></td>
<td><a href="#Santilli_ACL_23">Santilli (ACL'23)</a> · <a href="#REST_NAACL_24">REST (NAACL'24)</a> · <a href="#Lookahead_Decoding_ICML_24">Lookahead Decoding (ICML'24)</a> · <a href="#Ouroboros_EMNLP_24">Ouroboros (EMNLP'24)</a> · <a href="#CLLMs_ICML_24">CLLMs (ICML'24)</a> · <a href="#NEST_NeurIPS_24">NEST (NeurIPS'24)</a> · <a href="#MSN_EMNLP_24">MSN (EMNLP'24)</a> · <a href="#Token_Recycling_ACL_25">Token Recycling (ACL'25)</a> · <a href="#SuffixDecoding_NeurIPS_25">SuffixDecoding (NeurIPS'25)</a> · <a href="#SAM_Decoding_ACL_25">SAM Decoding (ACL'25)</a> · <a href="#DReSD_ACL_25">DReSD (Findings-ACL'25)</a> · <a href="#RAPID_ICML_25">RAPID (ICML'25)</a> · <a href="#Temporal-Locality_Drafting_NAACL_25">Hierarchy Drafting (Findings-NAACL'25)</a> · <a href="#RASD_ACL_25">RASD (Findings-ACL'25)</a> · <a href="#N-Gram_Trie_SD_EMNLP_25">N-Gram Trie (EMNLP'25)</a> · <a href="#Cacheback_EMNLP_25">Cacheback (EMNLP'25)</a> · <a href="#Jacobi_Forcing_arXiv_25">Jacobi Forcing (ICML'26)</a></td>
</tr>
<tr>
<td><a href="#subclass-1-4">1.4 Draft Structure and Sampling Strategies</a></td>
<td><a href="#SpecInfer_ASPLOS_24">SpecInfer (ASPLOS'24)</a> · <a href="#SpecTr_NeurIPS_23">SpecTr (NeurIPS'23)</a> · <a href="#MCSD_NLPCC_25">MCSD (NLPCC'25)</a> · <a href="#Sequoia_NeurIPS_24">Sequoia (NeurIPS'24)</a> · <a href="#Tree_Monte_Carlo_ICML_24">Tree Monte Carlo (ICML'24)</a> · <a href="#Block_Verification_ICLR_25">Block Verification (ICLR'25)</a> · <a href="#SLiM_NAACL_24">SLiM (Findings-NAACL'24)</a> · <a href="#Dynamic_Spec_Lookahead_arXiv_24">DISCO (NeurIPSW'24)</a> · <a href="#OPT-Tree_TACL_25">OPT-Tree (TACL'25)</a> · <a href="#Graph-Structured_SD_ACL_24">GSD (Findings-ACL'24)</a> · <a href="#DDD_arXiv_24">DDD (arXiv'24)</a> · <a href="#Multi-Draft_Canonical_ICLR_25">Multi-Draft Canonical (ICLR'25)</a> · <a href="#Theory_of_SD_NeurIPS_24">Theory of SD (NeurIPS'24)</a> · <a href="#SpecHub_EMNLP_24">SpecHub (EMNLP'24)</a> · <a href="#SVIP_EMNLP_25">SVIP (EMNLP'25)</a> · <a href="#AdaEAGLE_arXiv_24">AdaEAGLE (arXiv'24)</a> · <a href="#Judge_Decoding_ICLR_25">Judge Decoding (ICLR'25)</a> · <a href="#TETRIS_ACL_25">TETRIS (ACL'25)</a> · <a href="#Optimal_Multi-Draft_ICLR_25">Optimal Multi-Draft (ICLR'25)</a> · <a href="#Fuzzy_SD_ACL_25">Fuzzy SD (Findings-ACL'25)</a> · <a href="#Hierarchical_Dynamic_Window_NAACL_25">HSDDW (Findings-NAACL'25)</a> · <a href="#Exponential_Races_ACL_25">ERSS (Findings-ACL'25)</a> · <a href="#AASD_EMNLP_25">AASD (EMNLP'25)</a> · <a href="#Traversal_Verification_NeurIPS_25">Traversal Verification (NeurIPS'25)</a> · <a href="#STree_NeurIPS_25">STree (NeurIPS'25)</a> · <a href="#BanditSpec_ICML_25">BanditSpec (ICML'25)</a> · <a href="#HeteroSpec_arXiv_25">HeteroSpec (ACL'26)</a> · <a href="#List-Level_Coupling_NeurIPS_25">List-Level Coupling (NeurIPS'25)</a> · <a href="#SpecBranch_ICLR_26">SpecBranch (ICLR'26)</a> · <a href="#Drop-In_Adaptation_ACL_25">Drop-In Adaptation (ACL'25)</a> · <a href="#Pruned_Candidate_Tree_ACL_25">Pruned Candidate Tree (ACL'25)</a> · <a href="#Group_Tree_Opt_ICLR_26">GTO (ICLR'26)</a> · <a href="#polybasic_SD_ICML_25">polybasic SD (ICML'25)</a> · <a href="#Not-a-Bandit_ICLR_26">Not-a-Bandit (ICLR'26)</a> · <a href="#Global_Resolution_ICLR_26">Global Resolution (ICLR'26)</a> · <a href="#Loosely_SD_ICLR_26">FLy (ICLR'26)</a> · <a href="#Lossless_Hierarchical_SD_ICLR_26">Lossless Hierarchical SD (ICLR'26)</a> · <a href="#Learning_to_Draft_ICLR_26">LTD (ICLR'26)</a> · <a href="#Spec-Spec_Decoding_ICLR_26">SSD (ICLR'26)</a> · <a href="#Cactus_ICLR_26">Cactus (ICLR'26)</a> · <a href="#SpecBlock_arXiv_26">SpecBlock (arXiv'26)</a> · <a href="#JetSpec_arXiv_26">JetSpec (arXiv'26)</a></td>
</tr>
<tr>
<td><a href="#subclass-1-5">1.5 Cross-Technique Integration</a></td>
<td><a href="#Spec_Contrastive_ACL_24">SCD (ACL'24)</a> · <a href="#Watermark_SD_NeurIPS_24">Watermark SD (NeurIPS'24)</a> · <a href="#Constrained_Spec_Lookaheads_NAACL_25">CDSL (NAACL'25)</a> · <a href="#DREAM_NeurIPS_25">DREAM (NeurIPS'25)</a> · <a href="#MASSV_EMNLP_25">MASSV (Findings-EMNLP'25)</a> · <a href="#Spec-VLA_EMNLP_25">Spec-VLA (EMNLP'25)</a> · <a href="#SpecVLM_EMNLP_25">SpecVLM (EMNLP'25)</a> · <a href="#Reward-Shifted_SS_EMNLP_25">Reward-Shifted SS (EMNLP'25)</a> · <a href="#Safety-Aware_SD_EMNLP_25">Safety-Aware SD (EMNLP'25)</a> · <a href="#ViSpec_NeurIPS_25">ViSpec (NeurIPS'25)</a> · <a href="#SPEC-RL_arXiv_25">SPEC-RL (arXiv'25)</a> · <a href="#FastGRPO_arXiv_25">FastGRPO (ICLR'26)</a> · <a href="#ReSpec_MLSys_26">ReSpec (MLSys'26)</a> · <a href="#Beat-the-Long-Tail_MLSys_26">DAS (MLSys'26)</a> · <a href="#SpecActor_arXiv_25">SpecActor (arXiv'25)</a> · <a href="#TLT_ASPLOS_26">TLT (ASPLOS'26)</a> · <a href="#RLHFSpec_arXiv_25">RLHFSpec (arXiv'25)</a> · <a href="#SRT_NeurIPS_25_ER">SRT (NeurIPSW'25)</a> · <a href="#Watermark_SD-2_ICLR_26">Watermark SD-2 (ICLR'26)</a></td>
</tr>
<tr>
<td rowspan="3"><b>2. Sequence-Level Parallel Decoding</b></td>
<td><a href="#subclass-2-1">2.1 Multi-Sequence Parallel Generation</a></td>
<td><a href="#FastCoT_arXiv_23">FastCoT (arXiv'23)</a> · <a href="#SEED_COLING_25">SEED (COLING'25)</a> · <a href="#Speculative_RAG_ICLR_25">Speculative RAG (ICLR'25)</a> · <a href="#RSD_ICML_25">RSD (ICML'25)</a> · <a href="#Multi-Sample_SD_EMNLP_25">Multi-Sample SD (Findings-EMNLP'25)</a> · <a href="#SpecReason_NeurIPS_25">SpecReason (NeurIPS'25)</a> · <a href="#Speculative_Thinking_arXiv_25">Speculative Thinking (COLM'25)</a> · <a href="#SCoT_arXiv_25">SCoT (Findings-ACL'26)</a> · <a href="#SpecSearch_ICML_25">SpecSearch (ICML'25)</a> · <a href="#R2R_NeurIPS_25">R2R (NeurIPS'25)</a> · <a href="#Lookahead_Reasoning_NeurIPS_25">Lookahead Reasoning (NeurIPS'25)</a> · <a href="#STAND_EMNLP_25">STAND (EMNLP'25)</a> · <a href="#SpecExit_arXiv_25">SpecExit (ICML'26)</a> · <a href="#SpecCoT_EMNLP_25">SpecCoT (Findings-EMNLP'25)</a> · <a href="#SemanticSpec_arXiv_26">SemanticSpec (arXiv'26)</a> · <a href="#SpecGuard_arXiv_26">SpecGuard (Findings-ACL'26)</a></td>
</tr>
<tr>
<td><a href="#subclass-2-2">2.2 Intra-Sequence Parallel Generation</a></td>
<td><a href="#Skeleton-of-Thought_ICLR_24">Skeleton-of-Thought (ICLR'24)</a> · <a href="#Graph_of_Thoughts_AAAI_24">Graph of Thoughts (AAAI'24)</a> · <a href="#Plato_arXiv_24">Plato (COLM'25)</a> · <a href="#PASTA_ICML_25">PASTA (ICML'25)</a> · <a href="#Parallel-Decode-in-One-Seq_EMNLP_25">Parallel Decode in One Seq (EMNLP'25)</a> · <a href="#APR_COLM_25">APR (COLM'25)</a> · <a href="#Multiverse_NeurIPS_25">Multiverse (NeurIPS'25)</a> · <a href="#Sprint_arXiv_25">SPRINT (NeurIPS'25)</a> · <a href="#PCCoT_EMNLP_25">PCCoT (EMNLP'25)</a> · <a href="#Parallel-Think-Seq-Answer_arXiv_25">Parallel Think, Sequential Answer (arXiv'25)</a> · <a href="#Parallel_Loop_Transformer_arXiv_25">Parallel Loop Transformer (arXiv'25)</a></td>
</tr>
<tr>
<td><a href="#subclass-2-3">2.3 Path Exploration and Test-Time Scaling</a></td>
<td><a href="#More_Agents_TMLR_24-sequence">Agent Forest (TMLR'24)</a> · <a href="#Speculative_Rejection_NeurIPS_24">Speculative Rejection (NeurIPS'24)</a> · <a href="#DPTS_ACL_25">DPTS (ACL'25)</a> · <a href="#Group_Think_arXiv_25">Group Think (arXiv'25)</a> · <a href="#Adaptive_Termination_arXiv_25">SEAT (arXiv'25)</a> · <a href="#A2R_arXiv_25">A2R (arXiv'25)</a> · <a href="#Parallel-R1_ICLR_26">Parallel-R1 (ICLR'26)</a> · <a href="#ATTS_ICLR_26">ATTS (ICLR'26)</a> · <a href="#Parallel_TTS_for_Latent_ACL_26">Parallel Test-Time Scaling (ACL'26)</a> · <a href="#DeepPrune_ACL_26">DeepPrune (Findings-ACL'26)</a> · <a href="#DTS_NeurIPS_25_ER">DTS (ICML'26)</a> · <a href="#Parallel-Probe_ICML_26">Parallel-Probe (ICML'26)</a></td>
</tr>
<tr>
<td rowspan="2"><b>3. Task-Level Parallel Decoding</b></td>
<td><a href="#subclass-3-1">3.1 Multi-Agent Parallel Collaboration</a></td>
<td><a href="#AutoGen_COLM_24">AutoGen (COLM'24)</a> · <a href="#More_Agents_TMLR_24">More Agents (TMLR'24)</a> · <a href="#Mixture-of-Agents_ICLR_25">Mixture-of-Agents (ICLR'25)</a> · <a href="#M1-Parallel_ICML_25_MAS">M1-Parallel (ICMLW'25)</a></td>
</tr>
<tr>
<td><a href="#subclass-3-2">3.2 Parallel and Speculative Agent Execution</a></td>
<td><a href="#LLMCompiler_ICML_24">LLMCompiler (ICML'24)</a> · <a href="#LLM-Tool_Compiler_arXiv_24">LLM-Tool Compiler (arXiv'24)</a> · <a href="#Async_Tool_Usage_arXiv_24">Async Tool Usage (arXiv'24)</a> · <a href="#AsyncLM_arXiv_24">AsyncLM (arXiv'24)</a> · <a href="#DSP_ICLR_26">DSP (ICLR'26)</a> · <a href="#Speculative_Actions_ICLR_26">Speculative Actions (ICLR'26)</a> · <a href="#Spec_Tool_Calls_arXiv_25">Spec Tool Calls (arXiv'25)</a> · <a href="#W_D_ICLR_26_AIWILD">W\&D (ICLRW'26)</a> · <a href="#SimpleTool_arXiv_26">RealtimeTool (ICML'26)</a> · <a href="#PASTE_arXiv_26">PASTE (arXiv'26)</a></td>
</tr>
<tr>
<td rowspan="2"><b>4. Architecture-Level Parallel Decoding</b></td>
<td><a href="#subclass-4-1">4.1 Model Architecture and Training</a></td>
<td><a href="#NAT_ICLR_18">NAT (ICLR'18)</a> · <a href="#IterRefine_EMNLP_18">Iterative Refinement (EMNLP'18)</a> · <a href="#Discrete_Latent_ICML_18">Latent Transformer (ICML'18)</a> · <a href="#Mask-Predict_EMNLP_19">Mask-Predict (EMNLP'19)</a> · <a href="#D3PM_NeurIPS_21">D3PM (NeurIPS'21)</a> · <a href="#DiffusionLM_NeurIPS_22">Diffusion-LM (NeurIPS'22)</a> · <a href="#SEDD_arXiv_23">SEDD (ICML'24)</a> · <a href="#RADD_ICLR_25">RADD (ICLR'25)</a> · <a href="#MD4_NeurIPS_24">MD4 (NeurIPS'24)</a> · <a href="#MDLM_NeurIPS_24">MDLM (NeurIPS'24)</a> · <a href="#DiffuLLaMA_ICLR_25">DiffuLLaMA (ICLR'25)</a> · <a href="#SDTT_ICLR_25">SDTT (ICLR'25)</a> · <a href="#LLaDA_NeurIPS_25">LLaDA (NeurIPS'25)</a> · <a href="#Block_Diffusion_ICLR_25">Block Diffusion (ICLR'25)</a> · <a href="#Duo_ICML_25">Duo (ICML'25)</a> · <a href="#Dream_7B_arXiv_25">Dream 7B (arXiv'25)</a> · <a href="#SDLM_arXiv_25">SDLM (arXiv'25)</a> · <a href="#Fast-dLLM_v2_ICLR_26">Fast-dLLM v2 (ICLR'26)</a> · <a href="#RND1_blog">RND1 (misc'25)</a> · <a href="#E2D2_NeurIPS_25">E2D2 (NeurIPS'25)</a> · <a href="#SDAR_arXiv_25">SDAR (Findings-ACL'26)</a> · <a href="#NBDiff_arXiv_25">NBDiff (arXiv'25)</a> · <a href="#Efficient-DLM_arXiv_25">Efficient-DLM (ICML'26)</a> · <a href="#LLaDA2.0_arXiv_25">LLaDA2.0 (arXiv'25)</a> · <a href="#Diffusion-in-Diffusion_arXiv_26">Diffusion In Diffusion (arXiv'26)</a> · <a href="#MBD-LMs_arXiv_26">MBD-LMs (arXiv'26)</a></td>
</tr>
<tr>
<td><a href="#subclass-4-2">4.2 Decoding and Sampling Algorithms</a></td>
<td><a href="#EB-Sampler_NeurIPS_25">EB-Sampler (NeurIPS'25)</a> · <a href="#Fast-dLLM_ICLR_26">Fast-dLLM (ICLR'26)</a> · <a href="#SlowFast_Sampling_ICLR_26">SlowFast Sampling (ICLR'26)</a> · <a href="#WINO_ICLR_26">WINO (ICLR'26)</a> · <a href="#Prophet_ICLR_26">Prophet (ICLR'26)</a> · <a href="#RWS_EMNLP_25">RWS (EMNLP'25)</a> · <a href="#LSD_NeurIPS_25">LSD (NeurIPS'25)</a> · <a href="#ADJUST_arXiv_25">ADJUST (arXiv'25)</a> · <a href="#Learn2PD_ICLR_26">Learn2PD (ICLR'26)</a> · <a href="#dParallel_ICLR_26">dParallel (ICLR'26)</a> · <a href="#FreeDave_arXiv_25">FreeDave (NeurIPSW'25)</a> · <a href="#LocalLeap_arXiv_25">LocalLeap (arXiv'25)</a> · <a href="#Saber_arXiv_25">Saber (ACL'26)</a> · <a href="#LoPA_arXiv_25">LoPA (arXiv'25)</a> · <a href="#SchED_arXiv_25">SchED (Findings-ACL'26)</a> · <a href="#CadLLM_arXiv_25">CadLLM (Findings-ACL'26)</a> · <a href="#Learning_Unmasking_Policies_ICML_26">Learning Unmasking Policies (ICML'26)</a> · <a href="#Order-Token_Search_arXiv_26">Order-Token Search (arXiv'26)</a> · <a href="#d3LLM_ICML_26">d3LLM (ICML'26)</a> · <a href="#RDD_arXiv_26">RDD (arXiv'26)</a> · <a href="#ReMix_CVPR_26">ReMix (CVPR'26)</a> · <a href="#Info-Gain_Sampler_ICML_26">Info-Gain Sampler (ICML'26)</a> · <a href="#DOS_arXiv_26">DOS (Findings-ACL'26)</a> · <a href="#Confidence-Based_Decoding_arXiv_26">Confidence-Based Decoding (arXiv'26)</a></td>
</tr>
<tr>
<td rowspan="2"><b>5. System and Infrastructure</b></td>
<td><a href="#subclass-5-1">5.1 Serving and Scheduling</a></td>
<td><a href="#TriForce_COLM_24">TriForce (COLM'24)</a> · <a href="#BASS_ACL_24">BASS (Findings-ACL'24)</a> · <a href="#DSI_ICLR_25">DSI (ICLR'25)</a> · <a href="#EMS-SD_NAACL_25">EMS-SD (NAACL'25)</a> · <a href="#SpecExec_NeurIPS_24">SpecExec (NeurIPS'24)</a> · <a href="#GPU-Opt_SS_EMNLP_24">Optimized Speculative Sampling (EMNLP'24)</a> · <a href="#MagicDec_ICLR_25">MagicDec (ICLR'25)</a> · <a href="#PEARL_ICLR_25">PEARL (ICLR'25)</a> · <a href="#Dovetail_EMNLP_25">Dovetail (EMNLP'25)</a> · <a href="#Attention-Level_Speculation_ICML_25">ALSpec (ICML'25)</a> · <a href="#EasySpec_NeurIPS_25">EasySpec (NeurIPS'25)</a> · <a href="#AdaSpec_SoCC_25">AdaSpec (SoCC'25)</a> · <a href="#Substitute_SD_NeurIPS_25">SubSpec (NeurIPS'25)</a> · <a href="#Mirror-SD_arXiv_25">Mirror-SD (arXiv'25)</a> · <a href="#dInfer">dInfer (arXiv'25)</a> · <a href="#dLLM-Serve">dLLM-Serve (arXiv'25)</a></td>
</tr>
<tr>
<td><a href="#subclass-5-2">5.2 Algorithm-Infrastructure Co-Design</a></td>
<td><a href="#DeFT_ICLR_25">DeFT (ICLR'25)</a> · <a href="#QSpec_EMNLP_25">QSpec (EMNLP'25)</a> · <a href="#QuantSpec_ICML_25">QuantSpec (ICML'25)</a> · <a href="#Speculative_Prefill_ICML_25">SpecPrefill (ICML'25)</a> · <a href="#ML-SpecQD_arXiv_25">ML-SpecQD (arXiv'25)</a> · <a href="#SpeCache_ICML_25">SpeCache (ICML'25)</a> · <a href="#MoESD_NeurIPS_25">MoESD (NeurIPS'25)</a> · <a href="#Utility-Driven_MoE-SD_arXiv_25">Cascade (arXiv'25)</a> · <a href="#AsyncSpade_arXiv_25">AsyncSpade (ICML'26)</a> · <a href="#Yggdrasil_NeurIPS_25">Yggdrasil (NeurIPS'25)</a></td>
</tr>
<tr>
<td rowspan="2"><b>6. Hybrid Methods</b></td>
<td><a href="#subclass-6-1">6.1 Non-AR Drafter for AR Targets</a></td>
<td><a href="#SpecDiff_NAACL_25">SpecDiff (NAACL'25)</a> · <a href="#DiffuSpec_arXiv_25">DiffuSpec (Findings-ACL'26)</a> · <a href="#SpecDiff-2_arXiv_25">SpecDiff-2 (MLSys'26)</a> · <a href="#DEER_arXiv_25">DEER (arXiv'25)</a> · <a href="#DART_arXiv_26">DART (arXiv'26)</a> · <a href="#DFlash_ICML_26">DFlash (ICML'26)</a> · <a href="#JetSpec_arXiv_26-hybrid">JetSpec (arXiv'26)</a> · <a href="#DSpark_26">DSpark (arXiv'26)</a> · <a href="#AdaFlash_arXiv_26">AdaFlash (arXiv'26)</a></td>
</tr>
<tr>
<td><a href="#subclass-6-2">6.2 Speculation for Other Generative Paradigms</a></td>
<td><a href="#Cascade_SD_NeurIPS_24">CS Drafting (NeurIPS'24)</a> · <a href="#Faster_Cascades_ICLR_25">Faster Cascades (ICLR'25)</a> · <a href="#Spec_Jacobi_T2I_ICLR_25">SJD (ICLR'25)</a> · <a href="#LANTERN_ICLR_25">LANTERN (ICLR'25)</a> · <a href="#Accel_Diffusion_via_SS_ICML_25">Accelerated Diffusion via Speculative Sampling (ICML'25)</a> · <a href="#Diffusion_Exchangeable_ICML_25">ASD (ICML'25)</a> · <a href="#APD_NeurIPS_25">APD (NeurIPS'25)</a> · <a href="#Spiffy_arXiv_25">Spiffy (ICMLW'26)</a> · <a href="#SSD_arXiv_25">SSD (arXiv'25)</a> · <a href="#ODB_arXiv_25">ODB-dLLM (DAC'26)</a></td>
</tr>
<tr>
<td><b>7. Benchmarks and Evaluation</b></td>
<td> - </td>
<td><a href="#SD_Survey_ACL_24">Speculative Decoding Survey (Findings-ACL'24)</a> · <a href="#Parallel_Text_Gen_Survey_arXiv_25">Parallel Text Generation Survey (arXiv'25)</a> · <a href="#Parallel_Reasoning_Survey_arXiv_25">Parallel Reasoning Survey (arXiv'25)</a> · <a href="#Spec_Bench_website_24">Spec-Bench (misc'24)</a> · <a href="#TTS_Benchmark_ICLR_26">TTS Benchmark (ICLR'26)</a> · <a href="#dLLM_Efficiency_Eval_arXiv_25">dLLM Efficiency Evaluation (arXiv'25)</a> · <a href="#ParallelBench_ICLR_26">ParallelBench (ICLR'26)</a></td>
</tr>
</tbody>
</table>

<a id="class-1"></a>
## 📙 1. Token-Level Parallel Decoding

Token-level methods shorten autoregressive generation by proposing, organizing, or verifying multiple token candidates or future positions in parallel.

<a id="subclass-1-1"></a>
### 1.1 Drafting with the Draft Model

*An independent draft model proposes candidate tokens that the target LLM verifies; this includes foundational speculative-decoding frameworks, drafter training and distillation, and cross-architecture draft adaptation.*

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2018.11|<a id="Blockwise_NeurIPS_18"></a>[**[Blockwise]** Blockwise Parallel Decoding for Deep Autoregressive Models](https://arxiv.org/abs/1811.03115)|Mitchell Stern, Noam Shazeer, Jakob Uszkoreit (@UC Berkeley \& Google Brain)|[[code]](https://github.com/tensorflow/tensor2tensor)![](https://img.shields.io/github/stars/tensorflow/tensor2tensor?style=social)|NIPS'18|
|2022.03|<a id="SpecDec_EMNLP_23"></a>[**[SpecDec]** Speculative Decoding: Exploiting Speculative Execution for Accelerating Seq2seq Generation](https://arxiv.org/abs/2203.16487)|Heming Xia, Tao Ge et al. (@MSRA \& PKU)|[[code]](https://github.com/hemingkx/SpecDec)![](https://img.shields.io/github/stars/hemingkx/SpecDec?style=social)|Findings-EMNLP'23|
|2022.11|<a id="Leviathan_ICML_23"></a>[**Fast Inference from Transformers via Speculative Decoding**](https://arxiv.org/abs/2211.17192)|Yaniv Leviathan, Matan Kalman, Yossi Matias (@Google Research)|[[code (unofficial)]](https://github.com/feifeibear/LLMSpeculativeSampling)![](https://img.shields.io/github/stars/feifeibear/LLMSpeculativeSampling?style=social)|ICML'23|
|2023.02|<a id="Chen_arXiv_23"></a>[**Accelerating Large Language Model Decoding with Speculative Sampling**](https://arxiv.org/abs/2302.01318)|Charlie Chen et al. (@DeepMind)|[[code (unofficial)]](https://github.com/feifeibear/LLMSpeculativeSampling)![](https://img.shields.io/github/stars/feifeibear/LLMSpeculativeSampling?style=social)|arXiv'23|
|2023.02|<a id="BiLD_NeurIPS_23"></a>[**[BiLD]** Speculative Decoding with Big Little Decoder](https://arxiv.org/abs/2302.07863)|Sehoon Kim et al. (@UC Berkeley \& ICSI \& LBNL)|[[code]](https://github.com/kssteven418/BigLittleDecoder)![](https://img.shields.io/github/stars/kssteven418/BigLittleDecoder?style=social)|NeurIPS'23|
|2023.10|<a id="DistillSpec_ICLR_24"></a>[**[DistillSpec]** Improving Speculative Decoding via Knowledge Distillation](https://arxiv.org/abs/2310.08461)|Yongchao Zhou et al. (@Google \& U Toronto \& Princeton \& Mila)| |ICLR'24|
|2023.10|<a id="Online_SD_ICML_24"></a>[**[OSD]** Online Speculative Decoding](https://arxiv.org/abs/2310.07177)|Xiaoxuan Liu et al. (@UC Berkeley \& UCSD \& Google \& SJTU)|[[code]](https://github.com/LiuXiaoxuanPKU/OSD)![](https://img.shields.io/github/stars/LiuXiaoxuanPKU/OSD?style=social)|ICML'24|
|2024.02|<a id="Decoding_SD_NAACL_25"></a>[**Decoding Speculative Decoding**](https://arxiv.org/abs/2402.01528)|Minghao Yan et al. (@UW-Madison)|[[code]](https://github.com/uw-mad-dash/decoding-speculative-decoding)![](https://img.shields.io/github/stars/uw-mad-dash/decoding-speculative-decoding?style=social)|NAACL'25|
|2024.06|<a id="Multilingual_Drafters_EMNLP_24"></a>[**Towards Fast Multilingual LLM Inference: Speculative Decoding and Specialized Drafters**](https://arxiv.org/abs/2406.16758)|Euiin Yi, Taehyeon Kim et al. (@KAIST \& KT)|[[code]](https://github.com/Kthyeon/Multilingual-SpecBench)![](https://img.shields.io/github/stars/Kthyeon/Multilingual-SpecBench?style=social)|EMNLP'24|
|2024.08|<a id="HASS_ICLR_25"></a>[**[HASS]** Learning Harmonized Representations for Speculative Sampling](https://arxiv.org/abs/2408.15766)|Lefan Zhang et al. (@Xiaohongshu)|[[code]](https://github.com/HArmonizedSS/HASS)![](https://img.shields.io/github/stars/HArmonizedSS/HASS?style=social)|ICLR'25|
|2024.10|<a id="Temp-Centric_KD_EMNLP_24"></a>[**Temperature-Centric Investigation of Speculative Decoding with Knowledge Distillation**](https://arxiv.org/abs/2410.10141)|Siru Ouyang et al. (@UIUC \& Microsoft)|[[code]](https://github.com/ozyyshr/TempSpec)![](https://img.shields.io/github/stars/ozyyshr/TempSpec?style=social)|Findings-EMNLP'24|
|2024.10|<a id="ParallelSpec_arXiv_24"></a>[**[ParallelSpec]** Parallel Drafter for Efficient Speculative Decoding](https://arxiv.org/abs/2410.05589)|Zilin Xiao et al. (@Rice \& Tencent AI Lab \& UIUC)| |arXiv'24|
|2024.12|<a id="CTC_Drafter_NeurIPS_24"></a>[**[CTC-drafter]** Speculative Decoding with CTC-based Draft Model for LLM Inference Acceleration](https://arxiv.org/abs/2412.00061)|Zhuofan Wen et al. (@Institute of Computing Technology, CAS \& UCAS)|[[code]](https://github.com/ZhuofanWen/CTC-drafter)![](https://img.shields.io/github/stars/ZhuofanWen/CTC-drafter?style=social)|NeurIPS'24|
|2025.02|<a id="HeteroVocab_ICML_25"></a>[**Accelerating LLM Inference with Lossless Speculative Decoding Algorithms for Heterogeneous Vocabularies**](https://arxiv.org/abs/2502.05202)|Nadav Timor et al. (@Weizmann \& Intel \& d-Matrix)|[[code]](https://github.com/keyboardAnt/hf-bench)![](https://img.shields.io/github/stars/keyboardAnt/hf-bench?style=social)|ICML'25|
|2025.02|<a id="Collaborative_Decoding_ICML_25"></a>[**[CoS]** Fast Large Language Model Collaborative Decoding via Speculation](https://arxiv.org/abs/2502.01662)|Jiale Fu et al. (@SEU)|[[code]](https://github.com/Kamichanw/CoS)![](https://img.shields.io/github/stars/Kamichanw/CoS?style=social)|ICML'25|
|2025.02|<a id="GRIFFIN_NeurIPS_25"></a>[**[GRIFFIN]** Effective Token Alignment for Faster Speculative Decoding](https://arxiv.org/abs/2502.11018)|Shijing Hu et al. (@Fudan \& NUS \& SMU)|[[code]](https://github.com/hsj576/GRIFFIN)![](https://img.shields.io/github/stars/hsj576/GRIFFIN?style=social)|NeurIPS'25|
|2025.02|<a id="CORAL_ACL_25"></a>[**[CORAL]** Learning Consistent Representations across Multi-step Training with Lighter Speculative Drafter](https://arxiv.org/abs/2502.16880)|Yepeng Weng et al. (@Lenovo Research)| |ACL'25|
|2025.02|<a id="FR-Spec_ACL_25"></a>[**[FR-Spec]** Accelerating Large-Vocabulary Language Models via Frequency-Ranked Speculative Sampling](https://arxiv.org/abs/2502.14856)|Weilin Zhao, Tengyu Pan et al. (@THU \& HIT \& BUPT \& OpenBMB \& WeChat AI/Tencent)|[[code]](https://github.com/thunlp/FR-Spec)![](https://img.shields.io/github/stars/thunlp/FR-Spec?style=social)|ACL'25|
|2025.03|<a id="Domain_Drafters_ICLR_25"></a>[**Training Domain Draft Models for Speculative Decoding: Best Practices and Insights**](https://arxiv.org/abs/2503.07807)|Fenglu Hong et al. (@SambaNova)| |SCOPE @ ICLR'25|
|2025.05|<a id="Auto-Task_SD_ICPP_25"></a>[**[TaskSpec]** Automatic Task Detection and Heterogeneous LLM Speculative Decoding](https://arxiv.org/abs/2505.08600)|Danying Ge et al. (@BNU)| |arXiv'25|
|2025.06|<a id="Mamba_Drafters_EMNLP_25"></a>[**Mamba Drafters for Speculative Decoding**](https://arxiv.org/abs/2506.01206)|Daewon Choi et al. (@KAIST \& Amazon \& SNU)| |Findings-EMNLP'25|
|2025.07|<a id="OmniDraft_NeurIPS_25"></a>[**[OmniDraft]** A cross-vocabulary, online adaptive drafter for on-device speculative decoding](https://arxiv.org/abs/2507.02659)|Ramchalam Kinattinkara Ramakrishnan, Zhaocong Yuan et al. (@Qualcomm AI Research)| |NeurIPS'25|
|2025.07|<a id="SPECTRA_ACL_25"></a>[**[SPECTRA]** Faster Large Language Model Inference with Optimized Internal and External Speculation](https://aclanthology.org/2025.acl-long.685/)|Nguyen-Khang Le et al. (@JAIST)| |ACL'25|
|2025.09|<a id="RepSpec_ICLR_26"></a>[**[RepSpec]** Structural Re-parameterized Draft Model Training for Speculative Decoding](https://openreview.net/forum?id=bqEi97qzzz)|Feiye Huo et al. (@PKU \& Meituan)| |ICLR'26|
|2025.10|<a id="AdaSPEC_NeurIPS_25"></a>[**[AdaSPEC]** Selective Knowledge Distillation for Efficient Speculative Decoders](https://arxiv.org/abs/2510.19779)|Yuezhou Hu et al. (@UC Berkeley \& Georgia Tech \& THU)|[[code]](https://github.com/yuezhouhu/adaspec)![](https://img.shields.io/github/stars/yuezhouhu/adaspec?style=social)|NeurIPS'25|
|2026.01|<a id="Flatter_Tokens_ICLR_26"></a>[**Flatter Tokens are More Valuable for Speculative Draft Model Training**](https://arxiv.org/abs/2601.18902)|Jiaming Fan et al. (@SEU \& NUIST \& Qiyuan Tech)|[[code]](https://github.com/fjm9933/Flatness)![](https://img.shields.io/github/stars/fjm9933/Flatness?style=social)|ICLR'26|
|2026.03|<a id="OnlineSPEC_ICML_26"></a>[**[OnlineSPEC]** When Drafts Evolve: Speculative Decoding Meets Online Learning](https://arxiv.org/abs/2603.12617)|Yu-Yang Qian et al. (@NJU \& UCSD)|[[code]](https://github.com/ZinYY/OnlineSPEC)![](https://img.shields.io/github/stars/ZinYY/OnlineSPEC?style=social)|ICML'26|

<a id="subclass-1-2"></a>
### 1.2 Drafting within the Target Model

*The target model produces its own drafts through layer skipping, early exit, attached prediction heads, or internal feature prediction, removing the need for a separate full draft model.*

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2023.09|<a id="Draft_Verify_ACL_24"></a>[**[Draft \& Verify]** Lossless Large Language Model Acceleration via Self-Speculative Decoding](https://arxiv.org/abs/2309.08168)|Jun Zhang et al. (@ZJU \& UC Irvine)|[[code]](https://github.com/dilab-zju/self-speculative-decoding)![](https://img.shields.io/github/stars/dilab-zju/self-speculative-decoding?style=social)|ACL'24|
|2023.11|<a id="PaSS_NeurIPS_23"></a>[**[PaSS]** Parallel Speculative Sampling](https://arxiv.org/abs/2311.13581)|Giovanni Monea, Armand Joulin, Edouard Grave (@EPFL \& Apple)| |NeurIPSW'23|
|2024.01|<a id="Medusa_ICML_24"></a>[**[Medusa]** Simple LLM Inference Acceleration Framework with Multiple Decoding Heads](https://arxiv.org/abs/2401.10774)|Tianle Cai et al. (@Princeton \& Together AI \& UIUC \& CMU \& UConn)|[[code]](https://github.com/FasterDecoding/Medusa)![](https://img.shields.io/github/stars/FasterDecoding/Medusa?style=social)|ICML'24|
|2024.01|<a id="EAGLE_ICML_24"></a>[**[EAGLE]** Speculative Sampling Requires Rethinking Feature Uncertainty](https://arxiv.org/abs/2401.15077)|Yuhui Li et al. (@PKU \& Microsoft \& University of Waterloo \& Vector Institute)|[[code]](https://github.com/SafeAILab/EAGLE)![](https://img.shields.io/github/stars/SafeAILab/EAGLE?style=social)|ICML'24|
|2024.02|<a id="GliDe_with_a_CaPE_ICML_24"></a>[**[GliDe with a CaPE]** A Low-Hassle Method to Accelerate Speculative Decoding](https://arxiv.org/abs/2402.02082)|Cunxiao Du et al. (@SMU \& NUS \& PolyU \& HPC-AI Tech \& HIT \& Tencent AI Lab)|[[code]](https://github.com/NonvolatileMemory/GliDe_with_a_CaPE_ICML_24)![](https://img.shields.io/github/stars/NonvolatileMemory/GliDe_with_a_CaPE_ICML_24?style=social)|ICML'24|
|2024.02|<a id="Hydra_COLM_24"></a>[**[Hydra]** Sequentially-Dependent Draft Heads for Medusa Decoding](https://arxiv.org/abs/2402.05109)|Zachary Ankner et al. (@MIT \& MosaicML)|[[code]](https://github.com/zankner/Hydra)![](https://img.shields.io/github/stars/zankner/Hydra?style=social)|COLM'24|
|2024.03|<a id="ReDrafter_arXiv_24"></a>[**[ReDrafter]** Recurrent Drafter for Fast Speculative Decoding in Large Language Models](https://arxiv.org/abs/2403.09919)|Yunfei Cheng et al. (@Apple)|[[code]](https://github.com/apple/ml-recurrent-drafter)![](https://img.shields.io/github/stars/apple/ml-recurrent-drafter?style=social)|arXiv'24|
|2024.04|<a id="LayerSkip_ACL_24"></a>[**[LayerSkip]** Enabling Early Exit Inference and Self-Speculative Decoding](https://arxiv.org/abs/2404.16710)|Mostafa Elhoushi et al. (@Meta \& U Toronto \& CMU \& UW-Madison \& Dana-Farber)|[[code]](https://github.com/facebookresearch/LayerSkip)![](https://img.shields.io/github/stars/facebookresearch/LayerSkip?style=social)|ACL'24|
|2024.04|<a id="Kangaroo_NeurIPS_24"></a>[**[Kangaroo]** Lossless Self-Speculative Decoding for Accelerating LLMs via Double Early Exiting](https://arxiv.org/abs/2404.18911)|Fangcheng Liu et al. (@Huawei \& Consumer Business Group, Huawei)|[[code]](https://github.com/Equationliu/Kangaroo)![](https://img.shields.io/github/stars/Equationliu/Kangaroo?style=social)|NeurIPS'24|
|2024.04|<a id="MTP_ICML_24"></a>[**[MTP]** Better & Faster Large Language Models via Multi-token Prediction](https://arxiv.org/abs/2404.19737)|Fabian Gloeckle et al. (@Meta/FAIR \& École des Ponts ParisTech \& Université Paris-Saclay)| |ICML'24|
|2024.05|<a id="PPD_EMNLP_25"></a>[**[PPD]** Hardware-Aware Parallel Prompt Decoding for Memory-Efficient Acceleration of LLM Inference](https://arxiv.org/abs/2405.18628)|Hao Mark Chen et al. (@Imperial College London \& PolyU \& Samsung AI Center)|[[code]](https://github.com/hmarkc/parallel-prompt-decoding)![](https://img.shields.io/github/stars/hmarkc/parallel-prompt-decoding?style=social)|Findings-EMNLP'25|
|2024.05|<a id="S3D_arXiv_24"></a>[**[S3D]** A Simple and Cost-Effective Self-Speculative Decoding Scheme for Low-Memory GPUs](https://arxiv.org/abs/2405.20314)|Wei Zhong, Manasa Bharadwaj (@LG)| |arXiv'24|
|2024.06|<a id="EESD_ACL_24"></a>[**[EESD]** Speculative Decoding via Early-exiting for Faster LLM Inference with Thompson Sampling Control Mechanism](https://arxiv.org/abs/2406.03853)|Jiahao Liu, Qifan Wang et al. (@Meituan \& Meta)| |Findings-ACL'24|
|2024.06|<a id="EAGLE-2_EMNLP_24"></a>[**[EAGLE-2]** Faster Inference of Language Models with Dynamic Draft Trees](https://arxiv.org/abs/2406.16858)|Yuhui Li et al. (@PKU \& Microsoft \& University of Waterloo \& Vector Institute)|[[code]](https://github.com/SafeAILab/EAGLE)![](https://img.shields.io/github/stars/SafeAILab/EAGLE?style=social)|EMNLP'24|
|2024.06|<a id="Amphista_NAACL_25"></a>[**[Amphista]** Bi-directional Multi-head Decoding for Accelerating LLM Inference](https://arxiv.org/abs/2406.13170)|Zeping Li et al. (@AMD \& PKU)| |NAACL'25|
|2024.08|<a id="Clover-2_arXiv_24"></a>[**[Clover-2]** Accurate Inference for Regressive Lightweight Speculative Decoding](https://arxiv.org/abs/2408.00264)|Bin Xiao et al. (@Baichuan Inc. \& BIT)|[[code]](https://github.com/XiaoBin1992/clover)![](https://img.shields.io/github/stars/XiaoBin1992/clover?style=social)|arXiv'24|
|2024.08|<a id="KOALA_CSCWD_25"></a>[**[KOALA]** Enhancing Speculative Decoding for LLM via Multi-Layer Draft Heads with Adversarial Learning](https://arxiv.org/abs/2408.08146)|Kaiqi Zhang, Jing Zhao, Rui Chen (@DUT)| |CSCWD'25|
|2024.10|<a id="Draft-on-the-Fly_EMNLP_24"></a>[**Draft on the Fly: Adaptive Self-Speculative Decoding using Cosine Similarity**](https://arxiv.org/abs/2410.01028)|Michael R. Metel et al. (@Huawei \& UdeM)| |Findings-EMNLP'24|
|2024.10|<a id="SWIFT_ICLR_25"></a>[**[SWIFT]** On-the-Fly Self-Speculative Decoding for LLM Inference Acceleration](https://arxiv.org/abs/2410.06916)|Heming Xia et al. (@PolyU \& ZJU \& Sea AI Lab)|[[code]](https://github.com/hemingkx/SWIFT)![](https://img.shields.io/github/stars/hemingkx/SWIFT?style=social)|ICLR'25|
|2024.10|<a id="Mixture_of_Attentions_ICLR_25"></a>[**[Mixture of Attentions]** Mixture of Attentions For Speculative Decoding](https://arxiv.org/abs/2410.03804)|Matthieu Zimmer et al. (@Huawei \& UCL)|[[code]](https://github.com/huawei-noah/HEBO/tree/mixture-of-attentions)![](https://img.shields.io/github/stars/huawei-noah/HEBO?style=social)|ICLR'25|
|2024.12|<a id="DeepSeek-V3_arXiv_24"></a>[**[DeepSeek-V3]** DeepSeek-V3 Technical Report](https://arxiv.org/abs/2412.19437)|DeepSeek-AI (@DeepSeek)|[[code]](https://github.com/deepseek-ai/DeepSeek-V3)![](https://img.shields.io/github/stars/deepseek-ai/DeepSeek-V3?style=social)|arXiv'24|
|2025.02|<a id="On_MTP_ICLR_25"></a>[**On multi-token prediction for efficient LLM inference**](https://arxiv.org/abs/2502.09419)|Somesh Mehra, Javier Alonso Garcia, Lukas Mauch (@Sony \& EPFL)| |ICLRW'25|
|2025.03|<a id="EAGLE-3_NeurIPS_25"></a>[**[EAGLE-3]** Scaling up Inference Acceleration of Large Language Models via Training-Time Test](https://arxiv.org/abs/2503.01840)|Yuhui Li et al. (@PKU \& Microsoft \& University of Waterloo \& Vector Institute)|[[code]](https://github.com/SafeAILab/EAGLE)![](https://img.shields.io/github/stars/SafeAILab/EAGLE?style=social)|NeurIPS'25|
|2025.03|<a id="Gumiho_ICML_25"></a>[**[Gumiho]** A Hybrid Architecture to Prioritize Early Tokens in Speculative Decoding](https://arxiv.org/abs/2503.10135)|Jinze Li et al. (@HKU \& XJTU \& AMD)|[[code]](https://github.com/AMD-AIG-AIMA/Gumiho)![](https://img.shields.io/github/stars/AMD-AIG-AIMA/Gumiho?style=social)|ICML'25|
|2025.04|<a id="DEL_COLM_25"></a>[**[DEL]** Context-Aware Dynamic Exit Layer for Efficient Self-Speculative Decoding](https://arxiv.org/abs/2504.05598)|Hossein Entezari Zarch et al. (@USC)|[[code]](https://github.com/hoenza/DEL)![](https://img.shields.io/github/stars/hoenza/DEL?style=social)|COLM'25|
|2025.05|<a id="CLaSp_ACL_25"></a>[**[CLaSp]** In-Context Layer Skip for Self-Speculative Decoding](https://arxiv.org/abs/2505.24196)|Longze Chen et al. (@CAS \& UCAS \& SUTD \& UNSW \& Ritzz-AI)| |ACL'25|
|2025.05|<a id="L-MTP_NeurIPS_25"></a>[**[L-MTP]** Leap Multi-Token Prediction Beyond Adjacent Context for Large Language Models](https://arxiv.org/abs/2505.17505)|Xiaohao Liu et al. (@NUS \& HIT \& THU \& CAS \& CSU)|[[code]](https://github.com/Xiaohao-Liu/L-MTP)![](https://img.shields.io/github/stars/Xiaohao-Liu/L-MTP?style=social)|NeurIPS'25|
|2025.05|<a id="Beagle_arXiv_25"></a>[**[Beagle]** Cross-Attention Speculative Decoding](https://arxiv.org/abs/2505.24544)|Wei Zhong et al. (@LG)| |arXiv'25|
|2025.06|<a id="AdaDecode_ICML_25"></a>[**[AdaDecode]** Accelerating LLM Decoding with Adaptive Layer Parallelism](https://arxiv.org/abs/2506.03700)|Zhepei Wei et al. (@University of Virginia)|[[code]](https://github.com/weizhepei/AdaDecode)![](https://img.shields.io/github/stars/weizhepei/AdaDecode?style=social)|ICML'25|
|2025.07|<a id="Your_LLM_Knows_Future_arXiv_25"></a>[**Your LLM Knows the Future: Uncovering Its Multi-Token Prediction Potential**](https://arxiv.org/abs/2507.11851)|Mohammad Samragh et al. (@Apple)| |arXiv'25|
|2025.10|<a id="CAS-Spec_NeurIPS_25"></a>[**[CAS-Spec]** Cascade Adaptive Self-Speculative Decoding for On-the-Fly Lossless Inference Acceleration of LLMs](https://arxiv.org/abs/2510.26843)|Zhiyuan Ning et al. (@TeleAI \& SJTU \& HKUST)| |NeurIPS'25|
|2025.11|<a id="FractalLLM_EMNLP_25"></a>[**[FractalLLM]** Lossless Self-Speculative Decoding with Layer Embedded Self-Compression](https://aclanthology.org/2025.findings-emnlp.1286/)|Juhyeong Kim et al. (@Gachon University)|[[code]](https://github.com/YEonleo/FractalLLM)![](https://img.shields.io/github/stars/YEonleo/FractalLLM?style=social)|Findings-EMNLP'25|
|2025.11|<a id="Speculative_Streaming_EMNLP_25"></a>[**[Speculative Streaming]** Efficient and Scalable Speculative Decoding with Multi-Stream Attention](https://aclanthology.org/2025.emnlp-main.986/)|Nikhil Bhendawade et al. (@Apple)|[[code]](https://github.com/apple/ml-speculative-streaming)![](https://img.shields.io/github/stars/apple/ml-speculative-streaming?style=social)|EMNLP'25|
|2025.12|<a id="SparseSpec_arXiv_25"></a>[**[SparseSpec]** Accelerating Large-Scale Reasoning Model Inference with Sparse Self-Speculative Decoding](https://arxiv.org/abs/2512.01278)|Yilong Zhao et al. (@UC Berkeley \& MIT \& UW \& NVIDIA \& Cornell \& THU)|[[code]](https://github.com/sspec-project/SparseSpec)![](https://img.shields.io/github/stars/sspec-project/SparseSpec?style=social)|MLSys'26|
|2026.02|<a id="mtp-lm_arXiv_26"></a>[**[mtp-lm]** Multi-Token Prediction via Self-Distillation](https://arxiv.org/abs/2602.06019)|John Kirchenbauer et al. (@UMD \& LLNL \& Columbia \& Together AI)|[[code]](https://github.com/jwkirchenbauer/mtp-lm)![](https://img.shields.io/github/stars/jwkirchenbauer/mtp-lm?style=social)|ICMLW'26|
|2026.03|<a id="ESP_ICML_26"></a>[**[ESP]** Efficient Training-Free Multi-Token Prediction via Embedding-Space Probing](https://arxiv.org/abs/2603.17942)|Raghavv Goel et al. (@Qualcomm AI Research)| |ICML'26|

<a id="subclass-1-3"></a>
### 1.3 Retrieval-Based and Non-Parametric Drafting

*Drafts come from retrieved text, n-gram or suffix caches, reusable context patterns, or parameter-free fixed-point/Jacobi procedures.*

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2023.05|<a id="Santilli_ACL_23"></a>[**Accelerating Transformer Inference for Translation via Parallel Decoding**](https://arxiv.org/abs/2305.10427)|Andrea Santilli et al. (@Sapienza University of Rome \& University of Tübingen \& Tübingen AI Center)|[[code]](https://github.com/teelinsan/parallel-decoding)![](https://img.shields.io/github/stars/teelinsan/parallel-decoding?style=social)|ACL'23|
|2023.11|<a id="REST_NAACL_24"></a>[**[REST]** Retrieval-Based Speculative Decoding](https://arxiv.org/abs/2311.08252)|Zhenyu He et al. (@PKU \& Princeton)|[[code]](https://github.com/FasterDecoding/REST)![](https://img.shields.io/github/stars/FasterDecoding/REST?style=social)|NAACL'24|
|2024.02|<a id="Lookahead_Decoding_ICML_24"></a>[**[Lookahead Decoding]** Break the Sequential Dependency of LLM Inference Using Lookahead Decoding](https://arxiv.org/abs/2402.02057)|Yichao Fu et al. (@UCSD \& Google \& UC Berkeley)|[[code]](https://github.com/hao-ai-lab/LookaheadDecoding)![](https://img.shields.io/github/stars/hao-ai-lab/LookaheadDecoding?style=social)|ICML'24|
|2024.02|<a id="Ouroboros_EMNLP_24"></a>[**[Ouroboros]** Generating Longer Drafts Phrase by Phrase for Faster Speculative Decoding](https://arxiv.org/abs/2402.13720)|Weilin Zhao et al. (@THU \& Shanghai AI Lab \& ModelBest)|[[code]](https://github.com/thunlp/Ouroboros)![](https://img.shields.io/github/stars/thunlp/Ouroboros?style=social)|EMNLP'24|
|2024.03|<a id="CLLMs_ICML_24"></a>[**[CLLMs]** Consistency Large Language Models](https://arxiv.org/abs/2403.00835)|Siqi Kou et al. (@SJTU \& UCSD)|[[code]](https://github.com/hao-ai-lab/Consistency_LLM)![](https://img.shields.io/github/stars/hao-ai-lab/Consistency_LLM?style=social)|ICML'24|
|2024.05|<a id="NEST_NeurIPS_24"></a>[**[NEST]** Nearest Neighbor Speculative Decoding for LLM Generation and Attribution](https://arxiv.org/abs/2405.19325)|Minghan Li et al. (@Cohere \& FAIR \& University of Chicago \& CMU \& University of Waterloo)|[[code]](https://github.com/facebookresearch/NEST)![](https://img.shields.io/github/stars/facebookresearch/NEST?style=social)|NeurIPS'24|
|2024.06|<a id="MSN_EMNLP_24"></a>[**[MSN]** Make Some Noise: Unlocking Language Model Parallel Inference Capability through Noisy Training](https://arxiv.org/abs/2406.17404)|Yixuan Wang et al. (@HIT \& Du Xiaoman)|[[code]](https://github.com/wyxstriker/MakeSomeNoiseInference)![](https://img.shields.io/github/stars/wyxstriker/MakeSomeNoiseInference?style=social)|EMNLP'24|
|2024.08|<a id="Token_Recycling_ACL_25"></a>[**[Token Recycling]** Turning Trash into Treasure: Accelerating Inference of Large Language Models with Token Recycling](https://arxiv.org/abs/2408.08696)|Xianzhen Luo et al. (@HIT \& Du Xiaoman)|[[code]](https://github.com/Luowaterbi/TokenRecycling)![](https://img.shields.io/github/stars/Luowaterbi/TokenRecycling?style=social)|ACL'25|
|2024.11|<a id="SuffixDecoding_NeurIPS_25"></a>[**[SuffixDecoding]** Extreme Speculative Decoding for Emerging AI Applications](https://arxiv.org/abs/2411.04975)|Gabriele Oliaro et al. (@CMU \& Snowflake)|[[code]](https://github.com/snowflakedb/ArcticInference)![](https://img.shields.io/github/stars/snowflakedb/ArcticInference?style=social)|NeurIPS'25|
|2024.11|<a id="SAM_Decoding_ACL_25"></a>[**[SAM Decoding]** Speculative Decoding via Suffix Automaton](https://arxiv.org/abs/2411.10666)|Yuxuan Hu et al. (@Renmin University \& THU)|[[code]](https://github.com/hyx1999/SAM-Decoding)![](https://img.shields.io/github/stars/hyx1999/SAM-Decoding?style=social)|ACL'25|
|2025.02|<a id="DReSD_ACL_25"></a>[**[DReSD]** Dense Retrieval for Speculative Decoding](https://arxiv.org/abs/2502.15572)|Milan Gritta et al. (@Huawei \& University of Sheffield)|[[code]](https://github.com/huawei-noah/HEBO/tree/DReSD)![](https://img.shields.io/github/stars/huawei-noah/HEBO?style=social)|Findings-ACL'25|
|2025.02|<a id="RAPID_ICML_25"></a>[**[RAPID]** Long-Context Inference with Retrieval-Augmented Speculative Decoding](https://arxiv.org/abs/2502.20330)|Guanzheng Chen et al. (@NUS \& DAMO Academy/Alibaba \& Hupan Lab)|[[code]](https://github.com/NUS-TRAIL/RAPID)![](https://img.shields.io/github/stars/NUS-TRAIL/RAPID?style=social)|ICML'25|
|2025.02|<a id="Temporal-Locality_Drafting_NAACL_25"></a>[**[Hierarchy Drafting]** Lossless Acceleration of Large Language Models with Hierarchical Drafting based on Temporal Locality in Speculative Decoding](https://arxiv.org/abs/2502.05609)|Sukmin Cho et al. (@KAIST)|[[code]](https://github.com/zomss/Hierarchy_Drafting)![](https://img.shields.io/github/stars/zomss/Hierarchy_Drafting?style=social)|Findings-NAACL'25|
|2025.03|<a id="RASD_ACL_25"></a>[**[RASD]** Retrieval-Augmented Speculative Decoding](https://arxiv.org/abs/2503.03434)|Guofeng Quan et al. (@Alibaba Cloud)| |Findings-ACL'25|
|2025.11|<a id="N-Gram_Trie_SD_EMNLP_25"></a>[**Faster In-Context Learning for LLMs via N-Gram Trie Speculative Decoding**](https://aclanthology.org/2025.emnlp-main.911/)|Jinglin Chen et al. (@Wuhan University \& Xiaomi \& SJTU)|[[code]](https://github.com/mrlife219/Ngram-Trie)![](https://img.shields.io/github/stars/mrlife219/Ngram-Trie?style=social)|EMNLP'25|
|2025.11|<a id="Cacheback_EMNLP_25"></a>[**[Cacheback]** Speculative Decoding With Nothing But Cache](https://arxiv.org/abs/2511.21699)|Zhiyao Ma et al. (@Yale University)|[[code]](https://github.com/zyma98/Spec-Bench/tree/cacheback)![](https://img.shields.io/github/stars/zyma98/Spec-Bench?style=social)|EMNLP'25|
|2025.12|<a id="Jacobi_Forcing_arXiv_25"></a>[**[Jacobi Forcing]** Fast and Accurate Causal Parallel Decoding using Jacobi Forcing](https://arxiv.org/abs/2512.14681)|Lanxiang Hu et al. (@UCSD \& SJTU \& Snowflake)|[[code]](https://github.com/hao-ai-lab/JacobiForcing)![](https://img.shields.io/github/stars/hao-ai-lab/JacobiForcing?style=social)|ICML'26|

<a id="subclass-1-4"></a>
### 1.4 Draft Structure and Sampling Strategies

*The main contribution concerns candidate-tree structure, multi-draft coupling and acceptance, adaptive draft length, verification order, or theoretical properties of speculative sampling.*

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2023.05|<a id="SpecInfer_ASPLOS_24"></a>[**[SpecInfer]** Accelerating Large Language Model Serving with Tree-based Speculative Inference and Verification](https://arxiv.org/abs/2305.09781)|Xupeng Miao et al. (@CMU \& THU \& Stanford \& SJTU \& PKU \& UCSD)|[[code]](https://github.com/flexflow/FlexFlow)![](https://img.shields.io/github/stars/flexflow/FlexFlow?style=social)|ASPLOS'24|
|2023.10|<a id="SpecTr_NeurIPS_23"></a>[**[SpecTr]** Fast Speculative Decoding via Optimal Transport](https://arxiv.org/abs/2310.15141)|Ziteng Sun et al. (@Google Research)| |NeurIPS'23|
|2024.01|<a id="MCSD_NLPCC_25"></a>[**[MCSD]** Multi-candidate Speculative Decoding](https://arxiv.org/abs/2401.06706)|Sen Yang et al. (@NJU)|[[code]](https://github.com/NJUNLP/MCSD)![](https://img.shields.io/github/stars/NJUNLP/MCSD?style=social)|NLPCC'25|
|2024.02|<a id="Sequoia_NeurIPS_24"></a>[**[Sequoia]** Scalable and Robust Speculative Decoding](https://arxiv.org/abs/2402.12374)|Zhuoming Chen et al. (@CMU \& Together AI \& Yandex \& HSE \& FAIR)|[[code]](https://github.com/Infini-AI-Lab/Sequoia)![](https://img.shields.io/github/stars/Infini-AI-Lab/Sequoia?style=social)|NeurIPS'24|
|2024.02|<a id="Tree_Monte_Carlo_ICML_24"></a>[**Accelerated Speculative Sampling Based on Tree Monte Carlo**](https://proceedings.mlr.press/v235/hu24f.html)|Zhengmian Hu et al. (@University of Maryland)| |ICML'24|
|2024.03|<a id="Block_Verification_ICLR_25"></a>[**Block Verification Accelerates Speculative Decoding**](https://arxiv.org/abs/2403.10444)|Ziteng Sun et al. (@Google Research)| |ICLR'25|
|2024.05|<a id="SLiM_NAACL_24"></a>[**[SLiM]** Speculative Decoding with Hypothesis Reduction](https://aclanthology.org/2024.findings-naacl.63/)|Chi-Heng Lin et al. (@Samsung Research America)| |Findings-NAACL'24|
|2024.05|<a id="Dynamic_Spec_Lookahead_arXiv_24"></a>[**[DISCO]** Dynamic Speculation Lookahead Accelerates Speculative Decoding of Large Language Models](https://arxiv.org/abs/2405.04304)|Jonathan Mamou et al. (@Intel \& Weizmann \& Hebrew University)| |NeurIPSW'24|
|2024.06|<a id="OPT-Tree_TACL_25"></a>[**[OPT-Tree]** Speculative Decoding with Adaptive Draft Tree Structure](https://arxiv.org/abs/2406.17276)|Jikai Wang et al. (@Soochow University \& Huawei Cloud)|[[code]](https://github.com/Jikai0Wang/OPT-Tree)![](https://img.shields.io/github/stars/Jikai0Wang/OPT-Tree?style=social)|TACL'25|
|2024.07|<a id="Graph-Structured_SD_ACL_24"></a>[**[GSD]** Graph-Structured Speculative Decoding](https://arxiv.org/abs/2407.16207)|Zhuocheng Gong et al. (@PKU \& Meituan \& Tianjin University \& RUC)|[[code]](https://github.com/gzhch/gsd)![](https://img.shields.io/github/stars/gzhch/gsd?style=social)|Findings-ACL'24|
|2024.09|<a id="DDD_arXiv_24"></a>[**[DDD]** Dynamic Depth Decoding: Faster Speculative Decoding for LLMs](https://arxiv.org/abs/2409.00142)|Oscar Brown et al. (@ML Research Labs \& ANU)| |arXiv'24|
|2024.10|<a id="Multi-Draft_Canonical_ICLR_25"></a>[**Multi-Draft Speculative Sampling: Canonical Decomposition and Theoretical Limits**](https://arxiv.org/abs/2410.18234)|Ashish Khisti et al. (@Qualcomm AI Research \& University of Toronto)| |ICLR'25|
|2024.11|<a id="Theory_of_SD_NeurIPS_24"></a>[**A Theoretical Perspective for Speculative Decoding Algorithm**](https://arxiv.org/abs/2411.00841)|Ming Yin et al. (@Princeton \& Northwestern)| |NeurIPS'24|
|2024.11|<a id="SpecHub_EMNLP_24"></a>[**[SpecHub]** Provable Acceleration to Multi-Draft Speculative Decoding](https://arxiv.org/abs/2411.05289)|Ryan Sun et al. (@Lehigh University \& Samsung Research America \& University of Maryland)|[[code]](https://github.com/MasterGodzilla/Speculative_decoding_OT)![](https://img.shields.io/github/stars/MasterGodzilla/Speculative_decoding_OT?style=social)|EMNLP'24|
|2024.11|<a id="SVIP_EMNLP_25"></a>[**[SVIP]** Draft Model Knows When to Stop: Self-Verification Speculative Decoding for Long-Form Generation](https://arxiv.org/abs/2411.18462)|Ziyin Zhang et al. (@SJTU \& Tencent)|[[code]](https://github.com/Geralt-Targaryen/SVIP)![](https://img.shields.io/github/stars/Geralt-Targaryen/SVIP?style=social)|EMNLP'25|
|2024.12|<a id="AdaEAGLE_arXiv_24"></a>[**[AdaEAGLE]** Optimizing Speculative Decoding via Explicit Modeling of Adaptive Draft Structures](https://arxiv.org/abs/2412.18910)|Situo Zhang et al. (@SJTU)| |arXiv'24|
|2025.01|<a id="Judge_Decoding_ICLR_25"></a>[**[Judge Decoding]** Faster Speculative Sampling Requires Going Beyond Model Alignment](https://arxiv.org/abs/2501.19309)|Gregor Bachmann et al. (@Meta GenAI \& ETH Zurich)| |ICLR'25|
|2025.02|<a id="TETRIS_ACL_25"></a>[**[TETRIS]** Optimal Draft Token Selection for Batch Speculative Decoding](https://arxiv.org/abs/2502.15197)|Zhaoxuan Wu, Zijian Zhou et al. (@SMART \& NUS \& MIT)|[[code]](https://github.com/ZhaoxuanWu/Tetris)![](https://img.shields.io/github/stars/ZhaoxuanWu/Tetris?style=social)|ACL'25|
|2025.02|<a id="Optimal_Multi-Draft_ICLR_25"></a>[**Towards Optimal Multi-draft Speculative Decoding**](https://arxiv.org/abs/2502.18779)|Zhengmian Hu et al. (@University of Maryland \& Adobe Research \& University of Massachusetts Amherst)| |ICLR'25|
|2025.02|<a id="Fuzzy_SD_ACL_25"></a>[**Fuzzy Speculative Decoding for a Tunable Accuracy-Runtime Tradeoff**](https://arxiv.org/abs/2502.20704)|Maximilian Holsman et al. (@Duke University)|[[code]](https://github.com/maxholsman/fsd)![](https://img.shields.io/github/stars/maxholsman/fsd?style=social)|Findings-ACL'25|
|2025.04|<a id="Hierarchical_Dynamic_Window_NAACL_25"></a>[**[HSDDW]** Hierarchical Speculative Decoding with Dynamic Window](https://aclanthology.org/2025.findings-naacl.462/)|Shensian Syu, Hung-yi Lee (@NTU)|[[code]](https://github.com/valexsyu/HSDDW)![](https://img.shields.io/github/stars/valexsyu/HSDDW?style=social)|Findings-NAACL'25|
|2025.04|<a id="Exponential_Races_ACL_25"></a>[**[ERSS]** Speculative Sampling via Exponential Races](https://arxiv.org/abs/2504.15475)|Szymon Kobus \& Deniz Gündüz (@Imperial College London)| |Findings-ACL'25|
|2025.05|<a id="AASD_EMNLP_25"></a>[**[AASD]** Alignment-Augmented Speculative Decoding with Alignment Sampling and Conditional Verification](https://arxiv.org/abs/2505.13204)|Jikai Wang et al. (@Soochow University \& Huawei Cloud)| |EMNLP'25|
|2025.05|<a id="Traversal_Verification_NeurIPS_25"></a>[**Traversal Verification for Speculative Tree Decoding**](https://arxiv.org/abs/2505.12398)|Yepeng Weng et al. (@Lenovo Research \& CAS)| |NeurIPS'25|
|2025.05|<a id="STree_NeurIPS_25"></a>[**[STree]** Speculative Tree Decoding for Hybrid State Space Models](https://arxiv.org/abs/2505.14969)|Yangchao Wu et al. (@UCLA \& Yale)|[[code]](https://github.com/wyc1997/stree)![](https://img.shields.io/github/stars/wyc1997/stree?style=social)|NeurIPS'25|
|2025.05|<a id="BanditSpec_ICML_25"></a>[**[BanditSpec]** Adaptive Speculative Decoding via Bandit Algorithms](https://arxiv.org/abs/2505.15141)|Yunlong Hou et al. (@NUS \& Sea AI Lab \& SMU \& Yale University)|[[code]](https://github.com/sail-sg/BanditSpec)![](https://img.shields.io/github/stars/sail-sg/BanditSpec?style=social)|ICML'25|
|2025.05|<a id="HeteroSpec_arXiv_25"></a>[**[HeteroSpec]** Leveraging Contextual Heterogeneity for Efficient Speculative Decoding](https://arxiv.org/abs/2505.13254)|Siran Liu et al. (@PKU \& ScitiX AI)| |ACL'26|
|2025.06|<a id="List-Level_Coupling_NeurIPS_25"></a>[**List-Level Distribution Coupling with Applications to Speculative Decoding and Lossy Compression**](https://arxiv.org/abs/2506.05632)|Joseph Rowan et al. (@University of Toronto)|[[code]](https://github.com/jsrowan/MultiDraftSpeculativeDecoding)![](https://img.shields.io/github/stars/jsrowan/MultiDraftSpeculativeDecoding?style=social)|NeurIPS'25|
|2025.06|<a id="SpecBranch_ICLR_26"></a>[**[SpecBranch]** Speculative Decoding via Hybrid Drafting and Rollback-Aware Branch Parallelism](https://arxiv.org/abs/2506.01979)|Yuhao Shen, Junyi Shen et al. (@ZJU \& NUS \& USTC)|[[code]](https://github.com/Sylvan820/Specbranch)![](https://img.shields.io/github/stars/Sylvan820/Specbranch?style=social)|ICLR'26|
|2025.07|<a id="Drop-In_Adaptation_ACL_25"></a>[**A Drop-In Solution for On-the-Fly Adaptation of Speculative Decoding in Large Language Models**](https://aclanthology.org/2025.acl-long.482/)|Jiesong Liu et al. (@NCSU)| |ACL'25|
|2025.07|<a id="Pruned_Candidate_Tree_ACL_25"></a>[**Faster Speculative Decoding via Effective Draft Decoder with Pruned Candidate Tree**](https://aclanthology.org/2025.acl-long.486/)|Huanran Zheng, Xiaoling Wang (@ECNU)|[[code]](https://github.com/boom-R123/Faster_SD)![](https://img.shields.io/github/stars/boom-R123/Faster_SD?style=social)|ACL'25|
|2025.09|<a id="Group_Tree_Opt_ICLR_26"></a>[**[GTO]** Bridging Draft Policy Misalignment: Group Tree Optimization for Speculative Decoding](https://arxiv.org/abs/2509.22134)|Shijing Hu et al. (@Fudan \& NUS \& SMU)|[[code]](https://github.com/hsj576/GTO)![](https://img.shields.io/github/stars/hsj576/GTO?style=social)|ICLR'26|
|2025.10|<a id="polybasic_SD_ICML_25"></a>[**polybasic Speculative Decoding Through a Theoretical Perspective**](https://arxiv.org/abs/2510.26527)|Ruilin Wang et al. (@Xiamen University \& ByteDance \& Peng Cheng Lab)| |ICML'25|
|2025.10|<a id="Not-a-Bandit_ICLR_26"></a>[**Not-a-Bandit: Provably No-Regret Drafter Selection in Speculative Decoding for LLMs**](https://arxiv.org/abs/2510.20064)|Hongyi Liu et al. (@Rice \& AWS \& UCSD)|[[code]](https://github.com/aladinggit/hedgespec)![](https://img.shields.io/github/stars/aladinggit/hedgespec?style=social)|ICLR'26|
|2025.11|<a id="Global_Resolution_ICLR_26"></a>[**Global Resolution: Optimal Multi-Draft Speculative Sampling via Convex Optimization**](https://arxiv.org/abs/2511.15898)|Rahul Krishna Thomas \& Arka Pal (@Stanford \& Ritual)| |ICLR'26|
|2025.11|<a id="Loosely_SD_ICLR_26"></a>[**[FLy]** Training-Free Loosely Speculative Decoding: Accepting Semantically Correct Drafts Beyond Exact Match](https://arxiv.org/abs/2511.22972)|Jinze Li et al. (@AMD \& HKU)|[[code]](https://github.com/AMD-AGI/FLy)![](https://img.shields.io/github/stars/AMD-AGI/FLy?style=social)|ICLR'26|
|2026.01|<a id="Lossless_Hierarchical_SD_ICLR_26"></a>[**Overcoming Joint Intractability with Lossless Hierarchical Speculative Decoding**](https://arxiv.org/abs/2601.05724)|Yuxuan Zhou et al. (Alibaba (Qwen Team) \& UW)|[[code]](https://github.com/ZhouYuxuanYX/Hierarchical-Speculative-Decoding)![](https://img.shields.io/github/stars/ZhouYuxuanYX/Hierarchical-Speculative-Decoding?style=social)|ICLR'26|
|2026.03|<a id="Learning_to_Draft_ICLR_26"></a>[**[LTD]** Learning To Draft: Adaptive Speculative Decoding with Reinforcement Learning](https://arxiv.org/abs/2603.01639)|Jiebin Zhang et al. (@PKU \& MSRA)|[[code]](https://github.com/zhzihao/Learning-to-Draft)![](https://img.shields.io/github/stars/zhzihao/Learning-to-Draft?style=social)|ICLR'26|
|2026.03|<a id="Spec-Spec_Decoding_ICLR_26"></a>[**[SSD]** Speculative Speculative Decoding](https://arxiv.org/abs/2603.03251)|Tanishq Kumar, Tri Dao, Avner May (@Stanford \& Princeton \& Together AI)|[[code]](https://github.com/tanishqkumar/ssd)![](https://img.shields.io/github/stars/tanishqkumar/ssd?style=social)|ICLR'26|
|2026.04|<a id="Cactus_ICLR_26"></a>[**[Cactus]** Accelerating Auto-Regressive Decoding with Constrained Acceptance Speculative Sampling](https://arxiv.org/abs/2604.04987)|Yongchang Hao, Lili Mou (@University of Alberta \& Amii \& Canada CIFAR AI Chair)|[[code]](https://github.com/MANGA-UOFA/Cactus)![](https://img.shields.io/github/stars/MANGA-UOFA/Cactus?style=social)|ICLR'26|
|2026.05|<a id="SpecBlock_arXiv_26"></a>[**[SpecBlock]** Block-Iterative Speculative Decoding with Dynamic Tree Drafting](https://arxiv.org/abs/2605.07243)|Weijie Shi et al. (@HKUST \& MetaX \& ZJNU \& Soochow University)|[[code]](https://github.com/shiweijiezero/SpecBlock)![](https://img.shields.io/github/stars/shiweijiezero/SpecBlock?style=social)|arXiv'26|
|2026.06|<a id="JetSpec_arXiv_26"></a>[**[JetSpec]** Breaking the Scaling Ceiling of Speculative Decoding with Parallel Tree Drafting](https://arxiv.org/abs/2606.18394)|Lanxiang Hu et al. (@UCSD \& NJU \& StepFun)|[[code]](https://github.com/hao-ai-lab/JetSpec)![](https://img.shields.io/github/stars/hao-ai-lab/JetSpec?style=social)|arXiv'26|

<a id="subclass-1-5"></a>
### 1.5 Cross-Technique Integration

*Speculative decoding is integrated with another technique or application domain, including multimodal generation, quality/alignment/safety controls, watermarking, and accelerated reinforcement-learning rollouts.*

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2023.11|<a id="Spec_Contrastive_ACL_24"></a>[**[SCD]** Speculative Contrastive Decoding](https://arxiv.org/abs/2311.08981)|Hongyi Yuan et al. (@THU \& Alibaba)| |ACL'24|
|2024.10|<a id="Watermark_SD_NeurIPS_24"></a>[**Inevitable Trade-off between Watermark Strength and Speculative Sampling Efficiency for Language Models**](https://arxiv.org/abs/2410.20418)|Zhengmian Hu \& Heng Huang (@University of Maryland)|[[code]](https://github.com/xiaoniu-578fa6bff964d005/AcceleratedUnbiasedWatermark)![](https://img.shields.io/github/stars/xiaoniu-578fa6bff964d005/AcceleratedUnbiasedWatermark?style=social)|NeurIPS'24|
|2024.12|<a id="Constrained_Spec_Lookaheads_NAACL_25"></a>[**[CDSL]** Constrained Decoding with Speculative Lookaheads](https://arxiv.org/abs/2412.10418)|Nishanth Nakshatri et al. (@Purdue University \& AWS AI Labs)|[[repo (code coming soon)]](https://github.com/amazon-science/CDSL-NAACL2025)![](https://img.shields.io/github/stars/amazon-science/CDSL-NAACL2025?style=social)|NAACL'25|
|2025.05|<a id="DREAM_NeurIPS_25"></a>[**[DREAM]** Drafting with Refined Target Features and Entropy-Adaptive Cross-Attention Fusion for Multimodal Speculative Decoding](https://arxiv.org/abs/2505.19201)|Yunhai Hu et al. (@NYU \& UPenn \& Cerebras Systems)|[[code]](https://github.com/SAI-Lab-NYU/DREAM)![](https://img.shields.io/github/stars/SAI-Lab-NYU/DREAM?style=social)|NeurIPS'25|
|2025.05|<a id="MASSV_EMNLP_25"></a>[**[MASSV]** Multimodal Adaptation and Self-Data Distillation for Speculative Decoding of Vision-Language Models](https://arxiv.org/abs/2505.10526)|Mugilan Ganesan et al. (@University of Waterloo \& Cerebras Systems)| |Findings-EMNLP'25|
|2025.07|<a id="Spec-VLA_EMNLP_25"></a>[**[Spec-VLA]** Speculative Decoding for Vision-Language-Action Models with Relaxed Acceptance](https://arxiv.org/abs/2507.22424)|Songsheng Wang et al. (@University of Macau \& Infinigence AI \& THU \& Zhongguancun Academy)|[[code]](https://github.com/PineTreeWss/SpecVLA)![](https://img.shields.io/github/stars/PineTreeWss/SpecVLA?style=social)|EMNLP'25|
|2025.08|<a id="SpecVLM_EMNLP_25"></a>[**[SpecVLM]** Enhancing Speculative Decoding of Video LLMs via Verifier-Guided Token Pruning](https://arxiv.org/abs/2508.16201)|Yicheng Ji et al. (@ZJU \& PolyU \& BUPT)|[[code]](https://github.com/zju-jiyicheng/SpecVLM)![](https://img.shields.io/github/stars/zju-jiyicheng/SpecVLM?style=social)|EMNLP'25|
|2025.08|<a id="Reward-Shifted_SS_EMNLP_25"></a>[**Reward-Shifted Speculative Sampling Is An Efficient Test-Time Weak-to-Strong Aligner**](https://arxiv.org/abs/2508.15044)|Bolian Li et al. (@Purdue University)|[[code]](https://github.com/lblaoke/CARDS)![](https://img.shields.io/github/stars/lblaoke/CARDS?style=social)|EMNLP'25|
|2025.08|<a id="Safety-Aware_SD_EMNLP_25"></a>[**Speculative Safety-Aware Decoding**](https://arxiv.org/abs/2508.17739)|Xuekang Wang et al. (@BIT \& CAS)|[[code]](https://github.com/k-k1w-w1x-x/Speculative-Safety-Aware-Decoding)![](https://img.shields.io/github/stars/k-k1w-w1x-x/Speculative-Safety-Aware-Decoding?style=social)|EMNLP'25|
|2025.09|<a id="ViSpec_NeurIPS_25"></a>[**[ViSpec]** Accelerating Vision-Language Models with Vision-Aware Speculative Decoding](https://arxiv.org/abs/2509.15235)|Jialiang Kang et al. (@PKU \& Huawei)|[[code]](https://github.com/KangJialiang/ViSpec)![](https://img.shields.io/github/stars/KangJialiang/ViSpec?style=social)|NeurIPS'25|
|2025.09|<a id="SPEC-RL_arXiv_25"></a>[**[SPEC-RL]** Accelerating On-Policy Reinforcement Learning with Speculative Rollouts](https://arxiv.org/abs/2509.23232)|Bingshuai Liu et al. (@Xiamen University \& Shopee \& THU)|[[code]](https://github.com/ShopeeLLM/Spec-RL)![](https://img.shields.io/github/stars/ShopeeLLM/Spec-RL?style=social)|arXiv'25|
|2025.09|<a id="FastGRPO_arXiv_25"></a>[**[FastGRPO]** Accelerating Policy Optimization via Concurrency-aware Speculative Decoding and Online Draft Learning](https://arxiv.org/abs/2509.21792)|Yizhou Zhang et al. (@Lanzhou University \& HKU \& NUS)|[[code]](https://github.com/yedaotian9/GRPO_speculative)![](https://img.shields.io/github/stars/yedaotian9/GRPO_speculative?style=social)|ICLR'26|
|2025.10|<a id="ReSpec_MLSys_26"></a>[**[ReSpec]** Towards Optimizing Speculative Decoding in Reinforcement Learning Systems](https://arxiv.org/abs/2510.26475)|Qiaoling Chen et al. (@NTU \& THU \& NUS \& Qiji Zhifeng \& Shanghai Innovation Institute)| |MLSys'26|
|2025.11|<a id="Beat-the-Long-Tail_MLSys_26"></a>[**[DAS]** Beat the long tail: Distribution-Aware Speculative Decoding for RL Training](https://proceedings.mlsys.org/paper_files/paper/2026/hash/cbc4ab80cd77aa0eb87da062fbcddb46-Abstract-Conference.html)|Zelei Shao et al. (@UIUC \& Together AI \& UCSD \& Prime Intellect)| |MLSys'26|
|2025.11|<a id="SpecActor_arXiv_25"></a>[**[SpecActor]** Fast LLM Post-training via Decoupled and Fastest-of-N Speculation](https://arxiv.org/abs/2511.16193)|Rongxin Cheng et al. (@SJTU \& ByteDance Seed)| |arXiv'25|
|2025.11|<a id="TLT_ASPLOS_26"></a>[**[TLT]** Taming the Long-Tail: Efficient Reasoning RL Training with Adaptive Drafter](https://arxiv.org/abs/2511.16665)|Qinghao Hu et al. (@MIT \& NVIDIA \& ETH Zurich \& MIT-IBM AI Lab \& UMass Amherst)|[[code]](https://github.com/mit-han-lab/fastrl)![](https://img.shields.io/github/stars/mit-han-lab/fastrl?style=social)|ASPLOS'26|
|2025.12|<a id="RLHFSpec_arXiv_25"></a>[**[RLHFSpec]** Breaking the Efficiency Bottleneck in RLHF Training via Adaptive Drafting](https://arxiv.org/abs/2512.04752)|Siqi Wang et al. (@Beihang University)| |arXiv'25|
|2026.01|<a id="SRT_NeurIPS_25_ER"></a>[**[SRT]** Accelerating Reinforcement Learning via Speculative Rollout with Tree-Structured Cache](https://arxiv.org/abs/2601.09083)|Chi-Chih Chang et al. (@Cornell \& UIUC \& THU \& UW \& ByteDance)| |NeurIPSW'25|
|2026.02|<a id="Watermark_SD-2_ICLR_26"></a>[**Improving the Trade-off Between Watermark Strength and Speculative Sampling Efficiency for Language Models**](https://arxiv.org/abs/2602.01428)|Weiqing He et al. (@University of Pennsylvania)|[[code]](https://github.com/hwq0726/watermark-tradeoff)![](https://img.shields.io/github/stars/hwq0726/watermark-tradeoff?style=social)|ICLR'26|

<a id="class-2"></a>
## 📙 2. Sequence-Level Parallel Decoding

Sequence-level methods expose parallelism over semantic units, reasoning segments, partial chains, or complete candidate trajectories.

<a id="subclass-2-1"></a>
### 2.1 Multi-Sequence Parallel Generation

*Several reasoning continuations or complete answer sequences are generated in parallel and then verified, corrected, selected, or aggregated at the semantic level.*

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2023.11|<a id="FastCoT_arXiv_23"></a>[**[FastCoT]** Fast Chain-of-Thought: A Glance of Future from Parallel Decoding Leads to Answers Faster](https://arxiv.org/abs/2311.08263)|Hongxuan Zhang et al. (@NJU \& Ant Group)| |arXiv'23|
|2024.06|<a id="SEED_COLING_25"></a>[**[SEED]** Accelerating Reasoning Tree Construction via Scheduled Speculative Decoding](https://arxiv.org/abs/2406.18200)|Zhenglin Wang et al. (@SEU)|[[code]](https://github.com/Linking-ai/SEED)![](https://img.shields.io/github/stars/Linking-ai/SEED?style=social)|COLING'25|
|2024.07|<a id="Speculative_RAG_ICLR_25"></a>[**[Speculative RAG]** Enhancing Retrieval Augmented Generation through Drafting](https://arxiv.org/abs/2407.08223)|Zilong Wang et al. (@UCSD \& Google)| |ICLR'25|
|2025.01|<a id="RSD_ICML_25"></a>[**[RSD]** Reward-Guided Speculative Decoding for Efficient LLM Reasoning](https://arxiv.org/abs/2501.19324)|Baohao Liao et al. (@University of Amsterdam \& Salesforce AI Research)|[[code]](https://github.com/BaohaoLiao/RSD)![](https://img.shields.io/github/stars/BaohaoLiao/RSD?style=social)|ICML'25|
|2025.03|<a id="Multi-Sample_SD_EMNLP_25"></a>[**Speculative Decoding for Multi-Sample Inference**](https://arxiv.org/abs/2503.05330)|Yiwei Li et al. (@BIT \& Xiaohongshu)| |Findings-EMNLP'25|
|2025.04|<a id="SpecReason_NeurIPS_25"></a>[**[SpecReason]** Fast and Accurate Inference-Time Compute via Speculative Reasoning](https://arxiv.org/abs/2504.07891)|Rui Pan et al. (@Princeton \& CMU)|[[code]](https://github.com/ruipeterpan/specreason)![](https://img.shields.io/github/stars/ruipeterpan/specreason?style=social)|NeurIPS'25|
|2025.04|<a id="Speculative_Thinking_arXiv_25"></a>[**[Speculative Thinking]** Enhancing Small-Model Reasoning with Large Model Guidance at Inference Time](https://arxiv.org/abs/2504.12329)|Wang Yang et al. (@Case Western Reserve University \& Carnegie Mellon University)|[[code]](https://github.com/uservan/speculative_thinking)![](https://img.shields.io/github/stars/uservan/speculative_thinking?style=social)|COLM'25|
|2025.04|<a id="SCoT_arXiv_25"></a>[**[SCoT]** Efficient Reasoning for LLMs through Speculative Chain-of-Thought](https://arxiv.org/abs/2504.19095)|Jikai Wang et al. (@Soochow University \& CUHK \& Meta Stone \& Shanghai AI Lab)|[[code]](https://github.com/Jikai0Wang/Speculative_CoT)![](https://img.shields.io/github/stars/Jikai0Wang/Speculative_CoT?style=social)|Findings-ACL'26|
|2025.05|<a id="SpecSearch_ICML_25"></a>[**[SpecSearch]** Accelerating Large Language Model Reasoning via Speculative Search](https://arxiv.org/abs/2505.02865)|Zhihai Wang et al. (@USTC \& Huawei \& Tianjin University)|[[code]](https://github.com/MIRALab-USTC/LLMReasoning-SpecSearch)![](https://img.shields.io/github/stars/MIRALab-USTC/LLMReasoning-SpecSearch?style=social)|ICML'25|
|2025.05|<a id="R2R_NeurIPS_25"></a>[**[R2R]** Efficiently Navigating Divergent Reasoning Paths with Small-Large Model Token Routing](https://arxiv.org/abs/2505.21600)|Tianyu Fu et al. (@THU \& Infinigence AI \& SJTU)|[[code]](https://github.com/thu-nics/R2R)![](https://img.shields.io/github/stars/thu-nics/R2R?style=social)|NeurIPS'25|
|2025.06|<a id="Lookahead_Reasoning_NeurIPS_25"></a>[**[Lookahead Reasoning]** Scaling Speculative Decoding with Lookahead Reasoning](https://arxiv.org/abs/2506.19830)|Yichao Fu et al. (@UCSD \& SJTU)|[[code]](https://github.com/hao-ai-lab/LookaheadReasoning)![](https://img.shields.io/github/stars/hao-ai-lab/LookaheadReasoning?style=social)|NeurIPS'25|
|2025.06|<a id="STAND_EMNLP_25"></a>[**[STAND]** Accelerated Test-Time Scaling with Model-Free Speculative Sampling](https://arxiv.org/abs/2506.04708)|Woomin Song et al. (@KAIST \& Amazon AGI \& AirSignal)| |EMNLP'25|
|2025.09|<a id="SpecExit_arXiv_25"></a>[**[SpecExit]** Accelerating Large Reasoning Model via Speculative Exit](https://arxiv.org/abs/2509.24248)|Rubing Yang et al. (@Tencent)|[[code]](https://github.com/Tencent/AngelSlim)![](https://img.shields.io/github/stars/Tencent/AngelSlim?style=social)|ICML'26|
|2025.11|<a id="SpecCoT_EMNLP_25"></a>[**[SpecCoT]** Accelerating Chain-of-Thought Reasoning through Speculative Exploration](https://aclanthology.org/2025.findings-emnlp.1326/)|Junhan Shi et al. (@THU \& Peng Cheng Lab \& Xidian University)| |Findings-EMNLP'25|
|2026.02|<a id="SemanticSpec_arXiv_26"></a>[**[SemanticSpec]** Beyond Tokens: Semantic-Aware Speculative Decoding for Efficient Inference by Probing Internal States](https://arxiv.org/abs/2602.03708)|Ximing Dong et al. (@Huawei \& University of Manitoba \& Queen's University)| |arXiv'26|
|2026.04|<a id="SpecGuard_arXiv_26"></a>[**[SpecGuard]** From Tokens to Steps: Verification-Aware Speculative Decoding for Efficient Multi-Step Reasoning](https://arxiv.org/abs/2604.15244)|Kiran Purohit et al. (@IIT Kharagpur \& Adobe Research)| |Findings-ACL'26|

<a id="subclass-2-2"></a>
### 2.2 Intra-Sequence Parallel Generation

*One output is decomposed into structurally independent sections, subproblems, or graph nodes that can be generated concurrently.*

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2023.07|<a id="Skeleton-of-Thought_ICLR_24"></a>[**[Skeleton-of-Thought]** Prompting LLMs for Efficient Parallel Generation](https://arxiv.org/abs/2307.15337)|Xuefei Ning et al. (@THU \& Microsoft \& KU Leuven \& Infinigence-AI)|[[code]](https://github.com/imagination-research/sot)![](https://img.shields.io/github/stars/imagination-research/sot?style=social)|ICLR'24|
|2023.08|<a id="Graph_of_Thoughts_AAAI_24"></a>[**[Graph of Thoughts]** Solving Elaborate Problems with Large Language Models](https://arxiv.org/abs/2308.09687)|Maciej Besta et al. (@ETH Zurich \& Warsaw University of Technology \& Cledar)|[[code]](https://github.com/spcl/graph-of-thoughts)![](https://img.shields.io/github/stars/spcl/graph-of-thoughts?style=social)|AAAI'24|
|2024.02|<a id="Plato_arXiv_24"></a>[**[Plato]** Plan to Efficiently Decode for Large Language Model Inference](https://arxiv.org/abs/2402.12280)|Shuowei Jin et al. (@University of Michigan \& UC Berkeley \& CMU \& Duke \& USC)| |COLM'25|
|2025.02|<a id="PASTA_ICML_25"></a>[**[PASTA]** Learning to Keep a Promise: Scaling Language Model Decoding Parallelism with Learned Asynchronous Decoding](https://arxiv.org/abs/2502.11517)|Tian Jin et al. (@MIT \& Google Research \& Google DeepMind)| |ICML'25|
|2025.03|<a id="Parallel-Decode-in-One-Seq_EMNLP_25"></a>[**Accelerate Parallelizable Reasoning via Parallel Decoding within One Sequence**](https://arxiv.org/abs/2503.20533)|Yijiong Yu et al. (@THU \& OpenCSG \& Oregon State University)|[[code]](https://github.com/yuyijiong/parallel-decoding-in-one-sequence)![](https://img.shields.io/github/stars/yuyijiong/parallel-decoding-in-one-sequence?style=social)|EMNLP'25|
|2025.04|<a id="APR_COLM_25"></a>[**[APR]** Learning Adaptive Parallel Reasoning with Language Models](https://arxiv.org/abs/2504.15466)|Jiayi Pan et al. (@UC Berkeley \& UCSF)|[[code]](https://github.com/Parallel-Reasoning/APR)![](https://img.shields.io/github/stars/Parallel-Reasoning/APR?style=social)|COLM'25|
|2025.06|<a id="Multiverse_NeurIPS_25"></a>[**[Multiverse]** Your Language Models Secretly Decide How to Parallelize and Merge Generation](https://arxiv.org/abs/2506.09991)|Xinyu Yang et al. (@CMU \& NVIDIA)|[[code]](https://github.com/Infini-AI-Lab/Multiverse)![](https://img.shields.io/github/stars/Infini-AI-Lab/Multiverse?style=social)|NeurIPS'25|
|2025.06|<a id="Sprint_arXiv_25"></a>[**[SPRINT]** Enabling Interleaved Planning and Parallelized Execution in Reasoning Models](https://arxiv.org/abs/2506.05745)|Emil Biju et al. (@Stanford \& Microsoft \& Google)|[[code]](https://github.com/ShayanTalaei/SPRINT/tree/main)![](https://img.shields.io/github/stars/ShayanTalaei/SPRINT?style=social)|NeurIPS'25|
|2025.06|<a id="PCCoT_EMNLP_25"></a>[**[PCCoT]** Parallel Continuous Chain-of-Thought with Jacobi Iteration](https://arxiv.org/abs/2506.18582)|Haoyi Wu et al. (@ShanghaiTech)|[[code]](https://github.com/whyNLP/PCCoT)![](https://img.shields.io/github/stars/whyNLP/PCCoT?style=social)|EMNLP'25|
|2025.09|<a id="Parallel-Think-Seq-Answer_arXiv_25"></a>[**Parallel Thinking, Sequential Answering: Bridging NAR and AR for Efficient Reasoning**](https://arxiv.org/abs/2509.20744)|Qihang Ai, Haiyun Jiang (@NTU \& SJTU)| |arXiv'25|
|2025.10|<a id="Parallel_Loop_Transformer_arXiv_25"></a>[**Parallel Loop Transformer for Efficient Test-Time Computation Scaling**](https://arxiv.org/abs/2510.24824)|Bohong Wu et al. (@ByteDance)| |arXiv'25|

<a id="subclass-2-3"></a>
### 2.3 Path Exploration and Test-Time Scaling

*Parallel test-time compute is allocated across branching reasoning paths, with explicit control of exploration, pruning, early termination, and answer aggregation.*

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2024.02|<a id="More_Agents_TMLR_24-sequence"></a>[**More Agents Is All You Need**](https://arxiv.org/abs/2402.05120)|Junyou Li et al. (@Tencent)|[[code]](https://github.com/MoreAgentsIsAllYouNeed/AgentForest)![](https://img.shields.io/github/stars/MoreAgentsIsAllYouNeed/AgentForest?style=social)|TMLR'24|
|2024.10|<a id="Speculative_Rejection_NeurIPS_24"></a>[**[Speculative Rejection]** Fast Best-of-N Decoding via Speculative Rejection](https://arxiv.org/abs/2410.20290)|Hanshi Sun et al. (@CMU \& University of Virginia \& UC Berkeley \& Princeton \& FDU \& Google)|[[code]](https://github.com/Zanette-Labs/SpeculativeRejection)![](https://img.shields.io/github/stars/Zanette-Labs/SpeculativeRejection?style=social)|NeurIPS'24|
|2025.02|<a id="DPTS_ACL_25"></a>[**[DPTS]** Dynamic Parallel Tree Search for Efficient LLM Reasoning](https://arxiv.org/abs/2502.16235)|Yifu Ding et al. (@Beihang University \& NTU \& Wuhan University)|[[code]](https://github.com/yifu-ding/DPTS)![](https://img.shields.io/github/stars/yifu-ding/DPTS?style=social)|ACL'25|
|2025.05|<a id="Group_Think_arXiv_25"></a>[**[Group Think]** Multiple Concurrent Reasoning Agents Collaborating at Token Level Granularity](https://arxiv.org/abs/2505.11107)|Chan-Jan Hsu et al. (@MediaTek Research)| |arXiv'25|
|2025.07|<a id="Adaptive_Termination_arXiv_25"></a>[**[SEAT]** Adaptive Termination for Multi-round Parallel Reasoning: An Universal Semantic Entropy-Guided Framework](https://arxiv.org/abs/2507.06829)|Zenan Xu et al. (@Tencent Hunyuan \& CUHK)| |arXiv'25|
|2025.09|<a id="A2R_arXiv_25"></a>[**[A2R]** An Asymmetric Two-Stage Reasoning Framework for Parallel Reasoning](https://arxiv.org/abs/2509.22044)|Ziqi Wang et al. (@USTC \& Baidu \& USYD)| |arXiv'25|
|2025.09|<a id="Parallel-R1_ICLR_26"></a>[**[Parallel-R1]** Towards Parallel Thinking via Reinforcement Learning](https://arxiv.org/abs/2509.07980)|Tong Zheng et al. (@Tencent AI Lab \& University of Maryland \& UNC Chapel Hill \& CityU \& Washington University in St. Louis)|[[code]](https://github.com/zhengkid/Parallel-R1)![](https://img.shields.io/github/stars/zhengkid/Parallel-R1?style=social)|ICLR'26|
|2025.09|<a id="ATTS_ICLR_26"></a>[**[ATTS]** Asynchronous Test-Time Scaling via Conformal Prediction](https://arxiv.org/abs/2509.15148)|Jing Xiong et al. (@HKU \& Huawei \& UCL \& CUHK)|[[code]](https://github.com/menik1126/asynchronous-test-time-scaling)![](https://img.shields.io/github/stars/menik1126/asynchronous-test-time-scaling?style=social)|ICLR'26|
|2025.10|<a id="Parallel_TTS_for_Latent_ACL_26"></a>[**Parallel Test-Time Scaling for Latent Reasoning Models**](https://arxiv.org/abs/2510.07745)|Runyang You et al. (@PolyU \& USTC \& HIT Shenzhen \& Shandong Jianzhu University)|[[code]](https://github.com/ModalityDance/LatentTTS)![](https://img.shields.io/github/stars/ModalityDance/LatentTTS?style=social)|ACL'26|
|2025.10|<a id="DeepPrune_ACL_26"></a>[**[DeepPrune]** Parallel Scaling without Inter-trace Redundancy](https://arxiv.org/abs/2510.08483)|Shangqing Tu et al. (@THU \& ShanghaiTech University)|[[code]](https://github.com/THU-KEG/DeepPrune)![](https://img.shields.io/github/stars/THU-KEG/DeepPrune?style=social)|Findings-ACL'26|
|2025.11|<a id="DTS_NeurIPS_25_ER"></a>[**[DTS]** Enhancing Large Reasoning Models via Decoding Tree Sketching](https://arxiv.org/abs/2511.00640)|Zicheng Xu et al. (@Johns Hopkins University \& UNC Charlotte \& Rice University \& University of Minnesota)|[[code]](https://github.com/ZichengXu/Decoding-Tree-Sketching)![](https://img.shields.io/github/stars/ZichengXu/Decoding-Tree-Sketching?style=social)|ICML'26|
|2026.02|<a id="Parallel-Probe_ICML_26"></a>[**[Parallel-Probe]** Towards Efficient Parallel Thinking via 2D Probing](https://arxiv.org/abs/2602.03845)|Tong Zheng et al. (@University of Maryland \& Washington University in St. Louis \& UNC Chapel Hill)|[[code]](https://github.com/zhengkid/Parallel-Probe)![](https://img.shields.io/github/stars/zhengkid/Parallel-Probe?style=social)|ICML'26|

<a id="class-3"></a>
## 📙 3. Task-Level Parallel Decoding

Task-level methods coordinate concurrent agents, plans, tool calls, functions, or environment actions.

<a id="subclass-3-1"></a>
### 3.1 Multi-Agent Parallel Collaboration

*Multiple LLM agents or teams solve and deliberate concurrently, then combine their results through voting, synthesis, or coordination.*

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2023.08|<a id="AutoGen_COLM_24"></a>[**[AutoGen]** Enabling Next-Gen LLM Applications via Multi-Agent Conversations](https://arxiv.org/abs/2308.08155)|Qingyun Wu et al. (@Microsoft \& PSU \& UW \& Xidian University)|[[code]](https://github.com/microsoft/autogen)![](https://img.shields.io/github/stars/microsoft/autogen?style=social)|COLM'24|
|2024.02|<a id="More_Agents_TMLR_24"></a>[**More Agents Is All You Need**](https://arxiv.org/abs/2402.05120)|Junyou Li et al. (@Tencent)|[[code]](https://github.com/MoreAgentsIsAllYouNeed/AgentForest)![](https://img.shields.io/github/stars/MoreAgentsIsAllYouNeed/AgentForest?style=social)|TMLR'24|
|2024.06|<a id="Mixture-of-Agents_ICLR_25"></a>[**[Mixture-of-Agents]** Mixture-of-Agents Enhances Large Language Model Capabilities](https://arxiv.org/abs/2406.04692)|Junlin Wang et al. (@Duke University \& Together AI \& University of Chicago \& Stanford University)|[[code]](https://github.com/togethercomputer/moa)![](https://img.shields.io/github/stars/togethercomputer/moa?style=social)|ICLR'25|
|2025.07|<a id="M1-Parallel_ICML_25_MAS"></a>[**[M1-Parallel]** Optimizing Sequential Multi-Step Tasks with Parallel LLM Agents](https://arxiv.org/abs/2507.08944)|Enhao Zhang et al. (@UW \& Microsoft)| |ICMLW'25|

<a id="subclass-3-2"></a>
### 3.2 Parallel and Speculative Agent Execution

*Independent tools, functions, or predicted future actions are planned, dispatched, or speculatively executed in parallel.*

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2023.12|<a id="LLMCompiler_ICML_24"></a>[**[LLMCompiler]** An LLM Compiler for Parallel Function Calling](https://arxiv.org/abs/2312.04511)|Sehoon Kim et al. (@UC Berkeley \& ICSI \& LBNL)|[[code]](https://github.com/SqueezeAILab/LLMCompiler)![](https://img.shields.io/github/stars/SqueezeAILab/LLMCompiler?style=social)|ICML'24|
|2024.05|<a id="LLM-Tool_Compiler_arXiv_24"></a>[**[LLM-Tool Compiler]** An LLM-Tool Compiler for Fused Parallel Function Calling](https://arxiv.org/abs/2405.17438)|Simranjit Singh et al. (@Microsoft \& Southern Illinois University)| |arXiv'24|
|2024.10|<a id="Async_Tool_Usage_arXiv_24"></a>[**Asynchronous Tool Usage for Real-Time Agents**](https://arxiv.org/abs/2410.21620)|Antonio A. Ginart et al. (@Salesforce AI Research)| |arXiv'24|
|2024.12|<a id="AsyncLM_arXiv_24"></a>[**[AsyncLM]** Asynchronous LLM Function Calling](https://arxiv.org/abs/2412.07017)|In Gim, Seung-seob Lee, Lin Zhong (@Yale University)| |arXiv'24|
|2025.09|<a id="DSP_ICLR_26"></a>[**[DSP]** Dynamic Speculative Agent Planning](https://arxiv.org/abs/2509.01920)|Yilin Guan et al. (@Johns Hopkins University \& University of Alberta \& University of British Columbia \& Avey Research Center \& Google \& UCSB)|[[code]](https://github.com/guanyilin428/Dynamic-Speculative-Planning)![](https://img.shields.io/github/stars/guanyilin428/Dynamic-Speculative-Planning?style=social)|ICLR'26|
|2025.10|<a id="Speculative_Actions_ICLR_26"></a>[**[Speculative Actions]** A Lossless Framework for Faster Agentic Systems](https://arxiv.org/abs/2510.04371)|Naimeng Ye et al. (@Columbia University)|[[code]](https://github.com/naimengye/speculative-action)![](https://img.shields.io/github/stars/naimengye/speculative-action?style=social)|ICLR'26|
|2025.12|<a id="Spec_Tool_Calls_arXiv_25"></a>[**Optimizing Agentic Language Model Inference via Speculative Tool Calls**](https://arxiv.org/abs/2512.15834)|Daniel Nichols et al. (@UMD \& LLNL)| |arXiv'25|
|2026.02|<a id="W_D_ICLR_26_AIWILD"></a>[**[W\&D]** Scaling Parallel Tool Calling for Efficient Deep Research Agents](https://arxiv.org/abs/2602.07359)|Xiaoqiang Lin et al. (@Salesforce AI Research)|[[code]](https://github.com/SalesforceAIResearch/MCP-Universe/tree/main/mcpuniverse/benchmark/configs/deepresearch)![](https://img.shields.io/github/stars/SalesforceAIResearch/MCP-Universe?style=social)|ICLRW'26|
|2026.03|<a id="SimpleTool_arXiv_26"></a>[**[RealtimeTool]** RealtimeTool: Parallel Decoding for Real-Time LLM Function Calling](https://openreview.net/forum?id=f0IWycligk)|Xiaoxin Shi et al. (@SJTU \& Shanghai Innovation Institute \& FDU)|[[code]](https://github.com/HaxxorCialtion/SimpleTool)![](https://img.shields.io/github/stars/HaxxorCialtion/SimpleTool?style=social)|ICML'26|
|2026.03|<a id="PASTE_arXiv_26"></a>[**[PASTE]** Parallelizing Tool Execution and LLM Generation for Low-Latency Agent Serving](https://arxiv.org/abs/2603.18897)|Yifan Sui et al. (@SJTU \& Microsoft \& Stevens Institute of Technology \& Google \& HKUST)| |arXiv'26|

<a id="class-4"></a>
## 📙 4. Architecture-Level Parallel Decoding

Architecture-level methods redesign training or generation so strict left-to-right token causality is no longer the only decoding path.

<a id="subclass-4-1"></a>
### 4.1 Model Architecture and Training

*The work designs, trains, converts, or scales architectures that support non-left-to-right generation, including masked diffusion, block diffusion, and semi-autoregressive models.*

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2017.11|<a id="NAT_ICLR_18"></a>[**[NAT]** Non-Autoregressive Neural Machine Translation](https://arxiv.org/abs/1711.02281)|Jiatao Gu et al. (@Salesforce Research \& HKU)|[[code]](https://github.com/salesforce/nonauto-nmt)![](https://img.shields.io/github/stars/salesforce/nonauto-nmt?style=social)|ICLR'18|
|2018.02|<a id="IterRefine_EMNLP_18"></a>[**Deterministic Non-Autoregressive Neural Sequence Modeling by Iterative Refinement**](https://aclanthology.org/D18-1149/)|Jason Lee, Elman Mansimov, Kyunghyun Cho (@New York University \& CIFAR)|[[code]](https://github.com/nyu-dl/dl4mt-nonauto)![](https://img.shields.io/github/stars/nyu-dl/dl4mt-nonauto?style=social)|EMNLP'18|
|2018.03|<a id="Discrete_Latent_ICML_18"></a>[**[Latent Transformer]** Fast Decoding in Sequence Models Using Discrete Latent Variables](https://arxiv.org/abs/1803.03382)|Łukasz Kaiser et al. (@Google Brain)|[[code]](https://github.com/tensorflow/tensor2tensor)![](https://img.shields.io/github/stars/tensorflow/tensor2tensor?style=social)|ICML'18|
|2019.04|<a id="Mask-Predict_EMNLP_19"></a>[**[Mask-Predict]** Parallel Decoding of Conditional Masked Language Models](https://arxiv.org/abs/1904.09324)|Marjan Ghazvininejad et al. (@FAIR)|[[code]](https://github.com/facebookresearch/Mask-Predict)![](https://img.shields.io/github/stars/facebookresearch/Mask-Predict?style=social)|EMNLP'19|
|2021.07|<a id="D3PM_NeurIPS_21"></a>[**[D3PM]** Structured Denoising Diffusion Models in Discrete State-Spaces](https://proceedings.neurips.cc/paper/2021/hash/958c530554f78bcd8e97125b70e6973d-Abstract.html)|Jacob Austin et al. (@Google Research)|[[code]](https://github.com/google-research/google-research/tree/master/d3pm)![](https://img.shields.io/github/stars/google-research/google-research?style=social)|NeurIPS'21|
|2022.05|<a id="DiffusionLM_NeurIPS_22"></a>[**[Diffusion-LM]** Diffusion-LM Improves Controllable Text Generation](https://proceedings.neurips.cc/paper_files/paper/2022/hash/1be5bc25d50895ee656b8c2d9eb89d6a-Abstract.html)|Xiang Lisa Li et al. (@Stanford University)|[[code]](https://github.com/XiangLi1999/Diffusion-LM)![](https://img.shields.io/github/stars/XiangLi1999/Diffusion-LM?style=social)|NeurIPS'22|
|2023.10|<a id="SEDD_arXiv_23"></a>[**[SEDD]** Discrete Diffusion Modeling by Estimating the Ratios of the Data Distribution](https://proceedings.mlr.press/v235/lou24a.html)|Aaron Lou, Chenlin Meng, Stefano Ermon (@Stanford University \& Pika Labs)|[[code]](https://github.com/louaaron/Score-Entropy-Discrete-Diffusion)![](https://img.shields.io/github/stars/louaaron/Score-Entropy-Discrete-Diffusion?style=social)|ICML'24|
|2024.06|<a id="RADD_ICLR_25"></a>[**[RADD]** Your Absorbing Discrete Diffusion Secretly Models the Conditional Distributions of Clean Data](https://openreview.net/forum?id=sMyXP8Tanm)|Jingyang Ou et al. (@RUC \& Huawei)|[[code]](https://github.com/ML-GSAI/RADD)![](https://img.shields.io/github/stars/ML-GSAI/RADD?style=social)|ICLR'25|
|2024.06|<a id="MD4_NeurIPS_24"></a>[**[MD4]** Simplified and Generalized Masked Diffusion for Discrete Data](https://arxiv.org/abs/2406.04329)|Jiaxin Shi et al. (@Google DeepMind)|[[code]](https://github.com/google-deepmind/md4)![](https://img.shields.io/github/stars/google-deepmind/md4?style=social)|NeurIPS'24|
|2024.06|<a id="MDLM_NeurIPS_24"></a>[**[MDLM]** Simple and Effective Masked Diffusion Language Models](https://arxiv.org/abs/2406.07524)|Subham Sekhar Sahoo et al. (@Cornell Tech)|[[code]](https://github.com/kuleshov-group/mdlm)![](https://img.shields.io/github/stars/kuleshov-group/mdlm?style=social)|NeurIPS'24|
|2024.10|<a id="DiffuLLaMA_ICLR_25"></a>[**[DiffuLLaMA]** Scaling Diffusion Language Models via Adaptation from Autoregressive Models](https://arxiv.org/abs/2410.17891)|Shansan Gong et al. (@HKU \& UIUC \& Apple \& Tencent AI Lab)|[[code]](https://github.com/HKUNLP/DiffuLLaMA)![](https://img.shields.io/github/stars/HKUNLP/DiffuLLaMA?style=social)|ICLR'25|
|2024.10|<a id="SDTT_ICLR_25"></a>[**[SDTT]** Beyond Autoregression: Fast LLMs via Self-Distillation Through Time](https://arxiv.org/abs/2410.21035)|Justin Deschenaux, Caglar Gulcehre (@EPFL / CLAIRE Lab)|[[code]](https://github.com/jdeschena/sdtt)![](https://img.shields.io/github/stars/jdeschena/sdtt?style=social)|ICLR'25|
|2025.02|<a id="LLaDA_NeurIPS_25"></a>[**[LLaDA]** Large Language Diffusion Models](https://arxiv.org/abs/2502.09992)|Shen Nie et al. (@Renmin University of China \& Ant Group)|[[code]](https://github.com/ML-GSAI/LLaDA)![](https://img.shields.io/github/stars/ML-GSAI/LLaDA?style=social)|NeurIPS'25|
|2025.03|<a id="Block_Diffusion_ICLR_25"></a>[**[Block Diffusion]** Interpolating Between Autoregressive and Diffusion Language Models](https://arxiv.org/abs/2503.09573)|Marianne Arriola et al. (@Cornell Tech \& Stanford \& Cohere)|[[code]](https://github.com/kuleshov-group/bd3lms)![](https://img.shields.io/github/stars/kuleshov-group/bd3lms?style=social)|ICLR'25|
|2025.06|<a id="Duo_ICML_25"></a>[**[Duo]** The Diffusion Duality](https://proceedings.mlr.press/v267/sahoo25a.html)|Subham Sekhar Sahoo et al. (@Cornell Tech \& EPFL \& Cohere)|[[code]](https://github.com/s-sahoo/duo)![](https://img.shields.io/github/stars/s-sahoo/duo?style=social)|ICML'25|
|2025.08|<a id="Dream_7B_arXiv_25"></a>[**[Dream 7B]** Diffusion Large Language Models](https://arxiv.org/abs/2508.15487)|Jiacheng Ye et al. (@HKU \& Huawei)|[[code]](https://github.com/HKUNLP/Dream)![](https://img.shields.io/github/stars/HKUNLP/Dream?style=social)|arXiv'25|
|2025.09|<a id="SDLM_arXiv_25"></a>[**[SDLM]** Sequential Diffusion Language Models](https://arxiv.org/abs/2509.24007)|Yangzhou Liu et al. (@Shanghai AI Lab \& NJU \& THU \& FDU \& CUHK \& Soochow University \& Donghua University)|[[code]](https://github.com/OpenGVLab/SDLM)![](https://img.shields.io/github/stars/OpenGVLab/SDLM?style=social)|arXiv'25|
|2025.09|<a id="Fast-dLLM_v2_ICLR_26"></a>[**[Fast-dLLM v2]** Efficient Block-Diffusion LLM](https://arxiv.org/abs/2509.26328)|Chengyue Wu et al. (@HKU \& NVIDIA \& MIT)|[[code]](https://github.com/NVlabs/Fast-dLLM)![](https://img.shields.io/github/stars/NVlabs/Fast-dLLM?style=social)|ICLR'26|
|2025.10|<a id="RND1_blog"></a>[**[RND1]** Training Diffusion Language Models at Scale using Autoregressive Models](https://www.radicalnumerics.ai/assets/rnd1_report.pdf)|Radical Numerics Inc. (@Radical Numerics Inc.)|[[code]](https://github.com/RadicalNumerics/RND1)![](https://img.shields.io/github/stars/RadicalNumerics/RND1?style=social)|misc'25|
|2025.10|<a id="E2D2_NeurIPS_25"></a>[**[E2D2]** Encoder-Decoder Diffusion Language Models for Efficient Training and Inference](https://arxiv.org/abs/2510.22852)|Marianne Arriola et al. (@Cornell University)|[[code]](https://github.com/kuleshov-group/e2d2)![](https://img.shields.io/github/stars/kuleshov-group/e2d2?style=social)|NeurIPS'25|
|2025.10|<a id="SDAR_arXiv_25"></a>[**[SDAR]** A Synergistic Diffusion-AutoRegression Paradigm for Scalable Sequence Generation](https://arxiv.org/abs/2510.06303)|Shuang Cheng et al. (@Shanghai AI Lab \& ZJU \& UMD \& SJTU \& THU)|[[code]](https://github.com/JetAstra/SDAR)![](https://img.shields.io/github/stars/JetAstra/SDAR?style=social)|Findings-ACL'26|
|2025.12|<a id="NBDiff_arXiv_25"></a>[**[NBDiff]** From Next-Token to Next-Block: A Principled Adaptation Path for Diffusion LLMs](https://arxiv.org/abs/2512.06776)|Yuchuan Tian et al. (@PKU \& Huawei)|[[code]](https://github.com/YuchuanTian/NBDiff)![](https://img.shields.io/github/stars/YuchuanTian/NBDiff?style=social)|arXiv'25|
|2025.12|<a id="Efficient-DLM_arXiv_25"></a>[**[Efficient-DLM]** From Autoregressive to Diffusion Language Models, and Beyond in Speed](https://arxiv.org/abs/2512.14067)|Yonggan Fu et al. (@NVIDIA \& Georgia Tech \& UChicago \& HKU \& MIT)| |ICML'26|
|2025.12|<a id="LLaDA2.0_arXiv_25"></a>[**[LLaDA2.0]** Scaling Up Diffusion Language Models to 100B](https://arxiv.org/abs/2512.15745)|Tiwei Bie et al. (@Ant Group \& Renmin University of China \& ZJU \& Westlake University \& HKUST)|[[code]](https://github.com/inclusionAI/LLaDA2.0)![](https://img.shields.io/github/stars/inclusionAI/LLaDA2.0?style=social)|arXiv'25|
|2026.01|<a id="Diffusion-in-Diffusion_arXiv_26"></a>[**[Diffusion In Diffusion]** Reclaiming Global Coherence in Semi-Autoregressive Diffusion](https://arxiv.org/abs/2601.13599)|Linrui Ma et al. (@Huawei)| |arXiv'26|
|2026.06|<a id="MBD-LMs_arXiv_26"></a>[**[MBD-LMs]** Multi-Block Diffusion Language Models](https://arxiv.org/abs/2606.29215)|Yijie Jin et al. (@SJTU \& XJTU \& Huawei)|[[code]](https://github.com/SJTU-DENG-Lab/mbd-lms)![](https://img.shields.io/github/stars/SJTU-DENG-Lab/mbd-lms?style=social)|arXiv'26|

<a id="subclass-4-2"></a>
### 4.2 Decoding and Sampling Algorithms

*Inference algorithms accelerate trained non-autoregressive or diffusion language models through unmasking policies, confidence schedules, trajectory methods, or error correction.*

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2025.05|<a id="EB-Sampler_NeurIPS_25"></a>[**[EB-Sampler]** Accelerated Sampling from Masked Diffusion Models via Entropy Bounded Unmasking](https://arxiv.org/abs/2505.24857)|Heli Ben-Hamu et al. (@Meta)| |NeurIPS'25|
|2025.05|<a id="Fast-dLLM_ICLR_26"></a>[**[Fast-dLLM]** Training-free Acceleration of Diffusion LLM by Enabling KV Cache and Parallel Decoding](https://arxiv.org/abs/2505.22618)|Chengyue Wu et al. (@HKU \& NVIDIA \& MIT)|[[code]](https://github.com/NVlabs/Fast-dLLM)![](https://img.shields.io/github/stars/NVlabs/Fast-dLLM?style=social)|ICLR'26|
|2025.06|<a id="SlowFast_Sampling_ICLR_26"></a>[**[SlowFast Sampling]** Accelerating Diffusion Large Language Models with SlowFast Sampling: The Three Golden Principles](https://arxiv.org/abs/2506.10848)|Qingyan Wei et al. (@SJTU \& Shanghai AI Lab)|[[code]](https://github.com/LiangrunFlora/Slow-Fast-Sampling)![](https://img.shields.io/github/stars/LiangrunFlora/Slow-Fast-Sampling?style=social)|ICLR'26|
|2025.07|<a id="WINO_ICLR_26"></a>[**[WINO]** Wide-In, Narrow-Out: Revokable Decoding for Efficient and Effective DLLMs](https://arxiv.org/abs/2507.18578)|Feng Hong et al. (@SJTU \& Apple)|[[code]](https://github.com/Feng-Hong/WINO-DLLM)![](https://img.shields.io/github/stars/Feng-Hong/WINO-DLLM?style=social)|ICLR'26|
|2025.08|<a id="Prophet_ICLR_26"></a>[**[Prophet]** Diffusion Language Models Know the Answer Before Decoding](https://arxiv.org/abs/2508.19982)|Pengxiang Li et al. (@PolyU HK \& Dartmouth \& Surrey \& Sun Yat-sen University \& ELLIS Institute Tübingen \& MPI-IS \& Tübingen AI Center)|[[code]](https://github.com/pixeli99/Prophet)![](https://img.shields.io/github/stars/pixeli99/Prophet?style=social)|ICLR'26|
|2025.09|<a id="RWS_EMNLP_25"></a>[**[RWS]** Reward-Weighted Sampling: Enhancing Non-Autoregressive Characteristics in Masked Diffusion LLMs](https://arxiv.org/abs/2509.00707)|Daehoon Gwak et al. (@KAIST \& SKKU)| |EMNLP'25|
|2025.09|<a id="LSD_NeurIPS_25"></a>[**[LSD]** Learnable Sampler Distillation for Discrete Diffusion Models](https://arxiv.org/abs/2509.19962)|Feiyang Fu et al. (@UESTC)|[[code]](https://github.com/feiyangfu/LSD)![](https://img.shields.io/github/stars/feiyangfu/LSD?style=social)|NeurIPS'25|
|2025.09|<a id="ADJUST_arXiv_25"></a>[**[ADJUST]** Enabling Approximate Joint Sampling in Diffusion LMs](https://arxiv.org/abs/2509.22738)|Parikshit Bansal et al. (@UT-Austin)| |arXiv'25|
|2025.09|<a id="Learn2PD_ICLR_26"></a>[**[Learn2PD]** Learning to Parallel: Accelerating Diffusion Large Language Models via Learnable Parallel Decoding](https://arxiv.org/abs/2509.25188)|Wenrui Bao et al. (@UCF \& Mobi.AI \& HKUST)|[[code]](https://github.com/ims-kdks/Learning-to-Parallel-Decoding)![](https://img.shields.io/github/stars/ims-kdks/Learning-to-Parallel-Decoding?style=social)|ICLR'26|
|2025.09|<a id="dParallel_ICLR_26"></a>[**[dParallel]** Learnable Parallel Decoding for dLLMs](https://arxiv.org/abs/2509.26488)|Zigeng Chen et al. (@NUS)|[[code]](https://github.com/czg1225/dParallel)![](https://img.shields.io/github/stars/czg1225/dParallel?style=social)|ICLR'26|
|2025.10|<a id="FreeDave_arXiv_25"></a>[**[FreeDave]** Free Draft-and-Verification: Toward Lossless Parallel Decoding for Diffusion Large Language Models](https://arxiv.org/abs/2510.00294)|Shutong Wu et al. (@UW-Madison)|[[code]](https://github.com/cychomatica/FreeDave)![](https://img.shields.io/github/stars/cychomatica/FreeDave?style=social)|NeurIPSW'25|
|2025.10|<a id="LocalLeap_arXiv_25"></a>[**[LocalLeap]** Accelerating Diffusion LLM Inference via Local Determinism Propagation](https://arxiv.org/abs/2510.07081)|Fanheng Kong et al. (@Kuaishou)|[[code]](https://github.com/friedrichor/LocalLeap)![](https://img.shields.io/github/stars/friedrichor/LocalLeap?style=social)|arXiv'25|
|2025.10|<a id="Saber_arXiv_25"></a>[**[Saber]** Efficient Sampling with Adaptive Acceleration and Backtracking Enhanced Remasking for Diffusion Language Model in Code Generation](https://aclanthology.org/2026.acl-long.165/)|Yihong Dong et al. (@PKU)|[[code]](https://github.com/zhaoyMa/Saber)![](https://img.shields.io/github/stars/zhaoyMa/Saber?style=social)|ACL'26|
|2025.12|<a id="LoPA_arXiv_25"></a>[**[LoPA]** Scaling dLLM Inference via Lookahead Parallel Decoding](https://arxiv.org/abs/2512.16229)|Chenkai Xu et al. (@SJTU \& Huawei)|[[code]](https://github.com/SJTU-DENG-Lab/LoPA)![](https://img.shields.io/github/stars/SJTU-DENG-Lab/LoPA?style=social)|arXiv'25|
|2025.12|<a id="SchED_arXiv_25"></a>[**[SchED]** Fast-Decoding Diffusion Language Models via Progress-Aware Confidence Schedules](https://arxiv.org/abs/2512.02892)|Amr Mohamed et al. (@MBZUAI \& Ecole Polytechnique)|[[code]](https://github.com/amr-mohamedd/SchED)![](https://img.shields.io/github/stars/amr-mohamedd/SchED?style=social)|Findings-ACL'26|
|2025.12|<a id="CadLLM_arXiv_25"></a>[**[CadLLM]** Improving the Throughput of Diffusion-based Large Language Models via a Training-Free Confidence-Aware Calibration](https://arxiv.org/abs/2512.07173)|Jucheng Shen et al. (@Rice \& Intel \& UT-Austin)|[[code]](https://github.com/juchengshen/CadLLM)![](https://img.shields.io/github/stars/juchengshen/CadLLM?style=social)|Findings-ACL'26|
|2025.12|<a id="Learning_Unmasking_Policies_ICML_26"></a>[**Learning Unmasking Policies for Diffusion Language Models**](https://arxiv.org/abs/2512.09106)|Metod Jazbec et al. (@Apple \& UvA \& MIT)|[[code]](https://github.com/apple/ml-rl-dllm)![](https://img.shields.io/github/stars/apple/ml-rl-dllm?style=social)|ICML'26|
|2026.01|<a id="Order-Token_Search_arXiv_26"></a>[**[Order-Token Search]** Improving Diffusion Language Model Decoding through Joint Search in Generation Order and Token Space](https://arxiv.org/abs/2601.20339)|Yangyi Shen et al. (@Stanford \& ZJU)| |arXiv'26|
|2026.01|<a id="d3LLM_ICML_26"></a>[**[d3LLM]** Ultra-Fast Diffusion LLM using Pseudo-Trajectory Distillation](https://arxiv.org/abs/2601.07568)|Yu-Yang Qian et al. (@UCSD \& NJU \& SJTU)|[[code]](https://github.com/hao-ai-lab/d3LLM)![](https://img.shields.io/github/stars/hao-ai-lab/d3LLM?style=social)|ICML'26|
|2026.02|<a id="RDD_arXiv_26"></a>[**[RDD]** Reversible Diffusion Decoding for Diffusion Language Models](https://arxiv.org/abs/2602.00150)|Xinyun Wang et al. (@ECNU \& THU \& Oxford \& ZJU \& Rakuten Singapore)| |arXiv'26|
|2026.02|<a id="ReMix_CVPR_26"></a>[**[ReMix]** Rejection Mixing: Fast Semantic Propagation of Mask Tokens for Efficient DLLM Inference](https://openaccess.thecvf.com/content/CVPR2026/papers/Ye_Rejection_Mixing_Fast_Semantic_Propagation_of_Mask_Tokens_for_Efficient_CVPR_2026_paper.pdf)|Yushi Ye et al. (@SJTU \& Apple)|[[code]](https://github.com/Serpientw/ReMix-DLLM)![](https://img.shields.io/github/stars/Serpientw/ReMix-DLLM?style=social)|CVPR'26|
|2026.02|<a id="Info-Gain_Sampler_ICML_26"></a>[**[Info-Gain Sampler]** Improving Sampling for Masked Diffusion Models via Information Gain](https://arxiv.org/abs/2602.18176)|Kaisen Yang et al. (@THU \& MIT \& SJTU \& Beihang University)|[[code]](https://github.com/yks23/Information-Gain-Sampler)![](https://img.shields.io/github/stars/yks23/Information-Gain-Sampler?style=social)|ICML'26|
|2026.03|<a id="DOS_arXiv_26"></a>[**[DOS]** Dependency-Oriented Sampler for Masked Diffusion Language Models](https://arxiv.org/abs/2603.15340)|Xueyu Zhou et al. (@PolyU HK)| |Findings-ACL'26|
|2026.03|<a id="Confidence-Based_Decoding_arXiv_26"></a>[**Confidence-Based Decoding is Provably Efficient for Diffusion Language Models**](https://arxiv.org/abs/2603.22248)|Changxiao Cai et al. (@UMich \& CUHK)| |arXiv'26|

<a id="class-5"></a>
## 📙 5. System and Infrastructure

System and infrastructure work turns algorithmic parallelism into reliable latency, throughput, memory, and SLO gains through serving, scheduling, and algorithm–runtime co-design.

<a id="subclass-5-1"></a>
### 5.1 Serving and Scheduling

*Runtime and serving techniques optimize batching, GPU scheduling, pipelines, distributed execution, caching, or SLO-aware adaptation for parallel decoding.*

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2024.04|<a id="TriForce_COLM_24"></a>[**[TriForce]** Lossless Acceleration of Long Sequence Generation with Hierarchical Speculative Decoding](https://arxiv.org/abs/2404.11912)|Hanshi Sun et al. (@CMU \& FAIR)|[[code]](https://github.com/Infini-AI-Lab/TriForce)![](https://img.shields.io/github/stars/Infini-AI-Lab/TriForce?style=social)|COLM'24|
|2024.04|<a id="BASS_ACL_24"></a>[**[BASS]** Batched Attention-optimized Speculative Sampling](https://arxiv.org/abs/2404.15778)|Haifeng Qian et al. (@AWS)| |Findings-ACL'24|
|2024.05|<a id="DSI_ICLR_25"></a>[**[DSI]** Distributed Speculative Inference (DSI): Speculation Parallelism for Provably Faster Lossless Language Model Inference](https://arxiv.org/abs/2405.14105)|Nadav Timor et al. (@Weizmann \& Intel \& Texas A\&M University)|[[code]](https://github.com/keyboardAnt/distributed-speculative-inference)![](https://img.shields.io/github/stars/keyboardAnt/distributed-speculative-inference?style=social)|ICLR'25|
|2024.05|<a id="EMS-SD_NAACL_25"></a>[**[EMS-SD]** Efficient Multi-sample Speculative Decoding for Accelerating Large Language Models](https://arxiv.org/abs/2405.07542)|Yunsheng Ni et al. (@Huawei)|[[code]](https://github.com/niyunsheng/EMS-SD)![](https://img.shields.io/github/stars/niyunsheng/EMS-SD?style=social)|NAACL'25|
|2024.06|<a id="SpecExec_NeurIPS_24"></a>[**[SpecExec]** Massively Parallel Speculative Decoding For Interactive LLM Inference on Consumer Devices](https://arxiv.org/abs/2406.02532)|Ruslan Svirschevski et al. (@Yandex \& HSE \& Together AI \& CMU \& Meta AI)|[[code]](https://github.com/yandex-research/specexec)![](https://img.shields.io/github/stars/yandex-research/specexec?style=social)|NeurIPS'24|
|2024.06|<a id="GPU-Opt_SS_EMNLP_24"></a>[**Optimized Speculative Sampling for GPU Hardware Accelerators**](https://arxiv.org/abs/2406.11016)|Dominik Wagner et al. (@TH Nürnberg \& KAIST)|[[code]](https://github.com/dwgnr/optimized-speculative-sampling)![](https://img.shields.io/github/stars/dwgnr/optimized-speculative-sampling?style=social)|EMNLP'24|
|2024.08|<a id="MagicDec_ICLR_25"></a>[**[MagicDec]** Breaking the Latency-Throughput Tradeoff for Long Context Generation with Speculative Decoding](https://arxiv.org/abs/2408.11049)|Ranajoy Sadhukhan et al. (@CMU \& Moffett AI \& Together AI)|[[code]](https://github.com/Infini-AI-Lab/MagicDec)![](https://img.shields.io/github/stars/Infini-AI-Lab/MagicDec?style=social)|ICLR'25|
|2024.08|<a id="PEARL_ICLR_25"></a>[**[PEARL]** Parallel Speculative Decoding with Adaptive Draft Length](https://arxiv.org/abs/2408.11850)|Tianyu Liu et al. (@USTC \& Tencent \& Shanghai AI Lab)|[[code]](https://github.com/smart-lty/ParallelSpeculativeDecoding)![](https://img.shields.io/github/stars/smart-lty/ParallelSpeculativeDecoding?style=social)|ICLR'25|
|2024.12|<a id="Dovetail_EMNLP_25"></a>[**[Dovetail]** A CPU/GPU Heterogeneous Speculative Decoding for LLM inference](https://arxiv.org/abs/2412.18934)|Libo Zhang et al. (@NUDT)|[[code]](https://github.com/ddInference/Dovetail)![](https://img.shields.io/github/stars/ddInference/Dovetail?style=social)|EMNLP'25|
|2025.01|<a id="Attention-Level_Speculation_ICML_25"></a>[**[ALSpec] Attention-Level Speculation**](https://proceedings.mlr.press/v267/cai25g.html)|Jack Cai et al. (@University of Toronto \& Tenstorrent \& University of Waterloo)|[[code]](https://github.com/mcj-group/alspec)![](https://img.shields.io/github/stars/mcj-group/alspec?style=social)|ICML'25|
|2025.02|<a id="EasySpec_NeurIPS_25"></a>[**[EasySpec]** Layer-Parallel Speculative Decoding for Efficient Multi-GPU Utilization](https://arxiv.org/abs/2502.02493)|Yize Wu et al. (@CAS \& UCAS)|[[code]](https://github.com/Yize-Wu/EasySpec)![](https://img.shields.io/github/stars/Yize-Wu/EasySpec?style=social)|NeurIPS'25|
|2025.03|<a id="AdaSpec_SoCC_25"></a>[**[AdaSpec]** Adaptive Speculative Decoding for Fast, SLO-Aware Large Language Model Serving](https://arxiv.org/abs/2503.05096)|Kaiyu Huang et al. (@Tongji University \& HUST \& CUHK Shenzhen)|[[code]](https://github.com/cerebellumking/AdaSpec)![](https://img.shields.io/github/stars/cerebellumking/AdaSpec?style=social)|SoCC'25|
|2025.09|<a id="Substitute_SD_NeurIPS_25"></a>[**[SubSpec]** Speculate Deep and Accurate: Lossless and Training-Free Acceleration for Offloaded LLMs via Substitute Speculative Decoding](https://arxiv.org/abs/2509.18344)|Pei-Shuo Wang et al. (@NYCU \& Cornell)|[[code]](https://github.com/NYCU-EDgeAi/subspec)![](https://img.shields.io/github/stars/NYCU-EDgeAi/subspec?style=social)|NeurIPS'25|
|2025.10|<a id="Mirror-SD_arXiv_25"></a>[**[Mirror-SD]** Mirror Speculative Decoding: Breaking the Serial Barrier in LLM Inference](https://arxiv.org/abs/2510.13161)|Nikhil Bhendawade et al. (@Apple)| |arXiv'25|
|2025.10|<a id="dInfer"></a>[**[dInfer]** An Efficient Inference Framework for Diffusion Language Models](https://arxiv.org/abs/2510.08666)|Yuxin Ma et al. (@Ant Group \& ZJU \& Westlake University \& RUC \& UCAS \& SJTU)|[[code]](https://github.com/inclusionAI/dInfer)![](https://img.shields.io/github/stars/inclusionAI/dInfer?style=social)|arXiv'25|
|2025.12|<a id="dLLM-Serve"></a>[**[dLLM-Serve]** Taming the Memory Footprint Crisis: System Design for Production Diffusion LLM Serving](https://arxiv.org/abs/2512.17077)|Jiakun Fan et al. (@Virginia Tech)|[[code]](https://github.com/chosen-ox/dLLM-Serve)![](https://img.shields.io/github/stars/chosen-ox/dLLM-Serve?style=social)|arXiv'25|

<a id="subclass-5-2"></a>
### 5.2 Algorithm-Infrastructure Co-Design

*The decoding algorithm is co-designed with deployment constraints or systems techniques such as quantization, KV-cache management, MoE execution, and hardware-aware verification.*

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2024.04|<a id="DeFT_ICLR_25"></a>[**[DeFT]** Decoding with Flash Tree-attention for Efficient Tree-structured LLM Inference](https://arxiv.org/abs/2404.00242)|Jinwei Yao et al. (@Westlake University \& ZJU \& CMU \& UIUC \& HKUST)|[[code]](https://github.com/LINs-lab/DeFT)![](https://img.shields.io/github/stars/LINs-lab/DeFT?style=social)|ICLR'25|
|2024.10|<a id="QSpec_EMNLP_25"></a>[**[QSpec]** Speculative Decoding with Complementary Quantization Schemes](https://arxiv.org/abs/2410.11305)|Juntao Zhao et al. (@HKU)|[[code]](https://github.com/hku-netexplo-lab/QSpec)![](https://img.shields.io/github/stars/hku-netexplo-lab/QSpec?style=social)|EMNLP'25|
|2025.02|<a id="QuantSpec_ICML_25"></a>[**[QuantSpec]** Self-Speculative Decoding with Hierarchical Quantized KV Cache](https://arxiv.org/abs/2502.10424)|Rishabh Tiwari et al. (@UC Berkeley \& Apple \& ICSI \& LBNL)|[[code]](https://github.com/SqueezeAILab/QuantSpec)![](https://img.shields.io/github/stars/SqueezeAILab/QuantSpec?style=social)|ICML'25|
|2025.02|<a id="Speculative_Prefill_ICML_25"></a>[**[SpecPrefill]** Speculative Prefill: Turbocharging TTFT with Lightweight and Training-Free Token Importance Estimation](https://arxiv.org/abs/2502.02789)|Jingyu Liu et al. (@University of Chicago \& CMU)|[[code]](https://github.com/Jingyu6/speculative_prefill)![](https://img.shields.io/github/stars/Jingyu6/speculative_prefill?style=social)|ICML'25|
|2025.03|<a id="ML-SpecQD_arXiv_25"></a>[**[ML-SpecQD]** Multi-Level Speculative Decoding with Quantized Drafts](https://arxiv.org/abs/2503.13565)|Evangelos Georganas et al. (@Intel)| |arXiv'25|
|2025.03|<a id="SpeCache_ICML_25"></a>[**[SpeCache]** Speculative Key-Value Caching for Efficient Generation of LLMs](https://arxiv.org/abs/2503.16163)|Shibo Jie et al. (@PKU \& Huawei)| |ICML'25|
|2025.05|<a id="MoESD_NeurIPS_25"></a>[**[MoESD]** Unveil Speculative Decoding's Potential for Accelerating Sparse MoE](https://arxiv.org/abs/2505.19645)|Zongle Huang et al. (@THU \& Huawei)| |NeurIPS'25|
|2025.06|<a id="Utility-Driven_MoE-SD_arXiv_25"></a>[**[Cascade]** Utility-Driven Speculative Decoding for Mixture-of-Experts](https://arxiv.org/abs/2506.20675)|Anish Saxena et al. (@Georgia Tech \& NVIDIA)| |arXiv'25|
|2025.10|<a id="AsyncSpade_arXiv_25"></a>[**[AsyncSpade]** Efficient Test-Time Scaling with Asynchronous Sparse Decoding](https://arxiv.org/abs/2510.07486)|Shuqing Luo et al. (@UNC Chapel Hill \& Johns Hopkins \& UCLA)|[[repo (code coming soon)]](https://github.com/UNITES-Lab/AsyncSpade)![](https://img.shields.io/github/stars/UNITES-Lab/AsyncSpade?style=social)|ICML'26|
|2025.12|<a id="Yggdrasil_NeurIPS_25"></a>[**[Yggdrasil]** Bridging Dynamic Speculation and Static Runtime for Latency-Optimal Tree-Based LLM Decoding](https://arxiv.org/abs/2512.23858)|Yue Guan, Changming Yu et al. (@SJTU \& Qizhi Institute \& UCSD)| |NeurIPS'25|

### Related Serving Engines and Libraries

These software projects support speculative or parallel decoding but are not paper records in the taxonomy, so they are not included in the classified-paper counts above.

|Name|Type|Documented Support|Code|
|:---|:---|:---|:---:|
|<a id="vLLM"></a>[**vLLM**](https://docs.vllm.ai/en/latest/features/speculative_decoding.html)|Serving engine|EAGLE / EAGLE-3, MTP, DFlash, PARD, MLP, draft-model SD, n-gram, suffix decoding|[[code]](https://github.com/vllm-project/vllm)![](https://img.shields.io/github/stars/vllm-project/vllm?style=social)|
|<a id="SGLang"></a>[**SGLang**](https://docs.sglang.ai/advanced_features/speculative_decoding.html)|Serving engine|EAGLE-2 / EAGLE-3, MTP, DFlash, draft-model SD, n-gram|[[code]](https://github.com/sgl-project/sglang)![](https://img.shields.io/github/stars/sgl-project/sglang?style=social)|
|<a id="TensorRT-LLM"></a>[**TensorRT-LLM**](https://nvidia.github.io/TensorRT-LLM/advanced/speculative-decoding.html)|Serving engine|Medusa, EAGLE, ReDrafter, n-gram, draft-model SD, lookahead decoding|[[code]](https://github.com/NVIDIA/TensorRT-LLM)![](https://img.shields.io/github/stars/NVIDIA/TensorRT-LLM?style=social)|
|<a id="TGI"></a>[**TGI**](https://huggingface.co/docs/text-generation-inference/conceptual/speculation)|Serving engine|Medusa, n-gram speculation|[[code]](https://github.com/huggingface/text-generation-inference)![](https://img.shields.io/github/stars/huggingface/text-generation-inference?style=social)|
|<a id="llama.cpp"></a>[**llama.cpp**](https://github.com/ggml-org/llama.cpp/blob/master/docs/speculative.md)|Serving engine|Draft-model SD, EAGLE-3, DFlash, DSpark, MTP, n-gram cache/map|[[code]](https://github.com/ggml-org/llama.cpp)![](https://img.shields.io/github/stars/ggml-org/llama.cpp?style=social)|
|<a id="NVIDIA_Triton"></a>[**NVIDIA Triton**](https://github.com/triton-inference-server/tensorrtllm_backend)|Serving engine|Medusa, ReDrafter, Lookahead, EAGLE / EAGLE-3, draft-model SD, n-gram|[[code]](https://github.com/triton-inference-server/server)![](https://img.shields.io/github/stars/triton-inference-server/server?style=social)|
|<a id="Speculators"></a>[**Speculators**](https://github.com/vllm-project/speculators)|Library|Unified abstraction \& training for SD drafters (vLLM-native)|[[code]](https://github.com/vllm-project/speculators)![](https://img.shields.io/github/stars/vllm-project/speculators?style=social)|
|<a id="TorchSpec"></a>[**TorchSpec**](https://pytorch.org/blog/torchspec-speculative-decoding-training-at-scale/)|Training framework|PyTorch-native large-scale SD drafter training|[[code]](https://github.com/lightseekorg/TorchSpec)![](https://img.shields.io/github/stars/lightseekorg/TorchSpec?style=social)|

<a id="class-6"></a>
## 📙 6. Hybrid Methods

Hybrid methods make the deliberate combination of multiple decoding or generative paradigms their central contribution.

<a id="subclass-6-1"></a>
### 6.1 Non-AR Drafter for AR Targets

*A non-autoregressive or semi-autoregressive language model drafts multiple tokens in parallel for verification by an autoregressive target.*

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2024.08|<a id="SpecDiff_NAACL_25"></a>[**[SpecDiff]** Speculative Diffusion Decoding: Accelerating Language Generation through Diffusion](https://arxiv.org/abs/2408.05636)|Jacob K. Christopher et al. (@University of Virginia \& LLNL)| |NAACL'25|
|2025.10|<a id="DiffuSpec_arXiv_25"></a>[**[DiffuSpec]** Unlocking Diffusion Language Models for Speculative Decoding](https://arxiv.org/abs/2510.02358)|Guanghao Li, Zhihui Fu et al. (@THU \& SUSTech \& OPPO Research Institute)|[[code (unofficial)]](https://github.com/pluto0x0/DiffuSpec/tree/main)![](https://img.shields.io/github/stars/pluto0x0/DiffuSpec?style=social)|Findings-ACL'26|
|2025.11|<a id="SpecDiff-2_arXiv_25"></a>[**[SpecDiff-2]** Scaling Diffusion Drafter Alignment For Faster Speculative Decoding](https://proceedings.mlsys.org/paper_files/paper/2026/hash/041dad5ed2191b44ba3ed0e00cdc3187-Abstract-Conference.html)|Jameson Sandler et al. (@University of Virginia)| |MLSys'26|
|2025.12|<a id="DEER_arXiv_25"></a>[**[DEER]** Draft with Diffusion, Verify with Autoregressive Models](https://arxiv.org/abs/2512.15176)|Zicong Cheng, Guo-Wei Yang et al. (@THU \& Proxseer Inc \& SJTU)|[[repo (code coming soon)]](https://github.com/czc726/DEER)![](https://img.shields.io/github/stars/czc726/DEER?style=social)|arXiv'25|
|2026.01|<a id="DART_arXiv_26"></a>[**[DART]** Diffusion-Inspired Speculative Decoding for Fast LLM Inference](https://arxiv.org/abs/2601.19278)|Fuliang Liu, Xue Li et al. (@NJU \& Alibaba)|[[code]](https://github.com/fvliang/DART)![](https://img.shields.io/github/stars/fvliang/DART?style=social)|arXiv'26|
|2026.02|<a id="DFlash_ICML_26"></a>[**[DFlash]** Block Diffusion for Flash Speculative Decoding](https://arxiv.org/abs/2602.06036)|Jian Chen, Yesheng Liang, Zhijian Liu (@UCSD)|[[code]](https://github.com/z-lab/dflash)![](https://img.shields.io/github/stars/z-lab/dflash?style=social)|ICML'26|
|2026.06|<a id="JetSpec_arXiv_26-hybrid"></a>[**[JetSpec]** Breaking the Scaling Ceiling of Speculative Decoding with Parallel Tree Drafting](https://arxiv.org/abs/2606.18394)|Lanxiang Hu et al. (@UCSD \& NJU \& StepFun)|[[code]](https://github.com/hao-ai-lab/JetSpec)![](https://img.shields.io/github/stars/hao-ai-lab/JetSpec?style=social)|arXiv'26|
|2026.07|<a id="DSpark_26"></a>[**[DSpark]** Confidence-Scheduled Speculative Decoding with Semi-Autoregressive Generation](https://arxiv.org/abs/2607.05147)|Xin Cheng et al. (@DeepSeek-AI \& PKU)|[[code]](https://github.com/deepseek-ai/DeepSpec)![](https://img.shields.io/github/stars/deepseek-ai/DeepSpec?style=social)|arXiv'26|
|2026.07|<a id="AdaFlash_arXiv_26"></a>[**[AdaFlash]** Adaptive Speculative Decoding via On-Policy Distilled Diffusion Drafters](https://arxiv.org/abs/2607.19223)|Yu-Yang Qian et al. (@NJU \& Huawei)| |arXiv'26|

<a id="subclass-6-2"></a>
### 6.2 Speculation for Other Generative Paradigms

*The draft-then-verify principle is transferred to diffusion language models, model cascades, continuous diffusion models, and visual autoregressive generation.*

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2023.12|<a id="Cascade_SD_NeurIPS_24"></a>[**[CS Drafting]** Cascade Speculative Drafting for Even Faster LLM Inference](https://arxiv.org/abs/2312.11462)|Ziyi Chen et al. (@UIUC)|[[code]](https://github.com/lfsszd/CS-Drafting)![](https://img.shields.io/github/stars/lfsszd/CS-Drafting?style=social)|NeurIPS'24|
|2024.05|<a id="Faster_Cascades_ICLR_25"></a>[**Faster Cascades via Speculative Decoding**](https://arxiv.org/abs/2405.19261)|Harikrishna Narasimhan et al. (@Google Research \& Google DeepMind \& Mistral AI)| |ICLR'25|
|2024.10|<a id="Spec_Jacobi_T2I_ICLR_25"></a>[**[SJD]** Accelerating Auto-regressive Text-to-Image Generation with Training-free Speculative Jacobi Decoding](https://arxiv.org/abs/2410.01699)|Yao Teng, Han Shi et al. (@HKU \& Huawei \& CUHK \& THU \& SJTU \& Infinigence AI)|[[code]](https://github.com/tyshiwo1/Accelerating-T2I-AR-with-SJD)![](https://img.shields.io/github/stars/tyshiwo1/Accelerating-T2I-AR-with-SJD?style=social)|ICLR'25|
|2024.10|<a id="LANTERN_ICLR_25"></a>[**[LANTERN]** Accelerating Visual Autoregressive Models with Relaxed Speculative Decoding](https://arxiv.org/abs/2410.03355)|Doohyuk Jang, Sihwan Park et al. (@KAIST \& Intel \& AITRICS)|[[code]](https://github.com/jadohu/LANTERN)![](https://img.shields.io/github/stars/jadohu/LANTERN?style=social)|ICLR'25|
|2025.01|<a id="Accel_Diffusion_via_SS_ICML_25"></a>[**Accelerated Diffusion Models via Speculative Sampling**](https://arxiv.org/abs/2501.05370)|Valentin De Bortoli, Alexandre Galashov et al. (@Google DeepMind)| |ICML'25|
|2025.05|<a id="Diffusion_Exchangeable_ICML_25"></a>[**[ASD]** Diffusion Models are Secretly Exchangeable: Parallelizing DDPMs via Auto Speculation](https://arxiv.org/abs/2505.03983)|Hengyuan Hu, Aniket Das et al. (@Stanford)| |ICML'25|
|2025.06|<a id="APD_NeurIPS_25"></a>[**[APD]** Accelerating Diffusion LLMs via Adaptive Parallel Decoding](https://arxiv.org/abs/2506.00413)|Daniel Israel et al. (@UCLA)|[[code]](https://github.com/danielmisrael/apd)![](https://img.shields.io/github/stars/danielmisrael/apd?style=social)|NeurIPS'25|
|2025.09|<a id="Spiffy_arXiv_25"></a>[**[Spiffy]** Structuring The Future: Diffusion LLM Speculative Decoding via Calibrated Draft Graphs](https://openreview.net/forum?id=4pqGfgZ2e6)|Sudhanshu Agrawal, Risheek Garrepalli et al. (@Qualcomm AI Research)| |ICMLW'26|
|2025.10|<a id="SSD_arXiv_25"></a>[**[SSD]** Self Speculative Decoding for Diffusion Large Language Models](https://arxiv.org/abs/2510.04147)|Yifeng Gao, Ziang Ji et al. (@SJTU \& USTC \& Xidian University \& Shanghai AI Lab \& Huawei)| |arXiv'25|
|2025.11|<a id="ODB_arXiv_25"></a>[**[ODB-dLLM]** Orchestrating Dual-Boundaries: An Arithmetic Intensity Inspired Acceleration Framework for Diffusion Language Models](https://arxiv.org/abs/2511.21759)|Linye Wei, Wenjue Chen et al. (@PKU)|[[code]](https://github.com/PKU-SEC-Lab/ODB-dLLM)![](https://img.shields.io/github/stars/PKU-SEC-Lab/ODB-dLLM?style=social)|DAC'26|

> **Related hybrid resource.** *Speculative Cascades — A hybrid approach for smarter, faster LLM inference* ([Google Research blog, 2025](https://research.google/blog/speculative-cascades-a-hybrid-approach-for-smarter-faster-llm-inference/)) connects routing, selection, and speculation into one production-oriented framework.

<a id="class-7"></a>
## 📙 7. Benchmarks and Evaluation

This layer organizes and evaluates the field through related surveys, benchmark suites, and diagnostic studies.

**Related surveys.**

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2024.01|<a id="SD_Survey_ACL_24"></a>[**Unlocking Efficiency in Large Language Model Inference: A Comprehensive Survey of Speculative Decoding**](https://arxiv.org/abs/2401.07851)|Heming Xia et al. (@PolyU \& PKU \& MSRA \& Alibaba)|[[code]](https://github.com/hemingkx/SpeculativeDecodingPapers)![](https://img.shields.io/github/stars/hemingkx/SpeculativeDecodingPapers?style=social)|Findings-ACL'24|
|2025.08|<a id="Parallel_Text_Gen_Survey_arXiv_25"></a>[**A Survey on Parallel Text Generation: From Parallel Decoding to Diffusion Language Models**](https://arxiv.org/abs/2508.08712)|Lingzhe Zhang, Liancheng Fang et al. (@PKU \& UIC \& THU \& XPENG \& Alibaba \& HKUST(GZ))|[[list]](https://github.com/zhanglingzhe0820/Awesome-Parallel-Text-Generation)![](https://img.shields.io/github/stars/zhanglingzhe0820/Awesome-Parallel-Text-Generation?style=social)|arXiv'25|
|2025.10|<a id="Parallel_Reasoning_Survey_arXiv_25"></a>[**A Survey on Parallel Reasoning**](https://arxiv.org/abs/2510.12164)|Ziqi Wang, Boye Niu et al. (@USTC \& Baidu \& USYD)|[[list]](https://github.com/PPPP-kaqiu/Awesome-Parallel-Reasoning)![](https://img.shields.io/github/stars/PPPP-kaqiu/Awesome-Parallel-Reasoning?style=social)|arXiv'25|

**Benchmark suites.**

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2024.01|<a id="Spec_Bench_website_24"></a>[**[Spec-Bench]** A Comprehensive Benchmark and Unified Evaluation Platform for Speculative Decoding](https://sites.google.com/view/spec-bench)|Heming Xia et al. (@PolyU \& PKU \& MSRA \& Alibaba)|[[code]](https://github.com/hemingkx/Spec-Bench)![](https://img.shields.io/github/stars/hemingkx/Spec-Bench?style=social)|misc'24|
|2025.09|<a id="TTS_Benchmark_ICLR_26"></a>[**Scaling Up, Speeding Up: A Benchmark of Speculative Decoding for Efficient LLM Test-Time Scaling**](https://arxiv.org/abs/2509.04474)|Shengyin Sun, Yiming Li et al. (@CityU HK \& Huawei)|[[code]](https://github.com/sunshy-1/SpecTTS-Bench)![](https://img.shields.io/github/stars/sunshy-1/SpecTTS-Bench?style=social)|ICLR'26|

**Diagnostic studies.**

|Date|Title|Authors|Code|Venue|
|:---:|:---|:---|:---:|:---:|
|2025.10|<a id="dLLM_Efficiency_Eval_arXiv_25"></a>[**How Efficient Are Diffusion Language Models? A Critical Examination of Efficiency Evaluation Practices**](https://arxiv.org/abs/2510.18480)|Han Peng, Peiyu Liu et al. (@RUC \& UIBE \& THU \& CityU HK)| |arXiv'25|
|2025.10|<a id="ParallelBench_ICLR_26"></a>[**[ParallelBench]** Understanding the Trade-offs of Parallel Decoding in Diffusion LLMs](https://arxiv.org/abs/2510.04767)|Wonjun Kang, Kevin Galim et al. (@FuriosaAI \& UW-Madison \& Microsoft \& UC Berkeley \& SNU \& KRAFTON AI \& Ludo Robotics)|[[code]](https://github.com/furiosa-ai/ParallelBench)![](https://img.shields.io/github/stars/furiosa-ai/ParallelBench?style=social)|ICLR'26|

<!-- ### Benchmarks \& Datasets

|Benchmark / Dataset|Focus|Link|
|:---|:---|:---|
|**Spec-Bench**|Unified evaluation of speculative decoding across tasks|[[GitHub]](https://github.com/hemingkx/Spec-Bench) / [[Site]](https://sites.google.com/view/spec-bench)|
|**MT-Bench**|Multi-turn open-ended dialogue|[[FastChat / llm_judge]](https://github.com/lm-sys/FastChat/tree/main/fastchat/llm_judge)|
|**HumanEval**|Code generation (pass@k)|[[OpenAI HumanEval]](https://github.com/openai/human-eval)|
|**MBPP**|Basic Python code generation|[[MBPP]](https://github.com/google-research/google-research/tree/master/mbpp)|
|**GSM8K**|Grade-school math reasoning|[[OpenAI GSM8K]](https://github.com/openai/grade-school-math)|
|**CNN/DailyMail \& XSum**|Abstractive summarization|dataset hubs vary|
|**IWSLT / WMT**|Machine translation|official task sites vary| -->

### Related Awesome Lists

- [hemingkx/SpeculativeDecodingPapers](https://github.com/hemingkx/SpeculativeDecodingPapers) — speculative decoding papers
- [zhanglingzhe0820/Awesome-Parallel-Text-Generation](https://github.com/zhanglingzhe0820/Awesome-Parallel-Text-Generation) — parallel text generation (AR-based & non-AR / dLLM)
- [Geralt-Targaryen/Awesome-Speculative-Decoding](https://github.com/Geralt-Targaryen/Awesome-Speculative-Decoding) — speculative decoding (with detailed reading notes)
- [hemingkx/Awesome-Efficient-Reasoning](https://github.com/hemingkx/Awesome-Efficient-Reasoning) — efficient reasoning (speculative reasoning & parallel thinking)
- [Xiaohao-Liu/Awesome-Multi-Token-Prediction](https://github.com/Xiaohao-Liu/Awesome-Multi-Token-Prediction) — multi-token prediction
- [PPPP-kaqiu/Awesome-Parallel-Reasoning](https://github.com/PPPP-kaqiu/Awesome-Parallel-Reasoning) — parallel reasoning
- [wang2226/Awesome-LLM-Decoding](https://github.com/wang2226/Awesome-LLM-Decoding) — general LLM decoding
- [VILA-Lab/Awesome-DLMs](https://github.com/VILA-Lab/Awesome-DLMs) — diffusion language models
- [ML-GSAI/Diffusion-LLM-Papers](https://github.com/ML-GSAI/Diffusion-LLM-Papers) - diffusion large language models
- [MessiX77/Awesome-Efficient-dLLMs](https://github.com/MessiX77/Awesome-Efficient-dLLMs) — efficient diffusion language models (dLLMs)
<!-- - [codefuse-ai/Awesome-Code-LLM](https://github.com/codefuse-ai/Awesome-Code-LLM) — code LLMs
- [Geralt-Targaryen/Awesome-Education-LLM](https://github.com/Geralt-Targaryen/Awesome-Education-LLM) — education LLMs -->

---

## 🤝 Contributing

Contributions are very welcome! If you would like to add a paper, fix metadata (venue, affiliation, links), or propose a new category, please open a pull request or an issue. When adding a paper, please keep entries in chronological order within each section and follow the existing table format: `Date | [Alias] Title (linked to paper) | Authors (@Affiliation) | Code (+ stars badge) | Venue`. Where information is uncertain, please mark it as `N/A` rather than guessing.

---

## 📝 Citation

🌟 If you find this repository useful, please consider citing our survey paper: 🌟

```bibtex
@misc{survey-parallel-decoding,
  title        = {Parallel Decoding of Language Models: A Survey},
  author       = {Yu-Yang Qian and Jia-Chen Liu and Hao-Cong Wu and Lanxiang Hu and Hao Zhang and Peng Zhao},
  year         = {2026},
  note         = {\url{https://github.com/ZinYY/Awesome-Parallel-Decoding}}
}
```

Our survey paper is coming soon, stay tuned!