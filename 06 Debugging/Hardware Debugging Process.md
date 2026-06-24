# Hardware Debugging Process

**Tags:** #debugging #hardware #process

---

## The Rule

When a circuit does not work and there is no error message,
**never guess randomly.** Follow this sequence every time.
Start from the most fundamental cause and work toward the most complex.

---

## The Six-Step Sequence

### Step 1 — CHECK POWER
```
Is Arduino powered? Is the power LED on?
Is 5V reaching your breadboard power rail?
Measure with a multimeter if available.
```

### Step 2 — CHECK GROUND
```
Is GND connected?
Is it connected to the correct rail?
Is the GND rail connected from Arduino to breadboard?
```

> GND is the most common cause of total circuit failure.
> Check it before anything else.
> See [[Missing GND Bug]].

### Step 3 — CHECK COMPONENTS
```
Is the LED in the correct direction? (anode to + , cathode to –)
Is the TMP36 oriented correctly? (flat face toward you, left=5V)
Is the button connected on the correct terminals?
```

### Step 4 — CHECK CONNECTIONS
```
Is every wire in the correct breadboard row?
Are you crossing the center gap accidentally?
Left half (a–e) and right half (f–j) are NOT connected.
Are any wires loose or partially inserted?
```

### Step 5 — CHECK CODE
```
Does the pin number in code match the physical pin?
Is pinMode set correctly? (INPUT vs OUTPUT)
Is the logic correct? (INPUT_PULLUP means pressed = LOW)
Are you reading what you think you are reading?
Print values to Serial Monitor to confirm.
```

### Step 6 — ISOLATE
```
Disconnect half the circuit.
Test just the LED independently.
Add back one component at a time.
The fault reveals itself when behavior changes.
```

---

## Why This Order Matters

Skipping to code first is the most common beginner mistake.
It wastes hours before discovering the actual cause was
a missing GND wire — a 5-second fix.

Power and ground failures look identical to code errors
from the outside. Eliminate them first.

---

## Serial Monitor as Hardware Debugger

```cpp
Serial.print(F("Raw sensor: "));
Serial.println(analogRead(A0));
```

If the sensor reads 0 constantly — check power and GND to sensor.
If the sensor reads random values — check for floating input.
If the sensor reads a fixed wrong value — check orientation.

**Print the actual value. Never assume.**

---

## Related Notes

- [[Missing GND Bug]]
- [[Floating Pin Bug]]
- [[Wrong Resistor Calculation]]
- [[Pins 0 and 1 Upload Conflict]]
- [[Software Debugging Process]]
- [[UART]]
