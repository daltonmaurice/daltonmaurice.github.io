---
layout: page
title: "mintd: Versioned, Citable Data Products"
description: Data-product framework — producer/consumer registry, data contracts, access-tier governance, enclave delivery
importance: 2
category: data infrastructure
---

A lab produces a clean dataset, and several papers, analyses and dashboards depend on it. Six months later nobody can say which version of the data a published table was built from. **mintd** makes the dataset itself the unit of work: a **data product** that one project publishes, versions and cites, and that other projects import at a pinned commit.

It's a Python CLI that wraps Git (code and metadata) and DVC (the data bytes) into a single workflow, so the whole lab's datasets are discoverable from one catalog instead of hand-rolled S3 paths and folder conventions.

<div class="repositories d-flex flex-wrap gap-2 flex-md-row flex-column justify-content-between align-items-center">
{% include repository/repo.liquid repository="health-care-affordability-lab/mintd" %}
</div>

## Data Products

- **Producer/consumer split**: a producer project owns a dataset's metadata and its bytes in object storage; consumer projects import it and record exactly what they depended on
- **Registry and catalog**: a git-backed registry holds the catalog-audience subset of every producer's metadata, so consumers browse what's available with `mintd data list` rather than scraping storage
- **Data contracts**: every import records the pair `(contract_pin, artifact_pin)`—the producer's git commit and the DVC content hash—together, so a downstream analysis can never silently re-resolve to a different version of a dataset
- **Access tiers**: each project is classified `labonly`, `public` or `licensed` when it's created, and that governance classification travels with the product
- **Versioned releases**: `mintd publish` tags a release and updates the catalog entry, so a dataset can be cited at a specific version

## Enclave Delivery

For air-gapped and governed-access environments, data leaves the secure machine as a manifest plus archive instead of a push to S3. An enclave project pins the products it needs, pulls them outside the enclave, and packages them for transfer. Bundles are incremental—products already carried across the gap are skipped unless you ask for them again.

## Getting Started

`mintd init` scaffolds a standardized repository with Git and DVC already initialized and the storage remote configured:

```bash
mintd init data my-cleaned-survey --lang stata
```

Four project types—`data`, `project`, `code` and `enclave`—each with its own layout, and Python, R and Stata templates for the ones that carry analysis code. Producing and consuming a product are a handful of commands:

```bash
mintd data add data/final/survey.parquet   # track an output
mintd data push                            # push the bytes
mintd publish 0.1.0                        # cut a citable version

mintd data import my-cleaned-survey        # consume it, pinned
mintd check --upgrades                     # see if a dependency moved
```

## Technical Stack

- **Python CLI**, packaged as a standalone tool
- **DVC** with an S3 backend for data versioning
- **Pydantic** for metadata and contract validation
- **Jinja2** for language-specific scaffold templates
- **S3-compatible** object storage

## Resources

- [GitHub](https://github.com/health-care-affordability-lab/mintd)
