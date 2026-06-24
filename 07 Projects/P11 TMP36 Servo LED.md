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
