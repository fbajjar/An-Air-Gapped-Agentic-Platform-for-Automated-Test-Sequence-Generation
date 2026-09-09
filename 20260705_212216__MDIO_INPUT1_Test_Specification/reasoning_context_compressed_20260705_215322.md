## Document Identity
- Document Name: UNKNOWN
- Document Date: 1/2/14

# Industrial Test Specification Summary: MDIO-INPUT1

## 1) Document Identity
*   **Document Name:** MDIO-INPUT1 - 16-Channel 24V Digital Input Module
*   **Document No.:** TS-MDIO-INPUT1-001
*   **Revision:** A
*   **Issue Date:** 2026-06-30
*   **Classification:** Confidential - Internal Use Only
*   **Author:** Hardware & Test Engineering
*   **Status:** RELEASED
*   **Target Max Length:** 16000 characters (Constraint met)

## 2) Canonical Entity Map
*   **Device Under Test (DUT):** MDIO-INPUT1 PCB assembly.
    *   **Type:** Single-sided SMD/THT mixed-technology.
    *   **Function:** 16-channel, 24 V DC digital input module.
    *   **Isolation:** Galvanic isolation via quad optocouplers.
    *   **Interface:** I²C bus (PCF8575 16bit port expander).
    *   **Input Voltage Range:** 24 V DC (Nominal), Max 30 V DC.
    *   **Standards:** Class 2 per IPC-A-610; IEC 61131-2 Type 1.
*   **Components:**
    *   **U1:** PCF8575DBR (SSOP-24), 16-bit I²C I/O expander. Base address 0x20-0x27.
    *   **U2, U3:** LTV-844 (DIP-16), Quad phototransistor optocoupler (Channels 1-8).
    *   **U4, U5:** LTV-844 (DIP-16), Quad phototransistor optocoupler (Channels 9-16).
    *   **D1-D16:** LED (0805), Yellow/Amber (λd 585-595 nm), Channel status.
    *   **D17:** 1N5817 (DO-41), Schottky diode, Reverse-polarity protection.
    *   **D18:** LED (0805), Green (λd 520-535 nm), Power indicator.
    *   **R1-R16:** 2 kΩ (SMD 0805), Series current-limiting resistors.
    *   **R17-R32:** 330 Ω (SMD 0805), Collector pull-up / LED current-setting.
    *   **R33:** 330 Ω (SMD 0805), Power indicator LED current-setting.
    *   **C1:** 100 nF (SMD 0805), VDD bypass.
    *   **C2:** 10 µF (SMD 0805), VDD bulk decoupling.
    *   **J1:** 8-way terminal block (PT-1.5 3.5 mm), 24 V field input (CH1-8).
    *   **J10:** 9-way terminal block (PT-1.5 3.5 mm), 24 V field input (CH9-16) + COM.
    *   **J9:** 2-way terminal block (PT-1.5 3.5 mm), 5 V DC power input.
    *   **J6:** 4-pin header (2.54 mm), I²C Option A (Pin header).
    *   **J8:** 4-pin socket (2.54 mm), I²C Option B (Pin socket).
    *   **J4:** 2×4 IDC header (2.54 mm), I²C Option C (Flat ribbon).
    *   **J3, J5, J7:** 3-pin jumper header, Address configuration (ADD0/ADD1/ADD2).
*   **Test Equipment (TE):**
    *   **TE-01:** Bench DC Power Supply (0-30 V, 0-3 A, ±0.1 V).
    *   **TE-02:** Digital Multimeter (≥ 4½ digits, ±0.1% V, ±0.5% I).
    *   **TE-03:** Dielectric Withstand Tester (AC 0-3000 Vrms, ≤ 0.1 mA leakage).
    *   **TE-04:** Insulation Resistance Tester (500/1000 V DC, ≥ 10 GΩ).
    *   **TE-05:** I²C Protocol Analyser (100/400 kHz, 0x20-0x27).
    *   **TE-06:** 5 V DC Regulated Supply (±50 mV, ≥ 0.5 A).
    *   **TE-07:** Tristimulus Colorimeter/Spectroradiometer (CIE 1931, Δ(x,y) ≤ 0.002).
    *   **TE-08:** Luminance/Luminous Intensity Meter (0.1 mcd - 1000 mcd, ±10%).
    *   **TE-09:** Precision Resistance Box (10 Ω - 100 kΩ, ±0.1%).
    *   **TE-10:** Anti-Static Workstation & Wrist Strap (10⁶ - 10⁹ Ω).
    *   **TE-11:** Oscilloscope (≥ 10 MHz, 2 channels) - Optional.

## 3) Pin/Signal Mapping
*   **I²C Address Map:**
    *   **Format:** 0b010 0 A2 A1 A0 (Base 0x20).
    *   **Jumpers:** ADD2 (J7), ADD1 (J5), ADD0 (J3).
    *   **Combinations:**
        *   GND/GND/GND → 0x20 (Factory Default)
        *   GND/GND/VDD → 0x21
        *   GND/VDD/GND → 0x22
        *   GND/VDD/VDD → 0x23
        *   VDD/GND/GND → 0x24
        *   VDD/GND/VDD → 0x25
        *   VDD/VDD/GND → 0x26
        *   VDD/VDD/VDD → 0x27 (Max)
*   **Signal Path (Per Channel):**
    *   **Input:** 24 V field signal via J1 (CH1-8) or J10 (CH9-16).
    *   **Limiting:** 2 kΩ Resistor (R1-R16).
    *   **Isolation:** LTV-844 Optocoupler LED.
    *   **Logic Side:** 330 Ω Pull-up (R17-R32) to VDD via Status LED (D1-D16).
    *   **Output:** PCF8575 Port Pin.
        *   **CH1-8:** P00-P07.
        *   **CH9-16:** P10-P17.
        *   **Logic State:** Active = Logic 0 (LOW); Inactive = Logic 1 (HIGH).
*   **Connector Pinouts:**
    *   **J6 (Header):** Pin 1=VDD, Pin 2=SDA, Pin 3=SCL, Pin 4=GND.
    *   **J8 (Socket):** Pin 1=VDD, Pin 2=SDA, Pin 3=SCL, Pin 4=GND.
    *   **J4 (IDC):** Pin 1=VDD, Pin 2=GND, Pin 3=SCL, Pin 4=GND, Pin 5=SDA, Pin 6=GND, Pin 7=GND, Pin 8=NC.
    *   **J9 (Power):** Pin 1=GND, Pin 2=VDD+.
    *   **J10 (COM):** Pin 9 = Field Ground Return.

## 4) Timing Diagram Truth Table
*   **I²C Speed:** 400 kHz Fast Mode (ET-05, ET-10).
*   **Voltage Levels (400 kHz):**
    *   **V_OH (High):** ≥ 0.7 × VDD = 3.5 V.
    *   **V_OL (Low):** ≤ 0.3 × VDD = 1.5 V.
    *   **Rise Time (t_r):** ≤ 300 ns (with 1 kΩ pull-up).
*   **LED Forward Current (I_F):**
    *   **Chromaticity Test:** 10 mA ± 0.5 mA DC.
    *   **Operating Current (Calculated):** ≈ 11.4 mA at 24 V (24V - 1.2V / 2000Ω).
    *   **Acceptance Range:** 9.0 mA ≤ I_IN ≤ 14.0 mA.
*   **Hi-Pot Test:**
    *   **Voltage:** 1500 Vrms AC.
    *   **Duration:** 1 second.
    *   **Ramp:** 0 → 1500 Vrms over 5 seconds.
    *   **Leakage Limit:** < 10 mA.
*   **INT Pin Transition:**
    *   **Trigger:** Any channel state change.
    *   **Time:** ≤ 50 µs.
    *   **Voltage:** ≤ 0.8 V (Open-drain with external pull-up).

## 5) Appendix Definitions and Mappings
*   **IEC 61131-2 Type 1 (Digital Input):**
    *   **V_OFF(max):** 5 V DC.
    *   **V_ON(min):** 15 V DC.
    *   **V_nom:** 24 V DC.
    *   **V_max:** 30 V DC.
    *   **I_IN(OFF):** ≤ 15 mA.
    *   **I_IN(ON):** 2-15 mA (at 24 V).
*   **IEC 60073 LED Colors:**
    *   **Green:** Normal / Safe / ON (Power Present).
    *   **Yellow/Amber:** Caution / Active state (Channel ON).
*   **CIE 1931 Acceptance Polygons:**
    *   **D18 (Green):**
        *   x: 0.050 - 0.185
        *   y: 0.640 - 0.780
        *   λ_d: 520 - 535 nm
        *   Purity: ≥ 90%
    *   **D1-D16 (Yellow/Amber):**
        *   x: 0.400 - 0.540
        *   y: 0.400 - 0.560
        *   λ_d: 580 - 600 nm
        *   Purity: ≥ 85%
*   **Luminous Intensity:**
    *   **Min:** ≥ 2 mcd at I_F = 10 mA.
    *   **V_F Range:** 1.8 V - 3.5 V.
    *   **Channel Variation:** ≤ 0.3 V.
*   **Environmental Conditions:**
    *   **Temp:** 20 °C ± 5 °C.
    *   **Humidity:** 45% - 75 %RH (non-condensing).
    *   **Pressure:** 860 hPa - 1060 hPa.
    *   **Warm-up:** ≥ 2 minutes at 5 V.
*   **Visual Inspection (IPC-A-610J Class 2):**
    *   **Solder Joints:** ≥ 75% fillet, no voids > 25%, no bridging.
    *   **Through-hole:** 100% hole fill, lead protrusion 0.5-2.5 mm.
    *   **Defect:** Immediate rejection.
    *   **Process Indicator:** Log and review within 24 hours.

## 6) Numeric Requirements
*   **Power Supply:**
    *   **VDD Nominal:** 5.0 V.
    *   **VDD Acceptance (No Load):** 4.75 V - 5.25 V.
    *   **VDD Acceptance (Full Load):** ≥ 4.65 V (≤ 350 mV drop).
    *   **Reverse Polarity VDD:** < 0.5 V.
    *   **Idle Current:** 5 mA - 80 mA.
    *   **Reverse Polarity Current:** < 2 mA.
*   **Optocoupler Isolation:**
    *   **Rated:** 5000 Vrms (LTV-844).
    *   **Test Voltage:** 1500 Vrms (30% of rated).
    *   **Leakage Current:** < 10 mA.
    *   **Insulation Resistance:** ≥ 100 M Ω @ 500 V DC.
*   **Digital Input Thresholds:**
    *   **OFF State (≤ 5 V):** Logic 1 (Active High).
    *   **ON State (≥ 15 V):** Logic 0 (Active Low).
    *   **Max Input Current (ON):** ≤ 15 mA.
    *   **Input Current (24 V):** 9.0 mA - 14.0 mA.
*   **I²C Bus:**
    *   **Address Scan:** 0x20 - 0x27.
    *   **Signal Integrity:** V_OH ≥ 3.5 V, V_OL ≤ 1.5 V, t_r ≤ 300 ns.
*   **LED Performance:**
    *   **Chromaticity:** Within defined polygons.
    *   **Intensity:** ≥ 2 mcd @ 10 mA.
    *   **Forward Voltage:** 1.8 V - 3.5 V.
    *   **Batch Consistency:** Δ x ≤ 0.060, Δ y ≤ 0.060.
*   **Safety Limits:**
    *   **Current Limit (24 V):** ≤ 200 mA.
    *   **Hi-Pot Safety:** Hands clear, shrouded probes.

## 7) Unresolved/Unknown Facts
*   **Document Name:** UNKNOWN (Source text only provided "MDIO-INPUT1 - 16-Channel 24V Digital Input Module" in header, but formal document name field in table is empty/implicit).
*   **Specific Component Lot Numbers:** Not specified in source.
*   **Exact Calibration Cert Numbers:** Placeholder in Test Record Form (TE-01 to TE-09).
*   **Specific Ambient Conditions for Specific Tests:** General conditions apply unless stated otherwise; no unique deviations noted.
*   **Test Record Form Signatures:** Placeholder for Technician and Reviewer names.
*   **NCR Numbering Scheme:** Not defined in source.
*   **Quarantine Location:** Not specified (only "quarantine location" mentioned).
*   **Record Retention Period:** 10 years (per IEC 61010-1 recommendation), but specific internal policy not explicitly stated as overriding.
*   **Visual Inspection Magnification:** ×3 to ×10 (specified).
*   **Ambient Light for Photometric Tests:** < 1 lux (specified).
*   **Probe-to-LED Distance:** 5 mm ± 1 mm (specified).
*   **Series Resistor Value for LED Test:** 300 Ω (specified for 10 mA @ 5 V).
*   **PCF8575 Internal Pull-up Strength:** Not explicitly quantified, only behavior (HIGH when floating) described.
*   **INT Pin External Pull-up Value:** Not specified, only behavior (open-drain) described.
*   **Specific I²C Master Model:** Not specified, only capabilities (100/400 kHz) required.
*   **Specific Hi-Pot Tester Model:** Not specified, only specs (AC 0-3000 Vrms) required.
*   **Specific DMM Model:** Not specified, only specs (4½ digits) required.
*   **Specific Colorimeter Model:** Not specified, only specs (CIE 1931, Δ ≤ 0.002) required.
*   **Specific Luminance Meter Model:** Not specified, only specs (0.1-1000 mcd) required.
*   **Specific Resistance Box Model:** Not specified, only specs (10Ω-100kΩ) required.
*   **Specific Oscilloscope Model:** Not specified (optional), only specs (≥10 MHz) required.
*   **Specific Power Supply Model:** Not specified, only specs (0-30V, 0-3A) required.
*   **Specific 5V Supply Model:** Not specified, only specs (±50mV, ≥0.5A) required.
*   **Specific Anti-Static Equipment Model:** Not specified, only specs (10⁶-10⁹ Ω) required.
*   **Specific PCB Part Number:** "MDIO-INPUT1" is the module name; specific PCB part number is not explicitly distinct in source text (assumed same as module name or not provided).
*   **Specific Serial Number Format:** "min 6 characters alphanumeric" (specified).
*   **Specific Laser Etching/Labeling Standard