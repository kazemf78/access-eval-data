# access-eval-data

Full per-formulation model answers, judge grades and paraphrase-equivalence verdicts behind

> *Revealing what benchmark scores hide: Jointly measuring the breadth and reliability of LLM capabilities using
> accessibility profiles.* Kazem Faghih, Yize Cheng, Shoumik Saha, Mobina Pournemat, Armin Gerami, Soheil Feizi.
> [arXiv:2610.24957](https://arxiv.org/abs/2610.24957), 2026.

Code, the validated formulation sets and the compact tables that reproduce every figure and number of the paper are
in [access-eval](https://github.com/kazemf78/access-eval). You only need this repository to re-grade answers, audit
the judges, or analyze the answer texts.

All files are gzip-compressed CSV (read directly with `pandas.read_csv`) or JSON; `MANIFEST.json` lists every file
with its size and SHA-256. Tables larger than 90 MB are split into one file per model.

```bash
git clone --depth 1 https://github.com/kazemf78/access-eval-data
```

## Layout

| Path | Content |
|---|---|
| `<dataset>/answers.csv.gz` (or `answers/<model>.csv.gz`) | one row per (model, formulation): `model, idx, is_original, original_question, paraphrased_question, answer, gen_config_hash, ...`. Datasets: `SimpleQA`, `TruthfulQA`, `gsm8k_test`, `math_500_test`. |
| `<dataset>/grades.csv.gz` | LLM-judge grades (SimpleQA, TruthfulQA): `idx, model, evaluator, verdict, grading_response, ...`. GSM8K / MATH-500 are graded by answer parsing (`math_verify` + fallback extractor) in the code repository. |
| `TruthfulQA/grades_3gpt/<judge>.csv.gz` | the three GPT judges (gpt-3.5-turbo, gpt-4.1-mini, gpt-5.4-mini) whose binary majority gives the paper's TruthfulQA labels, with raw judge outputs |
| `<dataset>/formulations_manifest.csv.gz` | every candidate paraphrase with per-judge equivalence verdicts and its acceptance status |
| `<dataset>/equivalence_judge_verdicts.csv.gz` | all paraphrase-equivalence judge calls (judge model, prompt, verdict) |
| `mmlu/answers/<model>.csv.gz`, `mmlu/formulations.json` | MMLU (custom generative protocol): answers, extracted letter, correctness; the formulation set |
| `bfcl/{inputs,results,scores}.tar.gz` | BFCL live_multiple (600 inquiries x original + 10 paraphrases): harness inputs (plan, question files), model outputs, harness scores. Harness: [llm-unreliable-tool-preferences](https://github.com/kazemf78/llm-unreliable-tool-preferences). |
| `rq2/variability/`, `rq2/determinism/` | RQ2: 4 models x 3 benchmarks x 8 formulations x 5 samples at T = 0.7 (answers + labels), greedy answers, greedy repeats |
| `gsm8k_rerun/` | GSM8K greedy rerun of 5 models (run-to-run baseline): answers and the formulation pool |

`idx` is the row of the benchmark's test split (SimpleQA, TruthfulQA, GSM8K test, MATH-500, MMLU test).

## License

[CC-BY-4.0](LICENSE). The data are derived from GSM8K (MIT), MATH / MATH-500 (MIT), MMLU (MIT), SimpleQA (MIT),
TruthfulQA (Apache-2.0) and BFCL (Apache-2.0); please also cite the original benchmarks. Model outputs are subject to
the terms of the respective model providers.

## Citation

See [access-eval](https://github.com/kazemf78/access-eval#citation).
