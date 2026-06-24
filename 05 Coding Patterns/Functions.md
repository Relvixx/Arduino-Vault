# Functions

**Tags:** #coding #functions #modular

---

## Why Write Functions

Write once, use many times — no duplicated code
Give logic a name — code becomes self-documenting
Isolate bugs — test one function at a time
Constrain scope — local variables don't pollute globals
Enable independent modification — change one function

without touching others


---

## Function Anatomy

```cpp
// Return type  Name       Parameters
float          getTemp    (int pin, float offset) {
  int raw = analogRead(pin);
  float v = raw * (5.0 / 1023.0);
  return (v - 0.5) * 100.0 + offset;  // return statement
}
```

If function returns nothing: use `void`
```cpp
void updateLED(bool state) {
  digitalWrite(13, state ? HIGH : LOW);
  // no return statement needed
}
```

---

## Single Responsibility Rule

**Each function does exactly one thing.**

If you cannot describe what a function does in ONE sentence
without using "and" — split it into two functions.

```cpp
// BAD — does 3 things
void readSensorAndUpdateDisplayAndCheckThreshold() { ... }

// GOOD — each does one thing
float readSensor();
void updateDisplay(float value);
bool checkThreshold(float value);
```

---

## The Mature loop() Pattern

When your functions each do one thing, `loop()` becomes a
readable summary of the system:

```cpp
void loop() {
  float temp  = readTemperature();
  int light   = readLight();
  checkButton();
  updateState(temp, light);
  updateLEDs(currentState);
  updateBuzzer(currentState);
  printTelemetry(temp, light, currentState);
}
```

Seven lines. Anyone reading this understands the system immediately.

---

## Returning Values

```cpp
// Returns int
int getMax(int arr[], int size) {
  int maxVal = arr[0];            // initialize from actual data
  for (int i = 1; i < size; i++) { // start at 1 — arr[0] already stored
    if (arr[i] > maxVal) maxVal = arr[i];
  }
  return maxVal;
}
```

**Initialize `maxVal` from `arr[0]`, not from 0.**
If all values are negative, initializing from 0 would produce
a wrong result — 0 would be returned as "maximum."

---

## Related Notes

- [[Modular Code Architecture]]
- [[Variable Scope and Static]]
- [[Arrays]]
- [[Arduino Code Structure]]
