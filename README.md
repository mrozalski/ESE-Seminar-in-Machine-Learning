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

Urban travel behaviour is heterogeneous: different groups of citizens may respond differently to factors such as trip distance, vehicle availability, public transport access, household characteristics, weather conditions, and rush-hour travel.

This project investigates the following research question:

> **What are the most important factors influencing the choice of transport of Madrid citizens, and how do they differ across consumer segments?**

The analysis combines **unsupervised learning**, **supervised machine learning**, and **explainable AI** to identify traveller segments and study the determinants of transport mode choice within those groups.

The project consists of two main stages:

1. **Traveller segmentation**
   - Latent Class Clustering (LCC)
   - Standard K-Prototypes
   - Eskin-based K-Prototypes

2. **Transport mode choice analysis**
   - Classification and Regression Trees (CART)
   - Random Forest
   - XGBoost
   - LightGBM
   - SHAP-based interpretation

The paper compares the suitability of Latent Class Clustering and an enhanced K-Prototypes algorithm using the Eskin measure for mixed-type data.

---

## Research Paper

The complete research paper is available here:

**[Read the paper](ResearchPaper.pdf)**

The paper contains the full motivation, literature review, methodology, empirical results, discussion, and references.

---

## Methodology

### 1. Data preprocessing

The analysis uses transport survey data from the **Madrid Transport Consortium (CRTM)** and supplements it with meteorological information.

The dataset contains:

- trip-specific variables;
- individual socioeconomic characteristics;
- household characteristics;
- transport mode information;
- vehicle availability;
- public transport card ownership;
- trip purpose;
- rush-hour indicators;
- temperature;
- precipitation.

The original CRTM survey contains 222,744 observations. Hourly weather data is aggregated into trip-specific weather measures.

---

### 2. Latent Class Clustering

Latent Class Clustering is used to identify groups of travellers with similar socioeconomic and household characteristics.

Candidate cluster solutions are evaluated using:

- Akaike Information Criterion (AIC);
- Bayesian Information Criterion (BIC);
- Consistent Akaike Information Criterion (CAIC);
- average silhouette width;
- normalized entropy.

The final Latent Class solution identifies **five traveller segments**.

---

### 3. K-Prototypes Clustering

Because the dataset contains both numerical and categorical variables, K-Prototypes is considered as an alternative segmentation method.

Two variants are evaluated:

- standard K-Prototypes;
- **Eskin-based K-Prototypes**.

The Eskin-based extension modifies the dissimilarity measure used for categorical variables.

Cluster quality and stability are evaluated using:

- clustering cost;
- average silhouette width;
- bootstrapped Adjusted Rand Index;
- PCA-based visualisations.

---

### 4. Comparison of Clustering Approaches

Latent Class Clustering and Eskin-based K-Prototypes are compared using multiple approaches.

A **LightGBM classifier** is trained using the cluster labels as target variables. Classification performance and SHAP feature importance are used to assess how clearly and meaningfully the resulting clusters can be distinguished.

The analysis ultimately proceeds with the five-cluster Latent Class solution.

The identified groups are:

1. **Elderly**
2. **Children**
3. **Small Family**
4. **Large Family**
5. **Unemployed**

These five groups are the final segments used in the subsequent transport-mode analysis.

---

### 5. Classification and Regression Trees

Classification and Regression Trees are estimated for:

- the full sample;
- each of the five identified traveller segments.

Hyperparameters are selected using **Optuna**.

The models are used to investigate how variables such as:

- trip distance;
- car availability;
- public transport card ownership;
- demographics;
- household characteristics;
- weather;
- rush-hour travel;

affect the predicted mode of transport.

---

### 6. Random Forest, XGBoost and SHAP

To obtain a more detailed interpretation of transport-mode choice, the analysis combines:

- **Random Forest**
- **XGBoost**

SHAP values are calculated for both models.

The SHAP values are then combined using model-performance-based weights, producing a weighted average SHAP measure.

This allows feature importance to be examined:

- globally across all transport modes;
- separately for individual transport modes;
- separately across the identified traveller segments.

---

## Main Findings

The analysis finds that several variables consistently play an important role in transport mode choice:

- **trip distance**;
- **availability of a private car**;
- **possession of a public transport card**.

The SHAP analysis provides additional insights beyond the shallower decision trees.

The results indicate that:

- weather conditions influence transport choice;
- warmer weather generally increases the likelihood of walking;
- rush-hour travel affects transport preferences;
- the effects of individual variables differ across traveller segments.

The paper therefore highlights the importance of accounting for **heterogeneity across travellers** rather than analysing transport-mode choice only at the aggregate level.

---

## Repository Structure

```text
ESE-Seminar-in-Machine-Learning/
│
├── README.md
├── ResearchPaper.pdf
│
└── code/
    │
    ├── data_preprocessing.ipynb
    ├── LCA_Code.R
    ├── kprototype.ipynb
    ├── bootstrapping.ipynb
    ├── cluster_classification.ipynb
    ├── CART_Code.ipynb
    ├── FullSHAP.ipynb
    ├── Cluster1SHAP.ipynb
    ├── Cluster2SHAP.ipynb
    ├── Cluster3SHAP.ipynb
    ├── Cluster4SHAP.ipynb
    ├── Cluster5SHAP.ipynb
    │
    └── Datasets/
        ├── Madrid, Comunidad de Madr... 2018-02-08 to 2018-06-11.csv
        ├── hourly1.csv
        ├── hourly2.csv
        ├── hourly3.csv
        ├── hourly4.csv
        ├── full_df.csv
        ├── full_df_with_cluster.csv
        ├── cluster_lca_5.csv
        ├── kprot_clusters.csv
        ├── eskin_clusters.csv
        ├── kprototypes_costs.csv
        └── eskin_costs.csv
