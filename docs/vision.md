# Vision

## Goal

Build a reproducible, open source data stack that can:
- run locally on a developer machine
- run ephemerally for end-to-end testing
- run in Kubernetes for longer-lived environments
- evolve toward a higher-level abstraction and control plane over time

The project is not just about assembling popular tools. The goal is to create a coherent stack that works together cleanly and can eventually support automation, self-service patterns, and team-level tenancy.

## Core principles

### Reproducibility first
Each environment should be reproducible as a full stack instance.

That means:
- local development should be possible with Docker Compose
- test environments should be disposable and easy to recreate
- longer-lived environments should use the same basic architecture and conventions

### Git in front of the tooling
Version control should sit primarily in Git:
- infrastructure and deployment definitions
- transformation logic
- orchestration definitions
- naming conventions
- policies and configuration

This project should avoid pushing too much Git-like workflow meaning into the runtime data tools unless there is a clear reason to do so.

### Keep backend choices abstractable
The long-term direction may include a higher-level abstraction layer or operator-style control plane.

Because of that, backend-specific concepts should be treated as implementation details where possible.

Examples:
- catalog backend choice should not dominate the public abstraction model
- storage system choice should remain replaceable
- orchestration and ingestion tools should map into broader platform concepts

### Full stack, not just lakehouse plumbing
The end state is broader than object storage, catalog, and query.

The stack should eventually cover:
- storage
- catalog
- compute/query
- ingestion
- transformation
- orchestration
- data quality
- metadata/governance
- observability
- access/policy
- lifecycle/maintenance

## Value of the vision

If this works well, it provides:
- a fully open source end-to-end data platform foundation
- reproducible local and ephemeral environments for real testing
- a path to self-service data platform patterns
- a path to tenant/workspace abstractions later
- freedom to evolve tooling without rewriting the entire platform model

## Why this matters

Many stacks are either:
- too ad hoc to reproduce cleanly
- too tied to one vendor or commercial product
- too fragmented to feel like one coherent platform

This project aims to produce something more unified:
- open
- modular
- reproducible
- developer-friendly
- automation-friendly
