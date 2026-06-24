# Breadboard Internals

**Tags:** #hardware #circuits #breadboard

---

## What a Breadboard Is

A solderless prototyping board. Push components into the holes —
they connect electrically based on the board's internal copper strips.

---

## Internal Connection Pattern

════════════════════════════ +   ← power rail (full length)


════════════════════════════ -   ← ground rail (full length)
a b c d e   f g h i j

1 [■|■|■|■|■] [■|■|■|■|■]

2 [■|■|■|■|■] [■|■|■|■|■]

3 [■|■|■|■|■] [■|■|■|■|■]

↑               ↑

columns a–e    columns f–j

connected      connected

horizontally   horizontally


**The rules:**
- Holes in the **same row, same half** (e.g., 1a–1e) → electrically connected
- **Left half (a–e)** and **right half (f–j)** of the same row → NOT connected
- The center gap is intentional — for ICs that straddle both halves
- **Power rails** (+/–) run the full length of the board — vertically connected
- In Tinkercad: hover over a hole to see which others are connected to it

---

## Common Mistakes

| Mistake | Result |
|---|---|
| Component legs in different halves of same row | Not connected — circuit open |
| 5V and GND in same row | Short circuit — destroys components |
| LED anode/cathode reversed across the gap | Nothing — looks wired, isn't |
| Long wires jumping the wrong rows | Silent misconnection |

---

## Usage Habits

- Connect Arduino **5V** to the **+** power rail
- Connect Arduino **GND** to the **–** ground rail
- Use the rails to distribute power to all components
- Keep wire colors consistent: red = power, black/blue = ground

---

## Related Notes

- [[Ohms Law]]
- [[Series vs Parallel]]
- [[LED]]
- [[Push Button]]
- [[Missing GND Bug]]
