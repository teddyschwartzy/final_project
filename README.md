https://colab.research.google.com/assets/colab-badge.svg](https://colab.research.google.com/drive/1q_N0GQSw9cFm12FKMS4tuGefETixdczC?usp=sharing)

# Predicting Mortality Outcomes in HIV/AIDS Patients
## A Machine Learning Analysis of the ACTG 175 Clinical Trial Dataset

### Project Overview

This project explores the use of machine learning techniques to predict patient outcomes among individuals living with HIV/AIDS using the AIDS Clinical Trials Group Study 175 (ACTG 175) dataset from the UC Irvine Machine Learning Repository.

The project combines clinical data analysis, exploratory data analysis (EDA), and predictive modeling to identify factors associated with mortality and evaluate the performance of various binary classification algorithms. In addition to the technical objectives, this project reflects my personal and academic interest in HIV/AIDS research and LGBTQ+ health. As a gay man pursuing graduate studies in health data science, I am interested in understanding how data can be used to improve health outcomes while learning more about a disease that has had a profound impact on LGBTQ+ communities and public health.

---

## Research Question

Can demographic characteristics, behavioral risk factors, treatment history, and clinical indicators be used to accurately predict mortality outcomes among individuals enrolled in an HIV/AIDS clinical trial?

---

## Dataset Information

**Dataset:** AIDS Clinical Trials Group Study 175 (ACTG 175)

**Source:** UCI Machine Learning Repository

**Observations:** 2,139 patients

**Predictor Variables:** 23 features

**Target Variable:** `cid`

- 0 = Alive / Censored
- 1 = Death / Failure Event

### Example Predictor Variables

- Age
- Weight
- Treatment Group
- Hemophilia History
- Intravenous Drug Use History
- Homosexual Activity Indicator
- Karnofsky Performance Score
- CD4 Count
- CD8 Count
- Hemoglobin Levels
- Race
- Gender

---

## Running in Google Colab

This project was developed and tested in Google Colab.

To run the notebook:

1. Open the notebook in Google Colab.
2. Run the installation cell below.
3. Execute all notebook cells from top to bottom.

### Required Installation

```python
!pip install ucimlrepo
```

The ACTG 175 dataset is loaded directly from the UCI Machine Learning Repository using the `ucimlrepo` package, so no manual dataset download is required.

---

## Project Objectives

### Exploratory Data Analysis

- Examine dataset structure and variable types
- Calculate descriptive statistics
- Assess missing data
- Identify potential data quality issues
- Explore variable distributions
- Visualize relationships among predictors

### Data Preprocessing

- Handle missing values
- Encode categorical variables
- Scale numerical variables when appropriate
- Evaluate class balance
- Prepare data for machine learning models

### Predictive Modeling

The following models will be evaluated:

- Logistic Regression
- Decision Tree Classifier
- Random Forest Classifier
- Gradient Boosting Models

### Model Evaluation

Performance will be evaluated using:

- Accuracy
- Precision
- Recall
- F1 Score
- ROC-AUC

ROC-AUC will serve as the primary evaluation metric due to its suitability for healthcare-related binary classification problems.

---

## Exploratory Visualizations

Initial exploratory analyses include:

### Distribution of Survival Outcomes

Examines class balance within the binary target variable and identifies potential class imbalance concerns.

### Baseline CD4 Count by Outcome

Compares differences in immune system status across patient outcome groups.

### Age Distribution

Explores the demographic characteristics of the study population and identifies the overall age profile of participants.

### Correlation Heatmap

Evaluates relationships among numerical predictors and highlights potential multicollinearity concerns.

---

## Technologies

- Python
- Google Colab
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- UCI ML Repository API (`ucimlrepo`)

---

## Future Work

Future project phases will include:

- Feature engineering
- Model tuning and optimization
- Cross-validation
- Feature importance analysis
- Model comparison
- Clinical interpretation of predictive factors
- Discussion of ethical considerations and health equity implications
