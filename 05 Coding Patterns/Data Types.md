# Data Types

**Tags:** #coding #types #memory

---

## Arduino Data Types Reference

| Type | Bytes | Range | Use when |
|---|---|---|---|
| `bool` | 1 | true / false | flags, states |
| `char` | 1 | -128 to 127 | single characters |
| `byte` | 1 | 0 to 255 | small positive values |
| `int` | 2 | -32,768 to 32,767 | general counters |
| `unsigned int` | 2 | 0 to 65,535 | positive-only counts |
| `long` | 4 | -2.1B to 2.1B | large numbers |
| `unsigned long` | 4 | 0 to 4,294,967,295 | millis(), micros() |
| `float` | 4 | ±3.4×10³⁸ (6-7 sig fig) | sensor math, decimals |
| `double` | 4 | same as float on Uno | no benefit — use float |
| `String` | varies | heap-allocated | **AVOID** |
| `char[]` | varies | stack/global | use instead of String |

---

## Critical Rules

### int overflow on Arduino

`int` on the Uno is 16-bit. Maximum value: **32,767**.

```cpp
int x = 50000;   // WRONG — overflow, wraps to negative
long x = 50000;  // CORRECT
```

This is different from most modern languages where `int` is 32-bit.

### millis() MUST be unsigned long

```cpp
unsigned long previousTime = 0;   // CORRECT
int previousTime = 0;             // WRONG — overflows at ~32 seconds
```

After 32,767ms (~32 seconds), an `int` overflows to a large negative
value. The subtraction `millis() - previousTime` produces wrong results.
Timing appears to work then randomly breaks.

See [[millis Type Bug]].

### Integer division truncates

```cpp
int result = 7 / 2;       // result = 3 (not 3.5)
float result = 7.0 / 2;   // result = 3.5 — one float forces float math
float v = raw * (5.0 / 1023.0);   // CORRECT — 5.0 forces float division
float v = raw * (5 / 1023);       // WRONG — integer division = 0
```

### String object — avoid

```cpp
// DANGEROUS — heap fragmentation over time
String msg = "Temp: " + String(temp) + "C";

// SAFE — fixed memory, no fragmentation
char msg[20];
sprintf(msg, "Temp: %.1f C", temp);
Serial.println(msg);
```

The `String` class dynamically allocates heap memory.
Repeated concatenation fragments the 2KB SRAM over time.
The system crashes after minutes or hours — no error message.

See [[String Object Heap Fragmentation]].

---

## Memory-Conscious Choices

```cpp
// Wasteful
int sensorPin = A0;    // 2 bytes, never negative, never changes
long counter = 0;      // 4 bytes, only counts to 100

// Optimized
const byte sensorPin = A0;   // 1 byte, stored in Flash (const)
byte counter = 0;            // 1 byte, sufficient range
```

---

## Related Notes

- [[Memory Types]]
- [[millis Non-Blocking Pattern]]
- [[millis Type Bug]]
- [[String Object Heap Fragmentation]]
- [[Arrays]]
- [[Variable Scope and Static]]
