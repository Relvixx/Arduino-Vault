# 📚 Concept Index

> One-page jump map. Find any topic instantly.

---

## Hardware

| Topic | Note |
|---|---|
| What Arduino is | [[What is Arduino]] |
| Microcontroller vs CPU | [[Microcontroller vs Computer]] |
| Flash / SRAM / EEPROM | [[Memory Types]] |
| 16 MHz clock | [[The Clock]] |
| Uno vs Nano | [[Uno vs Nano]] |
| Pin map | [[Arduino Uno Anatomy]] |
| Digital pins | [[Digital Pins]] |
| Analog pins (A0–A5) | [[Analog Pins]] |
| PWM pins (~) | [[PWM Pins]] |
| Power pins (5V, GND, VIN) | [[Power Pins]] |
| Breadboard wiring rules | [[Breadboard Internals]] |
| V = IR | [[Ohms Law]] |
| Series vs parallel | [[Series vs Parallel]] |
| Voltage divider | [[Voltage Divider]] |

---

## Components

| Component | Note |
|---|---|
| LED | [[LED]] |
| Resistor + color code | [[Resistor]] |
| Push button | [[Push Button]] |
| Potentiometer | [[Potentiometer]] |
| Active / passive buzzer | [[Buzzer]] |
| LDR | [[LDR]] |
| TMP36 temperature sensor | [[TMP36]] |
| Servo motor | [[Servo Motor]] |
| DC motor | [[DC Motor]] |

---

## Signals

| Topic | Note |
|---|---|
| Digital vs analog signals | [[Digital vs Analog Signals]] |
| PWM explained | [[PWM Deep Dive]] |
| Serial / UART | [[UART]] |
| I2C protocol | [[I2C]] |
| SPI protocol | [[SPI]] |
| Hardware interrupts | [[Interrupts]] |

---

## Coding Patterns

| Pattern | Note |
|---|---|
| setup() and loop() | [[Arduino Code Structure]] |
| Data types and sizes | [[Data Types]] |
| Scope, static variables | [[Variable Scope and Static]] |
| Arrays and bounds | [[Arrays]] |
| Writing functions | [[Functions]] |
| Non-blocking timing | [[millis Non-Blocking Pattern]] |
| Edge detection | [[Edge Detection]] |
| State machines + enum | [[State Machines]] |
| Moving average filter | [[Moving Average Filter]] |
| Hysteresis bands | [[Hysteresis]] |
| map() + constrain() | [[map and constrain]] |
| Modular loop() design | [[Modular Code Architecture]] |

---

## Debugging

| Bug type | Note |
|---|---|
| Total circuit failure | [[Missing GND Bug]] |
| Random button reads | [[Floating Pin Bug]] |
| LED burns out | [[Wrong Resistor Calculation]] |
| Upload fails | [[Pins 0 and 1 Upload Conflict]] |
| Timing breaks at 32s | [[millis Type Bug]] |
| Wrong startup readings | [[Buffer Initialization Bug]] |
| First beep silent | [[Buzzer Timing Bug]] |
| Wrong state priority | [[State Transition Priority Bug]] |
| Crash after hours | [[String Object Heap Fragmentation]] |
| LED brightness wrong | [[map Float Argument Bug]] |
| General hardware debug | [[Hardware Debugging Process]] |
| General code debug | [[Software Debugging Process]] |
