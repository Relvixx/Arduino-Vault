# Arduino Functions Reference

**Tags:** #reference #functions #API

---

## Pin Control

```cpp
pinMode(pin, mode);
// mode: INPUT, INPUT_PULLUP, OUTPUT
// Call in setup() before using any pin

digitalWrite(pin, value);
// value: HIGH (5V) or LOW (0V)
// Pin must be set OUTPUT

int val = digitalRead(pin);
// Returns HIGH or LOW
// Works on INPUT and OUTPUT pins

int val = analogRead(pin);
// pin: A0–A5
// Returns 0–1023 (10-bit ADC, 0V–5V)
// Takes ~100 microseconds

analogWrite(pin, value);
// pin: PWM-capable only: 3, 5, 6, 9, 10, 11
// value: 0–255 (duty cycle)
// Does NOT output true analog voltage
```

---

## Timing

```cpp
unsigned long t = millis();
// Milliseconds since Arduino powered on
// Returns unsigned long
// Overflows to 0 after ~49 days
// Safe with unsigned long arithmetic

unsigned long t = micros();
// Microseconds since Arduino powered on
// Returns unsigned long
// Overflows to 0 after ~70 minutes
// Does NOT increment inside ISR

delay(ms);
// Halts ALL execution for ms milliseconds
// AVOID in multi-task projects
// Acceptable only in setup() for initialization

delayMicroseconds(us);
// Halts execution for us microseconds
// For precise short pulse timing
```

---

## Sound

```cpp
tone(pin, frequency);
// Generates square wave at frequency Hz
// Runs until noTone() called
// Uses Timer2 — disables PWM on pins 3 and 11
// Only one tone at a time

tone(pin, frequency, duration);
// Plays for duration ms then stops
// Non-blocking — loop continues during playback

noTone(pin);
// Stops tone on specified pin
```

---

## Serial Communication

```cpp
Serial.begin(baudRate);
// Must be called first in setup() if using Serial
// Common: 9600, 115200

Serial.print(value);
// Prints without newline
// Accepts int, float, char, String, char[]

Serial.println(value);
// Prints with newline
// Serial.println(temp, 1) → one decimal place

Serial.print(F("text"));
// Stores string in Flash — zero SRAM cost
// Use on ALL string literals

int n = Serial.available();
// Returns number of bytes waiting in receive buffer
// Check before reading

byte b = Serial.read();
// Reads one byte from buffer
// Returns -1 if buffer empty

int x = Serial.parseInt();
// Reads and returns next integer from buffer
```

---

## Math and Scaling

```cpp
long map(value, fromLow, fromHigh, toLow, toHigh);
// Scales value from one range to another
// Does NOT clamp — use constrain() after
// Takes long integers — cast float to (int) explicitly

int constrain(value, min, max);
// Clamps value to [min, max]
// Always use after map()

int abs(value);
// Returns absolute value

long min(a, b);
// Returns smaller of two values

long max(a, b);
// Returns larger of two values
```

---

## Interrupts

```cpp
attachInterrupt(digitalPinToInterrupt(pin), ISR, mode);
// pin: 2 or 3 on Uno (hardware interrupt pins only)
// ISR: function to call — must be void, no parameters
// mode: RISING, FALLING, CHANGE, LOW

detachInterrupt(digitalPinToInterrupt(pin));
// Disables interrupt on specified pin
```

---

## EEPROM

```cpp
#include <EEPROM.h>

EEPROM.write(address, value);
// Writes one byte (0–255) to address
// ~100,000 write cycles per address — do not write every loop

byte val = EEPROM.read(address);
// Reads one byte from address

EEPROM.put(address, data);
// Writes any data type (float, int, struct)

EEPROM.get(address, variable);
// Reads any data type into variable
```

---

## Servo Library

```cpp
#include <Servo.h>

Servo myServo;

myServo.attach(pin);
// Connects servo to pin
// Disables analogWrite() on pins 9 and 10 (Timer1)

myServo.write(angle);
// Moves servo to angle (0–180 degrees)
// Accepts int

myServo.detach();
// Releases pin, restores PWM capability
```

---

## Type Sizes Reference

| Type           | Bytes | Range          |
| -------------- | ----- | -------------- |
| bool           | 1     | true/false     |
| byte / uint8_t | 1     | 0–255          |
| char           | 1     | -128–127       |
| int            | 2     | -32,768–32,767 |
| unsigned int   | 2     | 0–65,535       |
| long           | 4     | -2.1B–2.1B     |
| unsigned long  | 4     | 0–4.29B        |
| float          | 4     | ±3.4×10³⁸      |

---

## Related Notes

- [[Digital Pins]]
- [[Analog Pins]]
- [[PWM Pins]]
- [[millis Non-Blocking Pattern]]
- [[UART]]
- [[Interrupts]]
