# MiniCNC

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
3. Good bits: V-bits, 0.8 / 1.0 / 1.2 mm drills, 1 mm end mill for outlines
4. Spindle runout

### Software (planned)
- Firmware: GRBL
- CAM: KiCad -> G-code (e.g. FlatCAM)
- Sender: UGS / bCNC / CandleLight
