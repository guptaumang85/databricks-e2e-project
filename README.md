# Databricks End-to-End Project — Restaurant Analytics & ML Platform

A real-world, end-to-end **Restaurant Analytics Platform** built on Databricks. It combines
streaming and batch ingestion, a medallion (bronze → silver → gold) architecture, AI-powered
sentiment analysis, and BI dashboards into a single lakehouse project.

![Project architecture](projects/databricks-e2e-project/diagrams/project_architecture.png)

## Highlights

- **Streaming ingestion** of live orders from **Azure Event Hub**
- **Batch ingestion / CDC** of operational data from **Azure SQL** via **LakeFlow Connect**
- **Medallion architecture** (Bronze → Silver → Gold) implemented with **Spark Declarative Pipelines**
- **Unity Catalog** for governance across the `01_bronze` / `02_silver` / `03_gold` schemas
- **AI-powered sentiment analysis** of customer reviews using **Mosaic AI**
- **AI/BI dashboards** for sales performance and review insights

## Tech stack

| Layer | Technology |
|-------|------------|
| Streaming source | Azure Event Hub |
| Batch source / CDC | Azure SQL + LakeFlow Connect |
| Processing | Databricks, Apache Spark, Spark Declarative Pipelines |
| Governance | Unity Catalog |
| ML / GenAI | Mosaic AI (sentiment analysis) |
| BI | Databricks AI/BI Dashboards |

## Repository layout

```
projects/databricks-e2e-project/
├── 00_synthetic_data/        # scripts that generate synthetic source data
│   ├── *.py                  # customers, orders, reviews, Event Hub order stream
│   ├── data/                 # generated CSV seed data
│   └── sql/                  # Azure SQL setup + silver/gold schema definitions
├── 01_pipelines/
│   ├── pipeline_ingest_eventhub.py          # streaming ingestion into bronze
│   └── pipeline_bronze_to_gold/
│       ├── silver/           # fact_orders, fact_order_items, fact_reviews
│       └── gold/             # d_sales_summary, d_customer_360, d_restaurant_reviews
├── diagrams/                 # architecture and data-flow diagrams
├── commands_used.md          # ad-hoc SQL / Bash commands used during the build
└── dashboard_metrics.md      # metrics powering the BI dashboards
spark_declarative_pipelines/  # declarative pipeline assets
lakeflow_jobs/                # LakeFlow Connect job assets
```

## Data model (Gold layer)

The gold layer exposes analytics-ready tables (see
`projects/databricks-e2e-project/00_synthetic_data/sql/gold_schemas.md`):

- **`d_sales_summary`** — daily orders, revenue, AOV, and order-type breakdowns
- **`d_customer_360`** — per-customer lifetime spend, loyalty tier, favourites, and churn-risk flag
- **`d_restaurant_reviews`** — per-restaurant ratings, sentiment, and review distributions

## Dashboards

Two AI/BI dashboards are built on the gold tables (see `dashboard_metrics.md`):

1. **Restaurant Chain Performance** — total orders/revenue, AOV, daily sales, best-selling items,
   peak-hour heatmap, revenue by order type and food category.
2. **Review Insights** — review volume, average rating, sentiment trend, ratings distribution, and
   issue categorisation.

## Getting started

1. Provision an Azure environment (Event Hub + Azure SQL) and a Databricks workspace with Unity
   Catalog enabled.
2. Copy `.env.example` to `.env` and fill in your Event Hub connection details:
   ```
   EVENTHUB_CONNECTION_STRING=
   EVENTHUB_NAME=
   ```
3. Install local dependencies: `pip install -r requirements.txt`
4. Generate synthetic source data with the scripts in `00_synthetic_data/`.
5. Deploy the pipelines in `01_pipelines/` to build the bronze → silver → gold layers.
6. Build the dashboards on top of the gold tables.

See **`projects/databricks-e2e-project/commands_used.md`** for the exact SQL and Bash commands used
throughout the project.
