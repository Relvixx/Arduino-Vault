# P05 — Potentiometer Controls LED Brightness

**Tags:** #project #potentiometer #LED #analogRead #analogWrite #PWM

---

## Goal

Turn a potentiometer knob → LED brightness changes proportionally.
Introduces analog input, ADC, PWM output, and `map()`.

---

## Components

| Component | Quantity | Notes |
|---|---|---|
| Arduino Uno | 1 | |
| Breadboard | 1 | |
| Potentiometer | 1 | 10kΩ |
| LED | 1 | |
| Resistor | 1 | 220Ω |
| Jumper wires | 5 | |

---

## Wiring Logic

```
Potentiometer:
  Left leg  → GND
  Right leg → 5V
  Middle leg (wiper) → A0

LED:
  Pin 9 (PWM~) → 220Ω → LED anode
  LED cathode  → GND
```

**Why pin 9, not pin 13?**
`analogWrite()` requires a PWM pin (marked ~).
Pin 13 is digital only. Pin 9 supports PWM.

---

## Code

```cpp
void setup() {
  pinMode(9, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int sensorValue = analogRead(A0);                       // 0–1023
  int brightness  = map(sensorValue, 0, 1023, 0, 255);   // scale
  brightness      = constrain(brightness, 0, 255);        // clamp
  analogWrite(9, brightness);

  Serial.print(F("Raw: "));
  Serial.print(sensorValue);
  Serial.print(F("  Brightness: "));
  Serial.println(brightness);

  delay(100);
}
```

---

## Key Concepts Introduced

- `analogRead()` returns 0–1023 (10-bit ADC)
- `analogWrite()` takes 0–255 (8-bit PWM)
- `map()` scales between ranges proportionally
- `constrain()` clamps values to valid range
- **Always use map() and constrain() together**
- Serial Monitor shows real-time values for debugging

---

## What Was Learned

- ADC range (0–1023) ≠ PWM range (0–255)
- `map()` does not clamp — constrain() is mandatory
- `Serial.print()` is the most important debugging tool
- PWM output is not true analog voltage — it is a fast square wave
  the LED experiences as average brightness

---

## Related Notes

- [[Potentiometer]]
- [[Analog Pins]]
- [[PWM Pins]]
- [[map and constrain]]
- [[UART]]
- [[P06 Pot Controlled Blink Speed]]
