# Evaluating Heterogeneity in Factors Influencing the Transport Mode Choice of Madrid's Citizens

This repository contains the research paper and accompanying code for the project:

**Evaluating Heterogeneity in Factors Influencing the Transport Mode Choice of Madrid's Citizens**

The project was completed as part of the **BSc Econometrics and Economics** programme at the **Erasmus School of Economics, Erasmus University Rotterdam**.

## Authors

- Mateusz Rozalski
- Celina Madaschi
- Sanzhar Suleimenov
- Nuria Díaz Jiménez

**Supervisor:** Hakan Akyuz  
**Final version:** April 2025

---

## Overview

Urban travel behaviour is highly heterogeneous: different groups of citizens may respond differently to factors such as travel distance, vehicle availability, weather conditions, household characteristics, and access to public transport.

This project investigates:

> **What are the most important factors influencing the choice of transport of Madrid citizens, and how do they differ across consumer segments?**

The analysis combines **unsupervised learning**, **supervised machine learning**, and **explainable AI** to identify distinct traveller segments and investigate the determinants of transport mode choice within those groups.

The project consists of two main stages:

1. **Traveller segmentation**
   - Latent Class Clustering (LCC)
   - Standard K-Prototypes
   - Eskin-based K-Prototypes

2. **Transport mode choice analysis**
   - Classification and Regression Trees (CART)
   - Random Forest
   - XGBoost
   - SHAP-based model interpretation

---

## Research Paper

The complete research paper is available here:

**[Read the paper](paper/SeminarResearchPaper_Team5.pdf)**

The paper contains the full motivation, literature review, methodology, empirical results, discussion, and references.

---

## Methodology

### 1. Data preprocessing

The original transport survey data is cleaned and transformed before modelling.

The dataset contains variables related to:

- individual socioeconomic characteristics;
- household characteristics;
- trip characteristics;
- transport mode;
- vehicle availability;
- public transport card ownership;
- trip purpose;
- rush-hour travel;
- temperature;
- precipitation.

Weather observations are combined with trip-level information to incorporate environmental conditions into the analysis.

---

### 2. Latent Class Clustering

Latent Class Clustering is used to identify groups of travellers with similar socioeconomic and household characteristics.

Several candidate cluster solutions are compared using criteria including:

- Akaike Information Criterion (AIC);
- Bayesian Information Criterion (BIC);
- Consistent AIC (CAIC);
- average silhouette width;
- normalized entropy.

The final LCC specification identifies **five traveller segments**.

---

### 3. K-Prototypes clustering

Because the dataset contains both numerical and categorical variables, K-Prototypes clustering is considered as an alternative segmentation method.

Two versions are evaluated:

- standard K-Prototypes;
- an extended **Eskin-based K-Prototypes** algorithm.

The Eskin measure modifies the treatment of categorical dissimilarities and gives different weights to mismatches depending on the cardinality of the categorical variable.

Cluster quality and stability are evaluated using measures such as:

- clustering cost;
- average silhouette width;
- bootstrapped Adjusted Rand Index;
- PCA-based visualisations.

---

### 4. Comparison of clustering approaches

Latent Class Clustering and Eskin-based K-Prototypes are compared using several complementary approaches.

A **LightGBM classifier** is trained using the resulting cluster labels as target variables. Classification performance and SHAP feature importance are then used to investigate how clearly and meaningfully each clustering solution separates observations.

The subsequent analysis uses the five-cluster Latent Class solution.

The identified traveller segments are:

1. **Elderly**
2. **Children**
3. **Small Family**
4. **Large Family**
5. **Unemployed**

---

### 5. Classification and Regression Trees

Classification trees are estimated for:

- the complete sample;
- each of the five identified traveller segments.

Hyperparameters are selected using **Optuna**.

The models investigate how variables such as trip distance, vehicle availability, public transport card ownership, demographics, weather and trip characteristics affect the predicted mode of transport.

---

### 6. Random Forest, XGBoost and SHAP

To obtain a more detailed interpretation of transport choice, the analysis combines:

- **Random Forest**
- **XGBoost**

SHAP values are calculated for both models.

Their contributions are combined using model-performance-based weights to produce a **weighted average SHAP measure**.

This allows feature importance to be examined both:

- globally across all transport modes; and
- separately for individual modes of transport and traveller segments.

---

## Main Findings

The analysis finds that several variables consistently play an important role in transport mode choice:

- **trip distance**;
- **availability of a private vehicle**;
- **possession of a public transport card**.

The SHAP analysis reveals additional heterogeneous effects that are less visible in shallower decision trees.

For example:

- weather conditions influence transport choices;
- warmer weather generally increases the likelihood of walking;
- rush-hour travel affects transport preferences;
- the effects of individual variables differ across the identified traveller segments.

The results therefore demonstrate the importance of accounting for **heterogeneity across travellers** instead of estimating only aggregate transport-choice relationships.

---

## Repository Structure

```text
.
├── README.md
│
├── paper/
│   └── SeminarResearchPaper_Team5.pdf
│
├── code/
│   ├── data_preprocessing.ipynb
│   ├── LCA_Code.R
│   ├── kprototype.ipynb
│   ├── bootstrapping.ipynb
│   ├── cluster_classification.ipynb
│   ├── CART_Code.ipynb
│   ├── FullSHAP.ipynb
│   ├── Cluster1SHAP.ipynb
│   ├── Cluster2SHAP.ipynb
│   ├── Cluster3SHAP.ipynb
│   ├── Cluster4SHAP.ipynb
│   └── Cluster5SHAP.ipynb
│
└── data/
    └── README.md
