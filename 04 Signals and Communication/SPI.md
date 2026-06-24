# SPI — Serial Peripheral Interface

**Tags:** #communication #SPI #protocol

---

## What It Is

A four-wire protocol for fast communication with peripherals.
Faster than I2C but uses more wires.

---

## The Four Wires

| Wire | Full name | Direction |
|---|---|---|
| MOSI | Master Out Slave In | Arduino → Device |
| MISO | Master In Slave Out | Device → Arduino |
| SCK | Serial Clock | Arduino → Device |
| SS | Slave Select | Arduino → Device (one per device) |

---

## When to Use SPI vs I2C

Use SPI when:
- Speed matters more than pin count
- Connecting SD card modules
- Connecting certain high-speed displays
- Connecting radio modules (nRF24L01, LoRa)

Use I2C when:
- Connecting many slow sensors
- Pin count is limited
- Speed is not critical

---

## Multiple SPI Devices

Unlike I2C (address-based), SPI uses a dedicated **SS (Slave Select)**
pin per device. Pull SS LOW to activate a device, HIGH to deactivate.

Each additional SPI device requires one additional digital pin.

---

## Practical Note

SPI is not needed for beginner or most intermediate projects.
Understanding it becomes important when working with:
- SD card data logging
- Fast wireless communication
- High-refresh-rate displays

See [[What to Study Next]] for when to learn this in depth.

---

## Related Notes

- [[I2C]]
- [[UART]]
- [[Digital Pins]]
- [[What to Study Next]]
