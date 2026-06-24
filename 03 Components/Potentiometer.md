# Potentiometer

**Tags:** #component #input #analog

---

## What It Is

A **variable resistor** — a knob or slider that changes
resistance from 0Ω to a maximum value (commonly 10kΩ).

It has three legs:
Left pin        Right pin
   │                │
GND ─┤════════════════├─ 5V

↑

Wiper pin (middle)

moves along the resistance track
Turned fully left  → wiper reads 0V

Turned fully right → wiper reads 5V

Turned halfway     → wiper reads 2.5V

---

## It Is a Built-in Voltage Divider

The potentiometer is simply a voltage divider with an
adjustable tap point. As the knob turns, the ratio of
resistance above and below the wiper changes — producing
a proportional output voltage.

See [[Voltage Divider]].

---

## How to Read It

```cpp
int raw = analogRead(A0);   // returns 0–1023
```

Map to a useful range:
```cpp
int brightness = map(raw, 0, 1023, 0, 255);
brightness = constrain(brightness, 0, 255);
analogWrite(9, brightness);
```

See [[map and constrain]].

---

## Wiring
Left leg  → GND

Right leg → 5V

Middle leg (wiper) → Analog pin (A0–A5)

Direction of left/right determines whether turning
clockwise increases or decreases the reading.
Either is valid — just be consistent.

---

## Common Uses

| Use | How |
|---|---|
| LED brightness control | Map 0–1023 to 0–255, analogWrite |
| Servo angle control | Map 0–1023 to 0–180, servo.write |
| Blink speed control | Map to delay interval |
| Threshold calibration | Set detection threshold manually |

---

## Related Notes

- [[Analog Pins]]
- [[Voltage Divider]]
- [[map and constrain]]
- [[LDR]]
- [[P05 Potentiometer LED Brightness]]
- [[P06 Pot Controlled Blink Speed]]
