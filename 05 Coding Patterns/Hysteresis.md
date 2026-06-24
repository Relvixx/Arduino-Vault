# Hysteresis

**Tags:** #coding #pattern #sensors #threshold

---

## The Problem

A single threshold causes chatter when the signal hovers
near the trigger point:

```cpp
if (sensorValue > 512) { ledOn = true; }
else                   { ledOn = false; }
```

If sensor fluctuates between 510 and 514:
514 → ON → 510 → OFF → 513 → ON → 511 → OFF → ...
The LED flickers dozens of times per second.

---

## The Solution — Two Separate Thresholds

```cpp
const int THRESHOLD_HIGH = 530;   // must exceed this to turn ON
const int THRESHOLD_LOW  = 490;   // must drop below this to turn OFF

if (sensorValue > THRESHOLD_HIGH) { outputOn = true; }
if (sensorValue < THRESHOLD_LOW)  { outputOn = false; }
// Between 490 and 530: NO CHANGE — stable zone
```

The gap between thresholds is the **hysteresis band**.
The signal must move clearly past the band to trigger a change.

---

## Critical Implementation Detail

This MUST use two separate `if` statements, not `if/else`:

```cpp
// CORRECT — both checks independent
if (sensorValue > THRESHOLD_HIGH) { outputOn = true; }
if (sensorValue < THRESHOLD_LOW)  { outputOn = false; }

// WRONG — else overrides the HIGH check
if (sensorValue > THRESHOLD_HIGH) { outputOn = true; }
else { outputOn = false; }   // fires when value is in the BAND too
```

With `if/else`, when the value is inside the hysteresis band,
the `else` fires and turns output OFF. Hysteresis is destroyed.

---

## Real-World Example — Thermostat

A thermostat set to 20°C does not turn on at 19.99°C and off at
20.01°C — that would cause the heater to cycle every few seconds.

Instead:
- Turns ON at 19°C (threshold low)
- Turns OFF at 21°C (threshold high)

The 2°C band prevents rapid cycling.
This is hysteresis in a real product.

---

## Capstone Example — Four Thresholds

```cpp
// Temperature
const float TEMP_ALERT_ON   = 35.5;   // enter alert above this
const float TEMP_ALERT_OFF  = 34.5;   // leave alert below this
const float TEMP_BORDER_ON  = 30.5;   // enter borderline above this
const float TEMP_BORDER_OFF = 29.5;   // leave borderline below this

// Light
const int LIGHT_ALERT_ON  = 200;      // alert if below this
const int LIGHT_ALERT_OFF = 215;      // clear alert if above this
```

Each threshold has a separate ON and OFF value.
The gap between them is the hysteresis band.

---

## Related Notes

- [[Moving Average Filter]]
- [[State Machines]]
- [[LDR]]
- [[TMP36]]
- [[P11 TMP36 Servo LED]]
- [[P12 Smart Environment Monitor]]
