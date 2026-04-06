# CCSDS Track Summary

## What This Track Is

CCSDS focuses on protocol correctness, session behavior, conformance evidence, and interoperability across independent implementations of CCSDS 235.1 session control.

## Difficulty Level

- Overall difficulty: High
- Why:
     - Heavy protocol-state complexity (SPDU, COP-P, session transitions, reconnection)
     - Strict conformance expectations and evidence quality requirements
     - Interoperability bugs can be subtle and hard to isolate

## Primary Requirements

- Wire-format and parser correctness for SPDU and frames
- Correct sequence control, retransmission, and state transitions
- Stable session lifecycle behavior (hailing, reconnect, COMM_CHANGE)
- Strong conformance artifacts (tests, matrix, PICS, reproducible evidence)

## Recommended Stack

Primary recommendation:

- Rust for protocol core and state/session layers

Why this stack:

- Strong memory safety and deterministic runtime behavior
- Good fit for low-latency protocol processing
- Excellent test/benchmark tooling for evidence-heavy scoring

Fallback recommendation:

- C++20 with strict memory and review discipline

Supporting tooling:

- CI with deterministic regression and benchmark jobs
- Structured artifact pipeline (test vectors, decode outputs, conformance reports)

## Team Lead Recommendation

Primary lead profile:

- Protocol/systems-heavy team should lead this track

Best lead roles:

- Engineer A (protocol/spec authority)
- Engineer B (validation/conformance authority)
- Engineer C (interoperability operations authority)

Developer focus:

- Developer A: SPDU/COP-P core
- Developer B: frame + transport + docs tooling
- Developer C: integration + benchmarks + automation

## Useful Execution Advice

- Lock section-to-test traceability early; do not defer it to final phase.
- Treat interoperability as weekly practice, not final-week activity.
- Keep state-machine behavior data-driven and exhaustively tested.
- Freeze submission assembly workflow before Gateway 8 so final weeks are hardening-only.

## Risk Profile

Top risks:

- Spec interpretation drift
- Sequence/state edge-case bugs
- Late interoperability mismatches
- Documentation evidence lag

Mitigation posture:

- Twice-weekly spec review, mandatory regression for bug fixes, and fixed interop cadence.

[⬆️ Back to Top ⬆️](#ccsds-track-summary)

---

[Back Page](./README.md) | [Next Page](./deliverables/README.md)
