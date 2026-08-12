# EEG Seizure Classification with Biomedical Features

A reproducible EEG seizure-classification pipeline using **nine interpretable biomedical signal-processing features** and classical machine learning.

## Overview

This project classifies EEG segments as **seizure** or **non-seizure** using explicitly defined signal-processing features rather than training a deep network directly on raw or undocumented inputs.

The feature set combines:

* **Five frequency-band power features:** Delta, Theta, Alpha, Beta, and Gamma
* **Three Hjorth parameters:** Activity, Mobility, and Complexity
* **Shannon entropy**

These nine features are extracted from each EEG segment and evaluated using **Logistic Regression** and **Random Forest** classifiers.

The project was developed as a follow-up to the submitted research work:

> **Adaptive Bottleneck Networks with Learnable Latent Compression for EEG Epileptic Seizure Classification**

The parent work used BEED's X1–X16 features, but their original feature-computation methodology is not fully documented. This project therefore takes an alternative, interpretable approach by constructing standard biomedical signal-processing features directly from EEG recordings.

---

## Dataset

The project uses the **UCI Epileptic Seizure Recognition dataset**, derived from the Bonn University EEG recordings.

* **11,500** EEG segments
* **178 samples** per segment
* Approximately **1 second** per segment
* Original task: **5 classes**
* Binary task used here:

  * **2,300 seizure segments**
  * **9,200 non-seizure segments**

The original five-class labels are collapsed into a binary classification problem where seizure activity is separated from all other conditions.

---

## Feature Extraction

Each EEG segment is converted into a 9-dimensional feature vector.

### 1. Frequency-Domain Features

Welch's method is used to estimate spectral power across five frequency bands:

| Band  | Frequency Range |
| ----- | --------------: |
| Delta |        0.5–4 Hz |
| Theta |          4–8 Hz |
| Alpha |         8–13 Hz |
| Beta  |        13–30 Hz |
| Gamma |        30–45 Hz |

### 2. Hjorth Parameters

Three standard time-domain EEG descriptors are calculated:

* **Activity** — signal variance
* **Mobility** — estimated mean frequency of the signal
* **Complexity** — change in frequency characteristics

### 3. Shannon Entropy

Shannon entropy is calculated from the signal amplitude distribution to capture its complexity and information content.

**Total: 9 features per EEG segment**

---

## Machine Learning

Two classical machine-learning models are evaluated:

### Logistic Regression

A linear baseline with feature standardization.

### Random Forest

An ensemble classifier using multiple decision trees to capture nonlinear relationships between the engineered EEG features and seizure labels.

Both models use feature scaling within the evaluation pipeline.

---

## Evaluation

The models are evaluated using:

* **5-fold stratified cross-validation**
* **70/30 stratified held-out test split**
* Accuracy
* Precision
* Recall
* F1-score
* Confusion matrix
* Random Forest feature importance

---

## Results

### 5-Fold Stratified Cross-Validation

| Model               |   Accuracy |  Precision |     Recall |         F1 |
| ------------------- | ---------: | ---------: | ---------: | ---------: |
| Logistic Regression | **96.63%** | **97.14%** | **85.65%** | **91.02%** |
| Random Forest       | **98.66%** | **97.52%** | **95.74%** | **96.62%** |

Random Forest produced the strongest overall performance.

### Held-Out Test Set

On the 70/30 held-out evaluation, Random Forest:

* Correctly identified **659 of 690 seizure segments**
* Missed **31 seizure segments**
* Incorrectly classified **21 of 2,760 non-seizure segments**

These results demonstrate that a relatively small set of explicitly defined biomedical signal-processing features can provide strong discrimination between seizure and non-seizure EEG segments on this dataset.

---

## Project Structure

```text
EEG-Seizure-Classification/
│
├── EEG_Seizure_Classification.ipynb
└── README.md
```

The complete workflow is contained in the Jupyter/Google Colab notebook.

---

## Requirements

Install the required Python packages with:

```bash
pip install numpy scipy scikit-learn matplotlib pandas
```

### Main Libraries

* NumPy
* SciPy
* pandas
* scikit-learn
* Matplotlib

---

## Running the Project

### Google Colab

Open:

```text
EEG_Seizure_Classification.ipynb
```

Then run the notebook from top to bottom.

The dataset is loaded from a public URL, so no manual dataset download is required.

### Local Jupyter Environment

Install the dependencies:

```bash
pip install numpy scipy scikit-learn matplotlib pandas
```

Then launch Jupyter:

```bash
jupyter notebook
```

Open the notebook and execute the cells sequentially.

---

## Research Motivation

This project explores an important question in EEG machine learning:

> **Can explicitly defined and interpretable biomedical signal-processing features provide strong seizure classification without relying on undocumented feature representations?**

The results provide a strong classical-machine-learning baseline and establish a feature representation that can be directly inspected, reproduced, and analyzed.

This also creates a basis for comparing interpretable handcrafted features with the undocumented feature representation used in the parent bottleneck-network study.

---

## Future Work

Planned extensions include:

* Use the nine engineered features as inputs to the parent bottleneck architecture.
* Compare the nine biomedical features directly against BEED's X1–X16 representation.
* Perform feature-ablation experiments to determine which feature groups contribute most to classification.
* Extend the binary problem to the original five-class classification task.
* Evaluate robustness under simulated channel dropout.
* Investigate recording-level/grouped validation to assess generalization beyond segment-level splits.

---

## Reference

R. G. Andrzejak, K. Lehnertz, F. Mormann, C. Rieke, P. David, and C. E. Elger,

*"Indications of nonlinear deterministic and finite-dimensional structures in time series of brain electrical activity: Dependence on recording region and brain state,"*

**Physical Review E**, vol. 64, no. 6, 2001.

---

## Author

**Ayesha Kauser**

Biomedical Engineering Undergraduate
University of Engineering & Technology (UET), Lahore

---

## Disclaimer

This project is intended for **research and educational purposes**. The reported performance is specific to the dataset and evaluation strategy used in this study and should not be interpreted as clinical diagnostic performance.
