# Orchestrated-Data-Warehouse-Pipeline-dbt-Airflow-Snowflake


This repository contains the source code for a complete, production-style data warehouse pipeline, integrating the "modern data stack": dbt (for transformation), Snowflake (as the data warehouse), and Airflow (for orchestration).

The project demonstrates how to build a robust, testable, and automated ELT workflow. The dbt project itself transforms raw TPC-H (a standard industry benchmark) data into a clean, analytics-ready Star Schema (staging -> marts).

The entire pipeline is designed to be triggered and monitored by Apache Airflow, which runs the dbt commands on a schedule.

Key Concepts & Project Recall (File Breakdown)
This project is a classic example of a dbt implementation, organized for clarity and maintainability:

models Folder (The "T" in ELT):

staging: This layer is the first step. It contains simple SELECT statements (e.g., stg_tpch_orders.sql, stg_tpch_line_items.sql) that read from the raw source data (defined in tpch_sources.yml) and apply basic cleaning, renaming, and type-casting.

marts: This is the final Gold layer for business users.

int_...: Intermediate models (e.g., int_order_items.sql) that join and aggregate staging tables.

fact_orders.sql: The final Fact table, which connects all the data and is ready for analysis.

macros Folder (Custom Logic):

pricing.sql: You created a custom dbt macro here. This file contains reusable SQL logic (like a function) to calculate pricing, which is then used in your models. This keeps your model code clean (DRY - Don't Repeat Yourself).

tests Folder (Data Quality):

You implemented custom data tests to ensure data quality.

fact_order_date_valid.sql: A test to ensure no order dates are in the future or invalid.

fact_order_discount.sql: A test to ensure all discount values are within a valid range (e.g., between 0 and 1).

CI/CD (Automation):

The presence of .github and .circleci folders (from your other image) shows this project is built for Continuous Integration. This means every time you push new code, an automated service runs dbt compile and dbt test to make sure your changes don't break the pipeline.

Airflow & Snowflake (The "Orchestra"):

Snowflake: The cloud data warehouse where all your TPC-H data lives and where dbt runs its SQL.

Airflow (Implied by Title): Airflow is the "conductor." You would have an Airflow DAG that runs dbt run and dbt test on a schedule, ensuring this entire transformation pipeline runs automatically.
