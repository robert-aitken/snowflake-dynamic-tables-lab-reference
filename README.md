# Snowflake Dynamic Tables Lab Reference

Personal lab reference for a Snowflake Dynamic Tables workshop covering declarative pipelines, target lag, refresh modes and SQL-based pipeline design.

This repository is a personal follow-along reference based on a Snowflake Dynamic Tables workshop. It is kept as a learning-history reference rather than as an active production project.

The original repository used for this fork was:

https://github.com/krisajenkins/sf-dynamic-tables

That repository was forked from:

https://github.com/MLH/build25-data-engineering-workshop

## Workshop Details

| Field | Details |
|---|---|
| Workshop | Creating Declarative Data Pipelines with Dynamic Tables |
| Provider | Snowflake |
| Speaker | Kris Jenkins |
| Format | Virtual Workshop |
| Date Completed | 29/04/2026 |
| Duration | 1.5 Hours |
| Badge Awarded | Yes |
| Credential URL | https://developerbadges.snowflake.com/7b142346-9ec4-4a33-8a49-a9371b379239 |

## Repository Contents

This repository contains SQL scripts and reference material used to follow along with the workshop.

| File | Purpose |
|---|---|
| setup.sql | Sets up the Snowflake environment for the workshop. |
| references/create-dt.sql | Creates Dynamic Tables used during the workshop. |
| references/pipeline.sql | Builds the Dynamic Table pipeline pattern used in the lab. |

## Lab Order Followed

I followed the workshop in this order:

1. Ran setup.sql
2. Ran references/create-dt.sql
3. Ran references/pipeline.sql

## What Gets Created

The setup creates a small Snowflake workshop environment, including:

### Warehouse

- COMPUTE_WH: X-Small warehouse with auto-suspend and auto-resume enabled.

### Databases

- RAW_DB: Storage for raw, unprocessed data.
- ANALYTICS_DB: Storage for transformed and analytics-ready data.

### Sample Tables

- CUSTOMERS: Synthetic customer records.
- PRODUCTS: Synthetic product records.
- ORDERS: Synthetic order records.

## High-Level Topics Covered

The workshop covered:

- Snowflake Dynamic Tables
- Declarative SQL-based pipelines
- Target lag
- Refresh modes
- DOWNSTREAM target lag
- Incremental and full refresh concepts
- Dynamic Table dependency graphs
- Staging and fact table pipeline patterns
- SQL-first data transformation in Snowflake

## Personal Takeaways

My main takeaway was that Snowflake Dynamic Tables can be used to build declarative data pipelines using SQL, without manually orchestrating every transformation step.

Dynamic Tables are useful for analytics or gold-layer modelling where transformed tables need to stay reasonably fresh from upstream source data.

I learnt that a Dynamic Table is defined by:

- the SQL query
- the warehouse used for refreshes
- the target lag
- the refresh behaviour

I also learnt that TARGET_LAG controls the desired freshness of the Dynamic Table. For example, a table with a target lag of 30 minutes should be kept no more than roughly 30 minutes behind the source data.

The workshop helped clarify the difference between materialized views and Dynamic Tables. Materialized views are useful for certain single-table optimisation patterns, while Dynamic Tables are more flexible and can support broader transformation logic.

A key practical learning point was the use of DOWNSTREAM. Upstream or intermediate Dynamic Tables can be set to DOWNSTREAM, while the final table that users care about can have an explicit target lag. This allows Snowflake to refresh supporting tables when needed to satisfy the downstream table's freshness target.

If every Dynamic Table in a pipeline is set to DOWNSTREAM and no final table has an explicit target lag, there is no refresh schedule driving the pipeline.

## Example Pipeline Pattern

A practical pattern from the workshop was:

sql ALTER DYNAMIC TABLE FCT_CUSTOMERS_ORDERS_DT SET TARGET_LAG = '30 minutes';  ALTER DYNAMIC TABLE STG_CUSTOMERS_DT SET TARGET_LAG = DOWNSTREAM;  ALTER DYNAMIC TABLE STG_ORDERS_DT SET TARGET_LAG = DOWNSTREAM; 

In this pattern:

- FCT_CUSTOMERS_ORDERS_DT is the downstream fact table that users care about.
- FCT_CUSTOMERS_ORDERS_DT gets the explicit freshness target.
- STG_CUSTOMERS_DT and STG_ORDERS_DT are supporting tables.
- The supporting tables refresh when needed to satisfy the downstream fact table's target lag.

## How This Relates To My Work

This workshop is relevant to Snowflake pipeline design, declarative data engineering, SQL-based transformation, analytics modelling and gold-layer table refresh patterns.

It is also useful background for comparing Snowflake Dynamic Tables with other pipeline approaches, such as dbt models, scheduled SQL jobs, Azure Data Factory pipelines and notebook-based processing.

## Private Notes

I kept a private Word document while following the workshop, including screenshots, trial account details, workspace links and personal notes.

That document is not included in this repository because it contains private account details, workspace links and screenshots.

This README only contains a rewritten summary of my own learning.

## What Is Not Included

This repository does not include:

- Passwords
- Trial account details
- Work email addresses
- Workspace URLs
- Private screenshots
- Private Word notes
- Copied workshop slide screenshots
- Private course or event material

## Notes On This Fork

This fork is retained as a personal archival reference for learning history only.

It should not be treated as a production-ready Snowflake project.

The original upstream history and commit metadata have been preserved by GitHub through the fork relationship.

## License

See LICENSE for details.
