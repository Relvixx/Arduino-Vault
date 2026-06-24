# P03 — Button Controlled LED

**Tags:** #project #button #LED #input #digital

---

## Goal

Press a button → LED turns on.
Release the button → LED turns off.

---

## Components

| Component | Quantity | Notes |
|---|---|---|
| Arduino Uno | 1 | |
| Breadboard | 1 | |
| LED | 1 | |
| Resistor | 1 | 220Ω for LED |
| Push button | 1 | |
| Jumper wires | 4 | |

---

## Wiring Logic

```
LED circuit:
  Pin 13 → 220Ω → LED anode
  LED cathode → GND

Button circuit (INPUT_PULLUP — no external resistor needed):
  Pin 2 → one terminal of button
  Other terminal of button → GND
```

---

## Code

```cpp
void setup() {
  pinMode(13, OUTPUT);
  pinMode(2, INPUT_PULLUP);   // built-in pull-up, no resistor needed
}

void loop() {
  int buttonState = digitalRead(2);

  if (buttonState == LOW) {        // LOW = pressed (INPUT_PULLUP inverts)
    digitalWrite(13, HIGH);        // LED on
  } else {
    digitalWrite(13, LOW);         // LED off
  }
}
```

---

## What Was Learned

- `INPUT_PULLUP` eliminates the need for an external resistor
- With `INPUT_PULLUP`: unpressed = HIGH, pressed = LOW (inverted)
- `digitalRead()` returns HIGH or LOW
- Forgetting to account for inverted logic is a top beginner mistake
- Floating pins produce random reads — `INPUT_PULLUP` is the fix

---

## What Could Be Improved

- Holding button keeps LED on every loop — no edge detection
- If LED should toggle, need [[Edge Detection]] pattern
- See [[P04 Toggle LED]]

---

## Related Notes

- [[Push Button]]
- [[Floating Pin Bug]]
- [[Edge Detection]]
- [[Digital Pins]]
- [[P04 Toggle LED]]
