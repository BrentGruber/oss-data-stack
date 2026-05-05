# Decisions

## Current architectural decisions

### Environment isolation model
Decision:
- use a fully separate stack instance per environment

Implications:
- do not model `dev/test/prod` primarily through catalogs, buckets, or schema prefixes inside a shared deployment
- prefer separate full environments for local, ephemeral test, dev, and prod
- optimize internal resource naming for platform organization rather than environment separation

Why:
- better reproducibility
- cleaner end-to-end testing
- simpler mental model
- easier long-term automation and lifecycle management

### Lakehouse bucket strategy
Decision:
- use one primary managed lakehouse bucket rather than separate bronze/silver/gold buckets

Recommended bucket list:
- `lakehouse`
- `raw-landing`
- `artifacts`
- `metadata`
- `temp`
- `backups`

Why:
- simpler stack configuration
- cleaner local and ephemeral environments
- better separation between managed tables and raw landing data
- bronze/silver/gold can be expressed logically rather than physically

### Medallion model representation
Decision:
- keep medallion concepts, but represent them logically rather than as separate object storage buckets

Why:
- familiarity matters
- bronze/silver/gold remains a useful mental model
- schemas and naming conventions can express layer boundaries without requiring separate warehouse buckets

### Catalog strategy
Current direction:
- start with one main catalog per stack instance

Guidance:
- do not split catalogs by environment
- do not split catalogs by medallion layer by default
- only introduce more catalogs when there is a real boundary such as sensitivity, governance, or strong isolation requirements

### Catalog backend stance
Current position:
- Nessie is acceptable as a current backend, but it should be treated as an implementation detail rather than the core public abstraction

Why:
- the long-term platform may abstract catalogs more generally
- the project should avoid coupling too tightly to Nessie-specific concepts such as branch/tag/ref semantics
- Git should remain the primary home for infrastructure and transformation intent

### Query engine strategy
Decision:
- Trino is the current query/compute entry point

Why:
- straightforward SQL access
- good fit for local validation and exploration
- compatible with the current storage and catalog direction

### Transformation strategy
Current direction:
- dbt is expected to be the most natural transformation layer

Why:
- aligns well with Git-based workflows
- fits the medallion-style modeling approach
- works naturally with Trino through dbt-trino

### Ingestion strategy
Current direction:
- Meltano is a likely ingestion/integration runtime

Why:
- flexible open source integration model
- good fit for plugin-driven data movement
- can be treated as an execution backend rather than the main control plane

### Orchestration strategy
Current direction:
- Kestra is the primary orchestration candidate

Why:
- good fit for coordinating multiple tools
- should own workflow scheduling, triggering, and coordination
- should not replace transformation graph ownership inside dbt or integration packaging inside Meltano

## Key abstraction guidance

The long-term platform model should prefer concepts such as:
- tenant
- workspace
- catalog
- warehouse
- connection
- dataset
- pipeline
- workflow
- schedule
- policy
- data product

It should avoid exposing backend-specific concepts too directly unless truly necessary.
