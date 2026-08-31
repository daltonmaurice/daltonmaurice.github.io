---
layout: page
title: "IRS Form 990 e-file corpus (2010–2026)"
description: 6.2M nonprofit tax filings parsed into 15 analysis-ready Parquet tables, with per-filing provenance
importance: 3
category: data infrastructure
---

The IRS publishes electronically filed Form 990 returns as raw XML—one file per filing, schemas that drift across years, and no practical way to ask a question across the whole corpus. This project turns that into something you can query.

**6.2M filings, 160.4M rows, tax years 2010–2026**, parsed into 15 analysis-ready Parquet tables covering the main return and Schedules A through R. Released CC0.

## What's in It

- **3.97M Form 990** and **2.24M Form 990-EZ** filings; 990-PF, 990-T and 990-N postcards are out of scope
- 15 relational tables—main return, officers and key persons, per-schedule tables, and facilities
- Hive-partitioned by tax year, sorted by EIN within each partition
- Every table carries `ein`, `object_id`, `return_type_cd` and `tax_year` for joins
- Concordance naming (`F9_<part>_<field>` on the main return, per-schedule prefixes elsewhere) so fields stay stable across schema versions
- A 16th config ships the original e-filed XML, for audit and re-extraction

## Provenance and Correctness

Filings are assembled from NBER archival mirrors (pre-2019) and IRS-direct downloads (2019–2026). Where the same filing appears in both, deduplication is deterministic and documented: latest archive year wins, and on ties the IRS-direct copy beats the NBER copy.

Each build emits a per-`object_id` provenance ledger and an `export_manifest.json` pinning the code commit that produced it, so any row can be traced back to the filing and the build that created it. Row counts are verified against the manifest by a multi-gate integrity check covering grain uniqueness and EIN/year coverage.

Coverage limits are documented rather than smoothed over: e-filed returns only, 2025–2026 still filling in behind the filing-season lag, and Schedule R ownership percentages increasingly missing after 2018 because they are absent from the upstream IRS XML.

## Using It

The tables stream directly from HuggingFace—no download required:

```sql
INSTALL httpfs; LOAD httpfs;

SELECT ein, F9_00_ORG_NAME_L1, F9_09_EXP_TOT_TOT
FROM read_parquet('hf://datasets/mauricedalton/irs-990/main/tax_year=2023/*.parquet');
```

Also readable with `datasets`, Polars, pandas and Dask.

## Resources

- [Dataset on HuggingFace](https://huggingface.co/datasets/mauricedalton/irs-990)
