---
layout: page
title: Are Hospital Quality Indicators Causal?
description: Testing whether risk-adjusted hospital quality measures capture real differences in care
importance: 2
category: research methods
---

Hospital quality indicators like 30-day mortality rates and readmission rates are published to help patients choose hospitals and to reward or penalize providers. But do these measures actually capture differences in hospital quality, or are they confounded by the types of patients each hospital treats?

**Chandra, Dalton, and Staiger (2023)** tests this using hospital closures as a natural experiment.

## The Idea

When a hospital closes, its patients must go elsewhere. If quality measures are meaningful, outcomes should change in predictable ways:
- Patients displaced from **high-quality** hospitals should see worse outcomes
- Patients displaced from **low-quality** hospitals should see better outcomes

We test whether this is what actually happens.

## What We Find

**Risk-adjusted measures get the direction right.** When quality indicators predict a closure will harm patients, mortality rises. When they predict improvement, mortality falls.

**But they overstate the magnitude.** Risk-adjusted indicators overestimate effects on mortality and readmissions by about 10%, and overstate effects on costs and length of stay by around 40%.

**Risk adjustment helps.** Unadjusted measures overstate effects even more—by 30-50% depending on the outcome.

**Health outcomes predict better than costs.** Mortality and readmission measures are more accurate than resource utilization measures.


## Resources

- [NBER Working Paper 31789](https://www.nber.org/papers/w31789) - Full paper
- [NBER Bulletin Summary](https://www.nber.org/bh/20241/how-informative-are-risk-adjusted-hospital-quality-measures) - Non-technical overview

## Stata Package

The methods from this paper are implemented in a public Stata package:

<div class="repositories d-flex flex-wrap gap-2 flex-md-row flex-column justify-content-between align-items-center">
{% include repository/repo.liquid repository="daltonmaurice/hospital_ebayes" %}
</div>

```stata
net install hospital_ebayes, from(https://raw.githubusercontent.com/daltonmaurice/hospital_ebayes/main)

hospital_ebayes mortality, hospitalid(providerid) year(year) ///
    controls(age female comorbidities)
```

The package implements empirical Bayes shrinkage estimators that adjust for patient selection, based on value-added methods from education research.
