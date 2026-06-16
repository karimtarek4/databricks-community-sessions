# Session 01 — Medallion Architecture and OBT Models

**Date:** 9 June 2026
**Presenters:** Kareem Tarek Fathy & Menna Mehrem
**Community:** Databricks Community (Deloitte)

## What we covered

We walked through how Medallion Architecture is built end-to-end on Databricks, using a Unity Catalog setup with a landing zone and bronze, silver, and gold schemas. And created an OBT from facts and dimensions. Along the way we covered:

- **Unity Catalog basics** — catalogs, schemas, and the namespace hierarchy
- **Volumes** — creating a managed volume and accessing files (CSV, JSON) through it
- **Landing** — landing raw files as views, 
- **Bronze** — cleaning and deduplicating records, casting types
- **Silver** — fixing malformed JSON, and exploding nested arrays into a fact table
- **Gold layer (with OBT)** — building a dimension table (customer), and a One Big Table (OBT) that joins them together for analytics

We closed with a Q&A covering ACID properties, OBT use cases, UNION operations, and data persistence.

## Notebooks

| Notebook | What it does |
|----------|--------------|
| `1.1 Setup.ipynb` | Creates the catalog, schemas (landing, bronze, silver, gold), and a managed Volume; covers the Unity Catalog namespace hierarchy and Databricks magic commands |
| `2.1 Landing.ipynb` | Creates unmaterialized views over raw customer (JSON), address (tab-CSV), and order (raw text) files in the Volume; explains why orders are landed as text rather than JSON |
| `2.2 Bronze.ipynb` | Profiles the raw customer data (nulls, duplicates), then deduplicates using `ROW_NUMBER()` to keep the latest record per customer → `bronze.customers` |
| `2.3 Silver.ipynb` | Casts types and derives `age` → `silver.customers`; pivots long-format addresses → `silver.addresses`; repairs malformed JSON, parses with `FROM_JSON`, and explodes nested items with `LATERAL VIEW EXPLODE` → `silver.fact_line_items` |
| `2.4 Gold.ipynb` | Joins silver customers and addresses into `gold.dim_customer`, then builds `gold.obt_orders` (One Big Table) by denormalizing all line-item facts with customer and address attributes; includes example analytics queries |

## Files in this folder

- `notebooks/` — the four SQL notebooks above
- `data/` — sample data used for the walkthrough

## Prerequisites

- Databricks workspace with Unity Catalog enabled
- Cluster running a recent Databricks runtime (DBR 14.x or later recommended)