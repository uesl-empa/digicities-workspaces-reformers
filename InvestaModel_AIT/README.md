# Investa Model (AIT)

> Digicities workspace for **InvestaModel_AIT**, part of the REFORMERS knowledge-graph corpus. The platform finds it automatically when this repo's root is mounted as `USECASES_DIR`.

## What this workspace is for

Investment-optimisation modelling for energy-system assets.

Replace this README with a description of your specific workpackage: scope, data sources, time horizon.

To change how this workspace appears on the Digicities landing page (name, description, tags, thumbnail), edit [`workspace_meta/metadata.json`](workspace_meta/metadata.json) and replace [`workspace_meta/image.png`](workspace_meta/image.png) with your own thumbnail. Full convention: [`../README.md`](../README.md#customise-your-workspace).

## How to populate it

1. **Find the template.** A blank copy is at [`ingestion/input/data_ingestion_template.xlsx`](ingestion/input/data_ingestion_template.xlsx). Rename it to `InvestaModel_AIT.xlsx` and fill in your components and attributes. Reference: [template README](https://github.com/uesl-empa/digicities-platform/blob/main/data/ingestion_template/README.md).
2. **Convert to TTL.** One command. The platform container already mounts this repo:

   ```bash
   docker exec digicities-streamlit \
       python -m backend.replica_builder.cli \
       /app/data/usecases/InvestaModel_AIT/ingestion/input/InvestaModel_AIT.xlsx
   ```

   The TTL lands in [`ingestion/output/`](ingestion/output/) as `InvestaModel_AIT.ttl`. (You can also do this from the Streamlit UI: *Replica Builder → Excel Import*.)
3. **Commit the `.ttl`.** Optionally also the `.xlsx`. The TTL is the canonical record of your workpackage's knowledge graph.

That's it. See [`../EnergySystemTechnologyDatabase/`](../EnergySystemTechnologyDatabase/) for a finished example.
