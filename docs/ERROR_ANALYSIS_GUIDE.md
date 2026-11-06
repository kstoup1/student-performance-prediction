# Understanding Our WEKA Model Errors

**Project:** Predicting Student Academic Success Using Machine Learning  
**Authors:** Konley Stoup and Camden Ball  
**Documentation contribution:** Camden Ball

## Why error analysis matters

The goal of this project is not just to predict whether a student will pass or fail, but to identify students who may need academic support. For that goal, **Fail recall** and the kinds of mistakes a classifier makes matter as much as overall accuracy.

We trained two classifiers, J48 Decision Tree and Logistic Regression, on the UCI mathematics student-performance dataset (395 records). Each classifier was evaluated with and without the first-period grade, G1, using **10-fold stratified cross-validation**. The models were trained and evaluated in WEKA; this document explains the recorded outputs and does not add new experiments.

## How to read the confusion matrices

For this project, **Fail** is the positive class.

- **True positive (TP):** An actual failing student predicted to Fail.
- **False negative (FN):** An actual failing student predicted to Pass — a missed warning.
- **False positive (FP):** An actual passing student predicted to Fail — a false alarm.
- **True negative (TN):** An actual passing student predicted to Pass.

The original WEKA confusion matrices use rows for the actual class and columns for the predicted class, in the order **Fail, Pass**.

| Model | TP: Fail correctly flagged | FN: Fail missed | FP: Pass falsely flagged | TN: Pass correctly predicted |
|---|---:|---:|---:|---:|
| J48, without grades | 48 | 82 | 51 | 214 |
| Logistic Regression, without grades | 48 | 82 | 42 | 223 |
| J48, with G1 | 101 | 29 | 33 | 232 |
| Logistic Regression, with G1 | 100 | 30 | 32 | 233 |

Each row accounts for all 395 students: **130 actual Fail** and **265 actual Pass**.

## Three measurements we used

**Accuracy** = (TP + TN) / total students.  
**Fail recall** = TP / (TP + FN): of all students who actually failed, how many were correctly flagged?  
**Fail precision** = TP / (TP + FP): of students flagged Fail, how many actually failed?

For **Logistic Regression with G1**:

- Accuracy = (100 + 233) / 395 = **84.3%**
- Fail recall = 100 / 130 = **0.769**
- Fail precision = 100 / (100 + 32) = **0.758**
- The model missed **30** failing students and incorrectly flagged **32** passing students.

## What changed when G1 was included?

Without grades, **both models identified just 48 of 130 failing students** and missed 82. Adding G1 increased correct detection to **101 with J48** and **100 with Logistic Regression**. The G1-inclusive models both reached **84.3% accuracy**, compared with 66.3% and 68.6% without grades.

The always-Pass baseline gets **67.1% accuracy** because most students pass, but it identifies **zero** failing students. This shows why a high overall accuracy can hide poor performance on the outcome we care about.

The G1 models also differ despite equal accuracy: J48 had slightly higher Fail recall (0.777 vs. 0.769), while Logistic Regression with G1 had a higher ROC AUC (0.921 vs. 0.820). AUC summarizes how well a model ranks risk across decision thresholds; it is not the same as accuracy at one threshold.

## What these results do *not* tell us

- The confusion matrices give **aggregate counts**, not the identities or reasons behind each individual mistaken prediction.
- We cannot assume similar accuracy for Auburn University students: the dataset comes from **Portuguese secondary schools**.
- Some recorded inputs, particularly total absences, may not have been available at an actual first-period prediction date.
- Sensitive student attributes raise fairness and privacy questions. These experiments did not include a fairness audit.
- Predicting failure does not prove that any specific action will prevent failure.

## Presentation takeaway

> Adding the first-period grade greatly improved our ability to identify students who eventually failed. However, the stronger models still missed 29–30 of 130 failing students. We therefore see these predictions as possible support for educator judgment, not a replacement for it.

## Evidence

These numbers are from the team's original WEKA text outputs and are also summarized in [the main project README](../README.md) and [metrics.csv](../results/metrics.csv). No additional training was performed for this document.
