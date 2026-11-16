# Component view

Who owns what across one full request. Lanes are components; the flow runs top-to-bottom through them.

```mermaid
swimlane-beta TB
  subgraph gui [GUI]
    q([User query])
    clar([Clarification])
    ans([Answer])
  end
  subgraph llm [Local LLM]
    decode[Constrained decoding<br/>intent ∈ enum]
    out[Intents + slots<br/>+ confidence]
  end
  subgraph val [Validator]
    scope{Scope + confidence OK?}
    slot{Slot exists in DB?}
  end
  subgraph map [Mapping]
    trans[intent → SQL / X-tee call]
    shape[Shape answer]
  end
  subgraph db [Relational DB / EHR]
    lookup[(Slot lookup)]
    query[(Business query)]
  end
  subgraph log [Audit log]
    rec[(intent IDs, confidence,<br/>slots, outcome)]
  end

  q --> decode --> out --> scope
  scope -->|no| clar
  scope -->|yes| slot --> lookup
  lookup -->|unknown| clar
  lookup -->|known| trans --> query --> shape --> ans

  out -.-> rec
  scope -.-> rec
  slot -.-> rec
  shape -.-> rec

  classDef userC fill:#E8F0FE,stroke:#4A6FA5,stroke-width:2px,color:#0D253F;
  classDef llmC  fill:#FFCCBC,stroke:#E64A19,stroke-width:2px,color:#4A1E1C;
  classDef detC  fill:#E2F0D9,stroke:#548235,stroke-width:2px,color:#1F3B14;
  classDef dataC fill:#F2F2F2,stroke:#7F7F7F,stroke-width:2px,color:#2B2B2B;
  class q,clar,ans userC;
  class decode,out llmC;
  class scope,slot,trans,shape detC;
  class lookup,query,rec dataC;
```

## Who does what, and why it lives there

| Lane | Responsibility | Why here, not elsewhere |
|---|---|---|
| **GUI** | Capture the query, show the answer or a clarification prompt. | Keep it dumb. No business logic, no schema knowledge — it survives rebranding, embeds, mobile. |
| **Local LLM** | Turn free text into `{intent, slots, confidence}`. Nothing else. | On-prem for personal data. Constrained decoding makes the intent space a closed set; answers never come from the model. |
| **Validator** | Reject out-of-scope / low-confidence, verify each slot resolves in the register. | Catches the two LLM failure modes (hallucinated slot, over-confident wrong intent) *before* any query runs. |
| **Mapping** | Translate validated intent into SQL / X-tee / API, shape rows into an answer. | Code — versioned and tested — so schema drift is a diff, not a retrain. |
| **Relational DB / EHR** | Source of truth for both existence checks and business queries. | Never exposed to the LLM. Only the Validator and Mapping speak to it. |
| **Audit log** | Record every intent, confidence, slot, outcome. | Feeds the retraining loop and gives regulators a straight line from question to query. |

Dashed arrows are observability (writes to the log) and failure paths (early exits back to the GUI). The solid chain is the happy path.
