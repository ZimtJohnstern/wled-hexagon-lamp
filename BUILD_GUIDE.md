# WLED Hexagon Lamp — Build Guide

*[Deutsche Version](BUILD_GUIDE.de.md)*

A modular 6-hexagon LED wall lamp based on [WLED](https://github.com/wled/WLED), WS2801 LED strips, and a custom ESP32 PCB.

![Finished lamp](docs/media/hexagon-lamp-photo.jpg)

<video src="docs/media/clip.mov" controls width="600">
  Your viewer can't play the video directly — <a href="docs/media/clip.mov">download it here</a>.
</video>

*(If the video doesn't play in your markdown viewer: it's at `docs/media/clip.mov` — GitHub plays it inline in the repo file viewer.)*

---

## What this is

6 printable LED hexagons, clicked together honeycomb-style, fitted with WS2801 strips and driven by a custom ESP32 PCB running WLED. Two independent segments ("Outer"/"Inner") can be driven with separate effects/colors.

## Prerequisites

- 3D printer (PLA for frame/corners, PETG for diffusers recommended)
- Soldering experience for the PCB (through-hole parts, no SMD)
- Basic ESP32/Arduino environment knowledge, or comfort flashing WLED via the web installer
- External PCB manufacturing (Gerber files included) or hand-soldering from the schematic

---

## 1. Bill of Materials (BOM)

Full BOM: [`hardware/pcb/BOM.txt`](hardware/pcb/BOM.txt)

| Ref | Part | Qty | Value/Type |
|---|---|---|---|
| — | ESP32-WROOM-32 devboard | 1 | 30-pin |
| — | LED strip | 135 LEDs total | WS2801, 5 V |
| — | Power supply | 1 | 5 V / 10 A |
| C1, C2 | Electrolytic cap | 2 | 470 µF |
| C3 | Capacitor | 1 | 100 nF |
| F1 | Fuse holder | 1 | 250 V / 5 A (Keystone 4527) |
| IC1 | Level buffer | 1 | 74AHCT125 |
| JP1, JP2 | Header 1×15 | 2 | Sockets for ESP32 devboard |
| R1, R2 | Resistor | 2 | 100 Ω |
| U$1 | 2-pin terminal | 1 | Power in: V+/V− (5V/10A supply, fused via F1) |
| U$2 | 4-pin terminal | 1 | LED output: V+, Data, CLK, V− |

**Note on IC1 (74AHCT125):** HCT variant with TTL-compatible input (tolerates the ESP32's 3.3V logic) and full 5V CMOS output swing — works as a level shifter between the ESP32 and the WS2801 strip. Often listed generically as just "74125" in stock/BOM exports — make sure you get the **AHCT** variant, otherwise you won't get a clean high signal at the strip.

---

## 2. PCB

- Schematic: [`hardware/pcb/schaltplan.pdf`](hardware/pcb/schaltplan.pdf)
- 3D model (STEP): [`hardware/pcb/ESP32_WLED_WS2801.step`](hardware/pcb/ESP32_WLED_WS2801.step)
- Manufacturing data (Gerber/drill/assembly, ready for JLCPCB/PCBWay etc.): [`hardware/pcb/CAMOutputs/`](hardware/pcb/CAMOutputs/)

**Assembly note:** JP1/JP2 are the two 15-pin sockets for the ESP32 devboard (30 pins total). U$1 takes the 5V supply from the power supply, U$2 outputs V+, Data, CLK and V− to the first WS2801 strip.

---

## 3. Enclosure

3D-print files for the power supply/PCB enclosure:

- STEP (if you want to modify it): [`hardware/enclosure/WLED_Case_5V10A_ESP32.step`](hardware/enclosure/WLED_Case_5V10A_ESP32.step)
- Ready-to-print 3MF files: [`Top.3mf`](hardware/enclosure/Top.3mf), [`Bottom.3mf`](hardware/enclosure/Bottom.3mf), [`Clamp_in.3mf`](hardware/enclosure/Clamp_in.3mf), [`Clamp_out.3mf`](hardware/enclosure/Clamp_out.3mf)
- Same parts as STL: [`Top.stl`](hardware/enclosure/Top.stl), [`Bottom.stl`](hardware/enclosure/Bottom.stl), [`Clamp_in.stl`](hardware/enclosure/Clamp_in.stl), [`Clamp_out.stl`](hardware/enclosure/Clamp_out.stl)

---

## 4. Print the hexagon frames

The hexagon frame model itself is **not** part of this project — it's from [NickiDotDK on Printables](https://www.printables.com/model/1063657) (original page saved as [`model-source/hexagon-led-lamp.pdf`](model-source/hexagon-led-lamp.pdf)).

- Rails/corners: PLA, 15% infill, no supports needed
- Diffusers: PETG
- LED strips: WS2801, 5 V, 5 LEDs per side, mounted on 10×2mm aluminum profiles as heatsinks

⚠️ **License:** The hexagon model is **CC BY-NC 4.0** — attribution to NickiDotDK required, no commercial use. Details: [`model-source/LICENSE-model.md`](model-source/LICENSE-model.md)

---

## 5. Wiring

- ESP32 Data → GPIO 23, Clock → GPIO 18 (buffered through IC1)
- Don't wire all 6 hexagons in one long daisy chain — voltage drop. **Important:** both the start and the end of the LED chain need to be fed 5V (U$2 supplies V+, Data, CLK), not just the start — otherwise voltage sags toward the end of the chain and the last LEDs go dim/off-color.
- Details: [`docs/hardware.md`](docs/hardware.md) *(German)*

---

## 6. Flash & configure WLED

1. Flash WLED (stock release or your own fork) onto the ESP32.
2. Web UI → **Config → LED Preferences**:
   - LED type: **WS2801 (SPI)**
   - Data pin: **23**, Clock pin: **18**
   - Color order: **BRG**
   - LED count: **135**
3. Set up segments: "Outer" (0–90), "Inner" (90–135)
4. Optional: import the ready-made config directly — [`firmware/wled_cfg_WLED.json`](firmware/wled_cfg_WLED.json) and [`firmware/wled_presets_WLED.json`](firmware/wled_presets_WLED.json) via WLED → Config → File Editor.

Background on these values: [`docs/wled-config.md`](docs/wled-config.md) *(German)*

---

## 7. LED mapping

So effects follow the visual hexagon layout instead of the physical wiring order, a `ledmap.json` is included: [`firmware/ledmap.json`](firmware/ledmap.json). Upload it under WLED → Config → LED Preferences → "Ledmap".

Reference diagram for pixel numbering (community resource by Jake Peters, independent of the hexagon model itself): [`docs/media/ledmap-reference-jakepeters.png`](docs/media/ledmap-reference-jakepeters.png)

---

## 8. Presets

9 preconfigured presets are included (Solid, Chase, Aurora, Candle Multi, Lake, Plasma, Rocktaves, Shimmer, Tetrix), each with separate effects for "Outer"/"Inner": [`firmware/wled_presets_WLED.json`](firmware/wled_presets_WLED.json)

---

## Troubleshooting

- **Wrong colors:** check the color order in WLED (here: BRG, not the default GRB).
- **No signal/flicker:** IC1 must be the **74AHCT125** variant, not the plain 74125 (no clean 5V high level otherwise).
- **Voltage drop at the end of the chain:** feed both the start and the end of the wiring with 5V, increase wire gauge if needed.

---

## Credits

- Hexagon 3D model: [NickiDotDK](https://www.printables.com/model/1063657), CC BY-NC 4.0 — no commercial use, attribution required
- LED mapping reference diagram: Jake Peters (community resource, WLED Hexagon Lamp)
- WLED firmware: [wled/WLED](https://github.com/wled/WLED) (MIT license)
- PCB, enclosure, firmware configuration: this project

## License

- Hexagon 3D model: [NickiDotDK](https://www.printables.com/model/1063657), CC BY-NC 4.0
- PCB, enclosure, firmware configuration of this project: **CC BY-NC-SA 4.0**, see [`LICENSE.md`](LICENSE.md) — commercial resale of replicas not permitted, private builds and non-commercial sharing with attribution welcome
