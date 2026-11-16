# Intent catalog

The **closed set**. Anything else is `out_of_scope`.

```mermaid
mindmap
  root((EHR))
    building
      construction_year
      area
      floors
      purpose_of_use
      energy_class
      technical_data
    permits
      building_permit_status
      use_permit_status
      permit_history
    land
      parcel_of_building
      parcel_area
      parcel_purpose
    location
      find_by_address
      find_by_cadastral_number
    meta
      out_of_scope
      needs_clarification
```

- ID format: `ehr.<group>.<intent>`, e.g. `ehr.permits.use_permit_status`.
- Same IDs in training data, mapping and logs.
- Draft list. Final set comes from the real EHR services.
