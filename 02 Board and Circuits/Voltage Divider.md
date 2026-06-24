# Voltage Divider

**Tags:** #electronics #circuits #sensors

---

## What It Is

A circuit that produces an output voltage proportional to
the ratio of two resistors. Used whenever a sensor's resistance
needs to be converted to a voltage that Arduino can read.

---

## The Circuit
VCC (5V)

│

[ R1 ]

│

├──── Output (to analog pin)

│

[ R2 ]

│

GND

Output voltage:
Vout = VCC × (R2 / (R1 + R2))

---

## Why Sensors Need This

Arduino's analog pins read **voltage**, not **resistance**.
Sensors like the LDR change their **resistance** based on the
environment. You cannot read resistance directly.

Solution: Place the sensor as one resistor in a voltage divider.
As the sensor resistance changes, the output voltage changes.
Arduino reads the voltage change.

---

## LDR Example
5V ──[ LDR ]──┬── A0 (read here)

│

[ 10kΩ ]

│

GND

- Bright light → LDR resistance drops → more voltage at A0 → higher reading
- Darkness → LDR resistance rises → less voltage at A0 → lower reading

The 10kΩ fixed resistor is chosen to sit in the middle of the
LDR's resistance range — maximizing sensitivity.

---

## Potentiometer as a Built-in Voltage Divider

A potentiometer IS a voltage divider with an adjustable tap point.
The wiper pin outputs any voltage between 0V and VCC depending
on knob position. See [[Potentiometer]].

---

## Related Notes

- [[LDR]]
- [[Potentiometer]]
- [[Analog Pins]]
- [[Ohms Law]]
- [[Series vs Parallel]]
