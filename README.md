# 7-Inch Long Range HD UAV (ExpressLRS + Digital FPV + 6S Li-ion)

[![View PCB on KiCanvas](https://hack.club/pcb-badge)](https://kicanvas.org/?repo=https://github.com/adrish-manna/long-range-uav/tree/main/pcb)
![Tier](https://img.shields.io/badge/Stardance%20Tier-X--Tier%20($578)-purple.svg)
![Flight Controller](https://img.shields.io/badge/FC-Matek%20F722--SE-blue.svg)
![Video](https://img.shields.io/badge/Video-Caddx%20Nebula%20Pro%20Vista-orange.svg)

I designed and engineered an autonomous-capable 7-inch long-range UAV tailored for extended flight times (25+ minutes cruising) and crystal clear HD video transmission. 

Most consumer drones are locked down, rely on proprietary video downlinks, and run out of battery in 10–12 minutes. This UAV uses an **STM32F722-based flight controller**, a high-density **6S Molicel Li-ion chemistry pack**, and custom-fabricated 3D mounts printed in PETG and TPU to eliminate motor harmonic vibration.

---

## Technical Highlights

* **Endurance Propulsion:** 4x BrotherHobby Avenger 2207.5 1750KV motors spinning factory-molded HQProp 7040 tri-blade glass-fiber propellers. Driven by a 6S 4000mAh Molicel P42A pack for maximum cruising efficiency.
* **Low-Latency Digital HD:** The Caddx Nebula Pro Vista system delivers a 720p 120fps low-latency video feed directly to digital goggles.
* **Telemetry & Rescue Navigation:** The Matek SAM-M8Q GPS module tracks multi-constellation satellites (GPS, GLONASS, Galileo) for automatic GPS Rescue return-to-home fail-safes.
* **Transient Voltage Suppression:** Integrated 35V 470µF low-ESR electrolytic capacitor across the main 14AWG XT60 harness to filter high-voltage back-EMF motor surges.
* **Vibration Isolation:** Flight controller mounted on M3 silicone bobbins; FPV camera housed in a flexible TPU bracket to isolate jello from motor vibrations.

---

## System Wiring & Pinout

+-------------------------------+
|  6S 4000mAh Li-ion Battery    |
+---------------+---------------+
| (25.2V VBAT / XT60)
v
+-------------------------------------+
| Hobbywing XRotor Micro 45A 4-in-1   |
| - 4x Motor Phase Outputs (DShot600) |
| - 35V 470uF Low-ESR Capacitor       |
+------------------+------------------+
| (VBAT, Current, Telemetry, PWM 1-4)
v
+--------------------------------------------------------------+
|             Matek Systems F722-SE Flight Controller          |
|                   (STM32F722RET6 @ 216MHz)                   |
+-------+--------------------+---------------------+-----------+
| (UART1: TX1/RX1)   | (UART2: TX2/RX2)    | (UART3: RX3)
v                    v                     v
+-------------------+ +-------------------+ +-------------------+
| Caddx Vista VTX   | | Matek SAM-M8Q GPS | | RadioMaster RP1   |
| Digital HD Video  | | Telemetry / Rescue| | 2.4GHz ELRS RC    |
+-------------------+ +-------------------+ +-------------------+
---

## 3D Printed Parts Specification

Parts printed in PETG and TPU for mechanical mounting on the 7-inch carbon fiber chassis:

1. **FPV Camera Housing (`cad/fpv_camera_mount.step`):** 
   * **Material:** Flexible TPU (100% infill, 0.2mm layer height)
   * **Purpose:** Damps motor resonance and protects the Nebula Pro lens during high-G maneuvers.
2. **4-in-1 ESC Spacer Plate (`cad/esc_mount.step`):**
   * **Material:** PETG (30% infill, 4 perimeters)
   * **Purpose:** Isolates the high-current ESC traces from direct carbon fiber conductivity.
3. **GPS Tower Mount (`cad/gps_tower_mount.step`):**
   * **Material:** PETG (20% infill)
   * **Purpose:** Elevates the ceramic GPS patch antenna 45mm above the carbon deck and Li-ion pack to eliminate electromagnetic interference.
4. **Motor Arm Protectors (`cad/motor_mounts.step`):**
   * **Material:** PETG (30% infill)
   * **Purpose:** Guards the BrotherHobby motor bell bases and wire leads from ground contact.

---

## Assembly & Build Procedure

### 1. Fabrication & Dry Fit
* Print the TPU camera housing and PETG mounts on 3D printer.
* Inspect carbon fiber edges of the iFlight XL7 V2 frame and verify arm alignments.

### 2. Wiring & Soldering
* Tin motor phase pads on the 4-in-1 ESC and solder phase leads from all 4 BrotherHobby motors.
* Solder the main 14AWG XT60 battery lead with the 35V 470uF low-ESR capacitor directly across the battery positive and negative pads.
* Connect the ESC signal harness to the Matek F722-SE flight controller.
* Wire the Caddx Vista to 9V/2A BEC, GND, TX1, and RX1.
* Wire the SAM-M8Q GPS module to 4.5V, GND, TX2, and RX2.
* Wire the RadioMaster ELRS receiver to 4.5V, GND, and RX3.

### 3. Electrical Continuity Check
* Use a digital multimeter in continuity mode across the XT60 power rails to ensure zero shorts before connecting the 6S battery.

### 4. Firmware Configuration (INAV / Betaflight)
* Flash MatekF722SE target via USB-C.
* Configure DShot600 on motor outputs.
* Set Port 1 to MSP (Vista HD Display), Port 2 to GPS (UBLOX 115200 baud), and Port 3 to Serial RX (CRSF protocol for ELRS).
* Configure GPS Rescue mode with a minimum return altitude of 30m.
* Mount HQProp 7040 propellers and lock down securely with the M5 flanged Nyloc nuts.

---

## Bill of Materials Summary

Total Hardware Cost Requested: **$578.00** (Full itemized table in `BOM.csv`).
