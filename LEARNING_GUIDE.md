# Study and Interview Guide

Review each notebook by answering four questions: **What did we do? Why? What did we observe? What can we legitimately conclude?**

## Notebook 1 — Business framing and data

The objective is to identify churn risk and explore whether retention campaign assignment affects churn. The dataset contains 11,896 anonymized customers, 180 columns, and 408 churn events (3.43%). `y` is churn; `t` is treatment assignment; `FACTOR3` was constant and removed from predictors. Use joint `t × y` stratification for a 60/20/20 train/validation/test split. High majority-class accuracy does not guarantee churn identification.

## Notebook 2 — Prediction

Establish a majority baseline, then compare Logistic Regression, weighted Logistic Regression, Random Forest, and CatBoost. Use training-only preprocessing pipelines to avoid leakage. ROC-AUC measures discrimination; Average Precision summarizes precision and recall across thresholds. Threshold choice controls the precision–recall trade-off. Top-K measures ranking quality for a fixed contact budget; calibration measures probability accuracy. Original CatBoost led on validation AP. Its locked test ROC-AUC was 0.8437, AP 0.1975, precision 0.2123 and recall 0.3827 at threshold 0.05. Precision@100 was 0.21.

## Notebook 3 — Uplift and causal caveats

Assess balance and treatment propensity before uplift. A T-Learner trains one outcome model per arm; an S-Learner trains one model using treatment as a feature. Estimated uplift (control churn risk minus treated churn risk) is not an observed individual causal effect. Compare ranking performance using Qini-style curves and stratified bootstrap intervals. The model ranking changed between validation and test. Validation paired bootstrap intervals included zero: no method was demonstrated to be superior. Campaign assignment differs from confirmed contact. Overall propensity weights require assumptions about the assignment mechanism.

## Interview rehearsal

1. Describe the business question in two sentences.
2. Explain why the 96.55% majority baseline was inadequate.
3. Contrast precision, recall, ROC-AUC and AP.
4. Explain how `Pipeline` reduces leakage.
5. Explain threshold tuning and Precision@K.
6. Explain why the original CatBoost was retained after tuning.
7. Distinguish churn risk from response to treatment.
8. Explain propensity overlap, T- and S-Learners, Qini, and bootstrap uncertainty.
9. State why the highest test score cannot be used for post hoc model selection.
10. Describe the data and methodological requirements before a retention policy could be deployed.

**Portfolio description:** Developed an end-to-end telecom churn prediction pipeline (held-out CatBoost ROC-AUC 0.844; AP 0.198), compared exploratory T- and S-Learner retention targeting strategies, and reported uncertainty and limitations transparently.
