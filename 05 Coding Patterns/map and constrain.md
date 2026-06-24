# map() and constrain()

**Tags:** #coding #functions #scaling

---

## map() — Scale Between Ranges

```cpp
long map(long x, long fromLow, long fromHigh, long toLow, long toHigh);
```

Proportionally converts a value from one range to another:

```cpp
int raw = analogRead(A0);                        // 0–1023
int brightness = map(raw, 0, 1023, 0, 255);     // 0–255
int angle = map(raw, 0, 1023, 0, 180);          // 0–180
```

---

## map() Does NOT Clamp

If the input exceeds the fromHigh value, the output exceeds toHigh:

```cpp
map(1030, 0, 1023, 0, 255);   // returns 256 — outside valid range
```

Sensor noise regularly produces out-of-range readings.
A servo receiving 181° may jitter or damage itself.
`analogWrite()` receiving 256 has undefined behavior.

**Always follow map() with constrain().**

---

## constrain() — Clamp to Range

```cpp
int constrain(int x, int low, int high);
```

Forces a value to stay within bounds:

```cpp
int safe = constrain(mapped, 0, 255);    // never below 0, never above 255
```

Values within range: returned unchanged.
Values below low: low is returned.
Values above high: high is returned.

---

## The Pair — Always Use Together

```cpp
int raw = analogRead(A0);
int mapped = map(raw, 0, 1023, 0, 180);
int angle  = constrain(mapped, 0, 180);   // safe regardless of noise
myServo.write(angle);
```

---

## The Float Argument Bug

`map()` takes `long` integers, not `float`:

```cpp
float temp = 25.7;
int angle = map(temp, 0, 50, 0, 180);    // implicit truncation: 25.7 → 25
```

This truncation happens silently. To make it explicit:

```cpp
int angle = map((int)temp, 0, 50, 0, 180);    // cast shows intent
```

The decimal precision is lost either way — but explicit casting
communicates that you intended the truncation.

See [[map Float Argument Bug]].

---

## Calibrating map() to Real Sensor Range

If your sensor never reaches its theoretical limits:

```cpp
// Potentiometer damaged — only outputs 100–900 instead of 0–1023
int brightness = map(raw, 100, 900, 0, 255);   // use actual range
brightness = constrain(brightness, 0, 255);
```

Rescaling to the actual sensor range uses the full output range.

---

## Related Notes

- [[Analog Pins]]
- [[Potentiometer]]
- [[Servo Motor]]
- [[PWM Deep Dive]]
- [[map Float Argument Bug]]
