# Buzzer Timing Bug

**Tags:** #debugging #buzzer #timing #millis #FSM

---

## Symptom

- First beep of the alert pattern is silent or inaudible
- Alert pattern appears to start at step 1 (gap) instead of step 0 (beep)
- Buzzer pattern seems to skip the very first ON phase
- Pattern works correctly from the second cycle onward

---

## Cause

When the state machine transitions to `ALERT_ACTIVE`,
`beepStep` is reset to 0 — but `lastBuzzerTime` is not reset.

`lastBuzzerTime` was initialized to 0 at program start.
By the time ALERT_ACTIVE is first entered, `millis()` may
be several seconds or minutes into the run:

```cpp
// In updateState() — WRONG
currentState = ALERT_ACTIVE;
beepStep = 0;
// lastBuzzerTime NOT reset — still 0 from initialization

// In updateBuzzer(), case 0:
case 0:
  digitalWrite(BUZZER_PIN, HIGH);   // buzzer turns on
  if (currentTime - lastBuzzerTime >= BEEP_ON_MS) {
    // currentTime = 5000ms, lastBuzzerTime = 0
    // 5000 - 0 = 5000 >= 100 → TRUE immediately
    lastBuzzerTime = currentTime;
    beepStep = 1;   // jumps to OFF in same iteration
  }
  break;
```

The buzzer turns HIGH for one loop iteration (microseconds),
then immediately advances to step 1 (OFF).
First beep is effectively inaudible.

---

## The Fix

When transitioning to `ALERT_ACTIVE`, reset `lastBuzzerTime`
at the same time as `beepStep`:

```cpp
// CORRECT
currentState = ALERT_ACTIVE;
beepStep = 0;
lastBuzzerTime = millis();   // reset timer — first beep plays fully
```

This must be added at EVERY location where a transition
to `ALERT_ACTIVE` occurs:

```cpp
case NORMAL:
  if (alertCondition) {
    currentState = ALERT_ACTIVE;
    beepStep = 0;
    lastBuzzerTime = millis();   // ← here
  }
  break;

case BORDERLINE:
  if (alertCondition) {
    currentState = ALERT_ACTIVE;
    beepStep = 0;
    lastBuzzerTime = millis();   // ← and here
  }
  break;
```

---

## Prevention

Rule: Whenever a timed sub-FSM variable (like `beepStep`) is
reset, always ask: **"Does the associated timer variable also
need to be reset?"**

If the timer is not reset, the sub-FSM inherits stale timing
from a previous run or from initialization — producing
wrong behavior on the first iteration.

---

## Related Notes

- [[Buzzer]]
- [[State Machines]]
- [[millis Non-Blocking Pattern]]
- [[P12 Smart Environment Monitor]]
