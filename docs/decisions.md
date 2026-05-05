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

## Alternative technologies by layer

### Storage / object layer
Current direction:
- RustFS

Alternatives:
- MinIO
- SeaweedFS
- Ceph

Notes:
- MinIO is likely the simplest compatibility-first alternative
- SeaweedFS remains interesting for lightweight deployments
- Ceph is more operationally heavy but more established as a broad storage platform

### Table format / lakehouse layer
Current direction:
- Apache Iceberg

Alternatives:
- Delta Lake
- Apache Hudi

Notes:
- Iceberg is currently the best fit for an open, modular, engine-friendly architecture

### Catalog layer
Current direction:
- Nessie for now, while treating the catalog backend as an implementation detail

Alternatives:
- Iceberg REST Catalog
- Hive Metastore
- JDBC-backed catalog patterns
- Apache Polaris, depending on maturity and fit

Notes:
- REST Catalog is the most attractive alternative if the project wants a simpler, less Git-like catalog model
- Hive Metastore remains a compatibility-oriented fallback

### Query / compute layer
Current direction:
- Trino as the primary SQL/query engine

Alternatives:
- Spark
- DuckDB for local and single-node workflows
- Flink for streaming-heavy scenarios
- Dremio OSS

Notes:
- Spark remains highly relevant for heavier transformation workloads
- DuckDB is especially useful for local development and lightweight workflows

### Transformation layer
Current direction:
- dbt through Trino

Alternatives:
- SQLMesh
- Spark SQL jobs
- Flink SQL
- custom SQL/project-specific transformation frameworks

Notes:
- dbt currently fits the Git-first modeling approach best
- SQLMesh is worth watching if stronger workflow/state semantics become important

### Ingestion / ELT layer
Current direction:
- Meltano as a likely ingestion and integration runtime

Alternatives:
- Airbyte OSS
- dlt
- Singer taps/targets outside of Meltano
- Apache NiFi
- custom extract/load services

Notes:
- Meltano is attractive for a flexible plugin/runtime model
- Airbyte is the most obvious alternative for connector breadth

### CDC layer
Current direction:
- not yet implemented

Alternatives / likely future choices:
- Debezium
- Kafka Connect
- Airbyte CDC in selected cases

Notes:
- Debezium is the strongest default candidate for serious CDC support

### Orchestration layer
Current direction:
- Kestra

Alternatives:
- Dagster
- Airflow
- Prefect OSS
- Argo Workflows
- Temporal for more application-style workflow coordination

Notes:
- Kestra currently looks like a good fit for coordinating multiple tools cleanly
- Dagster remains a strong alternative if the stack leans harder into software-defined data assets

### Metadata / governance / lineage layer
Current direction:
- not yet chosen

Alternatives / likely candidates:
- OpenMetadata
- DataHub
- Marquez + OpenLineage
- Apache Atlas
- Amundsen

Notes:
- OpenMetadata and DataHub are the leading broad candidates right now
- Marquez/OpenLineage is attractive if lineage is the first goal rather than full metadata governance

### Data quality layer
Current direction:
- not yet chosen

Alternatives / likely candidates:
- dbt tests
- Soda Core
- Great Expectations
- Deequ for Spark-oriented validation
- Pandera for Python-centric validation

Notes:
- dbt tests plus Soda Core is currently a strong likely direction

### Serving / BI / semantic layer
Current direction:
- not yet chosen

Alternatives / likely candidates:
- Apache Superset
- Metabase
- Lightdash
- Cube
- dbt Semantic Layer / MetricFlow related patterns

Notes:
- Superset is the broadest OSS BI option
- Lightdash is attractive if the stack leans hard into dbt
- Cube is interesting when a dedicated semantic-serving layer becomes important

### Secrets / identity / policy layer
Current direction:
- not yet chosen

Alternatives / likely candidates:
- OpenBao
- Vault
- Keycloak
- OPA
- Apache Ranger in some ecosystems

Notes:
- OpenBao or Vault likely cover secrets
- Keycloak is a likely identity candidate
- OPA is a strong policy-layer option

### Observability layer
Current direction:
- not yet chosen

Alternatives / likely candidates:
- Prometheus
- Grafana
- Loki
- Tempo
- OpenTelemetry

Notes:
- a Prometheus/Grafana/Loki/Tempo stack is the most likely overall direction

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
