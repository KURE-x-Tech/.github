# Gateway 1 Implementation Plan: LSIS-AFS Reference Architecture

> [!IMPORTANT]
> Back to the [Main Page](../README.md)

## 1. Strategic Transition: Moving from Gateway 0 Design to Gateway 1 Implementation

The evolution from the Gateway 0 "Blueprint" to the Gateway 1 "Operational Reference Implementation" represents a mandatory shift from theoretical interoperability requirements to implementable software. While Gateway 0 established the structural rules, Gateway 1 serves as the execution layer for the Lunar Augmented Navigation Service (LANS). This transition is the critical path for the mission; moving from high-level specifications into functional code for Signal-in-Space (SIS) generation and decoding is foundational for the cislunar PNT environment.

The previous architectural phase carried a 20% scoring weight, reflecting the necessity of a design that prevents cascading errors in high-complexity features. By moving to the execution layer in Gateway 1, we are enforcing a strict architectural mandate: the isolation of the LDPC encoding logic from the Physical Layer modulation. This isolation ensures that bit-level errors in message framing (Annex 3) do not compromise carrier synchronization or the integrity of the PN spreading codes.

| Milestone                | Target Date / Period | Primary Reference  | Strategic Focus                          |
| ------------------------ | -------------------- | ------------------ | ---------------------------------------- |
| Gateway 0 Completion     | April 15, 2026       | LNIS Volume A      | Baseline Design & System Architecture    |
| Gateway 1 Initialization | Post-April 15, 2026  | LNIS Volume A      | Software Initialization & Data Ingestion |
| Goonhilly Workshop       | June 2026            | LSIS-AFS Standards | Collaborative Dev & Expert Validation    |
| Final Submission         | August 31, 2026      | LNIS Volume A      | Standards Compliance & Prototyping       |

This strategic framework transitions directly into the specific repository initialization and data ingestion requirements.

## 2. Phase I: Repository Initialization and Reference Data Ingestion

A stable, version-controlled environment is a non-negotiable foundation for multi-agency collaborative development. Given the complexity of the LSIS-AFS, a consistent development baseline is mandatory to ensure reproducibility and to prevent integration conflicts during parallel module development.

Step 1: Version Control and Repository Initialization The project must utilize a Git-based repository with a directory structure that isolates functional blocks. The mandatory structure is as follows:

- /core_signal_processing: The high-performance C++ engine for I/Q sample generation and parallelized bit manipulation.
- /message_framing: Logic for navigation message construction, LDPC encoding, and sync word insertion.
- /unit_tests: Automated validation scripts to ensure individual function performance thresholds.

Step 2: Reference Data Loading from Annex 1 and 3 Implementation requires a modular ingestion layer for Annex 1 (Signal-in-Space parameters) and Annex 3 (Navigation Messages). The system must accommodate a 12-second frame structure and 20-minute block intervals. The 20-minute block interval is the primary driver for a non-negotiable Circular Buffer design within the repository; the system must hold and process these large data chunks to maintain the integrity of the Clock and Ephemeris Data (CED). This modularity allows the implementation to remain compliant while mapping varied provider parameters into a standardized AFS frame.

### Mandatory Technical Constraints

The development environment must be certified against the following constraints:

- TC-1 (Languages): High-performance C++ for the core signal processing engine; Python for orchestration and testing.
- TC-2 (Libraries): Exclusive use of open-source components, specifically GNU Radio, VOLK, and GPS-SDR-SIM.
- TC-3 (Data Formats): Binary I/Q for signals, JSON for configuration, and CSV for structured ephemeris data.
- TC-4 (Compatibility): Full cross-platform support (Linux and Windows) with POSIX compliance and ARM compatibility for future flight payloads.

This data ingestion layer provides the variables required for the subsequent code spreading logic.

## 3. Phase II: Spreading Module Implementation (Gold, Weil, and Hierarchical Codes)

The Spreading Module is a mandatory requirement for enabling signal acquisition across diverse receiver types. The architecture must support everything from high-fidelity agency systems to low-SWaP amateur payloads.

### Spreading Sequence Logic (Step 3)

In accordance with "Stage 1: Satellite-Specific Processing," the implementation must execute the following spreading sequences:

- Gold/PN Sequences: The generator must produce In-phase (I) and Quadrature (Q) PN codes at a rate of 1.023 MHz using specified polynomials for satellite identification.
- Weil/Data Spreading: Data symbols modulated at 500 sps must leverage the GPS L1C-derived architecture, providing the necessary throughput for lunar PNT users.
- Hierarchical Secondary Codes: These codes are mandatory on the Pilot channel to facilitate signal integration and enhance tracking sensitivity, allowing receivers to integrate energy over longer durations to acquire weak signals in the lunar environment.

### Mathematical Model for Spreading

The generation of the total AFS signal s(t) must adhere to the following model, with the PN code rate strictly synchronized to the 2492.028 MHz center frequency:

$$s(t) = \sqrt{2P_I} D_{AFS-I}(t) C_{AFS-I}(t) \cos(2\pi f_c t + \theta) + \sqrt{2P_Q} C_{AFS-Q}(t) \sin(2\pi f_c t + \theta)$$

Where $C_{AFS-I}$ and $C_{AFS-Q}$ represent the 1.023 Mcps spreading codes and $f_c$ is the carrier frequency. This generation phase transitions into physical layer modulation and signal accumulation.

## 4. Phase III: Baseband Modulation and Signal Accumulation

Real-time SIS generation requires a "two-stage parallelization strategy" to maintain computational efficiency. By decoupling satellite-specific processing from signal accumulation, the system can simulate multiple nodes without exceeding CPU limits.

### Baseband Module Requirements

The Baseband Module must perform BPSK(1) modulation. This is a non-negotiable departure from terrestrial BOC modulation. The implementation must accumulate independent I/Q sample streams into a single composite signal. Furthermore, the signal must be generated using Right-Hand Circular Polarization (RHCP) as an architectural mandate for interoperability.

To ensure the reference implementation remains hardware-agnostic and capable of streaming to SDR hardware (e.g., USRPs), the use of the Vector-Optimized Library of Kernels (VOLK) for SIMD optimization is mandatory. This ensures real-time performance on general-purpose hardware.

### Synchronization and Baseline Signal Generator (Step 4)

The implementation must include a 68-symbol sync word. This is critical for low-complexity receivers that lack the resources to track the pilot channel or perform immediate LDPC decoding. The sync word enables these receivers to acquire the signal and establish timing using only the data channel, even at the low power levels expected at the lunar surface.

## 5. Phase IV: Interoperability and Reference Frame Integration

Adherence to common reference systems is a non-negotiable requirement of the LunaNet "Network of Networks" concept. Without strict alignment, navigation data from disparate agencies cannot be aggregated.

### Coordinate and Time System Mapping (Step 5)

The implementation must integrate the following three reference systems:

1. Lunar Celestial Reference System (LCRS): The Moon-centered inertial reference system.
2. Principal Axis (PA) System: The body-fixed system for the Moon.
3. LunaNet Reference Time (LRT): Traced to UTC.

### Provider Plugin Pattern

To manage the flexibility of CED formats, the architecture must utilize a Provider Plugin pattern. This allows the software to ingest and map specific message formats from NASA’s LCRNS, ESA’s Moonlight, and JAXA’s LNSS while maintaining a common underlying signal structure.

### Orbital Dynamics for ELFO

The implementation must handle the unique dynamics of Elliptical Lunar Frozen Orbits (ELFO). Because ELFO is highly eccentric, the Doppler shift (\Delta f) is not constant. The propagator must handle rapidly changing relative velocities that far exceed terrestrial GNSS dynamics:

$$\Delta f = \frac{v_{rel}}{c} f_c$$

## 6. Validation, Verification (V&V), and Risk Mitigation

Rigorous V&V is required to ensure Signal-In-Space Error (SISE) compliance and the safety of human missions.

### Success Criteria Checklist

| Criterion                 | Validation Method            | Target Metric                                           |
| :------------------------ | :--------------------------- | :------------------------------------------------------ |
| **Signal Quality**        | **BER and SNR Calculation**  | **Power: -163 dBW; Polarization: RHCP**                 |
| **Frame Sync**            | **Sync Word Validation**     | **100% acquisition by low-complexity receiver mock-up** |
| **Environment Readiness** | **CI/CD Pipeline Execution** | **100% build/unit test success on all commits**         |
| **Standards Compliance**  | **Waveform Verification**    | **Center Frequency: 2492.028 MHz; PN Rate: 1.023 MHz**  |

### Risk Mitigation Strategy

- Computational Load: Mitigation via mandatory use of VOLK for SIMD optimization and multi-core parallelization.
- Standards Drift: Mitigated by a modular design that separates the message frame generator from the modulation modules.
- Signal Weakness: Addressed by the implementation of hierarchical secondary codes on the pilot channel to increase integration time for deep-space acquisition.

This Implementation Plan fulfills the LunaNet Interoperability Specification (LNIS) and provides the operational framework necessary to support the "Moon to Mars" vision.
