# Phase 2 Plan: Signal and Decoding (Gateways 4-6)

## Scope

- Gateway 4: signal generation and output formatting
- Gateway 5: synchronization and decoding
- Gateway 6: message parsing and timing reconstruction

## Objectives

- Produce standards-aligned baseband I/Q signals from encoded frame content.
- Recover data reliably through synchronization, deinterleaving, decoding, and CRC checks.
- Parse recovered subframes into structured navigation outputs with correct timing fields.

## Why This Phase Depends On Phase 1

- Signal-generation correctness depends on stable frame and code artifacts from Gateways 1-3.
- Decoder reliability depends on exact encoder and framing semantics; this phase validates round-trip behavior.

## Workstreams

1. I/Q synthesis and file-format compliance
2. Frame synchronization and decoder pipeline
3. Message parser and field extraction
4. End-to-end round-trip validation

## Week-by-Week Plan

### Weeks 9-10 (Gateway 4)

- Engineer A + Developer B
     - Implement AFS-I/AFS-Q modulation flow with required chip and symbol rates.
     - Verify mapping against signal parameters and file format headers.
- Engineer B + Developer C
     - Validate generated I/Q output dimensions, sample type, and timing alignment.
     - Benchmark generation latency and memory behavior.
- Engineer C + Developer A
     - Prepare interoperability-ready I/Q artifact package and metadata templates.

Exit criteria:

- I/Q output files conform to expected headers and sample formats.
- Symbol/chip timing alignment validated.
- Signal generation metrics recorded and reviewable.

### Weeks 11-12 (Gateway 5)

- Engineer A + Developer C
     - Implement sync detection and frame lock behavior for valid and degraded signals.
     - Implement deinterleaving and LDPC/BCH decode pipeline.
- Engineer B + Developer A
     - Validate decode reliability and BER behavior at target SNR profiles.
     - Build failure-injection and recovery tests for decode edge conditions.
- Engineer C + Developer B
     - Build decode artifact exchange loop and discrepancy reporting workflow.

Exit criteria:

- Reliable frame detection and lock performance.
- Decoded outputs pass CRC and structure validations.
- BER and decode thresholds measured against requirement targets.

### Weeks 13-14 (Gateway 6)

- Engineer A + Developer B
     - Implement parser for WN, ITOW, TOI, and subframe payload extraction.
     - Validate bit-offset and field-width correctness against table definitions.
- Engineer B + Developer C
     - Build parser conformance tests and malformed-payload rejection tests.
     - Add timing-reconstruction consistency checks.
- Engineer C + Developer A
     - Generate parser-level JSON outputs for interoperability replay and review.

Exit criteria:

- Parsed message fields are complete and consistent.
- Timing reconstruction is validated against expected formulas.
- Parser outputs are reproducible and exchange-friendly.

## Risks and Mitigations

- Risk: decode instability under noisy conditions
     - Mitigation: threshold tuning with controlled SNR test sweeps and replayable fixtures.
- Risk: parser misalignment with frame bit layout
     - Mitigation: explicit offset tests using `sb2_bit_layout.csv` and Table 14 constants.

## Phase 2 Definition of Success

- Gateways 4-6 complete with successful encode-to-decode-to-parse round-trip evidence.
- Signal, decoding, and parser outputs are reproducible and ready for integration-level evaluation.

[⬆️ Back to Top ⬆️](#phase-2-plan-signal-and-decoding-gateways-4-6)

---

[Back Page](./Phase-1-Foundation-and-Encoding.md) | [Next Page](./Phase-3-Integration-Submission.md)
