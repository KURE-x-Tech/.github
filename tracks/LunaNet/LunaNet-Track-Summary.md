# LunaNet Track Summary

## What This Track Is

LunaNet focuses on LSIS-AFS signal-processing implementation: spreading-code generation, FEC and frame assembly, I/Q signal synthesis, decode recovery, and parsed navigation outputs.

## Difficulty Level

- Overall difficulty: Very High
- Why:
     - Multi-domain complexity (DSP, coding theory, framing, decoding, parser correctness)
     - Tight coupling between constants/tables and algorithm behavior
     - Failures can propagate across encode/decode chain and be expensive to debug

## Primary Requirements

- Correct Gold/Weil code generation and PRN mapping
- Correct BCH/LDPC/CRC/interleaver behavior with exact frame layout
- Standards-aligned I/Q generation at required rates and formats
- Reliable synchronization and decode, including noisy-condition robustness
- Accurate parser extraction for WN/ITOW/TOI and timing reconstruction

## Recommended Stack

Primary recommendation:

- Hybrid stack:
     - Rust or C++ for performance-critical DSP/codec core
     - Python for analysis harnesses, vector validation, and tooling glue

Why this stack:

- Core path needs deterministic performance and strong control of memory/layout
- Validation and experiment workflows benefit from Python’s data tooling

Supporting tooling:

- Immutable table ingestion with schema/hash validation
- BER/SNR sweep automation and replay fixtures
- Artifact packaging for cross-team interoperability runs

## Team Lead Recommendation

Primary lead profile:

- DSP/communications + algorithm-heavy team should lead this track

Best lead roles:

- Engineer A (normative algorithm/spec interpretation)
- Engineer B (decode quality and performance metrics)
- Engineer C (interop operations and exchange governance)

Developer focus:

- Developer A: code generation + encoding
- Developer B: frame + signal synthesis
- Developer C: decoding + parser + automation

## Useful Execution Advice

- Treat spec tables as controlled inputs; validate schema and hashes in CI.
- Build round-trip tests early (encode -> signal -> decode -> parse) and run continuously.
- Separate algorithm kernels from orchestration to reduce debugging blast radius.
- Timebox tuning work; preserve reproducibility when optimizing BER/performance.

## Risk Profile

Top risks:

- Table drift and constant misuse
- Framing off-by-one errors
- Decoder instability at realistic noise levels
- Late interoperability divergence

Mitigation posture:

- Golden-vector gating, fixed SNR test suites, and mandatory weekly partner exchange.

[⬆️ Back to Top ⬆️](#lunanet-track-summary)

---

[Back Page](./README.md) | [Next Page](./deliverables/README.md)
