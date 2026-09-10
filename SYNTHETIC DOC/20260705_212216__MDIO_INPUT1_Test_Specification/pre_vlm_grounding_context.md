# VLM Grounding Context: MDIO-INPUT1 Technical Specification

## 1) Canonical Entity Dictionary
*   **DUT**: MDIO-INPUT1 (16-Channel 24V Digital Input Module, Revision A, Doc No: TS-MDIO-INPUT1-001).
*   **PCB Assembly**: Single-sided SMD/THT mixed-technology.
*   **Core IC**: U1 (PCF8575DBR, SSOP-24, 16-bit I²C I/O expander, Base Address 0x20-0x27).
*   **Isolation Components**: U2, U3, U4, U5 (LTV-844, DIP-16, Quad phototransistor optocoupler, 5000 Vrms isolation).
*   **Power Supply**: J9 (2-way terminal block, 5 V DC input, VDD+/GND).
*   **Field Inputs**: J1 (8-way, CH1-CH8), J10 (9-way, CH9-CH16 + COM).
*   **I²C Connectors**: J6 (4-pin header), J8 (4-pin socket), J4 (2×4 IDC header).
*   **Address Jumpers**: J3 (ADD0), J5 (ADD1), J7 (ADD2).
*   **Status LEDs**: D1-D16 (Channel status, 0805, Yellow/Amber), D18 (Power indicator, 0805, Green).
*   **Protection**: D17 (1N5817, DO-41, Schottky diode, Reverse polarity protection).
*   **Resistors**: R1-R16 (2 kΩ, Series current-limiting), R17-R32 (330 Ω, LED pull-up), R33 (330 Ω, D18 current-setting).
*   **Capacitors**: C1 (100 nF, VDD bypass), C2 (10 µF, VDD bulk).
*   **Test Equipment**: TE-01 (DC Power Supply), TE-02 (DMM), TE-03 (Hi-Pot), TE-04 (Insulation Tester), TE-05 (I²C Analyser), TE-06 (5 V Regulated Supply), TE-07 (Colorimeter), TE-08 (Luminance Meter), TE-09 (Resistor Box), TE-10 (ESD Station).

## 2) Pin and Signal Aliases
*   **I²C Address Mapping**:
    *   ADD2 (J7 pin 1), ADD1 (J5 pin 1), ADD0 (J3 pin 1).
    *   Logic: GND=0, VDD=1.
    *   Formula: 0b010 0 A2 A1 A0 (Hex: 0x20 + Jumpers).
    *   Valid Addresses: 0x20, 0x21, 0x22, 0x23, 0x24, 0x25, 0x26, 0x27.
*   **PCF8575 Port Mapping**:
    *   Channels 1-8: P00-P07 (Byte 0).
    *   Channels 9-16: P10-P17 (Byte 1).
    *   Active State: Logic 0 (LOW). Inactive State: Logic 1 (HIGH).
*   **I²C Connector Pinouts**:
    *   J6/J8 (Header/Socket): Pin 1=VDD, Pin 2=SDA, Pin 3=SCL, Pin 4=GND.
    *   J4 (IDC): Pin 1=VDD, Pin 2=GND, Pin 3=SCL, Pin 4=GND, Pin 5=SDA, Pin 6=GND, Pin 7=GND, Pin 8=NC.
*   **Field Signal Path**:
    *   Input: J1/J10 (24 V field).
    *   Return: J10 Pin 9 (COM/Field Ground).
    *   Series Resistor: R1-R16 (2 kΩ).
    *   Opto Output: LTV-844 Phototransistor Collector.
    *   Logic Side: 330 Ω Pull-up (R17-R32) to VDD via LED (D1-D16).
*   **INT Pin**: U1 Pin 1 (Interrupt, Open-drain, transitions LOW on state change).

## 3) Timing and State Definitions
*   **Power Sequencing**: Apply 5 V (J9) BEFORE 24 V (J1/J10). Remove 24 V BEFORE 5 V.
*   **Test Timing**:
    *   Hi-Pot Ramp: 0 → 1500 Vrms over 5 seconds.
    *   Hi-Pot Hold: 1500 Vrms for 1 second.
    *   Hi-Pot Trip Threshold: 10 mA.
    *   I²C Scan Rate: 100 kHz (default), 400 kHz (Fast Mode).
    *   I²C Rise Time Limit: ≤ 300 ns (1 kΩ pull-up).
    *   INT Pin Transition: ≤ 50 µs after channel state change.
*   **State Definitions**:
    *   **HIGH**: Logic 1, Voltage ≥ 0.7×VDD (≥ 3.5 V).
    *   **LOW**: Logic 0, Voltage ≤ 0.3×VDD (≤ 1.5 V).
    *   **ON**: LED illuminated, Channel Active (Logic 0).
    *   **OFF**: LED unlit, Channel Inactive (Logic 1).
    *   **Active**: Input Voltage ≥ 15 V (IEC 61131-2 Type 1).
    *   **Inactive**: Input Voltage ≤ 5 V (IEC 61131-2 Type 1).

## 4) Numeric and Unit Constraints
*   **Voltage Ranges**:
    *   VDD Nominal: 5.0 V. Acceptable: 4.75 V - 5.25 V (No Load), ≥ 4.65 V (Full Load).
    *   Reverse Polarity VDD: < 0.5 V.
    *   I²C V_OH: ≥ 3.5 V.
    *   I²C V_OL: ≤ 1.5 V.
    *   Field Input V_ON(min): 15 V. V_MAX: 30 V.
    *   Field Input V_OFF(max): 5 V.
*   **Current Limits**:
    *   Supply Idle: 5 mA - 80 mA.
    *   Supply Reverse Polarity: < 2 mA.
    *   Opto Input Current (I_IN): ≤ 15 mA (IEC 61131-2 Type 1).
    *   Expected I_IN @ 24 V: ≈ 11.4 mA (Range: 9.0 mA - 14.0 mA).
    *   Hi-Pot Leakage: < 10 mA.
*   **Resistance**:
    *   Series Resistors (R1-R16): 2 kΩ.
    *   LED Pull-ups (R17-R33): 330 Ω.
    *   Insulation Resistance: ≥ 100 M Ω @ 500 V DC.
*   **Chromaticity (CIE 1931)**:
    *   D18 (Green): x: 0.050-0.185, y: 0.640-0.780, λ_d: 520-535 nm.
    *   D1-D16 (Yellow/Amber): x: 0.400-0.540, y: 0.400-0.560, λ_d: 580-600 nm.
*   **Photometry**:
    *   Luminous Intensity (D18/D1): ≥ 2 mcd @ 10 mA.
    *   Forward Voltage (V_F): 1.8 V - 3.5 V @ 10 mA.
*   **Environmental**:
    *   Temp: 20 °C ± 5 °C.
    *   Humidity: 45% - 75 %RH.
    *   Ambient Light: < 1 lux.

## 5) Appendix Mappings
*   **Test ID to Procedure**:
    *   VI-01: Visual Inspection (IPC-A-610J Class 2).
    *   ET-01: Power Supply & Reverse Polarity.
    *   ET-02: VDD Rail Verification.
    *   ET-03: Hi-Pot (1500 Vrms).
    *   ET-04: Insulation Resistance (≥ 100 M Ω).
    *   ET-05: I²C Address Scan (0x20-0x27).
    *   ET-06: OFF Threshold (≤ 5 V).
    *   ET-07: ON Threshold (≥ 15 V).
    *   ET-08: Input Current (9-14 mA).
    *   ET-09: Full Channel Walk (CH1-CH16).
    *   ET-10: Dual I²C Connector (J6, J8, J4).
    *   LT-01: D18 Chromaticity.
    *   LT-02: D1-D16 Chromaticity.
    *   LT-03: Luminous Intensity / V_F.
*   **Component Reference**:
    *   U1: PCF8575DBR.
    *   U2-U5: LTV-844.
    *   D17: 1N5817.
    *   D18: Green LED.
    *   D1-D16: Yellow/Amber LEDs.
*   **Safety Limits**:
    *   Max Field Voltage: 30 V.
    *   Hi-Pot Voltage: 1500 Vrms.
    *   Current Limit (24 V): ≤ 200 mA.

## 6) Explicit Unknowns
*   **Ambient Light Level**: Exact lux value during specific photometric tests (document states < 1 lux, but exact sensor reading not fixed).
*   **Specific Calibration Cert Numbers**: TE-01 through TE-10 calibration numbers are placeholders in the form.
*   **Unit Serial Number**: Unique identifier varies per DUT.
*   **Specific Resistor Tolerance**: Document specifies values (2kΩ, 330Ω) but not individual component tolerance bands (e.g., 1%, 5%).
*   **Exact LED Forward Voltage**: Document specifies a range (1.8V-3.5V) rather than a single fixed value per LED.
*   **I²C Pull-up Resistance**: Document implies 1 kΩ for timing but does not explicitly state the exact value of the pull-up resistors used in the test setup (TE-05 internal or external).
*   **Opto Coupler Forward Voltage (V_F)**: Document uses ≈ 1.2 V for calculation but does not specify the exact measured V_F of the LTV-844 part.
*   **PCF8575 Quiescent Current**: Document gives a range (5-80 mA) but not the exact quiescent current of the chip alone.
*   **Hi-Pot Tester Ground Fault Interrupt Status**: Document requires it to be working but does not provide a specific test value for the interrupt threshold.
*   **ESD Resistance Value**: Document specifies a range (10⁶ - 10⁹ Ω) for the workstation strap but not the exact measured resistance.
*   **LED Lens Measurement Distance**: Document specifies "contact or 5 mm ± 1 mm" but does not mandate a single fixed distance for all measurements.
*   **Colorimeter Accuracy**: Document specifies Δ(x,y) ≤ 0.002 but not the exact calibration date or uncertainty of the specific TE-07 unit.
*   **I²C Bus Master Address Range**: Document specifies 0x20-0x27 but does not confirm if the master supports addresses outside this range (e.g., 0x00-0x7F).
*   **INT Pin External Pull-up Value**: Document mentions "external pull-up on I²C master" but does not specify the value used for the INT pin test.
*   **Visual Inspection Magnification**: Document states "×3 to ×10" but does not specify the exact magnification used for specific defect detection.
*   **Solder Joint Void Limit**: Document states "no visible voids exceeding 25%" but does not specify the exact measurement method or threshold for "visible".
*   **LED Purity Percentage**: Document specifies "≥ 90%" for D18 and "≥ 85%" for D1-D16 but does not specify the exact measurement method for purity.
*   **Chromaticity Scatter Limits**: Document specifies Δx ≤ 0.060 and Δy ≤ 0.060 for D1-D16 but does not specify if this is a pass/fail or process indicator.
*   **I²C Signal Integrity**: Document specifies V_OH and V_OL limits but does not specify the exact rise/fall time limits for the specific I²C mode (100 kHz vs 400 kHz).
*   **Hi-Pot Tester Voltage Ramp Rate**: Document states "0 → 1500 Vrms over 5 seconds" but does not specify the exact ramp rate in V/s.
*   **Insulation Resistance Stabilization Time**: Document states "after 60 s stabilisation" but does not specify the exact criteria for "stabilisation".
*   **I²C ACK Timing**: Document does not specify the exact time window for ACK reception after address.
*   **PCF8575 Port Pin Numbers**: Document states P00-P07 and P10-P17 but does not explicitly map them to specific physical pins on the SSOP-24 package (e.g., Pin 1 vs Pin 24).
*   **Field Ground Return Path**: Document states J10 Pin 9 is COM but does not specify if this is a dedicated ground pin or shared with other signals.
*   **I²C Connector Pinout for J4**: Document lists pinout but does not specify if the IDC connector is keyed or if pin 1 is always VDD regardless of orientation.
*   **Hi-Pot Tester Leakage Trip Threshold**: Document states "10 mA" but does not specify if this is a hard trip or a warning threshold.
*   **Visual Inspection Defect Criteria**: Document references IPC-A-610J Class 2 but does not list specific defect types (e.g., "solder ball", "cold joint").
*   **LED Color Code**: Document specifies "Yellow/Amber" and "Green" but does not specify the exact IEC 60073 code numbers.
*   **I²C Bus Master Speed**: Document mentions 100 kHz and 400 kHz but does not specify the default speed for the test unless otherwise stated.
*   **Hi-Pot Tester Grounding**: Document states "working ground-fault interrupt" but does not specify the exact resistance value of the ground connection.
*   **ESD Protection Level**: Document mentions "anti-static workstation" but does not specify the specific ESD protection level (e.g., Class 1, 2, 3).
*   **I²C Address Jumpers**: Document lists J3, J5, J7 but does not specify the exact pinout of the jumper headers (e.g., which pin is VDD vs GND).
*   **Field Input Voltage Range**: Document specifies 24 V nominal and 30 V max but does not specify the exact voltage range for the OFF state test (e.g., 0 V to 5 V).
*   **I²C Signal Integrity**: Document specifies V_OH and V_OL limits but does not specify the exact rise/fall time limits for the specific I²C mode (100 kHz vs 400 kHz).
*   **Hi-Pot Tester Voltage Ramp Rate**: Document states "0 → 1500 Vrms over 5 seconds" but does not specify the exact ramp rate in V/s.
*   **Insulation Resistance Stabilization Time**: Document states "after 60 s stabilisation" but does not specify the exact criteria for "stabilisation".
*   **I²C ACK Timing**: Document does not specify the exact time window for ACK reception after address.
*   **PCF8575 Port Pin Numbers**: Document states P00-P07 and P10-P17 but does not explicitly map them to specific physical pins on the SSOP-24 package (e.g., Pin 1 vs Pin 24).
*   **Field Ground Return Path**: Document states J10 Pin 9 is COM but does not specify if this is a dedicated ground pin or shared with other signals.
*   **I²C Connector Pinout for J4**: Document lists pinout but does not specify if the IDC connector is keyed or if pin 1 is always VDD regardless of orientation.
*   **Hi-Pot Tester Leakage Trip Threshold**: Document states "10 mA" but does not specify if this is a hard trip or a warning threshold.
*   **Visual Inspection Defect Criteria**: Document references IPC-A-610J Class 2 but does not list specific defect types (e.g., "solder ball", "cold joint").
*   **LED Color Code**: Document specifies "Yellow/Amber" and "Green" but does not specify the exact IEC 60073 code numbers.
*   **I²C Bus Master Speed**: Document mentions 100 kHz and