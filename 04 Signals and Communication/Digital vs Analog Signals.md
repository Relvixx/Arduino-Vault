# Digital vs Analog Signals

**Tags:** #signals #fundamentals #hardware

---

## Definition

### Digital Signal
Exists in only two states: HIGH (5V) or LOW (0V).
No intermediate values. Binary.

### Analog Signal
Can exist at any voltage between 0V and 5V.
Infinite possible values within the range.

---

## Visual Comparison
DIGITAL:

5V │‾‾‾│   │‾‾‾│

│   │   │   │

0V─┘   └───┘   └──

(on) (off)(on)(off)
ANALOG:

5V │   ╭──╮      ╭─

│  ╱    ╲    ╱

0V─┴─╯      ╲──╯

(smooth continuous wave)

---

## The Boundary Arduino Sits At

The real world is analog — temperature, light, sound all vary
continuously.

Computers and microcontrollers are digital — they think in 1s and 0s.

Arduino sits at the boundary:
- **Reads** the analog world via ADC (analog pins A0–A5)
- **Makes** digital decisions from those readings
- **Outputs** digital signals or PWM-simulated analog
Analog world → [ADC] → digital number → [decision] → digital output

---

## Noise Immunity

Digital signals are more immune to noise than analog.
A 4.8V signal still reads as HIGH (above ~3V threshold).
A 0.2V signal still reads as LOW (below ~1.5V threshold).

Analog signals are sensitive to every volt of noise.
This is why sensor readings need filtering.
See [[Moving Average Filter]].

---

## Which Pin Reads Which

| Signal type | Arduino pins |
|---|---|
| Digital input | 0–13 (with pinMode INPUT) |
| Digital output | 0–13 (with pinMode OUTPUT) |
| Analog input | A0–A5 (analogRead) |
| Analog output (simulated) | 3, 5, 6, 9, 10, 11 (analogWrite/PWM) |

---

## Related Notes

- [[Digital Pins]]
- [[Analog Pins]]
- [[PWM Deep Dive]]
- [[Moving Average Filter]]
- [[Arduino Uno Anatomy]]
