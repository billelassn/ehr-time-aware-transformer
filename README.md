# Dynamic Event Trajectory Modeling for Early Sepsis Prediction

**Authors:** Billel Aissani, Lou-Ann Le Grand, François Chapuis
**Context:** Deep Learning for Electronic Health Records (EHR) Project

## Project Overview
[cite_start]Sepsis is a leading cause of mortality in ICUs (approx. 20% of global deaths)[cite: 10]. [cite_start]Early prediction is critical as mortality increases by 7.6% for every hour of delayed treatment[cite: 13].

The goal of this project is to predict sepsis onset by modeling the full trajectory of patient events, rather than relying on static snapshots at admission.

## Dataset & Access
[cite_start]We used the **MIMIC-IV v3.1** dataset (approx. 29,000 selected patient trajectories)[cite: 66, 71].

**Important:** This dataset is restricted. Due to the PhysioNet Data Use Agreement (DUA), **no data files are included in this repository**. To reproduce the experiments:
1. Obtain credentialed access via [PhysioNet](https://physionet.org/).
2. Download MIMIC-IV.
3. Run the preprocessing scripts.

## Methodology
We treated patient history as a temporal sequence (similar to NLP sentences) and compared three architectures:

1.  [cite_start]**Baseline:** XGBoost with Bag of Words approach[cite: 94].
2.  [cite_start]**Sequential:** LSTM (Long Short-Term Memory) to capture time dependencies[cite: 125].
3.  [cite_start]**Proposed Model:** Time-Aware Transformer (GPT-like causal architecture)[cite: 152].
    * Features: Dual embedding (Categorical codes + Continuous time).
    * Loss: Binary Focal Loss to handle class imbalance.

![Model Architecture](./images/architecture.png)
*Figure 1: Overview of the Time-Aware Transformer architecture.*

## Results
We evaluated the models on AUROC and AUPRC metrics.

| Model | AUROC | AUPRC | Recall (Sepsis) |
|-------|-------|-------|-----------------|
| XGBoost | 0.794 | 0.437 | - |
| LSTM | 0.840 | 0.537 | 70% |
| **Transformer** | **0.840** | **0.520** | **73%** |

## Interpretability
While performance is similar to LSTM, the Transformer offers explainability through Attention Maps. [cite_start]The model successfully identifies clinical shifts, such as moving focus from routine care events to critical biomarkers (e.g., High Lactate) when they appear[cite: 213, 215].

![Attention Heatmap](./images/heatmap.png)
*Figure 2: Attention weights shifting to 'High Lactate' event.*

## Limitations
* [cite_start]**Performance Ceiling:** Results plateaued at AUPRC ~0.52 using structured data alone[cite: 228].
* [cite_start]**Future Work:** Integrating unstructured clinical notes (NLP) could further improve performance by capturing narrative context[cite: 237].
