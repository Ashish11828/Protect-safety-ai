# Protect Safety AI
Safety and reliability evaluation for automated exam-proctoring alerts
� � � �
Protect Safety AI is a lightweight, reproducible AI safety and reliability evaluation toolkit for binary alerts produced by remote-exam proctoring systems. It examines false alarms, missed events, performance under different lighting and noise conditions, and which alerts may need additional scrutiny.
Research question: How reliable are automated proctoring alerts across environmental conditions, and how can human review reduce harm from mistaken flags?
[!IMPORTANT] This repository does not train or provide an AI proctoring model. The supplied 12-event dataset is synthetic, created to demonstrate the analysis pipeline. The example metrics do not establish real-world detection performance, fairness, robustness, or accuracy. A flagged event must never be treated as proof of cheating.
Motivation
AI monitoring systems can make mistakes. Incorrectly identifying a student as suspicious can have serious consequences, especially when lighting, background noise, or an imperfect confidence score affects predictions.
The goal is to evaluate model decisions rather than automatically punish people. The project emphasizes:
Reliability: quantify false-positive and false-negative rates.
Context sensitivity: compare error rates across lighting and noise categories.
Human oversight: recommend review for all positive flags, with lower-confidence flags explicitly identified.
Reproducibility: accept a documented CSV format and produce structured JSON reports.
Privacy-conscious analysis: work with event-level labels rather than raw video, audio, or face images.
Current capabilities
Capability
Implementation
Confusion-matrix counts
True positives, true negatives, false positives, false negatives
Core metrics
Accuracy, precision, recall, false-positive rate, false-negative rate
Environmental breakdown
Separate reports by lighting and noise
Review-priority signal
IDs of positive predictions below a configurable confidence threshold
Input validation
Missing columns, invalid labels/confidence, duplicates, empty datasets
Output
Human-readable or saved JSON report
Tests
Four Python unittest tests
The requires_human_review_for_all_positive_flags value in the report is a policy recommendation, not a working human-review interface or enforcement mechanism.
Repository layout
At present, the runnable source is uploaded as proctorai_safety_eval_original.zip. Extract it to find:
Protect-safety-ai/
├── README.md                          # This overview
├── LICENSE
├── proctorai_safety_eval_original.zip
└── proctorai-safety-eval/              # Created after extraction
    ├── src/evaluate.py                # Evaluation CLI and metric calculations
    ├── data/synthetic_events.csv      # Illustrative 12-event dataset
    ├── tests/test_evaluate.py         # Four unit tests
    ├── README.md                      # Toolkit documentation
    └── .gitignore
The extracted directory is not yet a separate folder in the GitHub repository; it lives inside the ZIP. Publishing these files directly in the repository later would make the code easier to review.
Quick start
Requirements: Python 3.9 or later. No third-party packages are required for the evaluator.
git clone https://github.com/Ashish11828/Protect-safety-ai.git
cd Protect-safety-ai

# Extract the included project archive
unzip proctorai_safety_eval_original.zip
cd proctorai-safety-eval

# Generate a JSON evaluation report
python src/evaluate.py data/synthetic_events.csv --output report.json

# Run the unit tests
python -m unittest discover -s tests -v
On Windows, you can extract the ZIP using File Explorer or PowerShell's Expand-Archive, then run the Python commands from the extracted proctorai-safety-eval folder.
Change the confidence cutoff:
python src/evaluate.py data/synthetic_events.csv --review-threshold 0.8 --output report.json
The default review threshold is 0.7. It is illustrative, not an empirically validated decision threshold.
Input data format
Provide a UTF-8 CSV with these columns:
Column
Meaning
event_id
Unique event identifier
ground_truth_violation
Reference label: 0 or 1
predicted_violation
Model's binary alert: 0 or 1
confidence
Model-supplied number from 0 to 1
lighting
Condition label, e.g. bright or dim
noise
Condition label, e.g. quiet or noisy
Example synthetic event:
event_id,ground_truth_violation,predicted_violation,confidence,lighting,noise
E002,0,1,0.55,dim,quiet
For real assessments, defining trustworthy reference labels requires a carefully reviewed annotation protocol. A gaze shift, another voice, or other anomaly is not automatically evidence of misconduct.
Example output from synthetic data
Running the supplied dataset produces:
Metric
Illustrative value
Events
12
True positives / true negatives
3 / 4
False positives / false negatives
3 / 2
Accuracy
58.33%
Precision
50.00%
Recall
60.00%
False-positive rate
42.86%
False-negative rate
40.00%
In this fabricated demonstration, accuracy is 83.33% for bright and 33.33% for dim examples. These numbers are artifacts of a tiny synthetic dataset, not measured behaviour of any deployed system.
At the default 0.7 threshold, E002 and E011 appear in low_confidence_positive_event_ids. All positive alerts still require review regardless of score.
How the metrics are computed
Precision: TP / (TP + FP)
Recall: TP / (TP + FN)
False-positive rate: FP / (FP + TN)
False-negative rate: FN / (FN + TP)
Accuracy: (TP + TN) / N
Undefined rates are returned as null in JSON when the denominator is zero.
AI safety scope and limitations
This is an applied safety evaluation starter, not frontier-model alignment research or mechanistic interpretability research.
The dataset is too small and synthetic to make statistically supported claims about any model.
Grouping by lighting/noise is a narrow environmental robustness analysis, not a demographic fairness audit.
No AI model is trained, independently validated, or deployed by this toolkit.
Confidence values may be uncalibrated; low-confidence screening must not be treated as a verified risk estimate.
Real-world testing should use appropriate consent, data minimization, security practices, and human oversight.
High-confidence flags can still be wrong. No automated allegation or penalty should follow from a prediction alone.
Suggested next research steps
Define a documented, ethically reviewed protocol for collecting or simulating controlled alert events.
Obtain authorised, consented prediction data without publishing personal or biometric information.
Evaluate performance across prespecified lighting and noise conditions, reporting group sample sizes.
Add confidence calibration checks and uncertainty intervals for estimates.
Test a documented human-review process for ambiguous alerts.
Publish reproducible experiments, negative findings, and limitations.
These are future directions, not completed experiments.
Provenance and attribution
This is a standalone evaluation scaffold. It does not include the application code, datasets, model weights, or other assets of the third-party ProctorAI repository. The projects should not be confused, and possession of a fork or evaluation wrapper does not establish authorship of the underlying proctoring system.
The initial toolkit and documentation were prepared with AI assistance. Users and contributors should review, understand, and validate the code and any subsequent empirical research before representing the results as original experimental findings.
License
MIT License. See LICENSE.
Maintainer: Ashish Kumar · Area: trustworthy AI, safety evaluation, human-centered monitoring, reliability research.
