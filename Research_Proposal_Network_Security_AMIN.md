# RESEARCH PROPOSAL (研究計画書)

**Applicant:** AMIN Md Ruhul (アミン エムディー ルフル)  
**Proposed Period:** 1-Year Non-Degree Research Student (*Kenkyūsei* / 研究生 — April 2027 – March 2028)  
**Long-Term Objective:** Continuation into Doctoral (PhD) Program (April 2028 – March 2031)  
**Affiliation:** Master of Information Technology, The Kyoto College of Graduate Studies for Informatics (KCGI), Kyoto, Japan  
**Concentration:** Network Administration (CGPA: 3.47 / 4.00)  
**Contact:** mdruhul68@gmail.com | +81 80-6950-7426 | Sakyo-ku, Kyoto, Japan  

---

## 1. Research Title
**Proactive Cyber Defence under Distribution Shift: Robust Detection of Rare Attack Classes in Low False-Positive Network Environments**  
*(分布シフト下における能動的サイバー防衛：低誤検知環境における稀少攻撃クラスの高精度検知に関する研究)*

---

## 2. Research Background & Motivation

In modern enterprise and campus network infrastructures, Network Intrusion Detection Systems (NIDS) are the frontline defense against sophisticated cyber threats. However, operational security teams face two critical bottlenecks:

1. **The Alert Fatigue Crisis (The False Positive Problem):** A high-throughput 10 Gbps ingress gateway processes hundreds of millions of packets daily. Even a 1% false-positive rate produces thousands of erroneous alerts per day, overwhelming security analysts and causing real intrusion signals to be lost.
2. **Distribution Shift & Zero-Day Variants:** Real-world network traffic is inherently non-stationary. Training datasets fail to mirror production traffic because attackers continuously evolve their evasion strategies, protocol flags, and payload distributions.

During my Master Capstone Project (MP4, supervised by Prof. Sameera Abar at KCGI), I evaluated classical machine learning classifiers (Random Forest, XGBoost, Decision Trees, LDA, Logistic Regression) on tabular network features using the official **distribution-shifted `KDDTest+` split** (22,544 records). 

While my empirical results established that a leakage-safe Random Forest pipeline achieves an operational **1.84% False Positive Rate (FPR)** with rapid sub-second feature preprocessing (41 → 122 encoded → 35 selected features), it exposed a critical, unresolved vulnerability: **extreme detection failure on rare attack classes**.

---

## 3. Core Problem Statement: The Rare-Class Dilemma

In the distribution-shifted test set, the empirical detection rates showed extreme disparity:
* **Volumetric Attacks (DoS):** ~78.5% Recall (Reliably detected).
* **Surveillance Attacks (Probe):** ~71.2% Recall (Adequately detected).
* **Remote to Local (R2L):** **~4.1% Recall** (Severe Failure).
* **User to Root (U2R):** **0 of 67 instances caught (0% Recall — Complete Blindspot)**.

### Why this failure occurs:
Rare attack vectors (U2R/R2L) represent `< 1%` of network traffic flows. Because their packet headers closely mimic legitimate administrative privilege escalation and local root commands, standard objective functions penalize false alarms on normal traffic at the total expense of ignoring rare attacks.

**The Core Research Question for Kenkyusei & Doctoral Study:**  
*How can we mathematically augment rare-class boundary representations and incorporate cost-sensitive loss functions so that machine learning IDS can detect stealthy, low-volume privilege-escalation attacks (U2R/R2L) without degrading the baseline low false-positive rate (< 2% FPR)?*

---

## 4. Research Objectives

1. **Rare-Class Feature Synthesis & Geometric Augmentation:** Develop adaptive boundary oversampling (e.g., Borderline-SMOTE, Conditional Tabular GANs) tailored specifically for non-continuous, highly skewed network flow attributes.
2. **Asymmetric Cost-Sensitive Optimization:** Construct custom loss functions that assign severe penalties to missed privilege-escalation attacks while anchoring the global FPR below an operational threshold.
3. **Cross-Benchmark Generalization:** Validate the proposed architecture beyond legacy datasets on modern zero-trust telemetry benchmarks (e.g., CIC-IDS2017, CSE-CIC-IDS2018, UNSW-NB15, and real campus testbed traffic).
4. **Deployable Edge / Gateway Telemetry:** Implement the lightweight inference engine within Linux kernel packet-filtering hooks (eBPF / XDP) or standard Nginx/snort ingress pipelines to verify real-time processing under high-load streaming constraints.

---

## 5. Methodology & Technical Approach

```
┌────────────────────────┐     ┌────────────────────────┐     ┌────────────────────────┐
│ Raw Network Telemetry  │ ──► │ Leakage-Safe Feature   │ ──► │ Adaptive Rare-Class    │
│ (PCAP / Ingress Flows) │     │ Selection & Normalizer │     │ Boundary Augmentation  │
└────────────────────────┘     └────────────────────────┘     └───────────┬────────────┘
                                                                          │
┌────────────────────────┐     ┌────────────────────────┐                 │
│ Low-FPR Alert Gate     │ ◄── │ Asymmetric Cost-Tree   │ ◄───────────────┘
│ (< 1.84% Global FPR)   │     │ & Ensemble Classifier  │
└───────────┬────────────┘     └────────────────────────┘
            │
            ▼
┌────────────────────────┐
│ Threat Explainability  │
│ (SHAP / Feature Impact)│
└────────────────────────┘
```

1. **Data Ingestion & Leakage Prevention:** Strict temporal separation of training flows to prevent target leakage; real-time normalization and categorical mapping.
2. **Cost-Sensitive Multi-Tier Hierarchy:** A two-stage detection pipeline:
   - *Stage 1 (High-Throughput Filter):* Rapid binary classifier separating normal traffic from volumetric anomalies.
   - *Stage 2 (Specialized Rare-Class Inspector):* Dedicated cost-sensitive ensemble specifically trained on rare-class boundaries to classify U2R/R2L attempts.
3. **Explainable Threat Attribution:** Incorporate lightweight feature attribution (TreeSHAP) so security engineers receive human-interpretable rationale for each generated alert.

---

## 6. One-Year Research Student (*Kenkyūsei*) Milestone Plan

| Phase / Period | Milestones & Deliverables |
| :--- | :--- |
| **Q1 (April – June 2027)** | • Complete laboratory onboarding and literature review on state-of-the-art rare-class IDS.<br>• Replicate baseline models on modern benchmark datasets (CIC-IDS2018).<br>• Formalize the mathematical formulation of the asymmetric cost-sensitive loss function. |
| **Q2 (July – Sept 2027)** | • Implement and evaluate synthetic boundary oversampling techniques (CTGAN/Borderline-SMOTE).<br>• Benchmark rare-class recall improvements across multi-class network traffic.<br>• Submit preliminary findings to a domestic Japanese IEICE / IPSJ SIG conference. |
| **Q3 (Oct – Dec 2027)** | • Integrate TreeSHAP explainability module into the detection pipeline.<br>• Prepare and submit application materials for Doctoral (PhD) Course admission (Entrance Exam / Interview).<br>• Draft academic paper for international IEEE/ACM conference submission. |
| **Q4 (Jan – Mar 2028)** | • Prototype eBPF/Linux streaming inference pipeline on a laboratory testbed.<br>• Finalize 1-Year Research Student Report (*Kenkyūsei Seika Hōkoku*).<br>• Transition smoothly into the Doctoral (PhD) Program starting April 2028. |

---

## 7. Expected Outcomes & Long-Term Doctoral Vision

* **Immediate Research Outcomes:** A deployable, open-source lightweight machine learning intrusion detection framework demonstrating measurable recovery of rare privilege-escalation attacks (targeting > 50% U2R/R2L recall) while maintaining global FPR < 2.0%.
* **Doctoral Continuity (2028–2031):** Expansion into autonomous agentic cyber defense, zero-trust enterprise telecommunication networks, and cross-border digital infrastructure resilience linking Japan and international academic networks.

---

## 8. Selected References

1. Tavallaee, M., Bagheri, E., Lu, W., & Ghorbani, A. A. (2009). *A detailed analysis of the KDD CUP 99 data set*. IEEE Symposium on Computational Intelligence for Security and Defense Applications.
2. Sharafaldin, I., Lashkari, A. H., & Ghorbani, A. A. (2018). *Toward generating a new intrusion detection dataset and intrusion traffic characterization*. ICISSP.
3. Lundberg, S. M., & Lee, S. I. (2017). *A unified approach to interpreting model predictions*. Advances in Neural Information Processing Systems (NeurIPS).
4. Amin, M. R., & Abar, S. (2025). *Proactive Network Defence: A Lightweight Machine Learning System Using the NSL-KDD Dataset*. KCGI Master Project 4 Draft.
