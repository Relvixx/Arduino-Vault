<div align="center">

<h1><b>⚡ Arduino Vault</b></h1>

<p><em>A structured, mentor-reviewed engineering knowledge base — from bare hardware to a design-reviewed embedded capstone</em></p>

[![Made with Obsidian](https://img.shields.io/badge/Made%20with-Obsidian-7C3AED?style=for-the-badge&logo=obsidian&logoColor=white)](https://obsidian.md)
[![Arduino](https://img.shields.io/badge/Platform-Arduino-00979D?style=for-the-badge&logo=arduino&logoColor=white)](https://arduino.cc)
[![License: MIT](https://img.shields.io/badge/License-MIT-22c55e?style=for-the-badge)](./LICENSE)
[![Projects](https://img.shields.io/badge/Projects-12%20Built-f59e0b?style=for-the-badge)](#development-phases)
[![Modules](https://img.shields.io/badge/Modules-13%20Covered-3b82f6?style=for-the-badge)](#overview)

</div>

<br>
<hr>

## Table of Contents

1. [Overview](#overview)
2. [Architecture](#architecture)
3. [Development Phases](#development-phases)
4. [Capstone Highlight](#capstone-highlight)
5. [Navigation Guide](#navigation-guide)
6. [Engineering Notes](#engineering-notes)
7. [Roadmap](#roadmap)
8. [Contributing](#contributing)
9. [License](#license)

---

## Overview

Arduino Vault is a battle-tested engineering knowledge base synthesized from a structured, mentor-led Arduino course spanning 13 modules and 12 Tinkercad projects. Every note was reasoned through, corrected where wrong, and cross-linked for long-term retrieval — not transcribed. The vault covers the full stack from Ohm's Law and breadboard topology to multi-state FSMs, non-blocking concurrency, and sensor signal processing, culminating in a formal design-reviewed capstone embedded system.

### Key Features

- [x] Four structured navigation paths: Study, Debugging, Project, and Revision
- [x] 13 debugging patterns with root cause and fix — extracted from real bugs
- [x] 12 progressively complex Tinkercad projects with full wiring and annotated code
- [x] Capstone FSM (P12) with a formal design document and self-review
- [x] Moving average filter + hysteresis pattern with circular buffer implementation
- [x] Non-blocking timing (`millis()`) patterns contrasted against `delay()` anti-patterns
- [x] 15 non-negotiable engineering best practices with wrong/correct code comparisons
- [x] Obsidian-native with bidirectional links, concept index, and graph navigation

*Built with: Arduino C++, Obsidian, Markdown, Tinkercad.*

---

## Architecture

<details>
<summary>📁 Repository structure</summary>

```text
Arduino Vault/
├── 00 Home/
│   ├── 🏠 Home.md                  # Master navigation hub with four study paths
│   └── 📚 Concept Index.md         # One-page jump map across all topics
│
├── 01 Fundamentals/                # What Arduino is, memory types, clock, Uno vs Nano
├── 02 Board and Circuits/          # Pin maps, Ohm's Law, breadboard, voltage divider
├── 03 Components/                  # LED, resistor, button, pot, buzzer, LDR, TMP36, servo, DC motor
├── 04 Signals and Communication/   # Digital/analog, PWM deep dive, UART, I2C, SPI, interrupts
│
├── 05 Coding Patterns/             # Code structure, data types, millis(), edge detection,
│                                   # state machines, moving average filter, hysteresis,
│                                   # map/constrain, modular architecture
│
├── 06 Debugging/                   # 13 patterns: floating pin, millis type bug, String heap
│                                   # fragmentation, buffer init bug, state priority bug, and more
│
├── 07 Projects/
│   ├── P01 Basic LED Blink.md
│   ├── P02 Challenge Blink.md
│   ├── P03 Button Controlled LED.md
│   ├── P04 Toggle LED.md
│   ├── P05 Potentiometer LED Brightness.md
│   ├── P06 Pot Controlled Blink Speed.md
│   ├── P07 Thermometer with Buzzer.md
│   ├── P08 Non-Blocking Blink and Button.md
│   ├── P09 Traffic Light.md
│   ├── P10 Pedestrian Crossing.md
│   ├── P11 TMP36 Servo LED.md
│   └── P12 Smart Environment Monitor.md  # Capstone — design-reviewed FSM
│
├── 08 Reference/
│   ├── Arduino Functions Reference.md
│   ├── Best Practices.md           # 15 non-negotiables with wrong/correct comparisons
│   ├── Circuit Design Checklist.md
│   ├── Glossary.md
│   └── Mistakes and Corrections Log.md
│
└── 09 Review/
    ├── Final Reflection.md         # Module-by-module progression analysis
    └── What to Study Next.md       # Prioritised roadmap to robotics
```

</details>

---

## Development Phases

| Phase | Goal | Status | Outcome |
|---|---|---|---|
| Phase 1 — Modules 1–3 | Hardware fundamentals and component theory | ✅ Complete | Memory model, clock, pin types, and terminology fully grounded |
| Phase 2 — Modules 4–6 | Circuit design, components, and Ohm's Law | ✅ Complete | Correct resistor calculation, breadboard wiring, voltage divider |
| Phase 3 — Modules 7–9 | Programming foundations | ✅ Complete | `millis()`, `map()`/`constrain()`, `F()` macro, data types |
| Phase 4 — Modules 10–11 | Patterns and architecture | ✅ Complete | State machines, edge detection, circular buffer filter, hysteresis |
| Phase 5 — Modules 12–13 | Modular architecture and capstone | ✅ Complete | Formal design doc, 6 pre-code errors caught, `loop()` under 10 lines |
| P12 Capstone | Design-reviewed multi-sensor FSM | ✅ Complete | 4-state FSM, sub-FSM beep sequencer, dual sensor filtering |

> **Note:** Status indicators follow the convention: ✅ Complete · 🔄 In Progress · 🗓 Planned.

---

## Capstone Highlight

<table border="0" width="100%">
<tr>
<td width="55%" valign="top">

**P12 — Smart Environment Monitor**

The capstone is a fully non-blocking embedded monitoring system driven by a formal design document written and reviewed before any code was permitted.

- 4-state FSM: `NORMAL → BORDERLINE → ALERT_ACTIVE → ALERT_ACKNOWLEDGED`
- Dual filtered sensors: TMP36 (temperature) and LDR (light level) — both using 5-sample circular buffer moving average
- 6-step non-blocking beep sub-FSM: 3-beep pattern with gaps and silence, zero `delay()` calls
- Hysteresis on every threshold — two independent `if` statements, never `if/else`
- Button edge detection for alert acknowledgement with explicit transition priority rules
- `loop()` body: **7 lines**, reads like a table of contents
- Design review caught 6 architectural errors before implementation: wrong buffer type (`float` → `int`, saving 20 bytes SRAM), missing `ackFlag` global, undocumented beep step values, unspecified transition priority

</td>
<td width="45%" valign="top" align="center">

```cpp
// Final loop() — 7 lines
void loop() {
  float currentTemp  = readTemperature();
  int   currentLight = readLight();
  checkButton();
  updateState(currentTemp, currentLight);
  updateLEDs(currentState);
  updateBuzzer(currentState);
  printTelemetry(currentTemp,
                 currentLight,
                 currentState);
}
```

*Architecture: inputs → state → outputs, fully decoupled. Adding a 4th sensor changes only `updateState()` — `updateLEDs()` and `updateBuzzer()` are untouched.*

</td>
</tr>
</table>

---

## Navigation Guide

The vault ships four purpose-built navigation paths from `00 Home/🏠 Home.md`.

### Study Path (Linear)

Covers the full curriculum from zero: fundamentals → board anatomy → components → signals → coding patterns → architecture → capstone.

### Debugging Path

Starts at `Hardware Debugging Process` and walks through 13 extracted bug patterns by symptom — useful when something is broken and the cause is unknown.

### Project Path

| Project | Key Concept |
|---|---|
| P01 Basic LED Blink | `pinMode`, `digitalWrite`, `delay` |
| P02 Challenge Blink | `for` loop, timing |
| P03 Button Controlled LED | `digitalRead`, `INPUT_PULLUP` |
| P04 Toggle LED | Edge detection, state variable |
| P05 Potentiometer LED Brightness | `analogRead`, `map`, `analogWrite` |
| P06 Pot Controlled Blink Speed | Conditional blink timing |
| P07 Thermometer with Buzzer | TMP36, `tone()`, threshold logic |
| P08 Non-Blocking Blink and Button | `millis()`, simultaneous tasks |
| P09 Traffic Light | State machine, `enum`, `setLight()` |
| P10 Pedestrian Crossing | Dual `millis()` timers, `WARN` blink |
| P11 TMP36 Servo LED | Filter, hysteresis, servo mapping |
| P12 Smart Environment Monitor | Full FSM capstone |

### Revision Path

Quick-access to `Best Practices`, `Glossary`, `Circuit Design Checklist`, `Arduino Functions Reference`, and `Mistakes and Corrections Log` — suited for pre-build review sessions.

---

## Engineering Notes

> [!NOTE]
> The vault uses a **state-as-interface** architectural principle throughout. Output actuators (`updateLEDs`, `updateBuzzer`) are deliberately decoupled from sensor inputs — they read only the current state enum. This means adding a new sensor to P12 requires modifying exactly one function (`updateState`) and one line in `loop()`. No output function changes. This is documented and proven in `09 Review/Final Reflection.md`.

> [!IMPORTANT]
> All timing variables in the project files use `unsigned long`. Using `int` for `millis()` values causes silent overflow at approximately 32 seconds — documented as a real bug in `06 Debugging/millis Type Bug.md`. Every `previousTime`, `startTime`, and interval variable throughout the vault is typed `unsigned long` without exception.

> [!WARNING]
> The `String` object class is explicitly prohibited across all project code in this vault. Dynamic heap allocation on the ATmega328P's 2 KB SRAM causes heap fragmentation that manifests as random crashes after hours of runtime — not at compile time, not at startup. Use `Serial.print(F("..."))` with the `F()` macro for all string literals. The full failure mode is documented in `06 Debugging/String Object Heap Fragmentation.md`.

### Known Limitations

- All project simulations were built in Tinkercad — physical hardware results may differ due to real-world noise, capacitance, and component tolerances
- Light thresholds in P12 are fixed constants; dynamic baseline calibration (sampling ambient at `setup()`) is planned but not yet implemented
- `Servo.h` disables hardware PWM on pins 9 and 10 (Timer1 conflict) — any project using a servo cannot use `analogWrite()` on those pins
- No unit tests exist for the embedded C++ code; validation was done by simulation and Serial Monitor inspection

---

## Roadmap

- [ ] I2C OLED display (SSD1306) — extend P12 with visual output, remove Serial Monitor dependency
- [ ] DS3231 RTC — add timestamps to telemetry for data logging
- [ ] Interrupt-driven button handling — replace polling in P12 with hardware interrupt on pin 2
- [ ] EEPROM persistence — save light baseline calibration across power cycles (with wear-leveling)
- [ ] L298N H-bridge DC motor control — directional and speed control via PWM
- [ ] HC-SR04 ultrasonic sensor — distance measurement and obstacle detection
- [ ] PID control basics — servo and motor position control with proportional correction
- [ ] ESP8266/ESP32 WiFi — post P12 sensor data to a web dashboard via HTTP/MQTT

---

## Contributing

The vault is a personal engineering reference, but corrections, additions, and new debugging patterns are welcome. Open an issue with:

- The **file path** of the note being corrected
- The **specific factual error or gap**
- A **corrected version or source** — not just "this is wrong"

For new content, follow the existing note structure: goal, wiring, code, what-was-learned, related notes. Keep Obsidian wiki-link syntax (`[[Note Name]]`) intact so the graph stays connected.

> [!IMPORTANT]
> Before opening a pull request, verify that all Obsidian internal links resolve — a broken `[[wikilink]]` severs the graph and breaks the Concept Index. Run a quick link check in Obsidian's broken links panel (`Settings → Files & Links`) before submitting.

---

## License

[![License: MIT](https://img.shields.io/badge/License-MIT-22c55e?style=for-the-badge)](./LICENSE)

Distributed under the MIT License. See `LICENSE` for full terms.

---

<div align="center">
<sub>Built with ♥ by Relvixx &nbsp;·&nbsp; Arduino Vault &nbsp;·&nbsp; 2026</sub>
</div>
