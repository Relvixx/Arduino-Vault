# P07 — Thermometer with Buzzer Alert

**Tags:** #project #TMP36 #buzzer #temperature #threshold

---

## Goal

Read temperature continuously.
Display readings on Serial Monitor.
If temperature exceeds 30°C — buzz the buzzer as an alarm.

---

## Components

| Component | Quantity | Notes |
|---|---|---|
| Arduino Uno | 1 | |
| Breadboard | 1 | |
| TMP36 sensor | 1 | Check orientation carefully |
| Passive buzzer | 1 | Uses tone() |
| Jumper wires | 6 | |

---

## Wiring Logic

```
TMP36 (flat face toward you):
  Left leg  → 5V
  Middle leg → A0
  Right leg  → GND

Buzzer:
  Pin 9 → buzzer positive
  Buzzer negative → GND
```

---

## Design (Plain English)

```
1. Every loop:
   - Read A0, convert to voltage, convert to Celsius
   - Print raw value, voltage, and temperature to Serial
2. If temperature > 30°C:
   - Buzzer ON at 440 Hz for 500ms
   - Then OFF
3. If temperature ≤ 30°C:
   - Buzzer stays OFF
4. Wait 1 second before repeating
```

---

## Code

```cpp
void setup() {
  pinMode(9, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int raw        = analogRead(A0);
  float voltage  = raw * (5.0 / 1023.0);
  float tempC    = (voltage - 0.5) / 0.01;

  if (tempC > 30) {
    tone(9, 440);
    delay(500);
    noTone(9);
  } else {
    noTone(9);
  }

  Serial.print(F("Raw: "));
  Serial.print(raw);
  Serial.print(F(" Voltage: "));
  Serial.print(voltage);
  Serial.print(F(" Temperature: "));
  Serial.print(tempC);
  Serial.println(F(" °C"));

  delay(1000);
}
```

---

## What Was Learned

- TMP36 formula: `tempC = (voltage - 0.5) / 0.01`
  equivalent to `(voltage - 0.5) * 100.0`
- Must use `5.0` not `5` — integer division gives 0
- `tone(pin, freq)` for passive buzzer, `noTone(pin)` to stop
- Passive buzzer uses `tone()`, active buzzer uses `digitalWrite()`
- TMP36 pin orientation is critical — reversed → instant damage
- F() macro on all Serial string literals

---

## What Could Be Improved

- Uses `delay()` — system freezes while buzzing
- No moving average filter — raw readings are noisy
- No hysteresis — threshold chatters at exactly 30°C
- These improvements applied in [[P11 TMP36 Servo LED]]

---

## Related Notes

- [[TMP36]]
- [[Buzzer]]
- [[Analog Pins]]
- [[Moving Average Filter]]
- [[Hysteresis]]
- [[P11 TMP36 Servo LED]]
