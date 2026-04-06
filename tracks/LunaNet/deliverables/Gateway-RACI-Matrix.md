# LunaNet Gateway RACI Matrix (6-Person Team)

Roles:

- Engineer A: Systems and Spec Lead
- Engineer B: V&V and Performance Lead
- Engineer C: Interoperability and Operations Lead
- Developer A: Code and Encoding Core
- Developer B: Frame and Signal Core
- Developer C: Decode/Parser and Automation Core

Legend:

- R = Responsible
- A = Accountable
- C = Consulted
- I = Informed

## Gateway Ownership Matrix

| Gateway | Scope                        | Engineer A | Engineer B | Engineer C | Developer A | Developer B | Developer C |
| ------- | ---------------------------- | ---------- | ---------- | ---------- | ----------- | ----------- | ----------- |
| G0      | Architecture and environment | A          | C          | C          | R           | R           | R           |
| G1      | Code generation              | A          | C          | I          | R           | C           | R           |
| G2      | Encoding pipeline            | C          | A          | I          | R           | R           | C           |
| G3      | Frame structure              | A          | C          | C          | C           | R           | R           |
| G4      | Signal generation            | C          | A          | C          | I           | R           | R           |
| G5      | Sync and decoding            | C          | A          | C          | R           | I           | R           |
| G6      | Parsing and timing           | A          | C          | C          | I           | R           | R           |
| G7      | Integration and interop      | C          | A          | A          | R           | R           | R           |
| G8      | Documentation and submission | A          | A          | A          | R           | R           | R           |

## RACI Rationale

- Accountable roles emphasize independent technical authority: spec fidelity (Engineer A), measurable quality (Engineer B), and interoperability execution (Engineer C).
- Responsible assignments are distributed across all developers each gateway to avoid single-thread delivery risk.
- Integration and submission gateways intentionally use shared accountability because defects in these stages are cross-functional by nature.

## Usage Rules

- Any gateway entering review must have one accountable sign-off and all responsible owners reporting status with evidence links.
- Changes to ownership require documented reason and reviewer approval in weekly governance review.

[⬆️ Back to Top ⬆️](#lunanet-gateway-raci-matrix-6-person-team)

---

[Back Page](./Phase-3-Integration-Submission.md) | [Next Page](../requirements/LunaNet-Requirements-Traceability-Plan.md)
