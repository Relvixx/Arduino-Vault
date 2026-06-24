# P10 — Pedestrian Crossing System

**Tags:** #project #state-machine #millis #dual-timer #LED

---

## Goal

Three-state pedestrian crossing system:

```
WALK  → Green LED ON, pedestrians cross (5 seconds)
WARN  → Yellow LED blinks fast (200ms), warning (3 seconds)
STOP  → Red LED ON, pedestrians wait (4 seconds)
→ back to WALK → repeat
```

**Key challenge:** WARN state requires TWO simultaneous timers —
one for the 200ms blink rhythm, one for the 3-second total duration.

---

## Components

Same as [[P09 Traffic Light]].

---

## State Diagram

```
WALK → (after 5s) → WARN
WARN → (after 3s) → STOP
STOP → (after 4s) → WALK
```

---

## Code (Key Sections)

```cpp
enum PedState { WALK, WARN, STOP };
PedState currentState = WALK;

const unsigned long WALK_DURATION  = 5000;
const unsigned long WARN_DURATION  = 3000;
const unsigned long STOP_DURATION  = 4000;
const unsigned long BLINK_INTERVAL = 200;

unsigned long stateStart = 0;
unsigned long blinkStart = 0;
bool yellowOn = false;

void loop() {
  unsigned long now     = millis();
  unsigned long elapsed = now - stateStart;

  switch (currentState) {

    case WALK:
      if (elapsed >= WALK_DURATION) {
        currentState = WARN;
        stateStart   = now;
        blinkStart   = now;
        yellowOn     = false;
        digitalWrite(yellowPin, LOW);
      }
      break;

    case WARN:
      // Transition check FIRST — prevents double output on exit frame
      if (elapsed >= WARN_DURATION) {
        currentState = STOP;
        setLight(STOP);
        digitalWrite(yellowPin, LOW);
        stateStart = now;
      }
      // Blink logic — only runs if not transitioning
      else if (now - blinkStart >= BLINK_INTERVAL) {
        yellowOn = !yellowOn;
        digitalWrite(yellowPin, yellowOn);
        blinkStart = now;
      }
      break;

    case STOP:
      if (elapsed >= STOP_DURATION) {
        currentState = WALK;
        setLight(WALK);
        stateStart = now;
      }
      break;
  }
}
```

---

## What Was Learned

- Two simultaneous independent timers within one state
- `stateStart` tracks total time in state
- `blinkStart` tracks blink rhythm independently
- Transition check before blink logic prevents double-output on exit
- `else if` in WARN prevents both blocks firing in same iteration
- This was the first dual-timer state machine

---

## Key Insight

The WARN state demonstrates the core power of `millis()`:
two completely separate timing concerns — blink rhythm (200ms)
and total duration (3000ms) — managed simultaneously with
zero blocking.

---

## Related Notes

- [[State Machines]]
- [[millis Non-Blocking Pattern]]
- [[P09 Traffic Light]]
- [[P12 Smart Environment Monitor]]
