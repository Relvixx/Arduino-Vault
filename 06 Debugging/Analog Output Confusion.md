# Analog Output Confusion

**Tags:** #debugging #analog #output #pins

---

## Symptom

- `analogWrite()` called but LED brightness does not change
- Variable brightness not working on a specific pin
- Code looks correct but output behaves like on/off only

---

## Cause

`analogWrite()` (PWM) only works on pins marked with **~**:
**3, 5, 6, 9, 10, 11**

Calling `analogWrite()` on any other pin — such as pin 7 or pin 13 —
does nothing. No error. No warning. Silent failure.

```cpp
analogWrite(7, 128);   // pin 7 is NOT PWM → does nothing
analogWrite(9, 128);   // pin 9 IS PWM → works correctly
```

---

## Secondary Cause — Confusing Analog Input with Analog Output

A0–A5 are **analog INPUT** pins only.
They can read variable voltages. They cannot output variable voltages.

```
Analog pins (A0–A5) → INPUT only → reads 0V to 5V as 0 to 1023
PWM pins (3,5,6,9,10,11) → OUTPUT → simulates variable voltage
```

Calling `analogWrite(A0, 128)` silently does nothing useful.

---

## The Fix

```cpp
// Move your component to a PWM-capable pin
const int LED_PIN = 9;    // pin 9 supports analogWrite()
analogWrite(LED_PIN, brightness);
```

---

## Prevention

- Memorize PWM pins: **3, 5, 6, 9, 10, 11**
- Check the ~ symbol on the board before wiring
- When variable output is needed, plan pin assignment before
  building the circuit

---

## Timer Conflicts That Disable PWM

Even on valid PWM pins, timer conflicts can disable PWM:

| If you use | Pins affected |
|---|---|
| `tone()` | Pins 3 and 11 (Timer2) |
| `Servo.h` | Pins 9 and 10 (Timer1) |

If PWM suddenly stops working after adding Servo or tone(),
move your LED to an unaffected PWM pin.

---

## Related Notes

- [[PWM Pins]]
- [[Analog Pins]]
- [[PWM Deep Dive]]
- [[Digital vs Analog Signals]]
- [[Buzzer]]
- [[Servo Motor]]
