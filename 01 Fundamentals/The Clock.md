# The Clock

**Tags:** #fundamentals #hardware #timing

---

## What It Is

The Arduino Uno contains a **crystal oscillator** — a small silver
oval component on the board that vibrates at exactly **16 MHz**
due to its physical properties (like a tuning fork).

Each vibration = one clock cycle.
16 MHz = 16 million cycles per second.

---

## Why It Matters

The CPU executes instructions in sync with this clock.
Most instructions take 1–2 cycles.
Approximate throughput:
16 MHz / 2 cycles per instruction ≈ 8 million instructions/second

This is sufficient for:
- Reading sensors
- Controlling LEDs, motors, servos
- Running state machines
- Handling serial communication

It is NOT sufficient for:
- Video processing
- Complex audio synthesis
- Machine learning inference

---

## The Clock and Timing Code

`millis()` returns time in milliseconds since boot.
`micros()` returns time in microseconds.

Both depend on a **timer interrupt** — a hardware timer that increments
a counter every millisecond. This is why `millis()` stops counting
inside an ISR (the timer interrupt is blocked while your ISR runs).

See [[Interrupts]] and [[millis Non-Blocking Pattern]].

---

## Related Notes

- [[Microcontroller vs Computer]]
- [[millis Non-Blocking Pattern]]
- [[Interrupts]]
- [[How Code Runs]]
