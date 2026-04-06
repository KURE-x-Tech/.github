# IDA-02 Layered Architecture

## 1. Architecture View

Target layered model:

1. Application/API layer
2. Session control layer (hailing, lifecycle, reconnect)
3. State machine layer (duplex mode transitions)
4. COP-P layer (FOP-P, FARM-P, coordinator)
5. SPDU layer (encode/decode, directives, PLCW)
6. Frame layer (P/U frame handling, QoS, versioning)
7. Physical channel abstraction (mock + TCP/UDP)

Architecture rationale:

- This layering isolates concerns so protocol correctness can be verified independently from transport and application concerns.
- It also enables deterministic unit testing at lower layers while preserving realistic integration paths at higher layers.

## 2. Module Decomposition

For each module record:

- Purpose
- Inputs/outputs
- Deterministic behavior constraints
- Failure modes and error signaling
- Performance-critical paths

## 3. Data Flow

Document:

- Tx path from application payload to wire frame
- Rx path from wire frame to protocol events
- State update points and sequencing dependencies

Data-flow rationale (fill this section):

- Explain why each state update point is placed where it is.
- Explain how sequencing prevents race conditions and replay/ordering defects.

## 4. Concurrency Model

Define:

- Session concurrency strategy
- Synchronization boundaries
- Ownership/lifetime model for buffers and state
- Deadlock and race prevention strategy

## 5. Memory and Performance Strategy

- Pre-allocation policy
- Buffer reuse approach
- Avoided patterns (runtime allocation in hot paths)
- Measurement method for per-session overhead

Performance rationale (fill this section):

- Explain why chosen allocation and reuse strategy is expected to satisfy <50 KB/session.
- Explain why hot-path design is expected to satisfy <1 ms latency objectives.

## 6. Architecture Decisions (ADR links)

List key ADRs:

- Language and runtime choice
- Serialization strategy
- State transition representation
- Interface abstraction trade-offs

[⬆️ Back to Top ⬆️](#ida-02-layered-architecture)

---

[Back IDA](./IDA-01-System-Context-and-Requirements.md) | [Next IDA](./IDA-03-Interfaces-and-Wire-Format.md)
