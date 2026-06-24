# How Code Runs

**Tags:** #fundamentals #compilation #execution

---

## The Journey From Sketch to Execution
YOUR .ino FILE

│

▼

PREPROCESSOR

→ Handles #include, #define

→ Pastes library headers into your file

→ Resolves macros

│

▼

COMPILER (avr-gcc)

→ Translates C++ into AVR machine code

→ Checks syntax — error messages come from HERE

→ Produces a .hex binary file

│

▼

LINKER

→ Combines your code with library implementations

→ Resolves function references

→ Assigns memory addresses

→ Verifies everything fits in 32KB Flash

│

▼

UPLOADER (avrdude)

→ Sends .hex over USB/UART to Arduino

→ Bootloader receives and writes to Flash

│

▼

EXECUTION

→ ATmega328P reads Flash from address 0

→ Runs setup() once

→ Runs loop() forever

---

## The Bootloader

A small program permanently in protected Flash. Its only job:
Power on

│

▼

Wait ~1 second on RX pin

├── Upload incoming → write to Flash

└── Nothing → jump to your sketch and run it

This is why Arduino briefly pauses before your sketch starts.
This is why reset is sometimes needed during upload timing.
This is why you must not use pins 0/1 during upload — see
[[Pins 0 and 1 Upload Conflict]].

---

## No Operating System

There is no OS between your code and the hardware.

Consequences:
- Your code runs directly on the metal
- No multitasking — one thing at a time
- No memory protection — a bad pointer corrupts silently
- Full control of timing — deterministic behavior
- You must manage everything yourself

```cpp
void setup() {
  // Runs ONCE when power is connected
}

void loop() {
  // Runs FOREVER — no stopping unless reset
}
```

---

## Related Notes

- [[Arduino Code Structure]]
- [[Memory Types]]
- [[Pins 0 and 1 Upload Conflict]]
- [[The Clock]]
