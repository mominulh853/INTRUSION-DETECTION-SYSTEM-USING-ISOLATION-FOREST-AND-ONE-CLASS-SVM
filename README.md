# INTRUSION-DETECTION-SYSTEM-USING-ISOLATION-FOREST-AND-ONE-CLASS-SVM

A machine learning project that detects cyberattacks and network anomalies using **Machine Learning**. This system is trained exclusively on "Benign" (normal) traffic, allowing it to identify zero-day attacks and unseen anomalies by recognizing deviations from the norm.

---

## Project Overview

Traditional intrusion detection systems often rely on signatures or labeled attack data. However, new attacks appear every day. This project solves that problem by learning the "manifold" of normal network behavior.

I compared two powerful unsupervised anomaly detection algorithms:
1.  **Isolation Forest** (Tree-based ensemble)
2.  **One-Class SVM** (Kernel-based boundary detection)

### The Results
The **Isolation Forest** model proved superior for this high-dimensional dataset:
* **ROC-AUC Score:** 0.982
* **Detection Rate (Recall):** 99.1%
* **False Alarm Rate:** Significantly lower than SVM

---

## Dataset

* **Source:** [CSE-CIC-IDS2018 Dataset](https://www.unb.ca/cic/datasets/ids-2018.html)
* **Description:** Real-world network traffic logs containing Benign traffic and various attacks (FTP-BruteForce, SSH-Bruteforce, DOS, Heartbleed, etc.).
* **Preprocessing:** * Removed localized IP addresses and timestamps.
    * Handled Infinity/NaN values.
    * Hendled zero-variances column 
    * Applied `RobustScaler` to handle the bursty nature of network traffic.

---

## ⚙️ Methodology

### 1. Training Strategy 
* **Training Set:** Contains **only** Benign traffic (Normal behavior).
* **Validation & Test Sets:** Contains a mix of Benign and Attack traffic to evaluate performance.

### 2. Hyperparameter Optimization
I used **Optuna** to automatically search for the best model parameters:
* *Isolation Forest:* Optimized `n_estimators` (Best: 55) and `max_samples`.
* *One-Class SVM:* Optimized `nu` (outlier fraction) and `gamma`.

### 3. Threshold Tuning
Instead of using the default decision boundary (0), I dynamically calculated the **Optimal Threshold** using the Precision-Recall Curve on the Validation set to maximize the F1-Score.

---

## 📈 Performance Comparison

| Metric | Isolation Forest  | One-Class SVM  |
| :--- | :--- | :--- |
| **ROC-AUC** | **0.9825** | 0.8764 |
| **F1-Score** | **0.9754** | 0.9306 |
| **Balanced Accuracy** | **0.9760** | 0.8874 |
| **Missed Attacks** | **~1,800** | ~9,300 |

> **Conclusion:** Isolation Forest is significantly better at isolating modern network attacks, offering a 99% detection rate while maintaining computational efficiency.
