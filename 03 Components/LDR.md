# LDR — Light Dependent Resistor

**Tags:** #component #input #analog #sensor

---

## What It Is

A resistor whose resistance changes based on ambient light level.
Bright light  →  low resistance  (~100Ω)

Darkness      →  high resistance (~10,000Ω+)

---

## Why It Needs a Voltage Divider

Arduino reads **voltage**, not resistance.
The LDR changes resistance, not voltage.

Solution: Wire it in a voltage divider with a fixed 10kΩ resistor:
5V ──[ LDR ]──┬── A0

│

[ 10kΩ ]

│

GND

- Bright → LDR resistance drops → more voltage at A0 → higher reading
- Dark → LDR resistance rises → less voltage at A0 → lower reading

See [[Voltage Divider]].

---

## Reading It

```cpp
int lightLevel = analogRead(A1);   // returns 0–1023
```

**Higher reading = brighter. Lower reading = darker.**
(Assuming standard voltage divider with fixed resistor to GND.)

---

## Calibration

The exact values for "bright" and "dark" depend on:
- Your specific LDR batch
- Room ambient lighting conditions
- The fixed resistor value chosen

**Process:** Print raw values in your environment.
Measure in bright and dark conditions.
Set thresholds based on real measurements, not assumptions.

---

## Threshold with Hysteresis

To avoid flickering at the threshold:
```cpp
const int LIGHT_ALERT_ON  = 200;   // lights drops below → alert
const int LIGHT_ALERT_OFF = 215;   // light rises above → clear

if (lightLevel < LIGHT_ALERT_ON)  { alertActive = true; }
if (lightLevel > LIGHT_ALERT_OFF) { alertActive = false; }
```

The gap between 200 and 215 is the **hysteresis band**.
Inside this band: no change. Prevents chatter.

See [[Hysteresis]].

---

## Dynamic Baseline (Advanced)

Fixed thresholds break over a 24-hour period as daylight changes.

Better approach: Sample ambient light at startup and compute
thresholds as a percentage of the baseline:
```cpp
int baseline = 0;
for (int i = 0; i < 10; i++) {
  baseline += analogRead(A1);
  delay(10);
}
baseline /= 10;
int LIGHT_ALERT_ON = baseline * 0.40;
```

---

## Common Mistakes

| Mistake | Result |
|---|---|
| No voltage divider | Cannot read resistance directly |
| Fixed threshold for 24-hour operation | False alerts as daylight changes |
| No hysteresis | Output flickers at threshold |
| Floating A1 pin | Random garbage readings |

---

## Related Notes

- [[Voltage Divider]]
- [[Analog Pins]]
- [[Hysteresis]]
- [[Moving Average Filter]]
- [[Floating Pin Bug]]
- [[P12 Smart Environment Monitor]]
