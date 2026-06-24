# P04 — Toggle LED with Button

**Tags:** #project #button #LED #edge-detection #state

---

## Goal

Each button press toggles the LED state.
Press 1 → ON. Press 2 → OFF. Press 3 → ON.
No flickering while holding the button down.

---

## Components

Same as [[P03 Button Controlled LED]].

---

## Wiring Logic

Same as [[P03 Button Controlled LED]].

---

## Design (Plain English First)

```
Variables needed:
  - Current LED state (ON/OFF)
  - Previous button state (pressed/unpressed)

Trigger condition:
  - Button JUST became pressed
    (current state = LOW AND previous state = HIGH)

After toggle:
  - Flip LED state variable
  - Update LED output
  - Save current button state as new previous state
```

---

## Code

```cpp
const int buttonPin = 2;
const int ledPin    = 13;

int ledState        = LOW;
int prevButtonState = HIGH;   // HIGH = unpressed with INPUT_PULLUP

void setup() {
  pinMode(ledPin, OUTPUT);
  pinMode(buttonPin, INPUT_PULLUP);
}

void loop() {
  int currentButtonState = digitalRead(buttonPin);

  // Falling edge: was HIGH, now LOW → button just pressed
  if (currentButtonState == LOW && prevButtonState == HIGH) {
    ledState = !ledState;              // flip
    digitalWrite(ledPin, ledState);   // apply
  }

  prevButtonState = currentButtonState;   // always update at end
}
```

---

## What Was Learned

- Edge detection fires once per press, not continuously
- `prevButtonState` is a **global variable** — lives between loop iterations
- This pattern is called **falling edge detection**
- `!ledState` flips boolean — equivalent to
  `ledState = (ledState == HIGH) ? LOW : HIGH`
- Logic must be designed in plain English before coding
- This project introduced the habit of planning before typing

---

## The Design-First Moment

This was the first project where plain English logic was
required before code was accepted. The habit established here
carried through to the capstone.

---

## Real Hardware Note

On physical buttons, contacts bounce for ~5–20ms.
This causes multiple false falling edges from one press.
Fix: add a `delay(20)` after detection, or use
millis()-based debounce. See [[Edge Detection]].

---

## Related Notes

- [[Edge Detection]]
- [[Push Button]]
- [[Variable Scope and Static]]
- [[Floating Pin Bug]]
- [[P03 Button Controlled LED]]
