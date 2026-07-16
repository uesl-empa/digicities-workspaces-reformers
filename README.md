# REFORMERS — Ontology Extensions & Knowledge Graphs

Knowledge-graph workspaces for the REFORMERS project. Each subfolder is one partner's workspace. Each partner fills in a shared Excel template with their data, then converts it to a TTL file. That TTL is the workspace's knowledge graph.

> 🚀 **First time here?** Read [`STEP_BY_STEP.md`](STEP_BY_STEP.md). It walks you through everything in about 15 minutes.

## Project phases

Phase 1 is the priority. Get the data in first.

| Phase | What happens | Status |
|---|---|---|
| **1. Knowledge graphs & ontology extensions** | Each partner fills the Excel template and converts it to TTL. New classes go in the workspace's `ontology/extensions/`. | **Current priority** |
| 2. Services & scenarios | Register service requirements. Build what-if scenarios. | After Phase 1 |
| 3. RDP integration | Integrate workspaces and platform into the REFORMERS RDP architecture. | After Phase 2 |

## Workspaces

| Folder | Lead |
|---|---|
| [`WindForecasting_AIT`](WindForecasting_AIT/) | AIT |
| [`InvestaModel_AIT`](InvestaModel_AIT/) | AIT |
| [`InvestaScenarios_AIT`](InvestaScenarios_AIT/) | AIT |
| [`HeilooControl_TUD`](HeilooControl_TUD/) | TU Delft |
| [`CongestionPolicyTool_TUD`](CongestionPolicyTool_TUD/) | TU Delft |
| [`SolarForecasting_CIRCE`](SolarForecasting_CIRCE/) | CIRCE |
| [`EVForecasting_VUB`](EVForecasting_VUB/) | VUB |
| [`PriceForecasting_CERTH`](PriceForecasting_CERTH/) | CERTH |
| [`BuildingFlexibility_EMPA`](BuildingFlexibility_EMPA/) | Empa |
| [`EnergySystemTechnologyDatabase`](EnergySystemTechnologyDatabase/) | Worked example |

## The Excel template

Every workspace ships with a blank copy of the template at:

```
<WorkspaceFolder>/ingestion/input/data_ingestion_template.xlsx
```

Rename it to match your workspace (e.g. `WindForecasting_AIT.xlsx`). The converter uses the filename for the project URI.

The latest blank template is always at the platform repo: [`data_ingestion_template.xlsx`](https://github.com/uesl-empa/digicities-platform/raw/main/data/ingestion_template/data_ingestion_template.xlsx).

How to fill it in: [template README](https://github.com/uesl-empa/digicities-platform/blob/main/data/ingestion_template/README.md). It covers the header rows, the 15 attribute types, and how to cite your sources.

## How to populate your workspace

Full walkthrough: [`STEP_BY_STEP.md`](STEP_BY_STEP.md). Short version:

1. Open your workspace's `ingestion/input/data_ingestion_template.xlsx`. Rename it.
2. Fill in the rows.
3. Run this:

   ```bash
   docker exec digicities-streamlit \
       python -m backend.replica_builder.cli \
       /app/data/usecases/<WorkspaceFolder>/ingestion/input/<your_workbook>.xlsx
   ```

4. The TTL lands at `<WorkspaceFolder>/ingestion/output/<your_workbook>.ttl`. Commit it.

That's it for Phase 1. Scenarios, services, and custom queries come later.

### CLI options

```text
python -m backend.replica_builder.cli <xlsx-path> [--output PATH] [--project-uri URI] [--uri-mode MODE]
```

- `<xlsx-path>`: required. Container path to the workbook.
- `--output / -o`: override output path. Default: `<workspace>/ingestion/output/<stem>.ttl`.
- `--project-uri`: override the URI. Default: `https://digicities.info/proj/<stem>`.
- `--uri-mode`: `default` (most workbooks), `full-uri-in-cell`, or `complete-project-uri`. See the template README.

## Look at the example first

[`EnergySystemTechnologyDatabase/`](EnergySystemTechnologyDatabase/) has a real filled-in workbook (`MotelDB.xlsx`) and its converted TTL (`MotelDB.ttl`). Open it in the UI to see what a populated workspace looks like before doing your own.

## Plug this repo into Digicities

The platform discovers workspaces by scanning a parent folder. Mount this repo at that folder:

```yaml
# digicities-platform/docker-compose.override.yml
services:
  streamlit:
    volumes:
      - ../REFORMERS_Ontology-Extensions-and-Knowledge-Graphs:/app/data/usecases
    environment:
      USECASES_DIR: /app/data/usecases
```

Restart Streamlit, refresh http://localhost:8501. All workspaces in this repo show up.

## Customise your workspace

Two files control how your workspace appears on the landing page.

**`workspace_meta/metadata.json`** sets the name, description, and tags.

```json
{
  "id": "WindForecasting_AIT",
  "name": "Wind Forecasting (AIT)",
  "description": "Short-term wind power forecasting models and supporting data.",
  "created": "2026-05-28",
  "tags": ["forecasting", "wind", "AIT"]
}
```

Edit `name`, `description`, and `tags` freely. Don't change `id`. The platform uses it internally.

**`workspace_meta/image.png`** is the thumbnail. Replace it with your own (`.png`, `.jpg`, or `.jpeg`). About 400x300 px works well.

Changes appear on the next workspace load. The platform caches for 5 minutes, so hard-refresh (Ctrl+F5) or restart the streamlit container if you want them sooner.

## Conventions

- One topic, one folder. Don't mix data from two partners.
- Folder names: `<Topic>_<Partner>`. PascalCase topic, partner abbreviation.
- Excel workbooks in `ingestion/input/` are gitignored by default. The example workbook in `EnergySystemTechnologyDatabase` is tracked as an exception. To track your own, add it explicitly.

## Citing the platform and ontology

This repo is just workspace data. It doesn't carry its own citation. If you publish work using the underlying tools, cite these two:

- **Digicities platform** (Apache 2.0): see [its README](https://github.com/uesl-empa/digicities-platform#how-to-cite) for the BibTeX.
- **Digicities ontology** (CC BY 4.0): see [its README](https://github.com/uesl-empa/digicities-ontology#how-to-cite) for the BibTeX.

## Need help?

Open an issue on this repository.

## Related

- Digicities platform: [`uesl-empa/digicities-platform`](https://github.com/uesl-empa/digicities-platform)
- Core ontology: [`uesl-empa/digicities-ontology`](https://github.com/uesl-empa/digicities-ontology)
- Excel template: [`digicities-platform/data/ingestion_template/`](https://github.com/uesl-empa/digicities-platform/tree/main/data/ingestion_template)

## Funding & acknowledgements

Digicities was funded through the SFOE P+D program under the ERA-Net Smart Energy Systems joint initiative *Digital Transformation for the Energy Transition*, grant agreement No 88397.
