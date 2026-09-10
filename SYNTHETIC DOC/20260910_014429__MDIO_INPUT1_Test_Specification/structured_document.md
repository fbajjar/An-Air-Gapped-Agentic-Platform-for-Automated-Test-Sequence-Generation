## INDUSTRIAL TEST SPECIFICATION

MDIO-INPUT1  -  16-Channel 24V Digital Input Module

| Document No.                     | TS-MDIO-INPUT1-001          |
|----------------------------------|-----------------------------|
| Revision                         | A                           |
| 2026-06-30                       | Issue Date                  |
| Confidential - Internal Use Only | Classification              |
| Author                           | Hardware & Test Engineering |
| Status                           | RELEASED                    |

This document defines all acceptance tests required for the production release of the MDIO-INPUT1 PCB assembly.

## Document Revision History

| Rev   | Date       | Author         | Description of Change   |
|-------|------------|----------------|-------------------------|
| A     | 2026-06-30 | HW Engineering | Initial release         |

## Table of Contents

|    1 | Scope and Purpose                                          |
|------|------------------------------------------------------------|
|    2 | Referenced Standards and Documents                         |
|    3 | Device Under Test (DUT) Description                        |
|    4 | Test Equipment                                             |
|    5 | Safety Requirements                                        |
|    6 | Environmental Conditions                                   |
|    7 | Pre-Test Visual Inspection (VI-01)                         |
|    8 | Electrical Tests                                           |
|  8.1 | ET-01 - Power Supply & Reverse Polarity Protection         |
|  8.2 | ET-02 - VDD Rail Voltage Verification                      |
|  8.3 | ET-03 - Isolation / Hi-Pot (Dielectric Withstand)          |
|  8.4 | ET-04 - Insulation Resistance                              |
|  8.5 | ET-05 - I2C Device Detection & Address Configuration       |
|  8.6 | ET-06 - Digital Input OFF-State Threshold (IEC 61131-2 T1) |
|  8.7 | ET-07 - Digital Input ON-State Threshold (IEC 61131-2 T1)  |
|  8.8 | ET-08 - Input Current Verification                         |
|  8.9 | ET-09 - Full Channel Functional Test (All 16 Channels)     |
| 8.10 | ET-10 - Dual I2C Connector Verification                    |
|    9 | LED Chromaticity and Photometric Tests                     |
|  9.1 | LT-01 - Power Indicator LED D18 Chromaticity               |
|  9.2 | LT-02 - Channel Status LEDs D1-D16 Chromaticity            |
|  9.3 | LT-03 - Luminous Intensity & Forward Voltage               |
|   10 | Final Acceptance and Traceability                          |
|   11 | Test Record Form                                           |

## 1  Scope and Purpose

This specification defines the mandatory incoming inspection, electrical acceptance tests, and LED chromaticity/photometric tests required to validate every production unit of the MDIO-INPUT1 PCB assembly prior to shipment or integration into end systems.

The MDIO-INPUT1 is a 16-channel, 24 V DC digital input module intended for use in industrial automation and process-control environments. Its primary function is to receive field-side digital signals in the 24 V range, provide galvanic isolation via quad optocouplers, and report the logic states over a standard I²C bus using a PCF8575 16bit port expander. Two physical I²C connection options (pin header and flat cable/IDC) are provided.

All tests documented herein apply to Class 2 performance per IPC-A-610 unless otherwise stated. Tests are mandatory and must be completed in the sequence given. A unit that fails any test is quarantined and dispositioned before it may proceed to subsequent tests.

## 2  Referenced Standards and Documents

| Standard / Document      | Title / Scope                                                                              |
|--------------------------|--------------------------------------------------------------------------------------------|
| IEC 61131-2:2017         | Programmable controllers - Equipment requirements and tests (digital input Types 1/2/3)    |
| IPC-A-610J:2024          | Acceptability of Electronic Assemblies (visual workmanship, Class 2)                       |
| J-STD-001H:2020          | Requirements for Soldering Electrical and Electronic Assemblies                            |
| IEC 61010-1:2010         | Safety requirements for electrical equipment for measurement, control (clearance/creepage) |
| IEC 60068-2-1/2/14       | Environmental testing - Cold, dry heat, thermal shock (reference only)                     |
| CIE Publication 015:2018 | Colorimetry - CIE 1931 (x,y) chromaticity coordinate system                                |
| IEC 60073:2002           | Basic and safety principles - Coding for indicators and actuators (LED color meanings)     |
| IEC 62368-1:2018         | Audio/video, IT & comm equipment - Safety requirements                                     |
| LTV-844 Datasheet        | Lite-On Technology, Photocoupler 4-ch 5000 Vrms DIP-16 (DS-70-96-0013)                     |
| PCF8575 Datasheet        | Texas Instruments, 16-bit I²C/SMBus I/O expander (SCPS139D)                                |
| 1N5817 Datasheet         | Vishay, 1A Schottky Barrier Rectifier                                                      |

## 3  Device Under Test (DUT) Description

The MDIO-INPUT1 PCB is a single-sided SMD/THT mixed-technology assembly. The table below lists all key components extracted from the verified netlist:

| Ref    | Value / Part   | Package   | Function                                                              |
|--------|----------------|-----------|-----------------------------------------------------------------------|
| U1     | PCF8575DBR     | SSOP-24   | 16-bit I²C I/O expander; base I²C address 0x20 - 0x27 set by A0/A1/A2 |
| U2, U3 | LTV-844        | DIP-16    | Quad phototransistor optocoupler, 5000 Vrms isolation; channels 1-8   |
| U4, U5 | LTV-844        | DIP-16    | Quad phototransistor optocoupler, 5000 Vrms isolation; channels 9-16  |

| D1- D16    | LED (0805)                  | SMD           | Per-channel status indicators. Required color: YELLOW/AMBER ( λ _d 585-595 nm)         |
|------------|-----------------------------|---------------|----------------------------------------------------------------------------------------|
| D17        | 1N5817                      | DO-41         | Schottky diode; reverse-polarity protection on VDD supply rail                         |
| D18        | LED (0805)                  | SMD           | Power-on indicator. Required color: GREEN ( λ _d 520-535 nm)                           |
| R1- R16    | 2 k Ω                       | SMD 0805      | Series current-limiting resistors for 24 V optocoupler inputs (IEC 61131-2 compliance) |
| R17- R32   | 330 Ω                       | SMD 0805      | Collector pull-up / LED current-setting resistors for status LEDs D1-D16               |
| R33        | 330 Ω                       | SMD 0805      | Current-setting resistor for power indicator LED D18                                   |
| C1         | 100 nF                      | SMD 0805      | VDD bypass/decoupling capacitor                                                        |
| C2         | 10 µF                       | SMD 0805      | VDD bulk decoupling capacitor                                                          |
| J1         | 8-way terminal block        | PT-1.5 3.5 mm | 24 V field input channels 1-8 (OPTO_IN1-IN8)                                           |
| J10        | 9-way terminal block        | PT-1.5 3.5 mm | 24 V field input channels 9-16 + COM (field ground return)                             |
| J9         | 2-way terminal block        | PT-1.5 3.5 mm | 5 V DC power input (VDD+ and GND)                                                      |
| J6         | 4-pin header (right- angle) | 2.54 mm       | I²C bus connector option A - Pin header (VDD, SDA, SCL, GND)                           |
| J8         | 4-pin socket (right- angle) | 2.54 mm       | I²C bus connector option B - Pin socket (VDD, SDA, SCL, GND)                           |
| J4         | 2×4 IDC header              | 2.54 mm       | I²C bus connector option C - flat ribbon cable (VDD, GND×4, SCL, SDA, NC)              |
| J3, J5, J7 | 3-pin jumper header         | 2.54 mm       | Address configuration jumpers ADD0/ADD1/ADD2                                           |

## 3.1  Signal Path Architecture

Each of the 16 digital input channels follows an identical signal path:

- (1) 24 V field signal enters via J1 (ch. 1-8) or J10 (ch. 9-16) and returns to field via the COM terminal (J10 pin 9).
- (2) A 2 k Ω series resistor (R1-R16) limits the optocoupler LED forward current to ≈ 11.4 mA at 24 V nominal, complying with IEC 61131-2 Type 1.
- (3) The optocoupler LED (LTV-844, one of four channels per package) converts the field signal to an optically isolated phototransistor output on the 5 V logic side.
- (4) A 330 Ω pull-up resistor (R17-R32) connects VDD through a status LED (D1-D16) to the phototransistor collector. When the channel is active the transistor conducts, illuminating the LED and pulling the U1 port pin LOW.
- (5) U1 (PCF8575) reports all 16 channel states over I²C. Active input = logic 0 on the corresponding port pin (P00-P07 for channels 1-8; P10-P17 for channels 9-16).

## 3.2  I²C Address Map

The I²C 7-bit address is: 0b010 0 A2 A1 A0, i.e. base 0x20 plus the binary value of the three address jumpers (J7/J5/J3).

| ADD2 (J7)   | ADD1 (J5)   | ADD0 (J3)   | 7-bit Address (hex)   | Notes           |
|-------------|-------------|-------------|-----------------------|-----------------|
| GND         | GND         | GND         | 0x20                  | Factory default |
| GND         | GND         | VDD         | 0x21                  |                 |
| GND         | VDD         | GND         | 0x22                  |                 |
| GND         | VDD         | VDD         | 0x23                  |                 |
| VDD         | GND         | GND         | 0x24                  |                 |
| VDD         | GND         | VDD         | 0x25                  |                 |
| VDD         | VDD         | GND         | 0x26                  |                 |
| VDD         | VDD         | VDD         | 0x27                  | Max address     |

## 4  Test Equipment

The following calibrated instruments are required. All equipment must be within its calibration interval and traceable to national/international measurement standards (NIST or equivalent).

| ID                                           | Instrument                                                                      | Min. Specification                                    | Used In   |
|----------------------------------------------|---------------------------------------------------------------------------------|-------------------------------------------------------|-----------|
| TE- 01 Bench DC Power Supply                 | 0-30 V, 0-3 A, voltage accuracy ±0.1 V                                          | ET-01, ET-02, ET-06, ET-07,                           | ET-08     |
| TE- 02 Digital Multimeter (DMM)              | Resolution ≥ 4½ digits; voltage accuracy ±0.1%; current accuracy ±0.5%          | ET-01 through ET-04,                                  | ET-08     |
| Dielectric Withstand (Hi- Pot) Tester        | AC 0-3000 Vrms, leakage current resolution ≤ 0.1 mA; compliant with IEC 61010-1 | ET-03                                                 | TE- 03    |
| TE- 04 Insulation Resistance Tester          | 500 V DC / 1000 V DC resistance range ≥ 10 G Ω                                  | output; ET-04                                         |           |
| I²C Protocol Analyser / Bus Master           | 100 kHz / 400 kHz capable; read/write access; 0x20-0x27                         | address ET-05, ET-09, ET-10                           | TE- 05    |
| 5 V DC Regulated Supply                      | ±50 mV regulation; current ≥ 0.5 A                                              | limit ET-02, ET-05 through ET-10, series              | TE- 06 LT |
| Tristimulus Colorimeter or Spectroradiometer | CIE 1931 (x,y) wavelength range 380-780 accuracy Δ (x,y) ≤ 0.002                | measurement; nm; LT-01, LT-02                         | TE- 07    |
| 08 Luminance / Luminous- Intensity Meter     | Range: 0.1 mcd - accuracy ±10%                                                  | 1000 mcd; LT-03                                       | TE-       |
| TE- 09 Precision Resistance Box              | Decade                                                                          | 10 Ω - 100 k Ω ; accuracy ±0.1% ET-06, ET-07          |           |
| TE- 10                                       | Anti-Static Workstation & Wrist Strap                                           | Per IEC 61340-5-1; resistance to ground 10 ⁶ - 10 ⁹ Ω | All tests |

| TE- 11   | Oscilloscope (optional)   | Bandwidth ≥ 10 MHz; 2 channels with probe   | ET-05 (signal integrity)   |
|----------|---------------------------|---------------------------------------------|----------------------------|

## 5  Safety Requirements

The following safety precautions are MANDATORY. Failure to observe them may result in personal injury or destruction of the DUT.

- ESD protection: Handle DUTs only at an anti-static workstation. Wear a grounded wrist strap at all times (TE10). PCF8575 and SMD passive components are susceptible to ESD damage.
- Hi-Pot safety: The dielectric withstand test (ET-03) applies up to 1500 Vrms. Keep hands clear of the DUT and all leads during the test. Use safety-rated test probes with shrouded contacts (IEC 61010 Cat II minimum). Ensure the hi-pot tester (TE-03) has automatic discharge on completion.
- 24 V supply: When applying 24 V field voltage (ET-06, ET-07, ET-08, ET-09), ensure the power supply current limit is set to ≤ 200 mA before connection to prevent damage in the event of a short circuit.
- Power sequencing: Apply 5 V logic supply (TE-06) to J9 before applying 24 V field supply (TE-01) to J1/J10 inputs. Remove 24 V before removing 5 V supply.
- Isolation barrier integrity: Never connect the 24 V field ground (COM) to the 5 V logic GND during normal functional testing. These domains are isolated by the optocouplers. Bridging them invalidates hi-pot test results and risks damaging isolation.
- Optical measurement: When performing LED chromaticity tests, do not look directly into lit LEDs. Use the colorimeter/spectroradiometer contact adapter. Ambient light in the measurement area must be &lt; 1 lux during photometric measurements.

## 6  Environmental Conditions

All tests shall be performed under the following ambient conditions unless a specific test states otherwise:

| Parameter                 | Requirement                                                         |
|---------------------------|---------------------------------------------------------------------|
| Ambient Temperature       | 20 °C ± 5 °C                                                        |
| Relative Humidity         | 45% - 75 %RH(non-condensing)                                        |
| Atmospheric Pressure      | 860 hPa - 1060 hPa                                                  |
| Magnetic / RF environment | No known EMI sources within 1 m of DUT during hi-pot or I²C tests   |
| Warm-up time              | DUT must be powered at 5 V for ≥ 2 minutes before I²C and LED tests |

## 7  Pre-Test Visual Inspection  (VI-01)

Visual inspection shall be performed per IPC-A-610J Class 2 criteria using ×3 to ×10 magnification. This inspection must be completed and recorded BEFORE any powered test is applied to the DUT.

| Item    | Inspection Point   | Accept Criterion (IPC-A-610J Cl.2)                                                                                                                                  |
|---------|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| VI- 01a | PCB substrate      | No delamination, measling, crazing, blistering, or burns on any laminate layer. Board edge must be free of chipping greater than 0.5 mm into the copper/trace area. |

| VI- 01b                                         | SMD solder joints (R1-R33, C1-C2, D1-D16, D18, U1)                                                                                                                                     | Solder fillet present on ≥ 75 %of pad length (minimum toe fillet). No bridging, cold joints, or solder balls. Wetting angle ≤ 90°. No lifted pads. No visible voids exceeding 25 %of joint area.   |
|-------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Through-hole solder joints (U2- U5, D17)        | 100% hole fill (solder visible on component side). Lead protrusion 0.5 mm - 2.5 mm after trim. Concave fillet on solder side. No rough or grainy solder.                               | VI- 01c                                                                                                                                                                                            |
| Component presence & orientation                | All 74 components (per BOM) present and populated. LEDs D1- D18 polarity correct (cathode marking / dot toward GND pad per silkscreen). Electrolytic-like C2 polarity marking correct. | VI- 01d                                                                                                                                                                                            |
| Component value verification (sampling)         | Random sample of ≥ 5 SMD resistors: confirm resistor code matches BOM (2k Ω = 2001; 330 Ω = 331). LED package type 0805 confirmed by footprint measurement.                            | VI- 01e                                                                                                                                                                                            |
| Terminal blocks J1, J9, J10                     | Screws present and not pre-torqued. Connectors seated flush with PCB. No cracked or chipped housing. All wire-entry holes clear of solder.                                             | VI- 01f                                                                                                                                                                                            |
| Pin headers / sockets J3,J5,J6,J7,J8 and IDC J4 | Pins straight and not bent. Socket J8 contacts not spread or deformed. IDC J4 strain-relief bar present and fully engaged.                                                             | VI- 01g                                                                                                                                                                                            |
| Silkscreen and marking                          | Board reference silkscreen legible. Document/revision marking visible. Channel labels CH1-CH16 on J1/J10 legible.                                                                      | VI- 01h                                                                                                                                                                                            |
| Cleanliness                                     | No visible flux residue, contamination, solder balls free on board, or foreign material. No conductive particles visible with naked eye.                                               | VI- 01i                                                                                                                                                                                            |

Disposition: Any defect meeting IPC-A-610J Class 2 'Defect' condition is cause for immediate rejection. 'Process Indicator' conditions must be logged and reviewed by the Engineering team within 24 hours.

## 8  Electrical Tests

## ET-01  -  Power Supply &amp; Reverse-Polarity Protection

| Objective   | Verify the power input circuit accepts 5 V DC in correct polarity, that VDD is correctly distributed on board, and that reverse polarity is blocked by D17.   |
|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| References  | D17 (1N5817), J9, C1, C2; Board netlist: J9 pin2 → D17 anode → D17 cathode → VDD rail                                                                         |

|   Step | Action / Stimulus                                                                                                              | Measurement / Observation                                 | Accept Criterion                                                   |
|--------|--------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|--------------------------------------------------------------------|
|      1 | Set TE-01 to +5.0 V, current limit 200 mA. Connect (+) to J9 pin 2, (-) to J9 pin 1 (GND). Do NOT connect I²C or field inputs. | Measure VDD at C1 pad 2 vs. C1 pad 1 (GND).               | 4.7 V ≤ VDD ≤ 5.3 V. Supply current ≤ 80 mA (idle).                |
|      2 | Record supply current from TE-01 display.                                                                                      | Current reading.                                          | Idle current: 5 mA ≤ I_idle ≤ 80 mA (D18 lit + PCF8575 quiescent). |
|      3 | Remove power. Reverse polarity: connect (-) to J9 pin 2, (+) to J9 pin 1. Apply 5 V.                                           | Measure VDD at C1 pad 2. Observe power indicator LED D18. | VDD < 0.5 V (D17 blocking). D18 must be OFF. Supply current < 2 mA |

|    |                                                             |                                        | (leakage only). No damage.   |
|----|-------------------------------------------------------------|----------------------------------------|------------------------------|
|  4 | Remove power. Restore correct polarity for remaining tests. | Visual confirmation of correct wiring. | Correct polarity confirmed.  |

## ET-02  -  VDD Rail Voltage Verification

| Objective   | Confirm VDD rail accuracy across the PCB under loaded conditions representative of all channels active.   |
|-------------|-----------------------------------------------------------------------------------------------------------|
| References  | PCF8575 VDD: 2.5 V-5.5 V per datasheet; nominal operating point 5.0 V                                     |

|   Step | Action / Stimulus                                                                                       | Measurement / Observation                           | Accept Criterion                                  |
|--------|---------------------------------------------------------------------------------------------------------|-----------------------------------------------------|---------------------------------------------------|
|      1 | Power DUT with 5.0 V at J9 (correct polarity). All field inputs disconnected.                           | Measure VDD at U1 pin 24 vs. GND (U1 pin 12 or 24). | 4.75 V ≤ VDD ≤ 5.25 V.                            |
|      2 | Simultaneously apply a 24 V signal to all 16 input channels (all optocouplers conducting). Measure VDD. | Measure VDD at U1 pin 24 under full load.           | VDD ≥ 4.65 V ( ≤ 350 mV drop from TE-06 setting). |
|      3 | Record min VDD seen during step 2.                                                                      | Min VDD reading.                                    | Min VDD ≥ 4.65 V.                                 |

## ET-03  -  Isolation / Hi-Pot (Dielectric Withstand Test)

| Objective   | Verify that the galvanic isolation barrier between the 24 V field side and 5 V logic side can withstand 1500 Vrms AC for 1 second without breakdown, consistent with the LTV-844 5000 Vrms rated isolation voltage (production test at 30 %of rated).   |
|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| References  | LTV-844 V_iso = 5000 Vrms; IEC 61010-1; production hi-pot practice                                                                                                                                                                                      |

WARNING: This test uses hazardous voltages. Follow all safety precautions in Section 5. Confirm the hipot tester (TE-03) is in calibration and has a working ground-fault interrupt.

|   Step | Action / Stimulus                                                                                                                                    | Measurement / Observation                | Accept Criterion                                                     |
|--------|------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------|----------------------------------------------------------------------|
|      1 | Disconnect all external connections from J1, J9, J10, J6, J8, J4 (DUT completely isolated).                                                          | Visual confirmation.                     | All external connectors free.                                        |
|      2 | Short all field-side terminals together: J1 pins 1-8, J10 pins 1-9 (including COM). Connect to HV terminal of TE-03.                                 | Confirm short is secure.                 | Field-side bus formed.                                               |
|      3 | Short all logic-side terminals together: J9 pin 1 (GND), J9 pin 2 (VDD), J6 pins 1-4, J8 pins 1-4, J4 pins 1-8. Connect to LV/GND terminal of TE-03. | Confirm short is secure.                 | Logic-side bus formed.                                               |
|      4 | Set TE-03 to AC test mode, voltage ramp: 0 → 1500 Vrms over 5 seconds, hold 1500 Vrms for 1 second, leakage current trip threshold: 10 mA.           | Monitor leakage current throughout test. | Leakage current < 10 mA at all times. No arc, no flashover, no trip. |

|   5 | Discharge DUT via TE-03 built-in discharge function. Confirm discharge complete before touching DUT.   | TE-03 discharge status.   | Discharge confirmed (< 30 V residual).       |
|-----|--------------------------------------------------------------------------------------------------------|---------------------------|----------------------------------------------|
|   6 | Inspect DUT for signs of damage (burn marks, delamination, component damage).                          | Visual inspection.        | No visible damage. DUT appearance unchanged. |

## ET-04  -  Insulation Resistance

| Objective   | Confirm that the DC insulation resistance between field and logic domains meets minimum requirements after the hi-pot test.   |
|-------------|-------------------------------------------------------------------------------------------------------------------------------|
| References  | IEC 61010-1: minimum insulation resistance typically ≥ 100 M Ω at rated test voltage                                          |

|   Step | Action / Stimulus                                                               | Measurement / Observation                               | Accept Criterion   |
|--------|---------------------------------------------------------------------------------|---------------------------------------------------------|--------------------|
|      1 | Use the same field-side and logic-side bus shorts as ET-03 (or re-create them). | -                                                       | -                  |
|      2 | Apply TE-04 at 500 V DC between field-side bus and logic-side bus.              | Measure insulation resistance after 60 s stabilisation. | R_iso ≥ 100 M Ω .  |
|      3 | Discharge per TE-04 procedure before disconnecting.                             | Discharge confirmation.                                 | Complete.          |

## ET-05  -  I²C Device Detection &amp; Address Configuration

| Objective   | Verify U1 (PCF8575) responds on the I²C bus at the expected address for all 8 jumper combinations (J3/J5/J7), and that SDA and SCL signal integrity is acceptable.   |
|-------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| References  | PCF8575 datasheet; I²C specification 400 kHz Fast Mode; J3 (ADD0), J5 (ADD1), J7 (ADD2)                                                                              |

|   Step | Action / Stimulus                                                                                                                         | Measurement / Observation                       | Accept Criterion                                        |
|--------|-------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|---------------------------------------------------------|
|      1 | Power DUT at 5 V via J9. Connect I²C bus master (TE-05) to J6 pins: pin1=VDD, pin2=SDA, pin3=SCL, pin4=GND (or equivalent via J8 socket). | Confirm bus master powered and TE-05 connected. | Bus connected.                                          |
|      2 | Set J3, J5, J7 all to GND (address = 0x20). Issue an I²C address scan at 100 kHz.                                                         | Record device detected at 0x20.                 | ACK received at 0x20. No spurious devices on bus.       |
|      3 | Perform a 2-byte read from U1 at 0x20. All inputs floating HIGH (PCF8575 internal pull- up).                                              | Read 16-bit port value.                         | Both bytes = 0xFF (all ports HIGH = no active channel). |
|      4 | Change jumper to ADD0=VDD (J3 center to VDD pin). Repeat scan.                                                                            | Detect device.                                  | ACK received at 0x21. No response at 0x20.              |

|   5 | Test remaining 6 jumper combinations (0x22 through 0x27). For each, issue scan and verify correct address.   | Record detected address for each jumper combination.                         | Correct address ACK for all 8 combinations. No address collision.              |
|-----|--------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------|--------------------------------------------------------------------------------|
|   6 | With address at 0x20, measure SDA and SCL waveforms on oscilloscope (TE-11) at 400 kHz (Fast Mode).          | Waveform measurements: high level (V_OH), low level (V_OL), rise time (t_r). | V_OH ≥ 0.7×VDD = 3.5 V; V_OL ≤ 0.3×VDD = 1.5 V; t_r ≤ 300 ns (1 k Ω pull- up). |
|   7 | Restore J3/J5/J7 to GND (0x20) for remaining tests.                                                          | Confirm address 0x20.                                                        | 0x20 confirmed.                                                                |

## ET-06  -  Digital Input OFF-State Threshold (IEC 61131-2 Type 1)

| Objective   | Verify that every channel reads logic 1 (inactive) when the input voltage is at or below the Type 1 OFF threshold of 5 V DC, confirming the optocoupler does not falsely trigger.   |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| References  | IEC 61131-2:2017 Table 7 - Type 1, 24V DC: V_OFF(max) = 5 V, I_IN(OFF) ≤ 15 mA                                                                                                      |

Test is performed channel by channel. Connect TE-01 (+5 V) between the channel terminal (J1 pin N or J10 pin N) and COM (J10 pin 9) via TE-09 (set to 0 Ω bypass). Power DUT at 5 V.

|   Step | Action / Stimulus                                                        | Measurement / Observation                           | Accept Criterion                                                                      |
|--------|--------------------------------------------------------------------------|-----------------------------------------------------|---------------------------------------------------------------------------------------|
|      1 | Apply V_IN = 0 V to CH1 (J1 pin 1 to COM). Read CH1 via I²C.             | P00 bit value in PCF8575 read data.                 | P00 = 1 (inactive). D1 is OFF.                                                        |
|      2 | Ramp V_IN from 0 V to 5.0 V in 0.5 V steps. At each step read CH1 bit.   | Note voltage at which CH1 first reads '0' (active). | CH1 must remain '1' (inactive) at all voltages ≤ 5.0 V (exclusive). No false trigger. |
|      3 | Measure I_IN at V_IN = 5 V.                                              | Current reading (TE-02 series ammeter).             | I_IN(5V) ≤ 15 mA (IEC 61131-2 Type 1 requirement).                                    |
|      4 | Repeat steps 1-3 for each of the 16 channels (CH2-CH16). Record results. | 16 independent measurements.                        | All 16 channels: P00- P07, P10-P17 remain '1' at V_IN ≤ 5 V.                          |

## ET-07  -  Digital Input ON-State Threshold (IEC 61131-2 Type 1)

| Objective   | Verify that every channel reads logic 0 (active) when V_IN ≥ 15 V, and that it reports active at the nominal 24 V supply.   |
|-------------|-----------------------------------------------------------------------------------------------------------------------------|
| References  | IEC 61131-2:2017 Table 7 - Type 1, 24V DC: V_ON(min) = 15 V, V_nom = 24 V, V_max = 30 V                                     |

|   Step | Action / Stimulus                                               | Measurement / Observation   | Accept Criterion                               |
|--------|-----------------------------------------------------------------|-----------------------------|------------------------------------------------|
|      1 | Apply V_IN = 15.0 V to CH1 (J1 pin 1 to COM). Read CH1 via I²C. | P00 bit value.              | P00 = 0 (active). D1 must be ON (illuminated). |

|   2 | Apply V_IN = 24.0 V to CH1. Read CH1.                                                     | P00 bit value.              | P00 = 0 (active). D1 ON.                                        |
|-----|-------------------------------------------------------------------------------------------|-----------------------------|-----------------------------------------------------------------|
|   3 | Apply V_IN = 30.0 V (maximum operating voltage) to CH1. Read CH1. Monitor supply current. | P00 value and I_IN reading. | P00 = 0. I_IN(30V) ≤ 15 mA (= (30- 1.2)/2000 ≈ 14.4 mA). D1 ON. |
|   4 | Repeat steps 1-3 for all 16 channels. Verify each channel independently.                  | 16 × 3 measurements.        | All channels: active (0) at 15 V, 24 V, and 30 V.               |

## ET-08  -  Input Current Verification (IEC 61131-2 Type 1 Compliance)

| Objective   | Measure actual input current at nominal operating conditions (24 V) to confirm compliance with IEC 61131-2 Type 1 current band and verify correct resistor values R1-R16.   |
|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| References  | R1-R16 = 2 k Ω ; LTV-844 V_F ≈ 1.2 V; IEC 61131-2 Type 1: I_IN(24V) = 2-15 mA                                                                                               |

Expected: I\_IN = (24 V -V\_F\_opto) / R\_series ≈ (24 -1.2) / 2000 ≈ 11.4 mA.

|   Step | Action / Stimulus                                                                                  | Measurement / Observation   | Accept Criterion                                  |
|--------|----------------------------------------------------------------------------------------------------|-----------------------------|---------------------------------------------------|
|      1 | Insert TE-02 (ammeter mode) in series between TE-01 (+24 V) and CH1 terminal. Apply V_IN = 24.0 V. | Record I_IN for CH1.        | 9.0 mA ≤ I_IN ≤ 14.0 mA.                          |
|      2 | Repeat for all 16 channels.                                                                        | Record I_IN per channel.    | All 16 channels: 9.0 mA ≤ I_IN ≤ 14.0 mA.         |
|      3 | Check that all 16 values agree within ±15% of each other.                                          | Calculate max deviation.    | Max channel-to- channel deviation ≤ ±15 %of mean. |

## ET-09  -  Full Channel Functional Test (All 16 Channels)

| Objective   | Confirm that each channel independently and correctly maps its 24 V field state to the correct bit in the PCF8575 I²C register, with correct status LED behaviour, using channel walking and all-ON / all-OFF patterns.   |
|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| References  | U1 PCF8575 port map: P00-P07 = CH1-CH8 (byte 0); P10-P17 = CH9-CH16 (byte 1)                                                                                                                                              |

|   Step | Action / Stimulus                                                                                                               | Measurement / Observation         | Accept Criterion                                                      |
|--------|---------------------------------------------------------------------------------------------------------------------------------|-----------------------------------|-----------------------------------------------------------------------|
|      1 | Power DUT, all channels disconnected. Read I²C. Record baseline.                                                                | Baseline read value.              | Bytes 0 and 1 both = 0xFF (all inactive). All D1-D16 OFF.             |
|      2 | Activate CH1 only (24 V to J1 pin 1 / COM). Read I²C byte 0.                                                                    | Byte 0 value, D1 status.          | Byte 0 = 0xFE (bit 0 = 0). D1 lit. All other LEDs off. Byte 1 = 0xFF. |
|      3 | Walking 1 test: successively move the active channel from CH1 to CH16. After each channel activation, verify the correct bit is | 16 × (I²C byte pair, LED status). | For channel N: correct bit in correct                                 |

|    | zero and only that bit; verify corresponding LED is lit.                                         |                                                 | byte = 0. All other 15 bits = 1. Only D_N lit.                                                                  |
|----|--------------------------------------------------------------------------------------------------|-------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
|  4 | All-ON test: simultaneously apply 24 V to all 16 channels.                                       | Read I²C byte pair; visual LED check.           | Byte 0 = 0x00, Byte 1 = 0x00. All 16 LEDs D1-D16 illuminated simultaneously.                                    |
|  5 | All-OFF: remove 24 V from all channels.                                                          | Read I²C byte pair.                             | Byte 0 = 0xFF, Byte 1 = 0xFF. All LEDs off.                                                                     |
|  6 | Verify INT pin (U1 pin 1) transitions low upon any channel change. Connect TE-02 to INT and GND. | INT voltage: baseline and during channel event. | at baseline. INT ≤ 0.8 V within 50 µs of channel state change (open-drain with external pull-up on I²C master). |

## ET-10  -  Dual I²C Connector Verification (J6 Pin Header / J8 Socket / J4 IDC)

| Objective   | Confirm that both I²C connection options (J6 pin header and J8 socket) as well as the IDC flat cable connector J4 present identical bus behaviour with correct pin mapping.   |
|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| References  | J6: pins 1=VDD, 2=SDA, 3=SCL, 4=GND; J8: same pinout (socket); J4 (IDC 2×4): pin1=VDD, pin2=GND, pin3=SCL, pin4=GND, pin5=SDA, pin6=GND, pin7=GND, pin8=NC                    |

|   Step | Action / Stimulus                                                                                                              | Measurement / Observation   | Accept Criterion                                               |
|--------|--------------------------------------------------------------------------------------------------------------------------------|-----------------------------|----------------------------------------------------------------|
|      1 | Connect TE-05 to J6 (pin header). Apply 5 V. Issue I²C read at 0x20. Activate CH1 (24 V).                                      | Read byte 0 via J6.         | Byte 0 = 0xFE. Correct data via J6.                            |
|      2 | Disconnect TE-05 from J6. Connect to J8 (socket). Repeat read.                                                                 | Read byte 0 via J8.         | Byte 0 = 0xFE. Correct data via J8. SDA/SCL on identical nets. |
|      3 | Disconnect TE-05 from J8. Connect flat cable to J4 IDC header. Verify pin 5=SDA, pin 3=SCL, pin 1=VDD, pin 2=GND. Repeat read. | Read byte 0 via J4.         | Byte 0 = 0xFE. Correct data via J4 IDC.                        |
|      4 | Measure SDA and SCL signal levels on J4 under 400 kHz bus activity.                                                            | V_OH and V_OL on J4 pins.   | V_OH ≥ 3.5 V, V_OL ≤ 1.5 V (same as ET- 05 step 6).            |

## 9  LED Chromaticity and Photometric Tests

Per IEC 60073:2002, indicator lamp colors carry normative safety meanings in industrial equipment. The MDIOINPUT1 assigns specific colors to its LEDs to convey unambiguous status information:

| Ref    | Color          | IEC 60073 Meaning      | Application on MDIO-INPUT1                      |
|--------|----------------|------------------------|-------------------------------------------------|
| D18    | GREEN          | Normal / Safe / ON     | 5 V logic supply present and operational        |
| D1-D16 | YELLOW / AMBER | Caution / Active state | 24 V field signal present on that input channel |

The CIE 1931 (x, y) chromaticity test uses a calibrated colorimeter or spectroradiometer (TE-07) with the probe placed flush against the LED lens at a defined measurement distance (contact or 5 mm ± 1 mm). All measurements shall be performed in ambient light &lt; 1 lux (darkened test enclosure). Forward current is set at I\_F = 10 mA using TE-06 with a series precision resistor calculated per the DUT supply voltage. This current is representative of the in-circuit operating current ( ≈ 8-10 mA).

## 9.1  LT-01 - Power Indicator LED D18 Chromaticity (GREEN)

| Required Color      | GREEN - IEC 60073 code; assigned to 'Normal / Power Present' function                           |
|---------------------|-------------------------------------------------------------------------------------------------|
| Dominant Wavelength | λ _d = 520 nm - 535 nm                                                                          |
| CIE 1931 x          | 0.050 ≤ x ≤ 0.185                                                                               |
| CIE 1931 y          | 0.640 ≤ y ≤ 0.780                                                                               |
| Purity (min)        | Excitation purity ≥ 90 %(highly saturated, no appreciable white admixture)                      |
| Test current I_F    | 10 mA ± 0.5 mA DC, applied via precision bench supply and series 300 Ω resistor from 5 V source |

|   Step | Action / Stimulus                                                                                                                                                      | Measurement / Observation                       | Accept Criterion                                  |
|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------|---------------------------------------------------|
|      1 | Power TE-06 to 5 V. Connect in series with 300 Ω precision resistor and D18 anode/cathode pads (lift one end of R33 or probe directly). Set current to 10 mA ± 0.5 mA. | Confirm I_F with TE-02.                         | I_F = 10 mA ± 0.5 mA.                             |
|      2 | Place TE-07 probe flush with D18 lens. Darken ambient to < 1 lux. Acquire measurement ( ≥ 3 readings, average).                                                        | Record CIE (x, y) and dominant wavelength λ _d. | x: 0.050-0.185; y: 0.640-0.780; λ _d: 520-535 nm. |
|      3 | Verify the measured (x, y) point lies within the GREEN acceptance polygon (points: (0.050, 0.640), (0.185, 0.640), (0.185, 0.780), (0.050, 0.780)).                    | Plot or compute against acceptance window.      | Point within acceptance polygon. PASS.            |
|      4 | If any reading falls outside the polygon, reject. Log measurement data to test record.                                                                                 | Log data.                                       | See test record form.                             |

## 9.2  LT-02 - Channel Status LEDs D1-D16 Chromaticity (YELLOW / AMBER)

| Required Color                                                    | YELLOW / AMBER - IEC 60073 code; assigned to 'Active state / Channel ON' function   |
|-------------------------------------------------------------------|-------------------------------------------------------------------------------------|
| Dominant Wavelength                                               | λ _d = 580 nm - 600 nm                                                              |
| 0.400 ≤ x ≤ 0.540                                                 | CIE 1931 x                                                                          |
| 0.400 ≤ y ≤ 0.560                                                 | CIE 1931 y                                                                          |
| Excitation purity ≥ 85 %(high saturation, clearly distinguishable | Purity (min) from orange and green)                                                 |
| Test current I_F 10 mA ± 0.5 mA DC; applied pads                  | via 5 V supply, 300 Ω series resistor, direct to status LED                         |

Sampling plan

100 % test of all 16 channel LEDs (D1-D16). Production volumes &lt; 100 units/batch: full test. ≥ 100 units: AQL 1.5 Level II with Reduced Inspection (7-unit sample minimum).

|   Step | Action / Stimulus                                                                                                                                         | Measurement / Observation     | Accept Criterion                                                                                            |
|--------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------|-------------------------------------------------------------------------------------------------------------|
|      1 | Power DUT at 5 V. Drive CH1 active (24 V on J1 pin 1). D1 lights via normal circuit path at ≈ 8-10 mA.                                                    | Confirm D1 is lit.            | D1 illuminated.                                                                                             |
|      2 | Place TE-07 probe on D1. Acquire CIE (x,y) and λ _d. Record.                                                                                              | CIE (x, y) and λ _d for D1.   | x: 0.400-0.540; y: 0.400-0.560; λ _d: 580-600 nm.                                                           |
|      3 | Repeat step 1-2 for each of D2-D16 by activating each channel in turn. Record each (x, y, λ _d).                                                          | 16 individual measurements.   | All 16 LEDs pass chromaticity window.                                                                       |
|      4 | Check all 16 (x, y) values lie within the YELLOW/AMBER acceptance quadrilateral (points: (0.400, 0.400), (0.540, 0.400), (0.540, 0.560), (0.400, 0.560)). | Pass/fail per LED.            | All 16 within polygon. Any failure = batch HOLD, notify Engineering.                                        |
|      5 | Optionally compare D1 to D16 chromaticity scatter: max range of x values ≤ 0.060; max range of y values ≤ 0.060.                                          | Chromaticity spread analysis. | Within-batch consistency: Δ x ≤ 0.060, Δ y ≤ 0.060. This is a process indicator, not a hard fail criterion. |

## 9.3  LT-03 - Luminous Intensity and LED Forward Voltage

|   Step | Action / Stimulus                                                                               | Measurement / Observation    | Accept Criterion                                                                   |
|--------|-------------------------------------------------------------------------------------------------|------------------------------|------------------------------------------------------------------------------------|
|      1 | At I_F = 10 mA: measure luminous intensity of D18 using TE-08 at probe-to-LED distance 5 mm.    | Record I_v (mcd) for D18.    | I_v ≥ 2 mcd at I_F = 10 mA (minimum visibility under ambient industrial lighting). |
|      2 | At I_F = 10 mA: measure luminous intensity of D1, D4, D8, D12, D16 (5 representative channels). | Record I_v (mcd) for 5 LEDs. | All 5: I_v ≥ 2 mcd at I_F = 10 mA.                                                 |
|      3 | Measure forward voltage V_F of D18 at I_F = 10 mA.                                              | Record V_F (V).              | 1.8 V ≤ V_F ≤ 3.5 V (covers all visible LED chemistries).                          |
|      4 | Measure V_F of D1, D4, D8, D12, D16 at I_F = 10 mA.                                             | Record V_F per LED.          | 1.8 V ≤ V_F ≤ 3.5 V for all. Channel-to- channel variation ≤ 0.3 V.                |

## 10  Final Acceptance and Traceability

A unit is considered a PASS only when ALL of the following conditions are satisfied:

1. VI-01: All visual inspection items received a PASS disposition (no IPC-A-610J Class 2 Defect recorded).

2. ET-01 through ET-10: All electrical tests passed all step criteria without any substitution or re-test (one re-test per step permitted after documented rework, but original and re-test results must both be logged).
3. LT-01 through LT-03: All LED chromaticity coordinates fall within the defined acceptance polygons, and luminous intensity meets minimum values.
4. Test Record Form (Section 11) is fully completed, legible, and signed by the test technician and a second authorized reviewer.

| Traceability Requirement   | Detail                                                                                                                                                                                                                                     |
|----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Unit serialisation         | Each DUT must carry a unique serial number (laser-etched or permanent label, min 6 characters alphanumeric) affixed to the PCB before testing begins.                                                                                      |
| Calibration status         | All test equipment IDs and calibration expiry dates must be recorded on the Test Record Form prior to commencing tests.                                                                                                                    |
| Non-conformances           | Failed units must be tagged with a red HOLD label, assigned an NCR (Non- Conformance Report) number, and stored in a quarantine location. They must not be re-inserted into the production flow without a written Engineering disposition. |
| Record retention           | Completed test records shall be retained for a minimum of 10 years per IEC 61010-1 product liability recommendations.                                                                                                                      |

## 11  Test Record Form

| Unit Serial No.   | PCB Part No.   | MDIO-INPUT1   |
|-------------------|----------------|---------------|
| Test Date         | Test Spec Rev. | A             |
| Technician Name   | Reviewer Name  |               |
| Ambient Temp (°C) | Humidity (%RH) |               |

## Equipment Used:

| TE- ID   | Instrument / Model   | Calibration Cert No.   | Cal Expiry   |
|----------|----------------------|------------------------|--------------|
| TE- 01   |                      |                        |              |
| TE- 02   |                      |                        |              |
| TE- 03   |                      |                        |              |
| TE- 04   |                      |                        |              |
| TE- 05   |                      |                        |              |
| TE- 06   |                      |                        |              |
| TE- 07   |                      |                        |              |
| TE- 08   |                      |                        |              |
| TE- 09   |                      |                        |              |

## Test Results:

| Test ID                                | Parameter / Measurement   | Measured Value Accept Criterion   | P / F   |
|----------------------------------------|---------------------------|-----------------------------------|---------|
| Visual inspection - IPC-A-610J Class 2 | -                         | No Defect conditions              | VI-01   |
| VDD at C1 (5V applied)                 | ___ V                     | 4.7-5.3 V                         | ET-01a  |
| Idle supply current                    | ___ mA                    | 5-80 mA                           | ET-01b  |
| VDD at reverse polarity                | ___ V                     | < 0.5 V                           | ET-01c  |
| VDD at U1 pin 24 (no load)             | ___ V                     | 4.75-5.25 V                       | ET-02a  |
| VDD at U1 pin 24 (all ch active)       | ___ V                     | ≥ 4.65 V                          | ET-02b  |
| Hi-Pot 1500 Vrms × 1 s - leakage       | ___ mA                    | < 10 mA; no trip                  | ET-03   |
| Insulation resistance @500 V DC        | ___ M Ω                   | ≥ 100 M Ω                         | ET-04   |
| I²C ACK at 0x20                        | ACK / NACK                | ACK                               | ET-05a  |

| ET-05b                           | I²C ACK all 8 addresses (0x20- 0x27)   | Pass / Fail All 8 pass       |        |
|----------------------------------|----------------------------------------|------------------------------|--------|
| SDA/SCL V_OH @400 kHz            | ___ V                                  | ≥ 3.5 V                      | ET-05c |
| OFF threshold - all 16 ch@5V     | Pass / Fail                            | All remain 1 at ≤ 5V         | ET-06  |
| ON threshold - all 16 ch @15 V   | Pass / Fail                            | All read 0 at 15V            | ET-07a |
| ON @24 V and 30 V - all 16 ch    | Pass / Fail                            | All read 0                   | ET-07b |
| I_IN @24 V - all 16 ch           | ___ mA (range)                         | 9.0-14.0 mA                  | ET-08  |
| Walking-1 - all 16 channels      | Pass / Fail                            | Correct bit per ch           | ET-09a |
| All-ON: bytes 0,1 = 0x00         | 0x__ / 0x__                            | 0x00 / 0x00                  | ET-09b |
| INT pin transitions on change    | Pass / Fail                            | V_INT ≤ 0.8 V on event       | ET-09c |
| I²C data correct via J6, J8, J4  | Pass / Fail                            | All 3 connectors pass        | ET-10  |
| D18 CIE (x,y)                    | (___,___)                              | x:0.050-0.185; y:0.640-0.780 | LT-01  |
| D18 dominant wavelength          | ___ nm                                 | 520-535 nm                   | LT-01  |
| D1-D16 CIE (x,y) all channels    | (range)                                | x:0.400-0.540; y:0.400-0.560 | LT-02  |
| D1-D16 dominant wavelength       | ___ nm                                 | 580-600 nm                   | LT-02  |
| D18 luminous intensity @I_F=10mA | ___ mcd                                | ≥ 2 mcd                      | LT-03a |
| D1/D4/D8/D12/D16 intensity       | ___ mcd                                | ≥ 2 mcd each                 | LT-03b |
| D18 forward voltage @I_F=10mA    | ___ V                                  | 1.8-3.5 V                    | LT-03c |
| D1/D4/D8/D12/D16 V_F             | ___ V range                            | 1.8-3.5 V; Δ V_F ≤ 0.3 V     | LT-03d |

## Remarks / NCR References:

## Final Disposition:

Role Print Name Signature Date

Test Technician

Quality / Reviewer

Overall Result

- [ ] ☐ PASS (circle one)

- [ ] ☐ FAIL / HOLD