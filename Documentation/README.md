# Medical Insurance Fraud Detection System

An end-to-end fraud detection pipeline built on real medical insurance claims data from a Lebanese insurer. The system combines rule-based detection, machine learning, and network graph analysis to identify fraudulent claims and prioritize investigations.

> **Note:** This repository uses a synthetic dataset of 5,000 claims for demonstration purposes. The original analysis was conducted on 137,819 proprietary claims (2023–2026) and cannot be shared publicly due to confidentiality agreements. Real findings are documented in the methodology note below.

---

## What This Project Does

Medical insurers process thousands of claims monthly with limited capacity to manually review each one. This system automatically flags suspicious claims using three complementary approaches — catching what any single method would miss.

---

## Key Results (Original Dataset — 137,819 Claims)

| Metric | Value |
|---|---|
| Total claims analyzed | 137,819 |
| Fraud rate detected | 21.4% |
| Total fraud flagged | 29,562 claims |
| Investigation queue | 27,138 prioritized claims |
| Estimated recoverable savings | $2.3M |
| Model AUC | 0.998 |
| Fraud recall | 95% |
| Confirmed fraud ring providers | 30 |
| Members with 100% fraud rate | 147 |

---

## Three-Layer Detection Architecture

### Layer 1 — Rule-Based Detection
Six fraud typologies defined and operationalized:

- **Overbilling** — Provider invoiced significantly above pre-approved estimate (21,731 cases)
- **Duplicate Claims** — Identical claim submitted multiple times (6,251 cases)
- **Unbundling** — Single service split into multiple billable components (3,156 cases)
- **Phantom Billing** — Invoiced but not paid and not rejected (2,487 cases)
- **Audit Flagged** — Claims internally flagged by the insurer (2,623 cases)
- **Same-Day Clustering** — Member visiting multiple providers on the same date

### Layer 2 — Machine Learning (Random Forest)
- Trained on engineered features including overbill ratios, claim frequency, provider volume, and billing patterns
- Top fraud drivers: overbill amount, overbill percentage, same-day claim count
- SHAP explainability layer provides per-claim reasoning for investigators
- AUC: 0.998 | Recall: 95% | False positive rate: <0.3%

### Layer 3 — Network Graph Analysis
- Built a bipartite graph of member-provider relationships
- Identified clusters of high-fraud-rate members connected to the same providers
- Calculated betweenness centrality to find central fraud ring coordinators
- Confirmed fraud rings across 30 providers with 147 members showing 100% fraud rates

---

## Most Significant Findings (Original Data)

- Single claim overbill of **$303,743** at Hop. Bekaa flagged by the system
- **LABTEST** identified as highest betweenness centrality node — central coordinator of the fraud network
- **Lab. Wakim** — 13 ring members, highest member concentration
- **Medical Care Management** — 9 ring members, $21,742 in fully fraudulent invoices
- **Hop. Hayat** — 76.8% above hospital peers, risk score 81.7
- **Khoury Gen. Hosp** — 138.8% above physician peers, risk score 79.3

---

## Composite Fraud Score

Each flagged claim receives a weighted composite score (0–100) combining:

| Component | Weight |
|---|---|
| Provider peer deviation | 35% |
| Member fraud history | 25% |
| Diagnosis risk patterns | 15% |
| Claim-level rule flags | 15% |
| Network centrality | 10% |

Claims are ranked by this score to produce a prioritized investigation queue.

---

## Power BI Dashboard (6 Pages)

1. **Executive Summary** — KPIs, risk tier distribution, monthly fraud trend
2. **Provider Intelligence** — Highest risk providers, peer benchmarking, fraud ring providers
3. **Member & Claim Analysis** — Member-level fraud patterns
4. **Investigation Queue** — Prioritized claims with composite scores and estimated savings
5. **Portfolio Overview** — Active member trends, claims volume 2023–2026
6. **Dashboard Filters** — Interactive slicers by risk tier, year, business line, provider specialty, agency

---

## Repository Structure

```
├── README.md
├── Fraud_Detection_Pipeline.ipynb    # Full Python analysis
├── synthetic_claims_data.csv         # Synthetic demo dataset (5,000 claims)
├── visuals/
│   ├── confusion_matrix.png
│   ├── shap_importance.png
│   ├── feature_importance.png
│   ├── fraud_network.png
│   └── dashboard_screenshots/
│       ├── executive_summary.png
│       ├── provider_intelligence.png
│       ├── investigation_queue.png
│       └── portfolio_overview.png
└── methodology_note.md
```

---

## Tech Stack

- **Python** — pandas, numpy, scikit-learn, SHAP, NetworkX, matplotlib, seaborn
- **Power BI** — 6-page interactive dashboard
- **Machine Learning** — Random Forest Classifier
- **Graph Analysis** — NetworkX (betweenness centrality, bipartite graph)
- **Explainability** — SHAP values

---

## Limitations & Future Enhancements

- Procedure/CPT codes would enable more precise unbundling detection
- Member-to-contract mapping would strengthen member-level risk profiling
- Pharmacy data would enable pharmacy fraud network analysis
- Premium data would enable actuarial pricing impact quantification

---

## Author

**Marie-Christine Btaich**
Actuarial Science Graduate — Notre Dame University Louaize
Currently pursuing SOA Exam P | Actuarial & Insurance Analytics

[LinkedIn](https://www.linkedin.com/in/marie-christine-btaich) | [GitHub](https://github.com/msbtaich)
