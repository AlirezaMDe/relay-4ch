# relay-4ch

## 4-Channel Mains Relay Board - Altium Designer Reference Design

> ⚠️ **WARNING: MAINS VOLTAGE** ⚠️
> 
> This design handles 230VAC mains voltage. **This is a reference design only** and must be thoroughly reviewed, tested, and certified by qualified engineers before use with mains electricity. Improper handling of mains voltage can result in serious injury or death.

### Overview

This project contains a complete Altium Designer reference design for a 4-channel mains relay control board with optical isolation. The design is suitable for home automation, industrial control, and IoT applications requiring isolated switching of AC loads.

### Features

- **4 Independent Relay Channels**: Each channel can switch up to 10A @ 230VAC
- **Optical Isolation**: PC817 optocouplers provide galvanic isolation between control logic and mains
- **Isolated Power Supply**: Murata NME0505SC DC-DC converter for isolated relay coil power
- **Protection Circuitry**:
  - MOV (Metal Oxide Varistor) for overvoltage protection
  - Per-channel 10A fuses
  - RC snubber networks (100nF X2 + 100Ω) across relay contacts
- **3.3V Logic Compatible**: Direct interface with ESP32, STM32, Arduino, Raspberry Pi, etc.
- **Status LEDs**: Visual indication of each channel state
- **Test Points**: For VCC_3V3, GND, VCOIL_5V_ISO, and relay coils

### Technical Specifications

| Parameter | Value |
|-----------|-------|
| Board Size | 120mm × 60mm |
| Layers | 2 (Top + Bottom) |
| PCB Material | FR-4, 1.6mm, 1oz copper |
| Input Voltage | 3.3V DC (for logic and DC-DC) |
| Relay Type | Songle SRD-05VDC-SL-C (5V coil, SPDT) |
| DC-DC Converter | Murata NME0503SC (3.3V to 5V isolated) |
| Contact Rating | 10A @ 250VAC / 10A @ 30VDC |
| Isolation | 1kVDC (NME0505SC) |
| Control Interface | 6-pin header (3.3V, GND, IN1-IN4) |
| Creepage/Clearance | ≥8mm mains-to-logic, ≥3.5mm mains-to-mains |

### Safety Features

- **Milled Isolation Slot**: 2mm wide routed slot between logic and mains areas for enhanced creepage
- **Wide Trace Widths**: 3.5-4mm traces for mains current paths
- **X2 Safety Capacitors**: Rated for 275VAC continuous
- **Silkscreen Warnings**: "DANGER - MAINS VOLTAGE" prominently displayed
- **Separate Ground Domains**: GND (logic) and GND_ISO (mains) with optional solder jumper

### Project Structure

```
relay_4ch/
├── relay_4ch.PrjPcb       # Altium project file
├── relay_4ch.SchDoc       # Schematic document
├── relay_4ch.PcbDoc       # PCB layout document
├── Library/
│   ├── relay_4ch.SchLib   # Schematic symbols library
│   └── relay_4ch.PcbLib   # PCB footprints library
├── Output/
│   ├── BOM.csv            # Bill of Materials
│   ├── PickAndPlace.csv   # Pick and Place file
│   ├── relay_4ch_Outputs.OutJob  # Output job configuration
│   └── relay_4ch_project.zip     # Complete project archive
└── Gerber/
    ├── relay_4ch.GTL      # Top copper
    ├── relay_4ch.GBL      # Bottom copper
    ├── relay_4ch.GTS      # Top solder mask
    ├── relay_4ch.GBS      # Bottom solder mask
    ├── relay_4ch.GTO      # Top silkscreen
    ├── relay_4ch.GBO      # Bottom silkscreen
    ├── relay_4ch.GM1      # Mechanical/outline
    └── relay_4ch.DRL      # Drill file (Excellon)
```

### Key Components

| Designator | Part | Description |
|------------|------|-------------|
| U1-U4 | PC817 | Optocouplers for isolation |
| U5 | ULN2803A | 8-ch Darlington driver (4 used) |
| U6 | NME0503SC | Isolated DC-DC converter (3.3V to 5V) |
| K1-K4 | SRD-05VDC-SL-C | 5V SPDT relays |
| RV1 | 14D471K | MOV overvoltage protection |
| F1-F4 | 10A 5×20mm | Per-channel fuses |
| C1-C4 | 100nF X2 | Snubber capacitors |
| R9-R12 | 100Ω 2W | Snubber resistors |

### Design Notes

1. **ULN2803A COM Pin**: Connected to VCOIL_5V_ISO for internal flyback diode operation
2. **Ground Isolation**: GND and GND_ISO are separate by default; SJ1 allows optional connection
3. **Relay Placement**: Four relays arranged in a row for efficient routing
4. **Thermal Relief**: Applied to through-hole component pads in ground pour

### Usage

Connect the MCU header J1:
- Pin 1: VCC_3V3 (3.3V supply)
- Pin 2: GND
- Pins 3-6: IN1-IN4 (active-high control signals)

Drive INx HIGH to activate the corresponding relay channel.

### Manufacturing

- **Minimum feature size**: 0.25mm trace, 0.2mm clearance (signal)
- **Finish**: HASL recommended
- **Solder mask**: Green
- **Silkscreen**: White
- **Special**: Requires milled slot at X=48mm for isolation

### Certifications Required

Before use with mains voltage, this design should be reviewed for compliance with:
- IEC 60335-1 (Safety of household appliances)
- IEC 61010-1 (Safety of electrical equipment)
- UL 508 (Industrial control equipment)
- CE marking requirements (if applicable)

### License

This is a reference design provided for educational and prototyping purposes. Use at your own risk.

---

**⚠️ IMPORTANT: This design has NOT been certified for safety. Professional review and testing is required before any application involving mains voltage.**
