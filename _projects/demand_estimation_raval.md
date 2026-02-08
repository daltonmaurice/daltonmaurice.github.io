---
layout: page
title: "Beyond Geography: Demand-Based Hospital Market Definition"
description: Toolkit for defining hospital markets based on patient substitution patterns
importance: 1
category: research methods
---

Standard approaches to defining hospital markets rely on arbitrary geographic boundaries (counties, HRRs) or patient flow thresholds. These miss the economic reality—hospitals 10 miles apart may not compete if they serve different patient populations, while hospitals 50 miles apart may be close substitutes for specialized care.

This toolkit implements the **Raval, Rosenbaum, and Tenn (2017) semiparametric demand estimation approach** to define markets based on actual patient choice behavior.

## Key Insight

Instead of drawing lines on a map, measure how patients actually substitute between hospitals. Group patients with similar characteristics (ZIP, diagnosis, age) and use observed choice shares as probability estimates. No functional form assumptions, computationally tractable at scale.

## What It Produces

**Diversion ratios:** If hospital A closed, what fraction of its patients would go to hospital B? This identifies actual competitive relationships, not just geographic proximity.

**Patient-weighted HHI:** Concentration measures grounded in patient flows rather than arbitrary market definitions.

## Basic Usage

```stata
semiparametric_demand_estimation, ///
    admissions("/path/to/data.dta") ///
    outdbins("/path/to/output/dbins.dta") ///
    binthresh(25) ///
    binlist("zip ep_drg pat_age") ///
    idvar(aha_id)

calculate_diversion_ratios, ///
    predictions("/path/to/output/predictions.dta") ///
    out("/path/to/output/diversion.dta")
```

## Technical Stack

- **Stata 17+** with Mata acceleration
- **R** benchmarking against `healthcare.antitrust` package
- **Input:** Patient-level discharge data with ZIP, DRG, age, hospital ID

## References

- Raval, Rosenbaum, & Tenn (2017). "A semiparametric discrete choice model." *Economic Inquiry*, 55(4).
- Brot-Goldberg et al. (2024). "Is there too little antitrust enforcement in the US hospital sector?"

## Repository Access

**Private repository** developed for internal research. Contact for collaboration.
