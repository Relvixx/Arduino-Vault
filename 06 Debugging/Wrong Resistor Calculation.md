# Wrong Resistor Calculation Bug

**Tags:** #debugging #resistor #LED #circuit

---

## Symptom

- LED is very dim (resistor too high)
- LED burns out quickly or instantly (resistor too low or missing)
- LED brightness is inconsistent between different color LEDs
  using the same resistor value

---

## Cause

Incorrect application of Ohm's Law — specifically using the
wrong voltage value in the resistor calculation.

### Most Common Error:

```
Using LED forward voltage instead of voltage ACROSS the resistor.

WRONG:
R = V_led / I = 2.0 / 0.02 = 100Ω

CORRECT:
Voltage across R = Supply - V_led = 5V - 2V = 3V
R = V_resistor / I = 3 / 0.02 = 150Ω
```

Using 100Ω instead of 150Ω lets too much current through.
Over time, the LED degrades. Under stress, it burns immediately.

---

## The Correct Calculation

```
Given:
  Supply voltage (Arduino pin) = 5V
  LED forward voltage (Vf)     = 2.0V (red)
  Target current               = 20mA = 0.02A

Step 1: Find voltage across resistor
  V_R = 5V - 2.0V = 3V

Step 2: Apply Ohm's Law
  R = V_R / I = 3 / 0.02 = 150Ω

Step 3: Round to standard value
  150Ω → use 220Ω (more conservative, slightly dimmer, safer)
```

---

## Forward Voltages by LED Color

| Color | Vf | R (at 20mA on 5V) |
|---|---|---|
| Red | 2.0V | 150Ω → use 220Ω |
| Yellow | 2.1V | 145Ω → use 220Ω |
| Green | 2.2V | 140Ω → use 220Ω |
| Blue | 3.0V | 100Ω → use 100–150Ω |
| White | 3.2V | 90Ω → use 100Ω |

**Blue and white LEDs need a different resistor than red on 5V.**
Using a 220Ω resistor with a blue LED on 5V is safe but very dim.

---

## Prevention

Calculate before wiring. Never guess.
Memorize the formula: `R = (Supply - Vf) / I`
Always use the voltage *across the resistor*, not the LED's Vf alone.

---

## Related Notes

- [[Ohms Law]]
- [[LED]]
- [[Resistor]]
- [[Hardware Debugging Process]]
