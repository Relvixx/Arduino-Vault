# Series vs Parallel

**Tags:** #electronics #circuits

---

## Series — One Path

Components are chained one after another.
Current must pass through all of them in sequence.
5V ──[ R ]──[ LED ]──[ LED ]── GND

**Rules:**
- Same current flows through ALL components
- Voltage SPLITS between them (each takes its share)
- If ONE component fails open → entire circuit breaks

**Analogy:** Old Christmas lights — one bulb dies, all go dark.

**Arduino use:** Rarely intentional. Connecting two LEDs in series
requires a different resistor calculation.

---

## Parallel — Multiple Paths

Components share the same two nodes but have independent paths.
5V ──┬──[ R ]──[ LED ]──┬── GND

│                  │

└──[ R ]──[ LED ]──┘

**Rules:**
- Same voltage across ALL branches
- Current SPLITS between branches
- If ONE branch fails → others keep working

**Analogy:** House wiring — one light dies, others stay on.

---

## For Arduino Projects

Use **parallel** when controlling multiple LEDs independently.
Each LED gets its own:
- Pin
- Resistor
- Path to GND
Pin 9  → R → LED1 → GND

Pin 10 → R → LED2 → GND

Pin 11 → R → LED3 → GND

---

## Current Implications

In parallel, each branch draws current independently.
Three LEDs at 20mA each = 60mA total from Arduino.

Check: does total exceed 200mA combined pin limit?
See [[Power Pins]] and [[Arduino Uno Anatomy]].

---

## Related Notes

- [[Ohms Law]]
- [[LED]]
- [[Breadboard Internals]]
- [[Voltage Divider]]
