# Phase 1 Plan: Foundation and Encoding (Gateways 0-3)

## Scope

- Gateway 0: architecture and environment readiness
- Gateway 1: spreading code generation
- Gateway 2: message encoding (BCH, LDPC, CRC, interleaving)
- Gateway 3: navigation frame assembly and validation

## Objectives

- Establish deterministic architecture for algorithmic and DSP-heavy implementation.
- Deliver reproducible PRN code generation and encoded message artifacts.
- Prove framing correctness before signal synthesis work begins.

## Why This Phase Is First

- Code generation and encoding define every downstream artifact; defects here propagate into decoding and interoperability.
- Performance and correctness in later gateways depend on deterministic primitives and table-backed constants established now.

## Workstreams

1. Architecture and table integration
2. PRN code generation correctness
3. FEC and CRC encoding path
4. Frame construction and timing validation
5. Test vectors and benchmark baselines

## Week-by-Week Plan

### Weeks 1-2 (Gateway 0)

- Engineer A
     - Freeze architecture boundaries between code generation, encoding, signal, decoding, and parsing.
     - Approve ADRs for language/runtime and deterministic processing strategy.
- Engineer B
     - Define validation thresholds for correctness and runtime budgets.
- Engineer C
     - Define interoperability artifact schema and weekly exchange operating flow.
- Developer A
     - Build code-generation module scaffolding and parameter-loader interfaces.
- Developer B
     - Build frame/encoding module scaffolding and stable data model types.
- Developer C
     - Build CI jobs for table-validation, tests, and benchmark capture.

Exit criteria:

- Architecture baseline approved and traceable.
- Spec-table ingestion passes schema checks.
- CI executes deterministic baseline tests.

### Weeks 3-4 (Gateway 1)

- Engineer A + Developer A
     - Implement Gold, Weil primary, secondary, and tertiary code generation paths.
     - Validate PRN outputs against Annex and spec-table references.
- Engineer B + Developer C
     - Implement code-family test matrix for PRN 1-210 and selected edge checks.
     - Add code-generation runtime benchmark capture.
- Engineer C + Developer B
     - Generate interoperability-ready code artifacts with metadata and checksums.

Exit criteria:

- Reference-match correctness at required PRNs.
- Deterministic regeneration of identical code artifacts.
- Runtime metrics collected and published.

### Weeks 5-6 (Gateway 2)

- Engineer A + Developer A
     - Implement BCH path for Subframe 1 and CRC-24 path for longer subframes.
     - Integrate LDPC BG2 settings for SB2/SB3/SB4 paths.
- Engineer B + Developer C
     - Build unit and negative tests for BCH/CRC/LDPC/interleaver boundaries.
     - Validate encoded length and filler/bit-layout compliance.
- Engineer C + Developer B
     - Produce encoded-vector package format for cross-team decode checks.

Exit criteria:

- Encoding outputs match expected dimensions and constraints.
- Error-detection behavior validated under malformed input injections.
- Encoded vectors reproducible in clean environment.

### Weeks 7-8 (Gateway 3)

- Engineer A + Developer B
     - Assemble 12-second frame with sync, SB1, SB2, SB3, and SB4 structures.
     - Validate Table 14 constants and subframe layouts.
- Engineer B + Developer C
     - Validate frame structural integrity and timing-field consistency.
     - Add frame-level regression and property checks.
- Engineer C + Developer A
     - Build partner-facing frame artifact exports and decode expectations.

Exit criteria:

- Frame structure conforms to timing and layout constraints.
- Sync pattern and subframe segmentation validated.
- Frame artifacts are exchange-ready and replayable.

## Risks and Mitigations

- Risk: table-integration drift
     - Mitigation: strict schema validation and table-hash checks in CI.
- Risk: framing off-by-one defects
     - Mitigation: explicit bit-offset tests from `sb2_bit_layout.csv` and frame-constant checks.

## Phase 1 Definition of Success

- Gateways 0-3 complete with evidence-backed artifacts.
- Encoding and frame outputs are deterministic, valid, and exchange-ready.

[⬆️ Back to Top ⬆️](#phase-1-plan-foundation-and-encoding-gateways-0-3)

---

[Back Page](./LunaNet-Deliverables-Program-Plan.md) | [Next Page](./Phase-2-Signal-and-Decoding.md)
