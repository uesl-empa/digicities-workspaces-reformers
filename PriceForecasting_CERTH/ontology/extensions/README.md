# Ontology extensions

Project-specific extensions to the core Digicities ontology live here. Each file is a regular Turtle (`.ttl`) document declaring new classes/properties that inherit from `dici_onto:` parents.

## Example skeleton

```turtle
@prefix : <https://example.org/my_usecase/ontology#> .
@prefix dici_onto: <https://digicities.info/ontology#> .
@prefix owl: <http://www.w3.org/2002/07/owl#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .

:HighRiseBuilding a owl:Class ;
    rdfs:subClassOf dici_onto:EnergyConsumer ;
    rdfs:label "High-rise building"@en ;
    rdfs:comment "Residential or commercial building above 8 storeys."@en .
```

## Loading into your workspace

Drop the `.ttl` file in this folder, then either:

- In the Streamlit UI: **Ontology Manager → Extensions → Reload**.
- Or restart the platform container — extensions in this folder are auto-loaded on startup.

## Contributing back

If your extension is broadly useful (not site-specific), please consider opening a PR against the upstream [`digicities-ontology`](https://github.com/uesl-empa/digicities-ontology) repo so other usecases can benefit.
