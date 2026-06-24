# Analog Pins

**Tags:** #hardware #pins #analog #ADC

---

## Definition

Analog pins (A0–A5) can read **any voltage between 0V and 5V**
and convert it to a number from **0 to 1023**.

Digital pins know only HIGH or LOW.
Analog pins know the full range.
Digital = light switch (on/off)

Analog  = dimmer knob (any value from 0 to max)

---

## The ADC

The conversion is performed by the **ADC — Analog to Digital Converter**
built into the ATmega328P.

The Uno's ADC is **10-bit**:
2^10 = 1024 steps

0V   →  0

5V   →  1023

2.5V →  511

To convert a raw reading back to voltage:
```cpp
float voltage = analogRead(A0) * (5.0 / 1023.0);
```

---

## Critical Distinction — Input Only

**Analog pins A0–A5 are INPUT only for analog signals.**
They CANNOT output a variable voltage.

For variable output, use PWM pins (3, 5, 6, 9, 10, 11).
See [[PWM Pins]] and [[PWM Deep Dive]].

> This is one of the most common beginner confusions.
> Analog pins READ. PWM pins SIMULATE variable output.

---

## Analog Pins as Digital

Analog pins CAN function as digital I/O:
```cpp
pinMode(A0, OUTPUT);
digitalWrite(A0, HIGH);  // valid
```

**Warning:** If any external component is connected expecting
to drive A0, setting it as OUTPUT creates two voltage sources
on the same wire — damages both the pin and the component.

---

## analogRead() Details

```cpp
int value = analogRead(A0);   // returns 0–1023
```

- Takes approximately **100 microseconds** per call
- Calling it 10 times = 1ms of read time
- Floating analog pin = random garbage values (antenna effect)
- Default reference = 5V; can change with `analogReference()`

See [[Floating Pin Bug]].

---

## Common Uses

| Sensor | Signal type | Pin |
|---|---|---|
| Potentiometer | Voltage proportional to position | A0–A5 |
| LDR | Voltage proportional to light | A0–A5 |
| TMP36 | Voltage proportional to temperature | A0–A5 |

---

## Related Notes

- [[Digital Pins]]
- [[PWM Pins]]
- [[Potentiometer]]
- [[LDR]]
- [[TMP36]]
- [[Floating Pin Bug]]
- [[map and constrain]]
