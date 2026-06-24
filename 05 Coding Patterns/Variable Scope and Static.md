# Variable Scope and Static

**Tags:** #coding #scope #variables #memory

---

## Three Scope Levels

```cpp
int globalVar = 0;        // GLOBAL
                          // → Lives entire program duration
                          // → Visible to ALL functions
                          // → Stored in SRAM always

void setup() {
  int localVar = 10;      // LOCAL
                          // → Lives until } closes this block
                          // → Visible ONLY inside setup()
                          // → Gone when setup() ends
}

void loop() {
  static int persist = 0; // STATIC LOCAL
                          // → Lives entire program duration
                          // → Visible ONLY inside loop()
                          // → Retains value between calls
                          // → Initialized ONCE only
  persist++;
}
```

---

## Static Local — The Important One

`static` gives a variable **local scope but global lifetime.**

```cpp
void countPresses() {
  static int count = 0;    // initialized ONCE at program start
  count++;                 // retains value across ALL calls
  Serial.println(count);   // prints 1, 2, 3, 4...
}
```

Without `static`:
```cpp
void countPresses() {
  int count = 0;           // reset to 0 EVERY call
  count++;                 // always 1
  Serial.println(count);   // always prints 1
}
```

---

## When to Use Static vs Global

| Use static when | Use global when |
|---|---|
| State belongs to one function only | Multiple functions share the state |
| Want to hide implementation details | State is part of the system architecture |
| Avoiding polluting global namespace | Value must survive function scope |

**Principle:** use the most restricted scope that satisfies the need.
Global variables accessible everywhere are a maintenance risk in
large projects — any function can accidentally modify them.

---

## Practical Example — Telemetry Timer

```cpp
void printTelemetry(float temp, int angle) {
  static unsigned long lastPrint = 0;   // private to this function
  if (millis() - lastPrint >= 500) {
    lastPrint = millis();
    Serial.print(F("Temp: "));
    Serial.println(temp);
  }
}
```

`lastPrint` is invisible outside `printTelemetry()`.
It persists between calls. No global variable needed.
This is cleaner than making `lastPrint` global.

---

## The scope error you will see

```cpp
void setup() {
  int x = 10;    // x declared inside setup()
}

void loop() {
  Serial.println(x);   // ERROR: 'x' was not declared in this scope
}
```

`x` dies when `setup()` ends. `loop()` cannot see it.
Fix: declare `x` globally, or pass it as a parameter.

---

## Related Notes

- [[Data Types]]
- [[Memory Types]]
- [[Modular Code Architecture]]
- [[Arduino Code Structure]]
