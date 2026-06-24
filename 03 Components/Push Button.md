# Push Button

**Tags:** #component #input #digital

---

## What It Is

A **momentary switch** — connects two terminals only while pressed.
Release it → connection breaks immediately.
NOT pressed:           Pressed:

A ──┤  ├── B          A ──────── B

(open circuit)         (closed circuit)

---

## The Floating Pin Problem

When the button is not pressed, the Arduino pin is connected to nothing.
"Nothing" in electronics is not 0V — it is an **undefined floating state**.

The pin acts as an antenna, picking up electromagnetic noise.
It randomly reads HIGH or LOW with no pattern.

**Result:** Button appears to trigger randomly, even when not touched.

This is one of the most common and confusing beginner bugs.

---

## The Fix: Pull-up or Pull-down Resistor

### Pull-down (unpressed = LOW)
5V ──[ Button ]──┬── Arduino pin

│

[ 10kΩ ]

│

GND
Unpressed: pin connected to GND through resistor → reads LOW

Pressed:   5V overrides resistor → reads HIGH

### Pull-up (unpressed = HIGH)
5V ──[ 10kΩ ]──┬── Arduino pin

│

[ Button ]

│

GND
Unpressed: pin connected to 5V through resistor → reads HIGH

Pressed:   GND pulls pin down → reads LOW

---

## INPUT_PULLUP — The Easy Solution

Arduino Uno has built-in pull-up resistors (~20–50kΩ) on every
digital pin. Activate with one word — no external resistor needed:

```cpp
pinMode(2, INPUT_PULLUP);
```

**Logic is inverted:**
- Unpressed = HIGH
- Pressed = LOW

This catches almost every beginner who doesn't read the docs.

```cpp
// Correct reading with INPUT_PULLUP:
if (digitalRead(2) == LOW) {
  // button IS pressed
}
```

---

## Edge Detection

Without edge detection, holding the button fires the action
every single loop iteration — thousands of times per second.

**Edge detection fires once per press:**
```cpp
int prevButtonState = HIGH;  // HIGH = unpressed with INPUT_PULLUP

void loop() {
  int currentButtonState = digitalRead(2);
  
  if (currentButtonState == LOW && prevButtonState == HIGH) {
    // Button JUST became pressed — fire once
    doSomething();
  }
  
  prevButtonState = currentButtonState;
}
```

This pattern is called a **falling edge detection**.
It detects the moment the signal transitions from HIGH to LOW.

See [[Edge Detection]] for the full pattern.

---

## Debouncing

On real hardware, mechanical button contacts bounce rapidly for
a few milliseconds on press. This causes multiple false falling
edges from one physical press.

In Tinkercad: not visible (simulated perfect button).
On real hardware: add a small delay after detection, or use
a timing-based software debounce. See [[Edge Detection]].

---

## Common Mistakes

| Mistake | Result |
|---|---|
| `INPUT` without pull resistor | Floating pin — random reads |
| Forgetting inverted logic with `INPUT_PULLUP` | Everything backwards |
| No edge detection | Fires thousands of times per press |
| Connecting to pins 0 or 1 | Interferes with upload |

---

## Related Notes

- [[Floating Pin Bug]]
- [[Edge Detection]]
- [[Digital Pins]]
- [[Arduino Uno Anatomy]]
- [[P03 Button Controlled LED]]
- [[P04 Toggle LED]]
