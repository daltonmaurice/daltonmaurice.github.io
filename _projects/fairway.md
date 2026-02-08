---
layout: page
title: "Ingestion framework for Heterogeneous Research Data"
description: Scalable pipeline for reading and transforming research data across formats
importance: 1
category: data infrastructure
---

A scalable, modular framework for ingesting research data from heterogeneous sources. Handles messy file formats, inconsistent schemas, legacy codebases—and produces clean, standardized outputs ready for analysis.

Focused on HPC and PySpark for distributed processing. Designed to be extended: add new data sources without touching existing pipelines.

<div class="repositories d-flex flex-wrap gap-2 flex-md-row flex-column justify-content-between align-items-center">
{% include repository/repo.liquid repository="DISSC-yale/fairway" %}
</div>

## Technical Stack

- **Python**: CLI tooling
- **DuckDB**: Ingesting and transforming small to medium data
- **PySpark**: Ingesting and transforming large data
- **Modular Design**: Plugin architecture for new data sources

## What It Does

- Reads data from multiple formats
- Applies configurable transformations and validation rules
- Outputs standardized datasets
- Tracks lineage so you know where every row came from
