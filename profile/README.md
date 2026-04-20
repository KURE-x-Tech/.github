# KURE x Tech - LunaNet LSIS-AFS Reference Implementation

KURE x Tech is building a software reference implementation of the LunaNet Signal-In-Space - Augmented Forward Signal (LSIS-AFS) specification for NASA's LunaNet programme. The focus is **pure software digital signal processing**: spreading codes, message encoding/decoding, frame structure, and baseband IQ sample generation - not RF hardware.

This implementation is being developed as part of the **LSIS-AFS Reference Implementation Competition** running March-August 2026, which uses a gateway-based approach and scores teams on correctness, performance, completeness, code quality, and innovation.

---

## What this project implements

At a high level, the project aims to provide:

- Spreading code generators (Gold, Weil, Legendre; primary, secondary, tertiary) for PRN 1-210, validated against Annex 3 reference codes.
- Forward error correction and integrity: BCH(51,8), rate 1/2 LDPC encoders/decoders for SB2-SB4, and CRC-24 generation/validation.
- Navigation message framing: 12-second frames with sync pattern, Subframes 1-4, and correct bit/symbol allocations per LSIS-AFS Volume A.
- Baseband signal generation: dual-channel BPSK AFS-I/AFS-Q at 1.023 Mchips and 5.115 Mchips, producing IQ sample files at configurable sample rates.
- Decoding and parsing: frame synchronization, FEC decoding, CRC checking, and extraction of clock, ephemeris, and network access data.
- Validation utilities: test vectors, BER vs SNR checks, and LSIS requirement compliance reports.

See the [Gateway Roadmap](../docs/gateways.md) for how these capabilities are broken down and validated over Gateways 0-8.

---

## Who this is for

This repository is meant for:

- **Competition judges** who need to verify gateway completion, LSIS compliance, and performance against the published requirements and deliverables.
- **Contributors** implementing or extending the LSIS-AFS reference stack in any programming language with a focus on reproducible, test-driven DSP.
- **Researchers and students** looking for a worked example of a GNSS-style lunar navigation signal implementation aligned with the official LunaNet interoperability specification.

For technical background on LunaNet and LSIS-AFS, see the official specification and competition documents in `docs/spec/`.

---

## Documentation map

The documentation is organised into the following sections:

- **Project Overview & Requirements** - `docs/overview.md`  
  Short summary of the LSIS-AFS reference implementation scope and the functional/non-functional requirements.

- **Gateway Roadmap** - `docs/gateways.md`  
  One section per gateway (0-8), with goals, deliverables, validation criteria, and links to the relevant modules and tests.

- **Developer Guide** - `docs/developer-guide.md`  
  How to set up the environment, run tests, generate frames and IQ samples, and execute end-to-end encode -> decode workflows.

- **Compliance & Testing** - `docs/compliance.md`  
  Mapping between LSIS-AFS "shall" requirements, competition success criteria, and the concrete tests/artefacts in this repo.

- **Concept Guides** - `docs/concepts/`  
  Narrative tutorials such as [The Anatomy of a Space Mission: From Orbit to Earth](../docs/concepts/anatomy-of-a-space-mission.md), providing context on space, ground, and user segments.

---

## Getting started

If you are a **judge**, start with:

1. [docs/overview.md](../docs/overview.md) - project scope and requirements.
2. [docs/gateways.md](../docs/gateways.md) - gateway completion status and deliverables.
3. [docs/compliance.md](../docs/compliance.md) - LSIS requirement coverage and validation strategy.

If you are a **developer**:

1. Read [docs/developer-guide.md](../docs/developer-guide.md) to set up the environment and run the tests.
2. Implement or inspect modules gateway-by-gateway as described in [docs/gateways.md](../docs/gateways.md).
3. Consult [docs/concepts/anatomy-of-a-space-mission.md](../docs/concepts/anatomy-of-a-space-mission.md) for background on space mission architecture and LunaNet context as needed.
