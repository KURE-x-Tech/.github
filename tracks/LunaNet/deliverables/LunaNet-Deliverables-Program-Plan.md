# LunaNet Deliverables Program Plan

Source basis:

- `Tracks/LunaNet/deliverables.pdf`

## Mission

Deliver a full LSIS-AFS reference implementation covering spreading codes, FEC, frame building, baseband I/Q generation, decoding, parsing, integration, and final documentation.

## Why This Plan Uses Gateway Sequencing

- LunaNet work spans DSP, coding theory, and interoperability; gateway partitioning limits defect blast radius.
- Early code-generation and framing correctness reduce wasted work in decoder and parser phases.
- Integration success depends on strict artifact and timing correctness (12-second frame, symbol/chip rates).

## Competition Phases and Gateway Focus

- Foundation (April): Gateway 0 and Gateway 1
     - Architecture plus spreading code generation
- Encoding (May): Gateway 2 and Gateway 3
     - BCH/LDPC/CRC/interleaving and navigation frame assembly
- Signal (May-June): Gateway 4
     - I/Q signal generation and modulation behavior
- Decoding (July): Gateway 5 and Gateway 6
     - Sync detection, decoding, and message extraction
- Integration (August): Gateway 7 and Gateway 8
     - End-to-end validation, interoperability, documentation, packaging

## Minimum Viable Submission Gate (Pass/Fail)

Must include all:

- At least 12 PRN spreading codes generated correctly
- Complete 12-second navigation frame encoding
- I/Q signal files generated
- Validation tests for implemented features
- Setup/build instructions

Why this is critical:

- Failing the minimum gate yields zero evaluation regardless of partial progress.

## Scoring Strategy (from deliverables)

- Correctness: 40
- Performance: 20
- Completeness: 20
- Code quality: 10
- Innovation/extras: 10

## 6-Person Ownership Model

- Engineer A: architecture/spec interpretation (LSIS volume + gateway standards)
- Engineer B: validation/performance owner (BER, sync, runtime budgets)
- Engineer C: interoperability and operations owner (exchange, runbooks)
- Developer A: code generation + encoding core
- Developer B: frame/message + signal generation core
- Developer C: decoding/parser + integration automation

## Detailed Execution Docs

- Deliverables planning index: [README.md](./README.md)
- Phase 1 (Gateways 0-3): [Phase-1-Foundation-and-Encoding.md](./Phase-1-Foundation-and-Encoding.md)
- Phase 2 (Gateways 4-6): [Phase-2-Signal-and-Decoding.md](./Phase-2-Signal-and-Decoding.md)
- Phase 3 (Gateways 7-8): [Phase-3-Integration-Submission.md](./Phase-3-Integration-Submission.md)
- Gateway ownership matrix: [Gateway-RACI-Matrix.md](./Gateway-RACI-Matrix.md)

Why these companion docs:

- The program plan remains the high-level control view.
- Phase files and RACI isolate day-to-day execution detail and ownership accountability.

## Gateway-by-Gateway Execution Priorities

- Gateway 0
     - Freeze architecture, interfaces, and test strategy
- Gateway 1
     - Validate all PRN code families against references
- Gateway 2
     - Implement and verify BCH/LDPC/CRC/interleaving
- Gateway 3
     - Assemble frame structure with exact timing and field allocations
- Gateway 4
     - Generate standards-compliant I/Q outputs
- Gateway 5
     - Decode with reliable sync and error-correction behavior
- Gateway 6
     - Parse subframes and reconstruct timing fields
- Gateway 7
     - End-to-end round-trip + interoperability + benchmark proof
- Gateway 8
     - Submission-quality documentation and evidence package

## Submission Readiness Milestones

- T-21 days: all core gateways (0-6) feature complete
- T-14 days: integration and interoperability stable
- T-7 days: documentation + evidence freeze candidate
- T-3 days: clean-room rerun and artifact signing
- T-1 day: submission checklist lock

[⬆️ Back to Top ⬆️](#lunanet-deliverables-program-plan)

---

[Back Page](./README.md) | [Next Page](./Phase-1-Foundation-and-Encoding.md)
