# AI-Based Network Anomaly Detection Using Zeek Logs

An end-to-end, offline security auditing pipeline that captures network traffic with **Zeek**, applies three unsupervised machine learning models to detect anomalous connections, and visualizes the results through interactive **Kibana** dashboards.

## Overview

Traditional signature-based intrusion detection systems can't catch zero-day or previously unseen attacks. This project tackles that gap using unsupervised anomaly detection — no labeled attack data required — applied directly to Zeek's `conn.log` connection records.

**Pipeline:** Zeek (traffic capture) → Python preprocessing & feature engineering → three ML models trained in parallel → Elasticsearch indexing → Kibana dashboards.

## Models Compared

| Model | Anomalies Detected | Rate |
|---|---|---|
| Local Outlier Factor (LOF) | 26,795 | 10.15% |
| Isolation Forest | 26,305 | 9.96% |
| One-Class SVM | 24,952 | 9.45% |

- **263,992** Zeek connection log records analyzed
- **78,052** total anomalies flagged across all three models
- **2,437** records flagged as anomalous by all three models simultaneously (high-confidence)
- Results visualized across **2 Kibana dashboards, 18 panels**

## Tech Stack

- **Traffic capture:** Zeek 6.0
- **ML / preprocessing:** Python, scikit-learn (Isolation Forest, LOF, One-Class SVM), pandas, NumPy
- **Indexing & visualization:** Elasticsearch 8.11.0, Kibana (Lens editor)
- **Environment:** Google Colab

## Repository Structure

```
├── notebooks/   → full ML pipeline notebook (.ipynb)
├── dashboards/  → Kibana dashboard screenshots
├── report/      → full technical write-up (PDF)
└── README.md
```

## Methodology Summary

1. **Data collection** — Zeek passively captured TCP/UDP/ICMP traffic on a test network, producing structured `conn.log` records.
2. **Preprocessing** — missing-value handling, categorical encoding (protocol, service, connection state), feature selection, and min-max normalization.
3. **Feature engineering** — temporal, byte-ratio, packet-ratio, and duration-derived features added; feature importance ranked via a Random Forest proxy model.
4. **Anomaly detection** — Isolation Forest, Local Outlier Factor, and One-Class SVM trained in parallel on the same feature set, each with a 10% contamination assumption.
5. **Visualization** — results indexed into Elasticsearch and explored through Kibana dashboards covering protocol distribution, traffic-over-time, host communication patterns, and per-model anomaly comparison.

Full methodology, formulas, and results are in [`report/AI_Security_Auditing_Zeek_Report.pdf`](./report/AI_Security_Auditing_Zeek_Report.pdf).

## Key Takeaway

No single unsupervised model dominates across all traffic types — LOF, Isolation Forest, and OCSVM each catch different anomaly patterns, which is why a multi-model comparative approach gives a more reliable security assessment than relying on one algorithm alone.

## Author

Sivani S
