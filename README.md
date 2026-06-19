# LLM-as-Judge Style Bias

Replication and extension of *Judging the Judges: A Systematic Evaluation of Bias Mitigation Strategies in LLM-as-a-Judge Pipelines* (Soumik, 2026).

This project investigates whether the strong style bias reported in LLM-as-a-Judge systems persists in small, locally deployed open-weight models. Using the original STYLE benchmark, we evaluate Qwen2.5-7B-Instruct and Mistral-7B-Instruct-v0.3 under 4-bit NF4 quantization.

## Research Question

**Does style bias disappear in small open-weight LLM judges?**

The original paper reported substantial markdown preference across five frontier and mid-tier models. This replication tests whether that finding generalizes to:

* Qwen2.5-7B-Instruct
* Mistral-7B-Instruct-v0.3

running locally on consumer hardware.

## Main Results

| Model                    | Style Bias Score | 95% CI         |
| ------------------------ | ---------------- | -------------- |
| Qwen2.5-7B-Instruct      | 0.680            | [0.480, 0.860] |
| Mistral-7B-Instruct-v0.3 | 0.860            | [0.760, 0.940] |

Within the limits of a 50-pair replication, we find no evidence that style bias disappears in small open-weight models.

## Repository Structure

```text
.
├── requirements.txt
├── notebooks/
│   └── llmasjudge-replication.ipynb
└── results/
    ├── qwen_style_bias.json
    └── mistral_style_bias.json
```

## Methodology

### Dataset

We use the STYLE benchmark introduced by Soumik (2026).

Each evaluation pair contains:

* A markdown-formatted response
* A plain-prose response

with identical underlying content.

An unbiased judge should select **tie**.

### Models

* Qwen2.5-7B-Instruct
* Mistral-7B-Instruct-v0.3

### Inference Configuration

* Hugging Face Transformers
* 4-bit NF4 quantization
* Temperature = 0.1
* Local inference

### Metric

Following the original paper:

```text
Style Bias = P(prefers markdown) − P(prefers plain prose)
```

where:

* +1.0 = always prefers markdown
* 0.0 = unbiased
* −1.0 = always prefers plain prose

## Reproducing the Experiments

Install dependencies:

```bash
pip install -r requirements.txt
```

Launch Jupyter:

```bash
jupyter notebook
```

Open:

```text
notebooks/llmasjudge-replication.ipynb
```

and run all cells.

The notebook performs:

1. Dataset loading
2. Judge inference
3. Verdict parsing
4. Style bias calculation
5. Bootstrap confidence interval estimation
6. Result export to JSON

## Limitations

* Only two open-weight model families were evaluated.
* The STYLE benchmark contains 50 evaluation pairs.
* Quantization effects were not isolated from model-family effects.
* Results should be interpreted as a replication and extension rather than a definitive generalization study.

## Acknowledgements

This project builds upon the dataset, evaluation protocol, and findings released by Sadman Kabir Soumik in *Judging the Judges: A Systematic Evaluation of Bias Mitigation Strategies in LLM-as-a-Judge Pipelines* (2026).

## License

MIT License.
