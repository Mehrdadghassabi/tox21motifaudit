# tox21motifaudit

**Context Sensitivity and Correlation Balancing for Motif-Driven Toxicity Prediction on Tox21**

An exploratory audit of motif–label associations in fingerprint-based toxicity models on the
Tox21 benchmark. This repository holds the analysis notebooks, preprocessing configuration,
SMARTS motif definitions, fold assignments, and result tables behind the manuscript of the
same title.

> **Status: exploratory.** This study does **not** provide a causal test of internal model
> reliance, and the direction-based motif–label conflict rule used here is unreliable in
> several motif–assay pairs. Read the results as hypothesis-generating, not as evidence of
> shortcut reliance.

---

## Overview

Structural motifs can correlate with assay labels in molecular property prediction without
fully explaining the endpoint, raising the possibility that models exploit such associations
as shortcuts. This work audits that issue on Tox21 using 2,048-bit Morgan fingerprints
(radius 2) and five-fold structure-grouped cross-validation, comparing five learning
algorithms across all 12 assays and six SMARTS-defined motifs.

The study has **two sequential components**:

1. **Discovery screen** — a prior Random Forest–only analysis that evaluated within-motif
   discrimination across all 12 assays and nominated **alkyl halides** for follow-up.
2. **Harmonized full run** — a multi-model pipeline (RF, Extra Trees, XGBoost, LightGBM, MLP)
   applied to all 12 assays under a unified data-cleaning and cross-validation protocol.

The two runs use different duplicate-handling and evaluation details. **Their numbers are
reported separately and must not be read as a strict replication of one another.**

---

## Key findings

- Within-motif ROC-AUC was highest for **Extra Trees** (mean 0.680 over 41 eligible
  motif–assay groups) and **Random Forest** (0.674); the **MLP** was weakest (0.592).
- **Alkyl halides** were the most predictable motif: mean within-motif ROC-AUC of 0.739
  (Extra Trees) and 0.737 (RF).
- Approximate fingerprint-bit masking gave at most a small conflict log-loss gain
  (RF, +0.024) and **lowered overall ROC-AUC for all three tree models**.
- Correlation balancing reduced conflict log loss by 0.54–0.61 but **increased overall log
  loss by 0.11–0.18** — a trade-off, not evidence of improved generalization.
- The direction-based conflict rule can misassign expected labels under class imbalance;
  negative agreement–conflict gaps and undefined subgroup AUCs are concrete warning signs.
