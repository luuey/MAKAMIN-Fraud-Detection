# MAKĀMIN | مكامن
## Graph-Based Fraud Detection & Investigation

**MAKĀMIN** is a Data Science capstone project that combines machine learning, historical relational analysis, network analysis, and explainable AI to investigate fraud in the **IEEE-CIS Fraud Detection dataset**.

> **Detect the risk. Explain the prediction. Investigate the relationships.**

---

## Project Overview

Fraud is not always visible from a single suspicious transaction. Multiple transactions may share cards, addresses, email domains, or devices, creating relational patterns that are difficult to identify when transactions are analyzed independently.

This project investigates whether historical relational information can improve fraud detection beyond traditional transaction-level machine learning.

The solution combines:

- XGBoost
- Artificial Neural Networks (ANN)
- Historical relational feature engineering
- Network analysis
- SHAP explainability
- An interactive Streamlit investigation dashboard

---

## Dataset

The project uses the **IEEE-CIS Fraud Detection dataset**.

After merging the transaction and identity tables:

| Statistic | Value |
|---|---:|
| Transactions | 590,540 |
| Features after merge | 434 |
| Fraud transactions | 20,663 |
| Fraud rate | 3.5% |

Because fraud represents only a small proportion of the dataset, **Average Precision (AP)** is used as the primary model-comparison metric, together with ROC-AUC.

---

## Methodology

The project follows a chronological machine-learning workflow:

**Data Preparation → EDA → Chronological Split → Baseline Modeling → Network Analysis → Historical Feature Engineering → Model Comparison → SHAP → MAKĀMIN Dashboard**

The earliest **80% of transactions (472,432)** are used for training and the latest **20% (118,108)** for validation.

This preserves temporal order and better reflects a real fraud-detection setting where a model learns from past transactions and scores future ones.

---

## Historical Relational Features

Five target-free historical features were engineered.

For each transaction, they measure how many earlier transactions in the previous **24 hours** shared the same:

| Feature | Relationship |
|---|---|
| `card1_prev_count_24h` | Card |
| `addr1_prev_count_24h` | Address |
| `email_prev_count_24h` | Email domain |
| `device_prev_count_24h` | Device |
| `card_addr_prev_count_24h` | Card + Address |

Only information available before each transaction is used, and the fraud target is never used to construct these features.

---

## Model Comparison

Four main predictive configurations were evaluated using the same chronological validation strategy.

| Model | Features | Average Precision | ROC-AUC |
|---|---:|---:|---:|
| **XGBoost + Historical Features** | 428 | **0.521** | **0.909** |
| XGBoost Baseline | 423 | 0.519 | 0.908 |
| ANN Baseline | 423 | 0.451 | 0.861 |
| ANN + Historical Features | 428 | 0.440 | 0.873 |
| Random Guess | — | 0.034 | 0.500 |

The final model is **XGBoost with historical relational features**.

The relational features provide a modest improvement over baseline XGBoost, while XGBoost clearly outperforms the ANN configurations on this dataset.

---

## Final Model

### XGBoost + Historical Features

**Average Precision:** `0.5207`  
**ROC-AUC:** `0.9093`

At the reference threshold of **0.50**:

| Metric | Result |
|---|---:|
| Fraud detected | 3,014 |
| Fraud missed | 1,050 |
| False alerts | 11,108 |
| Recall | 74.16% |
| Precision | 21.34% |

The 0.50 threshold is used as a **reference threshold for evaluation and demonstration**, not as an optimized production operating point.

---

## Explainability with SHAP

SHAP is used to understand both global model behavior and individual fraud predictions.

Local SHAP explanations were generated for **5,000 validation transactions**.

For an individual transaction, MAKĀMIN highlights:

🔴 **Fraud Risk Increase** — features pushing the model toward a higher fraud-risk prediction.

🟢 **Fraud Risk Decrease** — features pushing the model toward a lower fraud-risk prediction.

Among the five engineered historical features, `card1_prev_count_24h` had the strongest global SHAP importance, ranking **17th out of 428 features**.

---

## MAKĀMIN Dashboard

**MAKĀMIN | مكامن** is an interactive fraud analytics and investigation dashboard built with **Streamlit, Plotly, and PyVis**.

It combines three stages of fraud investigation:

### 01 — Detect
Evaluate the transaction using the final XGBoost fraud-risk score.

### 02 — Explain
Understand the strongest positive and negative SHAP contributions behind the prediction.

### 03 — Investigate
Explore previous transactions from the prior 24 hours that share cards, addresses, email domains, or devices.

The dashboard summarizes the complete **118,108-transaction validation period** and provides detailed SHAP-based investigation for **5,000 transactions**.

The historical investigation sample contains:

- **5,000 / 5,000 transactions with historical relationships**
- **3,182,746 historical connection records**

For readability, interactive network visualizations display up to **150 historical connections** per selected transaction while statistics use the complete available neighborhood.

> A relational connection provides investigation context and should not independently be interpreted as proof of coordinated fraud.

---

## Key Findings

- XGBoost achieved the strongest predictive performance.
- Historical relational features improved XGBoost only modestly.
- Relationship information was useful as supporting predictive and investigative context.
- SHAP made individual fraud-risk predictions interpretable.
- Shared transaction attributes can reveal useful historical patterns, but a shared identifier does not prove coordinated fraud.
- Model complexity alone does not guarantee better performance.

---

## Project Structure

```text
MAKAMIN-Fraud-Detection/
│
├── README.md
├── MAKAMIN_Graph_Based_Fraud_Detection.ipynb
│
├── report/
│   └── MAKAMIN_Capstone_Report.pdf
│
├── presentation/
│   └── MAKAMIN_Final_Presentation.pdf
│
└── demo/
    └── MAKAMIN_Demo.mp4
```

---

## Technologies

**Python · Pandas · NumPy · XGBoost · TensorFlow/Keras · NetworkX · SHAP · Scikit-learn · Streamlit · Plotly · PyVis**

---

## Limitations

The engineered relational features provide only a modest improvement over baseline XGBoost.

The IEEE-CIS dataset also contains anonymized features and does not provide confirmed fraud-ring identities. Shared cards, addresses, email domains, or devices may occur for legitimate reasons.

The current system should therefore be viewed as an **evaluation and investigation prototype**, rather than a production fraud-detection system.

---

## Future Work

Future extensions could include richer temporal and relational features, more advanced graph-based learning approaches such as Graph Neural Networks (GNNs), time-based cross-validation, a separate final test period, cost-based threshold optimization, stronger entity identifiers, and real-time deployment of MAKĀMIN.

---

## Team — Group 8

**Nada Aljaafari · Lujain Alqarni · Remas Almutairi · Alia AlGhamdi**

Data Science Capstone Project · 2026

---

# MAKĀMIN | مكامن

### Detect the risk. Explain the prediction. Investigate the relationships.

**Uncover Hidden Fraud Patterns.**
