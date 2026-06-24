# Buzzer

**Tags:** #component #output #sound

---

## Two Types — Completely Different Behavior

| Feature | Active Buzzer | Passive Buzzer |
|---|---|---|
| Internal oscillator | Yes | No |
| To operate | Apply power | Send frequency signal |
| Tone control | Fixed — one tone only | Full pitch control |
| Arduino function | `digitalWrite()` | `tone()` |
| Identification | Usually has a sticker over the hole | Hole is open |

---

## Active Buzzer

Apply 5V → it beeps at its fixed internal frequency.
Remove power → silence.

```cpp
digitalWrite(buzzerPin, HIGH);   // beep
digitalWrite(buzzerPin, LOW);    // silence
```

**Advantage:** Simple. No timer conflicts.
**Limitation:** One pitch only. Cannot play notes.

---

## Passive Buzzer

Requires an oscillating signal at the desired frequency.
`tone()` generates this signal:

```cpp
tone(pin, frequency);              // start playing
tone(pin, frequency, duration);    // play for duration ms
noTone(pin);                       // stop
```

Frequency is pitch:
- Middle C = 262 Hz
- Concert A = 440 Hz
- Higher number = higher pitch

**Timer conflict:** `tone()` uses Timer2 internally.
This **disables PWM on pins 3 and 11** for as long as tone plays.
Move LEDs needing analogWrite away from those pins.

**Only one tone at a time.** Calling `tone()` on a second pin
stops the first — no polyphony without external hardware.

**tone() is non-blocking.** With the duration argument,
it returns immediately and plays in the background.
Your loop continues during playback.

---

## Non-Blocking Beep Pattern (Active Buzzer)

For repeating patterns without `delay()`, use a state machine
with `millis()`:

```cpp
// beepStep tracks position in pattern
// lastBuzzerTime tracks when step started
// See P12 Smart Environment Monitor for full implementation
```

See [[millis Non-Blocking Pattern]] and [[State Machines]].

---

## Common Mistakes

| Mistake | Result |
|---|---|
| Using `tone()` on active buzzer | Erratic — it has internal oscillator |
| Using `digitalWrite()` on passive buzzer | No sound or click only |
| `tone()` while using PWM on pin 3 or 11 | PWM stops working |
| Using `delay()` for beep timing | Freezes entire system |
| Not resetting `lastBuzzerTime` on state entry | First beep is silent |

The last mistake is documented in detail at [[Buzzer Timing Bug]].

---

## Related Notes

- [[Buzzer Timing Bug]]
- [[millis Non-Blocking Pattern]]
- [[State Machines]]
- [[PWM Pins]]
- [[P07 Thermometer with Buzzer]]
- [[P12 Smart Environment Monitor]]
