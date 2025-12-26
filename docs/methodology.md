# 📄 Methodology

**Plant Leaf Disease Detection — From Raw Dataset to Reliable Model v6**

---

## 1. Motivation

Plant leaf disease detection in the wild is far more complex than it appears.
Real agricultural images vary in:

* background and capture environment
* camera quality and compression
* species characteristics
* lesion size and visibility
* label accuracy

The objective of this work was **not** simply to maximize accuracy.
The goal was to build a system that is:

* **reliable** in realistic scenarios
* **interpretable** when it fails
* **honest** about the limits of the data

Ultimately, the guiding question became:

> *What can deep learning truly learn from imperfect agricultural datasets — and when should we stop pushing it?*

---

## 2. Early Assumptions vs. Reality

Initial expectations assumed:

* datasets were clean and representative
* diseases were visually separable
* improving accuracy was mostly a modeling problem

Early exploration disproved these assumptions.

Key discoveries:

* strong **background bias** (lab vs. field images)
* several classes contained **non-leaf images**
* some crops had **grayscale, low-quality, or inconsistent labels**
* multiple diseases were **visually indistinguishable**, even to humans
* external sources contained **noisy or unreliable labels**

This shifted the project toward a **data-centric** approach where modeling decisions followed evidence.

---

## 3. ML Lifecycle Followed

A reproducible lifecycle structured the project:

1. Reproducible environment setup
2. Dataset audit and quality profiling
3. Exploratory data analysis
4. Baseline validation model
5. Progressive fine-tuning
6. Robustness and domain-realistic augmentation
7. Diagnosis, explainability, and calibration
8. Evidence-based model selection

Each stage produced artifacts and informed the next step.

---

## 4. Dataset Audit & Curation

The original dataset contained **86 total classes** across multiple plant species.

### 4.1 Automated Profiling

The audit pipeline detected:

* corrupt images
* perceptual duplicates
* extreme brightness/contrast outliers
* background bias via embedding analysis

### 4.2 Manual Review

Automated findings were verified manually to avoid unsafe removals.

### 4.3 Noisy Crops Removed

Entire crops were removed when signal quality was fundamentally unreliable:

* Wheat
* Rice
* Sugarcane
* Cotton (investigated deeply before removal)

The curated dataset contained **53 classes**, chosen for clarity and consistency.

---

## 5. Baseline Modeling — Proving the Pipeline

A frozen **ResNet-50** with a linear classifier validated:

* correct label alignment
* correct preprocessing
* metrics that behaved logically

This confirmed the implementation was sound — while revealing that **data quality** was the primary limitation.

---

## 6. Progressive Fine-Tuning (EfficientNet Stage)

With a validated pipeline, the model was upgraded to **EfficientNet-B3** and fine-tuned in stages.

### Strategy

* warm-up on classifier head only
* gradual unfreezing
* discriminative learning rates
* mixed precision (AMP)
* label smoothing
* OneCycle scheduling
* early stopping

### Domain-Realistic Augmentation

Augmentations reflected real-world camera behavior:

* lighting and gamma changes
* compression artifacts
* mild blur
* lesion-focused crops

MixUp and CutMix were added **later**, after stability was achieved.

This produced a strong, stable model that became the basis for further work.

---

## 7. Error-Driven Refinement

Instead of chasing higher global accuracy, refinements targeted **observed failure modes**:

* background bias
* over-reliance on leaf shape
* confusion within the same species

Selective sampling, class-specific tweaks, and limited retraining were applied — always validated to avoid regressions.

---

## 8. Diagnosis & Explainability

Explainability was treated as a diagnostic tool.

Tools used:

* Grad-CAM attention maps
* CAM-overlap analysis (lesion vs background)
* per-class confusion tracking
* counterfactual edits (sharpening, cropping, compression)

Findings:

> The model often looked at correct regions — but the visual differences across diseases were sometimes inherently weak or noisy.

This explained why endless fine-tuning plateaued.

---

## 9. Curriculum-Guided Unified Fine-Tune

Explainability insights informed a **curriculum**:

* slightly oversample harder classes
* present confusing pairs together
* apply lesion-enhancing transforms only where useful
* use focal loss *selectively*, not globally

This produced the **v6 unified model** — stable, generalizable, and explainable.

---

## 10. Cotton Case Study — Investigation to Removal

Cotton received special analysis.

### Hypothesis

Grayscale imagery might be breaking pretrained features.

### Experiments

* cotton-only cleaning
* grayscale normalization
* weighted sampling
* hardened augmentations

### Result

Training destabilized repeatedly and reduced global performance.

Conclusion:

> Cotton quality issues were structural, not fixable through modeling.

It was removed as a **data-quality decision**, not an accuracy trick.

---

## 11. Research Trials (Not Deployed)

Experiments explored:

* hierarchical species+disease learning
* ArcFace metric learning
* CBAM attention modules
* species-specific classifier heads

Though interesting academically, they:

* increased complexity
* reduced global stability

They remained **documented experiments**, not production choices.

---

## 12. Calibration & Evaluation

Evaluation included:

* accuracy and weighted F1
* Top-3 accuracy
* per-class metrics
* confusion matrices
* Expected Calibration Error (ECE)

Temperature scaling reduced ECE from **0.13 → 0.002**, aligning confidence with reality — critical for decision-support systems.

---

## 13. Final Model Selection — v6

Model v6 was selected because it:

* balanced accuracy and generalization
* resisted over-specialization
* remained stable across crops
* was explainable and calibrated

Further attempts plateaued or regressed — reinforcing the decision to stop.

---

## 14. Limitations

* several diseases remain visually ambiguous
* label noise persists in parts of the dataset
* performance degrades when backgrounds dominate leaf visibility

These are acknowledged openly rather than hidden.

---

## 15. Key Learnings

* **Data quality > model complexity**
* Explainability reveals *why* models behave as they do
* Accuracy alone can mislead
* Calibration matters in real tools
* Knowing when to stop is a core ML skill

---

## 16. Conclusion

This project evolved into a full investigation of **applied, responsible machine learning**.

The primary contribution is not just a model — but a reproducible approach:

> audit → analyze → explain → correct → validate → stop when limits are reached.

---