# Step-by-step — populate your REFORMERS workspace

From a clean machine to a populated knowledge graph for your workpackage. About 15 minutes.

> **Before you start**: install [Docker Desktop](https://www.docker.com/products/docker-desktop/) and [Git](https://git-scm.com/downloads).

---

## One-time setup

### 1. Clone both repos

Pick a parent folder you like (e.g. `~/projects/`). Clone the platform and this repo side by side:

```bash
git clone https://github.com/uesl-empa/digicities-platform.git
mkdir -p usecases
git clone https://github.com/REFORMERS-EnergyValleys/REFORMERS_Ontology-Extensions-and-Knowledge-Graphs.git \
    usecases/REFORMERS_Ontology-Extensions-and-Knowledge-Graphs
```

You should now have:

```
projects/
├── digicities-platform/
└── usecases/
    └── REFORMERS_Ontology-Extensions-and-Knowledge-Graphs/   ← this repo
```

### 2. Point the platform at this repo

Open `digicities-platform/docker-compose.override.yml`. Replace the default `./demo_workspaces` mount with:

```yaml
services:
  streamlit:
    volumes:
      - .:/app
      - ../usecases/REFORMERS_Ontology-Extensions-and-Knowledge-Graphs:/app/data/usecases
    environment:
      USECASES_DIR: /app/data/usecases
```

### 3. Start the stack

```bash
cd digicities-platform
cp .env.example .env
docker compose up -d
```

First boot pulls the Fuseki image and builds the Streamlit image (about 3 minutes). Later starts are instant.

---

## Populate your workspace

### 4. Open the UI

Go to http://localhost:8501. You'll see the **Your Workspaces** page with all 10 REFORMERS workspaces. Click **📂 Open** on yours (e.g. *Wind Forecasting (AIT)*). The first open takes about 5 seconds. It loads the core ontology and computes inference.

### 5. Find your starting workbook

A blank copy of the Excel template is already in your workspace:

```
WindForecasting_AIT/ingestion/input/data_ingestion_template.xlsx
```

Rename it to match your workspace (e.g. `WindForecasting_AIT.xlsx`). The CLI uses the filename for the project URI, so a meaningful name matters.

> The latest blank template is at the platform repo if you need to refresh it: [`data_ingestion_template.xlsx`](https://github.com/uesl-empa/digicities-platform/raw/main/data/ingestion_template/data_ingestion_template.xlsx).

### 6. Fill in the workbook

Open the file in Excel and replace the demo rows with your data. Reference material:

- **Template README**: [the header convention and the 15 attribute types](https://github.com/uesl-empa/digicities-platform/blob/main/data/ingestion_template/README.md).
- **Worked example**: [`EnergySystemTechnologyDatabase/ingestion/input/MotelDB.xlsx`](EnergySystemTechnologyDatabase/ingestion/input/MotelDB.xlsx) is a real, finished workbook. Open it next to yours.

Save the workbook.

### 7. Convert to TTL

One command. The platform container already mounts this repo:

```bash
docker exec digicities-streamlit \
    python -m backend.replica_builder.cli \
    /app/data/usecases/WindForecasting_AIT/ingestion/input/WindForecasting_AIT.xlsx
```

Replace `WindForecasting_AIT` with your workspace name in both places. The TTL lands at `<WorkspaceFolder>/ingestion/output/<workbook-stem>.ttl`.

### 8. Reload the workspace

Back at http://localhost:8501, click **📂 Open** on your workspace again. The platform re-loads the new TTL into Fuseki and computes inference.

### 9. Explore

Inside the workspace:

- **Digital Replica Explorer**: every component type, with instance counts. Click a row to see instances and attributes.
- **Query Manager**: run saved SPARQL queries against your graph, or write your own.

### 10. Commit and push

The commands below are for **WindForecasting_AIT**. Substitute your own folder and filename:

```bash
cd usecases/REFORMERS_Ontology-Extensions-and-Knowledge-Graphs
git add WindForecasting_AIT/ingestion/output/WindForecasting_AIT.ttl
git commit -m "WindForecasting_AIT: initial replica"
git push
```

The `.ttl` is the canonical record. The `.xlsx` is gitignored by default. Add it explicitly if you want the raw input tracked too.

---

## When you get stuck

Please open an issue on this repository for any help and assistance.

---

## Related reading

- [`README.md`](README.md): repo overview, phase plan, workspace conventions.
- Template guide: [`digicities-platform/data/ingestion_template/README.md`](https://github.com/uesl-empa/digicities-platform/blob/main/data/ingestion_template/README.md).
- Workspace layout: [`digicities-platform/docs/WORKSPACE_LAYOUT.md`](https://github.com/uesl-empa/digicities-platform/blob/main/docs/WORKSPACE_LAYOUT.md).
- First-time platform tour: [`digicities-platform/docs/FIRST_TIME_USER.md`](https://github.com/uesl-empa/digicities-platform/blob/main/docs/FIRST_TIME_USER.md).
