# Software Debugging Process

**Tags:** #debugging #software #process

---

## When to Use This

Use after [[Hardware Debugging Process]] has confirmed:
- Power is correct
- GND is connected
- Components are oriented correctly
- Wiring connections are verified

If hardware is confirmed good and behavior is still wrong,
the bug is in the code.

---

## Step 1 — SERIAL PRINT THE SUSPECT

Never guess what a variable contains. Print it.

```cpp
// You think sensorValue is around 500
// It is actually 23 — that is why the threshold is never crossed
Serial.print(F("sensorValue: "));
Serial.println(sensorValue);
```

Confirm actual values before assuming logic is wrong.

---

## Step 2 — BINARY SEARCH THE BUG

Comment out half the code. Does the problem persist?

```
Problem persists with half removed → bug is in remaining half
Problem disappears with half removed → bug was in removed half
```

Repeat: comment out half of the suspect section.
Continue until the bug is isolated to a few lines.

---

## Step 3 — CHECK TYPES FIRST

Most silent bugs in Arduino are type issues:

```cpp
// int overflow — millis() value wraps at 32,767ms
int prevTime = 0;                // WRONG
unsigned long prevTime = 0;      // CORRECT

// Integer division — produces 0
float v = raw * (5 / 1023);     // WRONG — 5/1023 = 0
float v = raw * (5.0 / 1023.0); // CORRECT

// map() with float argument — silent truncation
map(floatTemp, 0, 50, 0, 180);          // WRONG
map((int)floatTemp, 0, 50, 0, 180);     // CORRECT — explicit
```

Before complex debugging: verify every variable's type
matches its intended use.

---

## Step 4 — REMOVE delay() SUSPECTS

If behavior is intermittent or timing-related:

```
Remove or minimize all delay() calls.
Replace with millis() pattern.
The bug may be a blocking call preventing a sensor
read or state check at the critical moment.
```

See [[millis Non-Blocking Pattern]].

---

## Step 5 — TEST FUNCTIONS IN ISOLATION

Write a minimal sketch that calls only the suspected function
with known, fixed input values:

```cpp
void setup() {
  Serial.begin(9600);
  // Test getAverage() with known values
  int testData[5] = {10, 20, 30, 40, 50};
  Serial.println(getAverage(testData, 5));  // should print 30
}
void loop() {}
```

Does it return the expected value?
If yes: the function is correct — check what you are passing to it.
If no: the bug is inside the function.

---

## Step 6 — CHECK STATE MACHINE PRIORITY

If a state machine transitions to the wrong state:

```
List all transitions out of the current state.
Trace which condition fires first in the code.
Check if the order matches the intended priority.
```

The first matching condition always wins in sequential code.

See [[State Transition Priority Bug]].

---

## Common Error Messages Decoded

```
'x' was not declared in this scope
→ x declared inside a function, used outside it
→ Fix: move declaration to global scope or pass as parameter

expected ';' before '}'
→ Missing semicolon on the line ABOVE the error location
→ Check the line before where the error points

invalid conversion from 'int' to 'char*'
→ Type mismatch in function argument
→ Fix: match the expected type

comparison between signed and unsigned
→ Comparing int with unsigned long (common with millis())
→ Fix: make both sides unsigned long
```

---

## Related Notes

- [[Hardware Debugging Process]]
- [[millis Type Bug]]
- [[State Transition Priority Bug]]
- [[map Float Argument Bug]]
- [[Buffer Initialization Bug]]
- [[String Object Heap Fragmentation]]
