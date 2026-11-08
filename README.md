# Diabetes Prediction Replication and New-Data Extensions

**COMP8240 — Applications of Data Science | Macquarie University | Assignment 3**

This repository reproduces and examines the XGBoost–ADASYN diabetes-prediction approach reported by Tasin et al. (2023), and extends the investigation to a separate public dataset and two experiments involving constructed Pima-style profiles. This is an academic reproducibility and data-quality study, **not a clinically validated diagnostic tool**.

## Repository contents

| File | Purpose |
| --- | --- |
| `COMP8240_Tasin_Replication.ipynb` | Replication on Pima and RTML data; comparison with published results and additional methodological checks. |
| `COMP8240_New_Dataset_Extension.ipynb` | Separate XGBoost–ADASYN modelling of UCI Early Stage Diabetes Risk Prediction (dataset 529), including exact-duplicate analysis. |
| `COMP8240_Synthetic_Dataset_Extension.ipynb` | Original experiment generating 200 independently sampled, unlabelled Pima-style profiles and obtaining model predictions. |
| `COMP8240_Supplementary_Synthetic_Quality_Improvement.ipynb` | **Additional** synthetic-data quality experiment using complete Pima training-record feature combinations with small perturbations and quality checks. Does **not** replace the original experiment. |
| `COMP8240_constructed_diabetes_dataset.csv` | Original constructed profiles. |
| `COMP8240_correlated_synthetic_profiles_200.csv` | Profiles from the supplementary experiment. |
| `COMP8240_synthetic_quality_audit.csv` | Per-feature supplementary quality-audit summary. |
| `requirements.txt` | Python dependency list; install before running the notebooks. |

## Data sources and access

1. **Pima Indians Diabetes dataset:** Obtain the standard `diabetes.csv` file with columns `Pregnancies`, `Glucose`, `BloodPressure`, `SkinThickness`, `Insulin`, `BMI`, `DiabetesPedigreeFunction`, `Age`, and `Outcome`. The replication and synthetic notebooks require this local file. Ensure you use the same 768-row Pima version used in the report.
2. **RTML dataset:** The replication notebook also requires `RTML without Insulin.xlsx` from the original research materials. This file is **not shown in the repository listing**; obtain the appropriate original file before running the replication. Without it, the complete replication cannot be rerun.
3. **UCI Early Stage Diabetes Risk Prediction:** Dataset ID **529** is fetched within the new-dataset notebook using `ucimlrepo`, so internet access is needed. This dataset has different features from Pima; a separately fitted model is used rather than treating the datasets as directly interchangeable.

Data-source availability and permissions should be checked before redistributing original datasets.

## How to run (Google Colab)

1. Open a notebook from this repository in Google Colab.
2. Install the dependencies listed in `requirements.txt` (for example, using `pip install -r requirements.txt` in an environment where the file is available). Some notebooks also contain Colab package-install cells.
3. Upload `diabetes.csv` to the Colab `/content/` directory for the replication and both synthetic notebooks. For the replication notebook, also upload `RTML without Insulin.xlsx` to `/content/`.
4. Run each notebook **from top to bottom** using **Runtime → Run all**. The supplementary notebook reads `diabetes.csv` from the current working directory, which is `/content/` by default in Colab.
5. The UCI notebook fetches dataset 529 online; it does not need the Pima or RTML files.
6. Compare outputs with the results reported in the final Assignment 3 report. The synthetic notebooks are separate experiments and their results must not be interchanged.

**Reproducibility note:** The supplementary notebook uses random seed 42. Saved notebook outputs are not a substitute for rerunning the code with the required source files and dependencies. Package-version differences can affect some results.

## Experiments and reported findings

### 1. Original replication

The combined Pima/RTML replication reports approximately **74.43% accuracy, F1 0.6087, and ROC-AUC 0.7949**. The study also discusses preprocessing before the train/test split as a possible source of leakage and reports an additional leakage-controlled analysis. These are experimental comparisons, not clinical performance claims.

### 2. Independent public-data extension

The UCI dataset has 520 rows, including **269 exact duplicates**; 251 unique rows remain after deduplication. The reported held-out accuracy after deduplication is **88.24%**. The notebook uses a separately trained model because its symptom-based features are not directly compatible with the Pima measurement-based features.

### 3. Original constructed-data experiment

The original notebook generates **200 hypothetical Pima-style profiles** by independently sampling feature values. The reported predictions are **106 negative and 94 positive**. These are **model-assigned predictions**, not verified diagnoses; there are no ground-truth outcome labels with which to calculate accuracy on these constructed profiles.

### 4. Supplementary synthetic-data quality experiment

This is an **additional experiment, not a replacement** for the original 200-profile experiment or its reported predictions.

- Split the original Pima data into training and holdout subsets; use training data only to construct synthetic profiles.
- Select donor records with **nonzero glucose, blood pressure, skin thickness, insulin, and BMI** measurements.
- Sample complete donor feature combinations and add small numerical perturbations to create 200 new profiles. Sampling whole records is intended to preserve feature relationships better than independently sampling each feature.
- Audit feature distributions, correlations, missing values, and exact duplicates; export the synthetic profiles and the audit summary to CSV.

The supplementary audit previously reported **zero exact synthetic duplicates** and an **average absolute pairwise-correlation difference of about 0.045** relative to the selected donor subset. These measures provide limited evidence of statistical similarity, **not** proof of clinical realism or privacy protection. They should be confirmed by rerunning the notebook.

**Important limitations:** Filtering for nonzero medical measurements changes the donor population and may introduce selection bias. Perturbed records are derived from existing observations and should not be assumed independent of, or private relative to, the source data. The supplementary profiles have **no independently verified outcome labels**, so they cannot be used to establish diagnostic accuracy. Statistical resemblance to the donor subset does not guarantee representativeness of the full Pima population.

## Interpretation and ethics

- Neither the UCI model nor the synthetic-data experiments establish clinical generalisation or readiness for medical deployment.
- The UCI and Pima datasets have different feature definitions and populations; their accuracy figures should not be treated as directly comparable evaluations of the same fitted model.
- Duplicate records, data leakage, sampling choices, and missing or implausible clinical values can distort evaluation.
- Synthetic profiles and model outputs are for coursework and methodological investigation only, not individual medical advice.

## Reference

Tasin et al. (2023), diabetes prediction using machine learning, *Healthcare Technology Letters*. DOI: https://doi.org/10.1049/htl2.12039

## Submission note

The final report is the primary account of the methods and results. If the supplementary experiment is discussed in the report, it must be clearly identified as an additional experiment; the original synthetic experiment's results must remain unchanged.
