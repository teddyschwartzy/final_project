[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)(https://colab.research.google.com/github/teddyschwartzy/final_project/blob/main/Final_Project.ipynb)
# Predicting Mortality Outcomes in HIV/AIDS Patients
## A Machine Learning Analysis of the ACTG 175 Clinical Trial Dataset

### Project Overview

This project explores the use of machine learning techniques to predict patient outcomes among individuals living with HIV/AIDS using the AIDS Clinical Trials Group Study 175 (ACTG 175) dataset from the UC Irvine Machine Learning Repository.

The project combines clinical data analysis, exploratory data analysis (EDA), and predictive modeling to identify factors associated with mortality and evaluate the performance of various binary classification algorithms. In addition to the technical objectives, this project reflects my personal and academic interest in HIV/AIDS research and LGBTQ+ health. As a gay man pursuing graduate studies in health data science, I am interested in understanding how data can be used to improve health outcomes while learning more about a disease that has had a profound impact on LGBTQ+ communities and public health.

Building on the refactoring work from earlier weeks, the notebook converts the analysis into a modular, class-based pipeline that handles data ingestion, validation, preprocessing, statistical testing, and model evaluation in a consistent and reproducible manner.

---

## Research Question

Can demographic characteristics, behavioral risk factors, treatment history, and clinical indicators be used to accurately predict mortality outcomes among individuals enrolled in an HIV/AIDS clinical trial?

---

## Dataset

**Dataset:** AIDS Clinical Trials Group Study 175 (ACTG 175)

**Source:** UCI Machine Learning Repository

**Observations:** 2,139 patients

**Predictor Variables:** 23 features (22 clinical predictors plus a time-to-event variable used for descriptive survival analysis)

**Target Variable:** `cid` (stored as `target` in the pipeline)

- 0 = Alive / Censored
- 1 = Death / Failure Event

### Example Predictor Variables

| Variable | Description |
|-----------|------------|
| age | Age in years at study entry |
| wtkg | Weight in kilograms |
| trt | Randomized treatment arm |
| gender | Sex of the participant |
| hemo | Hemophilia history |
| homo | Homosexual activity indicator |
| drugs | Intravenous drug use history |
| karnof | Karnofsky performance score |
| cd40 | Baseline CD4 count (cells/µL) |
| cd420 | CD4 count at 20 weeks (cells/µL) |
| cd80 | Baseline CD8 count (cells/µL) |
| cd820 | CD8 count at 20 weeks (cells/µL) |
| race | Race category |

---

## Project Objectives

### Data Input and Validation

- Fetch the dataset from the UCI Repository, or allow the user to upload a local copy
- Automated data profiling (dimensions, types, missing values, duplicates)
- Clinical plausibility validation against a data dictionary of expected ranges
- Confirmation of a binary 0/1 outcome before modeling

### Exploratory Data Analysis

- Calculate descriptive statistics
- Examine the distribution of the outcome and key clinical predictors
- Visualize variable relationships and multicollinearity
- Estimate the survival experience by treatment arm

### Inferential Statistics

- Independent-samples t-test (age)
- Chi-square test of independence (gender)
- One-way ANOVA (CD4 counts at 20 weeks across treatment arms)

### Data Preprocessing

- Handle implausible and missing values
- Encode categorical variables when present
- Create a CD4 at 20 weeks x age interaction term
- Split, impute, and scale the data using training-set statistics only

### Predictive Modeling

The following supervised classification models are evaluated:

- Logistic Regression (adaptively class-weighted under imbalance)
- Decision Tree Classifier
- Random Forest Classifier
- Gradient Boosting Classifier

### Model Evaluation

Performance is evaluated using:

- Accuracy
- Precision
- Recall
- F1 Score
- ROC-AUC
- 5-fold cross-validation stability check

ROC-AUC serves as the predefined primary evaluation metric due to its suitability for class-imbalanced, healthcare-related binary classification problems. The champion model is selected programmatically based on this metric.

---

## Automated Pipeline Workflow

The workflow follows the structure below:

1. **Data Import & Validation**
   - Fetches from the UCI Repository or accepts a local upload
   - Profiles shape, types, missing values, and duplicates
   - Validates numeric variables against clinical ranges
2. **Descriptive Statistics**
   - Central tendency and dispersion for all numeric variables
3. **Exploratory Data Analysis**
   - Outcome distribution
   - Age and CD4 count at 20 weeks by outcome
   - Correlation heatmap
   - Kaplan-Meier survival estimate by treatment arm
4. **Inferential Statistics**
   - Independent-samples t-test
   - Chi-square test
   - One-way ANOVA
5. **Preprocessing & Feature Engineering**
   - Categorical encoding (adaptive)
   - CD4 at 20 weeks x age interaction term (adaptive)
   - Stratified 70/30 split
   - Median imputation and standard scaling
6. **Model Training & Evaluation**
   - Four supervised classifiers
   - Test-set metrics plus cross-validation
   - Programmatic champion selection on ROC-AUC
7. **Interpretation**
   - ROC curve overlay
   - Confusion matrix for the champion model
   - Grouped metric comparison
   - Random Forest feature importance

---

## Pipeline Design and Enhancements

For this project, the analysis was reorganized from linear notebook cells into a single class-based pipeline (`HealthAnalysisPipeline`) so that data, preprocessing objects, and model outputs are stored as instance state. This keeps the training and test sets separate, applies the training-set imputation and scaling consistently to the test set, and removes the risk of running cells out of order.

To improve the functionality and rigor of the pipeline, the following enhancements were implemented:

1. **Modular pipeline refactoring** — Each stage is a method, and a `run()` orchestrator executes them in a fixed sequence, making the workflow reproducible and scalable for other clinical datasets.
2. **Automated data quality validation** — Reports class imbalance, missing values, and duplicate rows, and checks numeric variables against a clinical data dictionary of plausible ranges before modeling. Columns not present in the uploaded data are skipped, so the stage adapts to other clinical datasets.
3. **Adaptive feature engineering** — Categorical columns are encoded only if present, and a conditional CD4 at 20 weeks x age interaction term captures non-linear relationships that may be clinically significant in predicting mortality.
4. **Programmatic model selection** — The pipeline evaluates four models and automatically identifies the best-performing model based on a predefined primary metric (ROC-AUC), removing subjectivity from the selection process. Logistic Regression is additionally retrained with balanced class weighting when the event rate falls below the configured threshold.
5. **Cross-validation stability check** — Each model receives a 5-fold cross-validation score to check the stability of the single-split performance estimates.
6. **Expanded evaluation suite** — A ROC curve overlay, a confusion matrix for the champion model, a grouped comparison of classification metrics, and Random Forest feature importance.

---

## Key Findings

- The cohort is moderately imbalanced, with approximately three-quarters of patients alive or censored at the end of follow-up. This imbalance is why the pipeline uses ROC-AUC as the primary metric and applies adaptive class weighting to Logistic Regression.
- Inferential testing identified statistically meaningful associations between baseline characteristics and the outcome (see the test summary table in the notebook), which informed the predictors carried into the models.
- All four classifiers substantially outperformed chance discrimination, and the champion model was selected programmatically on the predefined primary metric.
- Random Forest feature importance placed baseline immune markers and patient age among the most influential predictors, consistent with the HIV clinical literature.

---

## Required Libraries

The notebook uses the following Python packages:

```python
pandas
numpy
matplotlib
seaborn
scipy
scikit-learn
ucimlrepo
```

Pinned versions are provided in `requirements.txt` to match the verified Colab runtime, and the notebook prints the exact installed versions in its first code cell.

---

## Running the Analysis

### Option 1: Google Colab

This project was developed and tested in Google Colab.

1. Open the notebook in Google Colab.
2. Run the installation cell.
3. Execute all notebook cells from top to bottom.

The ACTG 175 dataset is loaded directly from the UCI Machine Learning Repository using the `ucimlrepo` package, so no manual dataset download is required. To use your own copy instead, set `data_source` to `"local"` in the CONFIG cell and upload the file when prompted.

### Option 2: Local Execution

Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
```

Navigate to the project folder and install dependencies:

```bash
cd YOUR_REPOSITORY
pip install -r requirements.txt
```

Launch Jupyter Notebook and run all cells sequentially:

```bash
jupyter notebook
```

Note: the local-upload branch requires a Colab-style `files.upload()` prompt; outside Colab, the pipeline's default UCI Repository fetch works without modification.

---

## Repository Structure

```text
├── Final_Project.ipynb
├── README.md
├── actg175_dataset.csv
└── requirements.txt 
```

---

## Reproducibility

The notebook is designed to execute from start to finish without manual intervention beyond the optional dataset upload. A fixed random seed is applied globally, library versions are verified at runtime, all pipeline settings are centralized in a CONFIG dictionary, and each pipeline stage prints a confirmation banner so another user can verify that every step completed.

---

## Assumptions and Limitations

### Assumptions

- The uploaded or fetched dataset contains a binary outcome coded 0/1.
- Numeric predictor values outside the configured clinical ranges are implausible and are treated as missing.
- The baseline snapshot contains sufficient information to support prediction of the binary outcome.

### Limitations

- Results are specific to the ACTG 175 trial population and may not generalize to other cohorts or treatment eras.
- The binary outcome discards censoring information; a survival model would be a more appropriate inferential framework.
- Performance estimates from a single split are mitigated, but not replaced, by cross-validation.
- Feature importance reflects predictive association, not causation.
- The analysis is for instructional and demonstration purposes only.

---

## Future Directions

- Survival modeling with censored outcomes (e.g., Cox proportional hazards)
- Hyperparameter tuning with random or grid search
- Partial dependence and SHAP analyses for deeper interpretation
- External validation on an independent clinical cohort

---

## Author

Hunter Schwarz

MS Health Data Science

Dartmouth College - Geisel School of Medicine
