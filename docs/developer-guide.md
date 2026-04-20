# Developer Guide

This guide explains how to set up the environment, run tests, and use the KURE x Tech LSIS-AFS implementation for development and experimentation.

---

## Setup

1. Clone the repository.
2. Install dependencies as described in the project's build instructions (language-specific).
3. Run the test suite to verify your environment.

Example:

```bash
# example only - adapt to your tooling
git clone https://github.com/KURE-x-Tech/lunanet-lsis-afs.git
cd lunanet-lsis-afs
make test
```

---

## Code structure

The codebase is organised roughly as follows (names may vary by language):

- `src/codes/` - Gold, Weil, Legendre, secondary, and tiered code generators.
- `src/encoding/` - BCH, LDPC, CRC, and interleaver implementations.
- `src/messages/` - Subframe builders and parsers, frame assembly/disassembly.
- `src/signal/` - Baseband IQ generation and signal file I/O.
- `src/decoder/` - Frame synchronisation, symbol extraction, decoding pipeline.
- `tests/` - Unit and integration tests per gateway.

Refer to `docs/gateways.md` to see which modules correspond to each competition gateway.

---

## Common workflows

### Generate spreading codes and validate

- Use the code generation CLI or library entrypoint to generate codes for a chosen PRN.
- Run the provided tests or comparison script to check against Annex 3 reference codes.

### Build a navigation frame

- Call the SB1-SB4 builders with test data (ephemeris, clock, network access).
- Assemble them into a 12-second frame using the frame builder.
- Optionally export the frame as a symbol sequence for analysis.

### Generate IQ samples

- Choose PRN, frame content, and sample rate.
- Use the baseband generator to create IQ samples representing AFS-I/AFS-Q.
- Save samples as binary/CSV for downstream use.

### Decode and parse a signal

- Load IQ samples using the signal I/O utilities.
- Run frame synchronisation to locate frame boundaries.
- Decode SB1-SB4, verify CRCs, and parse navigation fields.

---

## Performance and validation

The project includes tests and benchmarks aligned with the published requirements:

- Code generation within 1 s per PRN.
- Frame encoding within 100 ms per frame.
- Frame decoding within 1 s per 12-second frame.
- BER ~= 1e-5 at SNR 0 dB for the target scenarios.

See `docs/compliance.md` for details and how to run these checks.
