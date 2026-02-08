---
layout: page
title: Health Systems and Provider Database (HSPD)
description: Database mapping healthcare providers to health systems using ownership and administrative data
importance: 4
category: research methods
---

The HSPD identifies and classifies health systems by tracing corporate ownership and management relationships across multiple administrative data sources. Rather than inferring connections from patient-sharing patterns, it directly links providers to their owning and managing entities using tax identification numbers (TINs) from enrollment, tax, and securities filings.

This database is now used in research published in JAMA and Health Services Research.

## How It Works

The methodology has five major steps:

1. **Create provider files** — Combine data from PECOS, IQVIA, AHA, CMS POS, Medicare claims, and commercial claims to build comprehensive files of physicians, hospitals, and post-acute care facilities
2. **Identify corporate ownership networks** — Use IRS 990 filings, SEC 10-K filings, IRS Business Master File, and M&A transaction data to group commonly owned corporate TINs into networks
3. **Link hospitals to owning entities** — Match hospital TINs to corporate networks, resolving joint ventures and multi-owner situations using ownership percentages
4. **Link physician practices to owning entities** — Identify practice ownership from PECOS, IRS filings, and claims-based analysis of hospital outpatient department billing
5. **Qualify networks as health systems** — Apply minimum criteria (at least one acute care hospital, 10 primary care physicians, and 50 total physicians within a single hospital referral region)

## Technical Stack

- **SAS, R, Stata**: Core algorithm implementation
- **R igraph**: Network construction from TIN-TIN ownership pairs
- **Data sources**: CMS PECOS, IRS 990/BMF, SEC 10-K, S&P Capital IQ, AHA, CMS POS, Medicare and commercial claims, IQVIA

## Publications Using This Data

**JAMA (2023):** "Organization and performance of US health systems"
Beaulieu, Chernew, McWilliams, Landrum, Dalton, et al.

**Health Services Research (2025):** "Integrated health systems and medical care quality during the COVID-19 pandemic"
Ghosh, Beaulieu, Dalton, et al.

## Resources

- [HSPD Project Page](https://www.nber.org/programs-projects/projects-and-centers/measuring-clinical-and-economic-outcomes-associated-delivery-systems/health-systems-and-provider-database-hspd-methodology-data-resources)
- [HSPD Technical Documentation (PDF)](https://www.nber.org/sites/default/files/2021-03/HSPD%20Technical%20Documentation%20-%20January%202021.pdf)
- [JAMA Paper](https://jamanetwork.com/journals/jama/article-abstract/2800592)
