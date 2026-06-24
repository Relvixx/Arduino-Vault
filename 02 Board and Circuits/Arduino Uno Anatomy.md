# Arduino Uno Anatomy

**Tags:** #hardware #board #pins

---

## Board Overview

The Arduino Uno has two rows of pins, several key components,
and one microcontroller chip that does all the actual work.

---

## Pin Groups Summary

| Group | Pins | Purpose |
|---|---|---|
| Digital I/O | 0–13 | HIGH or LOW only |
| PWM-capable | 3, 5, 6, 9, 10, 11 | Marked with ~ |
| Serial | 0 (RX), 1 (TX) | UART communication |
| Analog input | A0–A5 | Read 0–5V as 0–1023 |
| Power output | 5V, 3.3V | Supply components |
| Ground | GND (×2) | Circuit reference |
| External input | VIN | 7–12V raw input |
| Reset | RST | Restart sketch |

---

## Key Physical Components

### ATmega328P (the black chip)
The microcontroller. Contains CPU, Flash, SRAM, EEPROM, timers,
ADC, UART, I2C, SPI — all on one chip.

### Crystal Oscillator (silver oval)
Provides the 16 MHz clock signal. See [[The Clock]].

### Voltage Regulator (black rectangle near power jack)
Converts 7–12V input (from DC jack or VIN) to a clean 5V.
Protects the board from input voltage variation.
Does NOT protect pins from your wiring mistakes.

### USB Connector (Type-B, square)
Two purposes simultaneously:
1. Powers the board from computer
2. Uploads your code via UART

### Built-in LED (pin 13)
Soldered directly to digital pin 13.
Your safest test component — requires no external wiring.
Every beginner project starts here.

### Reset Button (red button)
Restarts the sketch from the beginning.
Identical to cutting power and restoring it.

---

## Signal Flow Mental Model
Sensor/button → INPUT pin → ATmega328P → OUTPUT pin → LED/motor

Every pin is a doorway. You declare direction in `setup()`:
```cpp
pinMode(13, OUTPUT);  // traffic flows OUT
pinMode(2, INPUT);    // traffic flows IN
```

---

## Critical Rules

1. **Never exceed 5V on any pin** — kills ATmega328P permanently
2. **Never exceed 40mA per digital pin** — damages the chip
3. **Total current from all pins ≤ 200mA** — regulator limit
4. **Never use pins 0/1 during upload** — blocks UART channel
5. **Always connect GND** — without it, nothing works

---

## Related Notes

- [[Digital Pins]]
- [[Analog Pins]]
- [[PWM Pins]]
- [[Power Pins]]
- [[The Clock]]
- [[How Code Runs]]
- [[Missing GND Bug]]
- [[Pins 0 and 1 Upload Conflict]]
