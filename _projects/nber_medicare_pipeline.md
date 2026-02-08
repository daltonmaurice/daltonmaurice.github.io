---
layout: page
title: Automating Medicare Claims Processing
description: Automated pipeline for processing Medicare claims data
importance: 3
category: data infrastructure
---

Ingesting CMS data previosly relied on SAS scripts needing manual adjustments. I modernized the stack by deploying Apache Airflow to automate ingestion and transformation. What used to be a fragile, manual yearly process is now a reliable pipeline handling multi-terabyte datasets.

## Technical Stack

- **Apache Spark**: Data transformation and validation
- **Apache Airflow**: Workflow orchestration and scheduling
- **Python**: Data transformation and validation
- **Parquet/Arrow**: Columnar storage for fast analytics

## What It Does

- Ingests raw CMS Medicare RIF files
- Applies consistent cleaning and validation rules
- Outputs analysis-ready datasets partitioned by year
- Runs automatically when new data arrives
- Automates the reading of metadata files to generate schemas for Spark

## Resources

- [NBER Medicare Public Data](https://data.nber.org/medicare/new_Public/index.html) - Public-use files of Medicare knowledge base for users of this data
