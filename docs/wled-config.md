# WLED-Setup

## Fork einbinden

Zwei Optionen, je nachdem ob ihr eigenen Code in WLED ändern wollt oder nur konfiguriert:

**Option A — WLED als git submodule (empfohlen, wenn ihr WLED-Quellcode selbst anpasst):**
```bash
cd WLED_Hexagone_Project
git submodule add https://github.com/<euer-fork>/WLED.git firmware/WLED
git commit -m "Add WLED fork as submodule"
```

**Option B — nur Verweis/Fork-Link im Doc, kein Submodule** (ausreichend, wenn ihr WLED nur über die Web-UI konfiguriert, ohne Quellcode-Änderungen).

## Verifizierte Hardware-Konfiguration

Aus `firmware/wled_cfg_WLED.json` (Gerätename: `WLED_Buero`), Werte gegen den WLED-Quellcode (`wled00/const.h`) geprüft:

| Feld | Wert | Bedeutung |
|---|---|---|
| `type` | 50 | `TYPE_WS2801` — bestätigt korrekt |
| `pin` | [23, 18] | Data = GPIO 23, Clock = GPIO 18 |
| `order` | 2 | `COL_ORDER_BRG` — Farbkanalreihenfolge Blau-Rot-Grün |
| `total` | 135 | Gesamtzahl LEDs (nicht 180 — siehe Korrektur unten) |
| `ledma` | 60 | max. mA-Limit pro LED (60mA) |
| `maxpwr` | 8000 | Strombegrenzung gesamt: 8000 mA |
| `fps` | 42 | |

**Korrektur ggü. erster Schätzung:** 135 LEDs gesamt, nicht 6×6×5=180. Aus den Presets ersichtlich: Segment "Aussen" = 90 LEDs (Index 0–90), Segment "Innen" = 45 LEDs (Index 90–135). Bei der Honeycomb-Anordnung (gemeinsame Kanten zwischen benachbarten Hexagonen) ist die tatsächliche Seitenzahl niedriger als bei 6 vollständig getrennten Hexagonen.

## LED-Mapping

`firmware/ledmap.json` enthält die physische Pixel-Reihenfolge (`map`-Array, 135 Einträge, Index 0–134) — vermutlich zur Kompensation der Verkabelungsreihenfolge, damit Segmente/Effekte in der WLED-UI der visuellen Anordnung der Hexagone entsprechen statt der physischen Kabelreihenfolge.

`docs/media/ledmap-reference-jakepeters.png` ist ein Referenzdiagramm mit LED-Nummerierung von **Jake Peters** ("WLED Hexagon Lamp", eigenständiges Projekt/Community-Referenz) — das ist **nicht** derselbe Urheber wie das physische Hexagon-Modell (NickiDotDK, siehe `model-source/`). Beide Namen sollten in der Doku nicht vermischt werden, falls ihr das Repo veröffentlicht: NickiDotDK = 3D-Modell (CC BY-NC 4.0), Jake Peters = LED-Mapping-Referenz (Lizenz unbekannt, hier nicht mitgeliefert, nur als eigenes Foto/Screenshot referenziert).

## Presets

`firmware/wled_presets_WLED.json` enthält 9 gespeicherte Presets (Solid, Chase, Aurora, Candle Multi, Lake, Plasma, Rocktaves, Shimmer, Tetrix), jeweils mit eigenen Effekt-/Palette-Einstellungen pro Segment (Aussen/Innen getrennt ansteuerbar).

## Hardware-Konfiguration in der WLED-Web-UI (zum Nachvollziehen/Neuaufsetzen)

1. Config → LED Preferences
2. LED-Typ: **WS2801 (SPI)**
3. Data-Pin: **23**, Clock-Pin: **18**
4. Farbreihenfolge: **BRG**
5. Anzahl LEDs: **135**
6. Segmente: "Aussen" (0–90), "Innen" (90–135) — siehe Presets für exakte Werte

