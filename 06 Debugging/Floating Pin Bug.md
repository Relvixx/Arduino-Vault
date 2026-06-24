# Floating Pin Bug

**Tags:** #debugging #input #button #analog

---

## Symptom

- Button triggers randomly without being pressed
- Analog pin returns random values with nothing connected
- Input pin reads HIGH and LOW unpredictably

---

## Cause

A pin configured as `INPUT` with nothing connected to it
has no defined voltage. It is in a **floating state** —
acting as an antenna, picking up electromagnetic noise
from the environment: nearby wires, your hand, the air.

The pin randomly reads HIGH or LOW with no predictable pattern.

---

## The Fix

### For digital input (buttons):

```cpp
// Option 1: Use INPUT_PULLUP — easiest, no external component
pinMode(2, INPUT_PULLUP);
// Unpressed = HIGH, Pressed = LOW (inverted logic)

// Option 2: External pull-down resistor
// 5V → button → pin → 10kΩ → GND
// Unpressed = LOW, Pressed = HIGH
```

### For analog input:

Never leave an analog pin connected to nothing while reading it.
Always have a complete voltage divider or signal connected.

---

## Prevention

- **Always** use `INPUT_PULLUP` for button inputs
- **Always** verify analog pins have a signal path
- In Tinkercad: hover over pins to confirm connections
- If a sensor is disconnected, expect garbage readings

---

## Why INPUT_PULLUP Inverts the Logic

With pull-up enabled:
- The internal resistor connects the pin to 5V
- Unpressed button: no path to GND → pin reads HIGH (5V)
- Pressed button: connects pin to GND → pin reads LOW (0V)

```cpp
// Correct reading with INPUT_PULLUP:
if (digitalRead(2) == LOW) {
  // button IS pressed
}
```

Getting this backwards is one of the top beginner mistakes.

---

## Related Notes

- [[Push Button]]
- [[Edge Detection]]
- [[Analog Pins]]
- [[Digital Pins]]
- [[Hardware Debugging Process]]
