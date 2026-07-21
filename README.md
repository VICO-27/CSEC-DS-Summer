# CSEC-DS-Summer

Summer sprint project for the CSEC-ASTU Club Data Science Division.

**Author:** Ashenafi Deresa

---

## About the Project

This repo tracks my progress on the **GeoAI Aquaculture Pond Identification Challenge**, hosted on Zindi by FAO (Food and Agriculture Organization) and ITU (AI for Good).

**Goal:** Build a machine learning model that predicts whether a 10m × 10m satellite patch is an aquaculture pond or not, using time-series features derived from Sentinel-1 (SAR) and Sentinel-2 (optical) satellite imagery.

**Key challenge:** The model is trained on one time period and tested on a different one — so it has to generalize across seasons and years, not just memorize patterns in the training data. Test data is also partially masked (only 4-6 consecutive months visible out of 12), while training data is complete.

🔗 [Challenge page on Zindi]([https://zindi.africa](https://zindi.africa/competitions/geoai-aquaculture-pond-identification-challenge))

---

## Repo Structure




---

## Progress So Far

### Week 1 — Data Understanding & EDA
- Explored dataset structure: 1,821 train rows / 1,030 test rows, 12 monthly composites × 12 bands (S1 radar + S2 optical).
- Found train is fully complete (no missing months), while test is heavily masked — each test row only shows 4-6 consecutive months.
- Discovered train and test come from **different acquisition periods**, confirmed via adversarial validation (~0.99 AUC separability between domains).

### Week 2 — Feature Engineering & Feature Selection
- Built window-position-agnostic features: per-band aggregate stats (mean/std/min/max/trend) computed only over valid months.
- Added spectral indices (NDVI, NDWI, MNDWI) and calibration-resistant normalized bands to reduce sensitivity to sensor drift.
- Simulated test-like masking on training data (random 4-6 month windows) to close the missingness gap between train and test.
- Tested feature selection (curated vs. full feature set) — found the full feature set generalizes better on the real leaderboard.

### Week 3 — Modeling & Evaluation *(in progress)*
- Trained LightGBM, CatBoost, and XGBoost with GroupKFold cross-validation.
- Tried domain-shift mitigation techniques: quantile normalization, adversarial reweighting, calibration-noise augmentation, pseudo-labeling.
- Current best leaderboard score: **0.819 blended (F1 + AUC)**.

---

## Key Lessons Learned

- Offline cross-validation can be badly miscalibrated when train/test come from different distributions — real leaderboard submissions became the most trustworthy signal.
- More features ≠ overfitting by default; cutting features to "simplify" the model actually hurt real performance here.
- Ensembling multiple models doesn't fix a shared domain-shift blind spot — it only reduces variance.

---

## Tools & Libraries

`pandas`, `numpy`, `scikit-learn`, `LightGBM`, `CatBoost`, `XGBoost` — developed and run in Google Colab.

---

## Status

🚧 Actively in progress — Summer Break Sprint, CSEC-ASTU Data Science Division.
