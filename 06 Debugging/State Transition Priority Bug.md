# State Transition Priority Bug

**Tags:** #debugging #FSM #state #transitions

---

## Symptom

- State machine transitions to the wrong state
- A lower-priority event overrides a higher-priority one
- System enters BORDERLINE when it should enter ALERT_ACTIVE
- Multiple conditions are true simultaneously — wrong one wins

---

## Cause

In a switch/case state machine, the first matching condition wins.
If transition conditions are checked in the wrong order,
a lower-priority condition can intercept control before
the higher-priority condition is reached.

### Example: NORMAL → ALERT bypass

```cpp
// WRONG ORDER
case NORMAL:
  if (temp > TEMP_BORDER_ON) {           // checked first
    currentState = BORDERLINE;           // transitions here...
  }
  else if (temp > TEMP_ALERT_ON) {       // never reached if border fires
    currentState = ALERT_ACTIVE;
  }
  break;
```

When `temp = 36°C` (above BOTH thresholds):
- `temp > TEMP_BORDER_ON` (30.5°C) → TRUE → transitions to BORDERLINE
- Never reaches the ALERT check
- System lands in BORDERLINE instead of ALERT_ACTIVE

---

## The Fix

Check the **most specific / highest priority** condition first:

```cpp
// CORRECT ORDER — most restrictive condition first
case NORMAL:
  if (temp > TEMP_ALERT_ON || light < LIGHT_ALERT_ON) {
    currentState = ALERT_ACTIVE;         // checked first — highest priority
    beepStep = 0;
    lastBuzzerTime = millis();
  }
  else if (temp > TEMP_BORDER_ON || light < LIGHT_BORDER_ON) {
    currentState = BORDERLINE;           // only if alert did not fire
  }
  break;
```

Now at `temp = 36°C`: ALERT_ACTIVE check fires first, correctly.

---

## Priority Rule for ALERT_ACTIVE Recovery

```cpp
case ALERT_ACTIVE:
  // 1. Auto-recovery FIRST — highest priority
  if (temp < TEMP_ALERT_OFF && light > LIGHT_ALERT_OFF) {
    currentState = NORMAL;    // or BORDERLINE
    ackFlag = false;
  }
  // 2. User acknowledgement SECOND
  else if (ackFlag == true) {
    currentState = ALERT_ACKNOWLEDGED;
    ackFlag = false;
  }
  break;
```

If both conditions are true simultaneously (conditions just
recovered AND button just pressed), auto-recovery wins.
This is the designed behavior — document it explicitly.

---

## Simultaneous Condition Design Rule

When multiple exit conditions can be true simultaneously:
1. Decide which has priority
2. Check that one first in code
3. Document the priority decision in a comment

Never leave priority ambiguous — "whichever if-block comes
first" is a design decision, not an accident.

---

## Prevention

Before implementing a state machine:
- List all transitions out of each state
- Order them by priority (highest first)
- Implement in that exact order
- Review the order as part of every code review

---

## Related Notes

- [[State Machines]]
- [[Software Debugging Process]]
- [[P12 Smart Environment Monitor]]
- [[Edge Detection]]
