# Edge Detection

**Tags:** #coding #pattern #button #input

---

## What It Is

A pattern that detects the **moment** a signal changes state —
not whether it is currently in a state.
Signal:  |‾‾‾‾‾‾‾‾‾|

↑         ↑

RISING edge   FALLING edge

(LOW→HIGH)    (HIGH→LOW)

---

## Why It Matters

Without edge detection:

```cpp
if (digitalRead(2) == LOW) {
  toggleLED();   // fires THOUSANDS of times per second
}                // while button is held
```

With edge detection:

```cpp
if (currentState == LOW && prevState == HIGH) {
  toggleLED();   // fires ONCE — at the moment of press
}
```

---

## Full Implementation

```cpp
int prevButtonState = HIGH;   // HIGH = unpressed with INPUT_PULLUP

void loop() {
  int currentButtonState = digitalRead(buttonPin);

  // Falling edge: was HIGH, now LOW → button just pressed
  if (currentButtonState == LOW && prevButtonState == HIGH) {
    handlePress();    // fire exactly once
  }

  prevButtonState = currentButtonState;   // always update at end
}
```

**Critical:** `prevButtonState = currentButtonState` must be at
the END of loop, after all checks. Updating it inside the `if`
block breaks the pattern.

---

## Toggle LED with Edge Detection

```cpp
bool ledState = false;
int prevButtonState = HIGH;

void loop() {
  int currentButtonState = digitalRead(2);

  if (currentButtonState == LOW && prevButtonState == HIGH) {
    ledState = !ledState;                  // flip state
    digitalWrite(13, ledState);
  }

  prevButtonState = currentButtonState;
}
```

First press: ON. Second press: OFF. Third press: ON.
No flicker. No multiple triggers.

---

## In State Machines

Edge detection integrates naturally with state machines:

```cpp
case ALERT_ACTIVE:
  if (currentBtn == LOW && prevBtn == HIGH) {
    currentState = ALERT_ACKNOWLEDGED;
    ackFlag = true;
  }
  break;
```

The button transition triggers a state transition — once per press.

---

## Debouncing

On real hardware, mechanical contacts bounce for ~5–20ms on press.
This creates multiple false falling edges from one physical press.

Simple software debounce:
```cpp
if (currentButtonState == LOW && prevButtonState == HIGH) {
  delay(20);    // wait for bounce to settle
  if (digitalRead(buttonPin) == LOW) {   // confirm still pressed
    handlePress();
  }
}
```

Better approach (no delay): timing-based debounce using `millis()`.
Record the time of first edge. Only accept the press if it is still
LOW after 20ms.

---

## Related Notes

- [[Push Button]]
- [[State Machines]]
- [[millis Non-Blocking Pattern]]
- [[Floating Pin Bug]]
- [[P04 Toggle LED]]
- [[P12 Smart Environment Monitor]]
