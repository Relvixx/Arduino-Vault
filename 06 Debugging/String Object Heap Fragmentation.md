# String Object Heap Fragmentation

**Tags:** #debugging #memory #SRAM #crash

---

## Symptom

- System runs correctly for 30 minutes to several hours
- Then randomly resets, freezes, or produces corrupt output
- Serial Monitor output looks normal right up until the crash
- No error message before or during the crash
- Restarting the Arduino fixes it temporarily

---

## Cause

The `String` class dynamically allocates memory from the **heap**
each time a String is created or modified.

When Strings are concatenated:

```cpp
String message = "Temp: " + String(temperature) + " C";
```

Arduino:
1. Allocates memory for "Temp: "
2. Allocates memory for String(temperature)
3. Allocates memory for the combined result
4. Frees the intermediate allocations

Over time, freed blocks leave small gaps in the 2KB SRAM heap.
New allocations cannot always fit into the gaps.
The heap and stack eventually collide.

This is **heap fragmentation** — and on 2KB of SRAM it causes
crashes after minutes to hours of runtime.

---

## Why It Is Hard to Diagnose

- The crash does not produce an error message
- The system works correctly during testing (short runtime)
- Only manifests in deployment (long runtime)
- Serial output looks normal until the instant of crash
- Restarting temporarily "fixes" it (clears fragmented heap)

---

## The Fix

Never use `String` objects in Arduino code.
Use character arrays (`char[]`) and `sprintf()` instead:

```cpp
// DANGEROUS — heap fragmentation
String message = "Temp: " + String(temp, 1) + " C";
Serial.println(message);

// SAFE — fixed memory on stack
char message[20];
sprintf(message, "Temp: %.1f C", temp);
Serial.println(message);
```

For Serial output, avoid String entirely:

```cpp
// SAFEST — no buffer needed
Serial.print(F("Temp: "));
Serial.print(temp, 1);
Serial.println(F(" C"));
```

---

## Prevention

- **Never** use `String` in any Arduino project
- Always use `char[]` for text buffers
- Use `F()` macro for all string literals in Serial
- When reviewing code, search for `String` as a warning sign

---

## Diagnosing SRAM Usage

Use the `freeMemory()` function from the MemoryFree library:

```cpp
#include <MemoryFree.h>
Serial.println(freeMemory());   // prints available SRAM
```

Print this periodically. If the value decreases over time,
you have a memory leak — likely from String objects.

---

## Related Notes

- [[Memory Types]]
- [[Data Types]]
- [[UART]]
- [[Software Debugging Process]]
- [[Modular Code Architecture]]
