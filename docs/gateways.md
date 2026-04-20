# Gateway Roadmap

The LunaNet LSIS-AFS reference implementation is structured into nine gateways (0-8). Each gateway is a self-contained, testable milestone with clear success criteria.

For each gateway below, we list the purpose, key deliverables in this repository, and how to validate completion.

---

## Gateway 0 - Design & Architecture

**Goal**: Design the system architecture and establish the development foundation.

This repository provides:

- High-level architecture and module diagrams in `docs/overview.md` and `docs/developer-guide.md`.
- Technology stack selection and coding conventions.
- Testing strategy (unit, integration, and compliance tests).

**Validation**:

- Architecture documents exist and describe all major modules (codes, encoding, messages, signal, decoder, utils).
- Development environment and test framework can be set up using documented commands.

---

## Gateway 1 - Spreading Code Generation

**Goal**: Generate all spreading codes per LSIS-AFS specification.

This repository provides:

- Gold code generator for 2046-chip AFS-I primary codes (PRN 1-210).
- Weil primary code generator for 10230-chip AFS-Q codes (PRN 1-210).
- Legendre sequence and Weil tertiary code generator for 1500-chip tertiary codes.
- Secondary code generator for the four 4-chip sequences.
- Tiered code assembly (primary + secondary + tertiary).

**Validation**:

- All codes match Annex 3 reference hex values for all PRNs.
- Code lengths match Table 9 and related LSIS-AFS parameters.
- Code generation per PRN completes within the 1-second target.

---

## Gateway 2 - Forward Error Correction

**Goal**: Implement FEC encoding and decoding for all relevant subframes.

This repository provides:

- BCH(51,8) encoder/decoder for Subframe 1.
- CRC-24 generator/validator.
- Rate 1/2 LDPC encoder/decoder for SB2, SB3, and SB4 using Annex 1 matrices.
- Block interleaver/deinterleaver (60x98) for SB2-SB4.

**Validation**:

- Round-trip tests show encoders/decoders recover original bits.
- CRC-24 output matches polynomial defined in LSIS-AFS Volume A.
- LDPC encoding handles puncturing and filler bits correctly.

---

## Gateway 3 - Navigation Message Framing

**Goal**: Build complete 12-second frames with correct subframe structure.

This repository provides:

- Sync pattern builder (68 uncoded symbols).
- Builders for SB1 (FID, TOI), SB2 (Clock & Ephemeris), SB3 (variable messages), SB4 (network access).
- Frame assembler that concatenates SP + SB1 + interleaved SB2-SB4 into 6000 symbols.

**Validation**:

- Frame symbol counts and bit allocations match LSIS-AFS tables (for example, 68 + 52 + 5880 symbols).
- Frame duration is 12 seconds and respects time parameters (TOI, ITOW, week number).

---

## Gateway 4 - Baseband Signal Generation

**Goal**: Generate AFS baseband IQ samples.

This repository provides:

- BPSK modulators for AFS-I (1.023 Mchips) and AFS-Q (5.115 Mchips).
- Tiered code application to produce complex IQ samples at configurable sample rates (for example, 10.23/20.46 MHz).
- Signal file exporters (binary and CSV).

**Validation**:

- Symbol rate of 500 symbols/s on AFS-I.
- Generated IQ sequences cover exactly one or more 12-second frames.
- Code/data alignment matches LSIS timing requirements.

---

## Gateway 5 - Frame Synchronisation & Decoding

**Goal**: Detect and decode frames from received IQ samples.

This repository provides:

- Sync pattern detection and frame boundary estimation.
- Symbol extraction from IQ streams.
- BCH, LDPC, and CRC-based decoding chain.

**Validation**:

- Frame detection reliability above 99% at SNR 0 dB in test scenarios.
- Decoding completes within 1 second per 12-second frame.

---

## Gateway 6 - Message Parsing

**Goal**: Extract navigation content from decoded subframes.

This repository provides:

- SB1 parser (FID, TOI).
- SB2 parser (Week Number, ITOW, Clock & Ephemeris, Health & Safety, Time Conversions).
- SB3/SB4 parsers for supported message types (for example, multiple orbit almanac, coordinate frame conversions, network access).

**Validation**:

- Parsed fields match known test vectors and satisfy bit-field definitions.
- Time of Transmission reconstruction is accurate to code-phase resolution.

---

## Gateway 7 - Integration & Interoperability

**Goal**: End-to-end validation and interoperability with other implementations.

This repository provides:

- Round-trip encode -> modulate -> decode -> parse tests.
- Test vector suite (codes, frames, signals).
- Performance benchmarks (throughput, latency).

**Validation**:

- 100% bit-accurate recovery in round-trip tests.
- Meets performance targets for frame processing.
- Cross-implementation comparison where available.

---

## Gateway 8 - Documentation & Examples

**Goal**: Provide complete documentation, examples, and validation reports.

This repository provides:

- Project overview, gateway roadmap, developer guide, and compliance report.
- Usage examples showing how to generate codes, frames, and IQ samples and how to decode them.
- Performance analysis and test documentation.

**Validation**:

- A new user can clone the repo, follow the docs, and reproduce key results without external guidance.
- All public APIs used in examples are documented.
