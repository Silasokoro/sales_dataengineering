# Logical data model

## Business entities

The source contains three business datasets:

- **Customers** — customer attributes.
- **Products** — product attributes.
- **Sales** — sales or transaction records that can be enriched with customer and product context.

![Conceptual data relationships](diagrams/sales_logical_data_model.svg)

## Relationship assumptions

The conceptual model assumes that one customer can be associated with multiple sales records and one product can appear in multiple sales records. This is a business-level illustration, not a validated physical schema.

Before treating the model as exact, confirm the actual key columns in the source files and transformation notebooks. In particular, verify the customer identifier and product identifier carried by each sales row, the sales business key, and how missing or unmatched references are handled. This repository documentation does not invent column names or data types that have not been confirmed.
