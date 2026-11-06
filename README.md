# Predicting Student Academic Success Using Machine Learning

**Auburn University • COMP 1100 • Fall 2026**  
**Team:** Konley Stoup and Camden Ball  
**Tools:** WEKA 3.8.6 and Microsoft Excel  
**Project type:** Machine-learning classification experiment, not a deployed application

## Overview

We tested whether machine-learning models could predict whether a student passes or fails a mathematics course, and how much knowing their first-period grade (G1) improves the prediction. The dataset contains **395 students** from Portuguese secondary schools.

Students were classified as **Pass** if final grade G3 was at least 10 on a 0–20 scale; otherwise **Fail**. There were 265 Pass records and 130 Fail records. The final grade was used to create the label **only**, never as an input to the models.

## Experiment design

We prepared two feature sets in Excel:

- **Without grades:** 30 student characteristics plus PassFail; G1, G2, and G3 excluded.
- **With G1:** those same characteristics plus first-period grade G1; G2 and G3 excluded.

In WEKA we evaluated **J48 Decision Tree** (`-C 0.25 -M 2`) and **Logistic Regression** (`-R 1.0E-8 -M -1`) on both datasets using **10-fold stratified cross-validation**, for four configurations.

## Verified results

| Classifier | Accuracy | Fail recall | Fail precision | ROC AUC | Failing students identified |
|---|---:|---:|---:|---:|---:|
| Always-Pass baseline | 67.1% | 0.000 | N/A | 0.500 | 0 / 130 |
| J48 — without grades | 66.3% | 0.369 | 0.485 | 0.572 | 48 / 130 |
| Logistic — without grades | 68.6% | 0.369 | 0.533 | 0.672 | 48 / 130 |
| J48 — with G1 | **84.3%** | **0.777** | 0.754 | 0.820 | **101 / 130** |
| Logistic — with G1 | **84.3%** | 0.769 | **0.758** | **0.921** | 100 / 130 |

The major improvement came from including G1: accuracy rose by about 16–18 percentage points, and the number of correctly identified failing students increased from 48 to about 100. Nonetheless, the stronger models missed 29–30 of the 130 failing students.

### Visualization

![Accuracy comparison](results/charts/accuracy.svg)

![Failing students correctly identified](results/charts/failing_students_identified.svg)

## Error analysis

The confusion matrices in WEKA used rows = actual Fail/Pass and columns = predicted Fail/Pass.

| Run | Actual Fail predicted Fail | Actual Fail predicted Pass | Actual Pass predicted Fail | Actual Pass predicted Pass |
|---|---:|---:|---:|---:|
| J48 — without grades | 48 | 82 | 51 | 214 |
| Logistic — without grades | 48 | 82 | 42 | 223 |
| J48 — with G1 | 101 | 29 | 33 | 232 |
| Logistic — with G1 | 100 | 30 | 32 | 233 |

A false negative for the Fail class is a student who actually fails but is predicted to pass. Avoiding these missed cases matters when the goal is identifying students who might need additional support.

## How to reproduce

1. Obtain the [UCI Student Performance dataset](https://archive.ics.uci.edu/dataset/320/student+performance), specifically the **mathematics** CSV.
2. Create a nominal PassFail outcome from G3 ≥ 10 and convert labels into fixed values before removing G3.
3. Make one CSV without G1/G2/G3 and one CSV with G1 but without G2/G3. Keep all 30 non-grade attributes, including `school`, and set `PassFail` as the class in WEKA.
4. Open each dataset in WEKA Explorer. Choose J48 or Logistic, select 10-fold cross-validation, and record the outputs.
5. Compare the metrics to `results/metrics.csv`. The numbers in this repository were taken from the team's saved WEKA outputs; no retraining was performed to produce them.

**Dataset availability:** The original prepared CSV files were not available to upload in this conversation, so they have not been fabricated or replaced. See [data/README.md](data/README.md) for the expected filenames.

## Limitations and responsible use

- The dataset represents **Portuguese secondary-school students**, not Auburn University students.
- Some features, especially cumulative absences, may not have been available at the first grading period. This is **not** a validated real-time early-warning system.
- The dataset includes sensitive or potentially unfair attributes. Any real educational use would require fairness and privacy review, external validation, consent and human oversight.
- Predictions do not themselves show that an intervention would improve performance. The relationship between G1 and G3 is predictive, not causal.

## Team

This project was completed collaboratively by **Konley Stoup and Camden Ball**, with an approximately equal division of responsibilities. Konley focused more on WEKA experiments, while Camden focused more on mid-project report documentation; both worked on data preparation, evaluation, reporting and presentation. See [team credits](docs/TEAM.md).

## AI assistance and attribution

The team used ChatGPT and Claude for planning, troubleshooting, explanations, and document organization. Modeling was performed with existing WEKA software, and charts summarize recorded model results. This is a student course project, **not a Python-trained model or deployed prediction service**.

**Data:** Cortez, P. & Silva, A. (2008), *Using data mining to predict secondary school student performance*, UCI Machine Learning Repository.  
**Software:** [WEKA Machine Learning Workbench](https://www.cs.waikato.ac.nz/ml/weka/).
