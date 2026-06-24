# UART — Serial Communication

**Tags:** #communication #serial #UART

---

## What It Is

**Universal Asynchronous Receiver Transmitter.**
The protocol behind `Serial.print()`.

UART is a two-wire communication system:
- **TX** — Transmit (sends data)
- **RX** — Receive (receives data)

On Arduino Uno: TX = **pin 1**, RX = **pin 0**

---

## How It Works
Arduino TX ──────────────────→ Computer RX

Arduino RX ←────────────────── Computer TX

Data is sent as a series of bits at a fixed rate called
the **baud rate** (bits per second).
Serial.begin(9600);   // 9600 bits per second

Both sides must agree on the same baud rate.
Mismatch → garbled output in Serial Monitor.

---

## The USB Conversion

The Arduino Uno has a **USB-to-UART converter chip** on the board.
When you plug in USB, this chip translates:
- USB protocol ↔ UART protocol

This is why you can use `Serial.print()` from a USB-connected sketch
even though the microcontroller only speaks UART.

---

## Common Baud Rates

| Rate | Use |
|---|---|
| 9600 | Standard learning/debugging |
| 115200 | Faster output for high-speed logging |

Always match baud rate in code and in Serial Monitor dropdown.

---

## Serial Functions

```cpp
Serial.begin(9600);           // initialize — ALWAYS first in setup()
Serial.print(value);          // print without newline
Serial.println(value);        // print with newline
Serial.print(value, 1);       // print float to 1 decimal place
Serial.print(F("text"));      // print from Flash (saves SRAM)

// Input:
int n = Serial.available();   // number of bytes waiting
byte b = Serial.read();       // read one byte (returns -1 if empty)
int x = Serial.parseInt();    // read integer from incoming stream
```

**`Serial.read()` on empty buffer returns -1, not 0.**
Check `Serial.available() > 0` before reading.

---

## Serial as a Debugging Tool

The most important debugging habit:

```cpp
// Print the value, not just the action
Serial.print(F("sensorValue: "));
Serial.println(sensorValue);    // see actual numbers

// Use F() macro on all string literals
Serial.println(F("Button pressed"));   // 0 SRAM cost
```

Three rules:
1. Print the value, not just a label
2. Use F() macro on every string literal
3. Remove debug prints before final deployment
   (they consume time and can affect timing-sensitive code)

---

## The Pin 0/1 Rule

Pins 0 (RX) and 1 (TX) are the hardware UART lines.
If anything is connected to these pins during upload,
it fights with the upload data — upload fails.

See [[Pins 0 and 1 Upload Conflict]].

---

## Related Notes

- [[Digital Pins]]
- [[Arduino Uno Anatomy]]
- [[How Code Runs]]
- [[Pins 0 and 1 Upload Conflict]]
- [[Memory Types]]
