# P02 — Challenge Blink Pattern

**Tags:** #project #LED #timing #loop

---

## Goal

Modify the basic blink so the LED blinks 3 times fast (200ms),
then pauses for 2 seconds, then repeats forever.
No copy-pasting from internet — reason it out from scratch.

---

## Components

Same as [[P01 Basic LED Blink]].

---

## Wiring Logic

Same as [[P01 Basic LED Blink]].

---

## Code

```cpp
void setup() {
  pinMode(13, OUTPUT);
}

void loop() {
  // Blink 3 times fast
  for (int i = 0; i < 3; i++) {
    digitalWrite(13, HIGH);
    delay(200);
    digitalWrite(13, LOW);
    delay(200);
  }
  // Pause completely off for 2 seconds
  delay(2000);
}
```

---

## Key Corrections Made During Course

**First attempt (wrong):**
```cpp
void loop() {
  digitalWrite(13, HIGH);
  delay(200);
  digitalWrite(13, LOW);
  // no gap delay
  delay(2000);   // only one blink, not three
}
```

Problems identified:
1. Only one blink — the HIGH/LOW block ran once, not three times
2. Missing OFF delay inside the blink cycle
3. Redundant `digitalWrite(LOW)` before the pause

**Fix:** `for` loop for repetition — pulled from Python knowledge
and applied correctly before loops were formally taught.

---

## What Was Learned

- A complete blink = HIGH + delay + LOW + delay (both halves)
- `for` loop eliminates code repetition
- Redundant lines should be removed — if LED is already LOW,
  writing LOW again does nothing and clutters code
- The OFF time in a blink is just as important as the ON time

---

## Related Notes

- [[P01 Basic LED Blink]]
- [[Digital Pins]]
- [[Arduino Code Structure]]
