# Business model

Estonian legal terms are first-class. INSPIRE / LADM names are aliases.

```mermaid
classDiagram
    class Parcel["Parcel (Katastriüksus)"] {
        cadastralNumber
        area
        purpose
    }
    class Building["Building (Ehitis)"] {
        ehrCode
        constructionYear
        netArea
        floors
        energyClass
    }
    class Address["Address (ADS)"] {
        adsOid
        text
    }
    class Permit {
        type : building | use
        status
        date
    }
    class Owner {
        role
    }
    Parcel "1" --> "*" Building : contains
    Building "1" --> "1..*" Address : located at
    Building "1" --> "*" Permit : has
    Building "1" --> "*" Owner : owned by
    Parcel "1" --> "*" Owner : owned by
```

| Concept | Alias (INSPIRE / LADM) | Note |
|---|---|---|
| Building | `bu-core2d:Building` | *ehitis* covers building **and** structure |
| Parcel | `cp:CadastralParcel` / `LA_Parcel` | |
| Owner | `LA_Party` / `LA_RRR` | personal data: service layer only |
| Permit | — | *ehitusluba* ≠ *kasutusluba* |

> Draft. To be validated against the real EHR data model.
