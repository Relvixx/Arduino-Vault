# DC Motor

**Tags:** #component #output #motor #actuator #power

---

## The Core Problem

A DC motor typically requires **200–600mA** to run.
Arduino digital pins supply a maximum of **40mA**.

**You can never connect a DC motor directly to an Arduino pin.**
Attempting it will damage the pin and produce no useful motion.

---

## Solution 1 — Transistor (One Direction)

Use the Arduino pin to control a transistor.
The transistor acts as a switch for the high-current motor circuit:
Arduino pin 9 → [1kΩ resistor] → Transistor base

Transistor collector → Motor → External 12V supply

Transistor emitter → GND
[Flyback diode across motor terminals — MANDATORY]

The Arduino pin controls the transistor with a few mA.
The transistor switches the full motor current safely.

**Flyback diode:** Motors generate voltage spikes when switched off
(back-EMF). The diode clamps these spikes and protects the transistor
and Arduino.

This approach: **speed control via PWM, one direction only.**

---

## Solution 2 — L298N H-Bridge (Full Control)

An H-bridge IC allows the motor to spin in **both directions**
and at variable speed:

```cpp
digitalWrite(IN1, HIGH);     // set direction
digitalWrite(IN2, LOW);
analogWrite(ENA, 200);       // speed via PWM (0–255)
```

Reversing direction:
```cpp
digitalWrite(IN1, LOW);
digitalWrite(IN2, HIGH);
analogWrite(ENA, 200);
```

Stop:
```cpp
analogWrite(ENA, 0);
```

---

## Power Planning for Motors

Always use **external power** for motors.
Arduino provides the logic signal — not the drive current.
Arduino GND ─────────────┐

└── shared ground

External 12V supply ──── Motor positive terminal

L298N or transistor ──── controlled by Arduino pin

---

## Related Notes

- [[Digital Pins]]
- [[PWM Pins]]
- [[Power Pins]]
- [[Servo Motor]]
- [[Arduino Uno Anatomy]]
