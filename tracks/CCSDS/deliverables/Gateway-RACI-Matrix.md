# CCSDS Gateway RACI Matrix (6-Person Team)

Roles:

- Engineer A: Systems and Protocol Lead
- Engineer B: V&V and Performance Lead
- Engineer C: Interoperability and Operations Lead
- Developer A: SPDU and COP-P Core
- Developer B: Frame, Transport, and Tooling
- Developer C: Integration, Benchmarks, and Automation

Legend:

- R = Responsible
- A = Accountable
- C = Consulted
- I = Informed

## Gateway Ownership Matrix

| Gateway | Scope                       | Engineer A | Engineer B | Engineer C | Developer A | Developer B | Developer C |
| ------- | --------------------------- | ---------- | ---------- | ---------- | ----------- | ----------- | ----------- |
| G0      | Design and architecture     | A          | C          | C          | R           | R           | R           |
| G1      | SPDU layer                  | A          | C          | I          | R           | C           | R           |
| G2      | COP-P layer                 | A          | A          | C          | R           | C           | R           |
| G3      | Frame layer                 | C          | C          | A          | C           | R           | R           |
| G4      | Physical abstraction        | C          | A          | A          | I           | R           | R           |
| G5      | State machine               | A          | A          | C          | R           | C           | R           |
| G6      | Hailing and session control | A          | C          | A          | R           | R           | C           |
| G7      | Integration testing         | C          | A          | A          | R           | R           | R           |
| G8      | Conformance and PICS        | C          | A          | C          | R           | R           | R           |
| G9      | Interoperability testing    | C          | C          | A          | R           | R           | R           |
| G10     | Documentation and examples  | A          | A          | A          | R           | R           | R           |

## RACI Rationale

- Protocol interpretation and normative fidelity stay anchored to Engineer A across protocol-centric gateways.
- Verification accountability remains with Engineer B where scoring depends on measurable compliance.
- Engineer C is accountable for interop and operations-heavy gateways to ensure exchange readiness and execution quality.
- Developers remain responsible across all gateways to keep implementation and evidence generation synchronized.

## Usage Rules

- Gateway closure requires accountable sign-off and evidence links from all responsible owners.
- Ownership changes require explicit review-note updates in weekly gateway governance.

[⬆️ Back to Top ⬆️](#ccsds-gateway-raci-matrix-6-person-team)

---

[Back Page](./Phase-4-Validation-Documentation.md) | [Next Page](./README.md)
