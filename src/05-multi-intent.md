# Multi-intent & slots

```mermaid
flowchart LR
    Q["Mis aastal see maja ehitati<br/>ja kas sellel on kasutusluba?"]:::user
    Q --> I1[ehr.building.construction_year]:::llm
    Q --> I2[ehr.permits.use_permit_status]:::llm
    S[/"slot: building<br/>(from context/address)"/]:::llm
    S -.-> I1
    S -.-> I2
    I1 --> V{Slots valid?}:::det
    I2 --> V
    V -->|yes| X[Execute both]:::det
    V -->|no| C[Ask clarification]:::det

    classDef user fill:#e8f0fe,stroke:#4a6fa5;
    classDef llm fill:#FFCCBC,stroke:#E64A19;
    classDef det fill:#e2f0d9,stroke:#548235;
```

Output shape:

```json
{"intents":[
  {"id":"ehr.building.construction_year","slots":{"building":"ref:ctx"}},
  {"id":"ehr.permits.use_permit_status","slots":{"building":"ref:ctx"}}
]}
```

| Slot | Formats |
|---|---|
| `ehr_code` | numeric |
| `cadastral_number` | `78401:101:0010` |
| `address` | free text → ADS lookup |
