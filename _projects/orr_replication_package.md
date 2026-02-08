---
layout: page
title: From Conditional Acceptance to Reproducible Code
description: Lessons from preparing a replication package for conditional acceptance at a top economics journal
importance: 6
category: presentations
redirect: https://daltonmaurice.github.io/orr-presentation/
---

A practical walkthrough of preparing an AEA-compliant replication package for the NBER working paper "Are Hospital Quality Indicators Causal?" This presentation covers the journey from conditional acceptance to final submission, including best practices learned along the way.

**Research Context:**
Our study using 20+ years of Medicare claims covering 30 million patients found that hospital quality indicators overstate causal impacts—by <10% for mortality/readmissions but ~40% for costs/length of stay. Hospital closures can reduce mortality by shifting patients to higher-quality facilities.

**Reproducibility Best Practices:**
- **Version Control**: Using pull requests to organize changes into meaningful updates that are easy to review later
- **Build Systems**: Achieving one-command replication using Make, Snakemake, or Data Version Control (DVC)
- **Package Development**: Creating reusable packages (example: [hospital_ebayes](https://github.com/daltonmaurice/hospital_ebayes)) to document methods
- **Table/Figure Management**: Auto-generating outputs through the build process; using org-mode to track manuscript evolution

**AI-Assisted ORR Process:**
- Using Gemini to parse AEA Data Editor guidance and create a step-by-step replication plan
- Leveraging Cursor + AWS Bedrock (Claude) to reorganize codebase following AEA standards
- Generating synthetic data for testing replication package workflows
- Balancing automation with manual verification for compliance

[View Full Presentation →](https://daltonmaurice.github.io/orr-presentation/)
