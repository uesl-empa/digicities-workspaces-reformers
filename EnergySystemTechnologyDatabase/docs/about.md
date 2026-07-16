# About — Energy System Technology Database

> ℹ️ **Source attribution.** This example data is from the [refuel.ch](https://refuel.ch) and SDSC MOTEL project for Energy System modelling. See that project for more information.

A catalogue of equipment specifications used to seed Digicities digital twins. Each row in the source workbook (`ingestion/input/MotelDB.xlsx`) describes one piece of energy-system technology (heat pumps, PV panels, storage, etc.) with its physical, cost, and lifecycle attributes.

## Why "MotelDB"?

Historical name — the workbook started life as the equipment database for a motel-energy modelling study and the filename stuck. The contents are general energy-system technology specs, not motels.

## What's here

| Path | What it is |
|---|---|
| `ingestion/input/MotelDB.xlsx` | The filled-in source workbook (~118 KB) following the platform's 7-row header convention. |
| `ingestion/output/MotelDB.ttl` | The converted RDF replica (~870 KB). This is the canonical, queryable form. |

## How to reproduce the conversion

With the platform stack running (`docker compose up -d`):

```bash
docker exec digicities-streamlit \
    python -m backend.replica_builder.cli \
    /app/data/usecases/EnergySystemTechnologyDatabase/ingestion/input/MotelDB.xlsx \
    -o /tmp/MotelDB_check.ttl
```

The regenerated `/tmp/MotelDB_check.ttl` should match the bundled `ingestion/output/MotelDB.ttl` modulo timestamps. (The Streamlit UI's *Replica Builder → Excel Import* tab does the same thing through the GUI.)

## How to use this as a starting point for your own workspace

Copy `MotelDB.xlsx` into your own workspace's `ingestion/input/`, rename it, then edit the rows. Run the same Replica Builder conversion. Commit the resulting `.ttl`.
