# Applying this documentation pack to the existing repository

This pack updates documentation and adds diagrams. It does **not** contain the existing Databricks notebook/job source files, so do not replace your repository with this pack as a standalone project.

1. Download and extract the ZIP.
2. Merge the extracted files into a clone of `https://github.com/Silasokoro/sales_dataengineering`.
3. Keep all existing files under `Sales_batch_pipeline/`. The added `Sales_batch_pipeline/README.md` is documentation only.
4. Review the logical data model and confirm the dependency edges against the live Databricks Job before publishing.
5. Review the catalog/schema in the README and remove or update any environment-specific value that is no longer correct.
6. Commit and push the merged files using your own GitHub credentials.

The GitHub repository was inspected publicly, but this session does not have an authenticated GitHub write connection, so no files were pushed to the remote repository.
