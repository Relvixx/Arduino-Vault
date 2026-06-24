# Moving Average Filter

**Tags:** #coding #pattern #sensors #filtering

---

## The Problem

Real sensors lie — not consistently, but randomly:
Expected: 512 512 512 512 512

Actual:   508 519 511 498 523 507 515

This noise causes threshold-based systems to chatter —
rapidly toggling on and off around the threshold value.

---

## The Solution

Average the last N readings. Random noise cancels out.
Real changes accumulate gradually:
Before filter: 508 519 511 498 523 507 515 ...

After filter:  512 513 512 511 512 511 512 ...

---

## Implementation — Circular Buffer

```cpp
const int NUM_SAMPLES = 10;
int buffer[NUM_SAMPLES] = {0};
int bufferIndex = 0;
long runningTotal = 0;

int getSmoothedReading(int newReading) {
  runningTotal -= buffer[bufferIndex];   // remove oldest value
  buffer[bufferIndex] = newReading;       // store new value
  runningTotal += newReading;             // add new value
  bufferIndex = (bufferIndex + 1) % NUM_SAMPLES;  // advance, wrap
  return runningTotal / NUM_SAMPLES;      // return average
}
```

**Why `long` for `runningTotal`:**
10 readings × max 1023 = 10,230 — exceeds `int` range (32,767 is fine
but `long` is safer and explicit).

**Why `int` for buffer, not `float`:**
Store raw ADC values (integers). Convert to temperature/voltage
AFTER averaging. This saves 20 bytes SRAM over a `float` buffer.

---

## Critical: Buffer Initialization in setup()

```cpp
void setup() {
  // Fill buffer with real readings before main loop starts
  for (int i = 0; i < NUM_SAMPLES; i++) {
    buffer[i] = analogRead(sensorPin);
    runningTotal += buffer[i];
  }
}
```

**Without this:** Buffer starts filled with zeros. First N readings
are averaged with zeros — producing wildly wrong initial values.

A TMP36 reading of 154 averaged with nine zeros gives 15.4 raw,
which converts to approximately -42°C. The system may make
incorrect state transitions at startup.

See [[Buffer Initialization Bug]].

---

## The Trade-off: Smoothing vs Response Speed
Fewer samples (N=3):  → faster response to real changes

→ less noise reduction

More samples (N=10):  → more noise reduction

→ slower response (takes 10 readings to

fully reflect a sudden real change)

For a sudden real change from value 500 to value 900
with a 10-sample window:
Reading 1:  540  (one 900 in 9 zeros → 540)

Reading 2:  580

...

Reading 10: 900  (fully reflected after 10 readings)

Choose N based on how fast your signal changes vs how much
noise you need to eliminate.

---

## Two Independent Filters

For two sensors, maintain separate buffers:

```cpp
int tempBuffer[10] = {0};
int lightBuffer[10] = {0};
uint8_t tempIndex  = 0;
uint8_t lightIndex = 0;
long tempSum  = 0;
long lightSum = 0;
```

`uint8_t` (1 byte) for indices instead of `int` (2 bytes).
Each index only needs to count to 9 — no reason for 2 bytes.

---

## Related Notes

- [[Arrays]]
- [[TMP36]]
- [[LDR]]
- [[Hysteresis]]
- [[Buffer Initialization Bug]]
- [[Data Types]]
