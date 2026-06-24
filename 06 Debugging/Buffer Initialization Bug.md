# Buffer Initialization Bug

**Tags:** #debugging #filter #startup #sensors

---

## Symptom

- System behaves incorrectly for the first few seconds after power-on
- Wrong state entered immediately at startup (e.g., alert triggers
  before any real condition exists)
- Sensor readings start at extreme values and gradually correct themselves
- In a temperature system: reads -40°C on startup, then corrects

---

## Cause

Moving average filter buffer initialized with zeros.

```cpp
int tempBuffer[10] = {0};   // all zeros
long tempSum = 0;           // total = 0
```

First reading with actual sensor value of ~154 (for ~25°C):

```
tempSum -= tempBuffer[0];   // 0 - 0 = 0
tempBuffer[0] = 154;
tempSum += 154;             // tempSum = 154
average = 154 / 10 = 15.4  // averaged with 9 zeros
voltage = 15.4 × (5/1023) = 0.075V
tempC = (0.075 - 0.5) × 100 = -42.5°C  ← wildly wrong
```

This continues for the first 10 readings (the window size).
Each reading, one zero is replaced by a real value.
After 10 readings, the buffer contains only real values — correct.

---

## The Fix

Pre-fill the buffer with real sensor readings in `setup()`:

```cpp
void setup() {
  // ... pinMode, Serial.begin, etc. ...

  // Pre-fill filter buffers with real readings
  for (int i = 0; i < NUM_SAMPLES; i++) {
    int t = analogRead(TEMP_PIN);
    tempBuffer[i] = t;
    tempSum += t;

    int l = analogRead(LIGHT_PIN);
    lightBuffer[i] = l;
    lightSum += l;
  }
}
```

Now the filter starts with real data. First reading from `loop()`
is immediately accurate.

---

## Prevention

**Any time you implement a moving average filter,
the next line you write is the setup() initialization.**
These two pieces of code belong together in your mental model.

---

## How Many Times This Was Caught in the Course

This bug was identified and corrected **three separate times**:
- Module 11: pointed out during exercise review
- Module 12: pointed out during refactor review
- Module 13: missing from capstone setup() — caught in code review

It is easy to write the filter function correctly and forget
the initialization. Make it automatic.

---

## Related Notes

- [[Moving Average Filter]]
- [[Arrays]]
- [[Arduino Code Structure]]
- [[TMP36]]
- [[LDR]]
