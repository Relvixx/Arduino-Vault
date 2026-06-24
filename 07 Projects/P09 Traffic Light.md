# P09 — Traffic Light State Machine

**Tags:** #project #state-machine #enum #millis #LED

---

## Goal

Three LEDs (red, yellow, green) cycle through traffic light
sequence with fixed durations — using a formal state machine
with `enum` and `switch/case`. No `delay()`.

```
RED (4s) → GREEN (3s) → YELLOW (1s) → RED → ...
```

---

## Components

| Component | Quantity | Notes |
|---|---|---|
| Arduino Uno | 1 | |
| Breadboard | 1 | |
| LED | 3 | Red, Yellow, Green |
| Resistor | 3 | 220Ω each |
| Jumper wires | 6 | |

---

## Wiring Logic

```
Pin 11 → 220Ω → Red LED → GND
Pin 10 → 220Ω → Yellow LED → GND
Pin 9  → 220Ω → Green LED → GND
```

---

## Code

```cpp
enum LightState { RED, GREEN, YELLOW };
LightState currentLight = RED;
unsigned long stateStart = 0;

const int redPin    = 11;
const int yellowPin = 10;
const int greenPin  = 9;

const unsigned long RED_DURATION    = 4000;
const unsigned long GREEN_DURATION  = 3000;
const unsigned long YELLOW_DURATION = 1000;

void setLight(LightState state) {
  digitalWrite(redPin,    state == RED);
  digitalWrite(yellowPin, state == YELLOW);
  digitalWrite(greenPin,  state == GREEN);
}

void setup() {
  pinMode(redPin,    OUTPUT);
  pinMode(yellowPin, OUTPUT);
  pinMode(greenPin,  OUTPUT);
  setLight(RED);
  stateStart = millis();
}

void loop() {
  unsigned long elapsed = millis() - stateStart;

  switch (currentLight) {
    case RED:
      if (elapsed >= RED_DURATION) {
        currentLight = GREEN;
        setLight(GREEN);
        stateStart = millis();
      }
      break;

    case GREEN:
      if (elapsed >= GREEN_DURATION) {
        currentLight = YELLOW;
        setLight(YELLOW);
        stateStart = millis();
      }
      break;

    case YELLOW:
      if (elapsed >= YELLOW_DURATION) {
        currentLight = RED;
        setLight(RED);
        stateStart = millis();
      }
      break;
  }
}
```

---

## The setLight() Trick

```cpp
digitalWrite(redPin, state == RED);
```

`state == RED` evaluates to `true` (1/HIGH) or `false` (0/LOW).
One line sets each pin correctly — no repetitive HIGH/LOW pairs.
Three lines replace what would otherwise be 9 lines.

---

## What Was Learned

- `enum` gives states readable names instead of magic numbers
- `switch/case` isolates each state's logic completely
- All durations as named `const unsigned long` — no magic numbers
- `setLight()` helper abstracts LED control cleanly
- State machine naturally expands — add a state by adding a case
- This was the first formally named state machine in the course

---

## Related Notes

- [[State Machines]]
- [[millis Non-Blocking Pattern]]
- [[P10 Pedestrian Crossing]]
- [[P12 Smart Environment Monitor]]
