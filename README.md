* TypeC-Debugger-F072 (DEBUGGER_V1.0)

USB-C (USB 2.0) STM32F072CBT6-based programmer/debug adapter board.

> Project status: Prototype

## What it does
- USB-C powered + USB 2.0 device interface
- Target programming/debug via **SWD**
- Dedicated reset control and debug access points
- On-board power section (labeled **POWER**) with switching and protection components

## Hardware overview
- MCU: **STM32F072CBT6**
- USB: **USB-C 2.0** (device/UFP configuration)
- Debug: **SWDIO / SWCLK / NRST / GND**
- Board: **DEBUGGER_V1.0**

## Connectors & headers
- **USB-C**: primary power + USB data connection
- **J2 (DEBUG header)**: main target interface header (SWD + reset + power reference)
- **J1 / J3 / S1 / S2**: board configuration/jumper/select features (see silkscreen)

## Test points (silkscreen labels)
- **TP2_3V3**: 3.3V rail test point
- **TP3_GND**: ground reference test point
- **TP4_NRST**: target reset test point
- **TP5_SWDIO**: SWD data test point
- **TP6_SWCLK**: SWD clock test point

## On-board power (POWER block)
Located on the left side of the board (near **SW1** and components marked U3/U4).
- Input comes from USB-C 5V
- Generates and distributes rails used by the MCU/debug interface
- Protection/fusing present (F1)

## Buttons / switches
- **RESET area (SW2)**: reset functionality (see silkscreen)
- **SW1 (POWER area)**: power-related switching (see silkscreen)

## Bring-up checklist
1. Inspect for shorts around USB-C, power, and the MCU.
2. Plug in USB-C and confirm:
   - **TP2_3V3** reads ~3.3V
   - **TP3_GND** is solid ground reference
3. Confirm reset behavior at **TP4_NRST**.
4. Verify SWD signals at **TP5_SWDIO** and **TP6_SWCLK**.
5. Connect target to **J2** and attempt SWD connection with your chosen firmware/protocol.

## Repo structure
- `hardware/` KiCad project, footprints, outputs
- `firmware/` programmer/debug firmware (protocol TBD)
- `images/` renders and board photos

## Notes
- This board is intended as a compact USB-C SWD debugger platform.
- Protocol/compatibility target (CMSIS-DAP, ST-Link-like, custom) is a firmware decision.

* BRINGUP.md (DEBUGGER_V1.0)

# Bring-up checklist for USB-C STM32F072 debugger board.

## Tools
- USB-C cable + host PC
- DMM (multimeter)
- Optional: USB power meter, bench supply w/ current limit, scope
- Optional: ST-Link or another known-good SWD debugger (for cross-checking target header wiring)

## Safety / first power-on setup
- If you have a bench supply: set **5.0V** with **current limit = 0.10A** (then raise later).
- If powering from USB: a USB power meter is helpful to spot obvious shorts (current spike).

## 1) Visual inspection (unpowered)
- Check component orientation: MCU, diode/LED, polarized caps (if any), USB-C connector.
- Look for solder bridges around:
  - STM32 pins
  - USB-C pins
  - Any fine-pitch parts near USB
- Verify the fuse (F1) is present and looks properly soldered.

## 2) Quick resistance checks (unpowered)
Using DMM ohms/continuity:
- **5V (VBUS) to GND**: should NOT be a short.
- **3V3 to GND** (TP2_3V3 to TP3_GND): should NOT be a short.
- If either reads near 0Ω, stop and inspect.

## 3) First power-on (power only)
Power the board from USB-C (or bench 5V if you have a safe injection point).

### Measure rails
- Measure **TP2_3V3 → TP3_GND**
  - Expected: **~3.3V**
- Measure any labeled VIN/VBUS node (if you have a test point) to GND
  - Expected: **~5.0V**

## 4) Reset line check
- Measure **TP4_NRST** relative to GND.
  - Expected: typically **high** (near 3.3V) when not pressed.
  - When reset is asserted (button/switch): should pull **low**.

## 5) USB enumeration check (data path)
Plug into a PC and verify the device shows up.
- macOS: System Information → USB
- Windows: Device Manager
- Linux: `lsusb`

Expected outcome (pre-firmware): may enumerate only if boot firmware/config supports it.
If it does NOT enumerate yet, that’s OK at this stage. Confirm 3V3 first.

## 6) SWD header continuity sanity (J2)
J2 pinout reference:
- Pin 1: +3V3
- Pin 2: SWDIO
- Pin 3: GND
- Pin 4: SWCLK
- Pin 5: GND
- Pin 10: NRST

Continuity checks (unpowered):
- J2 GND pins → TP3_GND should beep.
- J2 +3V3 (Pin 1) → TP2_3V3 should beep.
- J2 NRST (Pin 10) → TP4_NRST should beep.

## 7) Firmware bring-up (later)
Not implemented yet. The board is still in transit and firmware work will start after first power-on validation.
Planned direction: **CMSIS-DAP** (subject to change).

## Tools
- USB-C cable + host PC
- DMM (multimeter)
- Optional: USB power meter, bench supply w/ current limit, scope
- Optional: ST-Link or another known-good SWD debugger (for cross-checking target header wiring)

## Safety / first power-on setup
- If you have a bench supply: set **5.0V** with **current limit = 0.10A** (then raise later).
- If powering from USB: a USB power meter is helpful to spot obvious shorts (current spike).

## 1) Visual inspection (unpowered)
- Check component orientation: MCU, diode/LED, polarized caps (if any), USB-C connector.
- Look for solder bridges around:
  - STM32 pins
  - USB-C pins
  - Any fine-pitch parts near USB
- Verify the fuse (F1) is present and looks properly soldered.

## 2) Quick resistance checks (unpowered)
Using DMM ohms/continuity:
- **5V (VBUS) to GND**: should NOT be a short.
- **3V3 to GND** (TP2_3V3 to TP3_GND): should NOT be a short.
- If either reads near 0Ω, stop and inspect.

## 3) First power-on (power only)
Power the board from USB-C (or bench 5V if you have a safe injection point).

### Measure rails
- Measure **TP2_3V3 → TP3_GND**
  - Expected: **~3.3V**
- Measure any labeled VIN/VBUS node (if you have a test point) to GND
  - Expected: **~5.0V**

## 4) Reset line check
- Measure **TP4_NRST** relative to GND.
  - Expected: typically **high** (near 3.3V) when not pressed.
  - When reset is asserted (button/switch): should pull **low**.

## 5) USB enumeration check (data path)
Plug into a PC and verify the device shows up.
- macOS: System Information → USB
- Windows: Device Manager
- Linux: `lsusb`

Expected outcome (pre-firmware): may enumerate only if boot firmware/config supports it.
If it does NOT enumerate yet, that’s OK at this stage. Confirm 3V3 first.

## 6) SWD header continuity sanity (J2)
J2 pinout reference:
- Pin 1: +3V3
- Pin 2: SWDIO
- Pin 3: GND
- Pin 4: SWCLK
- Pin 5: GND
- Pin 10: NRST

Continuity checks (unpowered):
- J2 GND pins → TP3_GND should beep.
- J2 +3V3 (Pin 1) → TP2_3V3 should beep.
- J2 NRST (Pin 10) → TP4_NRST should beep.

## 7) Firmware bring-up (later)
Once hardware passes power + reset checks:
- Decide firmware direction (likely **CMSIS-DAP**)
- Confirm USB stack + clock config
- Validate SWD IO toggling on TP5_SWDIO / TP6_SWCLK
- Attempt a target attach via J2

## If something fails
- 3V3 missing: check regulator/power section, fuse, and shorts on 3V3 rail.
- 3V3 present but no USB enumerate: check CC resistors/config, D+/D- routing, ESD parts, USB connector soldering.
- Reset stuck low: check pull-ups, reset switch wiring, NRST net shorts.
- High current draw: inspect for solder bridges, reversed parts, or incorrect footprints.

## Notes log
Date:
Observations:
Fixes tried:

Open ibom.html to view the interactive BOM and highlighted placement.

Engineered and Designed by Brandon Shelly