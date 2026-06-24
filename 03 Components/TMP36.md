# TMP36 — Temperature Sensor

**Tags:** #component #input #analog #sensor #temperature

---

## What It Is

A three-pin analog temperature sensor. Output voltage scales
linearly with temperature at 10mV per degree Celsius.

---

## Pin Orientation
  TMP36 (flat face toward you)
[ Left ]  [ Middle ]  [ Right ]

│          │           │

5V       Output        GND

(to A0)

**NEVER reverse the power pins.**
Wrong orientation → sensor heats instantly → permanent damage
within seconds. Check orientation before every connection.

---

## Output Voltage Formula
Vout = 0.5V + (temperature_°C × 0.01V)
At 25°C:  Vout = 0.5 + (25 × 0.01) = 0.75V

At 0°C:   Vout = 0.5 + (0 × 0.01)  = 0.50V

At 100°C: Vout = 0.5 + (100 × 0.01) = 1.50V

---

## Reading Temperature in Code

```cpp
int raw = analogRead(A0);
float voltage = raw * (5.0 / 1023.0);
float tempC = (voltage - 0.5) * 100.0;
// Equivalent: (voltage - 0.5) / 0.01 — same result
```

**Always use `5.0` not `5` in the voltage conversion.**
Integer division `5 / 1023` = 0 in C++.
Float division `5.0 / 1023.0` = 0.00489 — correct.

---

## Calibration

Manufacturing variation means two TMP36 sensors may differ
by 1–2°C in the same environment.

```cpp
const float TEMP_OFFSET = -1.2;   // measured empirically
float tempC = ((voltage - 0.5) * 100.0) + TEMP_OFFSET;
```

Find your offset by comparing against a known reference
(calibrated thermometer, ice water at 0°C, boiling at 100°C).

---

## Using with a Moving Average Filter

Never use raw readings directly for threshold decisions.
Apply a moving average filter to smooth noise:

```cpp
// Store raw ADC int values (not floats) in buffer
// Average the integers, then convert once to temperature
// See [[Moving Average Filter]] for full implementation
```

---

## Why Reversed TMP36 Damages Instantly

The TMP36 is an **active IC** — it contains internal transistors
that draw current. When powered backwards, current flows the wrong
way through internal semiconductor junctions → rapid thermal damage.

An LED reversed is harmless because it simply blocks current
(passive component, diode behavior).
A TMP36 reversed cannot block current — it conducts destructively.

---

## Common Mistakes

| Mistake | Result |
|---|---|
| Reversed power pins | Permanent damage in seconds |
| Using `5` instead of `5.0` | Integer division → 0 → wrong temperature |
| No moving average filter | Noisy readings, threshold chatter |
| Using float buffer instead of int | Wastes 20 bytes of SRAM needlessly |

---

## Related Notes

- [[Analog Pins]]
- [[Moving Average Filter]]
- [[Hysteresis]]
- [[Voltage Divider]]
- [[P07 Thermometer with Buzzer]]
- [[P11 TMP36 Servo LED]]
- [[P12 Smart Environment Monitor]]
