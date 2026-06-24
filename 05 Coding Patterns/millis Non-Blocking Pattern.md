# millis() Non-Blocking Pattern

**Tags:** #coding #timing #millis #pattern

---

## The Problem with delay()

`delay()` freezes EVERYTHING:

```cpp
void loop() {
  digitalWrite(13, HIGH);
  delay(1000);              // Arduino frozen — cannot read sensors,
                            // check buttons, or do anything for 1 second
  digitalWrite(13, LOW);
  delay(1000);              // frozen again
}
```

Any project needing to do more than one thing simultaneously
cannot use `delay()`.

---

## The millis() Solution

`millis()` returns milliseconds since Arduino powered on.
Instead of waiting, you **check how much time has passed:**

```cpp
unsigned long previousTime = 0;         // when did we last act?
const unsigned long INTERVAL = 1000;    // how long to wait

void loop() {
  unsigned long currentTime = millis();

  if (currentTime - previousTime >= INTERVAL) {
    previousTime = currentTime;          // reset the timer
    // do the timed action here
    digitalWrite(13, !digitalRead(13)); // toggle LED
  }

  // Everything else runs freely — no blocking
  checkButton();
  readSensor();
}
```

---

## Why currentTime - previousTime Works Through Overflow

`millis()` uses `unsigned long` — overflows to 0 after ~49 days.
Before overflow:  currentTime = 4,294,967,290

After overflow:   currentTime = 5  (wrapped around)
currentTime - previousTime = 5 - 4,294,967,290

In unsigned arithmetic: = 11 (correct!)

Unsigned integer overflow wraps around safely.
The subtraction always gives the correct elapsed time.
**This is why previousTime must be `unsigned long`, not `int`.**

---

## Multiple Simultaneous Timers

Each timed action needs its own `previousTime` variable:

```cpp
unsigned long lastBlink  = 0;
unsigned long lastPrint  = 0;
unsigned long lastSample = 0;

void loop() {
  unsigned long now = millis();

  if (now - lastBlink >= 500) {
    lastBlink = now;
    // toggle LED
  }

  if (now - lastPrint >= 2000) {
    lastPrint = now;
    // print telemetry
  }

  if (now - lastSample >= 100) {
    lastSample = now;
    // sample sensor
  }
}
```

Three independent timers. Zero blocking. All run simultaneously.

---

## Store millis() Once Per Loop

```cpp
// WRONG — multiple calls may return different values
if (millis() - lastBlink >= 500) {
  lastBlink = millis();    // slightly different time than above
}

// CORRECT — consistent snapshot
unsigned long now = millis();
if (now - lastBlink >= 500) {
  lastBlink = now;         // same value used everywhere
}
```

---

## Using static for Internal Timers

When a function manages its own timing:

```cpp
void printTelemetry(float temp) {
  static unsigned long lastPrint = 0;
  if (millis() - lastPrint >= 2000) {
    lastPrint = millis();
    Serial.println(temp);
  }
}
```

No global variable needed. Timer is private to the function.

---

## Related Notes

- [[millis Type Bug]]
- [[State Machines]]
- [[Edge Detection]]
- [[Variable Scope and Static]]
- [[P08 Non-Blocking Blink and Button]]
- [[P09 Traffic Light]]
- [[P10 Pedestrian Crossing]]
