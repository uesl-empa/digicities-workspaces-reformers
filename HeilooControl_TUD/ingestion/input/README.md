# Ingestion input

Drop your filled-in Digicities Excel workbooks here.

Get the template from the platform repo: [`data_ingestion_template.xlsx`](https://github.com/uesl-empa/digicities-platform/blob/main/data/ingestion_template/data_ingestion_template.xlsx). Copy it under a project-specific name (e.g. `my_district_v1.xlsx`), fill in your components and attributes, save.

## Workflow

1. Drop your `<name>.xlsx` here.
2. Open the Streamlit UI: http://localhost:8501 → Replica Builder → Excel Import.
3. Pick your workbook, accept the auto-derived project URI (or set your own), click *Convert to TTL*.
4. The output appears in [`../output/`](../output/) as `<name>.ttl`.
5. Commit the `.ttl`. By default this folder gitignores `*.xlsx` — toggle that in the repo-root `.gitignore` if you want the workbooks tracked too.

The `example.xlsx` file (if present) is the only workbook tracked by default — keep it as the canonical worked example for your usecase.
