# 🏠 Arduino Knowledge Vault

> Built from a complete mentor-led Arduino course.
> Beginner → Independent Intermediate/Advanced.
> This vault is a living engineering reference — not a transcript.

---

## What This Vault Is

This vault was synthesized from a structured Arduino course covering
hardware mastery and programming mastery across 13 modules and 12
Tinkercad projects. Everything here has been reasoned through,
corrected where wrong, and organized for long-term use.

---

## 🗺 Navigation Paths

### 📖 Study Path (Linear)
Start here if reviewing from scratch.

1. [[What is Arduino]]
2. [[Microcontroller vs Computer]]
3. [[Memory Types]]
4. [[How Code Runs]]
5. [[Arduino Uno Anatomy]]
6. [[Digital Pins]] → [[Analog Pins]] → [[PWM Pins]]
7. [[Ohms Law]] → [[Breadboard Internals]]
8. [[LED]] → [[Resistor]] → [[Push Button]] → [[Potentiometer]]
9. [[PWM Deep Dive]] → [[UART]] → [[I2C]] → [[Interrupts]]
10. [[Arduino Code Structure]] → [[Data Types]] → [[Variable Scope and Static]]
11. [[millis Non-Blocking Pattern]] → [[Edge Detection]] → [[State Machines]]
12. [[Moving Average Filter]] → [[Hysteresis]]
13. [[Modular Code Architecture]]
14. [[P12 Smart Environment Monitor]]

---

### 🔧 Debugging Path
Start here when something is broken.

1. [[Hardware Debugging Process]] — always start here
2. [[Missing GND Bug]] — most common total failure
3. [[Floating Pin Bug]] — random button behavior
4. [[Wrong Resistor Calculation]] — LED burns out
5. [[Pins 0 and 1 Upload Conflict]] — upload fails
6. [[millis Type Bug]] — timing breaks after ~32 seconds
7. [[Buffer Initialization Bug]] — bad sensor readings at startup
8. [[Buzzer Timing Bug]] — first beep silent
9. [[String Object Heap Fragmentation]] — random crash after hours
10. [[Software Debugging Process]] — general code debugging

---

### 🏗 Project Path
Start here to review or rebuild any project.

| Project | Key Concept |
|---|---|
| [[P01 Basic LED Blink]] | pinMode, digitalWrite, delay |
| [[P02 Challenge Blink]] | for loop, timing |
| [[P03 Button Controlled LED]] | digitalRead, INPUT_PULLUP |
| [[P04 Toggle LED]] | Edge detection, state variable |
| [[P05 Potentiometer LED Brightness]] | analogRead, map, analogWrite |
| [[P06 Pot Controlled Blink Speed]] | Conditional blink timing |
| [[P07 Thermometer with Buzzer]] | TMP36, tone(), threshold logic |
| [[P08 Non-Blocking Blink and Button]] | millis(), simultaneous tasks |
| [[P09 Traffic Light]] | State machine, enum, setLight() |
| [[P10 Pedestrian Crossing]] | Dual millis() timers, WARN blink |
| [[P11 TMP36 Servo LED]] | Filter, hysteresis, servo mapping |
| [[P12 Smart Environment Monitor]] | Full FSM capstone |

---

### 📋 Revision Path
Quick review before an exam or build session.

- [[Concept Index]] — jump to any topic directly
- [[Best Practices]] — engineering habits checklist
- [[Mistakes and Corrections Log]] — what went wrong and why
- [[Glossary]] — term definitions
- [[Circuit Design Checklist]] — 5 questions before any build
- [[Arduino Functions Reference]] — all functions in one place

---

## 📌 What to Revisit Next

After completing this course, the next logical areas are:

- [ ] I2C peripherals — OLED display, RTC clock
- [ ] Interrupt-driven button handling (replace polling)
- [ ] EEPROM persistence — saving calibration data
- [ ] L298N H-bridge — DC motor direction control
- [ ] PID control basics
- [ ] ESP8266/ESP32 — WiFi connectivity
- [ ] See [[What to Study Next]] for the full roadmap

---

## Vault Stats
- Modules covered: 13
- Projects built: 12
- Debugging patterns extracted: 13
- Phase 1 (Hardware): Modules 1–6
- Phase 2 (Programming): Modules 7–13
