# Product Requirements Document: Universal MIDI Utility

**Project Name:** MIDITools 
**Version:** 1.0  
**Date:** January 2026  
**Status:** Draft  

---

## Summary

This document defines the product requirements for an open-source, cross-platform MIDI utility designed to consolidate and modernize the fragmented landscape of MIDI tools. The application will serve as the definitive utility for musicians, sound designers, and developers working with MIDI hardware—from vintage synthesizers and samplers to modern MIDI 2.0 devices.

The MIDI utility space has remained largely stagnant, with legacy tools like MIDI-OX (Windows-only, development ended at Windows XP) and Snoize SysEx Librarian (macOS-only) serving as the de facto standards. Commercial alternatives like MIDIQuest are expensive and have dated interfaces. This project aims to create a unified, modern solution that addresses the gaps and limitations of existing tools while providing an extensible architecture for community contributions.

---

## 1. Objectives and Success Criteria

### 1.1 Primary Objectives

| Objective | Description | Success Metric |
|-----------|-------------|----------------|
| **Unification** | Consolidate fragmented MIDI utility functions into a single application | All core features from MIDI-OX, SysEx Librarian, and Elektron C6 available in one tool |
| **Cross-Platform** | Support Windows, macOS, and Linux | Native builds for all three platforms with consistent UX |
| **Modernization** | Provide a contemporary user interface with modern UX patterns | User satisfaction scores >4/5 in beta testing |
| **Extensibility** | Enable community-driven expansion through plugin architecture | JSON-based SysEx definition format with >50 device definitions within 12 months |
| **Open Source** | Build a sustainable open-source community | Active contributor community with >10 contributors within 6 months |
| **Future-Proofing** | Support emerging standards | MIDI 2.0 UMP message support from launch |

### 1.2 Secondary Objectives

- **Education**: Help users understand MIDI at a deeper level through plain-language message explanations
- **Preservation**: Support vintage hardware through comprehensive legacy protocol support
- **Reliability**: Provide robust SysEx transfer with configurable timing and handshaking
- **Discoverability**: Make device capabilities explorable without requiring manual reference

### 1.3 Success Criteria

1. **Adoption**: 10,000+ downloads within first year
2. **Community**: 50+ community-contributed device definitions within first year
3. **Reliability**: 99.9% successful SysEx transfers with supported devices

### 1.4 Non-Goals (Explicit Exclusions)

- **DAW/Sequencer functionality**: This is a utility, not a production tool
- **Audio processing**: No audio effects or recording (that will be a separate sampler-oriented project under the StudioTools umbrella)
- **Patch editing UI**: Device-specific graphical editors are out of scope (that too will be a separate project)

---

## 2. Features: Epics and Use Cases

### Epic 1: MIDI Monitoring and Diagnostics

**Goal**: Provide comprehensive real-time visibility into all MIDI activity

#### 2.1.1 Real-Time MIDI Monitor

**Description**: Display all incoming and outgoing MIDI messages across all connected ports with timestamping, filtering, and multiple display formats.

**Use Cases**:
- UC-1.1: As a musician, I want to see what MIDI data my controller is sending so I can troubleshoot why my DAW isn't responding correctly
- UC-1.2: As a developer, I want to monitor raw hex data alongside decoded messages so I can debug my MIDI implementation
- UC-1.3: As a technician, I want to "spy" on MIDI traffic between applications so I can diagnose routing issues without interrupting the signal flow

**Requirements**:
| ID | Requirement | Priority |
|----|-------------|----------|
| MON-001 | Display timestamp (clock time, relative, host ticks) for each message | Must |
| MON-002 | Show human-readable interpretation alongside raw hex | Must |
| MON-003 | Filter by message type (Note, CC, SysEx, Clock, etc.) | Must |
| MON-004 | Filter by MIDI channel (1-16, omni) | Must |
| MON-005 | Filter by port (input/output selection) | Must |
| MON-006 | Pause/resume monitoring while buffering data | Must |
| MON-007 | Export captured data to text/CSV/MIDI file | Should |
| MON-008 | Color-code messages by type | Should |
| MON-009 | "Spy" mode to intercept MIDI between other applications | Should |
| MON-010 | Configurable display columns | Could |
| MON-011 | Support MIDI 2.0 UMP message display | Must |

**User Feedback Addressed**:
- MIDI-OX users report the interface is "confusing for beginners"—provide simplified default view
- Users want clear indication of running status and implicit messages

#### 2.1.2 Message Statistics and Analysis

**Description**: Provide aggregated statistics and pattern analysis for MIDI traffic.

**Use Cases**:
- UC-1.4: As a live performer, I want to see message throughput statistics so I can identify bandwidth bottlenecks
- UC-1.5: As a developer, I want to see timing distribution histograms so I can verify my timing implementation

**Requirements**:
| ID | Requirement | Priority |
|----|-------------|----------|
| STAT-001 | Count messages by type over time | Should |
| STAT-002 | Calculate messages/second throughput | Should |
| STAT-003 | Identify potential MIDI choke conditions | Could |
| STAT-004 | Show timing jitter analysis | Could |

---

### Epic 2: SysEx Management

**Goal**: Provide comprehensive System Exclusive message handling for all devices

#### 2.2.1 SysEx Send/Receive

**Description**: Send and receive SysEx messages to/from MIDI devices with configurable timing, handshaking, and progress feedback.

**Use Cases**:
- UC-2.1: As a synthesist, I want to back up my synth's memory to a file so I can restore it later
- UC-2.2: As a studio owner, I want to send firmware updates to my gear so I can keep devices current
- UC-2.3: As a vintage gear collector, I want to transfer patches between incompatible devices by exporting to a common format

**Requirements**:
| ID | Requirement | Priority |
|----|-------------|----------|
| SYX-001 | Send .syx files to selected MIDI output | Must |
| SYX-002 | Receive SysEx data and save to .syx files | Must |
| SYX-003 | Configurable inter-message delay (1ms-5000ms) | Must |
| SYX-004 | Configurable buffer size for compatibility with older devices | Must |
| SYX-005 | Progress indicator with byte count and percentage | Must |
| SYX-006 | Auto-split received dumps by F0/F7 boundaries | Must |
| SYX-007 | Support handshaking protocols (ACK/NAK/WAIT/CANCEL) | Must |
| SYX-008 | Read SysEx from Standard MIDI Files (.mid) | Should |
| SYX-009 | Convert between .syx and .mid formats | Should |
| SYX-010 | Batch send multiple files with configurable ordering | Should |
| SYX-011 | Checksum calculation and verification (Roland, Yamaha, etc.) | Should |
| SYX-012 | "Turbo" mode support for high-speed interfaces | Could |

**User Feedback Addressed**:
- SysEx Librarian users request ability to "compare or edit sysex files"—provide hex editor
- Users report "random crashes and lockups" with large SysEx—implement robust timeout handling
- Speed adjustment requested for older devices that can't keep up

#### 2.2.2 SysEx Library Management

**Description**: Organize, categorize, and manage a library of SysEx files.

**Use Cases**:
- UC-2.4: As a user with many synths, I want to organize my SysEx backups by device and date
- UC-2.5: As a performer, I want to create setlists of patches to send to multiple devices before a show

**Requirements**:
| ID | Requirement | Priority |
|----|-------------|----------|
| LIB-001 | Import .syx and .mid files containing SysEx | Must |
| LIB-002 | Organize files in user-defined folder structure | Must |
| LIB-003 | Auto-detect manufacturer from SysEx ID bytes | Must |
| LIB-004 | Tag files with metadata (device, date, description) | Should |
| LIB-005 | Search library by tag, manufacturer, date | Should |
| LIB-006 | Create "sets" of files to send together | Should |
| LIB-007 | Associate files with program change numbers for triggering | Could |

#### 2.2.3 SysEx Hex Editor

**Description**: View and edit raw SysEx data with contextual assistance.

**Use Cases**:
- UC-2.6: As a power user, I want to manually edit SysEx bytes to fix corrupted data
- UC-2.7: As a developer, I want to craft custom SysEx messages for testing

**Requirements**:
| ID | Requirement | Priority |
|----|-------------|----------|
| HEX-001 | Display SysEx in hex with ASCII sidebar | Must |
| HEX-002 | Edit bytes with validation (0x00-0x7F data range) | Must |
| HEX-003 | Recalculate checksums after editing | Should |
| HEX-004 | Compare two SysEx files side-by-side | Should |
| HEX-005 | Search/replace byte patterns | Should |
| HEX-006 | Highlight manufacturer ID, model ID, command bytes | Could |

---

### Epic 3: SysEx Definition Engine

**Goal**: Enable human-readable interpretation of SysEx messages through extensible device definitions

#### 2.3.1 Definition Format and Parser

**Description**: Parse JSON-based device definition files that describe SysEx message structure for specific devices.

**Use Cases**:
- UC-3.1: As a user, I want the app to decode my Roland JV-2080's SysEx so I understand what parameters are being set
- UC-3.2: As a community member, I want to contribute a definition for my obscure device so others can benefit

**Requirements**:
| ID | Requirement | Priority |
|----|-------------|----------|
| DEF-001 | Parse JSON definition files conforming to published schema | Must |
| DEF-002 | Support manufacturer ID matching (1-byte and 3-byte IDs) | Must |
| DEF-003 | Support device/model ID matching | Must |
| DEF-004 | Define parameter addresses and ranges | Must |
| DEF-005 | Define parameter names and human-readable values | Must |
| DEF-006 | Support different encoding schemes (see Epic 4) | Must |
| DEF-007 | Define checksum algorithms (Roland, Yamaha, etc.) | Should |
| DEF-008 | Include parameter groupings (e.g., "Filter", "Envelope") | Should |
| DEF-009 | Version definitions for firmware variations | Should |
| DEF-010 | Hot-reload definitions without app restart | Could |

#### 2.3.2 Plain-Language Message Explanation

**Description**: Display decoded SysEx messages in plain English based on loaded definitions.

**Use Cases**:
- UC-3.3: As a beginner, I want to understand what a SysEx message means without reading hex
- UC-3.4: As a technician, I want to verify that parameter changes were transmitted correctly

**Requirements**:
| ID | Requirement | Priority |
|----|-------------|----------|
| EXP-001 | Display parameter name (e.g., "Filter Cutoff") | Must |
| EXP-002 | Display decoded value with units (e.g., "3200 Hz") | Must |
| EXP-003 | Show address path (e.g., "Part 1 > Filter > Cutoff") | Should |
| EXP-004 | Indicate when value is at min/max/center | Should |
| EXP-005 | Link to definition source for community editing | Could |

#### 2.3.3 Definition Repository

**Description**: Manage and update device definitions from a community repository.

**Requirements**:
| ID | Requirement | Priority |
|----|-------------|----------|
| REPO-001 | Bundle common definitions with application | Must |
| REPO-002 | Check online repository for definition updates | Should |
| REPO-003 | User-contributed definitions via pull request workflow | Should |
| REPO-004 | Definition validation/linting tool | Should |

---

### Epic 4: Data Encoding/Decoding

**Goal**: Handle all manufacturer-specific data encoding schemes used to work around MIDI's 7-bit limitation

#### 2.4.1 Nibbling and Bit-Packing Support

**Description**: Encode and decode data using various manufacturer-specific schemes.

**Background**: MIDI restricts data bytes to 0-127 (7 bits). Manufacturers use different schemes to encode 8-bit or larger values:
- **Roland**: Nibblized addressing with checksum; data often as direct 7-bit values
- **Korg**: High bit packed into prefix byte (7 data bytes preceded by MSB accumulator)
- **Yamaha**: Various schemes including signed offset representations
- **E-mu**: Packed 7-bit with specific byte ordering
- **Sequential/DSI**: Direct 7-bit with specific data structures

**Use Cases**:
- UC-4.1: As a developer, I want to decode Korg 7-bit packed data so I can read patch parameters
- UC-4.2: As a definition author, I want to specify the encoding scheme in my definition file

**Requirements**:
| ID | Requirement | Priority |
|----|-------------|----------|
| ENC-001 | Roland nibblized encoding/decoding | Must |
| ENC-002 | Korg 7-bit packed encoding/decoding | Must |
| ENC-003 | Standard MSB/LSB 14-bit encoding | Must |
| ENC-004 | Sequential packed format | Should |
| ENC-005 | E-mu packed format | Should |
| ENC-006 | Configurable signed/unsigned interpretation | Should |
| ENC-007 | Offset-64 (bipolar) parameter handling | Should |
| ENC-008 | Calculator tool for manual conversions | Should |

#### 2.4.2 Checksum Calculation

**Description**: Calculate and verify checksums for various manufacturer algorithms.

**Requirements**:
| ID | Requirement | Priority |
|----|-------------|----------|
| CHK-001 | Roland checksum (128 - (sum mod 128)) | Must |
| CHK-002 | Yamaha checksum variants | Should |
| CHK-003 | XOR checksum | Should |
| CHK-004 | Auto-detect checksum type from definition | Should |
| CHK-005 | Recalculate checksum after manual edits | Should |

---

### Epic 5: MIDI Sample Dump Standard (SDS)

**Goal**: Provide comprehensive sample transfer support for vintage and modern samplers

#### 2.5.1 SDS Transfer

**Description**: Transfer audio samples to/from samplers using the MIDI Sample Dump Standard.

**Use Cases**:
- UC-5.1: As a vintage sampler owner, I want to transfer WAV files to my Akai S1000 via MIDI
- UC-5.2: As a user, I want to back up samples from my sampler to my computer

**Requirements**:
| ID | Requirement | Priority |
|----|-------------|----------|
| SDS-001 | Send WAV/AIFF files as SDS dumps | Must |
| SDS-002 | Receive SDS dumps and save as WAV | Must |
| SDS-003 | Support open-loop (no handshaking) transfers | Must |
| SDS-004 | Support closed-loop (handshaking) transfers | Must |
| SDS-005 | Handle 8-28 bit sample formats | Must |
| SDS-006 | Configure sample number/position | Must |
| SDS-007 | Set/preserve loop points | Should |
| SDS-008 | Support extended SDS (larger files, names) | Should |
| SDS-009 | Batch transfer multiple samples | Should |
| SDS-010 | Sample rate conversion | Could |
| SDS-011 | Stereo-to-mono conversion options | Should |

**Devices Supported**: Akai S1000 series, E-mu EMAX/Emulator series, Roland S-750/760, Yamaha TX16W, Korg DSS-1, Ensoniq EPS, Prophet 2000/2002, Elektron devices, and any SDS-compliant hardware.

---

### Epic 6: NRPN and RPN Support

**Goal**: Simplify working with Non-Registered and Registered Parameter Numbers

#### 2.6.1 NRPN/RPN Message Construction

**Description**: Generate properly-formatted NRPN and RPN message sequences.

**Use Cases**:
- UC-6.1: As a user, I want to send NRPN messages to my Kemper without manually calculating byte splits
- UC-6.2: As a performer, I want to create NRPN presets for complex parameter changes

**Requirements**:
| ID | Requirement | Priority |
|----|-------------|----------|
| NRPN-001 | GUI for constructing NRPN messages (parameter + value) | Must |
| NRPN-002 | Automatic MSB/LSB calculation and splitting | Must |
| NRPN-003 | Send complete 4-message NRPN sequence | Must |
| NRPN-004 | Decode incoming NRPN sequences | Must |
| NRPN-005 | RPN message construction (Pitch Bend Range, etc.) | Must |
| NRPN-006 | Data Increment/Decrement support | Should |
| NRPN-007 | Null RPN transmission to reset | Should |
| NRPN-008 | Device-specific NRPN databases | Could |

**User Feedback Addressed**:
- Users report NRPN is "like cracking the enigma code"—provide visual calculator
- Kemper users request spreadsheet of all NRPN parameters—enable community databases

---

### Epic 7: Test Tone and MIDI Generation

**Goal**: Generate MIDI messages for testing and calibration

#### 2.7.1 Note Generator

**Description**: Generate MIDI notes for testing sound modules and connections.

**Use Cases**:
- UC-7.1: As a technician, I want to send test notes to verify a MIDI connection
- UC-7.2: As a sound designer, I want to audition patches across the keyboard range

**Requirements**:
| ID | Requirement | Priority |
|----|-------------|----------|
| GEN-001 | Send single notes (pitch, velocity, channel, duration) | Must |
| GEN-002 | Keyboard widget for mouse/computer keyboard input | Must |
| GEN-003 | Programmable note sequences | Should |
| GEN-004 | Velocity curves/patterns | Should |
| GEN-005 | Arpeggiation modes | Could |
| GEN-006 | MIDI clock generation | Should |

#### 2.7.2 Control Surface Emulation

**Description**: Generate control change and other messages via on-screen controls.

**Requirements**:
| ID | Requirement | Priority |
|----|-------------|----------|
| CTRL-001 | Virtual sliders for CC messages | Must |
| CTRL-002 | Program Change sender | Must |
| CTRL-003 | Bank Select (MSB/LSB) sender | Must |
| CTRL-004 | Pitch bend wheel | Should |
| CTRL-005 | Mod wheel | Should |
| CTRL-006 | All Notes Off / Reset All Controllers buttons | Must |

---

### Epic 8: MIDI Routing and Filtering

**Goal**: Route and transform MIDI data between ports and applications

#### 2.8.1 Port Routing

**Description**: Route MIDI data between inputs, outputs, and virtual ports.

**Use Cases**:
- UC-8.1: As a user, I want to route one MIDI input to multiple outputs
- UC-8.2: As a performer, I want to merge multiple controllers to a single output

**Requirements**:
| ID | Requirement | Priority |
|----|-------------|----------|
| RTE-001 | Connect any input to any output | Must |
| RTE-002 | Multiple simultaneous routes | Must |
| RTE-003 | Virtual MIDI ports (input/output) | Should |
| RTE-004 | Save/load routing configurations | Should |
| RTE-005 | Per-route filtering | Should |

#### 2.8.2 Message Filtering

**Description**: Filter MIDI messages based on type, channel, and data values.

**Requirements**:
| ID | Requirement | Priority |
|----|-------------|----------|
| FLT-001 | Filter by message type (pass/block) | Must |
| FLT-002 | Filter by channel | Must |
| FLT-003 | Filter by value range (e.g., CC number, note range) | Should |
| FLT-004 | Channel remapping | Should |
| FLT-005 | Velocity scaling/curves | Could |
| FLT-006 | Note transposition | Could |

---

### Epic 9: Preset and Patch Library

**Goal**: Manage synthesizer presets and facilitate community sharing

#### 2.9.1 Preset Browser

**Description**: Browse, search, and preview synthesizer presets.

**Use Cases**:
- UC-9.1: As a synthesist, I want to browse thousands of presets and find the right sound quickly
- UC-9.2: As a collector, I want to organize factory and third-party patches

**Requirements**:
| ID | Requirement | Priority |
|----|-------------|----------|
| PST-001 | Import presets from SysEx files | Must |
| PST-002 | Display preset names extracted from SysEx | Must |
| PST-003 | Organize presets by device, category, tags | Should |
| PST-004 | Search presets by name, tags | Should |
| PST-005 | One-click audition (send to device) | Should |
| PST-006 | Export selected presets to SysEx | Should |
| PST-007 | Community preset sharing integration | Could |

---

### Epic 10: MIDI 2.0 Support

**Goal**: Support the emerging MIDI 2.0 standard

#### 2.10.1 MIDI 2.0 UMP Display

**Description**: Monitor and decode MIDI 2.0 Universal MIDI Packet messages.

**Requirements**:
| ID | Requirement | Priority |
|----|-------------|----------|
| M2-001 | Display MIDI 2.0 Channel Voice messages | Must |
| M2-002 | Display 32-bit velocity, controllers | Must |
| M2-003 | Show per-note controllers | Should |
| M2-004 | Display MIDI-CI discovery messages | Should |
| M2-005 | Profile Configuration display | Could |
| M2-006 | Property Exchange display | Could |

---

## 3. UX Flow and Design Notes

### 3.1 Design Principles

1. **Progressive Disclosure**: Simple default view with advanced features accessible but not overwhelming
2. **Immediate Feedback**: All actions provide visual confirmation
3. **Non-Destructive**: Original files never modified; exports create new files
4. **Cross-Platform Consistency**: Same UX on Windows, macOS, Linux
5. **Keyboard Accessible**: Full keyboard navigation for power users
6. **Dark Mode**: Default dark theme with light option (reduce eye strain in studios)

### 3.2 Main Interface Layout

```
┌─────────────────────────────────────────────────────────────────────────┐
│  [Ports ▼] [Definitions ▼]           │ Codex Sonorum      [─][□][×] │
├──────────────────────┬──────────────────────────────────────────────────┤
│                      │                                                  │
│   PORTS              │   WORKSPACE (tabbed)                            │
│   ──────             │   ──────────────────                            │
│   ▼ Inputs           │   [Monitor] [SysEx] [SDS] [NRPN] [Tools]       │
│     ├ USB MIDI       │                                                  │
│     └ Virtual 1      │   ┌────────────────────────────────────────┐   │
│   ▼ Outputs          │   │                                        │   │
│     ├ Roland JV-2080 │   │   Active tool workspace               │   │
│     ├ USB MIDI       │   │                                        │   │
│     └ Virtual 1      │   │                                        │   │
│                      │   │                                        │   │
│   LIBRARY            │   │                                        │   │
│   ───────            │   │                                        │   │
│   ▼ Backups          │   └────────────────────────────────────────┘   │
│     ├ JV-2080_2024   │                                                  │
│     └ D-50_factory   │   ┌────────────────────────────────────────┐   │
│   ▼ Presets          │   │ MESSAGE DETAIL / HEX VIEW              │   │
│                      │   └────────────────────────────────────────┘   │
├──────────────────────┴──────────────────────────────────────────────────┤
│  Status: Connected │ TX: 0 │ RX: 1,247 │ Errors: 0                      │
└─────────────────────────────────────────────────────────────────────────┘
```

### 3.3 Key Workflows

#### 3.3.1 First-Time User Workflow

1. Launch application → Auto-detect MIDI ports
2. Display welcome panel with common tasks:
   - "Monitor MIDI Activity"
   - "Backup a Synth"
   - "Send a SysEx File"
3. Clicking task provides guided walkthrough
4. Settings wizard for advanced port configuration

#### 3.3.2 SysEx Backup Workflow

1. Select output port (destination device)
2. Click "Request Dump" or device-specific dump button
3. Device selection from definition database
4. Progress indicator during receive
5. Auto-save with timestamp to library
6. Optional: decode and display contents

#### 3.3.3 SysEx Restore Workflow

1. Select file from library or browse
2. Preview contents (if definition available)
3. Select output port
4. Configure timing (preset or manual)
5. Click Send → Progress indicator
6. Verification report

### 3.4 Visual Design Notes

- **Color Palette**: Dark background (#1E1E1E), accent blue (#0078D4), success green (#4CAF50), warning orange (#FF9800), error red (#F44336)
- **Typography**: Monospace for hex/data (Fira Code), sans-serif for UI (Inter)
- **Density**: Compact mode for experienced users, comfortable mode default
- **Animations**: Subtle transitions (150ms), progress indicators for long operations
- **Icons**: Consistent icon set (Material Design or similar)

### 3.5 Accessibility

- Full keyboard navigation with visible focus indicators
- Screen reader support with ARIA labels
- Configurable font sizes
- High contrast mode
- Color-blind safe palettes for message type coding

---

## 4. Assumptions, Constraints, and Dependencies

### 4.1 Assumptions

| ID | Assumption | Impact if Wrong |
|----|------------|-----------------|
| A1 | Users have functioning MIDI interfaces | Must provide clear error messaging for connection issues |
| A2 | Target devices support standard SysEx protocols | Some devices may require workarounds documented in definitions |
| A3 | Community will contribute device definitions | May need to prioritize popular devices for built-in support |
| A4 | MIDI 2.0 adoption will increase | Investment in M2 support justified by future-proofing |
| A5 | Users accept open-source licensing | GPL/MIT acceptable; avoid proprietary dependencies |

### 4.2 Constraints

| ID | Constraint | Rationale |
|----|-----------|-----------|
| C1 | Cross-platform (Win/Mac/Linux) from v1.0 | Core value proposition; eliminates MIDI-OX vs SysEx Librarian divide |
| C2 | Open source license (MIT or similar) | Enables community contribution; differentiator from MIDIQuest |
| C3 | No external service dependencies for core features | Must work offline; internet only for updates/repository |
| C4 | Definition files must be human-editable | JSON format; avoid binary/compiled definitions |
| C5 | Must not require root/admin for basic operation | Standard user installation on all platforms |

### 4.3 Technical Dependencies

| Dependency | Purpose | Risk Mitigation |
|------------|---------|-----------------|
| Cross-platform MIDI library (RtMidi, PortMidi, or similar) | Hardware access | Evaluate multiple options; abstract interface for future swap |
| GUI framework (Qt, Electron, or similar) | User interface | Choose framework with strong cross-platform support |
| JSON parsing library | Definition files | Use standard library (nlohmann/json, etc.) |
| Audio library (for SDS) | WAV/AIFF handling | libsndfile or similar |
| OS MIDI services | System integration | Test on all platform versions; provide fallbacks |

### 4.4 External Dependencies

| Dependency | Description | Risk |
|------------|-------------|------|
| MIDI Association specifications | Protocol documentation | Publicly available; low risk |
| Manufacturer SysEx documentation | Device-specific protocols | Varies; some devices undocumented |
| Community contributors | Definition files, testing | Address through clear contribution guidelines |
| OS vendors | MIDI subsystem maintenance | Windows MIDI Services in flux; monitor Microsoft developments |

### 4.5 Risks

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| MIDI 2.0 specification changes | Medium | High | Abstract M2 implementation; stay engaged with MMA |
| Community contribution insufficient | Medium | Medium | Seed with popular devices; provide excellent tooling |
| Platform-specific MIDI bugs | High | Medium | Extensive testing matrix; user reporting mechanism |
| Legacy device incompatibility | Medium | Low | Timing configuration options; device-specific workarounds |

---

## 5. Competitive Analysis and Gap Assessment

### 5.1 Competitive Landscape Overview

| Tool | Platform | Price | Last Updated | Primary Focus |
|------|----------|-------|--------------|---------------|
| MIDI-OX | Windows | Free | ~2012 | Monitoring, SysEx, routing |
| SysEx Librarian | macOS | Free/OSS | 2024 | SysEx send/receive, library |
| MIDI Monitor | macOS | Free/OSS | 2024 | Monitoring only |
| MIDIQuest | Win/Mac | $99-$499 | 2024 | Editor/librarian (device-specific) |
| Bome MIDI Translator Pro | Win/Mac | $59 | 2024 | MIDI mapping/translation |
| Elektron C6 | Win/Mac | Free | ~2014 | SysEx, SDS (Elektron-focused) |
| Mountain MIDI Tools | Windows | Donationware | 2023 | NRPN, SysEx, utilities |
| Sample Wrench | Windows | Free | 2020s | SDS, sample editing |

### 5.2 Detailed Feature Comparison

| Feature | MIDI-OX | SysEx Lib | MIDIQuest | Bome MTP | Elektron C6 | **Codex** |
|---------|---------|-----------|-----------|----------|-------------|-----------|
| **Platform** | Win | Mac | Both | Both | Both | **All 3** |
| **MIDI Monitor** | ✓ | ✗ | ✗ | ✓ | ✗ | **✓** |
| **SysEx Send/Receive** | ✓ | ✓ | ✓ | ✗ | ✓ | **✓** |
| **SysEx Library** | Basic | ✓ | ✓ | ✗ | Basic | **✓** |
| **SysEx Decoding** | ✗ | ✗ | ✓* | ✗ | Elektron only | **✓** |
| **Sample Dump (SDS)** | ✗ | ✗ | ✗ | ✗ | ✓ | **✓** |
| **NRPN Tools** | ✗ | ✗ | ✓ | ✓ | ✗ | **✓** |
| **MIDI Routing** | ✓ | ✗ | ✗ | ✓ | ✗ | **✓** |
| **MIDI Filtering** | ✓ | ✗ | ✗ | ✓ | ✗ | **✓** |
| **MIDI 2.0 Support** | ✗ | ✗ | ✗ | Partial | ✗ | **✓** |
| **Device Definitions** | ✗ | ✗ | ✓* | ✗ | Elektron only | **✓** |
| **Open Source** | ✗ | ✓ | ✗ | ✗ | ✗ | **✓** |
| **Active Development** | ✗ | ✓ | ✓ | ✓ | ✗ | **✓** |
| **Test Tone Generator** | ✓ | ✗ | ✗ | ✗ | ✗ | **✓** |
| **Hex Editor** | Basic | ✗ | ✗ | ✗ | ✗ | **✓** |

*MIDIQuest: Definitions are proprietary and per-device licensed ($19-$99 each)

### 5.3 Gap Analysis: What Users Are Missing

#### 5.3.1 Gaps in MIDI-OX

| Gap | User Impact | Our Solution |
|-----|-------------|--------------|
| Windows-only | Mac/Linux users excluded | Cross-platform from day one |
| No macOS Catalina+ support | Modern Mac users can't use | Native macOS build |
| Dated UI | "Interface can be confusing for beginners" | Modern, progressive disclosure UI |
| No development since ~2012 | No MIDI 2.0, no modern features | Active development roadmap |
| No SysEx decoding | Raw hex only | Definition-based plain-language decoding |
| No SDS support | Can't transfer samples | Full SDS implementation |

#### 5.3.2 Gaps in Snoize SysEx Librarian

| Gap | User Impact | Our Solution |
|-----|-------------|--------------|
| macOS-only | Windows/Linux users excluded | Cross-platform |
| No MIDI monitoring | Need separate app (MIDI Monitor) | Integrated monitoring |
| No SysEx editing | "Doesn't allow to compare or edit sysex files" | Hex editor with diff |
| No SysEx decoding | Just raw data | Definition engine |
| No routing/filtering | Single-purpose tool | Integrated routing |
| No SDS support | Sample transfers require different tool | Integrated SDS |

#### 5.3.3 Gaps in MIDIQuest

| Gap | User Impact | Our Solution |
|-----|-------------|--------------|
| Expensive ($99-$499) | Barrier to entry | Free and open source |
| Per-device licensing | "Editors are extremely overpriced" | All definitions included free |
| Dated interface | "Makes horrible use of screen real estate" | Modern responsive UI |
| Closed ecosystem | Can't contribute or customize | Open definition format |
| Touch-unfriendly | iPad app "not close to justifying per-synth editor cost" | Future: responsive web/touch UI |

#### 5.3.4 Gaps in Elektron C6

| Gap | User Impact | Our Solution |
|-----|-------------|--------------|
| 32-bit only | "Not supported under macOS 10.15 (Catalina) or later" | Modern 64-bit builds |
| Elektron-focused | Limited utility for other brands | Universal device support |
| No monitoring | SysEx only | Integrated monitoring |
| No active development | Feature-frozen | Active development |

#### 5.3.5 Gaps Across All Tools

| Universal Gap | Description | Our Solution |
|---------------|-------------|--------------|
| No unified solution | Users need 2-4 tools for complete workflow | Single application |
| No MIDI 2.0 | None support emerging standard | MIDI 2.0 UMP from v1.0 |
| No community definitions | Each tool is siloed | Open JSON definition format with repository |
| No plain-language decoding | Only MIDIQuest (paid, proprietary) | Free, extensible definitions |
| Linux excluded | Zero Linux options | Full Linux support |
| NRPN complexity | "Like cracking the enigma code" | Visual NRPN calculator |
| Sample dump fragmented | C6, Sample Wrench—none cross-platform modern | Cross-platform SDS |

### 5.4 User Feedback Synthesis

**Collected from forums, reviews, and community discussions:**

1. **Cross-platform is critical**: "If only MIDI-OX worked on Mac" / "Why is SysEx Librarian Mac-only?"
2. **Modern UI needed**: "Interface can be confusing for beginners" (MIDI-OX) / "Makes horrible use of screen real estate" (MIDIQuest)
3. **SysEx reliability**: "Random crashes and lockups" with large SysEx transfers
4. **Timing issues**: "If you have problems with transfers not making it over to the other side, increase delay"
5. **Definition coverage**: Users want support for obscure/vintage devices
6. **Price sensitivity**: "Editors are extremely overpriced" (MIDIQuest at $19-$99 per device)
7. **Documentation**: "Kemper's MIDI documentation needs to make this far easier. Ridiculous in 2025."
8. **NRPN complexity**: "I've never been able to fully trust NRPN"
9. **Abandonment concerns**: Multiple tools are unmaintained (MIDI-OX, C6)

### 5.5 Strategic Differentiation

| Differentiator | Rationale |
|----------------|-----------|
| **True cross-platform** | Only solution that treats all platforms equally |
| **Open source with open definitions** | Community can contribute and verify |
| **Unified feature set** | Eliminates need for multiple tools |
| **Modern UX** | Designed for 2025+, not 1995 |
| **MIDI 2.0 ready** | First utility with comprehensive M2 support |
| **Free** | No per-device licensing; democratizes access |
| **Extensible** | JSON definitions enable infinite device support |

---

## 6. Appendices

### Appendix A: JSON Definition Schema (Draft)

```json
{
  "$schema": "https://codexsonorum.org/schemas/device-definition-v1.json",
  "manufacturer": {
    "name": "Roland",
    "id": "0x41"
  },
  "device": {
    "name": "JV-2080",
    "model_id": "0x6A",
    "firmware_versions": ["1.00", "1.01", "1.02"]
  },
  "sysex": {
    "encoding": "roland_nibblized",
    "checksum": "roland",
    "address_bytes": 4,
    "commands": {
      "data_set_1": "0x12",
      "data_request_1": "0x11"
    }
  },
  "parameters": [
    {
      "address": "0x00000000",
      "name": "System Common",
      "children": [
        {
          "address": "0x00000000",
          "name": "Master Tune",
          "size": 1,
          "type": "signed",
          "min": 0,
          "max": 127,
          "display": {
            "offset": -64,
            "suffix": " cents"
          }
        }
      ]
    }
  ]
}
```

### Appendix B: Supported Manufacturer IDs (Initial)

| Manufacturer | ID (Hex) | Priority |
|--------------|----------|----------|
| Roland | 41 | P1 |
| Yamaha | 43 | P1 |
| Korg | 42 | P1 |
| Sequential/DSI | 01 | P1 |
| E-mu | 18 | P2 |
| Oberheim | 10 | P2 |
| Ensoniq | 0F | P2 |
| Akai | 47 | P2 |
| Waldorf | 3E | P2 |
| Access (Virus) | 00 20 33 | P2 |
| Elektron | 00 20 3C | P2 |
| Novation | 00 20 29 | P2 |
| Arturia | 00 20 6B | P3 |
| Nord | 00 20 27 | P3 |
| Behringer | 00 20 32 | P3 |

### Appendix C: Glossary

| Term | Definition |
|------|------------|
| **CC** | Control Change - MIDI message for continuous controllers |
| **LSB** | Least Significant Byte/Bits - lower portion of multi-byte value |
| **MSB** | Most Significant Byte/Bits - upper portion of multi-byte value |
| **NRPN** | Non-Registered Parameter Number - extended CC for device-specific parameters |
| **RPN** | Registered Parameter Number - standardized extended CC |
| **SDS** | Sample Dump Standard - MIDI protocol for audio sample transfer |
| **SysEx** | System Exclusive - MIDI message type for manufacturer-specific data |
| **UMP** | Universal MIDI Packet - MIDI 2.0 message format |

### Appendix D: References

1. MIDI 1.0 Specification - MIDI Association
2. MIDI 2.0 Specification - MIDI Association  
3. Universal System Exclusive Messages (MIDI 1.0)
4. Sample Dump Standard (SDS) - MIDI Association
5. Roland MIDI Implementation documentation
6. Korg MIDI Implementation documentation
7. Yamaha MIDI Data Format documentation

---

## Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | January 2026 | Graham | Initial draft |

---

*This document is released under Creative Commons Attribution 4.0 International (CC BY 4.0)*
