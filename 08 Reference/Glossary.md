# Glossary

**Tags:** #reference #glossary #definitions

---

## A

**ADC (Analog to Digital Converter)**
Hardware inside the ATmega328P that converts a continuous
voltage (0–5V) into a discrete number (0–1023).
The Uno's ADC is 10-bit: 2^10 = 1024 possible values.

**Active buzzer**
A buzzer with a built-in oscillator. Apply power → it beeps
at a fixed internal frequency. Controlled with `digitalWrite()`.
Does not respond to `tone()`.

**analogRead()**
Arduino function that reads a voltage on an analog pin (A0–A5)
and returns a value from 0 (0V) to 1023 (5V).

**analogWrite()**
Arduino function that outputs a PWM signal on a PWM-capable pin
(3, 5, 6, 9, 10, 11). Takes a value 0–255 representing duty cycle.
Does NOT output true analog voltage.

**Analog pin**
Pins A0–A5 on the Arduino Uno. Can read any voltage between
0V and 5V via the ADC. Input only for analog signals.
Can also function as digital I/O.

**ATmega328P**
The microcontroller chip on the Arduino Uno. Contains:
CPU, 32KB Flash, 2KB SRAM, 1KB EEPROM, ADC, timers,
UART, I2C, SPI — all on one chip.

**Anode**
The positive terminal of an LED (longer leg).
Current enters at the anode.

---

## B

**Baud rate**
Speed of serial communication in bits per second.
Both sides (Arduino and computer) must use the same rate.
Common value: 9600 baud.

**Bootloader**
A small program permanently stored in protected Flash memory.
Waits briefly at startup for an incoming upload.
If no upload arrives → runs your sketch.
If upload arrives → writes new sketch to Flash.

**Breadboard**
A solderless prototyping board. Components pushed into holes
connect internally by rows. Left and right halves of the same
row are NOT connected. Power rails run the full length.

---

## C

**Cathode**
The negative terminal of an LED (shorter leg).
Current exits at the cathode.

**Circular buffer**
A fixed-size array where new values overwrite the oldest.
Index wraps back to 0 using modulo (`%`).
Used in moving average filters.

**Clock cycle**
One tick of the crystal oscillator. At 16 MHz:
one cycle = 1/16,000,000 seconds = 62.5 nanoseconds.
Most ATmega328P instructions execute in 1–2 cycles.

**Compile**
The process of translating your C++ sketch into AVR machine
code (binary instructions the ATmega328P can execute directly).
Error messages come from the compiler, not from Arduino.

**constrain()**
Arduino function that clamps a value to a specified range.
Values below minimum → returns minimum.
Values above maximum → returns maximum.
Always use after `map()`.

**Crystal oscillator**
The small silver oval component on the Uno board.
Vibrates at exactly 16 MHz — provides the system clock.

---

## D

**Debouncing**
The process of eliminating false edge detections caused by
mechanical button contact bounce (~5–20ms of rapid oscillation
on press). Handled with a delay or millis()-based timing check.

**delay()**
Arduino function that halts all execution for a specified
number of milliseconds. Freezes sensors, buttons, outputs.
Replace with `millis()` in any project doing more than one thing.

**Diode**
A component that allows current to flow in one direction only.
An LED is a diode that emits light.
A flyback diode protects circuits from voltage spikes.

**digitalRead()**
Arduino function that reads the state of a digital pin.
Returns HIGH (5V) or LOW (0V).

**digitalWrite()**
Arduino function that sets a digital output pin to HIGH (5V)
or LOW (0V).

**Digital pin**
Pins 0–13 on the Arduino Uno. Can only exist in two states:
HIGH (5V) or LOW (0V). Max 40mA per pin.

**Duty cycle**
In PWM: the percentage of time the signal is HIGH in one
complete on/off cycle. 50% duty cycle = signal HIGH half the time.
`analogWrite(pin, 128)` = 50% duty cycle.

---

## E

**Edge detection**
A software pattern that detects the exact moment a signal
changes state (HIGH→LOW or LOW→HIGH) rather than its current state.
Fires once per transition, not continuously while in a state.

**EEPROM**
Electrically Erasable Programmable Read-Only Memory.
1KB on the Uno. Survives power-off. Limited to ~100,000
write cycles per address. Use only for settings/calibration
that must persist — never write every loop.

**enum**
C++ keyword that defines a set of named integer constants.
Used for state machine states:
`enum SystemState { NORMAL, BORDERLINE, ALERT_ACTIVE };`
Preferred over raw integers for readability.

**Event-driven**
A programming approach that responds to moments of change
(events) rather than continuously checking conditions.
Example: "button just pressed" vs "button is pressed."

---

## F

**F() macro**
Stores string literals in Flash memory instead of SRAM.
`Serial.println(F("Hello"))` — zero SRAM cost.
`Serial.println("Hello")` — consumes SRAM permanently.
Use on every string literal in Serial calls.

**Flash memory**
32KB non-volatile memory on the Uno.
Stores your compiled program. Cannot be written during execution.
Survives power-off. Bootloader occupies ~2KB of protected Flash.

**Floating pin**
An input pin with no defined connection — no pull-up, no pull-down,
no signal. Behaves as an antenna, reading random HIGH/LOW values.
Fix with `INPUT_PULLUP` or external pull resistor.

**Forward voltage (Vf)**
The voltage an LED consumes when conducting.
Red: ~2.0V. Blue: ~3.0V. White: ~3.2V.
Used in resistor calculations: R = (Supply - Vf) / I.

**FSM (Finite State Machine)**
A system that can be in one of a finite number of states,
transitions between states based on defined conditions,
and produces defined outputs or actions in each state.

---

## G

**GND (Ground)**
The 0V reference point for all voltage measurements.
Without GND, no circuit is complete and nothing works.
The first thing to check when a circuit does nothing.

**Global variable**
A variable declared outside all functions.
Visible to and modifiable by every function.
Persists for the entire program duration.
Lives in SRAM.

---

## H

**H-bridge**
An electronic circuit (often an IC like L298N) that allows
a DC motor to spin in both directions by controlling the
polarity of voltage applied to the motor terminals.

**Heap**
A region of SRAM used for dynamic memory allocation.
`String` objects and `malloc()` use the heap.
On Arduino, heap fragmentation causes crashes over time.
Avoid dynamic allocation.

**Hysteresis**
A technique using two separate threshold values (one for ON,
one for OFF) to prevent output flickering when a signal
hovers near a single threshold.
The gap between ON and OFF thresholds is the hysteresis band.

---

## I

**I2C (Inter-Integrated Circuit)**
A two-wire communication protocol (SDA + SCL).
Supports multiple devices on the same two wires using addresses.
On Uno: SDA = A4, SCL = A5.
Used by OLED displays, RTC clocks, IMU sensors.

**INPUT**
`pinMode()` mode that sets a pin as an input with high impedance.
Does not prevent floating — always use `INPUT_PULLUP` for buttons.

**INPUT_PULLUP**
`pinMode()` mode that sets a pin as input AND activates the
internal pull-up resistor (~20–50kΩ to 5V).
Unpressed button = HIGH. Pressed button = LOW.
Inverted logic compared to pull-down configuration.

**ISR (Interrupt Service Routine)**
A function called immediately when a hardware interrupt fires.
Must be short and fast. Cannot use `delay()` or `Serial`.
Variables shared with ISR must be declared `volatile`.

---

## L

**LDR (Light Dependent Resistor)**
A resistor whose resistance decreases as light increases.
Requires a voltage divider to convert resistance to readable voltage.
Bright light → low resistance → higher ADC reading.

**Local variable**
A variable declared inside a function.
Visible only within that function.
Created when the function is called, destroyed when it returns.

---

## M

**map()**
Arduino function that proportionally scales a value from
one range to another. Does NOT clamp — always follow with `constrain()`.
Takes `long` integer arguments — pass floats with explicit `(int)` cast.

**millis()**
Arduino function returning milliseconds since Arduino powered on.
Returns `unsigned long`. Overflows to 0 after ~49 days.
Unsigned arithmetic handles overflow correctly.
Use for non-blocking timing instead of `delay()`.

**Microcontroller**
A single chip containing CPU, memory (Flash, SRAM, EEPROM),
I/O pins, timers, and communication peripherals.
Designed for embedded, dedicated-purpose applications.
The ATmega328P is the microcontroller on Arduino Uno.

**Moving average filter**
A technique that reduces sensor noise by averaging the last N
readings. Uses a circular buffer for efficiency.
Trade-off: more samples = smoother but slower to respond.

---

## O

**Ohm's Law**
V = I × R. The fundamental relationship between voltage,
current, and resistance.
Rearranged: I = V/R (find current), R = V/I (find resistance).

**OUTPUT**
`pinMode()` mode that sets a pin as a digital output.
Pin actively drives HIGH (5V) or LOW (0V).
Max 40mA source or sink.

---

## P

**Passive buzzer**
A buzzer without an internal oscillator.
Requires an alternating signal at the desired frequency.
Controlled with `tone(pin, frequency)` and `noTone(pin)`.
Full pitch control — can play musical notes.

**pinMode()**
Arduino function that configures a pin as INPUT, INPUT_PULLUP,
or OUTPUT. Must be called in `setup()` before using the pin.

**Potentiometer**
A three-terminal variable resistor (knob or slider).
Acts as a voltage divider — wiper outputs 0V to VCC
proportional to position. Read with `analogRead()`.

**PROGMEM**
A directive that stores constant data (arrays, strings) in
Flash memory instead of SRAM.
`const int table[] PROGMEM = {0, 1, 2, ...};`
Read back with `pgm_read_word()`.
Used for large lookup tables to preserve SRAM.

**Pull-down resistor**
A resistor connecting an input pin to GND.
Ensures the pin reads LOW when no signal is applied.
Button pulled down: unpressed = LOW, pressed = HIGH.

**Pull-up resistor**
A resistor connecting an input pin to VCC.
Ensures the pin reads HIGH when no signal is applied.
Button pulled up: unpressed = HIGH, pressed = LOW.
Arduino has internal pull-ups (activate with INPUT_PULLUP).

**PWM (Pulse Width Modulation)**
A technique for simulating variable analog output by switching
a digital pin HIGH and LOW rapidly. The component experiences
the average voltage. PWM pins on Uno: 3, 5, 6, 9, 10, 11 (~).

---

## R

**Resistance**
Opposition to current flow. Measured in Ohms (Ω).
Resistors are components designed to provide specific resistance.

**Reset**
Restarting Arduino — runs `setup()` again, then `loop()` forever.
Triggered by: Reset button, power cycle, RESET pin pulled LOW,
or Watchdog Timer timeout.

---

## S

**SCL**
Serial Clock Line. Used in I2C communication.
Arduino Uno: A5.

**SDA**
Serial Data Line. Used in I2C communication.
Arduino Uno: A4.

**Serial Monitor**
A tool in the Arduino IDE (and Tinkercad) that displays data
sent from Arduino via `Serial.print()`.
Essential debugging tool — always print suspect variables.

**Single Responsibility Principle**
A function should do exactly one thing.
If you cannot describe it in one sentence without "and" — split it.

**SPI (Serial Peripheral Interface)**
A four-wire high-speed communication protocol.
Wires: MOSI, MISO, SCK, SS. One SS pin per device.
Used for SD cards, radio modules, fast displays.

**SRAM (Static Random Access Memory)**
2KB of volatile memory on the Uno.
Stores global variables, stack, heap during execution.
Lost when power is removed.
Most constrained resource on Arduino.

**Stack**
A region of SRAM used for local variables and function call
information. Grows downward. Collides with heap if both grow too large.

**State machine**
See FSM.

**static (keyword)**
Declares a local variable that persists between function calls
(global lifetime) but remains visible only within its function
(local scope). Initialized exactly once.

---

## T

**Timer**
A hardware counter in the ATmega328P that increments at a
fixed rate. Used internally by `millis()`, `delay()`, `tone()`,
`analogWrite()`, and `Servo.h`. Timer conflicts occur when
two features use the same timer.

**TMP36**
An analog temperature sensor. Output: 0.5V + (temperature × 0.01V).
Three pins: 5V, output, GND. Reversing power pins destroys it.
Formula: `tempC = (voltage - 0.5) * 100.0`

**tone()**
Arduino function that generates a square wave at a specified
frequency on a pin. Uses Timer2. Conflicts with PWM on pins 3 and 11.
Only one tone at a time.

**TX/RX**
Transmit / Receive. UART communication pins.
TX (pin 1): Arduino sends data.
RX (pin 0): Arduino receives data.
Do not connect components here during upload.

---

## U

**UART (Universal Asynchronous Receiver Transmitter)**
Serial communication protocol used by `Serial.print()`.
Two wires: TX and RX. Speed set by baud rate.
Used for USB communication and inter-device serial links.

**unsigned long**
32-bit unsigned integer type. Range: 0 to 4,294,967,295.
Required for `millis()` and `micros()` return values.
Handles rollover correctly via unsigned arithmetic.

---

## V

**Voltage divider**
A circuit with two resistors in series that produces an output
voltage proportional to the ratio of the resistors.
Used with sensors (LDR, thermistor) to convert resistance to voltage.

**volatile**
C++ keyword applied to variables shared between ISR and main code.
Tells the compiler not to cache the variable — always read from memory.
Required for any variable modified inside an ISR.

**Vf (Forward voltage)**
See Forward voltage.

**VIN**
The raw input voltage pin on Arduino (7–12V).
Bypasses the USB connection but passes through the voltage regulator.

---

## W

**Wire.h**
Arduino library for I2C communication.
Used internally by most I2C device libraries.
Direct use: `Wire.begin()`, `Wire.beginTransmission()`, `Wire.write()`.

---

## Related Notes

- [[Concept Index]]
- [[Best Practices]]
- [[Arduino Functions Reference]]
