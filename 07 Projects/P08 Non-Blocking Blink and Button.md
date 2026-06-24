# P08 — Non-Blocking Blink and Button

**Tags:** #project #millis #non-blocking #button #LED #simultaneous

---

## Goal

Two independent behaviors running simultaneously:
1. LED on pin 13 blinks every 500ms using `millis()` — never pauses
2. Button on pin 2 toggles a second LED on pin 12 independently

Both behaviors must work at the same time.
Button press must not affect blink timing.
**No `delay()` anywhere.**

This project is impossible with `delay()`. It is straightforward
with `millis()`.

---

## Components

| Component | Quantity | Notes |
|---|---|---|
| Arduino Uno | 1 | |
| Breadboard | 1 | |
| LED | 2 | different colours helps |
| Resistor | 2 | 220Ω each |
| Push button | 1 | |
| Jumper wires | 7 | |

---

## Wiring Logic

```
Blink LED:
  Pin 13 → 220Ω → LED anode → cathode → GND

Toggle LED:
  Pin 12 → 220Ω → LED anode → cathode → GND

Button:
  Pin 2 → button terminal
  Other terminal → GND
  (INPUT_PULLUP used — no external resistor)
```

---

## Design (Plain English)

```
Variables:
  - previousTime: when did LED last toggle?
  - toggleState: is the toggle LED currently on or off?
  - prevButtonState: was button pressed last loop?

Blink logic (every loop):
  - Get currentTime from millis()
  - If currentTime - previousTime >= 500:
      toggle blink LED, update previousTime

Button logic (every loop):
  - Read button
  - If just pressed (falling edge):
      flip toggleState, update toggle LED
  - Save button state as previous
```

---

## Code (Final Corrected Version)

```cpp
unsigned long previousTime  = 0;
const unsigned long INTERVAL = 500;

const int blinkLED   = 13;
const int toggleLED  = 12;
const int buttonPin  = 2;

bool toggleState     = false;
int prevButtonState  = HIGH;

void setup() {
  pinMode(blinkLED,  OUTPUT);
  pinMode(toggleLED, OUTPUT);
  pinMode(buttonPin, INPUT_PULLUP);
}

void loop() {
  unsigned long currentTime = millis();

  // Non-blocking blink
  if (currentTime - previousTime >= INTERVAL) {
    previousTime = currentTime;
    digitalWrite(blinkLED, !digitalRead(blinkLED));
  }

  // Edge-detected button toggle
  int currentButtonState = digitalRead(buttonPin);
  if (currentButtonState == LOW && prevButtonState == HIGH) {
    toggleState = !toggleState;
    digitalWrite(toggleLED, toggleState);
  }
  prevButtonState = currentButtonState;
}
```

---

## Key Correction During Course

**First attempt used `delay(200)` for button debounce.**
This reintroduced blocking and caused blink stutter on press.
Fix: proper edge detection eliminates the need for `delay()`
entirely. Edge detection fires once — no debounce delay needed
in Tinkercad simulation.

---

## What Was Learned

- `millis()` pattern allows multiple simultaneous timed behaviors
- Each behavior needs its own `previousTime` variable
- `const unsigned long` for interval — not `const int`
- `millis()` stored once per loop (`currentTime`) — not called twice
- Edge detection and non-blocking timing work together naturally
- `delay()` and multi-tasking are fundamentally incompatible

---

## Related Notes

- [[millis Non-Blocking Pattern]]
- [[Edge Detection]]
- [[millis Type Bug]]
- [[Push Button]]
- [[P09 Traffic Light]]
