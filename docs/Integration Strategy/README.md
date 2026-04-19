# Integration Strategy Report: LunaNet LSIS-AFS into Mission Control Architectures

> [!NOTE] Back to Home
> [Click Here](../README.md)

Table of Content:

- [Integration Strategy Report: LunaNet LSIS-AFS into Mission Control Architectures](#integration-strategy-report-lunanet-lsis-afs-into-mission-control-architectures)
     - [Strategic Context and Integration Objectives](#strategic-context-and-integration-objectives)
     - [Analysis of Ground Segment Infrastructure and Subsystems](#analysis-of-ground-segment-infrastructure-and-subsystems)
     - [Functional Evaluation of LunaNet LSIS-AFS Implementation](#functional-evaluation-of-lunanet-lsis-afs-implementation)
          - [Digital Baseband Signal Generation](#digital-baseband-signal-generation)
          - [Message Encoding and Decoding](#message-encoding-and-decoding)
          - [Frame Structure and Synchronization](#frame-structure-and-synchronization)
     - [Integration Strategy: Mapping LunaNet Gateways to MCC Subsystems](#integration-strategy-mapping-lunanet-gateways-to-mcc-subsystems)
     - [Algorithmic Compatibility and Operational Requirements](#algorithmic-compatibility-and-operational-requirements)
     - [Integration Roadmap and Validation Framework](#integration-roadmap-and-validation-framework)
          - [Implementation Roadmap](#implementation-roadmap)
          - [Success Criteria for Operational Integration](#success-criteria-for-operational-integration)

## Strategic Context and Integration Objectives

The transition of lunar exploration from isolated missions to a sustained presence requires a fundamental shift in Ground Segment philosophy. We must move beyond traditional point-to-point deep space tracking and adopt a robust, interoperable network architecture. Integrating the LunaNet Signal-In-Space Augmented Forward Signal (LSIS-AFS) into existing Mission Control Centre (MCC) frameworks is a strategic imperative to bridge this gap. This integration ensures seamless cross-agency interoperability, specifically between ESA and NASA nodes, allowing our tracking networks to provide the high-fidelity navigation and timing data required for the LunaNet framework. By shifting toward software-defined digital signal processing (DSP), we enable a modular transition that maintains heritage reliability while meeting the complex multi-node requirements of the lunar environment.

The technical objectives for this integration, derived from the LSIS-AFS requirements, are as follows:

- Spreading Code Implementation: Deployment of software-defined generators for Gold, Weil, and Legendre sequences to facilitate multi-user signal acquisition.
- Digital Baseband Generation: Production of complex I/Q samples for AFS-I and AFS-Q signals at their respective chip rates of 1.023 Mchip/s and 5.115 Mchip/s.
- Advanced Forward Error Correction (FEC): Implementation of BCH(51,8) and Rate 1/2 LDPC encoding/decoding to maintain link integrity in high-path-loss scenarios.
- Message Orchestration: Developing high-fidelity builders and parsers for structured navigation messages including Ephemeris, Clock, and Network Access data.
- Precision Utilities: Algorithmic reconstruction of Time of Transmission (ToT) and Signal-In-Space Error (SISE) calculation for operational safety.
- Test Vector Generation and Validation: Establishing a rigorous validation framework to cross-check implementation against Annex 3 reference standards.

This report evaluates the intersection of these requirements with current Ground Segment infrastructure to define a path toward full LunaNet compliance.

## Analysis of Ground Segment Infrastructure and Subsystems

In a standard Ground Segment architecture, the Mission Control Centre (MCC) functions as the central nervous system, managing the bidirectional flow of Telemetry and Telecommand (TM/TC). Successful LSIS-AFS integration depends on aligning these new digital signal capabilities with the existing offline and online components of the mission control chain.

The following table evaluates the core MCC subsystems defined in the "AE6030 - Mission Operations" framework and their roles in managing space-to-ground links:

| Subsystem                               | Core Functional Role                                                                                                               |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| Spacecraft Control System (SCS)         | Manages the online component, driving the ground station and processing real-time telemetry/commands.                              |
| Ground Station Network Interface (GSNI) | Acts as the interface between the MCC and the tracking network, facilitating complex data exchange.                                |
| Automation System                       | Orchestrates the SCS and GSNI to execute mission tasks and contact sequences without manual intervention.                          |
| Flight Dynamics System (FDS)            | Maintains spacecraft orbit models, providing essential propagated orbit data and navigation products.                              |
| Database Preparation System (DPS)       | Consolidates, updates, and validates the spacecraft database (MIB), ensuring the ground system’s telemetry dictionary is accurate. |

Physical connectivity is provided by the European Space Tracking (ESTRACK) network, which currently operates 9 stations across 6 countries. These stations are hubbed through the European Space Operations Centre (ESOC) in Darmstadt, Germany. The network’s three Deep Space Antennas are critical for LSIS-AFS, as they possess the sensitivity required to up-link commands and down-link high-volume scientific data from lunar distances. These antennas provide the raw ranging observations that the FDS processes to maintain orbital accuracy.

As we shift toward software-defined ground stations, these subsystems must evolve from handling fixed RF hardware to managing dynamic, software-generated signal parameters.

## Functional Evaluation of LunaNet LSIS-AFS Implementation

The LSIS-AFS specification utilizes a modular 8-gateway approach for implementation. As an architectural strategy, this tiered validation minimizes risk by isolating signal processing, encoding, and message parsing into testable units before full-system integration.

The core software-defined components are deconstructed as follows:

### Digital Baseband Signal Generation

The system generates AFS-I and AFS-Q baseband signals via complex I/Q sampling. AFS-I utilizes a Gold code (2046 chips) at 1.023 Mchip/s, while AFS-Q utilizes a Weil code (10230 chips) at 5.115 Mchip/s. The implementation requires tiered code assembly, combining primary, secondary, and tertiary codes, to ensure coherent generation without chip slips. This coherency is vital for stable phase tracking at the receiver.

### Message Encoding and Decoding

Data integrity is maintained through a tiered FEC architecture. Subframe 1 (Navigation Data) utilizes BCH(51,8) encoding, while Subframes 2 through 4, containing Clock, Ephemeris, and Network Access data, use LDPC (Rate 1/2) supplemented by CRC-24 for error detection. This ensures robust link performance in the low SNR conditions characteristic of deep space.

### Frame Structure and Synchronization

The signal is organized into 12-second frames, each initiated by a 68-symbol unencoded synchronization pattern (0xCC63F74536F49E04A). Operating at 500 symbols/s, this rigid timing allows the ground system to maintain sub-frame resolution for precise timing reconstruction.

The completion of Phases 1 through 4 represents a significant architectural shift away from hardware-centric RF models. By moving these functions into the software domain, we enable more flexible, cloud-ready mission control architectures capable of rapid adaptation to evolving LunaNet standards.

## Integration Strategy: Mapping LunaNet Gateways to MCC Subsystems

Successful integration requires a precise mapping between the LunaNet software gateways and established MCC subsystems to ensure data integrity and low-latency processing.

| LunaNet Functional Gateway    | MCC Subsystem Alignment          | Strategic "So What?"                                                                                                                                               |
| ----------------------------- | -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Message Builder / Parser      | SCS & DPS (MIB)                  | The Parser extracts telemetry fields; aligning this with the DPS ensures that the MIB (Spacecraft Database) accurately reflects LunaNet-defined navigation fields. |
| Encoder / Baseband Generation | GSNI                             | Digital baseband generation at 1.023/5.115 Mchip/s must interface directly with GSNI digital signal equipment for immediate uplink preparation.                    |
| ToT Calculation               | Flight Dynamics System (FDS)     | LSIS time reconstruction based on the LRT epoch provides the high-precision timestamps required for FDS orbit modeling and propagation.                            |
| SISE Calculation              | Flight Operations Planning (FOP) | SISE position (40m) and velocity (1 cm/s) limits act as safety triggers for FOP, determining the reliability of navigation links for mission planning.             |

By aligning the Message Parser with the Spacecraft Database (MIB), we ensure that navigation message fields (Ephemeris/Clock) are seamlessly integrated into the mission control telemetry dictionary, preventing data silos between navigation and control.

## Algorithmic Compatibility and Operational Requirements

Interoperability across international lunar nodes requires strict algorithmic compliance with LSIS-AFS specifications. The following analysis details the implementation requirements for operational tracking:

- Spreading Code Generation (Gold/Weil): Implementation involves 11-stage LFSRs for Gold codes and quadratic residue calculations for Weil sequences. These provide 210 unique PRN sequences, essential for mitigating multi-user interference as lunar orbital traffic increases.
- Forward Error Correction (LDPC/BCH): The LDPC implementation utilizes the Belief Propagation (sum-product algorithm) for high-sensitivity decoding. The BCH(51,8) encoder employs an 8-stage LFSR. These choices maximize gain, targeting a Bit Error Rate (BER) < 10^{-5} at SNR levels near 0 dB.
- Frame Sync and Correlation: Unlike hardware-fixed correlators, this software-defined approach utilizes soft-decision correlation for the BCH decoder and cross-correlation for the 68-symbol sync pattern. This provides the robustness needed to handle the significant Doppler shifts and timing offsets inherent in lunar trajectories.

These algorithms provide the mathematical rigor required for a reliable, multi-agency lunar communications infrastructure.

## Integration Roadmap and Validation Framework

Maintaining high-availability mission operations during the transition to LunaNet requires a phased deployment strategy. Following the 8-Gateway modularity allows for systematic testing before operational cutover.

### Implementation Roadmap

1. Design & Architecture: Finalize stack, interface definitions, and GSNI integration points.
2. Foundation: Implement spreading code generators (Gold, Weil) and BCH LFSR modules.
3. Encoding & Messaging: Deploy LDPC (Belief Propagation) and CRC-24 modules.
4. Signal Generation: Finalize I/Q sample pipeline and tiered code coherency.
5. Decoding & Parsing: Implement frame sync, message parsers, and ToT reconstruction.
6. Testing & Polish: Validate against Annex 3 test vectors and optimize for real-time throughput.

### Success Criteria for Operational Integration

- Code Integrity: Exact matching of spreading codes against LSIS-AFS Annex 3 references.
- Sensitivity: Achieve a Bit Error Rate (BER) < 10^{-5} at SNR > 0 dB.
- Frame Sync Reliability: Synchronization success rate exceeding 99%.
- Latency & Throughput: Process 12-second frames in < 1 second, maintaining a Real-time factor > 1x.

The integration of LunaNet LSIS-AFS into existing MCC architectures is not merely a software update, but a foundational step toward a unified deep-space network. By following this phased roadmap and adhering to cross-agency interoperability standards, we ensure a secure and efficient future for lunar exploration.
