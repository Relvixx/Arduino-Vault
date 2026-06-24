# PWM Deep Dive

**Tags:** #signals #PWM #output #analog

---

## The Problem PWM Solves

Arduino output pins can only produce 5V or 0V.
Many real-world applications need values in between:
LED brightness, motor speed, servo position.

PWM's solution: **switch the pin on and off so fast
that the component experiences the average voltage.**

---

## Duty Cycle — The Key Concept

Duty cycle = percentage of time the signal is HIGH
in one complete on/off cycle.
100% duty cycle:  ████████████████  → full power

75% duty cycle:  ████████████░░░░  → 3/4 power

50% duty cycle:  ████████░░░░░░░░  → half power

25% duty cycle:  ████░░░░░░░░░░░░  → quarter power

0% duty cycle:  ░░░░░░░░░░░░░░░░  → off

The component responds to the **average**, not the peaks.
An LED on 50% duty cycle appears half as bright.
A motor on 50% duty cycle runs at roughly half speed.

---

## The analogWrite() Function

```cpp
analogWrite(pin, value);
// value: 0 (0% duty cycle) to 255 (100% duty cycle)
```

The mapping:
analogWrite(9, 0)   → 0%   duty cycle

analogWrite(9, 64)  → 25%  duty cycle

analogWrite(9, 128) → 50%  duty cycle

analogWrite(9, 191) → 75%  duty cycle

analogWrite(9, 255) → 100% duty cycle

Formula for duty cycle percentage:
Duty cycle % = (value / 255) × 100

---

## PWM Pins and Frequencies

Only pins marked ~ support PWM: **3, 5, 6, 9, 10, 11**

| Pins | Frequency |
|---|---|
| 5, 6 | ~980 Hz |
| 3, 9, 10, 11 | ~490 Hz |

Switching happens faster than the eye can detect (~60Hz flicker
threshold). At 490Hz, an LED appears as steady brightness.

---

## What PWM Is NOT

PWM does not output a true intermediate voltage.
Between the digital pin and GND, you would measure a
square wave alternating between 0V and 5V at high speed.

The component "sees" an average — but the signal itself
is purely digital at the pin level.

For precision analog circuits requiring a true DC voltage,
PWM is insufficient. Use a DAC (digital-to-analog converter).

---

## Timer Conflicts

| Timer | PWM pins | Conflicts with |
|---|---|---|
| Timer0 | 5, 6 | `millis()`, `delay()` — do NOT disable |
| Timer1 | 9, 10 | `Servo.h` library |
| Timer2 | 3, 11 | `tone()` function |

Practical rules:
- Using `tone()` → avoid `analogWrite()` on pins 3 and 11
- Using `Servo.h` → avoid `analogWrite()` on pins 9 and 10
- Never disable Timer0 — it breaks `millis()` and `delay()`

---

## Related Notes

- [[PWM Pins]]
- [[Analog Pins]]
- [[Digital vs Analog Signals]]
- [[Servo Motor]]
- [[Buzzer]]
- [[LED]]
- [[map and constrain]]
