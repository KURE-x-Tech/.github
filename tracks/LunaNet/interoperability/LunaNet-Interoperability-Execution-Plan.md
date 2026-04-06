# LunaNet Interoperability Execution Plan

Source basis:

- `Tracks/LunaNet/interoperability.pdf`

## Goal

Ensure independent LSIS-AFS implementations can exchange compatible codes, frames, signals, decoded outputs, and parsed message fields.

## Interoperability Levels

- Level 1: code generation interoperability
- Level 2: encoding interoperability
- Level 3: signal generation interoperability
- Level 4: decoding interoperability
- Level 5: message parsing interoperability

Why this progression:

- It follows data flow from primitive codes to final interpreted navigation content.

## Standardized Artifacts

- `codes_prn{N}.hex`
- Frame export binaries and metadata
- I/Q sample exports (binary/CSV)
- Parsed JSON field exports

## Required Cross-Validation Matrix

- Every implementation decodes every other implementation’s generated assets.
- Record pass/fail and mismatch class per test level.

## Target Metrics

- Code reference match: 100%
- Frame structure correctness: 100%
- Frame detection >99%
- BER target at specified SNR thresholds
- Performance targets from requirements/deliverables

## Weekly Operating Rhythm

- Tuesday: publish standardized vectors and hashes
- Thursday: internal cross-implementation replay
- Friday: partner exchange and discrepancy closure

## 6-Person Roles

- Engineer C: interoperability coordinator and issue owner
- Engineer A: spec-conformance interpretation owner
- Engineer B: metrics/compliance owner
- Developer A: code and encoding mismatch fixes
- Developer B: frame/signal mismatch fixes
- Developer C: decode/parser mismatch fixes + replay automation

## Report Template

- Test level
- Vector ID and hash
- Producer implementation
- Consumer implementation
- Expected vs observed
- Mismatch class
- Fix owner
- Re-run result

[⬆️ Back to Top ⬆️](#lunanet-interoperability-execution-plan)

---

[Back Page](../spec-tables/LunaNet-Spec-Tables-Integration-Guide.md) | [Next Page](../submission-template/LunaNet-Submission-Authoring-Guide.md)
