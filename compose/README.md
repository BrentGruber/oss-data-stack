# compose

Local Docker Compose stack for the OSS data stack experiment.

## Included services

- `rustfs` - S3-compatible object storage
- `nessie` - catalog service
- `trino` - query engine
- `postgres` - backing store for Kestra
- `kestra` - orchestration service
- `meltano` - ELT/integration workspace container

## Ports

- RustFS S3 API: `9000`
- RustFS console: `9001`
- Nessie API/UI: `19120`
- Trino: `8080`
- Kestra: `8081`
- Postgres for Kestra: `5433`

## Quick start

1. Copy env file:

```bash
cp .env.example .env
```

2. Start the stack:

```bash
docker compose up -d
```

3. Check services:

```bash
docker compose ps
```

## Notes

This is an initial local stack, not a production deployment.

Notable simplifications in this first version:
- Nessie uses in-memory storage
- Kestra uses local storage plus Postgres for state
- Meltano starts as a workspace container and is not yet preconfigured with plugins
- RustFS bucket bootstrap is not yet automated

## Suggested next steps

- add automatic bucket bootstrap for `lakehouse`, `raw-landing`, `artifacts`, `metadata`, `temp`, `backups`
- initialize a sample Meltano project
- add example Kestra flows
- add dbt or SQL transformation examples
- switch Nessie from in-memory to persistent backing if needed
