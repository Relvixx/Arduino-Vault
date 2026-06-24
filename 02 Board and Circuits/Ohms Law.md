# Ohm's Law

**Tags:** #electronics #circuits #fundamentals

---

## The Equation
V = I × R
V = Voltage (Volts)

I = Current (Amperes)

R = Resistance (Ohms, Ω)

Three forms of the same truth:
V = I × R     → find voltage

I = V / R     → find current

R = V / I     → find resistance

---

## The Water Pipe Analogy

| Electricity | Water |
|---|---|
| Voltage | Water pressure |
| Current | Flow rate |
| Resistance | Pipe narrowness |

More pressure + narrow pipe = less flow.
More voltage + more resistance = less current.

---

## Why This Matters for Arduino

Every time you connect an LED, you need a resistor.
Ohm's Law tells you exactly which one.

### Example: Resistor for a Red LED
Supply voltage:        5V

LED forward voltage:   2V  (LED "uses" this)

Voltage across R:      5V - 2V = 3V

Required current:      20mA = 0.02A
R = V / I

R = 3 / 0.02

R = 150Ω  → round up to 220Ω (standard value)

**This calculation is mandatory before every LED connection.**

---

## LED Forward Voltages (for calculation)

| Color | Forward voltage |
|---|---|
| Red | ~2.0V |
| Yellow | ~2.1V |
| Green | ~2.2V |
| Blue | ~3.0V |
| White | ~3.2V |

Blue and white LEDs need a different resistor than red ones
on the same 5V supply.

---

## What Happens Without a Resistor
I = V / R

I = 5V / ~0Ω (LED alone)

I = nearly infinite

LED burns out in under one second. No recovery.

---

## Related Notes

- [[LED]]
- [[Resistor]]
- [[Series vs Parallel]]
- [[Voltage Divider]]
- [[Wrong Resistor Calculation]]
