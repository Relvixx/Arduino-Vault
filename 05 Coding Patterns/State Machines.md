# State Machines

**Tags:** #coding #pattern #FSM #architecture

---

## Definition

A state machine is a way of organizing logic so that your program
**behaves differently depending on its current state**, and
**transitions between states only when certain conditions are met.**

---

## The Three Components

STATES      — distinct situations the system can be in
TRANSITIONS — conditions that move between states
ACTIONS     — what happens in each state or on each transition


---

## Why Use State Machines

Without state machines, complex behavior requires deeply nested
`if/else` chains that become impossible to maintain:

```cpp
// BAD — unmanageable at scale
if (buzzerOn && buttonPressed && tempHigh && !alreadyAcknowledged) {
  ...
}
```

With state machines, each state is isolated:

```cpp
// GOOD — each case is independent and clear
switch (currentState) {
  case IDLE:    handleIdle();    break;
  case ALARM:   handleAlarm();   break;
  case ACK:     handleAck();     break;
}
```

---

## Implementation Pattern — enum + switch

```cpp
// Step 1: Define states with readable names
enum SystemState {
  NORMAL,
  BORDERLINE,
  ALERT_ACTIVE,
  ALERT_ACKNOWLEDGED
};

SystemState currentState = NORMAL;   // initial state

// Step 2: Handle each state in switch
void loop() {
  switch (currentState) {

    case NORMAL:
      if (conditionForAlert) {
        currentState = ALERT_ACTIVE;    // transition
        lastBuzzerTime = millis();      // action on transition
      }
      break;

    case ALERT_ACTIVE:
      runBuzzerPattern();               // action in state
      if (buttonPressed) {
        currentState = ALERT_ACKNOWLEDGED;
      }
      break;
  }
}
```

---

## State Transition Priority

When multiple transitions are possible from the same state,
**order matters.** The first matching condition wins.

Design rule: check highest-priority transitions first.

```cpp
case ALERT_ACTIVE:
  // 1. Auto-recovery checked FIRST (highest priority)
  if (conditionsNormal) {
    currentState = NORMAL;
    return;
  }
  // 2. User acknowledgement checked SECOND
  if (ackFlag) {
    currentState = ALERT_ACKNOWLEDGED;
  }
  break;
```

See [[State Transition Priority Bug]].

---

## Sub-State Machines

Complex behaviors within one state can use their own
variable-based sub-machine:

```cpp
// beepStep tracks position within the ALERT_ACTIVE state
case ALERT_ACTIVE:
  switch (beepStep) {
    case 0: buzzerOn();  if (timeElapsed) beepStep = 1; break;
    case 1: buzzerOff(); if (timeElapsed) beepStep = 2; break;
    // ...
  }
  break;
```

The 6-step beep sequencer in the capstone uses exactly this pattern.

---

## The "Isolation" Principle

Each state is isolated in its own `case` block.
Adding a new state means:
1. Add name to `enum`
2. Add `case` block with its behavior

Nothing else changes. The other states are untouched.

Compare to `if/else`: adding a condition requires modifying
the entire chain — risk of introducing bugs everywhere.

---

## States vs Events

| Term | Definition | Example |
|---|---|---|
| **State** | Current situation the system is in | `ALERT_ACTIVE` |
| **Event** | Something that happens at a moment in time | Button just pressed |

Events trigger transitions. States determine behavior.

---

## Related Notes

- [[Edge Detection]]
- [[millis Non-Blocking Pattern]]
- [[State Transition Priority Bug]]
- [[P09 Traffic Light]]
- [[P10 Pedestrian Crossing]]
- [[P12 Smart Environment Monitor]]
