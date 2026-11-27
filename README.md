# Protect Safety AI

## Overview

A Python evaluation scaffold for binary remote-exam proctoring alerts. It calculates confusion-matrix metrics, compares lighting and noise categories, and identifies low-confidence positive flags for additional review.

The supplied 12-event dataset is synthetic. Its example metrics demonstrate the pipeline and do not establish real-world detection accuracy, fairness, or robustness. A flagged event is not proof of cheating.

## Capabilities

| Capability | Output |
| --- | --- |
| Confusion matrix | True/false positives and negatives |
| Metrics | Accuracy, precision, recall, false-positive and false-negative rates |
| Environmental breakdown | Separate reports by lighting and noise |
| Review signal | IDs of positive predictions below a confidence threshold |
| Input checks | Missing columns, invalid labels/confidence, duplicates, and empty data |
| Reporting | Human-readable or saved JSON |

The report's recommendation to review all positive flags is a policy recommendation, not an implemented review interface or enforcement system.

## Source location and quick start

The runnable source is distributed as `proctorai_safety_eval_original.zip`. Extract it to access `proctorai-safety-eval/`, including the evaluator, synthetic data, tests, and its own README. The source has not been unpacked into the GitHub tree.

The supplied documentation specifies Python 3.9 or later with no third-party evaluator dependencies:

```bash
unzip proctorai_safety_eval_original.zip
cd proctorai-safety-eval
python src/evaluate.py data/synthetic_events.csv --output report.json
python -m unittest discover -s tests -v
```

On Windows, extract the archive with File Explorer or PowerShell's `Expand-Archive`, then run the Python commands from the extracted project directory.

To change the illustrative review cutoff:

```bash
python src/evaluate.py data/synthetic_events.csv --review-threshold 0.8 --output report.json
```

The documented default is 0.7. It is not an empirically validated threshold. Archive contents and tests were not executed during this documentation cleanup.

## Input format

Provide a UTF-8 CSV with these fields:

| Column | Meaning |
| --- | --- |
| `event_id` | Unique event identifier |
| `ground_truth_violation` | Reference label: 0 or 1 |
| `predicted_violation` | Binary model alert: 0 or 1 |
| `confidence` | Model-supplied value between 0 and 1 |
| `lighting` | Condition label, such as bright or dim |
| `noise` | Condition label, such as quiet or noisy |

```csv
event_id,ground_truth_violation,predicted_violation,confidence,lighting,noise
E002,0,1,0.55,dim,quiet
```

## Interpreting results

Precision is TP / (TP + FP); recall is TP / (TP + FN); false-positive rate is FP / (FP + TN); false-negative rate is FN / (FN + TP). Accuracy is (TP + TN) / N. The documented JSON output uses null when a rate's denominator is zero.

The original sample report describes 3 true positives, 4 true negatives, 3 false positives, and 2 false negatives. Those fabricated examples are too small to support claims about a deployed system. Confidence values may be uncalibrated, and high-confidence alerts can still be wrong.

## Research limits and next steps

This is an evaluation starter rather than a trained proctoring model or a demographic fairness audit. Meaningful extensions require a documented annotation protocol, appropriately authorized event data, uncertainty estimates, calibration checks, and an evaluated human-review process. None of those future experiments is claimed as completed here.

## Provenance

This standalone scaffold does not include the code, datasets, model weights, or assets of the third-party ProctorAI application. A fork or evaluation wrapper does not establish authorship of that system.

The original toolkit and documentation were prepared with AI assistance. Contributors should review and validate the work before describing any results as original experimental findings.

## Committed files

- `LICENSE`
- `proctorai_safety_eval_original.zip`

## License

MIT. See [LICENSE](LICENSE).
