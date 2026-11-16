# Mapping to data

```mermaid
flowchart LR
    subgraph Intents
        A[building.construction_year]:::llm
        B[permits.use_permit_status]:::llm
        C[building.age_over_n_years]:::llm
    end
    subgraph Mapping["Mapping (versioned, tested)"]
        A1[direct field]:::det
        B1[lookup + filter]:::det
        C1[computed]:::det
    end
    subgraph EHR
        T1[(building table)]:::data
        T2[(permit table)]:::data
    end
    A --> A1 --> T1
    B --> B1 --> T2
    C --> C1 --> T1

    classDef llm fill:#FFCCBC,stroke:#E64A19;
    classDef det fill:#e2f0d9,stroke:#548235;
    classDef data fill:#f2f2f2,stroke:#7f7f7f;
```

| Kind | Example | Mapping form |
|---|---|---|
| Direct | construction year | field |
| Lookup | use permit status | filtered query |
| Computed | older than 50 years? | function |

- Mapping is code: versioned, reviewed, tested against known buildings.
- Table and API names here are placeholders until the EHR schema is extracted.
