# Resistor

**Tags:** #component #passive #resistance

---

## What It Is

A **passive component** that opposes current flow.
It dissipates electrical energy as heat.

It does not generate, store, or amplify — only resists.

---

## Why It Matters for Arduino

1. **LED protection** — limits current to safe 20mA
2. **Pull-up/pull-down** — gives floating pins a stable default
3. **Voltage divider** — converts sensor resistance to readable voltage

---

## Reading Color Codes (4-band)
[ Band 1 ][ Band 2 ][ Band 3 multiplier ][ Band 4 tolerance ]
Example: RED | RED | BROWN | GOLD

Band 1 (RED)   = 2

Band 2 (RED)   = 2

Band 3 (BROWN) = × 10¹

Band 4 (GOLD)  = ± 5% tolerance
Value = 22 × 10 = 220Ω ± 5%

### Color → Number

| Color | Value |
|---|---|
| Black | 0 |
| Brown | 1 |
| Red | 2 |
| Orange | 3 |
| Yellow | 4 |
| Green | 5 |
| Blue | 6 |
| Violet | 7 |
| Grey | 8 |
| White | 9 |
| Gold (multiplier) | × 0.1 |
| Silver (multiplier) | × 0.01 |

**Memorise first five:** Black, Brown, Red, Orange, Yellow (0–4).
They appear most often.

---

## Standard Values You Will Use

| Value | Purpose |
|---|---|
| 220Ω | LED current limiting (red/yellow/green on 5V) |
| 100–150Ω | LED current limiting (blue/white on 5V) |
| 1kΩ | Transistor base current limiting |
| 10kΩ | Pull-up / pull-down / LDR voltage divider |

---

## Resistors Are Not Polarised

Unlike LEDs and capacitors, resistors have no + or – leg.
Either leg can face either direction — they work the same.

---

## Related Notes

- [[LED]]
- [[Ohms Law]]
- [[Push Button]]
- [[LDR]]
- [[Voltage Divider]]
- [[Wrong Resistor Calculation]]
