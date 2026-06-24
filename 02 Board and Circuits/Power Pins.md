# Power Pins

**Tags:** #hardware #pins #power #voltage

---

## The Power Pin Group

Located on the bottom header of the Uno.

| Pin | What it does |
|---|---|
| **5V** | Regulated 5V output — powers components |
| **3.3V** | Regulated 3.3V output — for lower-voltage components |
| **GND** | Ground — 0V reference (two GND pins available) |
| **VIN** | Raw input voltage (7–12V) bypasses regulator |
| **RST** | Pulling LOW restarts the Arduino |

---

## GND — The Most Important Pin

Ground is not just "negative."

**Ground is the reference point** that makes every voltage
measurement meaningful. 5V only means 5V *relative to GND*.
Without GND:  no complete circuit → no current → nothing works

With GND:     current flows out of pin, through component,

back to GND — a complete loop

**GND is the first thing to check when a circuit does nothing.**
See [[Missing GND Bug]].

---

## Voltage Regulator

The black rectangular component near the power jack.
Converts messy 7–12V input to a clean, stable 5V.

**What it protects:** the board from input voltage variation.
**What it does NOT protect:** the pins from your wiring mistakes.

If you connect more than 5V to a pin → ATmega328P dies.
No error. No recovery. Permanent damage.

---

## 3.3V Components

Some sensors and modules operate at 3.3V:
- Connecting them to 5V can damage them permanently
- Always check the datasheet before connecting power

---

## Current Budget
5V pin max output:    ~500mA (from USB) or ~1A (from DC jack)

3.3V pin max output:  ~150mA

Per digital pin:      40mA max

All pins combined:    200mA max

For motors, relays, and high-brightness LEDs — use external power.
Arduino provides the control signal, not the drive current.
See [[DC Motor]].

---

## Related Notes

- [[Arduino Uno Anatomy]]
- [[Missing GND Bug]]
- [[Ohms Law]]
- [[DC Motor]]
- [[LED]]
