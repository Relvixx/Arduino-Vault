# LED — Light Emitting Diode

**Tags:** #component #output #LED

---

## What It Is

A **diode** that emits light when current flows through it
in the correct direction.

The word *diode* is the critical part:
a diode allows current to flow in **one direction only**.
It is a one-way valve for electricity.

---

## Polarity — Which Way Round
  longer leg = ANODE (+)   → current enters here
       │
      ╱│
     ╱ │  ← diode symbol (triangle + line)
     ╲ │
      ╲│
       │
  shorter leg = CATHODE (–) → current exits here
Current must flow: ANODE → CATHODE (+ to –)

**Reversed LED:** Does nothing. Not damaged.
Diode blocks reverse current — silently.

> Debugging trap: A reversed LED looks identical to a burnt-out LED.
> Before discarding, flip it around and test.

---

## Key Specifications

| Spec | Typical value | What it means |
|---|---|---|
| Forward voltage | 1.8V–3.3V | Voltage consumed by the LED |
| Max current | 20mA | Exceed this → permanent burnout |

### Forward Voltage by Color

| Color | Vf |
|---|---|
| Red | ~2.0V |
| Yellow | ~2.1V |
| Green | ~2.2V |
| Blue | ~3.0V |
| White | ~3.2V |

Different colors need different resistors on the same supply.

---

## Mandatory Resistor Calculation

An LED has near-zero resistance by itself.
Without a resistor: `I = 5V / ~0Ω = enormous` → instant burnout.
Supply voltage       = 5V

LED forward voltage  = 2V  (red)

Voltage across R     = 5V - 2V = 3V

Required current     = 20mA = 0.02A
R = V / I = 3 / 0.02 = 150Ω

→ Round up to 220Ω (standard safe value)

**220Ω is the standard resistor for red/yellow/green LEDs on 5V.**
Blue and white LEDs on 5V:
R = (5 - 3.0) / 0.02 = 100Ω minimum → use 100Ω or 150Ω

---

## Wiring Pattern
Arduino pin → [220Ω resistor] → LED anode (+)

LED cathode (–) → GND

---

## The Built-in LED

Pin 13 has an LED soldered directly on the Uno board.
Use it for testing code without any external components.

---

## Common Mistakes

| Mistake | Result |
|---|---|
| No resistor | Burns out instantly |
| Reversed polarity | Nothing — flip it |
| Using wrong resistor for blue/white LED | Under-protected — degrades faster |
| Two LEDs on one pin, no buffer | Exceeds 40mA pin limit |

---

## Related Notes

- [[Resistor]]
- [[Ohms Law]]
- [[Digital Pins]]
- [[PWM Deep Dive]]
- [[Wrong Resistor Calculation]]
- [[P01 Basic LED Blink]]
