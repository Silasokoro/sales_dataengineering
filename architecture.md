# Sales pipeline architecture

## End-to-end flow

Google Sheets holds the customer, sales, and product datasets. The datasets are uploaded to an ADLS Gen2 landing location. Databricks uses `COPY INTO` for incremental file ingestion, then transforms the data through the Bronze, Silver, and Gold layers in Unity Catalog. The Databricks Job `Salesautopipeline` orchestrates the processing tasks, including the `dq` task.

![Architecture](diagrams/sales_pipeline_architecture.svg)

## Layer responsibilities

- **Landing zone — ADLS Gen2:** keeps source files available for ingestion and traceability.
- **Bronze:** stores source-aligned records with limited transformation.
- **Silver:** cleans and standardizes the customer, product, and sales datasets before analytical joins.
- **Enrichment:** `silver_enriched` combines relevant Silver entities into a dataset for downstream analytics.
- **Gold:** holds curated output for analytical use. Refer to the actual Gold notebook for the table names and business logic.
- **Data quality:** `dq` is a task in the orchestration workflow. Its exact assertions should be read from the implementation.

## Orchestration tasks

The task names associated with the workflow are `setup`, `bronze_sales`, `bronze_products`, `bronze_customers`, `silver_sales`, `silver_products`, `silver_customers`, `silver_enriched`, `gold`, and `dq`.

The diagram is a documentation-level dependency map. The Databricks Job UI remains the authoritative reference for configured task dependencies.
