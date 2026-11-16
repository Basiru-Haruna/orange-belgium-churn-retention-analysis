# Orange Belgium — Customer Churn Prediction & Retention Analytics

**Portfolio project | Python · pandas · scikit-learn · CatBoost · statistical evaluation**

## Business objective

Identify customers at risk of leaving a telecommunications provider and investigate whether retention-campaign targeting might improve on conventional risk ranking. **Risk prediction and causal response prediction are different problems.** The anonymized features do not allow strong explanations of why a customer leaves.

## Dataset

The anonymized Orange Belgium churn/uplift dataset has **11,896 records and 180 columns**: 160 principal-component numeric variables (`PC1`–`PC160`), 18 categorical factors (`FACTOR1`–`FACTOR18`), outcome `y` (`1`=churn), and treatment assignment `t` (`1`=assigned to retention campaign). There are **408 churn events (3.43%)**. `FACTOR3` is constant and excluded from the 177-predictor matrix. The training/validation/test split is **7,137 / 2,379 / 2,380**, stratified jointly on treatment and churn with `random_state=42`.

**The raw CSV is intentionally excluded from this repository.** Obtain it through the original dataset/source documentation and place it at `data/raw/Orange Belgium_churn_uplift_anonymized.csv`. Do not upload private customer data or source data without permission. The analysis assumes the same anonymized public/research dataset and column schema.

## Notebook sequence

| Notebook | Question answered | Main methods |
|---|---|---|
| [01 — Audit and splitting](notebooks/01_business_understanding_and_data_audit.ipynb) | What does the dataset contain and how are we evaluating fairly? | EDA, missingness, constants, joint stratified splitting |
| [02 — Churn prediction](notebooks/02_churn_prediction.ipynb) | Which customers are likely to churn? | Dummy baseline, Logistic Regression, weighting, Random Forest, CatBoost, AP/ROC-AUC, thresholds, Top-K, calibration, training-only CV, locked test |
| [03 — Retention uplift](notebooks/03_customer_retention_uplift.ipynb) | Whose churn outcome might be affected by campaign assignment? | Balance/propensity diagnostics, T- and S-Learners, exploratory Qini-style ranking, stratified bootstrap, fixed test assessment |

## Key results

### Churn prediction (Notebook 2)

The original CatBoost was retained because it had the strongest validation Average Precision (AP **0.2061**, ROC-AUC **0.8200**). A training-only hyperparameter search did **not** improve held-out validation AP. On the reserved test split, original CatBoost at threshold **0.05** achieved:

| Test metric | Result |
|---|---:|
| ROC-AUC | **0.8437** |
| Average Precision | **0.1975** |
| Precision / Recall | 0.2123 / 0.3827 |
| F1 / Accuracy | 0.2731 / 0.9307 |
| Precision@100 / Recall@100 | 0.2100 / 0.2593 |

It flagged 146 test customers, 31 of whom churned (115 false positives), while 50 churners were missed. These values were observed during the original experiment and should be verified when the source CSV is available.

### Retention uplift (Notebook 3)

Validation exploratory Qini-style areas were **+27,895.83 (churn-risk ranking)**, **−14,562.97 (T-Learner)**, and **−10,214.45 (S-Learner)**. Test-set values were **+12,902.22**, **+11,521.76**, and **+37,129.95**, respectively. Validation and test rankings changed substantially. Bootstrap intervals for the principal validation model comparisons encompassed zero. Thus **no targeting strategy was demonstrated to be causally superior**. The S-Learner's highest test score must **not** be used retroactively to declare it the winning strategy.

**Causal limitations:** `t` denotes campaign *assignment*, not contact or acceptance. The original study describes randomization but exact customer/campaign-level assignment probabilities were not confirmed here; exploratory Qini-style curves use overall arm proportions and are not definitive policy-value estimates. Rare events, observed arm imbalances, limited outcome-model calibration, and one held-out test split restrict conclusions. Model-generated individual uplift scores are not observed individual causal effects.

## Reproduce locally

1. Create a Python 3.10+ environment and install packages: `pip install -r requirements.txt`.
2. Place the **original** dataset at `data/raw/Orange Belgium_churn_uplift_anonymized.csv` (excluded from version control).
3. Launch Jupyter from the repository root: `jupyter lab`.
4. Run the notebooks **in numerical order**. Each notebook re-creates identical row-index splits and can be run independently once the data is supplied.
5. Check the generated metrics against the values above. Some notebook outputs are saved from earlier runs; they are **not substitutes for full local reproduction**.

Model fitting and hyperparameter search may take time on CPU. The project is a reproducible **learning and research portfolio**, not a production retention decision engine.

## Project structure

```text
notebooks/
  01_business_understanding_and_data_audit.ipynb
  02_churn_prediction.ipynb
  03_customer_retention_uplift.ipynb
data/raw/                   # obtain source data yourself; ignored by git
figures/                    # reserved for exported graphics
requirements.txt
.gitignore
README.md
LICENSE
```

## Responsible use

No identifiable customer records, private credentials, or unauthorized original datasets should be committed. Statistical associations and model explanations do not by themselves establish causal mechanisms. Future work should verify campaign assignment probabilities, estimate policy value with justified designs, replicate findings on independent cohorts, and assess retention economics.
