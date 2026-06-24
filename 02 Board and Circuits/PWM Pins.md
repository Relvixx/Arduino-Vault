# PWM Pins

**Tags:** #hardware #pins #PWM #output

---

## What PWM Is

**Pulse Width Modulation** — a technique for simulating variable
analog output using digital switching.

Arduino's output pins can only produce 5V or 0V.
PWM solves this by switching the pin on and off so fast
that the component experiences the **average voltage**.

---

## Duty Cycle

The percentage of time the signal is HIGH in one cycle.
100% duty cycle → always ON  → full brightness

75% duty cycle → ON 3/4 time

50% duty cycle → ON half time → half brightness

25% duty cycle → ON 1/4 time

0% duty cycle → always OFF

`analogWrite(pin, 128)` = 50% duty cycle (128/255 ≈ 50%)

---

## Which Pins Support PWM

Only pins marked with **~** on the board:

**3, 5, 6, 9, 10, 11**

Calling `analogWrite()` on any other pin does nothing — no error,
just silence. This is a frequent debugging trap.

---

## PWM Frequencies

- Pins 5 and 6: ~**980 Hz**
- Pins 3, 9, 10, 11: ~**490 Hz**

Usually irrelevant. Matters if driving frequency-sensitive components.

---

## Timer Conflicts

PWM pins share hardware timers with other functions:

| Timer | Pins | Conflicts with |
|---|---|---|
| Timer0 | 5, 6 | millis(), delay() |
| Timer1 | 9, 10 | Servo library |
| Timer2 | 3, 11 | tone() function |

**Practical rule:** When using `tone()`, avoid PWM on pins 3 and 11.
When using `Servo.h`, avoid `analogWrite()` on pins 9 and 10.

---

## How to Use

```cpp
void setup() {
  pinMode(9, OUTPUT);     // not strictly required for analogWrite
}

void loop() {
  analogWrite(9, 0);      // fully off
  analogWrite(9, 128);    // ~50% — half brightness
  analogWrite(9, 255);    // fully on
}
```

---

## analogWrite() Does Not Output True Analog

It outputs a PWM square wave — switching rapidly between 0V and 5V.
For LEDs and motors: the component reacts to average power, so it
appears to dim or slow proportionally.
For precision analog circuits: this is not suitable.

---

## Related Notes

- [[Analog Pins]]
- [[Digital vs Analog Signals]]
- [[PWM Deep Dive]]
- [[Buzzer]]
- [[Servo Motor]]
- [[DC Motor]]
- [[map and constrain]]
