# Operational Reporting – Gold Data Pipeline

End-to-end operational reporting pipeline built with **n8n**, designed to ingest data from multiple operational domains, normalize it into a unified **Gold metrics layer**, and publish daily KPIs ready for reporting and analysis.

This project simulates a real-world data engineering scenario focused on **operational reporting**, not toy automation.

---

## 🧩 Overview

The pipeline collects data from heterogeneous sources (APIs and files), applies domain-specific transformations and daily aggregations, and consolidates everything into a single **Gold data model**.

The final output is stored in **Google Sheets**, making it immediately usable for dashboards, analysis, or stakeholder reporting.

---

## ❓ What Problems This Solves

In many companies, operational data is:

- Spread across multiple systems (Sales, Support, HR, Ops)
- Stored with different schemas and granularities
- Difficult to combine into a single reporting view
- Not consistently aggregated or documented

This project addresses those problems by:

- Providing a **unified metric schema** across domains
- Separating raw data from reporting-ready metrics
- Applying consistent **daily aggregations**
- Enabling fast, low-friction reporting without a full data warehouse
- Demonstrating a clear **Bronze → Silver → Gold** design pattern

The result is a lightweight but realistic reporting architecture similar to what is used in production analytics environments.

---

## 🏗️ Architecture

```
Mock APIs / Source Systems
        │
        ▼
 Domain Pipelines (Daily Aggregation)
  ├─ Sales
  ├─ Support
  ├─ HR
  └─ Ops
        │
        ▼
 Unified Gold Layer
        │
        ▼
 Google Sheets (Reporting & Analysis)
```

---

## 📊 Gold Data Model

All domains converge into a single, consistent structure:

| Field | Type | Description |
|-----|------|-------------|
| `date` | string | Metric date (YYYY-MM-DD) |
| `source_system` | string | Origin domain (`sales`, `support`, `hr`, `ops`) |
| `metric_name` | string | Name of the metric |
| `metric_value` | number | Aggregated value |
| `dimensions` | object | Flexible dimensions as JSON |

### Example record
```json
{
  "date": "2026-01-01",
  "source_system": "sales",
  "metric_name": "revenue",
  "metric_value": 119.97,
  "dimensions": {
    "region": "EU"
  }
}
```

---

## 📦 Domains & Metrics

### 🛒 Sales
- `revenue`
- `orders_count`
- Dimensions:
  - `region`

### 🎧 Support
- `tickets_created`
- `tickets_resolved`
- `resolution_time_hours` (daily average)
- Dimensions:
  - `category`
  - `priority`
  - `team`

### 🧑‍💼 HR
- `hours_worked`
- Dimensions:
  - `department`

### ⚙️ Operations
- `request_count`
- `error_count`
- `response_time_ms` (daily average)
- Dimensions:
  - `service`
  - `environment`

---

## 🔁 Data Flow

1. **Ingestion**
   - REST APIs consumed via HTTP
   - CSV / JSON files read from disk
2. **Normalization**
   - Domain-specific fields mapped into metric records
3. **Aggregation**
   - Metrics aggregated daily by business-relevant dimensions
4. **Gold Consolidation**
   - All domains merged into a single Gold stream
5. **Persistence**
   - Metrics appended to Google Sheets for reporting

---

## 🛠️ Tech Stack

- **n8n** – Workflow orchestration
- **JavaScript** – Data transformation and aggregation
- **Mock REST APIs** – Simulated operational systems
- **Google Sheets API** – Reporting storage
- **Docker** – Local execution environment

---

## 📁 Repository Structure

```
.
├─ workflows/
│  ├─ sales.json
│  ├─ support.json
│  ├─ hr.json
│  ├─ ops.json
│  └─ gold-final-union.json
├─ mock-api/
│  └─ sales.json
├─ README.md
└─ docker-compose.yml
```

---

## 🚀 How to Run

1. Start n8n locally (Docker recommended)
2. Import workflows from the `/workflows` directory
3. Configure Google Sheets credentials
4. Execute the workflow manually or on a schedule
5. Review results in Google Sheets

---

## 🎯 Use Cases

- Daily operational KPI reporting
- Lightweight analytics layer without a data warehouse
- Cross-domain performance analysis
- Data engineering and automation portfolio project

---

## ⚠️ Notes & Limitations

- Data sources are mock datasets
- No deduplication or idempotency logic implemented
- Designed for reporting, not transactional workloads
- Easily extensible to additional domains or storage backends

---

## 📜 License

MIT
