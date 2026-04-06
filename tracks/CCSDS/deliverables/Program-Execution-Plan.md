# Program Execution Plan (Team of 6)

## 1. Mission Context

Build a specification-compliant, interoperable CCSDS 235.1-W-0.4 reference implementation by 31 Aug 2026, optimized for:

- Protocol correctness first
- Measurable interoperability
- Sub-1 ms encode/decode and frame processing
- Sub-50 KB session overhead
- Reproducibility and strong documentation

## 1.1 Strategic Path Rationale

- The chosen path prioritizes correctness first because Protocol Correctness has the largest scoring weight (40 points).
- Performance work starts early (not late) because deterministic latency and memory behavior require architectural choices, not only final optimization.
- Interoperability rehearsals are scheduled before final submission to avoid discovering wire-format mismatches when schedule slack is gone.

## 1.2 Language and Stack Recommendation

Primary recommendation:

- Rust for protocol core and session/state components.

Why Rust is recommended:

- Deterministic memory behavior without GC pauses supports latency and memory targets.
- Strong type system and ownership model reduce classes of runtime defects in stateful protocol code.
- Zero-cost abstractions allow clean architecture without paying runtime penalties in hot paths.
- Excellent test tooling (`cargo test`, property testing, benchmark ecosystem) supports evidence-heavy scoring categories.

Fallback recommendation:

- C++20 if team has deeper C++ performance expertise and can enforce strict memory discipline.

Why fallback is second choice:

- C++ can match performance but typically needs more process rigor to avoid memory safety regressions during rapid iteration.

Not recommended for protocol hot path:

- GC/interpreted stacks for frame/SPDU critical path.

Why not:

- Non-deterministic pauses and allocator behavior can complicate under-1 ms guarantees and long-duration stress reliability.

## 2. Team Operating Model

Role placeholders:

- Engineer A: Systems/Protocol Lead
- Engineer B: V&V and Performance Lead
- Engineer C: Interoperability and Operations Lead
- Developer A: Core Protocol Developer
- Developer B: Infrastructure/Tooling Developer
- Developer C: Integration and Automation Developer

### Responsibility Split

- Engineer A
     - Owns interpretation of CCSDS sections and decision log
     - Owns state machine correctness and interface definitions
     - Co-owns workshop and interoperability readiness
- Engineer B
     - Owns test strategy, coverage model, benchmark design
     - Owns conformance matrix and PICS draft
     - Co-owns risk and failure-mode analysis
- Engineer C
     - Owns workshop interoperability operations and external exchange readiness
     - Owns runbook quality, operator workflows, and incident triage process
     - Co-owns release readiness and submission packaging checks
- Developer A
     - Owns SPDU, COP-P core implementation, and parser rigor
     - Owns serialization/deserialization correctness and byte order utilities
- Developer B
     - Owns frame layer implementation, physical channel adapters, CI/CD, reproducible builds, docs tooling
- Developer C
     - Owns integration harnesses, failure-injection tooling, and regression automation
     - Owns benchmark orchestration, artifact generation tooling, and verification scripts

## 3. Cadence and Governance

- Planning: weekly Monday 45 minutes
- Daily sync: 15 minutes
- Spec alignment review: twice weekly (Engineer A leads)
- Test and benchmark review: weekly (Engineer B leads)
- Interoperability rehearsal review: weekly (Engineer C leads)
- Gateway readiness review: 48 hours before each gateway close
- Decision records: all non-trivial choices captured in ADR format

Why this cadence:

- Short daily syncs keep integration risk low without reducing implementation time.
- Twice-weekly spec reviews prevent drifting interpretations from becoming expensive rework.
- ADR discipline captures rationale, enabling faster review and conformance justification.

## 4. Quality Gates (apply to every phase)

- No merge without tests for changed behavior
- No open severity-1 bug older than 48 hours
- Coverage target trendline: >= 90% unit coverage by Gateway 8
- Deterministic behavior required in core protocol tests
- Public interfaces documented before phase close

Why these gates:

- They prevent hidden quality debt from crossing gateways.
- They tie day-to-day development decisions directly to judged outcomes (correctness, conformance, reproducibility).

## 5. Branching and Release Strategy

- `main`: releasable baseline
- `develop`: integration branch
- `feature/<gateway>-<topic>` branches
- Release tags:
     - `g0-arch`, `g1-spdu`, `g2-copp`, `g3-frame`, `g4-physical`, `g5-state`, `g6-session`, `g7-int`, `g8-conf`, `g9-interop`, `g10-docs`

## 6. Risk Register (initial)

- Ambiguity in spec interpretation
     - Mitigation: formal decision log + test vectors tied to sections
- Performance regressions near final phase
     - Mitigation: benchmark in CI from Gateway 1 onward
- Interoperability surprises
     - Mitigation: workshop artifact generation and decode replay tests
- Over-focus on implementation, under-focus on docs
     - Mitigation: documentation treated as parallel workstream every phase

## 7. Deliverable Control Board

Track each gateway with:

- Scope committed
- Scope delivered
- Validation checklist pass/fail
- Blockers and mitigations
- Evidence links (tests, benchmarks, reports)

## 8. Score Maximization Strategy

- 40 points correctness: strict section-to-test traceability
- 25 points testing/conformance: build conformance matrix early
- 15 points interoperability: artifact rehearsals and exchange drills
- 10 points performance: continuous microbench and stress profiles
- 10 points documentation: complete API, user, architecture docs with runnable examples

Why this score strategy works:

- It maps engineering effort to scoring weights, ensuring team time is spent where points are concentrated.
- It reduces the risk of over-investing in low-weight categories before high-weight mandatory evidence is stable.

## 9. Immediate Next 7 Days (from upcoming meeting)

- Day 1-2: finalize stack, architecture boundaries, repository scaffolding
- Day 3-4: define SPDU data model and endian utility API
- Day 5: bring up test harness with golden vectors
- Day 6: set benchmark harness and memory measurement method
- Day 7: Gateway 0 readiness review and adjustments

Rationale for the first 7 days:

- Days 1-2 establish constraints first so implementation choices remain consistent under pressure.
- Days 3-4 define data model and byte-order contract early because these choices affect every downstream gateway.
- Days 5-6 front-load verification infrastructure so performance and correctness regressions are visible immediately.
- Day 7 institutionalizes review before coding velocity increases, reducing avoidable architecture churn.

## 10. Why This Gateway Order

- Gateway 0 before all others: architecture and test harness are forcing functions for disciplined execution.
- Gateway 1 before Gateway 2: COP-P correctness depends on trusted SPDU encode/decode semantics.
- Gateway 2 before Gateway 3: frame behavior must encapsulate already-correct protocol data units.
- Workshop buffer before session control: external interoperability confidence should be established before introducing higher-level session complexity.
- Gateways 7-10 grouped at end but developed continuously: final phase is for hardening and evidence consolidation, not first-time creation.

[⬆️ Back to Top ⬆️](#program-execution-plan-team-of-6)

---

[Back Page](./README.md) | [Next Page](./Phase-1-Foundation-Core-Protocol.md)
