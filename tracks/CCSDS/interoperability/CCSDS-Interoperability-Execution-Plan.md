# CCSDS Interoperability Execution Plan

Source basis:

- `Tracks/CCSDS/interoperability.pdf`

## Goal

Ensure independent implementations can exchange CCSDS 235.1 traffic correctly across SPDU, COP-P, session, and recovery behaviors.

## Interoperability Levels

- Level 1: SPDU interoperability
- Level 2: COP-P interoperability
- Level 3: Session establishment interoperability
- Level 4: Data transfer interoperability
- Level 5: Error recovery interoperability

Why this sequence:

- It validates wire primitives before stateful behavior.
- It isolates failures early so root causes are easier to identify.

## Standard Exchange Assets

- SPDU export format (`spdu_*.bin` + metadata JSON)
- Frame export format (`frame_*.bin` + metadata)
- Session event traces (`session_*.json`)

Why standardized formats matter:

- Without common metadata, teams can disagree about inputs and still think they disagree about protocol behavior.
- Structured artifacts allow deterministic replay and bug triage.

## Core Test Cases To Run Every Week

1. Basic SPDU exchange
2. COP-P sequence validation
3. COP-P retransmission recovery
4. Session establishment in full duplex
5. Session establishment in half duplex
6. Expedited data transfer
7. Sequence-controlled data transfer under loss
8. Reconnect behavior
9. Resynchronization behavior
10. Multi-channel PCID routing

## Metrics and Targets

- SPDU encoding match rate: 100%
- SPDU decoding success: 100%
- Sequence-controlled delivery guarantee: 100%
- Session establishment success: >99%
- Frame throughput and latency per CCSDS targets
- Memory overhead and sustained operation stability

## 6-Person Interoperability Operating Cell

- Engineer C (lead): exchange governance, partner coordination, issue triage
- Engineer A: normative protocol behavior arbitration
- Engineer B: metric verification and acceptance checks
- Developer A: SPDU/COP-P defects and fixes
- Developer B: frame/channel tooling and parser traces
- Developer C: replay automation and report generation

## Weekly Interop Rhythm

- Monday: publish test vectors and baseline expected outputs
- Wednesday: internal cross-validation run
- Friday: external exchange run and discrepancy review

Why weekly cadence:

- Interop drift can appear after seemingly small code changes.
- Frequent exchange keeps compatibility current instead of deferring risk to final weeks.

## Report Template (per run)

- Partner implementation ID
- Asset versions and hashes
- Test cases executed
- Pass/fail matrix
- Defects and root-cause category
- Resolution owner and ETA
- Re-run status

[⬆️ Back to Top ⬆️](#ccsds-interoperability-execution-plan)

---

[Back Page](../requirements/CCSDS-Requirements-Traceability-Plan.md) | [Next Page](../spec/CCSDS-Spec-Implementation-Guide.md)
