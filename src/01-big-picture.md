# Big picture

> Running example: *"Mis aastal see maja ehitati ja kas sellel on kasutusluba?"*
> (When was this house built, and does it have a use permit?)

```mermaid
flowchart LR
    Q([User query]):::user --> L[Local LLM<br/>constrained decoding]:::llm
    L --> I[Intents + slots<br/>business vocabulary]:::llm
    I --> M[Mapping layer]:::det
    M --> E[(EHR data)]:::data
    E --> A([Answer]):::user

    classDef user fill:#e8f0fe,stroke:#4a6fa5;
    classDef llm fill:#FFCCBC,stroke:#E64A19;
    classDef det fill:#e2f0d9,stroke:#548235;
    classDef data fill:#f2f2f2,stroke:#7f7f7f;
```

- The LLM only **detects** intents. It never answers from memory.
- Everything after the LLM is deterministic.

**Legend (all chapters):** orange = LLM, green = deterministic, grey = data, blue = user.
