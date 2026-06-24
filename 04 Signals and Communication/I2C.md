# I2C — Inter-Integrated Circuit

**Tags:** #communication #I2C #protocol #sensors

---

## What It Is

A two-wire protocol for connecting multiple devices to one
Arduino using only **two shared wires**.
Arduino      Device 1    Device 2    Device 3

│              │           │           │

SDA├─────────────┤───────────┤───────────┤

SCL├─────────────┤───────────┤───────────┤

│              │           │           │

GND────────────────────────────────────GND

- **SDA** — Serial Data (data travels here)
- **SCL** — Serial Clock (timing signal)

On Arduino Uno: **SDA = A4**, **SCL = A5**

---

## How Multiple Devices Work

Each I2C device has a unique **address** (7-bit, 0–127).

Arduino calls a device by its address — only that device responds.
Like sending a letter with a specific house number on a shared street.

```cpp
#include <Wire.h>

Wire.begin();                      // initialize as master
Wire.beginTransmission(0x3C);      // address device at 0x3C
Wire.write(data);                  // send data byte
Wire.endTransmission();            // release bus
```

---

## Why It Matters for Projects

Most common I2C devices you will encounter:
- OLED displays (typically address 0x3C or 0x3D)
- IMU sensors (accelerometer/gyroscope)
- Real-time clocks (RTC)
- Barometric pressure sensors

When you use a higher-level library (like `Adafruit_SSD1306`
for OLED), it calls `Wire.h` internally. Understanding I2C
helps when the library behaves unexpectedly.

---

## I2C vs SPI Trade-off

| Feature | I2C | SPI |
|---|---|---|
| Wires | 2 | 4 |
| Speed | Slower (100kHz–400kHz) | Faster (up to MHz) |
| Multiple devices | Address-based | SS pin per device |
| Best for | Many slow sensors | Fast single peripherals |

---

## Troubleshooting I2C

If a device is not responding:
1. Check SDA and SCL connections (A4 and A5)
2. Verify device address — use I2C scanner sketch
3. Check pull-up resistors (most breakout boards include them)
4. Verify power supply to device (3.3V vs 5V)

---

## Related Notes

- [[SPI]]
- [[Analog Pins]]
- [[Arduino Uno Anatomy]]
- [[What to Study Next]]
