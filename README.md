# oss-data-stack

An experimental open source data stack focused on reproducibility, local end-to-end testing, and eventual Kubernetes/operator-style deployment.

## Initial focus

This repository starts with a local Docker Compose stack for exploring a modular lakehouse-oriented platform.

Current tools in scope:
- RustFS for S3-compatible object storage
- Nessie for cataloging
- Trino for query/compute
- Meltano for ELT-style data integration workflows
- Kestra for orchestration

## Layout

- `compose/` - local Docker Compose stack and related configuration

## Goals

- make the stack easy to run locally on a developer machine
- keep the architecture close to the longer-term cluster design
- support end-to-end testing of storage, catalog, compute, integration, and orchestration
- provide a foundation for future abstraction and automation work
