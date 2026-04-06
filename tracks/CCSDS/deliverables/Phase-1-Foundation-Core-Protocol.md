# Phase 1 Plan: Foundation and Core Protocol (Weeks 1-10)

## Scope

- Gateway 0: Design and Architecture (5 Apr - 19 Apr)
- Gateway 1: SPDU Layer (20 Apr - 10 May)
- Gateway 2: COP-P Layer (11 May - 31 May)
- Gateway 3: Frame Layer (1 Jun - 14 Jun)

## Phase Objectives

- Establish architecture and test strategy that can scale to full compliance
- Deliver protocol core needed for workshop participation
- Prove deterministic and performant implementation behavior early

## Why This Phase Matters Most

- Gateways 0-3 form the scoring and interoperability foundation; instability here cascades into every later gateway.
- Early protocol correctness is cheaper than late rework after session and interoperability complexity is added.
- Benchmark and negative-test instrumentation in this phase reduces false confidence and prevents brittle progress.

## Workstreams

1. Architecture and design control
2. SPDU and directive serialization
3. COP-P behavior and state tracking
4. Frame construction/parsing + QoS
5. Unit tests and benchmark harness

## Week-by-Week Plan

### Weeks 1-2 (Gateway 0)

- Engineer A
     - Produce layer boundary design and section mapping
     - Draft initial ADRs for architecture choices
- Engineer B
     - Define verification strategy, coverage targets, and performance methodology
- Engineer C
     - Define interoperability rehearsal objectives and artifact exchange readiness criteria
     - Prepare operations runbook structure for workshop and gateway demos
- Developer A
     - Scaffold protocol crates/modules and shared types
- Developer B
     - Configure build/test tooling and CI skeleton
- Developer C
     - Build benchmark harness wiring and integration-test scaffolding

Exit criteria:

- Architecture document complete
- Build passes in clean environment
- Test framework executes baseline tests

Why these steps:

- Architecture and ADR work first creates shared mental models and avoids implementation divergence across team members.
- Early CI/test bring-up ensures every later gateway can be measured by evidence, not subjective progress.

### Weeks 3-5 (Gateway 1)

- Engineer A + Developer A
     - Implement SPDU encode/decode for F1, F2, 1-5
     - Implement directive decode for Type 1 and Type 2
- Engineer B + Developer C
     - Build malformed input and negative test matrix
     - Add sub-1 ms microbench checks per SPDU class
- Engineer C + Developer B
     - Build golden-vector generation pipeline and artifact review checklist
     - Integrate SPDU conformance evidence output into CI reports

Exit criteria:

- All SPDU variants pass golden tests
- Malformed SPDUs rejected gracefully
- Benchmarks indicate under 1 ms per SPDU

Why these steps:

- SPDU and directive handling define canonical wire semantics used by COP-P and frame processing.
- Negative tests are paired with performance checks to ensure robustness does not come at hidden latency cost.

### Weeks 6-8 (Gateway 2)

- Engineer A + Developer A
     - Implement FOP-P logic including 8/16-bit sequence handling
     - Implement FARM-P logic for receive/retransmit behavior
- Engineer B + Developer C
     - Implement COP-P coordinator behavior for Expedited and Sequence Controlled modes
     - Add persistence process tests and wrap-around edge cases
- Engineer C + Developer B
     - Implement COP-P observability events and triage dashboards for interoperability debugging
     - Build replay tooling for retransmit/resync scenarios captured during tests

Exit criteria:

- Sequence state variables validated under nominal and wrap conditions
- Retransmit and resynchronization scenarios pass
- Window logic enforces boundaries

Why these steps:

- COP-P sequence and retransmission logic is the reliability core of the protocol; correctness must be proven under edge conditions.
- Wrap-around and resynchronization tests are mandatory because defects often appear only at sequence boundaries.

### Weeks 9-10 (Gateway 3)

- Developer B + Engineer A
     - Implement P-frame and U-frame tx/rx pipeline
     - Add Version-3/Version-4 frame support and QoS flag handling
- Developer A + Engineer B
     - Build frame parser robustness tests and malformed frame rejection
     - Validate under 1 ms frame processing
- Engineer C + Developer C
     - Build frame-level interoperability drill scripts and transport trace capture tooling
     - Validate frame artifacts against workshop exchange runbook

Exit criteria:

- P-frame/U-frame handling correct and differentiated
- SPDU extraction from P-frames validated
- QoS and version flags verified

Why these steps:

- Frame layer correctness is the bridge from internal state logic to interoperable wire behavior.
- Version and QoS validation is separated explicitly because these fields are frequent interoperability failure points.

## Required Documents Produced in Phase 1

- Architecture baseline (for Gateway 0)
- Interface map for SPDU/COP-P/frame layers
- Unit test evidence report (Gateways 1-3)
- Initial benchmark report

## Key Risks and Mitigations

- Risk: misread section semantics
     - Mitigation: section-linked test names and review checkpoints
- Risk: hidden latency spikes
     - Mitigation: benchmark each merge on representative payloads

Rationale for risk controls:

- Section-linked tests make spec interpretation auditable.
- Per-merge benchmarking catches regressions at commit time instead of at gateway deadlines.

## Phase 1 Definition of Success

- Gateways 0-3 evidence complete
- Workshop prerequisites met (working core + hex dump capability)

[⬆️ Back to Top ⬆️](#phase-1-plan-foundation-and-core-protocol-weeks-1-10)

---

[Back Page](./Program-Execution-Plan.md) | [Next Page](./Phase-2-Workshop-Preparation.md)
