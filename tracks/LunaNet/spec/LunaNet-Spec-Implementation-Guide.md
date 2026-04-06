# LunaNet Spec Implementation Guide

Source basis:

- `Tracks/LunaNet/spec.pdf`
- `Tracks/LunaNet/lunanet-signal-in-space-recommended-standard-augmented-forward-signal-vol-a.pdf - LSIS-AFS-volA.pdf`

## Purpose

Translate LSIS-AFS normative definitions into implementation boundaries and verification rules for the software reference implementation.

## Normative Focus Areas For This Competition

- Spreading code generation families and assignments
- Frame/message structure and timing
- BCH/LDPC/CRC/interleaving behavior
- Baseband modulation and I/Q generation
- Frame synchronization and decode flows
- Message parsing and timing reconstruction

## Sections With Highest Implementation Impact

- 2.3.x signal structure and code generation behavior
- 2.4.x frame structure and coding flow
- 2.5.x message content and field semantics
- Appendix C/D/E code parameter references

Why this prioritization:

- These sections directly determine algorithm outputs and interoperability, which dominate scoring.

## Architecture Rules

- Separate deterministic code generation from framing/encoding.
- Separate signal synthesis from decoding pipelines.
- Keep codec primitives (BCH/LDPC/CRC/interleaver) independently testable.
- Keep parser logic independent from decoder internals.

Why these rules:

- They improve test isolation and reduce failure coupling between encode/decode paths.

## Critical Constants To Treat As Configuration-Backed

- PRN assignments and code parameters
- Sync pattern symbols and frame lengths
- Subframe sizes and coding settings
- Chip and symbol rates

Why configuration-backed:

- Hardcoding constants across modules creates drift and difficult auditability.

## Risk Hotspots

- Off-by-one errors in frame/subframe boundaries
- Incorrect puncturing/interleaving behavior in LDPC path
- Inconsistent code coherency across channels
- Symbol/chip rate mismatch and output timing drift

## Spec Governance Roles

- Engineer A: normative interpretation owner
- Engineer B: evidence and test-threshold owner
- Engineer C: interoperability conformance owner
- Developers A/B/C: section-linked implementation and test evidence generation

[⬆️ Back to Top ⬆️](#lunanet-spec-implementation-guide)

---

[Back Page](../requirements/LunaNet-Requirements-Traceability-Plan.md) | [Next Page](../spec-tables/LunaNet-Spec-Tables-Integration-Guide.md)
