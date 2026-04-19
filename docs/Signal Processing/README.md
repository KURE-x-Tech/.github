# LSIS-AFS Signal Processing Pipeline: Engineering Design Specification

> [!NOTE]
> [Back to Home](../README.md)

Table of Contents:

- [LSIS-AFS Signal Processing Pipeline: Engineering Design Specification](#lsis-afs-signal-processing-pipeline-engineering-design-specification)
     - [Specification Overview and System Context](#specification-overview-and-system-context)
          - [Table 1: Implementation Scope](#table-1-implementation-scope)
     - [Modular Software Architecture \& Gateway Methodology](#modular-software-architecture--gateway-methodology)
     - [Spreading Code Generation: Logic and Parameters](#spreading-code-generation-logic-and-parameters)
          - [Table 2: Signal Parameter Matrix](#table-2-signal-parameter-matrix)
          - [Mathematical Logic](#mathematical-logic)
     - [Forward Error Correction (FEC) and Data Integrity](#forward-error-correction-fec-and-data-integrity)
     - [Frame Structure and Navigation Message Assembly](#frame-structure-and-navigation-message-assembly)
          - [Table 3: Frame Assembly Map](#table-3-frame-assembly-map)
          - [Subframe Builder Logic \& Constraints:](#subframe-builder-logic--constraints)
     - [Digital Baseband Signal Generation (I/Q)](#digital-baseband-signal-generation-iq)
          - [Alignment \& Output Requirements:](#alignment--output-requirements)
     - [Performance Benchmarking and Compliance Validation](#performance-benchmarking-and-compliance-validation)
          - [Table 4: Performance Targets \& Success Criteria](#table-4-performance-targets--success-criteria)

### Specification Overview and System Context

The LunaNet Signal-In-Space Augmented Forward Signal (LSIS-AFS) is a cornerstone of the LunaNet interoperability framework, providing critical Positioning, Navigation, and Timing (PNT) services for lunar assets. This software implementation serves as the deterministic bridge between raw navigation data, comprising ephemeris, clock states, and network access information, and the digital baseband samples required for high-fidelity radio frequency transmission. By formalizing the digital signal processing (DSP) chain, this specification ensures that the Augmented Forward Signal meets the rigorous synchronization and data integrity requirements of the deep-space environment.

The scope of this document is restricted to the software-defined components of the signal chain, enabling portability across different ground station architectures within the European Space Tracking (ESTRACK) network.

#### Table 1: Implementation Scope

| In-Scope (Software Implementation)               | Out-of-Scope (RF Hardware)                       |
| ------------------------------------------------ | ------------------------------------------------ |
| Spreading code generation (Gold, Weil, Legendre) | RF carrier generation and modulation             |
| Digital baseband signal generation (I/Q samples) | Antenna design and polarization                  |
| Message encoding/decoding (BCH, LDPC, CRC-24)    | Power amplifiers and transmitters                |
| Frame structure assembly and message parsing     | Receiver front-end hardware (LNA/Downconverters) |
| Time of Transmission (ToT) and SISE algorithms   | Phase noise and RF physical impairments          |
| Test vector generation and validation suite      | Ground station dish structural mechanics         |

The LSIS-AFS development follows a "Gateway" methodology, emphasizing incremental validation of the signal-in-space. This philosophy ensures that each mathematical building block, from spreading codes to error correction, is verified in isolation before integration, facilitating the parallel development of the modular pipeline.

### Modular Software Architecture & Gateway Methodology

In space-grade systems, modularity is essential to ensure reliability, ease of testing, and adaptability to evolving LunaNet standards. A decoupled architecture allows for the insertion of optimized algorithms (e.g., accelerated LDPC decoders) without disrupting the upstream message assembly or downstream I/Q mapping.

The system is organized into the following functional modules:

- `codes/`: Implements the fundamental spreading logic. It manages Gold, Weil, and Legendre generators, ensuring the signal possesses high processing gain and low cross-correlation.
- `encoding/`: The core of signal robustness. This module handles FEC (BCH and LDPC), CRC-24 parity, and block interleaving, directly impacting the Bit Error Rate (BER) in high-noise lunar channels.
- `messages/`: Responsible for the structural assembly of Subframes 1–4. It transforms PNT parameters into the bit-level organization required for the 12-second frame.
- `signal/`: The digital baseband engine. It maps encoded symbols to BPSK I/Q samples and ensures strict timing alignment between code tiers.
- `utils/`: Shared utilities, including a "Code assignment database" for PRN mapping, LRT epoch conversion, and SISE (Signal-In-Space Error) calculation algorithms.

The implementation is executed through eight strategic gateways:

1. Gateway 1: Spreading Code Generation: So What? This provides the unique "address" of the node. Without validated codes, the receiver cannot correlate or even detect the presence of the signal.
2. Gateway 2: Forward Error Correction: So What? Establishes the mathematical survivability of the data. This is a prerequisite for Gateway 5, as decoders must be ready to handle punctured and interleaved symbol streams.
3. Gateway 3: Navigation Message Framing: So What? Organizes bits into a temporal sequence. This allows the system to transition from raw math to a structured "language" that assets can interpret.
4. Gateway 4: Baseband Generation: So What? Produces the final digital product. This is the hand-off point to RF hardware, where theoretical frames become physical waves.
5. Gateway 5: Frame Sync & Decoding: So What? The critical receiver-side milestone. Frame synchronization is the "master clock" for the entire receiver chain; without it, LDPC decoding is impossible because symbol boundaries remain unknown.
6. Gateway 6: Message Parsing: So What? Extracts actionable PNT data. This transforms received bits back into physical coordinates and time.
7. Gateway 7: Integration & Validation: So What? Ensures end-to-end coherency. This stage proves the pipeline can handle the 12 interim test codes (PRN 1–12) required for initial deployment.
8. Gateway 0 & 8: Architecture & Documentation: Ensures the system is maintainable and flight-ready for ground segment operators.

### Spreading Code Generation: Logic and Parameters

To ensure high processing gain and robust cross-correlation properties in the lunar environment, LSIS-AFS utilizes tiered spreading sequences.

#### Table 2: Signal Parameter Matrix

| Parameter         | AFS-I             | AFS-Q                                 |
| ----------------- | ----------------- | ------------------------------------- |
| Primary Code      | Gold (2046 chips) | Weil (10230 chips)                    |
| Chip Rate         | 1.023 Mchip/s     | 5.115 Mchip/s                         |
| Secondary Code    | N/A               | 4 chips (8 ms)                        |
| Tertiary Code     | N/A               | 1500 chips (12 seconds)               |
| Quadratic Residue | N/A               | Legendre sequence length 10223 & 1499 |

#### Mathematical Logic

- Gold Code (AFS-I): Generated via an 11-stage LFSR with specific feedback taps. The sequence is short-cycled from its natural 2047-chip length to 2046 chips to ensure integer alignment with the 2ms code period. PRNs 1-210 are generated using initialization vectors from Appendix C.
- Weil Code (AFS-Q): Constructed from Legendre sequences (L) using quadratic residue calculations mod prime (x^2 \equiv k \pmod p). The primary Weil code uses length p=10223 with insertion indices applied. The tertiary code uses length p=1499.
- Tiered Assembly: AFS-Q combines codes via modulo-2 addition: \text{Primary} \oplus \text{Secondary} \oplus \text{Tertiary}. The secondary code mapping (S0=1110, S1=0111, S2=1011, S3=1101) provides an intermediate timing layer, while the tertiary code achieves the 12-second coherent frame duration necessary for weak-signal acquisition.

Code-phase synchronization is mandatory; the tertiary code start must precisely synchronize with the 12-second frame start to avoid correlation loss at the receiver.

### Forward Error Correction (FEC) and Data Integrity

The lunar electromagnetic channel is hostile, necessitating a multi-tiered FEC strategy to maintain a BER < 10^{-5} at SNR > 0 dB.

- BCH(51,8): Protects Subframe 1. It uses an 8-stage LFSR with octal generator polynomial 763. The encoder takes the 8 LSBs of the 9-bit field, performs the linear shift, and handles the MSB through modulo-2 addition and prepending to generate the 52-symbol codeword.
- LDPC (Rate 1/2): Protects Subframes 2, 3, and 4 using sparse matrix multiplication in GF(2). The encoder parses submatrices A, B, C, D, and B^{-1}. For Subframes 3/4, 10 zero-filler bits are added. Puncturing is strictly applied to the first z bits of the systematic portion to match output symbol counts.
- CRC-24: Applied to Subframes 2–4. It uses the generator polynomial from LSIS-FID0-467 to ensure frame-level integrity before decoding.
- Block Interleaving: To mitigate burst errors, a 60x98 matrix (5880 symbols) is applied to Subframes 2, 3, and 4. Symbols are written row-wise and read out column-wise, ensuring that temporal interference does not overwhelm the LDPC decoder’s correction capability.

### Frame Structure and Navigation Message Assembly

The fundamental unit of transmission is the 12-second frame, ensuring predictable Time of Transmission (ToT) calculations.

#### Table 3: Frame Assembly Map

| Component        | Encoding Type    | Bit Count | Symbol Count |
| ---------------- | ---------------- | --------- | ------------ |
| Sync Pattern     | None (Unencoded) | -         | 68           |
| Subframe 1 (SB1) | BCH(51,8)        | 9         | 52           |
| Subframe 2 (SB2) | LDPC(1/2)        | 1200      | 2400         |
| Subframe 3 (SB3) | LDPC(1/2)        | 870       | 1740         |
| Subframe 4 (SB4) | LDPC(1/2)        | 870       | 1740         |
| **Total**        |                  |           | **6000**     |

#### Subframe Builder Logic & Constraints:

- SB1: Packs Frame ID (FID: 0-3) and Time of Interval (TOI: 0-99).
- SB2 (CED): Contains Week Number (WN: 0-8191), Interval Time of Week (ITOW: 0-503), and TOI. Includes Clock/Ephemeris Data and Health status.
- SB3 & SB4: Support variable payloads like Orbit Almanacs and Network Access Information.
- Sync Pattern: The 68-symbol sequence (0xCC63F74536F49E04A) enables frame boundary detection via cross-correlation.

### Digital Baseband Signal Generation (I/Q)

Encoded symbols are mapped to complex I/Q samples using Binary Phase Shift Keying (BPSK): Logic 1 -> -1.0, Logic 0 -> +1.0.

#### Alignment & Output Requirements:

1. Data-to-Code Alignment: Data symbols must align strictly with primary code boundaries.
2. Tiered Alignment: Secondary codes align with primary; tertiary codes align with secondary.
3. Coherency: The 12-second tertiary code epoch must coincide with the frame start (LSIS-220).
4. Sampling: The DSP pipeline must support configurable sample rates, typically 10.23 MHz or 20.46 MHz, to accommodate various Software Defined Radio (SDR) front-ends.
5. Format: Output shall be in binary int16 or float32 for compatibility with ESTRACK signal file I/O.

### Performance Benchmarking and Compliance Validation

Flight-readiness is determined by the system’s ability to operate in real-time within the ground segment infrastructure (ESOC).

#### Table 4: Performance Targets & Success Criteria

| Metric               | Target / Limit                    |
| -------------------- | --------------------------------- |
| Code Generation      | < 1 second per PRN                |
| Frame Encoding       | < 100 milliseconds                |
| Frame Decoding       | < 1 second (Real-time capability) |
| Bit Error Rate (BER) | < 10^{-5} at SNR > 0 dB           |
| SISE Position Error  | < 40 meters (95th percentile)     |
| SISE Velocity Error  | < 1 cm/s (95th percentile)        |

Validation Protocol: The implementation is validated against reference test vectors for all 210 PRNs. Success is defined as 100% data recovery in round-trip (encode-to-decode) testing across all 12 interim test codes. This specification ensures that the LSIS-AFS pipeline provides a reliable, interoperable PNT link for the next generation of European and international lunar missions.
