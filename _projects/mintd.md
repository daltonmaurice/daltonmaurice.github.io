---
layout: page
title: "mintd: Effortless Research Project Management"
description: CLI tool for creating version-controlled research projects with cloud storage
importance: 2
category: data infrastructure
---

Every research project starts the same way: create a folder, maybe initialize git, figure out where data goes, write the same boilerplate setup code. **mintd** automates all of that.

It's a CLI tool that scaffolds standardized research repositories in seconds—Git initialized, data directories tracked with DVC, cloud storage configured. One command, done.

<div class="repositories d-flex flex-wrap gap-2 flex-md-row flex-column justify-content-between align-items-center">
{% include repository/repo.liquid repository="health-care-affordability-lab/mintd" %}
</div>

## Key Features

- **Rapid Setup**: `mintd create data --name my_project --lang python` and you're ready to go
- **Multi-Language**: Python, R, and Stata with language-specific templates
- **Version Control**: Automatic Git and DVC initialization
- **Cloud Storage**: S3-compatible storage (AWS, Wasabi, MinIO) with secure credentials
- **Registry Integration**: Optional project registration for discoverability

## Technical Stack

- **Python CLI** built with Click
- **DVC** for data versioning
- **Jinja2** for language-specific templates
- **S3-compatible** cloud storage

## Resources

- [Documentation](https://health-care-affordability-lab.github.io/mintd/)
- [GitHub](https://github.com/health-care-affordability-lab/mintd)
