# Building Flexibility (Empa)

> Digicities workspace for **BuildingFlexibility_EMPA**, part of the REFORMERS knowledge-graph corpus. The platform finds it automatically when this repo's root is mounted as `USECASES_DIR`.

## What this workspace is for

Demand-side flexibility characterisation for buildings.

Replace this README with a description of your specific workpackage: scope, data sources, time horizon.

To change how this workspace appears on the Digicities landing page (name, description, tags, thumbnail), edit [`workspace_meta/metadata.json`](workspace_meta/metadata.json) and replace [`workspace_meta/image.png`](workspace_meta/image.png) with your own thumbnail. Full convention: [`../README.md`](../README.md#customise-your-workspace).

## How to populate it

1. **Find the template.** A blank copy is at [`ingestion/input/data_ingestion_template.xlsx`](ingestion/input/data_ingestion_template.xlsx). Rename it to `BuildingFlexibility_EMPA.xlsx` and fill in your components and attributes. Reference: [template README](https://github.com/uesl-empa/digicities-platform/blob/main/data/ingestion_template/README.md).
2. **Convert to TTL.** One command. The platform container already mounts this repo:

   ```bash
   docker exec digicities-streamlit \
       python -m backend.replica_builder.cli \
       /app/data/usecases/BuildingFlexibility_EMPA/ingestion/input/BuildingFlexibility_EMPA.xlsx
   ```

   The TTL lands in [`ingestion/output/`](ingestion/output/) as `BuildingFlexibility_EMPA.ttl`. (You can also do this from the Streamlit UI: *Replica Builder → Excel Import*.)
3. **Commit the `.ttl`.** Optionally also the `.xlsx`. The TTL is the canonical record of your workpackage's knowledge graph.

That's it. See [`../EnergySystemTechnologyDatabase/`](../EnergySystemTechnologyDatabase/) for a finished example.

## Connect this workspace to a flexibility optimizer service

This workspace submits scenarios to an external flexibility optimizer service.
A typical setup is a Redis-stream optimizer with a small `flexibility-adapter`
(FastAPI, port 8090) that bridges HTTP → Redis, so Digicities just POSTs the
converted scenario.

Concepts and the per-service template are in the platform's
[`docs/INTEGRATING_A_SERVICE.md`](https://github.com/uesl-empa/digicities-platform/blob/main/docs/INTEGRATING_A_SERVICE.md).
The exact steps:

1. **Start your optimizer service** — bring up the optimizer and its HTTP adapter
   (plus Redis if you use the stream transport). The adapter listens on `:8090`,
   `POST /run`.

2. **Register the endpoint in Digicities** — open this workspace →
   **API Data Submission → API Configuration** → service **FlexibilityOptimizer**,
   transport **http**, URL `http://host.docker.internal:8090/run`
   (use `host.docker.internal`, not `localhost` — the Streamlit app runs in a
   container; the optimizer stack is a separate compose project).

3. **Convert + submit** — **Upload & Convert** with the **FlexibilityOptimizer**
   template ([`services/flexibility_optimizer.yaml`](services/flexibility_optimizer.yaml)),
   pick [`scenarios/flexibility_demo_scenario.ttl`](scenarios/flexibility_demo_scenario.ttl)
   (or a variant), **Convert**, then **API Submission → Submit**. You get an
   optimal result back (cost, grid energy, temperatures per building).

**Weather.** The Location's `WeatherEPW` carries
`dici_onto:hasLiveTimeSeriesReference "weather.forecasts.openmeteo.Alkmaar"`, so the
optimizer plans the coming 24 hours from that forecast, temperature and irradiance by
their timestamps, **if the data crawler is publishing it**; otherwise it plans a
representative day from a static file selected by `weather_location` (e.g. `fribourg`),
and the result says why. The building here is a demonstration: its weather is Alkmaar's,
but its data is not from Alkmaar.

**Redis transport (alternative).** Instead of the HTTP adapter you can register
transport **redis** (streams `flexibility.requests` / `flexibility.results`); the
Streamlit image needs the `redis` dependency for that path.
