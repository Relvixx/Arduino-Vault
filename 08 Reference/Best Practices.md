# Best Practices

**Tags:** #reference #engineering #habits

---

## The Non-Negotiables

These are not suggestions. Every project, every time.

---

### 1. Design Before Coding

Write plain English logic before touching the keyboard:
```
1. What variables does this program need to remember?
2. What is the trigger condition?
3. What happens on trigger?
4. What updates at the end of every loop?
```

The toggle LED and capstone FSM were both designed this way.
Projects designed first had zero structural bugs.
Projects coded directly had 3–4 bugs per attempt.

---

### 2. Use millis() Instead of delay()

```cpp
// NEVER in a multi-task project:
delay(1000);

// ALWAYS:
unsigned long previousTime = 0;
const unsigned long INTERVAL = 1000;

void loop() {
  unsigned long now = millis();
  if (now - previousTime >= INTERVAL) {
    previousTime = now;
    // timed action
  }
}
```

`delay()` freezes everything. `millis()` freezes nothing.

---

### 3. Always Use Edge Detection for Buttons

```cpp
// Wrong — fires thousands of times while held:
if (digitalRead(2) == LOW) { doThing(); }

// Correct — fires once per press:
if (currentState == LOW && prevState == HIGH) { doThing(); }
prevState = currentState;
```

---

### 4. Always Use Hysteresis on Thresholds

```cpp
// Wrong — chatters at 512:
if (value > 512) { activate(); }
else { deactivate(); }

// Correct — stable switching:
if (value > 530) { activate(); }
if (value < 490) { deactivate(); }
```

Two separate `if` statements. Never `if/else` for hysteresis.

---

### 5. Always Filter Sensor Readings

Raw sensor readings are noisy. Never use them directly
for threshold decisions in a deployed system.

```cpp
int getSmoothed(int newReading) {
  total -= buffer[idx];
  buffer[idx] = newReading;
  total += newReading;
  idx = (idx + 1) % NUM;
  return total / NUM;
}
```

Initialize the buffer in `setup()` with real readings.

---

### 6. Always Use map() and constrain() Together

```cpp
// map() alone:
int angle = map(raw, 0, 1023, 0, 180);
// Sensor noise can push raw to 1025 → angle = 181 → servo damage

// Correct pair:
int angle = constrain(map(raw, 0, 1023, 0, 180), 0, 180);
```

---

### 7. Use enum for State Machines

```cpp
// Wrong — unreadable:
int state = 2;
if (state == 2) { ... }

// Correct — self-documenting:
enum State { NORMAL, ALERT, ACKNOWLEDGED };
State current = NORMAL;
if (current == ALERT) { ... }
```

---

### 8. F() Macro on Every Serial String Literal

```cpp
// Wrong — consumes SRAM permanently:
Serial.println("Temperature reading");

// Correct — stored in Flash, zero SRAM cost:
Serial.println(F("Temperature reading"));
```

No downside. Make it automatic. No exceptions.

---

### 9. Use Minimum Type That Fits

```cpp
// Wasteful:
int pinNumber = 9;      // 2 bytes, never changes, never negative
long counter = 0;       // 4 bytes, only counts to 100

// Efficient:
const byte pinNumber = 9;   // 1 byte, const = Flash not SRAM
byte counter = 0;           // 1 byte, sufficient
unsigned long timer = 0;    // 4 bytes — required for millis()
```

---

### 10. Initialize Filter Buffers in setup()

```cpp
void setup() {
  for (int i = 0; i < NUM_SAMPLES; i++) {
    buffer[i] = analogRead(SENSOR_PIN);
    total += buffer[i];
  }
}
```

Without this: first N readings averaged with zeros → wrong values.
With this: filter is accurate from the very first loop iteration.

---

### 11. Modular Functions — One Responsibility Each

```cpp
// Wrong — does everything:
void loop() {
  // 50 lines of mixed sensor, LED, buzzer, serial code
}

// Correct — each function does one thing:
void loop() {
  float temp = readTemperature();
  updateLED(temp);
  printTelemetry(temp);
}
```

`loop()` should be under 10 lines and read like a table of contents.

---

### 12. All Timing Variables as unsigned long

```cpp
unsigned long previousTime = 0;    // CORRECT
unsigned long startTime    = 0;    // CORRECT
int previousTime           = 0;    // WRONG — overflows at 32s
```

---

### 13. Check Memory After Every Compile

```
Global variables use 312 bytes (15%)  → comfortable
Global variables use 1,700 bytes (83%) → investigate now
Global variables use 1,890 bytes (92%) → fix before proceeding
```

---

### 14. Hardware Debugging Order

```
1. Check power
2. Check GND (most common cause of total failure)
3. Check component orientation
4. Check breadboard connections
5. Check code logic
6. Isolate and test sections individually
```

Never skip to code first.

---

### 15. Never Use String Objects

```cpp
// WRONG — heap fragmentation → crash after hours:
String msg = "Temp: " + String(temp);

// CORRECT — fixed memory, no fragmentation:
Serial.print(F("Temp: "));
Serial.println(temp);
```

---

## Related Notes

- [[Mistakes and Corrections Log]]
- [[Hardware Debugging Process]]
- [[Software Debugging Process]]
- [[millis Non-Blocking Pattern]]
- [[State Machines]]
- [[Moving Average Filter]]
- [[Hysteresis]]
