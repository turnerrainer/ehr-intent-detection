# Training pipeline

```mermaid
flowchart LR
    S[EHR schema]:::data --> BM[Business model]:::det
    G[Glossary<br/>et / en / ru]:::det --> GEN
    BM --> GEN[Synthetic generation<br/>strong LLM]:::llm
    GEN --> F[Filter + dedupe<br/>+ 5% human check]:::det
    F --> FT[QLoRA fine-tune<br/>7–9B]:::llm
    FT --> EV{Eval}:::det
    H[Hand-written eval set<br/>300–500, never trained on]:::user --> EV
    EV -->|pass| D([Deploy]):::user
    EV -->|fail| GEN

    classDef user fill:#e8f0fe,stroke:#4a6fa5;
    classDef llm fill:#FFCCBC,stroke:#E64A19;
    classDef det fill:#e2f0d9,stroke:#548235;
    classDef data fill:#f2f2f2,stroke:#7f7f7f;
```

- Variation axes: persona, register, language, typos, slot style.
- Multi-intent examples are composed programmatically, then rephrased naturally.
- Include hard negatives and `out_of_scope`.
