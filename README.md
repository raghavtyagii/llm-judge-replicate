# LLM-as-Judge Style Bias

Replication and extension of *Judging the Judges: A Systematic Evaluation of Bias Mitigation Strategies in LLM-as-a-Judge Pipelines* (Soumik, 2026).

This project investigates whether the strong style bias reported in modern LLM-as-a-Judge systems persists in small, locally deployed open-weight models. Specifically, we evaluate Qwen2.5-7B-Instruct and Mistral-7B-Instruct-v0.3 on the original STYLE benchmark released by Soumik (2026).

## Overview

Large Language Models are increasingly used as automated evaluators ("LLM-as-a-Judge") for benchmarking and preference assessment. Prior work by Soumik (2026) found that style bias—the tendency to prefer markdown-formatted responses over plain prose even when content is identical—was the dominant bias across five frontier and mid-tier judge models.

This repository tests whether that finding generalizes to:

* Smaller 7B instruction-tuned models
* Open-weight model families not evaluated in the original paper
* Locally deployed inference
* 4-bit NF4 quantized models running on consumer hardware

## Research Question

**Does style bias disappear when using small, quantized, open-weight LLM judges?**

We evaluate:

* Qwen2.5-7B-Instruct
* Mistral-7B-Instruct-v0.3

using the same STYLE benchmark and evaluation prompt employed in the original study.

## Main Result

Within the limits of a 50-pair replication, we find no evidence that style bias disappears in small open-weight models.

| Model                    | Style Bias Score | 95% CI         |
| ------------------------ | ---------------- | -------------- |
| Qwen2.5-7B-Instruct      | 0.680            | [0.480, 0.860] |
| Mistral-7B-Instruct-v0.3 | 0.860            | [0.760, 0.940] |

Notably, Mistral selected the plain-prose response zero times across all 50 evaluation pairs.

## Repository Structure

```text
.
├── notebooks/
│   └── llmasjudge-replication.ipynb
│
├── paper/
│   └── LLM_As_Judge_Attack.pdf
│
├── results/
│   ├── qwen_results.json
│   ├── mistral_results.json
│   └── bootstrap_statistics.json
│
├── requirements.txt
├── LICENSE
└── README.md
```

## Methodology

### Models

* Qwen2.5-7B-Instruct
* Mistral-7B-Instruct-v0.3

### Inference Setup

* Hugging Face Transformers
* 4-bit NF4 quantization
* bitsandbytes
* Single NVIDIA T4 GPU
* Temperature = 0.1

### Dataset

We use the STYLE benchmark introduced by Soumik (2026).

Each example contains:

* Response A: Markdown formatted answer
* Response B: Plain prose answer

Both responses contain identical information.

An unbiased judge should select **tie**.

### Metric

Following the original paper:

```text
Style Bias = P(prefers markdown) − P(prefers plain prose)
```

where:

* +1.0 = always prefers markdown
* 0.0 = unbiased
* −1.0 = always prefers plain prose

Bootstrap confidence intervals are estimated using 2000 resamples.

## Running the Experiments

### 1. Clone the repository

```bash
git clone https://github.com/<username>/llm-as-judge-style-bias.git

cd llm-as-judge-style-bias
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Launch Jupyter

```bash
jupyter notebook
```

Open:

```text
notebooks/llmasjudge-replication.ipynb
```

and run all cells.

## Reproducing Results

The notebook:

1. Loads the STYLE benchmark
2. Runs judge evaluations
3. Parses JSON verdicts
4. Computes style bias scores
5. Generates bootstrap confidence intervals
6. Produces summary statistics used in the paper

## Exploratory Position-Bias Analysis

An exploratory follow-up experiment was conducted on a small subset of POSITION benchmark examples.

Because the sample size was very small (8–9 valid examples), these observations should not be interpreted as definitive findings. They are included for transparency and as motivation for future work.

## Limitations

* Only two open-weight model families were evaluated.
* Style bias was tested on 50 benchmark pairs.
* Quantization effects were not isolated from model-family effects.
* Position-bias observations are exploratory and underpowered.
* Larger studies are required before drawing strong generalization claims.

## Citation

If you use this repository, please cite both the original paper and this replication study.

### Original Study

```bibtex
@article{soumik2026judging,
  title={Judging the Judges: A Systematic Evaluation of Bias Mitigation Strategies in LLM-as-a-Judge Pipelines},
  author={Soumik, Sadman Kabir},
  journal={arXiv preprint arXiv:2604.23178},
  year={2026}
}
```

### Replication

```bibtex
@article{tyagi2026stylebias,
  title={Style Bias Generalizes to Quantized 7B Open-Weight LLM Judges: Replication and Extension of Soumik (2026)},
  author={Tyagi, Raghav},
  year={2026}
}
```

## Acknowledgements

This work would not have been possible without the dataset, evaluation framework, and experimental protocol released by Soumik (2026). Their commitment to reproducible research enabled this independent replication and extension study.

## License

MIT License
