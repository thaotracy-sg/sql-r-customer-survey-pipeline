# 🛒 Shopping Mall Customer Survey — Data Wrangling & SQL Pipeline
### Python · MySQL · R · ggplot2 | ANL503 Data Wrangling | SUSS 2025

> **Academic project** completed as part of the ANL503 Data Wrangling module
> at Singapore University of Social Sciences (SUSS), April 2025.
> Dataset: shopping mall customer survey (course-provided data).

---

## 📌 Overview

This project builds a **complete end-to-end data pipeline** - from raw CSV file
to a monthly analytics dashboard - combining Python, MySQL, and R across
four stages of data engineering and analysis.

The dataset contains customer survey responses from a shopping mall across
12 months, covering satisfaction scores, complaint rates, and complaint handling quality.

---

## 🔄 Pipeline Architecture

```
ECA_data_raw.csv
      │
      ▼
[Python + SQLAlchemy]
      │  Load CSV → MySQL table (ECA_data_raw)
      ▼
[SQL]  Standardise date format → ECA_data
      │
      ▼
[SQL]  Aggregate by month → ECA_summary (12 rows × 7 columns)
      │
      ▼
[R + DBI]  Read from MySQL → Visualise with ggplot2
```

---

## 🔬 What Each Step Does

| Step | Tool | Task |
|---|---|---|
| **1a** | Python · pandas · SQLAlchemy | Load `ECA_data_raw.csv` into MySQL with explicit column types |
| **1b** | MySQL SQL | Standardise `doi` column — handle both integer offsets and date strings → `YYYY-MM-DD` |
| **1c** | MySQL SQL | Assess and justify data types (`TINYINT UNSIGNED`, `DATE`) for storage efficiency |
| **1d** | MySQL SQL | Aggregate into 12-row monthly summary with avg scores + TNCR calculation |
| **1e** | R · DBI · ggplot2 | Connect to MySQL, read summary table, produce dual-axis trend visualisation |

---

## 📊 Dataset Variables

| Column | Description | Type |
|---|---|---|
| `doi` | Date of interview | DATE |
| `satis` | Customer satisfaction score (1–10) | TINYINT |
| `confirm` | Confirmation of mall quality (1–10) | TINYINT |
| `ideal` | How close to ideal mall (1–10) | TINYINT |
| `comp` | Whether customer complained (0/1) | TINYINT |
| `handle` | Complaint handling satisfaction (1–10) | TINYINT |
| `nocomp` | Reason for not complaining (1–4) | TINYINT |

---

## 📈 Key Findings

| Observation | Detail |
|---|---|
| 😊 **Positive overall sentiment** | Satisfaction, confirmation, and ideal scores consistently above 6.6–7.7 |
| 📅 **Peak months** | March and December — likely seasonal events or campaigns |
| ✅ **High TNCR** | True Non-Complaint Rate 81–85% — most non-complainers had no reason to complain |
| ⚠️ **TNCR dips in April & June** | Possible operational issues during those months |
| 🔧 **Weakest metric** | Complaint handling (`handle`) scores 5.5–6.3 — significantly below satisfaction scores |

---

## 💡 SQL Highlights

**Date standardisation using CASE + REGEXP:**
```sql
CASE
    WHEN doi REGEXP '^[0-9]+$'
        THEN DATE_ADD('2024-01-01', INTERVAL CAST(doi AS UNSIGNED) DAY)
    ELSE STR_TO_DATE(doi, '%Y-%m-%d')
END AS doi
```

**TNCR calculation using conditional aggregation:**
```sql
CONCAT(
    ROUND(
        SUM(CASE WHEN nocomp = 1 THEN 1 ELSE 0 END) / COUNT(*) * 100,
    2), '%') AS TNCR
```

---

## 🛠️ Tools & Stack

| Tool | Purpose |
|---|---|
| `Python` · `pandas` · `SQLAlchemy` | Load CSV into MySQL with type control |
| `MySQL` | Data storage, cleaning, and aggregation |
| `R` · `DBI` · `RMySQL` | Connect R to MySQL and read tables |
| `ggplot2` · `tidyr` | Dual-axis monthly trend visualisation |

---

## 🔗 More Projects

 📊 [Power BI IMS Dashboard Enhancement – Healthcare](https://github.com/thaotracy-sg/powerbi-ims-dashboard-enhancement-healthcare) <br>
 📱 [Causal Analysis Using Panel Logit Model – Game App](https://github.com/thaotracy-sg/Causal-analysis-panel-logit-R-gameapp) <br>
 🌿 [Sustainable Brand Logit Analysis](https://github.com/thaotracy-sg/Sustainable-brand-logit-analysis) <br>
 🧴 [Commercial Analytics – Skincare Channel Performance](https://github.com/thaotracy-sg/Commercial-analytics-skincare-channel-performance) <br>
 🔋 [Time Series Forecasting – Electric Power & CO2 Emissions](https://github.com/thaotracy-sg/Time-series-forecasting-electric-power-co2-emission-europe) <br>
---

*Analysis by Tracy Nguyen | ANL503 Data Wrangling | SUSS 2025*
