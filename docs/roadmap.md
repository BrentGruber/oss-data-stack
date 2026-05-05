# Roadmap

## Short-term goals

### 1. Stabilize the local stack
- make Docker Compose startup reliable
- persist the right services where useful
- automate common local bootstrap tasks
- add bucket bootstrap for:
  - `lakehouse`
  - `raw-landing`
  - `artifacts`
  - `metadata`
  - `temp`
  - `backups`

### 2. Prove one full vertical slice
- land a small sample dataset
- create bronze-layer tables
- transform to silver
- transform to gold
- query the result through Trino

### 3. Add transformation tooling
- wire in dbt with Trino
- establish naming conventions for schemas and tables
- define a minimal project structure for transformations

### 4. Add orchestration examples
- create sample Kestra workflows
- orchestrate ingestion plus transformation
- define the basic execution model for the stack

### 5. Add ingestion examples
- initialize a Meltano project
- demonstrate at least one simple ingestion path
- document how ingestion lands data in the stack

## Medium-term goals

### 6. Add quality and validation
- dbt tests
- possibly Soda Core or similar checks
- promotion and trust rules for datasets

### 7. Add metadata and governance direction
- evaluate OpenMetadata, DataHub, or lighter lineage patterns
- document ownership, lineage, and discovery strategy

### 8. Add observability
- logs
- metrics
- workflow visibility
- query visibility
- storage and maintenance visibility

### 9. Add housekeeping and lifecycle tasks
- snapshot cleanup
- orphan file cleanup
- retention patterns
- maintenance workflows

## Long-term goals

### 10. Abstract the stack
- identify portable platform concepts
- define a control-plane model above the individual tools
- avoid locking the public model too tightly to one catalog or one orchestrator

### 11. Move toward self-service patterns
- team or tenant abstractions
- storage and catalog provisioning
- policy and access automation
- reproducible environment lifecycle management

### 12. Support multiple backends over time
- catalog backend flexibility
- compute backend flexibility
- storage backend flexibility
- orchestration backend flexibility where practical

## What success looks like

A successful early version of this project should allow a developer to:
- clone the repo
- start the stack locally
- bootstrap storage and core services
- ingest a sample dataset
- transform it through medallion-style layers
- query the result
- inspect workflows and artifacts

A successful longer-term version should allow teams to consume the platform through a higher-level abstraction rather than wiring each underlying tool by hand.
