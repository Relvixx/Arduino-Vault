# map() Float Argument Bug

**Tags:** #debugging #types #map #float

---

## Symptom

- Servo or LED brightness changes in coarse steps instead of smoothly
- Output jumps between values instead of tracking sensor smoothly
- Decimal precision from sensor reading is lost
- At 25.7°C and 25.3°C, servo moves to the same angle

---

## Cause

`map()` takes `long` integer arguments, not `float`:

```cpp
long map(long x, long fromLow, long fromHigh, long toLow, long toHigh);
```

When a `float` is passed as the first argument, it is
**implicitly truncated** to `long` before the calculation:

```cpp
float temp = 25.7;
int angle = map(temp, 0, 50, 0, 180);
// temp is silently truncated to 25 before map() sees it
// 25.7°C and 25.1°C both produce the same angle as 25°C
```

This is not a compiler error. It compiles cleanly.
The truncation happens silently at runtime.

---

## The Fix

Make the truncation **explicit** with a cast:

```cpp
float temp = 25.7;
int angle = map((int)temp, 0, 50, 0, 180);
// The (int) cast shows that truncation is intentional
// 25.7 → 25 → mapped to correct angle for 25°C
```

The decimal precision is still lost — but now it is your
deliberate design choice, visible in the code.

For applications where decimal precision matters in the output:
calculate manually instead of using `map()`:

```cpp
// Manual mapping preserving float precision
float angle = (temp - 0) * (180.0 - 0) / (50.0 - 0) + 0;
int safeAngle = constrain((int)angle, 0, 180);
```

---

## Prevention

Rule: When calling `map()` with a float input, always cast
explicitly with `(int)` to document that truncation is intended.

When reviewing code: any `map()` call with a variable that
might be float deserves a second look.

---

## Related Notes

- [[map and constrain]]
- [[Data Types]]
- [[Servo Motor]]
- [[TMP36]]
- [[P11 TMP36 Servo LED]]
