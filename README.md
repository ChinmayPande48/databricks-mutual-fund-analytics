# Databricks Mutual Fund Analytics

---

## 1. Project Title & Overview

The **databricks-mutual-fund-analytics** project is a data pipeline built on the Databricks Lakehouse Platform to automate the ingestion, transformation, and analysis of mutual fund performance data. Its primary goal is to provide a structured and clean dataset for financial analytics, enabling users to gain insights into mutual fund performance, trends, and risk metrics.

---

## 2. Architecture

This project uses the **Medallion Architecture**, a multi-layered data design pattern that promotes data quality and reliability. Each layer serves a specific purpose, from raw data ingestion to curated, business-ready datasets. 

* **Bronze Layer:** This layer stores the raw, untransformed source data exactly as it was ingested. It's the immutable source of truth for the project.
* **Silver Layer:** Data in this layer is cleaned, structured, and enriched. It's the first step in the transformation process, creating a single, consistent table named **`fund_nav_details`**. This layer is suitable for data scientists and analysts who need a clean dataset.
* **Gold Layer:** The final layer is for business-level analytics. It contains highly refined, aggregated data and views tailored for specific business intelligence and reporting needs. This is the "analytics-ready" layer, used by business users and dashboards.

---

## 3. Gold Layer Tables & Views

The Gold layer produces several key assets for analytics:

* **`monthly_average_nav` Table:** This aggregated table provides a high-level view of fund performance trends over time. It's used for historical analysis and trend visualization. Key metrics include `fund_id`, `nav_date` (at a monthly granularity), and `average_nav`.
* **`fund_performance_yoy` View:** This standard SQL view is designed to rank mutual funds based on their year-over-year performance. The calculations for metrics like return and rank are performed at query execution time, ensuring the results are always up-to-date. Key metrics include `fund_id`, `one_year_return_pct`, `performance_rank`, and `latest_date`.
* **`sharpe_ratio` View:** This standard SQL view provides a crucial risk-adjusted performance metric. The Sharpe Ratio measures the excess return per unit of volatility. Like the performance view, this is a standard SQL view, and the `sharpe_ratio` metric is calculated on the fly.

---

## 4. Key Metrics & Business Value

The project calculates several key metrics in the Gold layer to provide actionable business value:

* **Monthly Average NAV:** Provides a high-level overview of a fund's value trends, essential for **historical analysis**.
* **Year-over-Year Return & Rank:** Enables direct comparison and **ranking of fund performance** over a one-year period, helping identify top and bottom performers.
* **Sharpe Ratio:** A critical metric for **evaluating risk-adjusted returns**. It helps investors compare funds with different levels of volatility.
* **Expense Ratio:** Although an external input to the pipeline, this metric is vital for calculating a fund's **net return**. It represents the cost of owning the fund.
* **Standard Deviation:** A measure of a fund's **volatility and risk**. Used in the calculation of the Sharpe Ratio.

---

## 5. How to Run the Project

To run this project, you'll need access to a Databricks workspace and a cluster with the necessary permissions.

**Prerequisites:**

* A Databricks Workspace
* Access to a Databricks Cluster (e.g., a standard or job cluster)
* Python 3.x environment

**Instructions:**

1.  Upload the Databricks notebook (`mutual_fund_analytics.ipynb`) to your workspace.
2.  Attach the notebook to an available cluster.
3.  Before running, configure the necessary variables at the top of the notebook, such as the name of the Silver layer table (`silver_table = "mutual_fund_silver.fund_nav_details"`).
4.  Run the notebook cells sequentially to execute the data pipeline from ingestion to the Gold layer creation.

---

## 6. Technologies Used

* **Databricks:** The core platform for data processing and analytics.
* **Apache Spark (PySpark):** The distributed computing engine used for efficient data transformation.
* **Delta Lake:** The storage layer for all medallion tables, providing ACID transactions and schema enforcement.
* **SQL:** Used extensively for data manipulation and creating views within the Gold layer.
* **Python:** The primary programming language for the notebook logic.
