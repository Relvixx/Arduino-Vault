# Arduino Code Structure

**Tags:** #coding #structure #fundamentals

---

## The Two Mandatory Functions

Every Arduino sketch requires exactly these two functions:

```cpp
void setup() {
  // Runs ONCE when Arduino powers on or resets
  // Use for: pinMode, Serial.begin, sensor initialization,
  //           buffer pre-filling, initial states
}

void loop() {
  // Runs FOREVER in a continuous cycle
  // Use for: reading sensors, checking conditions,
  //           updating outputs, timing checks
}
```

No `main()`. No `import`. No OS. Direct execution.

---

## The Execution Flow
Power on / Reset

│

▼

setup()    ← runs once

│

▼

loop()     ← runs

│

▼

loop()     ← runs again

│

▼

loop()     ← forever

---

## Forward Declarations

In Arduino C++, functions must be declared before they are called —
or a forward declaration placed before `setup()`:

```cpp
// Forward declarations
void checkButton();
float readTemperature();
void updateState(float temp, int light);

void setup() { ... }
void loop() { ... }

// Actual function definitions below
float readTemperature() { ... }
```

This is required when functions call each other in complex orders.
Tinkercad often handles this automatically — the real IDE is stricter.

---

## setup() Best Practices

```cpp
void setup() {
  Serial.begin(9600);               // always first if using Serial

  // Pin modes
  pinMode(13, OUTPUT);
  pinMode(2, INPUT_PULLUP);

  // Initial output states
  digitalWrite(13, LOW);            // explicit starting state

  // Sensor buffer pre-filling
  for (int i = 0; i < 10; i++) {
    buffer[i] = analogRead(A0);     // fill with real readings
    total += buffer[i];             // avoid zero-average startup bug
  }
}
```

---

## What Belongs in loop()

A mature `loop()` function should read like a table of contents:

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

Seven lines. Every function name describes what it does.
No logic in `loop()` itself — logic lives in the functions.

---

## Related Notes

- [[How Code Runs]]
- [[Modular Code Architecture]]
- [[millis Non-Blocking Pattern]]
- [[Buffer Initialization Bug]]
- [[Variable Scope and Static]]
