# Missing GND Bug

**Tags:** #debugging #circuit #GND #power

---

## Symptom

- Circuit does nothing at all
- No component shows any sign of life
- Code appears correct
- Power is connected
- No error message of any kind

---

## Cause

Ground (GND) is missing from the circuit or disconnected.

Without GND, there is no complete circuit loop.
Current cannot flow — not because the components are broken,
but because there is nowhere for it to return to.

---

## Understanding Why GND is Required

Voltage is a **difference** between two points.
5V is only meaningful relative to a 0V reference point — GND.

```
Without GND:
5V pin → resistor → LED → ??? (nowhere to go)
No current flows. LED does not light.

With GND:
5V pin → resistor → LED anode → LED cathode → GND
Complete loop. Current flows. LED lights.
```

---

## The Fix

```
1. Connect Arduino GND pin to breadboard GND rail (– rail)
2. Connect all component negative terminals to GND rail
3. If using external power: share GND between Arduino 
   and external supply
```

---

## Prevention

**GND is the first thing to check when a circuit does nothing.**

This rule is stated in [[Hardware Debugging Process]] Step 2.
Check it before touching the code. Before checking component
orientation. Before anything else.

Checklist:
- [ ] Arduino GND → breadboard – rail?
- [ ] All LED cathodes → GND?
- [ ] All sensor GND pins → GND?
- [ ] Buzzer GND → GND?
- [ ] Button one terminal → GND? (for pull-up configuration)

---

## A Subtle GND Mistake

In Tinkercad and on real breadboards: the GND rail on the
left side and the GND rail on the right side are NOT
automatically connected to each other.

If you are using both sides of the breadboard, connect
the GND rails together with a wire.

---

## Related Notes

- [[Hardware Debugging Process]]
- [[Breadboard Internals]]
- [[Power Pins]]
- [[Arduino Uno Anatomy]]
