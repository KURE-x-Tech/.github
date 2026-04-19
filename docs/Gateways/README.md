# LunaNet Gateways

Listen up. As your Lead Systems Engineer, I need you to understand that the LunaNet Signal-In-Space Augmented Forward Signal (LSIS-AFS) Reference Implementation is not a typical web stack—this is a pure digital signal processing (DSP) pipeline. To build a system that can reliably transmit and receive navigation data from the Moon, we are executing this project using a strict "Gateway-based approach".

There are 9 gateways total (numbered 0 through 8), designed to enforce incremental validation so we catch complex DSP errors early. Here is the architectural logic, the pitfalls to watch out for, and the actionable execution plan for each gateway.

## Gateway 0: Design & Architecture

[Gateway 0 Documentation](./0/README.md)

- **Focus:** Designing the system architecture and establishing our cross-platform development foundation.
- **Architectural Logic:** This is your API design phase. You are defining the I/O interfaces between modular DSP blocks so that our transmitters and receivers can talk to each other seamlessly.
- **Technical Skepticism:** Do not over-engineer hardware dependencies. A common pitfall here is tightly coupling code to specific RF receiver hardware, which is strictly out of scope.
- **Actionable Steps:**
     - _Task:_ Set up version control (Git) and select open-source math libraries.
     - _Implementation:_ Draft the system architecture document and component interaction diagrams.
     - _Validation:_ Ensure the testing framework is established and the team is aligned.
- **Compliance:** The tech stack _shall_ process specified data formats (Binary I/Q, CSV, JSON) and support standard open-source build tools.

## Gateway 1: Spreading Code Generation

[Gateway 1 Documentation](./1/README.md)

- **Focus:** Generating all spreading codes (Gold, Weil, Secondary) for PRN 1–210.
- **Architectural Logic:** These pseudo-random noise (PRN) sequences are the mathematical root of our Code Division Multiple Access (CDMA) signal. They allow the receiver to isolate our specific spacecraft's signal from the background noise of space.
- **Technical Skepticism:** Shift-register implementations are notorious for off-by-one errors. Pay strict attention to your polynomial generator seeds.
- **Actionable Steps:**
     - _Task:_ Build the sequence generators.
     - _Implementation:_ Code the 2046-chip Gold codes, 10230-chip Weil primary codes, and 1500-chip Weil tertiary codes.
     - _Validation:_ Match your output exactly to the reference hexadecimal values in Annex 3.
- **Compliance:** The system _shall_ generate all spreading codes for PRN 1-210 in under 1 second per PRN.

## Gateway 2: Forward Error Correction (FEC)

[Gateway 2 Documentation](./2/README.md)

- **Focus:** Implementing BCH, LDPC, and CRC-24 encoders and decoders.
- **Architectural Logic:** FEC is what protects our navigation payload from the high bit-error rates of a 400,000 km cislunar RF link.
- **Technical Skepticism:** LDPC (Low-Density Parity-Check) decoding is computationally massive. A naive implementation will destroy our real-time performance budget. Watch your bit-endianness when calculating the CRC-24 checksums.
- **Actionable Steps:**
     - _Task:_ Build the error detector and correction algorithms.
     - _Implementation:_ Write the BCH, LDPC, and CRC-24 logic.
     - _Validation:_ Pass all provided test vectors with zero errors.
- **Compliance:** The implementation _shall_ accurately encode and decode using BCH, LDPC, and CRC-24 protocols.

## Gateway 3: Navigation Message Framing

[Gateway 3 Documentation](./3/README.md)

- **Focus:** Building complete 12-second navigation frames.
- **Architectural Logic:** Here we pack our FEC-encoded data (like ephemeris, time of week, and maneuver alerts) into a continuous, structured digital stream.
- **Technical Skepticism:** Misaligning your payload fields by a single bit will corrupt the entire 12-second frame.
- **Actionable Steps:**
     - _Task:_ Construct the frame assembly system.
     - _Implementation:_ Multiplex the data channels and pilot channels into the correct frame structure.
     - _Validation:_ Generate mathematically valid 12-second frames.
- **Compliance:** The system _shall_ format all messages according to the LSIS-AFS Volume A specification.

## Gateway 4: Baseband Signal Generation

[Gateway 4 Documentation](./4/README.md)

- **Focus:** Generating complex numerical I/Q baseband samples using BPSK modulation.
- **Architectural Logic:** This gateway converts your digital binary frames into a mathematical representation of radio waves, allowing us to simulate the physical transmission.
- **Technical Skepticism:** Floating-point precision loss is a massive risk here when generating I/Q samples. You must strictly control your sample rates and ensure the S-band carrier phase remains continuous.
- **Actionable Steps:**
     - _Task:_ BPSK Modulator implementation.
     - _Implementation:_ Apply the spreading codes from GW1 to the frames from GW3 to generate the baseband output.
     - _Validation:_ Produce binary I/Q signal files ready for receiver testing.
- **Compliance:** The system _shall_ output properly structured baseband files representing the dual-channel BPSK signal.

## Gateway 5: Frame Sync & Decoding

[Gateway 5 Documentation](./5/README.md)

- **Focus:** Detecting and decoding frames from received signals.
- **Architectural Logic:** We are now transitioning to the "Receiver" side. You are building the logic to hunt for the AFS signal in the noise, lock onto it, and strip away the modulation to recover the raw bits.
- **Technical Skepticism:** Signal correlation peaks can easily be buried in the noise floor. Beware of phase ambiguities when demodulating BPSK, which can invert your entire data stream.
- **Actionable Steps:**
     - _Task:_ Build the acquisition and tracking loops.
     - _Implementation:_ Ingest the I/Q files from GW4 and establish frame synchronization.
     - _Validation:_ Successfully detect frames from clean baseline signals.
- **Compliance:** The receiver _shall_ correctly synchronize with the 12-second frame boundaries.

## Gateway 6: Message Parsing

[Gateway 6 Documentation](./6/README.md)

- **Focus:** Extracting the raw navigation data from the decoded frames.
- **Architectural Logic:** This translates the raw, error-corrected bits back into usable telemetry data (like satellite attitude, clock data, and orbital parameters) for the end-user.
- **Technical Skepticism:** If your frame sync in GW5 was loose, your parser will pull garbage data. Validate your bit-masks rigorously.
- **Actionable Steps:**
     - _Task:_ Build message field extractors.
     - _Implementation:_ Parse subframes and extract values like the Time of Transmission.
     - _Validation:_ Accurately pull all navigation data from the frame.
- **Compliance:** The parser _shall_ successfully extract standard LunaNet messages, such as MSG-G10 (Maneuver) and MSG-G11 (Ephemeris).

## Gateway 7: Integration & Validation

[Gateway 7 Documentation](./7/README.md)

- **Focus:** End-to-end functionality, interoperability, and compliance.
- **Architectural Logic:** This is the ultimate test of our DSP chain. We feed data into GW2, transmit it in GW4, and catch it in GW5 to ensure the pipeline operates seamlessly.
- **Technical Skepticism:** Latency bottlenecks hidden in your LDPC decoder or code generator will surface here and crash the system during real-time processing limits.
- **Actionable Steps:**
     - _Task:_ Execute full round-trip tests (encode → signal → decode → verify).
     - _Implementation:_ Run interoperability tests with other implementations and track Bit Error Rate (BER) vs SNR performance curves.
     - _Validation:_ Prove 100% round-trip data recovery accuracy.
- **Compliance:** The software _shall_ process a full 12-second frame in under 1 second, and pass all LSIS compliance checks.

## Gateway 8: Documentation & Examples

[Gateway 8 Documentation](./8/README.md)

- **Focus:** Complete documentation for using and understanding the implementation.
- **Architectural Logic:** LunaNet is an open, international standard. Our code will be the foundation for future ESA and NASA engineers; it must be highly readable and strictly reproducible.
- **Technical Skepticism:** Do not let documentation rot. Ensure your API docs match the final refactored code exactly, otherwise, the next team will fail their integration tests.
- **Actionable Steps:**
     - _Task:_ Create user guides, API docs, and performance reports.
     - _Implementation:_ Build tutorial notebooks and clear setup instructions.
     - _Validation:_ Ensure a third-party developer can execute reproducible builds and run the test vector suite using just your README.
- **Compliance:** The repository _shall_ include public API documentation and working usage examples for all components.
