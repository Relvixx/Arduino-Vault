# Servo Motor

**Tags:** #component #output #motor #actuator

---

## What It Is

A motor that moves to a **specific angle** on command.
Typical range: 0° to 180°.

Internally, the servo contains:
- A DC motor
- A gearbox (reduces speed, increases torque)
- A position feedback sensor (potentiometer)
- A control circuit

You give it an angle — it holds that position precisely.

---

## Library

The `Servo.h` library handles all low-level PWM pulse
width calculations internally:

```cpp
#include 

Servo myServo;

void setup() {
  myServo.attach(9);      // connect to PWM pin 9
}

void loop() {
  myServo.write(0);       // move to 0°
  myServo.write(90);      // move to 90° (center)
  myServo.write(180);     // move to 180°
}
```

---

## Timer Conflict

`Servo.h` uses **Timer1** internally.
This **disables `analogWrite()` on pins 9 and 10**
while the Servo library is active.

If you need PWM output alongside servo control,
use pins 3, 5, 6, or 11 for `analogWrite()`.

---

## Mapping Sensor Input to Servo Angle

```cpp
int raw = analogRead(A0);
int angle = map(raw, 0, 1023, 0, 180);    // scale range
angle = constrain(angle, 0, 180);          // clamp to valid range
myServo.write(angle);
```

**Always use `constrain()` after `map()`.**
Sensor noise can push values slightly outside the expected range.
A value of 181° may cause servo jitter or damage.

See [[map and constrain]].

---

## Mapping Temperature to Servo Angle

```cpp
float temp = readTemperature();
int angle = map((int)temp, 0, 50, 0, 180);
// Cast float to int before map() — map() takes long integers
// (float → map() causes silent truncation, but making it
//  explicit shows intent)
angle = constrain(angle, 0, 180);
myServo.write(angle);
```

See [[map Float Argument Bug]].

---

## Power Considerations

Standard hobby servos draw **150–250mA** under load.
This is within Arduino's USB power budget for a single servo
but approaches the limit.

For multiple servos or high-torque applications:
use an external 5V supply for servo power.
Keep the signal wire connected to Arduino pin.
Share GND between Arduino and external supply.

---

## Related Notes

- [[PWM Pins]]
- [[map and constrain]]
- [[map Float Argument Bug]]
- [[DC Motor]]
- [[P11 TMP36 Servo LED]]
