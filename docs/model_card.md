#  Model Card — Plant Leaf Disease Detection (Model v6)

## 1. Model Summary

**Model name:** PlantLeaf-v6
**Task:** Multi-class image classification (plant leaf diseases)
**Architecture:** EfficientNet-B3 (fine-tuned)
**Outputs:** Top-k disease predictions + calibrated confidence scores
**Intended use:** Decision-support aid — **not** an automated diagnosis tool

Model v6 is the final, stable model trained in this project.
It prioritizes **reliability, generalization, and calibrated confidence** over chasing marginal accuracy gains.

---

## 2. Intended Use

### Primary (Recommended)

* Educational demos on **data-centric ML**
* Research and experimentation on:

  * dataset bias
  * explainability
  * calibration
* Decision-support for agronomists **with human review**
* Prototyping early agricultural AI tools

### Out-of-Scope (Not Recommended)

* Fully automated plant disease diagnosis
* High-stakes farming recommendations without human verification
* Deployment on unseen crops not represented in training
* Medical, safety-critical, or regulatory-compliant use cases

> This model **assists** — it does not replace expert judgment.

---

## 3. Dataset

### Source Overview

* Original dataset contained **86 classes**
* Mixed lab + field environments
* Significant noise, bias, and label uncertainty

### Curation Decisions

The following entire crops were removed due to unreliable signal:

* Wheat
* Rice
* Sugarcane
* Cotton (deeply investigated before removal)

Final training set: **53 curated disease classes**

### Known Dataset Issues

* Some diseases are visually indistinguishable
* Residual label noise remains
* Background context sometimes correlates with class
* Certain species are over-represented

These issues motivated the emphasis on **explainability and calibration** throughout the project.

---

## 4. Training & Methodology (Brief)

* Backbone: **EfficientNet-B3**
* Strategy:

  * head-only warm-up → gradual unfreeze
  * discriminative learning rates
  * AMP (mixed precision)
  * label smoothing
  * OneCycle LR
* Augmentations:

  * lighting / gamma variation
  * compression artifacts
  * mild blur
  * lesion-focused crops
* Later:

  * MixUp + CutMix (soft targets)
* Curriculum-guided refinement informed by explainability diagnostics

Full methodology:
👉 **docs/methodology.md**

---

## 5. Evaluation

### Metrics Reported

* Accuracy
* Macro / Weighted F1
* Top-3 accuracy
* Per-class metrics
* Confusion matrices
* **Expected Calibration Error (ECE)**

### Calibration

Temperature scaling reduced ECE:

> **0.13 → 0.002**

Confidence probabilities now map closely to real correctness likelihood — critical for decision support.

---

## 6. Strengths

* Performs well across curated species
* Robust to realistic distortions (lighting, compression)
* Confidence values are calibrated
* Explainability tools included (Grad-CAM, CAM overlap)
* Engineering pipeline is reproducible and transparent

---

## 7. Limitations

* Struggles with visually ambiguous diseases
* Sensitive to residual label noise
* Background-heavy images degrade accuracy
* Does not generalize to crops outside the curated set
* Not evaluated in real farm deployment environments

---

## 8. Ethical & Responsible Use

This model should **not** be used as the sole basis for agricultural decisions.

Recommended practices:

* keep a **human-in-the-loop**
* treat model outputs as *suggestions*, not ground truth
* prioritize farmer safety and crop risk mitigation
* clearly disclose model uncertainty to end-users
* re-train and validate on **local farm datasets** before deployment

---

## 9. Monitoring & Updates

If deployed:

* monitor misclassifications
* track confidence vs correctness drift
* re-evaluate when:

  * new crops are added
  * capture conditions change
  * new labeled data becomes available

Future improvements may include:

* small specialist heads per species (carefully integrated)
* semi-supervised cleaning
* active-learning loops for ambiguous samples

---

## 10. Citation (if referenced)

If you reference this model in reports or portfolios, cite as:

> *PlantLeaf-v6: A calibrated and explainable deep-learning model for plant leaf disease classification under imperfect data conditions (2025).*

---

## 11. Contact / Maintenance

Maintainer: **Advait Nitin Ithape**
Status: **Active but not production-hardened**

---
