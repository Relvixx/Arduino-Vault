# millis() Type Bug

**Tags:** #debugging #timing #millis #types

---

## Symptom

- Timing code works correctly for the first ~32 seconds
- After ~32 seconds, timing suddenly breaks — events trigger
  at wrong intervals or stop triggering entirely
- `millis()` subtraction produces a large negative number
- System resets or behaves erratically after 32 seconds of runtime

---

## Cause

`millis()` returns `unsigned long` — range: 0 to 4,294,967,295.

If `previousTime` is declared as `int` (range: -32,768 to 32,767):

```cpp
int previousTime = 0;    // WRONG type

void loop() {
  if (millis() - previousTime >= 1000) {
    previousTime = millis();   // millis() returns unsigned long
                               // stored into int → overflow!
  }
}
```

When `millis()` exceeds 32,767 (after ~32 seconds):
- The value is truncated when stored into `int`
- `int` overflows and wraps to a large negative number
- `millis() - previousTime` = large positive minus large negative
  = enormous number >> 1000
- Condition fires continuously or calculation breaks

---

## The Fix

```cpp
unsigned long previousTime = 0;   // CORRECT — matches millis() return type

void loop() {
  unsigned long currentTime = millis();
  if (currentTime - previousTime >= 1000) {
    previousTime = currentTime;
  }
}
```

**All timing variables must be `unsigned long`.**
This is not optional. It is the only correct type.

---

## Why unsigned long Handles Overflow Safely

After ~49 days, `millis()` overflows back to 0.
With `unsigned long` arithmetic:

```
currentTime  = 5          (just overflowed)
previousTime = 4,294,967,290  (just before overflow)

currentTime - previousTime
= 5 - 4,294,967,290
= 11 (in unsigned arithmetic — wraps correctly)
```

The subtraction gives the correct elapsed time even through overflow.
**This only works when both variables are `unsigned long`.**
With `int`, signed overflow is undefined behavior in C++.

---

## Compiler Warning

This bug produces a compiler warning:
```
warning: comparison between signed and unsigned integer expressions
```

Take this warning seriously. It almost always indicates
this exact bug.

---

## Prevention

Rule: **Never store the return value of `millis()` in anything
other than `unsigned long`.**

```cpp
const unsigned long INTERVAL = 500;   // also unsigned long
unsigned long previousTime = 0;
unsigned long currentTime = millis();
```

---

## Related Notes

- [[millis Non-Blocking Pattern]]
- [[Data Types]]
- [[Software Debugging Process]]
- [[The Clock]]
