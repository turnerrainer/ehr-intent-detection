# Rollout

```mermaid
flowchart LR
    subgraph P1[Phase 1 · Foundation]
        direction TB
        A1[Extract EHR schema]:::det
        A2[Business model + intent catalog]:::det
        A3[Mapping layer]:::det
        A1 --> A2 --> A3
    end

    subgraph P2[Phase 2 · Model]
        direction TB
        B1[Eval set, hand-written]:::det
        B2[Prompt-only baseline]:::llm
        B3[Synthetic data + fine-tune]:::llm
        B1 --> B2 --> B3
    end

    subgraph P3[Phase 3 · Launch]
        direction TB
        C1[Pilot]:::user
        C2[Feedback loop]:::user
        C1 --> C2
    end

    A1 --> B1
    A2 --> B2
    A3 --> B3
    B3 --> C1

    classDef llm fill:#FFCCBC,stroke:#E64A19;
    classDef det fill:#e2f0d9,stroke:#548235;
    classDef user fill:#e8f0fe,stroke:#4a6fa5;
```

- **Foundation** has to land first: schema → catalog → mapping. Without it there is nothing to validate against or map to.
- **Model** work starts in parallel the moment the schema exists: eval set first (so quality has a ruler), then prompt baseline, then fine-tuning once synthetic data can be generated against the catalog.
- **Launch** is gated on a model that beats the eval threshold; feedback loop runs continuously after pilot opens.

> Ordering is a commitment; pace is not. Each phase ends when its exit criteria are met, not when a clock runs out.
