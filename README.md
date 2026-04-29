# Data for LLMs: A Reading List

> **Context**: This list grew out of a [thread](https://x.com/yoavgo/status/2048781896499212518?s=20) on what to teach in an LLM course. This list covers the creation and scaling of pretraining, mid-train and post-training datasets.
> 
> Work in progress, PRs welcome

## Table of Contents

- [Pretraining Data](#pretraining-data)
  - [Datasets & Documentation](#datasets--documentation)
  - [Data Audits and Transparency](#data-audits-and-transparency)
  - [Deduplication](#deduplication)
  - [Quality Filtering & Data Selection](#quality-filtering--data-selection)
  - [Scaling Laws & Data Selection](#scaling-laws--data-selection)
  - [Data Mixing & Domain Weights](#data-mixing--domain-weights)
- [Post-training Data](#post-training-data)
  - [Mid-training](#mid-training)
  - [SFT](#sft)
  - [Preference Data & RLHF](#preference-data--rlhf)
  - [RL](#rl)
- [Further Reading: Practitioner Perspectives](#further-reading-practitioner-perspectives-wip)


## Pretraining Data

Going from common crawl to a reasonable dataset with data audits, ablations, data quality classifiers, ablations, mixture studies/scaling laws etc. 


### Datasets & Documentation

| Paper | Notes |
|---|---|
| [Exploring the Limits of Transfer Learning (T5)](https://arxiv.org/abs/1910.10683) — Raffel et al. (2020) | Introduced C4 (Colossal Clean Crawled Corpus) via heuristic filtering of Common Crawl; data quality methodology became a template for many subsequent corpora |
| [Language Models are Few-Shot Learners (GPT-3)](https://arxiv.org/abs/2005.14165) — Brown et al. (2020) | Section 2.2 is the foundational text for classifier-based quality filtering: trained a logistic regression classifier to distinguish high-quality text (WebText/Reddit) from raw Common Crawl, then probabilistically upsampled high-scoring documents — the direct ancestor of Ask-LLM |
| [The Pile: An 800GB Dataset of Diverse Text for Language Modeling](https://arxiv.org/abs/2101.00027) — Gao et al. (2020) | Pre-dates Dolma and C4 but was foundational for open-source datasets; established the blueprint for diverse, multi-domain data mixing (22 sources) and rigorous dataset documentation |
| [Scaling Language Models: Methods, Analysis & Insights from Training Gopher](https://arxiv.org/abs/2112.11446) — Rae et al. (2021) | Effectively the encyclopedia of heuristic and quality filtering; details the exact pipeline for cleaning MassiveText, combining document-level heuristics, repetition removal, and classifier-based quality selection |
| [Dolma](https://arxiv.org/abs/2402.00159) — Soldaini et al. (2024) | Open 3T-token corpus from AI2 used to train OLMo; documents full filtering pipeline and data provenance; one of the most transparent large pretraining datasets |

### Data Audits and Transparency

| Paper | Notes |
|---|---|
| [Quality at a Glance: An Audit of Web-Crawled Multilingual Datasets](https://arxiv.org/abs/2103.12028) — Kreutzer et al. (2022) | Massive manual audit of multilingual web corpora (mC4, OSCAR); found lower-resource languages are plagued with boilerplate, wrong-language text, and incorrect language classifications |
| [What's in the Box? An Analysis of Undesirable Content in the Common Crawl Corpus](https://arxiv.org/abs/2105.02732) — Luccioni & Viviano (2021) | Foundational audit of Common Crawl showing the volume of hate speech and adult content present before filtering; proves data curation is a safety necessity, not just an optimization |
| [Documenting Large Webtext Corpora: A Case Study on the Colossal Clean Crawled Corpus](https://arxiv.org/abs/2104.08758) — Dodge et al. (2021) | Definitive case study on what survives a filtering pipeline; C4 still contains machine-translated patents and benchmark contamination, and blocklist filtering disproportionately erases text from minority identities |
| [Data Portraits: Recording Foundation Model Training Data](https://arxiv.org/abs/2303.03919) — Marone & Van Durme (2023) | Efficient data sketching method letting downstream users query whether specific text was included in a model's training data; critical for checking test set leakage and memorization |
| [What's In My Big Data? (WIMBD)](https://arxiv.org/abs/2310.20707) — Elazar et al. (2023) | Scalable platform to search and analyze massive pretraining corpora; exposes toxic content, PII, benchmark leakage, and duplicated domains at macro scale |

### Deduplication

| Paper | Notes |
|---|---|
| [Deduplicating Training Data Makes LMs Better](https://arxiv.org/abs/2107.06499) — Lee et al. (2022) | Canonical dedup paper; exact + near-dedup; shows >1% of LM output is verbatim training data |
| [Scaling Data-Constrained LMs](https://arxiv.org/abs/2305.16264) — Muennighoff et al. (2023) | What happens when you run out of unique data? Up to 4 epochs of repetition is mostly fine |

### Quality Filtering & Data Selection

| Paper | Notes |
|---|---|
| [A Pretrainer's Guide to Training Data](https://arxiv.org/abs/2305.13169) — Longpre et al. (2023) | Systematic study of how data age, domain coverage, quality, and toxicity affect downstream performance; practical guidance on which data choices matter most |
| [How to Train Data-Efficient LLMs (Ask-LLM)](https://arxiv.org/abs/2402.09668) — Sachdeva et al. (2024) | Compares 19 data selection methods; Ask-LLM (LLM-based quality scoring) and Density (coverage-based) emerge as the best in their respective categories |
| [The RefinedWeb Dataset for Falcon LLM](https://arxiv.org/abs/2306.01116) — Penedo et al. (2023) | Proved that aggressively filtered and deduplicated web data (CommonCrawl) alone can match or outperform curated corpora; a masterclass in macro-data processing |

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

A major open question is when to midtrain at all (and relatedly, when to SFT vs. RL): [Midtraining Bridges Pretraining and Posttraining Distributions](https://arxiv.org/abs/2510.14865) — Liu et al. (2025)

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
| [Direct Preference Optimization (DPO)](https://arxiv.org/abs/2305.18290) — Rafailov et al. (2023) | Revolutionized post-training by bypassing the explicit reward model and unstable PPO loop; allows preference tuning directly on the policy via a simple classification loss |

#### When Human Judgment Falls Short: Non-Verifiable Tasks [WIP]

The core bottleneck of RLHF is that humans must be able to evaluate outputs reliably. As models get smarter on specialized tasks, this assumption breaks down.

| Paper | Notes |
|---|---|
| [AI Safety via Debate](https://arxiv.org/abs/1805.00899) — Irving et al. (2018) | Proposed training agents via self-play debate: two agents argue opposing positions and a human judges; in theory, debate with optimal play can answer any question in PSPACE with polynomial-time judges — a theoretical framework for supervising superhuman models |
| [Measuring Progress on Scalable Oversight for Large Language Models](https://arxiv.org/abs/2211.03540) — Bowman et al. (2022) | Empirical study of scalable oversight; uses tasks where domain experts succeed but non-experts struggle to simulate the problem of evaluating outputs that exceed the rater's competence |

### RL

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

#### Reward Models [WIP]

The [Llama 3 paper](https://arxiv.org/abs/2407.21783) Section 4.2 is worth reading for how Meta trained and iterated on reward models in production — covering preference annotation guidelines, data mix, and the RM's role in filtering SFT data.

| Paper | Notes |
|---|---|
| [RewardBench: Evaluating Reward Models for Language Modeling](https://arxiv.org/abs/2403.13787) — Lambert et al. (2024) | First systematic benchmark for reward models; covers chat, reasoning, and safety; reveals failure modes in models that appear strong by other metrics |

#### Annotation & Human Feedback Quality

| Paper | Notes |
|---|---|
| [Human Data Quality](https://lilianweng.github.io/posts/2024-02-05-human-data-quality/) — Lilian Weng (2024) | Comprehensive survey of annotation challenges: inter-annotator agreement, label noise, rater bias; covers crowdsourcing and expert labeling |
| [RLHF and ChatGPT Data Moats](https://www.interconnects.ai/p/rlhf-chatgpt-data-moats?utm_source=publication-search) — Interconnects | Argues proprietary preference data, not model weights, is the lasting competitive moat; contextualizes why companies guard their feedback data |
| [MATH-Shepherd](https://arxiv.org/abs/2312.08935) — Wang et al. (2023) | Automated PRM construction without human labels via execution-based step verification |

#### RL scaling

| Paper | Notes |
|---|---|
| [Midtraining Bridges Pretraining and Posttraining Distributions](https://arxiv.org/abs/2510.14865) — Liu et al. (2025) | Frames midtraining as bridging the gap between pretraining and posttraining data distributions; clarifies when midtraining helps vs. when SFT or RL is the right lever |
| [The Art of Scaling Reinforcement Learning Compute for LLMs](https://arxiv.org/abs/2510.13786) — Khatri et al. (2025) | Systematic study (400K+ GPU-hours) establishing predictive scaling laws for RL; fits sigmoidal compute-performance curves and introduces ScaleRL, a stable recipe that scales to 100K GPU-hours with pre-training-level predictability |

#### Synthetic Data [WIP]

Synthetic data spans the full training pipeline: generating textbook-quality pretraining text, diverse instruction-following data, and complex multi-step reasoning traces. Several of the RL datasets discussed above can be argued to be synthetic. 

I've deliberately excluded papers that distill RL data from stronger closed-source models, so, I've mainly linked some blog posts here that cover the high level principles:

- TODO: self-play and self-distillation papers

| Resource | Notes |
|---|---|
| [Learning with not Enough Data Series](https://lilianweng.github.io/posts/2021-12-05-semi-supervised/) — Lilian Weng | Three part series on Semi-Supervised Learning, Active Learning and Data Generation |
| [Frontiers in Synthetic Data](https://www.interconnects.ai/p/frontiers-in-synthetic-data?utm_source=publication-search) — Nathan Lambert | Overview of synthetic data use across pretraining, SFT, and RL; covers quality control, diversity, and the role of verifiers |

## Further Reading: Practitioner Perspectives [WIP]

Papers don't capture the operational side of data pipelines — what it takes to run annotation at scale, manage contractor quality, and handle the human side of RLHF. Resources here are more practitioner-facing.

| Resource | Notes |
|---|---|
| [Anthropic Uses Surge AI for RLHF](https://surgehq.ai/blog/anthropic-surge-ai-rlhf-platform-train-llm-assistant-human-feedback) — Surge AI | Practitioner account of running RLHF annotation at scale; covers task design, quality control, and annotator calibration from a data vendor perspective |
