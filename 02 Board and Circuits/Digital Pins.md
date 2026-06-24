# Digital Pins

**Tags:** #hardware #pins #digital

---

## Definition

Digital pins can only exist in one of two states:

| State | Voltage | Arduino constant |
|---|---|---|
| HIGH | 5V | `HIGH` or `1` |
| LOW | 0V | `LOW` or `0` |

Nothing in between. Binary. Like a light switch.

---

## Pins Available

Pins **0 through 13** on the Arduino Uno.
14 digital pins total.

---

## How to Use

```cpp
// In setup():
pinMode(13, OUTPUT);   // this pin will SEND signals
pinMode(2, INPUT);     // this pin will RECEIVE signals

// In loop():
digitalWrite(13, HIGH);        // send 5V
digitalWrite(13, LOW);         // send 0V
int state = digitalRead(2);    // read HIGH or LOW
```

---

## Special Digital Pin Facts

### Reading an OUTPUT pin is valid
```cpp
// Toggle without a state variable:
digitalWrite(13, !digitalRead(13));
```
`digitalRead()` on an OUTPUT pin returns what the pin is currently driving.

### HIGH and LOW are just integers
```cpp
HIGH == 1   // true
LOW  == 0   // true
```
`digitalWrite(pin, true)` and `digitalWrite(pin, 1)` both work.
Use the named constants for readability.

---

## Current Limits

- **Max per pin: 40mA** (source or sink)
- **Max total all pins: 200mA**
- A standard LED needs 20mA — always use a resistor
- Two LEDs on one pin hits the 40mA ceiling exactly
- Anything drawing more than 40mA needs a transistor buffer

See [[LED]], [[Resistor]], [[DC Motor]].

---

## Pins 0 and 1 — Special Case

Pins 0 (RX) and 1 (TX) are the hardware UART lines.
**Never connect components to these pins while uploading code.**
The upload uses the same channel — anything on pins 0/1 blocks
the upload with no clear error message.

See [[Pins 0 and 1 Upload Conflict]] and [[UART]].

---

## Related Notes

- [[Analog Pins]]
- [[PWM Pins]]
- [[Arduino Uno Anatomy]]
- [[Push Button]]
- [[LED]]
- [[Pins 0 and 1 Upload Conflict]]
