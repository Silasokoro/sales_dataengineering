# Pipeline runbook

## Before a run

1. Confirm that the expected source files for customers, sales, and products have landed in the configured ADLS Gen2 path.
2. Verify the configured storage connection and permissions, Unity Catalog access, catalog/schema names, and destination tables.
3. Open the Databricks Job `Salesautopipeline` and review the current task graph.

## Execute

Run the complete job from Databricks Workflows / Jobs. Use the task-level run details and logs to find failures. Avoid running downstream tasks in isolation unless their required upstream tables already exist and are current.

## After a run

- Review the status of `setup`, Bronze, Silver, enrichment, Gold, and `dq` tasks.
- Confirm that expected target tables exist in Unity Catalog and their latest data reflects the run.
- Review the `dq` task output and investigate failures before using the Gold output for reporting.
- Record the run date, job result, and any remediation performed in the project notes.

## Common investigation areas

| Symptom | What to check |
|---|---|
| A source is not ingested | ADLS Gen2 path, file arrival, file format, storage permissions, and `COPY INTO` options. |
| A Bronze task fails | Source schema or format changes, access to the landing location, and task logs. |
| A Silver task fails | Expected columns, data types, null handling, joins, and duplicate handling in the notebook. |
| Enrichment returns missing matches | Actual join keys and unmatched customer/product identifiers. |
| Gold or `dq` fails | Upstream task status, target table permissions, transformation assumptions, and validation output. |

## Environment note

The recorded development catalog/schema is `abs_dev.abs_db`. Verify the actual target names and permissions in the workspace; do not copy development settings blindly into another environment.
