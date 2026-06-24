# Circuit Design Checklist

**Tags:** #reference #circuits #design #checklist

---

## The Five Questions — Answer Before Every Build

Answer all five before placing any wire.

---

### Question 1 — POWER
```
What voltage does each component need?
Will 5V damage anything?
Does any component need 3.3V instead?
Is there a component requiring external supply (motor, relay)?
```

### Question 2 — CURRENT
```
How much current does each component draw?
Will any single pin exceed 40mA?
Will all active pins combined exceed 200mA?
Does anything need a transistor or driver IC?
```

### Question 3 — DIRECTION
```
Does polarity matter for this component?
LEDs: anode to + , cathode to –
TMP36: check flat face orientation before connecting
Electrolytic capacitors: + marked leg to higher voltage
```

### Question 4 — COMMUNICATION
```
Does this component need a special protocol?
I2C → uses A4 (SDA) and A5 (SCL)
SPI → uses dedicated MOSI/MISO/SCK/SS pins
UART → pins 0 and 1 — do NOT use during upload
```

### Question 5 — PINS
```
Which Arduino pins can this component use?
Does it need PWM? → must use 3, 5, 6, 9, 10, 11
Does it need analog input? → must use A0–A5
Does it need interrupt capability? → must use 2 or 3
Does it conflict with a timer or library?
```

---

## Pre-Build Wiring Checklist

```
[ ] All pin assignments decided before wiring
[ ] Resistor values calculated for all LEDs
[ ] GND connected from Arduino to breadboard rail
[ ] 5V connected to breadboard power rail (if needed)
[ ] Both power rails on breadboard connected
    (left side GND and right side GND are separate)
[ ] No component connected to pins 0 or 1
    (unless intentional serial device, disconnect for upload)
[ ] PWM pins used only where analogWrite() is needed
[ ] Servo on pin 9 or 10 → no analogWrite() on those pins
[ ] tone() planned → avoid PWM on pins 3 and 11
```

---

## Post-Build Debug Checklist (when nothing works)

```
[ ] Step 1: Arduino powered? Power LED on?
[ ] Step 2: GND connected to breadboard? (MOST COMMON FAILURE)
[ ] Step 3: Component polarity correct?
[ ] Step 4: Breadboard connections correct?
            (not crossing center gap accidentally)
[ ] Step 5: Pin number in code matches physical pin?
[ ] Step 6: pinMode() called for every used pin?
[ ] Step 7: Serial.print() added to verify actual values?
[ ] Step 8: Isolate — test components one at a time
```

---

## Resistor Quick Reference

| Situation | Resistor value |
|---|---|
| Red/yellow/green LED on 5V (20mA) | 220Ω |
| Blue/white LED on 5V (20mA) | 100–150Ω |
| Transistor base current limiting | 1kΩ |
| Pull-up or pull-down for button | 10kΩ |
| LDR voltage divider | 10kΩ |

---

## Related Notes

- [[Ohms Law]]
- [[Hardware Debugging Process]]
- [[LED]]
- [[Resistor]]
- [[Power Pins]]
- [[Breadboard Internals]]
