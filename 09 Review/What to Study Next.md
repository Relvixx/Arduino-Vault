# What to Study Next

**Tags:** #review #roadmap #robotics #advanced

---

## Current Level

Independent Arduino intermediate.
Can design, build, debug, and document embedded systems
from problem statements without assistance.

---

## Immediate Next Skills (High Priority)

### 1. I2C Peripherals — OLED Display
**Why:** Visual output without Serial Monitor. Essential for
standalone devices. Builds on I2C knowledge from Module 5.

**What to build:**
- Wire an SSD1306 OLED display (128×64 pixels)
- Display TMP36 temperature reading in real time
- Extend the Smart Environment Monitor with visual output

**Libraries:** `Adafruit_SSD1306`, `Adafruit_GFX`
**Pins used:** A4 (SDA), A5 (SCL)

---

### 2. I2C Real-Time Clock (RTC)
**Why:** Timestamps for data logging. Timekeeping that survives
power cycles.

**What to build:**
- Wire DS3231 RTC module
- Display current time on OLED
- Log temperature readings with timestamps via Serial

**Library:** `RTClib`

---

### 3. Hardware Interrupts in Practice
**Why:** You understand interrupts conceptually from Module 5.
Now build something that requires them.

**What to build:**
- Interrupt-driven button counter
- Emergency stop for a motor system
- Replace polling in Smart Environment Monitor with interrupt

**Pins:** 2 and 3 on Uno only

---

### 4. EEPROM Persistence
**Why:** Save calibration data and settings across power cycles.
Directly applicable to improving the Smart Environment Monitor.

**What to build:**
- Save light baseline calibration to EEPROM at setup
- Read it back on next power-on
- Count power cycles and display on Serial

**Limit:** 100,000 write cycles per address — implement
wear leveling for anything written frequently

---

### 5. DC Motor Control with L298N
**Why:** First step toward robotics. Full directional motor control.

**What to build:**
- Wire L298N H-bridge with a DC motor
- Forward, reverse, stop via Serial commands
- Speed control via PWM on ENA pin
- Add button-triggered direction change

---

## Intermediate Robotics Skills (Medium Priority)

### 6. Ultrasonic Sensor (HC-SR04)
**Why:** Distance measurement. Foundation for obstacle avoidance.

```cpp
// Trigger: send 10μs pulse
// Echo: measure how long HIGH pulse lasts
// Distance = (echo_duration × 0.034) / 2  (cm)
```

**What to build:**
- Distance meter with Serial output
- LED or buzzer that changes behavior by distance
- Parking sensor (beep frequency increases as object gets closer)

---

### 7. PID Control (Proportional-Integral-Derivative)
**Why:** Precise motor control. Used in robotics, drones,
CNC machines, temperature controllers.

**Concept:** Instead of "if too fast, slow down" —
calculate how far from target and apply proportional correction.

**What to build:**
- Line follower robot with two IR sensors and two motors
- Temperature controller holding exact target temperature
- Servo position controller with PID

---

### 8. Encoder Feedback for Motors
**Why:** Know actual motor speed/position, not just commanded value.
Closed-loop control requires feedback.

**What to build:**
- Attach encoder to DC motor
- Count pulses in ISR (interrupt)
- Display RPM on Serial
- Use encoder feedback to maintain constant speed under load

---

## Communication and Connectivity (When Ready)

### 9. SoftwareSerial — Two Serial Ports
**Why:** Communicate with GPS or Bluetooth module while
keeping Serial for debugging.

```cpp
#include <SoftwareSerial.h>
SoftwareSerial mySerial(10, 11);   // RX, TX
```

---

### 10. ESP8266 / ESP32 — WiFi
**Why:** Connect projects to the internet. Send sensor data
to a server. Control Arduino from a web browser.

**Migration path:**
- Start with ESP8266 NodeMCU (Arduino-compatible)
- Same C++ code structure — slight library differences
- Add WiFi capabilities with `ESP8266WiFi.h`
- Send data to ThingSpeak or a custom server

**What to build:**
- WiFi temperature monitor posting to a web dashboard
- Remote-controlled LED via web browser
- MQTT-based home automation prototype

---

## Suggested Learning Order

```
Current → I2C OLED Display
        → RTC Clock
        → Hardware Interrupts
        → EEPROM Persistence
        → DC Motor + L298N
        → Ultrasonic Sensor
        → PID Control basics
        → Encoder feedback
        → ESP8266 WiFi
        → Full robotics project
```

---

## The Robotics Project Goal

By the end of this path, you should be able to build:

**An autonomous obstacle-avoiding robot with:**
- Two DC motors (L298N H-bridge)
- HC-SR04 ultrasonic sensor
- Encoder feedback for speed control
- OLED display showing mode and distance
- WiFi reporting to a dashboard
- All logic in a clean state machine
- No delay() anywhere

Every concept needed for that robot is either already in
this vault or on this roadmap.

---

## Related Notes

- [[Final Reflection]]
- [[State Machines]]
- [[millis Non-Blocking Pattern]]
- [[I2C]]
- [[Interrupts]]
- [[DC Motor]]
- [[Best Practices]]
