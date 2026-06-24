# Modular Code Architecture

**Tags:** #coding #architecture #design #functions

---

## The Principle

**Each function does exactly one thing.**
Each function's name describes that one thing completely.
The main loop reads like a table of contents.

---

## Monolithic vs Modular

### Monolithic (bad)
```cpp
void loop() {
  int raw = analogRead(A0);
  float v = raw * (5.0/1023.0);
  float t = (v-0.5)*100.0;
  if (t > 37) { ledOn = true; }
  if (t < 33) { ledOn = false; }
  digitalWrite(11, ledOn);
  int a = constrain(map((int)t,0,50,0,180),0,180);
  myServo.write(a);
  if (millis()-lastPrint>=500) {
    Serial.print(t);
    lastPrint=millis();
  }
}
```

### Modular (good)
```cpp
void loop() {
  float temp = readTemperature();
  int   angle = getServoAngle(temp);
  updateServo(temp);
  updateLED(temp);
  printTelemetry(temp, angle);
}
```

The second version: anyone understands the system in 5 lines.

---

## Single Responsibility — How to Check

Test: can you describe this function in ONE sentence without "and"?

| Function | Single responsibility? |
|---|---|
| `readTemperature()` | Returns filtered temperature in °C ✅ |
| `updateLED(temp)` | Sets LED state based on temperature ✅ |
| `readSensorAndUpdateLED()` | ❌ Two responsibilities — split it |

---

## The static Pattern for Internal Timing

Functions that manage their own timing do not need global variables:

```cpp
void printTelemetry(float temp, int angle) {
  static unsigned long lastPrint = 0;    // private, persistent
  if (millis() - lastPrint >= 500) {
    lastPrint = millis();
    Serial.print(F("Temp: "));
    Serial.println(temp);
  }
}
```

`lastPrint` is invisible outside this function. No global namespace
pollution. No risk of accidental modification elsewhere.

---

## Adding a New Sensor — Modular Impact

With proper architecture, adding a 4th sensor changes only:
1. New `readSensor4()` function — new code
2. One condition in `updateState()` — minimal change
3. One line in `loop()` — call the new function

What does NOT change:
- `updateLEDs()` — doesn't know about sensors
- `updateBuzzer()` — doesn't know about sensors
- All other functions — completely isolated

This is the proof of a genuinely modular design.

---

## loop() Line Count Rule

A well-architected `loop()` should be **under 10 lines.**

If `loop()` exceeds 10 lines, ask: what logic inside `loop()`
should be extracted into its own function?

---

## Related Notes

- [[Functions]]
- [[Variable Scope and Static]]
- [[State Machines]]
- [[Arduino Code Structure]]
- [[P12 Smart Environment Monitor]]
