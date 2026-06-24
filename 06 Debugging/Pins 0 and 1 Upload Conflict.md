# Pins 0 and 1 Upload Conflict

**Tags:** #debugging #upload #UART #pins

---

## Symptom

- Upload fails repeatedly with "programmer not responding"
- Upload worked before, now fails without any code change
- Removing a component from the breadboard makes upload work again

---

## Cause

Pins 0 (RX) and 1 (TX) are the **hardware UART lines** —
the same channel used to upload code from the IDE.

If any component is connected to these pins during upload,
it fights with the upload data stream:

```
IDE sends code → TX (pin 1) → USB converter → ATmega328P
                     ↑
              Your component is ALSO on TX,
              corrupting the data stream
```

The upload fails because the microcontroller receives garbled data.

---

## The Fix

```
1. Disconnect everything from pins 0 and 1 before uploading
2. After successful upload, reconnect components
3. OR: use different pins for your components
```

If you truly need pins 0/1 for a component (e.g., hardware serial
communication with another device), always disconnect during upload.

---

## Secondary Upload Failure Causes

If removing components from pins 0/1 does not fix the issue:

1. **Wrong COM port selected** — check Tools → Port in Arduino IDE
2. **Wrong board selected** — check Tools → Board
3. **Faulty USB cable** — try a different cable (data cable, not charge-only)
4. **Reset timing** — press Reset button just before upload starts
5. **Driver issue** — reinstall CH340 or FTDI drivers

---

## Prevention

- **Never** use pins 0 or 1 for buttons, LEDs, or sensors
- Reserve them exclusively for serial communication with
  external devices (GPS, Bluetooth, other Arduinos)
- Develop the habit of checking pin assignments before uploading

---

## Why This Happens — The Bootloader Window

When Arduino resets, the bootloader waits ~1 second for
incoming serial data. The IDE times the upload to hit this
window. Any signal on RX/TX during this window corrupts
the bootloader's interpretation of the upload.

See [[How Code Runs]].

---

## Related Notes

- [[How Code Runs]]
- [[UART]]
- [[Digital Pins]]
- [[Arduino Uno Anatomy]]
- [[Hardware Debugging Process]]
