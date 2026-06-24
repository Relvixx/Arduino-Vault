# Mistakes and Corrections Log

**Tags:** #reference #mistakes #corrections #learning

---

> This log preserves every significant mistake made during the course
> and the correction applied. Mistakes are organized by type.
> Reading this before a project prevents repeating them.

---

## Type Errors

### M01 — Wrong voltage in LED resistor calculation
**Mistake:** Used LED forward voltage (2V) as the resistor voltage.
`R = 2V / 0.02A = 100Ω`
**Correction:** The resistor sees supply minus LED voltage.
`R = (5V - 2V) / 0.02A = 150Ω`
**Rule:** R = (Supply − Vf) / I. Always subtract Vf first.

---

### M02 — int used for millis() timing variable
**Mistake:** `int previousTime = 0;`
**Symptom:** Timing worked for 32 seconds, then broke.
**Correction:** `unsigned long previousTime = 0;`
**Rule:** All millis() variables must be `unsigned long`. No exceptions.

---

### M03 — float buffer for moving average filter
**Mistake:** `float tempBuffer[10]` — storing processed temperatures.
**Correction:** `int tempBuffer[10]` — store raw ADC integers.
Convert to temperature once after averaging.
**Saving:** 20 bytes of SRAM (4 bytes × 10 entries vs 2 bytes × 10).
**Rule:** Store the rawest form of data. Convert once at the end.

---

### M04 — map() called with float argument
**Mistake:** `map(floatTemp, 0, 50, 0, 180)` — silent truncation.
**Symptom:** Servo moved in coarse steps, lost decimal precision.
**Correction:** `map((int)floatTemp, 0, 50, 0, 180)` — explicit cast.
**Rule:** map() takes long integers. Make float truncation explicit.

---

### M05 — Integer division in voltage conversion
**Mistake:** `float v = raw * (5 / 1023);` — produces 0.
**Correction:** `float v = raw * (5.0 / 1023.0);` — float division.
**Rule:** At least one operand must be float for float division.

---

## Logic Errors

### M06 — Incomplete blink cycle
**Mistake:** Blink pattern had HIGH + delay + LOW but no delay after LOW.
**Symptom:** OFF time uncontrolled — determined by whatever came next.
**Correction:** Every blink = HIGH + delay + LOW + delay.
**Rule:** A complete blink always has two halves. Both need timing.

---

### M07 — Wrong boundary operator
**Mistake:** `if (x > 512)` and `else if (x < 512)` — missed x == 512.
**Correction:** `else if (x <= 512)` captures the exact boundary.
**Rule:** Explicitly decide whether boundary belongs to ON or OFF case.

---

### M08 — Hysteresis with if/else instead of two if statements
**Mistake:** Using `if/else` for hysteresis.
**Symptom:** Inside the hysteresis band, the `else` fired → turned off.
**Correction:** Two completely independent `if` statements.
**Rule:** Hysteresis requires two independent ifs. Never if/else.

---

### M09 — State transition priority wrong
**Mistake:** BORDERLINE condition checked before ALERT condition.
**Symptom:** System entered BORDERLINE when ALERT_ACTIVE was required.
**Correction:** ALERT checked first (most restrictive), BORDERLINE second.
**Rule:** In state machines, check most specific condition first.

---

### M10 — ackFlag missing from design document
**Mistake:** Design document referenced ackFlag without declaring it.
**Symptom:** Found during design review — checkButton() had nowhere
to write its result.
**Correction:** Added `bool ackFlag = false` to global variable table
with full lifecycle documentation.
**Rule:** Every variable written by one function and read by another
must be explicitly documented in the design.

---

## Omissions

### M11 — Buffer not initialized in setup()
**Mistake:** Moving average buffer left as all zeros.
**Symptom:** First 10 readings wildly wrong (e.g., -42°C from TMP36).
**Correction:** Pre-fill buffer in setup() with real analogRead() values.
**Note:** This mistake occurred THREE times during the course —
Module 11, Module 12, and Module 13 capstone.
**Rule:** Writing a filter function → immediately write the setup()
initialization. They are one unit.

---

### M12 — lastBuzzerTime not reset on state entry
**Mistake:** `beepStep = 0` set on ALERT_ACTIVE entry, but
`lastBuzzerTime` not reset.
**Symptom:** First beep effectively silent — condition
`currentTime - lastBuzzerTime >= 100` immediately true.
**Correction:** `lastBuzzerTime = millis()` at every ALERT_ACTIVE entry.
**Rule:** When resetting a sub-FSM step variable, always ask:
"Does the associated timer also need resetting?"

---

### M13 — F() macro omitted from Serial strings
**Mistake:** `Serial.print("text")` — string stored in SRAM.
**Symptom:** Unnecessary SRAM consumption. IDE may warn at high usage.
**Correction:** `Serial.print(F("text"))` — string stored in Flash.
**Note:** This slipped after Module 7 introduced it.
Recurred in Module 11.
**Rule:** F() macro on every quoted string in any Serial call.
No exceptions. Build it as muscle memory.

---

## Terminology Errors

### M14 — "register" instead of "resistor"
**Mistake:** "Add a register to limit current."
**Correction:** "Add a **resistor** to limit current."
A register is a memory location inside the CPU.
A resistor is an electronic component.
**Rule:** Hardware terminology must be precise. Wrong words
cause confusion in documentation and datasheet reading.

---

### M15 — "analog pins output variable voltage"
**Mistake:** Stating that analog pins A0–A5 can output variable voltage.
**Correction:** A0–A5 are INPUT only for analog signals.
Variable voltage OUTPUT requires PWM pins (3, 5, 6, 9, 10, 11).
**Rule:** Analog pins READ. PWM pins SIMULATE variable output.

---

### M16 — Describing microcontroller as "runs one specific task"
**Mistake:** "A microcontroller runs one specific task."
**Correction:** It runs one program — but that program can
coordinate multiple components sequentially.
It does not do true parallel multitasking.
**Rule:** Precision in technical descriptions matters.

---

## Habit Failures

### M17 — Skipping plain English logic step
**Mistake:** Writing code directly without designing logic first.
**Symptom:** Code had multiple bugs that a design step would have prevented.
**Pattern:** Occurred at Module 3 challenge, Module 4 challenge,
Module 5 challenge. Corrected each time.
**Rule:** No code before logic. Always. This habit protects you
at the scale where projects get complex.

---

### M18 — Technical writing in Hindi
**Mistake:** Plain English logic and code comments written in Hindi.
**Correction:** All technical writing in English — comments,
logic breakdowns, variable names, documentation.
**Reason:** Datasheets, error messages, libraries, documentation,
and collaborators all use English. Consistency is not optional.

---

### M19 — Answering questions without reading them
**Mistake:** Answering 4 questions as if they were one question.
Submitted a list of 5 reasons for Q4 while Q1–Q3 were skipped.
**Correction:** Read each question. Answer each question.
Flag uncertainty rather than skipping.
**Rule:** Skipping questions hides gaps. Partial answers expose them.
Gaps exposed early are fixed cheaply. Gaps hidden persist.

---

## Related Notes

- [[Best Practices]]
- [[Hardware Debugging Process]]
- [[Software Debugging Process]]
- [[Buffer Initialization Bug]]
- [[millis Type Bug]]
- [[Buzzer Timing Bug]]
