# What is Arduino

**Tags:** #fundamentals #hardware

---

## Definition

Arduino is a **microcontroller development platform** consisting of:
- A physical board with a microcontroller chip (ATmega328P on the Uno)
- A software IDE for writing and uploading code
- A large library ecosystem for common components

The board can read inputs (sensors, buttons) and produce outputs
(LEDs, motors, buzzers) — all controlled by code you write.

---

## The Core Insight

Arduino has everything a computer needs — but on one chip, for one purpose:

| What it needs | Why | Arduino's answer |
|---|---|---|
| Processor | Execute instructions | 8-bit ATmega328P CPU |
| Memory | Store program and variables | Flash + SRAM + EEPROM |
| I/O | Talk to the physical world | 14 digital + 6 analog pins |
| Power | Run everything | 5V regulated supply |
| Clock | Synchronize execution | 16 MHz crystal oscillator |

---

## Why It Matters

Without understanding *what* Arduino is, you cannot understand *why* 
its constraints exist:
- 2KB SRAM forces careful memory management
- 16 MHz clock means no OS, no multitasking
- 40mA per pin means you can't drive motors directly

---

## Related Notes

- [[Microcontroller vs Computer]]
- [[Memory Types]]
- [[The Clock]]
- [[Arduino Uno Anatomy]]
- [[How Code Runs]]
