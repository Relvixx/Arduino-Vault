# P11 — TMP36 Sensor with Servo and LED

**Tags:** #project #TMP36 #servo #LED #filter #hysteresis

---

## Goal

Read temperature from TMP36.
Servo angle maps to temperature (0°C → 0°, 50°C → 180°).
LED turns ON above 37°C, OFF below 33°C (hysteresis).
Moving average filter smooths sensor noise.
Print telemetry every 500ms via Serial.

---

## Components

| Component | Quantity | Notes |
|---|---|---|
| Arduino Uno | 1 | |
| Breadboard | 1 | |
| TMP36 | 1 | |
| Servo motor | 1 | |
| LED | 1 | |
| Resistor | 1 | 220Ω |
| Jumper wires | 8 | |

---

## Wiring Logic

```
TMP36:
  Left  → 5V
  Middle → A0
  Right  → GND

Servo:
  Signal → Pin 9
  VCC    → 5V
  GND    → GND

LED:
  Pin 11 → 220Ω → LED anode → cathode → GND
```

---

## Code (Modular Final Version)

```cpp
#include <Servo.h>

const int NUM_SAMPLES = 5;
int samples[NUM_SAMPLES] = {0};
int sampleIndex = 0;
long runningTotal = 0;

Servo myServo;
const int LED_PIN    = 11;
const int SENSOR_PIN = A0;
const float THRESHOLD_HIGH = 37.0;
const float THRESHOLD_LOW  = 33.0;

float readTemperature() {
  runningTotal -= samples[sampleIndex];
  int raw = analogRead(SENSOR_PIN);
  samples[sampleIndex] = raw;
  runningTotal += raw;
  sampleIndex = (sampleIndex + 1) % NUM_SAMPLES;
  float voltage = (runningTotal / (float)NUM_SAMPLES) * (5.0 / 1023.0);
  return (voltage - 0.5) * 100.0;
}

void updateServo(float temp) {
  int angle = map((int)temp, 0, 50, 0, 180);
  angle = constrain(angle, 0, 180);
  myServo.write(angle);
}

void updateLED(float temp) {
  static bool ledOn = false;
  if (temp > THRESHOLD_HIGH) ledOn = true;
  if (temp < THRESHOLD_LOW)  ledOn = false;
  digitalWrite(LED_PIN, ledOn ? HIGH : LOW);
}

void printTelemetry(float temp, int angle) {
  static unsigned long lastPrint = 0;
  if (millis() - lastPrint >= 500) {
    lastPrint = millis();
    Serial.print(F("Temp: "));
    Serial.print(temp);
    Serial.print(F(" °C | Servo: "));
    Serial.println(angle);
  }
}

void setup() {
  Serial.begin(9600);
  myServo.attach(9);
  pinMode(LED_PIN, OUTPUT);

  // Pre-fill filter buffer
  for (int i = 0; i < NUM_SAMPLES; i++) {
    samples[i] = analogRead(SENSOR_PIN);
    runningTotal += samples[i];
  }
}

void loop() {
  float temp = readTemperature();
  int angle  = constrain(map((int)temp, 0, 50, 0, 180), 0, 180);
  updateServo(temp);
  updateLED(temp);
  printTelemetry(temp, angle);
}
```

---

## What Was Learned

- Moving average filter with circular buffer
- `static` local variables inside functions for persistent state
- `static bool ledOn` → hysteresis state lives inside `updateLED()`
- Two separate `if` statements for hysteresis — NOT `if/else`
- `map()` requires explicit `(int)` cast for float temperature
- `Servo.h` disables PWM on pins 9 and 10 (Timer1 conflict)
- `loop()` under 5 lines when functions are properly modular
- Buffer initialization in `setup()` prevents wrong startup readings

---

## Related Notes

- [[TMP36]]
- [[Servo Motor]]
- [[Moving Average Filter]]
- [[Hysteresis]]
- [[Modular Code Architecture]]
- [[map Float Argument Bug]]
- [[Buffer Initialization Bug]]
- [[P12 Smart Environment Monitor]]
```

---

## `07 Projects/P12 Smart Environment Monitor.md`

```markdown
# P12 — Smart Environment Monitor (Capstone)

**Tags:** #project #capstone #FSM #state-machine #sensors
#filter #hysteresis #modular #millis

---

## Goal

A complete embedded monitoring system for a room environment.
Monitors temperature (TMP36) and light level (LDR).
Uses a four-state FSM to drive LED indicators and a buzzer alarm.
All inputs filtered. All thresholds with hysteresis.
No `delay()`. Fully modular. `loop()` under 10 lines.

---

## System States

```
NORMAL            → Green LED ON, all conditions good
BORDERLINE        → Yellow LED ON, one condition approaching limit
ALERT_ACTIVE      → Red LED ON, buzzer playing 3-beep pattern
ALERT_ACKNOWLEDGED → Red LED ON, buzzer silenced (button pressed)
```

---

## Components

| Component | Pin | Notes |
|---|---|---|
| TMP36 | A0 | Left=5V, Middle=A0, Right=GND |
| LDR | A1 | Voltage divider with 10kΩ to GND |
| Push button | 2 | INPUT_PULLUP, edge-detected |
| Active buzzer | 4 | digitalWrite() control |
| Red LED | 7 | 220Ω resistor |
| Yellow LED | 8 | 220Ω resistor |
| Green LED | 12 | 220Ω resistor |

---

## Threshold Table

| Condition | Alert ON | Alert OFF | Borderline ON | Borderline OFF |
|---|---|---|---|---|
| Temperature | > 35.5°C | < 34.5°C | > 30.5°C | < 29.5°C |
| Light | < 200 ADC | > 215 ADC | < 230 ADC | > 245 ADC |

---

## Beep Sequencer (Sub-FSM)

6-step pattern using `beepStep` variable:

```
Step 0: Buzzer ON  (Beep 1, 100ms)
Step 1: Buzzer OFF (Gap, 100ms)
Step 2: Buzzer ON  (Beep 2, 100ms)
Step 3: Buzzer OFF (Gap, 100ms)
Step 4: Buzzer ON  (Beep 3, 100ms)
Step 5: Buzzer OFF (Long silence, 1000ms → return to step 0)
```

---

## State Transition Priority

In `ALERT_ACTIVE`:
1. **Auto-recovery checked FIRST** — if conditions clear,
   go to NORMAL/BORDERLINE regardless of button state
2. **Button acknowledgement checked SECOND** — only if
   conditions have NOT recovered

---

## Final loop() — 7 Lines

```cpp
void loop() {
  float currentTemp  = readTemperature();
  int   currentLight = readLight();
  checkButton();
  updateState(currentTemp, currentLight);
  updateLEDs(currentState);
  updateBuzzer(currentState);
  printTelemetry(currentTemp, currentLight, currentState);
}
```

---

## Key Functions

```
readTemperature()   → circular buffer filter, returns float °C
readLight()         → circular buffer filter, returns int ADC
checkButton()       → edge detection, sets global ackFlag
updateState()       → FSM transitions with hysteresis and priority
updateLEDs()        → sets 3 LED outputs from current state
updateBuzzer()      → 6-step sub-FSM for beep pattern
printTelemetry()    → non-blocking 2s serial output
```

---

## Bugs Found and Fixed in Code Review

### Bug 1 — First Beep Silent
`lastBuzzerTime` not reset when entering `ALERT_ACTIVE`.
Fix: add `lastBuzzerTime = millis()` alongside `beepStep = 0`
at every NORMAL→ALERT and BORDERLINE→ALERT transition.

### Bug 2 — Wrong Startup Sensor Readings
Filter buffer initialized with zeros in global declarations.
Fix: pre-fill buffers in `setup()` with real `analogRead()` values.

---

## Design Document Process

This project required a formal design document before any
code was written (Deliverable 1). The document was reviewed
and corrections required before coding was permitted.

Issues caught at design stage:
- `tempBuffer` declared as `float` → corrected to `int` (saves 20 bytes SRAM)
- `tempSum` type corrected to `long`
- `ackFlag` global variable missing from design
- `beepStep` values 0–5 undocumented → required explicit definition
- Simultaneous transition priority undocumented → required explicit rule
- Buzzer type unspecified → specified as active buzzer

---

## Self-Review Reflections

**Hardest part to design:**
The 6-step non-blocking beep sub-FSM — required complete
paradigm shift from sequential `delay()` thinking to
event-driven state tracking across thousands of loop iterations.

**Where plan changed during coding:**
`tempBuffer` type issue — caught at design review, not at
implementation. Proof that design review prevents code bugs.

**What would break first in 24-hour deployment:**
Fixed light thresholds become wrong as ambient daylight changes
over a day/night cycle. Fix: dynamic baseline calibration
in `setup()` — sample ambient light and compute thresholds
as percentage of measured baseline.

**Adding a 4th sensor:**
Change only: new `readHumidity()` function, one condition
in `updateState()`, one line in `loop()`.
Do NOT change: `updateLEDs()`, `updateBuzzer()`, everything else.
Proof of genuine modularity — outputs are decoupled from inputs.

---

## Complete Code

See full `.ino` file in course materials.
Key architectural patterns are documented in:
- [[State Machines]]
- [[Moving Average Filter]]
- [[Hysteresis]]
- [[millis Non-Blocking Pattern]]
- [[Modular Code Architecture]]
- [[Edge Detection]]

---

## Related Notes

- [[TMP36]]
- [[LDR]]
- [[Push Button]]
- [[Buzzer]]
- [[State Machines]]
- [[Moving Average Filter]]
- [[Hysteresis]]
- [[Buzzer Timing Bug]]
- [[Buffer Initialization Bug]]
- [[State Transition Priority Bug]]
- [[Modular Code Architecture]]
