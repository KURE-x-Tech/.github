# Gateway 0 Execution Plan: Design & Architecture for LSIS-AFS Reference Implementation

> [!NOTE]
> Back to the [Main Page](../README.md)

## 1. Gateway 0 Strategic Overview

Gateway 0 constitutes the mandatory architectural alignment between the LSIS-AFS Volume A physical layer requirements and the software execution environment. It is not merely a preparatory phase; it is the foundational blueprint that shall govern every aspect of the software implementation. This phase provides the critical link between high-level specification and functional software, ensuring that the reference implementation is capable of eventually interfacing with the European Space Tracking network (ESTRACK) and the Mission Control Centres (ESOC) defined in the NASA/ESA ground segment infrastructure.

The core mission for Month 1 is the establishment of the implementation framework and compliance baseline. This design-first approach mandates that all module boundaries and technology choices be finalized before a single symbol is modulated. By defining these "Natural integration points," Gateway 0 secures the project against architectural drift and ensures the system remains "real-time capable," processing 12-second frames in under one second.

### Gateway 0 Summary Table

| Attribute            | Mandated Detail                                                                                                                               |
| -------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| Primary Objective    | Establishment of the implementation framework, directory structure, and compliance baseline.                                                  |
| Target Completion    | End of Month 1 (of the 6-month development cycle).                                                                                            |
| Scoring Significance | Gateway 0 provides the "Partial Credit" logic by establishing independent, testable blocks; failure here invalidates all subsequent gateways. |

The following technical outputs are required to transition from design to the high-priority execution of core signal generation.

## 2. Gateway 0: The Eight Mandatory Deliverables

A modular, document-first approach is mandated for high-assurance aerospace software. This strategy ensures that independent DSP and messaging components can be developed and validated in parallel, minimizing integration errors at the ground station interface.

- System Architecture Document: Detailed mapping of the lsis-afs/ directory tree (including codes/, encoding/, messages/, signal/, and utils/). So What?: Establishing a rigid structure prevents cross-module pollution and ensures that utilities like codes_db are available to both the encoder and decoder pipelines.
- Technology Stack Selection: Finalized choice of language (C, Rust, Python, Go, or Java) and build system (CMake, Cargo, etc.). So What?: This selection determines the "real-time capability" of the system, mandating that the chosen stack can achieve the 100ms encoding threshold without runtime overhead.
- Interface Definition Document (IDD): Rigorous specification of module boundaries for codes/, encoding/, and signal/. So What?: By freezing the binary signatures for these modules, DSP engineers and message-parsing teams can develop concurrently without dependency bottlenecks, ensuring the 6-month cycle is met.
- Testing & Validation Framework: Selection of automated tools (e.g., CTest, pytest) for unit and integration testing. So What?: This framework provides the "fail-fast" mechanism required to validate implementation against Annex 3 reference codes and Legendre sequence lengths.
- Reference Data Integration Plan: Procedures for the ingestion of Annex 1 (LDPC matrices) and Annex 3 (Reference codes). So What?: Automated parsing of these constants eliminates human error in the mathematical foundations of the signal, ensuring 100% parity with LSIS-AFS Volume A.
- Development Environment Configuration: Containerized environment specifications (e.g., Docker) to ensure cross-platform parity. So What?: This guarantees that the reference implementation behaves identically across Linux x86_64, macOS ARM64, and Windows, a prerequisite for global LunaNet interoperability.
- Version Control & CI/CD Strategy: Branching logic and automated pipelines for parallel component development. So What?: Automated validation of every PR against the Requirement Traceability Matrix ensures that new code does not break existing LSIS compliance.
- Requirement Traceability Matrix: Direct mapping of implementation modules to LSIS IDs (e.g., LSIS-160 for code sync, LSIS-467 for CRC). So What?: This matrix provides the definitive proof of compliance required for software to be certified as a "Reference Implementation" by the Standards Committee.

These deliverables define the technical boundaries that shall govern the software execution life-cycle.

## 3. Technical Constraints and Technology Stack Strategy

Constraints are the primary drivers of interoperability within the LunaNet ecosystem; they enforce the performance and standardization required for deep-space communication.

### Constraints vs. Implementation Strategy

| Constraint ID | Requirement                  | Prescriptive Implementation Strategy                                                                                                                                             |
| ------------- | ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| TC-1          | Language & Build System      | Evaluation shall prioritize memory-safe languages (Rust) or optimized C to ensure the 100ms encoding target is not compromised by garbage collection overhead.                   |
| TC-2          | Open-Source Library Policy   | All third-party libraries must be documented in the Submission Report; use shall be restricted to ensure the "stand-alone" status of the reference implementation is maintained. |
| TC-3          | Standardized Data Formats    | Implementation shall mandate Binary I/Q (int16/float32) and MSB-first bit/byte ordering per Phase 7.3 to ensure endianness compatibility with ESA/NASA ground stations.          |
| TC-4          | Cross-Platform Compatibility | Validation is mandated across Linux, macOS, and Windows to ensure reference implementation ubiquity for all LunaNet participants.                                                |

Adhering to these strategies eliminates architectural drift and ensures the final product remains a viable reference for the global space community.

## 4. Validation Checklist and Success Criteria

Gateway 0 operates on a "fail-fast" philosophy, where design integrity must be verified before Phase 1 core spreading code generation begins.

### Gateway 0 Validation Checklist

- [ ] Development Environment: Verified compilers, build systems, and dependency managers established and documented in the Submission Report.
- [ ] Architecture Clarity: Completed lsis-afs/ tree with empty "stubs" for all Phase 1-4 modules, including tiered/, bch/, and ldpc/.
- [ ] Reference Alignment: Ingestion scripts for Annex 1 LDPC matrices and Annex 3 codes verified against reference lengths (e.g., Legendre sequence length 10223).
- [ ] Data Integrity: Checksums of ingested LDPC submatrices (A, B, C, D, B⁻¹) verified against the Volume A specification.
- [ ] Testing Readiness: "Hello World" integration test passing within the selected framework.

### Architectural Performance Targets (Non-Functional Requirements)

The design must accommodate the following NFRs to meet competition benchmarks:

- Encoding Efficiency: < 100ms per 12-second frame.
- Real-time Factor: > 1.0x (Processing time must be less than signal duration).
- Code Synchronization: Design shall accommodate alignment requirements for LSIS-160, 170, 171, and 220.
- Sync Reliability: > 99% reliability at SNR > 0 dB in the synchronization detection module.

Successful completion of this checklist signals readiness for immediate signal-in-space execution.

## 5. Phase 1 First Steps: Immediate Execution Roadmap

The first 72 hours of execution (Sprint 0) shall focus on the tactical mobilization of the core signal generation pipeline.

### Sprint 0 Checklist (Immediate Actions)

1. Repository Initialization: Establish the mandated lsis-afs/ directory structure:

- lsis-afs/codes/ (gold, weil, legendre, tiered)
- lsis-afs/encoding/ (bch, ldpc, crc, interleaver)
- lsis-afs/messages/ (sb1, sb2, sb3, sb4, frame)
- lsis-afs/signal/ (modulator, sync)
- lsis-afs/utils/ (sise, time, codes_db)

2. Annex Data Ingestion: Develop the codes_db utility to parse Annex 3 reference codes and Annex 1 LDPC submatrices (A, B, C, D, B⁻¹) for SB2 and SB3/4.
3. PRN Mapping Baseline: Implement the master Look-Up Table (LUT) for PRN 1-210.

- AFS-I Primary: Gold codes (2046 chips).
- AFS-Q Primary: Weil codes (10230 chips).

4. LFSR Prototyping: Define the 11-stage LFSR interface for the Gold Code Generator, ensuring feedback taps are implemented exactly as specified in Phase 1.1.
5. Module Interfacing: Finalize binary function signatures for the modulator and interleaver to permit parallel development between the signal and encoding teams.

The Gateway Principle is absolute. Successful completion of Gateway 0 is the only path to guaranteeing the integrity of the 12-second composite code sequence. All timing, spreading, and synchronization shall be perfectly aligned with LunaNet standards to ensure mission-critical interoperability.
