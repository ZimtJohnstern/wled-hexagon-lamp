# Printables.com — Submission Package

This project isn't a modification of the hexagon frame geometry itself — it's a **controller/case add-on** (custom PCB + WLED firmware + power enclosure) that complements NickiDotDK's original hexagon model. Two ways to list it, pick based on how much you want to maintain there:

## Option A — Quick: post as a "Make" of the original model

Go to [NickiDotDK's Hexagon LED Lamp page](https://www.printables.com/model/1063657) → **"I Made This"** → upload photo/video, short text mentioning your custom WS2801/ESP32/WLED controller. Fast, low-maintenance, automatically credited and linked to the original — but no separate page for your PCB/case files.

## Option B — Own listing: new model, explicitly linked as related work

Create a **new model page**, upload only the files that are actually yours (`printables/*.3mf` — the enclosure), and reference the hexagon frames as a required companion print. This gives your PCB/enclosure its own visibility and license.

### Files to upload

From `printables/`:
- `Top.3mf` / `Top.stl`
- `Bottom.3mf` / `Bottom.stl`
- `Clamp_in.3mf` / `Clamp_in.stl`
- `Clamp_out.3mf` / `Clamp_out.stl`

Both formats are included — STL is often preferred on Printables for broader slicer compatibility, 3MF preserves any print settings/color info from Fusion 360. Upload both, or just STL if you want to keep the listing lean.

Do **not** re-upload the hexagon frame STLs — link to NickiDotDK's page instead (required by their CC BY-NC 4.0 attribution clause, and it's their work, not yours to redistribute as your own listing).

### Suggested title

**WLED Hexagon Lamp — ESP32/WS2801 Controller & Enclosure**

### Suggested summary (short)

Custom ESP32 + WS2801 controller PCB and 5V/10A power enclosure for NickiDotDK's Hexagon LED Lamp, running WLED.

### Suggested description (long, ready to paste)

> This is a controller/enclosure add-on for [NickiDotDK's Hexagon LED Lamp](https://www.printables.com/model/1063657) — **print that model first**, this listing only covers the electronics enclosure.
>
> Included:
> - 3D-printable enclosure (Top/Bottom/Clamp_in/Clamp_out) for a custom PCB + 5V/10A power supply
> - Full open-source repo with schematic, Gerber files, BOM, and WLED configuration: **[link your GitHub repo here]**
>
> Hardware: ESP32-WROOM-32 (30-pin), WS2801 LED strips (135 LEDs across 6 hexagons), 74AHCT125 level buffer, 5V/10A PSU.
>
> Full build guide (BOM, wiring, WLED setup, troubleshooting): see the GitHub repo linked above.
>
> **Credit:** hexagon frame design by NickiDotDK, licensed CC BY-NC 4.0. This controller/enclosure add-on is not affiliated with the original creator.

### Print settings (enclosure)

- Material: PLA (matches rails/corners of the base hexagon model) or PETG for extra heat tolerance near the power supply
- Infill: 15–20% (higher than the base model recommended, since it houses a mains-adjacent PSU)
- Supports: check per part in your slicer — not verified here, test print first

### License on Printables

Printables lets you pick a license per upload. To stay consistent with this project's GitHub license, select **CC BY-NC-SA 4.0** if offered, or the closest equivalent (CC BY-NC if SA isn't available) — matches [`LICENSE.md`](../LICENSE.md) in the repo.

### Tags

`wled` `esp32` `ws2801` `led` `hexagon` `honeycomb` `led-lamp` `addressable-led` `home-decor` `smart-lighting`

---

## Before you publish — checklist

- [ ] Link to NickiDotDK's original model is visible and prominent (attribution requirement of their CC BY-NC 4.0 license)
- [ ] GitHub repo link added to the description once pushed
- [ ] License selected on Printables matches `LICENSE.md`
- [ ] Photo/video uploaded (same cleaned files as `docs/media/`)
