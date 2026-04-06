# Phase 3 Plan: Session Control and Integration (Weeks 13-18)

## Scope

- Gateway 4: Physical Layer Abstraction (4 Jul - 12 Jul)
- Gateway 5: State Machine (13 Jul - 26 Jul)
- Gateway 6: Hailing and Session Control (27 Jul - 9 Aug)

## Objectives

- Integrate protocol core into session-capable system behavior
- Enforce strict state transition correctness across duplex modes
- Deliver reliable hailing, reconnect, and COMM_CHANGE behavior

## Why This Phase Comes After Workshop Prep

- Session-control defects are harder to diagnose than core wire-format defects, so this phase starts only after artifact-level confidence exists.
- Physical abstraction and state transitions are cross-cutting concerns that must be stabilized before final validation work begins.

## Workstreams

1. Physical channel abstraction and adapters
2. State machine implementation and validation
3. Session control roles and lifecycle
4. Integration tests and observability hooks

## Week-by-Week Plan

### Weeks 13-14 (Gateway 4)

- Engineer A + Developer B
     - Define `PhysicalChannel` interface with PCID semantics
     - Implement in-memory deterministic loopback
     - Implement TCP/UDP ground test adapter
- Engineer B + Developer A
     - Add channel status and failure-injection test hooks
     - Build unit tests for multi-channel behavior
- Engineer C + Developer C
     - Define channel operations runbook, observability thresholds, and incident triage flow
     - Build multi-host rehearsal scripts for ground-test operations

Exit criteria:

- Mock and network channels working
- PCID support validated
- Channel health/status observable in logs/tests

Why these steps:

- A strict channel abstraction decouples protocol logic from transport details, enabling deterministic testing and realistic ground testing in parallel.
- Failure-injection hooks are included now because recovery behavior must be validated before session control is finalized.

### Weeks 15-16 (Gateway 5)

- Engineer A
     - Map tables 5-1/5-2/5-3/5-4 to explicit transition definitions
- Developer A
     - Implement state engine and transition guards
- Engineer B
     - Build transition matrix tests including invalid transitions
- Engineer C
     - Validate transition behavior against interoperability scenarios and operator workflow constraints
- Developer B
     - Add state history logging and analysis tooling
- Developer C
     - Automate transition regression packs and failure-replay test jobs

Exit criteria:

- Full/Half/Simplex duplex transitions match tables
- Invalid transitions rejected deterministically
- State history logs are complete and queryable

Why these steps:

- State tables encoded as explicit transitions prevent implicit behavior from drifting away from CCSDS tables.
- History logging provides forensic visibility for debugging invalid or rare transitions.

### Weeks 17-18 (Gateway 6)

- Engineer A + Developer A
     - Implement caller/responder hailing logic (Tables 5-7 and 5-10)
     - Implement reconnect/rehailing (Tables 5-9 and 5-12)
- Engineer B + Developer B
     - Implement COMM_CHANGE behavior (Tables 5-8 and 5-11)
     - Build session lifecycle and failure-recovery tests
- Engineer C + Developer C
     - Run operations-focused end-to-end rehearsals for role handoff, recovery, and channel-change procedures
     - Produce triage guide updates from integration findings

Exit criteria:

- Link establishment succeeds with correct role behavior
- Reconnect recovers communication loss cases
- Session lifecycle completes and is test-verified

Why these steps:

- Hailing/reconnect/COMM_CHANGE are integration-heavy flows; pairing implementation with targeted failure-recovery tests reduces late surprises.
- Role-based behavior validation (caller vs responder) is essential for external interoperability.

## Required Documents Produced in Phase 3

- Physical layer interface spec and adapter guide
- State machine transition matrix (traceable to spec tables)
- Session control sequence diagrams and lifecycle report

## Risks and Mitigations

- Risk: state explosion and hidden transition bugs
     - Mitigation: transition table as executable data + matrix tests
- Risk: network nondeterminism masking bugs
     - Mitigation: deterministic loopback is mandatory baseline

Rationale for risk controls:

- Deterministic test baselines isolate protocol defects from transport jitter.
- Executable transition matrices reduce human interpretation errors in complex state logic.

## Phase 3 Definition of Success

- Gateways 4-6 complete with passing unit/integration tests
- Session control features demonstrably stable under recovery scenarios

[⬆️ Back to Top ⬆️](#phase-3-plan-session-control-and-integration-weeks-13-18)

---

[Back Page](./Phase-2-Workshop-Preparation.md) | [Next Page](./Phase-4-Validation-Documentation.md)
