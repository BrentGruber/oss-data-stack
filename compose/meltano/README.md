# Meltano workspace

This directory is mounted into the `meltano` container as the project workspace.

## First steps

Once the stack is running, you can initialize Meltano here:

```bash
docker compose exec meltano meltano init .
```

Then add plugins as needed, for example taps, targets, dbt, or utility plugins.

## Why it starts empty

The first version of this stack is focused on wiring the core services together locally without prematurely locking in a specific Meltano project shape.
