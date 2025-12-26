# 🌿 Plant Leaf Disease Detection

**Reliable ML for imperfect agricultural datasets**

---

## Overview

This project builds a **robust and explainable deep-learning system** for plant leaf disease detection.

Unlike benchmark demos, real agricultural data contains:

* noisy and inconsistent labels
* background bias (lab vs field)
* grayscale / low-quality samples
* visually similar diseases
* unpredictable lighting and compression artifacts

Instead of chasing raw accuracy, this project focuses on:

✔ reliability
✔ interpretability
✔ honest evaluation
✔ reproducible engineering practices


---

## Project Goals

* Build a model that works **beyond controlled datasets**
* Diagnose **what the model is actually learning**
* Use explainability to guide corrections
* Stop where the dataset reaches its realistic limits
* Document a transparent, reproducible ML pipeline

---

## Dataset Summary

The dataset used in this project was created by merging and cleaning multiple open datasets from Kaggle.
I removed duplicates, low-quality images, mislabeled classes, and biased categories, and reorganized everything into a unified format.

👉 **Kaggle Dataset:** https://www.kaggle.com/datasets/adiithape1/plant-disease-detection-dataset-master-version

* Original: **86 classes**
* Final curated: **53 classes**

Entire crops were removed due to inconsistent or unreliable signal:

> Wheat, Rice, Sugarcane, Cotton

Cotton underwent a full investigation before removal (documented in the methodology).

Details:
👉 See **docs/methodology.md**

---

## ML Lifecycle

This repository follows an evidence-driven lifecycle:

1️⃣ Dataset audit & cleaning
2️⃣ EDA & quality profiling
3️⃣ Baseline validation model
4️⃣ Progressive fine-tuning (EfficientNet-B3)
5️⃣ Robust augmentations
6️⃣ Diagnosis & explainability
7️⃣ Calibration
8️⃣ Final model selection (v6)

All decisions are documented.

---

## Model Architecture

* Backbone: **EfficientNet-B3**
* Training strategy:

  * warm-up → gradual unfreeze
  * discriminative learning rates
  * AMP (mixed precision)
  * label smoothing
  * OneCycle LR
* Augmentations:

  * realistic lighting/gamma shifts
  * compression artifacts
  * mild blur
  * lesion-focused crops
* Later additions:

  * MixUp
  * CutMix

Experimental branches (not deployed) include hierarchical learning, ArcFace, and CBAM — documented but intentionally not used.

---

## Explainability & Diagnostics

Explainability was used as a **debugging tool**:

* Grad-CAM
* CAM overlap (lesion vs background)
* confusion tracking
* counterfactual edits (crop, sharpen, compression)

These insights guided curriculum-style fine-tuning and helped identify noisy classes.

---

## Evaluation

Beyond raw accuracy, evaluation includes:

* macro & weighted F1
* Top-3 accuracy
* per-class reports
* confusion matrices
* Expected Calibration Error (ECE)

Calibration reduced ECE:

> **0.13 → 0.002**
> meaning model confidence now reflects real probability — essential for deployment.

---

## Final Model — v6

Model v6 was selected because it:

* generalizes well across species
* avoids over-specialization
* remains stable under distortions
* is explainable and calibrated

Attempts beyond v6 showed diminishing or negative returns — and were stopped intentionally.

---

## Limitations

* some diseases are visually indistinguishable
* residual label noise exists
* models struggle when leaves are dominated by background
* field deployment requires human-in-loop validation


---


## What This Project Demonstrates

✔ data-centric ML thinking
✔ diagnostic use of explainability
✔ responsible stopping criteria
✔ transparent documentation


---

## 📄 More Details

Read the full methodology:

👉 **docs/methodology.md**

---
