# PIC16F887 Parking Lot Control System

## Introduction
This project implements a **simple automatic parking lot system** using the **PIC16F887** microcontroller.  
It counts cars entering/exiting using **IR sensors**, controls a **servo barrier**, displays status on **LCD 16x2**, and alerts when the lot is **full**.

- **Purpose**: Learn interrupt handling, PWM servo control, EEPROM persistence, and LCD interfacing on PIC16F887.
- **Programming Language**: C (CCS PIC C Compiler)
- **Tools**: MPLAB X IDE + XC8 (or CCS C), Proteus (simulation), USB PIC Programmer.
- **Hardware**: PIC16F887, LCD 16x2, Servo SG90, 2x IR sensors, Buzzer, LED, Reset button.

---

## Features
| Feature | Description |
|-------|-----------|
| **Car Entry/Exit Detection** | 2 IR sensors (IR_IN, IR_OUT) on RB0, RB1 |
| **Servo Barrier Control** | Opens to 90° when car passes, then closes |
| **LCD Display** | Shows current car count and status (`CON CHO` / `BAI DAY`) |
| **Full Lot Alert** | Buzzer + LED when 10 cars reached |
| **Reset Button** | RB2 – resets count to 0 |
| **Data Persistence** | Saves car count in EEPROM (retains after power off) |
| **Debounced Interrupts** | 50ms delay to avoid noise |

---

## Hardware Connections

| Component | Pin |
|---------|-----|
| LCD RS | RD0 |
| LCD EN | RD1 |
| LCD D4–D7 | RD4–RD7 |
| IR_IN (Entry) | RB0 |
| IR_OUT (Exit) | RB1 |
| Reset Button | RB2 |
| Servo Signal | RC1 (CCP1/PWM) |
| Buzzer | RC0 |
| LED Full | RC1 |
| +5V, GND | Standard |

> **Note**: Use `lcd.c` driver provided by CCS Compiler.

---

## Software Requirements
- **Compiler**: CCS PIC C Compiler (or XC8 with `lcd.c` adapted)
- **IDE**: MPLAB X (optional), or CCS IDE
- **Simulator**: Proteus 8
- **Programmer**: PICkit 3/4, USBurn, etc.

---

## How It Works

1. **Initialization**:
   - Configure PORTB as input (RB0–RB2), PORTC/D as output
   - Initialize LCD, PWM (20ms period for servo), enable RB interrupts
   - Read saved car count from EEPROM

2. **Main Loop**:
   - Continuously update LCD: `So xe: X`
   - If full → show `BAI DAY`, turn on buzzer + LED
   - Else → show `CON CHO`

3. **Interrupt (RB Change)**:
   - Triggered when IR sensor or reset button is pressed
   - **Entry**: `IR_IN == 0` → increment count, open servo
   - **Exit**: `IR_OUT == 0` → decrement count, open servo
   - **Reset**: `RESET_BUTTON == 0` → set count = 0
   - Save count to EEPROM after every change

4. **Servo Control**:
   - PWM frequency ~50Hz (20ms period)
   - Duty cycle:  
     - `0°` → 1ms (1000)  
     - `90°` → 2ms (2000)

---

## Build & Flash
1. Open in CCS C or MPLAB X + XC8
2. Include lcd.c and 16F887.h
3. Set fuses: HS, clock = 20 MHz
4. Compile → generate .hex
5. Flash using programmer

--- 

## Simulation (Proteus)

1. Place PIC16F887, LCD, Servo, IR sensors (use switches), Buzzer
2. Connect as per pinout
3. Load .hex file
4. Press switches to simulate car entry/exit

<div align="center">
  <p><strong>© 2025 – Embedded Systems Mini Project</strong></p>
  <p><i>Nguyễn Phạm Huy Hoàng</i> 
  <p><em>Simple. Reliable. Smart Parking.</em></p>
</div>