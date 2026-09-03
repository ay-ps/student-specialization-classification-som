# Classification of Study Specialization in Informatics Education Using Self-Organizing Maps

![Python](https://img.shields.io/badge/Python-3.x-blue) ![Jupyter](https://img.shields.io/badge/Notebook-Jupyter-orange) ![Method](https://img.shields.io/badge/Method-SOM%20%2B%20SMOTE-9cf) ![Framework](https://img.shields.io/badge/Framework-CRISP--DM-lightgrey)

An interpretable, data-driven framework for classifying student academic specialization tendencies in Informatics Education, built on **CRISP-DM**, **Self-Organizing Maps (SOM)**, and **SMOTE**. This repository contains the full implementation used in the accompanying research paper, from data preparation to model evaluation.

## Table of Contents

- [Background](#background)
- [Research Workflow](#research-workflow)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Repository Structure](#repository-structure)
- [Results](#results)
- [Key Findings](#key-findings)
- [Limitations](#limitations)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [Publication](#publication)
- [Authors](#authors)

## Background

Choosing an academic specialization track is a pivotal decision that shapes a student's learning trajectory and career readiness, yet it is frequently driven by subjective factors - peer influence, family encouragement - rather than objective academic performance. This mismatch between a student's actual competencies and their chosen track can weaken academic outcomes and reduce graduate competitiveness.

This project proposes **SOM** as an unsupervised, interpretable approach to this problem. Unlike supervised methods that require pre-labeled data, SOM projects high-dimensional academic records onto a two-dimensional topological grid, revealing natural groupings in student profiles without label dependency, while **SMOTE** addresses the class imbalance inherent in specialization data.

**Contributions of this work:**
1. Implementing SOM within the CRISP-DM framework for objective student specialization classification.
2. Applying a systematic grid search across 960 hyperparameter configurations to identify the optimal SOM model.
3. Generating an interpretable two-dimensional topological map as a foundation for academic specialization decision-support systems.

## Research Workflow

The project follows the **CRISP-DM** (Cross-Industry Standard Process for Data Mining) framework end to end, from business understanding through evaluation.

<img src="images/Research%20%20Methodology.png" width="40%" alt="Research Workflow">

## Dataset

- **141** academic records from Informatics Education students at Universitas Negeri Jakarta, spanning the **2022–2023** cohorts.
- Input features: **11 core course grades**, grouped into three specialization domains:

| Specialization Domain | Core Courses |
|---|---|
| **Software Engineering** | Algorithm and Programming Structure, Database, Software Engineering, Data Structure |
| **Computer and Network Engineering** | Computer Networks, Data Communication, Operating Systems, Computer Organization and Architecture |
| **Multimedia** | Human-Computer Interaction, Web Design, Multimedia Systems |

- **Target classes:** Software Engineering, Computer and Network Engineering, Multimedia.
- The dataset exhibits natural class imbalance, with Software Engineering as the majority class and Computer and Network Engineering as the minority class.

> **Note:** The raw dataset is not included in this repository, as it contains student academic records. Only the model implementation source code is provided.

## Methodology

### 1. Data Preparation
- **Data Cleaning** - handling missing values and ensuring consistency.
- **Min-Max Normalization** - scaling all academic features to a [0, 1] interval.
- **Feature Selection and Grouping** - aggregating core course grades into the three specialization domains.
- **Cubic Transformation** - redistributing high-density grade values to reduce the "crowding effect" and improve feature discriminability.
- **Data Splitting** - a strict **80:20 hold-out** split (112 training / 29 testing samples), with SMOTE and hyperparameter tuning strictly confined to the training subset to prevent data leakage.
- **Data Resampling** - two parallel training scenarios: the original imbalanced set (**Baseline**) and a SMOTE-balanced set (**SMOTE**).

### 2. Modeling
- An exhaustive **grid search** was run across both scenarios, covering map dimensions up to 20×20, learning rates (0.1–0.9), sigma / neighborhood radius (0.5–3.0), and iteration counts (1,000–15,000) - **960 configurations** in total.
- The optimal SOM was trained to project the academic feature vectors onto a 2D topological grid.
- **Neuron labeling via majority voting:**
  1. Each training sample is assigned to its Best Matching Unit (BMU) by minimizing Euclidean distance.
  2. Each neuron is labeled by the statistical mode (majority class) of its mapped samples.
  3. Unmapped neurons inherit the label of their nearest labeled neighbor by grid topological distance.
- **Model selection heuristic:** classification accuracy is the primary criterion; ties are broken first by Quantization Error (QE), then by Topographic Error (TE).

### 3. Evaluation
Two complementary evaluation lenses were used:
- **Clustering quality** - QE (average distance between input vectors and their BMUs; lower = better data-mapping resolution) and TE (proportion of samples whose first and second BMUs are not adjacent; lower = better topology preservation).
- **Classification performance** - Accuracy, Precision, Recall, and F1-Score derived from the confusion matrix.

## Repository Structure

```
.
├── notebooks/
│   ├── SOM_Baseline.ipynb      # Baseline SOM pipeline (no SMOTE)
│   └── SOM_SMOTE.ipynb         # SOM pipeline with SMOTE-balanced training data
├── images/
│   ├── Alur Penelitian.svg               # CRISP-DM research workflow 
│   ├── U-Matrix Baseline.svg             # U-Matrix, baseline model 
│   ├── U-Matrix SMOTE.svg                # U-Matrix, SMOTE model
│   ├── SOM Classification Map Baseline.svg  # Labeled neuron map, baseline 
│   └── SOM Classification Map SMOTE.svg     # Labeled neuron map, SMOTE
├── .gitignore
└── README.md
```

## Results

### Optimal Hyperparameters

Grid search converged on different optimal configurations for each scenario:

| Parameter | Baseline | SMOTE |
|---|---|---|
| Map Size | 20×20 | 15×15 |
| Learning Rate | 0.5 | 0.3 |
| Sigma | 3.0 | 1.5 |
| Iterations | 5,000 | 15,000 |
| Neighborhood Function | Triangle | Gaussian |

### SOM Map Visualization

**U-Matrix** - visualizes average weight distances between neighboring neurons; darker regions indicate homogeneous clusters, lighter regions indicate transitional boundaries.

| Baseline | SMOTE |
|---|---|
| ![U-Matrix Baseline](images/U-Matrix%20Baseline.svg) | ![U-Matrix SMOTE](images/U-Matrix%20SMOTE.svg) |

The baseline U-Matrix shows compact clusters with a visible transition zone, indicating partially overlapping academic profiles. The SMOTE-balanced U-Matrix distributes cluster boundaries more evenly, reflecting the more proportional representation of all three specializations.

**Classification Map** - specialization labels assigned to each neuron after majority-vote labeling.

| Baseline | SMOTE |
|---|---|
| ![SOM Classification Map Baseline](images/SOM%20Classification%20Map%20Baseline.svg) | ![SOM Classification Map SMOTE](images/SOM%20Classification%20Map%20SMOTE.svg) |

In the baseline map, Software Engineering dominates the topological area, reflecting its majority-class status. After SMOTE, the specialization regions become substantially more balanced, with Multimedia and Computer and Network Engineering occupying larger, better-defined areas.

### Clustering Quality

| Metric | Baseline | SMOTE |
|---|---|---|
| Quantization Error (QE) | 0.0003 | 0.0024 |
| Topographic Error (TE) | 0.0625 | 0.0513 |

The baseline achieves a lower QE (finer mapping resolution on the original, less dense data), while SMOTE achieves a lower TE (better-preserved topological structure thanks to the balanced training distribution).

### Classification Performance

| Model | Accuracy | Correct Predictions (of 29 test samples) |
|---|---|---|
| Baseline SOM | **79%** | 23 / 29 |
| SOM + SMOTE | **83%** | 24 / 29 |

**Per-class highlights:**
- **Software Engineering** - strongest baseline performer (F1 = 0.82, recall = 0.93); precision further improved from 0.74 to 0.81 with SMOTE.
- **Multimedia** - weakest baseline performer (F1 = 0.71, recall = 0.62) due to minority-class under-detection; SMOTE completely resolved this, lifting recall to a perfect 1.00.
- **Computer and Network Engineering** - perfect baseline precision (1.00) but recall trade-off; recall declined further under SMOTE (0.67 → 0.50, F1 0.80 → 0.67) as synthetic samples introduced some overlap with adjacent classes.

## Key Findings

- SMOTE improves overall accuracy and largely fixes minority-class (Multimedia) under-detection, at the cost of some precision/recall trade-off for Computer and Network Engineering.
- Discrepancies between model predictions and students' actual enrollment choices suggest that **non-academic factors** (peer influence, family encouragement) also shape specialization decisions - academic performance alone is not a sufficient sole criterion.
- SOM is best positioned as an **objective, interpretable decision-support tool** for academic advising, rather than a deterministic specialization-assignment system.

## Limitations

- The dataset is limited to 141 records from a single institution, which may limit generalizability.
- Evaluation relies on a single hold-out split rather than cross-validation; model stability across different partitions has not been fully examined.
- A rigorous 80:20 split with QE as a tiebreaker was enforced to mitigate overfitting risk given the small dataset.

**Future work:** cross-validation, larger multi-institutional datasets, incorporating non-academic variables, and benchmarking against supervised classifiers.

## Tech Stack

- **Python**
- [`pandas`](https://pandas.pydata.org/) - reading and processing the dataset
- [`numpy`](https://numpy.org/) - numerical operations and feature transformation
- [`scikit-learn`](https://scikit-learn.org/) - normalization, train/test split, and evaluation metrics
- [`MiniSom`] - building and training the Self-Organizing Map
- [`imbalanced-learn`](https://imbalanced-learn.org/) - applying SMOTE to the training data

## Getting Started

```bash
pip install pandas numpy scikit-learn minisom imbalanced-learn
```

```bash
jupyter notebook notebooks/SOM_Baseline.ipynb
# or
jupyter notebook notebooks/SOM_SMOTE.ipynb
```

## Publication

**Classification of Study Specialization in Informatics Education using Self-Organizing Maps**
Presented at the 4th International Conference on Information Technology and Computing (ICITCOM 2026), held under The 10th International Conference on Sustainable Innovation (ICoSI), Universitas Muhammadiyah Yogyakarta.

## Authors
**Ayu Parnida Sinaga** - Department of Informatics and Computer Engineering Education, Universitas Negeri Jakarta
