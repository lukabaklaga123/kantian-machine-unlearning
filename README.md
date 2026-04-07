# 🛡️ Kantian Machine Unlearning

[![Hugging Face Space](https://img.shields.io/badge/🤗%20Hugging%20Face-Live%20Demo-blue)](https://huggingface.co/spaces/GodelModel/Master_Thesis/tree/main)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1zL1DuGnGYqp1kzfuhOXlvn9dXcMvW2Ss?usp=sharing)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**A cryptographically secure, GDPR-compliant machine unlearning framework for High-Risk Clinical AI Systems.** Developed as part of a Dual-Degree Master's Thesis in Cybersecurity at Northern Kentucky University (NKU), and St. Andrew the First-Called Georgian University (SANGU).

---

## 📖 Abstract
Under the EU AI Act and GDPR Article 17 (Right to Erasure), clinical AI models must be capable of permanently forgetting sensitive Patient Health Information (PHI) without requiring a complete model retraining from scratch. Standard unlearning algorithms often leave mathematically detectable "scars" in the latent space, leaving the model highly vulnerable to Membership Inference Attacks (MIA). This repository introduces a **Kantian Penalty Constraint ($\lambda$)**—an information-entropy-aware unlearning algorithm that forces a Minimax Nash equilibrium between diagnostic utility and cryptographic privacy.

## 💾 Dataset & Architecture
* **Dataset:** The [MTSamples Medical Transcription Dataset](https://www.kaggle.com/datasets/tboyle10/medicaltranscriptions) (via Kaggle).
* **Architecture:** `dmis-lab/biobert-base-cased-v1.1`. *(Note: BioBERT is distributed under the Apache-2.0 License).*
* **Model Weights:** Due to GitHub file size limits, the final `.pt` defender and attacker weights are hosted directly on the Hugging Face Hub via the Live Demo link above.

## 📊 Empirical Results
The following metrics were generated using a rigorous adversarial audit comparing our Kantian Unlearning algorithm against standard Gradient Ascent and a true "Retrain-from-Scratch" Gold Standard.

| Methodology | Diagnostic Utility (Test F1) | Privacy Leakage (MIA Risk) | Regulatory Status |
| :--- | :---: | :---: | :--- |
| **Baseline (Gradient Ascent)** | 0.532 | 51.8% | 🛑 High Leakage Risk |
| **Kantian Penalty ($\lambda=10$)** | 0.611 | 53.0% | ⚠️ Moderate Risk |
| **Kantian Penalty ($\lambda=50$)** | 0.482 | **49.8%** | ✅ **GDPR Compliant (Nash Equilibrium)** |
| **Kantian Penalty ($\lambda=100$)**| 0.308 | 44.6% | ❌ Utility Collapse (Over-scrubbing) |
| *Gold Standard (Retrain from scratch)* | *0.548* | *49.2%* | *Reference Baseline* |

> **Statistical Validation:** A two-sample Kolmogorov-Smirnov (K-S) test confirms that the latent privacy distribution of the $\lambda=50$ Kantian model is mathematically equivalent to the Gold Standard retrained model, effectively achieving true data deletion.

## 🚀 Live Interactive Cyber-Range
A fully interactive Gradio dashboard has been deployed to simulate adversarial audits in real-time. It visualizes the shift in the model's self-attention—proving the neural network successfully diverts its focus away from intercepted PII and toward clinical symptoms.

👉 **[Launch the Live Adversarial Simulator on Hugging Face](https://huggingface.co/spaces/GodelModel/Master_Thesis)**

## 💻 How to Run the Experiments
The empirical data and statistical proofs were generated across two primary Google Colab environments. You can reproduce the entire pipeline by running them sequentially:

1. **`01_Kantian_Experiment_and_Training.ipynb`**
   * *Purpose:* Executes the core PyTorch training pipeline, applies the Kantian constraint ($\lambda$), and generates the Pareto frontier showing the trade-off between privacy and utility.
   * *Hardware:* Google Colab (T4 GPU recommended).

2. **`02_Gold_Standard_Validation.ipynb`**
   * *Purpose:* Addresses the ultimate benchmark in machine unlearning. It evaluates "Forget-Set Task Accuracy" (Forget F1) and trains a fresh BioBERT purely on the Retain Set (the Gold Standard). It concludes with the Kolmogorov-Smirnov (K-S) statistical test proving equivalence.
   * *Hardware:* Google Colab (T4 GPU recommended).

