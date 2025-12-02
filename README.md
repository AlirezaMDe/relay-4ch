# relay-4ch

Reference project for 4-channel mains relay board (Altium Designer 20+)

## ⚠️ WARNING - SAFETY NOTICE

**THIS IS A REFERENCE DESIGN FOR EDUCATIONAL PURPOSES ONLY.**

This design involves **230VAC mains voltage** which can cause serious injury or death. Before using this design:

1. **DO NOT** use this design for production without professional review and certification
2. **MUST** be reviewed by a qualified electrical engineer
3. **MUST** comply with local electrical safety regulations
4. **MUST** be properly certified (CE, UL, etc.) before any commercial use
5. Ensure proper isolation, creepage, and clearance for your target application
6. Use appropriate enclosure and safety measures

## Project Overview

A 4-channel mains relay control board with optical isolation, designed for home automation and industrial control applications.

### Key Features

- 4 independent relay channels (SPDT, 10A @ 230VAC per channel)
- Optical isolation via PC817 optocouplers
- ULN2803A Darlington driver for relay coils
- Isolated DC-DC power supply (NME0505SC) for relay coils
- 3.3V MCU compatible input interface
- Per-channel fuse protection
- RC snubber circuits for contact protection
- MOV for transient voltage suppression
- Status LEDs for each channel
- Test points for debugging

### Specifications

| Parameter | Value |
|-----------|-------|
| Input Voltage (Logic) | 3.3V DC |
| Input Voltage (Relay Coils) | 5V DC (isolated) |
| Output Rating | 10A @ 230VAC per channel |
| Board Size | 120mm x 60mm |
| PCB Layers | 2 (Top + Bottom) |
| PCB Thickness | 1.6mm FR-4 |
| Copper Weight | 1 oz (35µm) |
| Creepage Distance | ≥8mm (mains to logic) |
| Air Clearance | ≥3.5mm (mains traces) |

## Circuit Description

### Signal Flow

```
MCU (3.3V) → Current Limiting Resistor → PC817 Optocoupler → ULN2803A → Relay Coil
                                                              ↓
                                              VCOIL_5V_ISO (Isolated 5V)
```

### Isolation Architecture

- **Logic Side (GND)**: MCU interface, optocoupler LED side
- **Isolated Side (GND_ISO)**: Relay coils, ULN2803A, DC-DC output
- **Optional**: Solder jumper SJ1 to connect GND and GND_ISO if isolation not required

### Power Supply

- Logic side: 3.3V from MCU header (J1)
- Relay coils: 5V isolated via NME0505SC DC-DC converter
- ULN2803A COM pin connected to VCOIL_5V_ISO for internal flyback diodes

## File Structure

```
relay-4ch/
├── relay_4ch.PrjPcb          # Altium project file
├── relay_4ch.SchDoc          # Schematic document
├── relay_4ch.PcbDoc          # PCB layout document
├── relay_4ch.SchLib          # Schematic symbol library
├── relay_4ch.PcbLib          # PCB footprint library
├── relay_4ch_project.zip     # Complete project archive
├── Project_Outputs/
│   ├── BOM/
│   │   └── BOM.csv           # Bill of Materials
│   ├── Gerber/
│   │   ├── relay_4ch-Top.GTL           # Top copper layer
│   │   ├── relay_4ch-Bottom.GBL        # Bottom copper layer
│   │   ├── relay_4ch-Top_Mask.GTS      # Top solder mask
│   │   ├── relay_4ch-Bottom_Mask.GBS   # Bottom solder mask
│   │   ├── relay_4ch-Top_Silk.GTO      # Top silkscreen
│   │   ├── relay_4ch-Bottom_Silk.GBO   # Bottom silkscreen
│   │   ├── relay_4ch-Top_Paste.GTP     # Top paste (stencil)
│   │   ├── relay_4ch-Board_Outline.GKO # Board outline
│   │   └── relay_4ch.DRL               # Drill file (Excellon)
│   └── Pick-and-Place/
│       └── relay_4ch_PnP.csv           # Pick and place file
└── README.md
```

## Component List (Key Parts)

| Designator | Part | Description |
|------------|------|-------------|
| K1-K4 | SRD-05VDC-SL-C | 5V SPDT Relay, 10A (Songle) |
| U1-U4 | PC817 | Optocoupler |
| U5 | ULN2803A | Darlington Transistor Array |
| PS1 | NME0505SC | 5V Isolated DC-DC Converter (Murata) |
| RV1 | 14D471K | Metal Oxide Varistor 470V |
| F1-F4 | 5x20mm Fuse | 10A Glass Fuse |
| SN1-SN4 | RC Snubber | 100nF X2 + 100Ω |
| J1 | Header 1x6 | MCU Interface |
| J2 | Terminal 2P | Mains Input (L/N) |
| J3-J6 | Terminal 3P | Relay Outputs (COM/NO/NC) |

## PCB Design Notes

### Safety Features

1. **Isolation Slot**: 2mm milled slot between mains and logic areas
2. **Creepage**: Minimum 8mm between mains and logic traces
3. **Clearance**: Minimum 3.5mm air gap for 230VAC traces
4. **Wide Traces**: 3.5-4mm trace width for mains current paths
5. **Ground Pour**: Bottom layer ground pour on logic side only

### Silkscreen Warnings

- "DANGER - MAINS VOLTAGE" near mains terminals
- "L" and "N" labels for mains input
- Channel labels (CH1-CH4) for outputs

## MCU Interface (J1)

| Pin | Name | Description |
|-----|------|-------------|
| 1 | VCC_3V3 | 3.3V Power Input |
| 2 | GND | Ground |
| 3 | IN1 | Channel 1 Control (Active High) |
| 4 | IN2 | Channel 2 Control (Active High) |
| 5 | IN3 | Channel 3 Control (Active High) |
| 6 | IN4 | Channel 4 Control (Active High) |

## Test Points

| Test Point | Signal |
|------------|--------|
| TP1 | VCC_3V3 |
| TP2 | GND |
| TP3 | VCOIL_5V_ISO |
| TP4-TP7 | K1-K4 Coil Voltage |

## Manufacturing Notes

- **Gerber Format**: RS-274X
- **Drill Format**: Excellon
- **Minimum Track Width**: 0.25mm (logic), 3.5mm (mains)
- **Minimum Via Hole**: 0.3mm
- **Surface Finish**: HASL or ENIG recommended

## License

This is a reference design provided as-is without warranty. Use at your own risk.

## Changelog

- v1.0 - Initial release
