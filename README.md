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



### Deduplication

| Paper | Notes |
|---|---|
| [Deduplicating Training Data Makes LMs Better](https://arxiv.org/abs/2107.06499) — Lee et al. (2022) | Canonical dedup paper; exact + near-dedup; shows >1% of LM output is verbatim training data |
| [Scaling Data-Constrained LMs](https://arxiv.org/abs/2305.16264) — Muennighoff et al. (2023) | What happens when you run out of unique data? Up to 4 epochs of repetition is mostly fine |

### Quality Filtering & Data Selection

| Paper | Notes |
|---|---|
| Ask-LLM and Perplexity Filtering — Sachdeva et al. (2024) | Compares LLM prompting vs. perplexity as quality proxies; neither is clearly dominant |

### Data Mixing & Domain Weights

| Paper | Notes |
|---|---|
| [DoReMi](https://arxiv.org/abs/2305.10429) — Xie et al. (2023) | Train a small proxy model with Group DRO to find optimal domain weights; 2.6x speedup |
| [RegMix](https://arxiv.org/abs/2407.01492) — Liu et al. (2024) | Treat data mixing as regression; more sample-efficient than DoReMi |
| Data Mixing Laws — Ye et al. (2024) | Scaling laws for domain mixing; predict optimal mix from small experiments |

---

## Post-training Data

collecting user preference data, collecting verifiable rewards data, challenges creating data for non-verifiable tasks, RL datasets creation (honestly getting a person from a data vendor to walk through creating one of their off-the-shelf training sets would be very useful), maybe reward models, deciding whether you want to teach a model a capability through pretraining, midtraining, SFT or RL, scaling laws again. 


### Mid-training

Mid-training (continued pretraining on curated domain data) is how models acquire specialized knowledge or new capabilities without full retraining. It's often the most cost-efficient way to improve a model on a specific task distribution.

| Paper | Notes |
|---|---|
| [Don't Stop Pretraining](https://arxiv.org/abs/2004.10964) — Gururangan et al. (2020) | Canonical case for domain-adaptive pretraining; robustly improves downstream tasks |
| Llama 2 Long — Chen et al. (2023) | How Meta extended context window in mid-training; data mix shifts during training matter |
| MiniCPM — Hu et al. (2024) | Multi-stage training with data decay schedules; shows how staged training affects final quality |

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
| Anthropic HH Dataset — Bai et al. (2022) | Helpful & Harmless preference data with open release; how raters are instructed shapes everything |
| [Constitutional AI](https://arxiv.org/abs/2212.08073) — Bai et al. (2022) | Replace human preference labeling with AI feedback (RLAIF); opens up data scale |



### RL & Verifiable Rewards

The core insight: verifiable rewards (math, code) make RL from scratch tractable. 

#### Reasoning & Math Datasets

| Dataset | Notes |
|---|---|
| [GSM8K](https://arxiv.org/abs/2110.14168) — Cobbe et al. (2021) | Grade school math with solutions; became standard for chain-of-thought evaluation |
| MATH — Hendrycks et al. (2021) | Competition math; hard ceiling that drove PRM and ORM research |
| [HumanEval](https://arxiv.org/abs/2107.03374) — Chen et al. (2021) | Code generation with unit test verification; a model of what verifiable rewards look like |

#### Process vs. Outcome Reward Models

| Paper | Notes |
|---|---|
| [Let's Verify Step by Step](https://arxiv.org/abs/2305.20050) — Lightman et al. (2023) | PRMs outperform ORMs on MATH; releases PRM800K (800K step-level human feedback labels) |
| [MATH-Shepherd](https://arxiv.org/abs/2312.08935) — Wang et al. (2023) | Automated PRM construction without human labels via execution-based step verification |

### Annotation & Human Feedback Quality

| Paper | Notes |
|---|---|
| [Blog on Human Data Quality](https://lilianweng.github.io/posts/2024-02-05-human-data-quality/) | P |
| [MATH-Shepherd](https://arxiv.org/abs/2312.08935) — Wang et al. (2023) | Automated PRM construction without human labels via execution-based step verification |

- https://www.interconnects.ai/p/rlhf-chatgpt-data-moats?utm_source=publication-search

### Synthetic Data

Synthetic data spans the full training pipeline: generating textbook-quality pretraining text, diverse instruction-following data, and complex multi-step reasoning traces.

- https://lilianweng.github.io/posts/2022-04-15-data-gen/
- https://lilianweng.github.io/posts/2022-02-20-active-learning/
- https://lilianweng.github.io/posts/2021-12-05-semi-supervised/
- https://www.interconnects.ai/p/frontiers-in-synthetic-data?utm_source=publication-search



### Scaling Laws & Data Selection

| Paper | Notes |
|---|---|
| [Scaling Laws for Neural LMs](https://arxiv.org/abs/2001.08361) — Kaplan et al. (2020) | Original scaling laws: loss scales as power law with model size, data, and compute |
| [Chinchilla](https://arxiv.org/abs/2203.15556) — Hoffmann et al. (2022) | Models are undertrained; optimal token/parameter ratio ~20x; rebalanced the whole field |
| Beyond Chinchilla-Optimal (2024) | Inference costs change the optimal training point; deploy at scale → train on more data |
| [Scaling Data-Constrained LMs](https://arxiv.org/abs/2305.16264) — Muennighoff et al. (2023) | Data-constrained scaling laws; when to repeat vs. augment vs. synthesize |

