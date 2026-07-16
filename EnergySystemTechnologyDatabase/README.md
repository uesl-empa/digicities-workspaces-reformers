# Energy System Technology Database

> A worked-example workspace inside REFORMERS. Shows the full ingestion pipeline: a real, filled-in Excel workbook in [`ingestion/input/MotelDB.xlsx`](ingestion/input/MotelDB.xlsx) and its converted RDF replica in [`ingestion/output/MotelDB.ttl`](ingestion/output/MotelDB.ttl).

> ℹ️ **Source attribution.** This data is from the [refuel.ch](https://refuel.ch) and SDSC MOTEL project for energy-system modelling. See that project for more information.

A catalogue of energy-system equipment specifications. Despite the historical `MotelDB` filename, the contents are general technology specs (heat pumps, PV panels, storage, etc.), not motels.

## What's in here

| Path | Size | What it is |
|---|---|---|
| [`ingestion/input/MotelDB.xlsx`](ingestion/input/MotelDB.xlsx) | ~118 KB | Filled-in workbook. Your starting point for copy/adapt. |
| [`ingestion/output/MotelDB.ttl`](ingestion/output/MotelDB.ttl) | ~870 KB | The converted RDF replica. Canonical, queryable form. |
| [`docs/about.md`](docs/about.md) | — | Provenance, naming history, how to reproduce. |
| All other folders | empty | Canonical workspace skeleton. See [`docs/WORKSPACE_LAYOUT.md`](https://github.com/uesl-empa/digicities-platform/blob/main/docs/WORKSPACE_LAYOUT.md). |

## Use this as a model for your own workspace

1. Open this workspace in the Digicities UI. Inspect the populated graph in the Component Explorer and Query Manager.
2. Open [`ingestion/input/MotelDB.xlsx`](ingestion/input/MotelDB.xlsx) in Excel to see the shape of a finished workbook.
3. Copy it into your own workspace's `ingestion/input/` as a starting point. Or grab the blank template: [`data_ingestion_template.xlsx`](https://github.com/uesl-empa/digicities-platform/raw/main/data/ingestion_template/data_ingestion_template.xlsx).
4. Edit the rows with your data. Save.
5. Re-convert with one command. The platform container already mounts this repo:

   ```bash
   docker exec digicities-streamlit \
       python -m backend.replica_builder.cli \
       /app/data/usecases/<YourWorkspace>/ingestion/input/<your_workbook>.xlsx
   ```

   The new `.ttl` lands in your workspace's `ingestion/output/`. (The Streamlit UI's *Replica Builder → Excel Import* tab does the same thing through the GUI.)

See the [template README](https://github.com/uesl-empa/digicities-platform/blob/main/data/ingestion_template/README.md) for the header convention and supported attribute types.
