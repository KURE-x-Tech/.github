# Compliance & Testing

This document maps key LSIS-AFS specification requirements and competition criteria to the tests and artefacts in this repository.

---

## Specification coverage

We target the software-relevant parts of LSIS-AFS Volume A and the competition spec:

- Signal structure, spreading codes, and modulation (LSIS-AFS Section 2.3).
- Navigation message format and encoding (LSIS-AFS Section 2.4).
- Time parameters and frame timing (competition spec time parameters table).
- Code assignment and test code support (Appendix F and interim test assignments).

A detailed requirement-to-test matrix is maintained in `docs/compliance-matrix.(md|csv)` (optional).

---

## Gateway validation

Each gateway has success criteria defined in the Requirements and Deliverables documents. For example:

- **Gateway 1** - all PRN codes match Annex 3 reference hex files; generation time per PRN meets performance target.
- **Gateway 3** - frame symbol counts and bit allocations match LSIS tables; frame duration is 12 seconds.
- **Gateway 5** - frame detection reliability above 99% at SNR 0 dB; LDPC decoder converges in <= 50 iterations.

Each criterion is associated with one or more automated tests (unit/integration) and, where relevant, benchmark scripts.

---

## Running the test suite

High-level guidance:

- `make test` - run all unit tests.
- `make test-gateway N=1` - run tests relevant to a specific gateway.
- `make bench` - run performance benchmarks (encoding/decoding throughput, frame processing time).

Refer to the project's build scripts or CI configuration for exact commands.

---

## Known limitations

Any LSIS requirements or optional competition features not currently implemented are listed here, with rationale and impact.

Examples might include:

- Partial coverage of SB3/SB4 message types.
- Limited SISE calculation utilities.
- Restricted set of channel impairments in BER testing.

These do not prevent the core gateways from being validated but are documented for transparency.
