# P06 — Potentiometer Controls Blink Speed

**Tags:** #project #potentiometer #LED #analogRead #conditional

---

## Goal

Potentiometer position controls LED blink speed:
- Above halfway (raw > 512) → blink fast (100ms)
- At or below halfway (raw ≤ 512) → blink slowly (1000ms)
- Print raw sensor value to Serial Monitor every loop

---

## Components

Same as [[P05 Potentiometer LED Brightness]].

---

## Wiring Logic

Same as [[P05 Potentiometer LED Brightness]] —
except LED can stay on pin 13 (no PWM needed for on/off blink).

---

## Design (Plain English)

```
1. When sensorValue > 512: fast blink — ON 100ms, OFF 100ms
2. When sensorValue ≤ 512: slow blink — ON 1000ms, OFF 1000ms
3. Every loop: print raw sensor value to Serial
```

---

## Code (Corrected Final Version)

```cpp
void setup() {
  pinMode(13, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int sensorValue = analogRead(A0);

  if (sensorValue > 512) {
    digitalWrite(13, HIGH);
    delay(100);
    digitalWrite(13, LOW);
    delay(100);               // complete blink cycle
  }
  else if (sensorValue <= 512) {
    digitalWrite(13, HIGH);
    delay(1000);
    digitalWrite(13, LOW);
    delay(1000);              // complete blink cycle
  }

  Serial.println(sensorValue);
}
```

---

## Key Corrections Made During Course

**First attempt problems:**
1. Incomplete blink cycles — LOW had no delay after it
2. `> 512` and `< 512` missed the boundary exactly at 512
3. Dead variable (`brightness`) calculated but never used
4. Stray `delay(100)` added uncontrolled OFF time to every cycle
5. Plain English logic step skipped

**Fixes applied:**
- Added delay after every LOW
- Changed `< 512` to `<= 512` to capture the boundary
- Removed dead variable
- Removed stray delay

---

## What Was Learned

- A complete blink cycle = HIGH + delay + LOW + delay
- Boundary conditions (`<=` vs `<`) must be deliberate choices
- Dead code (variables never used) must be removed
- Plan logic in plain English before writing conditions
- `Serial.println()` adds a newline — without it values run together

---

## Related Notes

- [[Potentiometer]]
- [[Analog Pins]]
- [[P05 Potentiometer LED Brightness]]
- [[P08 Non-Blocking Blink and Button]]
