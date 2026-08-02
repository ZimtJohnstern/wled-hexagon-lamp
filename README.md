# WLED Hexagon Project

*[Deutsche Version](README.de.md)*

![Finished lamp](docs/media/hexagon-lamp-photo.jpg)

6 clip-together LED hexagons (frame design: [Hexagon LED Lamp by NickiDotDK, Printables](https://www.printables.com/model/1063657), CC BY-NC 4.0), driven by WLED on an ESP32 with WS2801 strips and a custom PCB.

**→ Full build guide including video: [BUILD_GUIDE.md](BUILD_GUIDE.md)**

## Hardware overview

| Component | Value/Type |
|---|---|
| MCU | ESP32-WROOM-32, 30-pin devboard |
| LEDs | WS2801, 135 LEDs total, 6 hexagons |
| Power supply | 5 V / 10 A |
| Level buffer | IC1: SN74AHCT125N |
| Data pin (ESP32) | GPIO 23 |
| Clock pin (ESP32) | GPIO 18 |

Details, BOM and wiring: [`docs/hardware.md`](docs/hardware.md) · WLED configuration: [`docs/wled-config.md`](docs/wled-config.md)

*(Note: `docs/hardware.md` and `docs/wled-config.md` are currently German-only. Ask/open an issue if you'd like an English translation.)*

## Repo structure

```
WLED_Hexagone_Project/
├── README.md / README.de.md
├── BUILD_GUIDE.md / BUILD_GUIDE.de.md
├── LICENSE.md                   # CC BY-NC-SA 4.0 for board/enclosure/firmware config
├── .gitignore
├── docs/
│   ├── hardware.md              # BOM, pinout, wiring (German)
│   ├── wled-config.md           # WLED setup, verified config values, LED mapping (German)
│   └── media/
│       ├── hexagon-lamp-photo.jpg
│       ├── clip.mov
│       └── ledmap-reference-jakepeters.png
├── hardware/
│   ├── pcb/
│   │   ├── schaltplan.pdf       # schematic
│   │   ├── ESP32_WLED_WS2801.step
│   │   ├── BOM.txt
│   │   └── CAMOutputs/           # Gerber, drill, assembly (manufacturing data)
│   └── enclosure/
│       ├── WLED_Case_5V10A_ESP32.step
│       ├── Top.3mf / Bottom.3mf / Clamp_in.3mf / Clamp_out.3mf
│       └── Top.stl / Bottom.stl / Clamp_in.stl / Clamp_out.stl
├── model-source/
│   ├── LICENSE-model.md         # CC BY-NC 4.0 notice for the hexagon STL/STEP model
│   └── hexagon-led-lamp.pdf     # Original Printables product page export
├── printables/                  # Printables.com submission package
└── firmware/
    ├── ledmap.json
    ├── wled_cfg_WLED.json
    └── wled_presets_WLED.json
```

## WLED fork

This project uses a fork of [wled/WLED](https://github.com/wled/WLED) (official upstream, MIT license). Include the fork as a **git submodule** or a **separate repo** — see [`docs/wled-config.md`](docs/wled-config.md) for the recommended command.

## License

- Hexagon 3D model: [NickiDotDK](https://www.printables.com/model/1063657), CC BY-NC 4.0 — see `model-source/LICENSE-model.md`
- PCB, enclosure, firmware configuration of this project: **CC BY-NC-SA 4.0** — see [`LICENSE.md`](LICENSE.md). Commercial resale of replicas is not permitted; private builds and non-commercial sharing with attribution are welcome.
