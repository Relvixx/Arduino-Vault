# Memory Types

**Tags:** #fundamentals #memory #architecture

---

## The Three Types

| Memory | Size | Analogy | Purpose | Survives power off? |
|---|---|---|---|---|
| **Flash** | 32KB | Recipe book on shelf | Stores your compiled program | ✅ Yes |
| **SRAM** | 2KB | Kitchen counter | Variables in use right now | ❌ No |
| **EEPROM** | 1KB | Sticky note on fridge | Small persistent data | ✅ Yes |

---

## What Lives Where During Execution

### Flash (read-only during execution)
- Bootloader (~2KB, protected)
- Your compiled sketch
- String literals stored with `F()` macro
- `const` variables

### SRAM (read/write during execution)
- Global variables
- Static variables
- Stack (function calls, local variables)
- Heap (dynamic allocation — avoid on Arduino)

### EEPROM
- Only what you explicitly write with `EEPROM.write()`
- Write limit: **~100,000 cycles per address**
- Never write every loop — only write on change

---

## The SRAM Warning

2KB fills faster than you expect:

```cpp
char message[] = "Temperature reading: ";  // 22 bytes
int readings[50];                           // 100 bytes
float calibration[10];                      // 40 bytes
// Already 162 bytes — 8% of total SRAM
```

When SRAM runs out, Arduino **does not crash cleanly**.
It produces: random resets, variables changing by themselves,
code skipping sections. No error message. Very hard to diagnose.

---

## The F() Macro Fix

```cpp
Serial.println("Temperature reading");     // 22 bytes of SRAM consumed
Serial.println(F("Temperature reading")); // 0 bytes of SRAM — stored in Flash
```

Use `F()` on **every** string literal in `Serial.print()`.
No downside. Make it automatic.

---

## Checking Memory After Compile

The IDE shows:
Sketch uses 1,234 bytes (3%) of program storage space.

Global variables use 212 bytes (10%) of dynamic memory.

Leaving 1,836 bytes for local variables.

Rule of thumb:
- Below 70% SRAM → comfortable
- Above 83% SRAM → investigate
- Above 92% SRAM → fix before continuing

---

## Related Notes

- [[What is Arduino]]
- [[Microcontroller vs Computer]]
- [[Data Types]]
- [[String Object Heap Fragmentation]]
- [[Modular Code Architecture]]
