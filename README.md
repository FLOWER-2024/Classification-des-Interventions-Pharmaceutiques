# 💊 Pharmaceutical Intervention Classification with NLP & Machine Learning

> **NLP · Machine Learning · Clinical Pharmacy · Medication Safety · Health Data**

[![Python](https://img.shields.io/badge/Python-3.x-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)](https://jupyter.org/)
[![Scikit--learn](https://img.shields.io/badge/scikit--learn-Machine%20Learning-F7931E?logo=scikit-learn&logoColor=white)](https://scikit-learn.org/)
[![XGBoost](https://img.shields.io/badge/XGBoost-Gradient%20Boosting-189FDD)](https://xgboost.readthedocs.io/)
[![NLP](https://img.shields.io/badge/NLP-Text%20Classification-purple)](#)
[![Healthcare](https://img.shields.io/badge/Domain-Healthcare-red)](#)

---

## 📌 Overview

This project explores the use of **Natural Language Processing (NLP)** and **supervised machine learning** to automate the classification of **pharmaceutical interventions (PIs)** recorded during the clinical review of hospital prescriptions.

The project uses pharmacists' comments and prescribed molecule information to address two complementary classification tasks:

1. **Detection of potentially serious pharmaceutical interventions**
2. **Classification of pharmaceutical interventions into 11 SFPC categories**

The project is positioned at the intersection of:

- clinical pharmacy;
- medication safety;
- natural language processing;
- machine learning;
- healthcare data science.

> **Academic / research project — the models presented here are not clinically validated decision-support tools and must not be used for individual patient care.**

---

# 🎯 Research Question

> **Can natural language processing and machine learning be used to automatically identify potentially serious pharmaceutical interventions and classify pharmaceutical interventions according to the SFPC classification?**

---

# 🎯 Objectives

## Main objective

Develop a reproducible NLP and machine learning pipeline capable of automatically classifying pharmaceutical interventions from clinical pharmacy data.

## Secondary objectives

The project aims to:

- preprocess and normalize pharmacist comments;
- transform clinical text into numerical representations;
- integrate domain-specific features;
- handle class imbalance;
- compare multiple machine learning algorithms;
- optimize classification thresholds;
- evaluate binary and multiclass classification performance;
- investigate the feasibility of automated pharmaceutical intervention classification.

---

# 🏥 Clinical Context

Pharmaceutical interventions are an important component of medication safety and clinical pharmacy practice.

During prescription review, pharmacists may identify situations such as:

- overdosing;
- contraindications;
- drug interactions;
- inappropriate prescribing;
- non-indicated treatments;
- other medication-related problems.

Automating part of this classification process could potentially support:

- prioritization of high-risk situations;
- analysis of pharmaceutical intervention activity;
- large-scale retrospective studies;
- medication safety monitoring.

However, automated predictions should be considered as **decision-support research outputs**, not replacements for pharmacist expertise.

---

# 🗃️ Data

The project uses pharmaceutical intervention data from:

**Hôpitaux Universitaires de Strasbourg (HUS)**

The dataset contains information related to pharmaceutical interventions, including pharmacist comments, prescribed molecules and intervention classes.

## Main files

| File | Description |
|---|---|
| `data_defi3.csv` | Main dataset used for analysis and model development |
| `SFPC_encodage.csv` | Mapping table for SFPC intervention categories |
| `valid_set.csv` | Validation dataset |
| `predictions_test_set.csv` | Model predictions generated on the test set |
| `predictions_valid_set.csv` | Model predictions generated on the validation set |
| `defi_2_VERSION_FINALE .ipynb` | Main analysis and modelling notebook |

---

# 🔐 Data Governance & Confidentiality

Healthcare data require particular attention to:

- confidentiality;
- data governance;
- access control;
- authorized reuse;
- regulatory compliance.

The repository should only contain data that are authorized for storage and redistribution.

If the underlying data are subject to institutional or access restrictions, the corresponding raw files should **not be redistributed publicly**.

---

# 🔬 Methodology

The overall analytical workflow is:

```text
Clinical Pharmacy Data
        │
        ▼
Data Cleaning
        │
        ▼
Text Preprocessing
        │
        ▼
Domain-specific Features
        │
        ▼
TF-IDF Representation
        │
        ▼
Class Imbalance Handling
        │
        ▼
Machine Learning Models
        │
        ▼
Cross-validation / Evaluation
        │
        ▼
Threshold Optimization
        │
        ▼
Final Predictions
