# CitrusCNC

A very small CNC mill with a 75 mm x 75 mm work area. It is built mainly for milling PCBs (mostly through-hole boards), but it can also cut other soft materials. The design is inspired by cheap 1610-style CNCs.

## Goals
- Work area: 75 x 75 mm
- Main use: isolation routing and drilling of simple THT PCBs
- Secondary use: wood, plastic, wax and other soft materials

## Notes so far

### Spindle
- Starting plan: 775 DC motor with an ER11 collet adapter.
- Weak points: adapter runout (0.05-0.2 mm typical), bearing quality, axial play, vibration, brush noise.
- Acceptable for THT boards (0.3-0.5 mm traces). Drilling 0.8-1.0 mm holes is where a loose spindle hurts most.
- Measure runout with a dial indicator. Aim for under about 0.03 mm.
- Design the spindle mount around a 52 mm clamp so a brushless spindle can be swapped in later.

### Priorities for PCB quality
1. Flat bed or spoilboard, or a probed height map (bCNC / UGS)
2. Z-axis rigidity and consistent depth
3. Good bits: 30 degree V-bits (holes and outline are done by hand)
4. Spindle runout

### Control board
All-in-one GRBL board with offline controller (SD card), bought from AliExpress.
- ATmega328P (Arduino Nano), GRBL, USB via CH340
- 3x A4988 stepper drivers: 2 A max, 1.5 A or less recommended
- MOSFET PWM spindle output, up to 10 A (spec sheet says up to 60 V, recommended within 48 V)
- Z-probe, limit switch and E-stop inputs
- The spec sheet is inconsistent on supply voltage (diagram says 24 V, text says 12 V steppers). Confirm with the seller before powering up.
- 775 motors are probably 12 V, so use a 12 V supply of 10 A or more, with an inline fuse.
- Add a small capacitor (about 100 nF) across the motor terminals to reduce noise that can false-trigger the probe or limit switches.

### Bits
Everything uses a 3.175 mm shank and a 3.175 mm ER11 collet.
- Isolation routing: 30 degree flat-bottom carbide V-bit, 0.2 mm tip (0.3 mm is sturdier). Buy 2-3.
- Pilot holes: a shallow plunge with a spare V-bit (60 or 90 degree is gentler), or a small 0.6-0.8 mm drill. Don't use the good isolation bit for this.
- Final holes and board outline are done by hand (drill and rotary tool), so no larger drills or end mills are needed for the CNC.
- Inspect new bits for a bent shank or off-center tip.

### Spindle shaft
The 775 shafts are round (not D-shaft). Plan: file a small flat on the shaft for the grub screw, use thread locker or retaining compound, then check runout with a dial indicator.

### Probing and zeroing
- Alligator clip on the bit (isolated from the motor circuit), another on the copper board, wired to the probe input and GND.
- Sand the copper lightly so it conducts. Probe before isolation routing, while the copper is still one connected sheet.
- Probe slowly (about 20-50 mm/min) to protect the tip.
- Probing sets Z only. Set X and Y zero by eye at the board corner.
- Use a sender with height mapping (UGS or bCNC).

### Frame
- Linear shafts are cheap 304 stainless rod. Cut to length with a cutoff disc or bi-metal hacksaw, then file and chamfer the ends.
- Check straightness (roll on a flat surface) and diameter (calipers) before use.
- Can be upgraded to hardened shafts later if the Z axis feels loose.

### Software (planned)
- Firmware: GRBL
- CAM: KiCad -> G-code (e.g. FlatCAM)
- Sender: UGS / bCNC / CandleLight
