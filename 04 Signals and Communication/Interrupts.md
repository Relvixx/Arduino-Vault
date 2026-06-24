# Interrupts

**Tags:** #signals #interrupts #ISR #timing

---

## The Problem Interrupts Solve

Normal `loop()` polling has a timing problem:

```cpp
void loop() {
  doSomethingFor500ms();   // button press here → MISSED
  doSomethingElse();       // button press here → MISSED
  checkButton();           // only checked here
}
```

If your loop takes 500ms and a button press lasts 200ms,
you will miss it entirely.

---

## What an Interrupt Does

You tell Arduino: *"No matter what you are doing, the moment
this pin changes — stop everything and run this function immediately."*

```cpp
void setup() {
  attachInterrupt(
    digitalPinToInterrupt(2),  // which pin
    buttonPressed,              // which function to call
    FALLING                     // when: HIGH→LOW transition
  );
}

void buttonPressed() {
  // runs INSTANTLY when pin 2 goes HIGH→LOW
  // regardless of what loop() is doing
}
```

---

## Interrupt Trigger Modes

| Mode | When it fires |
|---|---|
| `RISING` | Signal goes LOW → HIGH |
| `FALLING` | Signal goes HIGH → LOW |
| `CHANGE` | Any state change |
| `LOW` | While pin is LOW (fires continuously) |

`FALLING` is the standard mode for button presses with `INPUT_PULLUP`.

---

## Interrupt-Capable Pins on Uno

Hardware interrupts only work on **pins 2 and 3**.

This is not a software limitation — pins 2 and 3 are hardwired
to the interrupt system inside the ATmega328P.

---

## ISR Rules — Critical

The function called by an interrupt is the **ISR (Interrupt Service Routine).**

Rules that must not be broken:

Keep ISRs SHORT and FAST

→ The ISR blocks all other interrupts while running
Never use delay() inside an ISR

→ delay() depends on interrupts — it will freeze forever
Never use Serial.print() inside an ISR

→ Serial uses interrupts internally — undefined behavior
Never do complex calculations inside an ISR

→ Use a flag variable instead
Variables shared between ISR and main code

→ Must be declared volatile


---

## The Flag Pattern — Correct ISR Design

```cpp
volatile bool buttonFlag = false;   // volatile: tells compiler
                                    // this variable can change
                                    // outside normal program flow
void buttonISR() {
  buttonFlag = true;    // set flag only — minimal work
}

void loop() {
  if (buttonFlag) {
    buttonFlag = false;
    handleButtonPress();  // do the actual work here
  }
}
```

---

## millis() Inside ISRs

`millis()` stops incrementing inside an ISR because it depends
on a timer interrupt that is blocked while your ISR runs.

If you need timing inside an ISR: use `micros()` instead.
`micros()` keeps running because it uses a different timer register.

---

## Interrupts vs Edge Detection Polling

| Approach | Best for |
|---|---|
| Edge detection in `loop()` | Short loops, simple projects |
| Hardware interrupt | Long loops, time-critical response |

For most beginner/intermediate projects with fast loops,
edge detection in `loop()` is sufficient and simpler.

---

## Related Notes

- [[Edge Detection]]
- [[millis Non-Blocking Pattern]]
- [[Push Button]]
- [[The Clock]]
- [[P08 Non-Blocking Blink and Button]]
