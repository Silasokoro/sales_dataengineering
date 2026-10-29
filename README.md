# sales_dataengineering
An end-to-end batch data engineering project that moves sales, customer, and product datasets from Google Sheets into Azure Data Lake Storage Gen2 (ADLS Gen2), then processes them in Azure Databricks using COPY INTO, Unity Catalog, Delta tables, and the Medallion Architecture.

The Databricks workflow Salesautopipeline orchestrates the configuration, Bronze, Silver, Gold, and data-quality tasks.

Project overview

The pipeline turns source datasets into a structured lakehouse flow:

Source data — customer, sales, and product datasets maintained in Google Sheets.

Landing — source datasets are uploaded to ADLS Gen2.

Incremental ingestion — Databricks COPY INTO loads new source files into the Bronze layer.

Transformation — data moves through Bronze, Silver, and Gold tables registered in Unity Catalog.

Orchestration — the Databricks Job Salesautopipeline coordinates task dependencies.

Quality checks — the dq task runs as part of the workflow.

# Architecture

<img width="1801" height="242" alt="sales_pipeline_architecture" src="https://github.com/user-attachments/assets/2fba7ec4-0f00-43b7-9ac8-c284ed6f3c72" />


See Architecture notes for the responsibilities of each layer and Pipeline runbook for execution guidance.

Medallion layers

Layer / stage

Tasks

Purpose

Setup / configuration

setup

Prepare the configuration and lakehouse objects required by downstream tasks.

Bronze

bronze_sales, bronze_products, bronze_customers

Ingest source-aligned data with minimal transformation.

Silver

silver_sales, silver_products, silver_customers

Prepare cleaned and conformed datasets for downstream use.

Enrichment

silver_enriched

Bring sales data together with relevant customer and product information.

Gold

gold

Publish curated data for analytics and reporting.

Data quality

dq

Run the data-quality task included in the workflow.

The names above describe the pipeline tasks. Exact table columns, Gold outputs, and individual quality rules should be confirmed from the notebooks and the Databricks Job configuration rather than inferred from this README.

Pipeline dependencies

The following diagram documents the logical task flow. Compare it with the live task graph in Databricks before changing dependencies.



Editable diagram source: sales_pipeline_dag.mmd.

Logical data relationships

At a business level, sales records relate to customers and products. The relationship diagram is conceptual: identifiers shown in it are examples, not a claim about the exact column names in the source files.



See Logical data model for the key assumptions to validate against the actual datasets.

Technology stack

Azure Data Lake Storage Gen2 — landing zone for the source datasets.

Azure Databricks — ingestion, transformation, and job orchestration.

Databricks COPY INTO — incremental file ingestion into Delta tables.

Delta Lake — table storage across the Medallion layers.

Unity Catalog — table governance and organization.

PySpark / Spark SQL — data processing in Databricks notebooks.

Databricks Jobs — orchestration through Salesautopipeline.

Repository structure

sales_dataengineering/
├── Sales_batch_pipeline/       # Existing Databricks notebook/job assets; retain these files
├── config/
│   └── README.md               # Configuration and secret-handling notes
├── data/
│   └── README.md               # Guidance for source/sample data
├── docs/
│   ├── architecture.md
│   ├── data_model.md
│   ├── pipeline_runbook.md
│   └── diagrams/
│       ├── sales_pipeline_architecture.svg
│       ├── sales_pipeline_dag.svg
│       ├── sales_logical_data_model.svg
│       └── *.mmd                # Editable Mermaid diagram sources
├── tests/
│   └── README.md               # Validation guidance and suggested checks
├── .gitignore
├── CONTRIBUTING.md
└── README.md

Running the pipeline

The pipeline runs in the configured Databricks workspace; this repository documentation does not provision Azure resources or replace workspace-specific configuration.

Confirm that the latest customer, sales, and product source files are present in the configured ADLS Gen2 landing path.

Check that the storage access, external location or volume, catalog, schema, and target table settings match the workspace.

In Databricks Workflows / Jobs, open Salesautopipeline and review its task dependencies.

Run the complete job and inspect each task's status and logs.

Confirm that the expected Bronze, Silver, and Gold tables were written to Unity Catalog, and review the output from the dq task.

The current development naming recorded for this project is catalog abs_dev and schema abs_db (abs_dev.abs_db). Treat these as environment-specific values and verify them in the target workspace before running the job.

Data quality and validation

A dq task is included in the described workflow. The exact assertions it performs are defined by the project notebooks and should be reviewed there. For an auditable pipeline, useful checks to confirm or add include:

Required identifiers are not null.

Customer and product references in sales resolve to the relevant Silver datasets.

Duplicate business keys are identified according to the chosen rules.

Data types and numeric fields conform to the expected schema.

Row counts and key measures are reconciled between layers where appropriate.

See tests/README.md. Suggested checks are recommendations, not a claim that every check is already implemented.

Data and security

This repository should contain code and documentation, not credentials or confidential source records. Do not commit Azure storage keys, access tokens, service-principal secrets, .env files, or unapproved customer data. Keep authentication in Databricks-supported secret or identity configuration and keep private source files outside Git unless you have authorization to publish them.

Project scope

This README documents the implementation described for this project: Google Sheets source datasets, ADLS Gen2 landing, incremental ingestion with COPY INTO, Unity Catalog, Medallion transformations, and Databricks Job orchestration. It does not claim that Azure Data Factory, Bicep infrastructure deployment, automated CI/CD, a Power BI dashboard, or specific production-scale performance benchmarks are implemented.

Author

Silas Okoro · GitHub

Project repository: sales_dataengineering
