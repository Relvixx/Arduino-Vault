# P01 — Basic LED Blink

**Tags:** #project #LED #digital #output

---

## Goal

Make an external LED connected to pin 13 blink on and off
every second using `delay()`.

---

## Components

| Component | Quantity | Notes |
|---|---|---|
| Arduino Uno | 1 | |
| Breadboard | 1 | small |
| LED | 1 | any colour |
| Resistor | 1 | 220Ω |
| Jumper wires | 3 | |

---

## Wiring Logic

```
Arduino pin 13 → 220Ω resistor → LED anode (long leg)
LED cathode (short leg) → Arduino GND
```

---

## Code

```cpp
void setup() {
  pinMode(13, OUTPUT);
}

void loop() {
  digitalWrite(13, HIGH);   // LED on
  delay(1000);              // wait 1 second
  digitalWrite(13, LOW);   // LED off
  delay(1000);              // wait 1 second
}
```

---

## What Was Learned

- `pinMode()` declares pin direction before use
- `digitalWrite()` sends HIGH (5V) or LOW (0V)
- `delay()` pauses execution in milliseconds
- Every LED needs a current-limiting resistor
- Resistor calculation: R = (5V - 2V) / 0.02A = 150Ω → 220Ω

---

## What Could Be Improved

- `delay()` freezes the system — nothing else can run
- Later replaced by `millis()` non-blocking pattern
- See [[P08 Non-Blocking Blink and Button]]

---

## Related Notes

- [[LED]]
- [[Resistor]]
- [[Digital Pins]]
- [[Ohms Law]]
- [[millis Non-Blocking Pattern]]
