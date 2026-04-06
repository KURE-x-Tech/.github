# LunaNet Spec Tables Integration Guide

Source basis:

- `Tracks/LunaNet/spec_tables/README.md`
- `Tracks/LunaNet/spec_tables/*.csv`

## Purpose

Provide a controlled method for consuming machine-readable LSIS-AFS constants and table data in code, tests, and validation reports.

## Table Families

Code generation tables:

- `appendix_c_gold_g2_delays.csv`
- `appendix_d_weil_primary_params.csv`
- `appendix_e_weil_tertiary_params.csv`
- `table_10_secondary_codes.csv`
- `table_11_code_assignments.csv`

Frame/message tables:

- `table_12_sync_pattern.csv`
- `table_14_frame_structure.csv`
- `sb2_bit_layout.csv`

Encoding/signal/time tables:

- `encoding_parameters.csv`
- `signal_parameters.csv`
- `time_system_parameters.csv`
- `file_format_headers.csv`

## Integration Rules

1. Parse CSV files once at startup or build time and validate schema.
2. Generate immutable in-memory representations for runtime use.
3. Reject runs when required tables are missing or malformed.
4. Version and hash table assets in build reports.

Why these rules:

- They prevent silent deviations from official parameter sets.
- They make results reproducible across environments and team members.

## Validation Strategy

- Unit tests validate parser correctness and schema checks.
- Golden tests validate PRN outputs against Annex references.
- Frame tests validate sync pattern and structure constants.
- Signal tests validate rate and timing constants are consumed correctly.

## Ownership

- Developer A: code-generation table ingestion and validation
- Developer B: frame/signal/time table integration
- Developer C: table schema tests and CI validation guardrails
- Engineer A/B/C: review and sign-off for normative table usage

## Change Control

- Any table update requires:
     - hash/version change record
     - impacted test list
     - before/after evidence

[⬆️ Back to Top ⬆️](#lunanet-spec-tables-integration-guide)

---

[Back Page](../spec/LunaNet-Spec-Implementation-Guide.md) | [Next Page](../interoperability/LunaNet-Interoperability-Execution-Plan.md)
