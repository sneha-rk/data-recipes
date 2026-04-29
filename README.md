# Data for LLMs: A Reading List

> **Context**: This list grew out of a [thread](https://x.com/yoavgo/status/2048781896499212518?s=20) on what to teach in an LLM course. This list covers the creation and scaling of pretraining, mid-train and post-training datasets.



## Table of Contents

- [Pretraining Data](#pretraining-data)
- [Mid-training](#mid-training)
- [Instruction Tuning (SFT)](#instruction-tuning-sft)
- [Preference Data & RLHF](#preference-data--rlhf)
- [RL & Verifiable Rewards](#rl--verifiable-rewards)
- [Synthetic Data](#synthetic-data)
- [Scaling Laws & Data Selection](#scaling-laws--data-selection)
- [Annotation & Human Feedback Quality](#annotation--human-feedback-quality)


## Pretraining Data

Going from common crawl to a reasonable dataset with data audits, ablations, data quality classifiers, ablations, mixture studies/scaling laws etc. 


### Datasets & Documentation

| Paper | Notes |
|---|---|
| [Exploring the Limits of Transfer Learning (T5)](https://arxiv.org/abs/1910.10683) — Raffel et al. (2020) | Introduced C4 (Colossal Clean Crawled Corpus) via heuristic filtering of Common Crawl; data quality methodology became a template for many subsequent corpora |
| [Dolma](https://arxiv.org/abs/2402.00159) — Soldaini et al. (2024) | Open 3T-token corpus from AI2 used to train OLMo; documents full filtering pipeline and data provenance; one of the most transparent large pretraining datasets |

### Data Audits

| Paper | Notes |
|---|---|
| [Quality at a Glance: An Audit of Web-Crawled Corpora](https://arxiv.org/abs/2103.12028) — Luccioni & Viviano (2021) | Audits C4, mC4, OSCAR, and REALNEWS for hate speech, adult content, and boilerplate; motivates quality filtering as a necessity, not just an optimization |

### Deduplication

| Paper | Notes |
|---|---|
| [Deduplicating Training Data Makes LMs Better](https://arxiv.org/abs/2107.06499) — Lee et al. (2022) | Canonical dedup paper; exact + near-dedup; shows >1% of LM output is verbatim training data |
| [Scaling Data-Constrained LMs](https://arxiv.org/abs/2305.16264) — Muennighoff et al. (2023) | What happens when you run out of unique data? Up to 4 epochs of repetition is mostly fine |

### Quality Filtering & Data Selection

| Paper | Notes |
|---|---|
| [A Pretrainer's Guide to Training Data](https://arxiv.org/abs/2305.13169) — Longpre et al. (2023) | Systematic study of how data age, domain coverage, quality, and toxicity affect downstream performance; practical guidance on which data choices matter most |
| Ask-LLM and Perplexity Filtering — Sachdeva et al. (2024) | Compares LLM prompting vs. perplexity as quality proxies; neither is clearly dominant |

### Scaling Laws & Data Selection

| Paper | Notes |
|---|---|
| [Scaling Laws for Neural LMs](https://arxiv.org/abs/2001.08361) — Kaplan et al. (2020) | Original scaling laws: loss scales as power law with model size, data, and compute |
| [Chinchilla](https://arxiv.org/abs/2203.15556) — Hoffmann et al. (2022) | Models are undertrained; optimal token/parameter ratio ~20x; rebalanced the whole field |
| [Scaling Data-Constrained LMs](https://arxiv.org/abs/2305.16264) — Muennighoff et al. (2023) | Data-constrained scaling laws; when to repeat vs. augment vs. synthesize |

### Data Mixing & Domain Weights

| Paper | Notes |
|---|---|
| [DoReMi](https://arxiv.org/abs/2305.10429) — Xie et al. (2023) | Train a small proxy model with Group DRO to find optimal domain weights; 2.6x speedup |
| [RegMix](https://arxiv.org/abs/2407.01492) — Liu et al. (2024) | Treat data mixing as regression; more sample-efficient than DoReMi |
| [Data Mixing Laws](https://arxiv.org/abs/2403.16952) — Ye et al. (2024) | Scaling laws for domain mixing; predict optimal mix from small experiments without running full-scale training |


## Post-training Data

The [Llama 3 paper](https://arxiv.org/abs/2407.21783) (Dubey et al., 2024) is the most detailed public account of an end-to-end post-training pipeline, covering SFT data curation, preference data collection, and RL with verifiable rewards.

### Mid-training

Mid-training (continued pretraining on curated domain data) is how models acquire specialized knowledge or new capabilities without full retraining. It's often the most cost-efficient way to improve a model on a specific task distribution.

| Paper | Notes |
|---|---|
| [Don't Stop Pretraining](https://arxiv.org/abs/2004.10964) — Gururangan et al. (2020) | Canonical case for domain-adaptive pretraining; robustly improves downstream tasks |
| [Llama 2 Long](https://arxiv.org/abs/2309.16039) — Chen et al. (2023) | How Meta extended context window in mid-training; data mix shifts during training matter |
| [MiniCPM](https://arxiv.org/abs/2404.06395) — Hu et al. (2024) | Multi-stage training with data decay schedules; shows how staged training affects final quality |

---

### SFT

| Paper | Notes |
|---|---|
| [FLAN](https://arxiv.org/abs/2109.01652) — Wei et al. (2021) | First large-scale instruction tuning; template-based construction from existing NLP benchmarks |
| [Self-Instruct](https://arxiv.org/abs/2212.10560) — Wang et al. (2022) | Bootstrap instruction data from a model's own generations; the basis for most synthetic SFT pipelines |

---

### Preference Data & RLHF

| Paper | Notes |
|---|---|
| [InstructGPT](https://arxiv.org/abs/2203.02155) — Ouyang et al. (2022) | Defined the RLHF pipeline; read the data collection and labeler instruction sections closely |
| [Anthropic HH Dataset](https://arxiv.org/abs/2204.05862) — Bai et al. (2022) | Helpful & Harmless preference data with open release; how raters are instructed shapes everything |
| [Constitutional AI](https://arxiv.org/abs/2212.08073) — Bai et al. (2022) | Replace human preference labeling with AI feedback (RLAIF); opens up data scale |



### RL & Verifiable Rewards

The core insight: verifiable rewards (math, code) make RL from scratch tractable. 

#### Reasoning & Math Datasets

| Dataset | Notes |
|---|---|
| [GSM8K](https://arxiv.org/abs/2110.14168) — Cobbe et al. (2021) | Grade school math with solutions; became standard for chain-of-thought evaluation |
| [MATH](https://arxiv.org/abs/2103.03874) — Hendrycks et al. (2021) | Competition math; hard ceiling that drove PRM and ORM research |
| [HumanEval](https://arxiv.org/abs/2107.03374) — Chen et al. (2021) | Code generation with unit test verification; a model of what verifiable rewards look like |

#### Process vs. Outcome Reward Models

| Paper | Notes |
|---|---|
| [Let's Verify Step by Step](https://arxiv.org/abs/2305.20050) — Lightman et al. (2023) | PRMs outperform ORMs on MATH; releases PRM800K (800K step-level human feedback labels) |
| [MATH-Shepherd](https://arxiv.org/abs/2312.08935) — Wang et al. (2023) | Automated PRM construction without human labels via execution-based step verification |

### Annotation & Human Feedback Quality

| Paper | Notes |
|---|---|
| [Human Data Quality](https://lilianweng.github.io/posts/2024-02-05-human-data-quality/) — Lilian Weng (2024) | Comprehensive survey of annotation challenges: inter-annotator agreement, label noise, rater bias; covers crowdsourcing and expert labeling |
| [RLHF and ChatGPT Data Moats](https://www.interconnects.ai/p/rlhf-chatgpt-data-moats?utm_source=publication-search) — Interconnects | Argues proprietary preference data, not model weights, is the lasting competitive moat; contextualizes why companies guard their feedback data |
| [MATH-Shepherd](https://arxiv.org/abs/2312.08935) — Wang et al. (2023) | Automated PRM construction without human labels via execution-based step verification |

### Synthetic Data

Synthetic data spans the full training pipeline: generating textbook-quality pretraining text, diverse instruction-following data, and complex multi-step reasoning traces.

| Resource | Notes |
|---|---|
| [Learning with not Enough Data Part 1: Semi-Supervised Learning](https://lilianweng.github.io/posts/2021-12-05-semi-supervised/) — Lilian Weng | Self-training, consistency regularization, co-training; foundational methods for leveraging unlabeled data |
| [Learning with not Enough Data Part 2: Active Learning](https://lilianweng.github.io/posts/2022-02-20-active-learning/) — Lilian Weng | Query strategies for selecting the most informative samples; relevant to human feedback efficiency |
| [Learning with not Enough Data Part 3: Data Generation](https://lilianweng.github.io/posts/2022-04-15-data-gen/) — Lilian Weng | Data augmentation and generation techniques; covers early LLM-based generation approaches |
| [Frontiers in Synthetic Data](https://www.interconnects.ai/p/frontiers-in-synthetic-data?utm_source=publication-search) — Nathan Lambert | Overview of synthetic data use across pretraining, SFT, and RL; covers quality control, diversity, and the role of verifiers |
