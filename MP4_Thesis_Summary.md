# Master Project 4 (MP4) — Comprehensive Research Knowledge Base
*Official Master Thesis Document — Ingested: September 3, 2026*

- **Document Title:** *Proactive Network Defence: A Lightweight Machine Learning System Using the NSL-KDD Dataset*
- **Author:** AMIN Md Ruhul (アミン エムディー ルフル / Student ID: M25W0668)
- **Institution:** The Kyoto College of Graduate Studies for Informatics (KCGI / 京都情報大学院大学)
- **Concentration:** Network Administration (Applied Information Technology / Web Business Technology)
- **Academic Weight:** 4-Credit Capstone Project / Thesis Equivalent
- **Supervisor:** Prof. Sameera Abar

---

## 1. Problem Statement & Research Philosophy
- **The Core Conflict:** Deep learning IDS models (CNN, LSTM) achieve high laboratory accuracy but suffer from the **"Black-Box" problem** (opaque decision-making) and heavy computational demands, making them impractical for edge hardware and mathematical auditability.
- **The Core Solution:** Engineering a lightweight, zero-leakage data-preprocessing middleware with classical, interpretable ML algorithms (Random Forest, XGBoost, Decision Tree, LDA, Logistic Regression) optimized under a **False-Positive-Rate (FPR) First** operational priority.

---

## 2. Dataset & Feature Engineering Pipeline
- **Benchmark:** Official **NSL-KDD** dataset.
  - **Training Set (`KDDTrain+`):** 125,973 records (67,343 Normal, 45,927 DoS, 11,656 Probe, 995 R2L, 52 U2R).
  - **Test Set (`KDDTest+`):** 22,544 records containing **17 zero-day attack variants** under severe distribution shift.
- **Preprocessing Pipeline (0.51s execution, zero data leakage):**
  - *Data Cleansing:* NaN, infinite value removal, duplicate elimination.
  - *Categorical Encoding:* One-Hot Encoding on `protocol_type` (3), `service` (70), and `flag` (11) ➔ expanding 41 raw features to 122 numerical features.
  - *Normalization:* Min-Max Scaling strictly fitted on `KDDTrain+` and transformed onto `KDDTest+`.
  - *Feature Selection:* Random Forest Gini impurity reduction selecting the **top 35 high-variance features** (>70% dimensionality reduction). Top discriminators: `flag_SF`, `dst_host_srv_count`, `dst_host_same_srv_rate`, `logged_in`.

---

## 3. Official Empirical Benchmark Results (on `KDDTest+` — n = 22,544)

### A. Binary Classification (FPR-First Ranking):
| Model Architecture | Threshold | False Positive Rate (FPR) | Precision | Recall | F1-Score | Accuracy | ROC-AUC |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Random Forest (Recommended)** | **0.880** | **1.84%** *(Lowest)* | **0.9738** | 0.5182 | **0.6764** | **0.7178** | **0.9412** |
| **XGBoost Classifier** | 0.885 | 2.53% | 0.9673 | 0.5671 | 0.7150 | 0.7426 | 0.9560 |
| **Linear Discriminant (LDA)** | 0.950 | 2.54% | 0.9648 | 0.5279 | 0.6824 | 0.7203 | 0.9232 |
| **Logistic Regression** | 0.935 | 5.98% | 0.9159 | 0.4932 | 0.6411 | 0.6857 | 0.8789 |
| **Decision Tree (CART)** | 0.780 | 7.77% | 0.9169 | 0.6493 | 0.7602 | 0.7669 | 0.7862 |

### B. Multiclass & Rare-Class Detection Dynamics:
- **DoS (Denial of Service):** ~78.5% Recall (Reliably detected).
- **Probe (Reconnaissance):** ~71.2% Recall (Adequately detected).
- **R2L (Remote to Local):** **~4.1% Recall** (XGBoost achieves **97.6% Precision** on detected alerts).
- **U2R (User to Root):** **0 of 67 caught in XGBoost / 1 of 67 caught in Decision Tree (1.5% Recall)**.
- *Logistic Regression Asymmetry:* Detects 76.1% U2R but with extreme false alarm rate (Precision: 0.0269 — 36 false alerts per true alert).

---

## 4. Lightweight Deployment Metrics (Hardware Viability)
- **Inference Latency & Throughput:**
  - Decision Tree: **3,462,031 records/second** (0.007s total test inference).
  - Logistic Regression / LDA: **~2,300,000 records/second** (0.010s).
  - Random Forest: **141,824 records/second** (0.159s).
  - XGBoost: **93,005 records/second** (0.242s).
- **On-Disk Model Footprint:**
  - Decision Tree / LDA / LR: `< 0.15 MB`.
  - XGBoost: `1.6 MB` (Binary) / `5.9 MB` (Multiclass).
  - Random Forest: `10.7 MB` (Binary) / `17.8 MB` (Multiclass).

---

## 5. Master Thesis Future Work (The 1-Year Kenkyusei & Doctoral Agenda)
1. **Rare-Class Synthetic Augmentation:** Adaptive boundary oversampling (Borderline-SMOTE, Conditional Tabular GANs) for skewed network flow data.
2. **Modern Benchmark Generalization:** Evaluating models on CSE-CIC-IDS2018, UNSW-NB15, and real-time university testbed traffic.
3. **Hardware & Kernel-Level Streaming:** Deploying inference via Linux kernel eBPF / XDP packet hooks.
4. **Explainable AI (XAI):** Integrating TreeSHAP feature attribution for real-time human analyst alert interpretation.
5. **Real-Time Streaming & Agentic Cyber Defense:** Building autonomous agentic loops that dynamically re-tune detection thresholds based on live network telemetry.
