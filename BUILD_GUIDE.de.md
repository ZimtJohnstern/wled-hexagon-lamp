# WLED Hexagon Lamp — Bauanleitung

*[English version](BUILD_GUIDE.md)*

Eine modulare 6-Hexagon-LED-Wandlampe auf Basis von [WLED](https://github.com/wled/WLED), WS2801-LED-Strips und einer selbstentwickelten ESP32-Platine.

![Fertige Lampe](docs/media/hexagon-lamp-photo.jpg)

<video src="docs/media/clip.mov" controls width="600">
  Dein Browser/Client kann das Video nicht direkt abspielen — <a href="docs/media/clip.mov">hier herunterladen</a>.
</video>

*(Falls das Video in deinem Markdown-Viewer nicht abspielt: Datei liegt unter `docs/media/clip.mov`, GitHub spielt sie im Repo-Viewer direkt ab.)*

---

## Was das ist

6 druckbare LED-Hexagone, honeycomb-artig zusammengesteckt, mit WS2801-Strips bestückt und über eine eigene ESP32-Platine mit WLED angesteuert. Zwei unabhängige Segmente ("Aussen"/"Innen") lassen sich getrennt mit Effekten/Farben belegen.

## Voraussetzungen

- 3D-Drucker (PLA für Rahmen/Ecken, PETG für Diffusoren empfohlen)
- Löterfahrung für die Platine (THT-Bauteile, keine SMD)
- Grundkenntnisse ESP32/Arduino-Umgebung bzw. Web-Flashen von WLED
- PCB-Fertigung extern (Gerber-Files liegen bei) oder eigene Lötplatine nach Schaltplan

---

## 1. Stückliste (BOM)

Vollständige BOM: [`hardware/pcb/BOM.txt`](hardware/pcb/BOM.txt)

| Ref | Bauteil | Menge | Wert/Typ |
|---|---|---|---|
| — | ESP32-WROOM-32 Devboard | 1 | 30-Pin |
| — | LED-Strip | 135 LEDs gesamt | WS2801, 5 V |
| — | Netzteil | 1 | 5 V / 10 A |
| C1, C2 | Elko | 2 | 470 µF |
| C3 | Kondensator | 1 | 100 nF |
| F1 | Sicherungshalter | 1 | 250 V / 5 A (Keystone 4527) |
| IC1 | Level-Buffer | 1 | 74AHCT125 |
| JP1, JP2 | Pfostenleiste 1×15 | 2 | Steckleisten für ESP32-Devboard |
| R1, R2 | Widerstand | 2 | 100 Ω |
| U$1 | Klemme 2-polig | 1 | Power In: V+/V− (5V/10A-Netzteil, über F1 abgesichert) |
| U$2 | Klemme 4-polig | 1 | LED-Ausgang: V+, Data, CLK, V− |

**Hinweis IC1 (74AHCT125):** HCT-Variante mit TTL-kompatiblem Eingang (verträgt 3,3V-Logik vom ESP32) und vollem 5V-CMOS-Ausgangshub — funktioniert als Pegelwandler zwischen ESP32 und WS2801. Auf dem Bauteil selbst bzw. im Handel teils nur generisch als "74125" gelistet — auf die **AHCT**-Variante achten, sonst kein sauberer High-Pegel am Strip.

---

## 2. Platine

- Schaltplan: [`hardware/pcb/schaltplan.pdf`](hardware/pcb/schaltplan.pdf)
- 3D-Modell (STEP): [`hardware/pcb/ESP32_WLED_WS2801.step`](hardware/pcb/ESP32_WLED_WS2801.step)
- Fertigungsdaten (Gerber/Drill/Assembly, fertig für JLCPCB/PCBWay o. ä.): [`hardware/pcb/CAMOutputs/`](hardware/pcb/CAMOutputs/)

**Bestückungshinweis:** JP1/JP2 sind die beiden 15-poligen Buchsenleisten für das ESP32-Devboard (30 Pin gesamt). U$1 nimmt die 5V-Versorgung vom Netzteil auf, U$2 gibt V+, Data, CLK und V− an den ersten WS2801-Strip weiter.

---

## 3. Gehäuse

3D-Druckdateien für das Netzteil-/Platinengehäuse:

- STEP (falls ihr's anpassen wollt): [`hardware/enclosure/WLED_Case_5V10A_ESP32.step`](hardware/enclosure/WLED_Case_5V10A_ESP32.step)
- Druckfertige 3MF-Dateien: [`Top.3mf`](hardware/enclosure/Top.3mf), [`Bottom.3mf`](hardware/enclosure/Bottom.3mf), [`Clamp_in.3mf`](hardware/enclosure/Clamp_in.3mf), [`Clamp_out.3mf`](hardware/enclosure/Clamp_out.3mf)
- Dieselben Teile als STL: [`Top.stl`](hardware/enclosure/Top.stl), [`Bottom.stl`](hardware/enclosure/Bottom.stl), [`Clamp_in.stl`](hardware/enclosure/Clamp_in.stl), [`Clamp_out.stl`](hardware/enclosure/Clamp_out.stl)

---

## 4. Hexagon-Rahmen drucken

Das eigentliche Hexagon-Modell stammt **nicht** aus diesem Projekt, sondern von [NickiDotDK auf Printables](https://www.printables.com/model/1063657) (Original: [`model-source/hexagon-led-lamp.pdf`](model-source/hexagon-led-lamp.pdf)).

- Rails/Ecken: PLA, 15% Infill, kein Support nötig
- Diffusoren: PETG
- LED-Strips: WS2801, 5 V, 5 LEDs pro Seite, aufgeklebt auf 10×2mm Aluprofile als Kühlkörper

⚠️ **Lizenz beachten:** Das Hexagon-Modell steht unter **CC BY-NC 4.0** — Namensnennung (NickiDotDK) Pflicht, keine kommerzielle Nutzung. Details: [`model-source/LICENSE-model.md`](model-source/LICENSE-model.md)

---

## 5. Verkabelung

- ESP32 Data → GPIO 23, Clock → GPIO 18 (über IC1 gepuffert)
- Nicht alle 6 Hexagone in einer Kette durchverkabeln — Spannungsabfall vermeiden. **Wichtig:** Sowohl Anfangs- als auch Endpunkt der LED-Kette müssen mit 5V gespeist werden (U$2 liefert V+, Data, CLK), nicht nur der Startpunkt — sonst fällt die Spannung zum Kettenende hin ab und die letzten LEDs werden dunkler/farbverfälscht.
- Details: [`docs/hardware.md`](docs/hardware.md)

---

## 6. WLED flashen & konfigurieren

1. WLED (Standard-Release oder eigenen Fork) auf den ESP32 flashen.
2. Web-UI → **Config → LED Preferences**:
   - LED-Typ: **WS2801 (SPI)**
   - Data-Pin: **23**, Clock-Pin: **18**
   - Farbreihenfolge: **BRG**
   - Anzahl LEDs: **135**
3. Segmente anlegen: "Aussen" (0–90), "Innen" (90–135)
4. Optional: fertige Konfiguration direkt importieren — [`firmware/wled_cfg_WLED.json`](firmware/wled_cfg_WLED.json) und [`firmware/wled_presets_WLED.json`](firmware/wled_presets_WLED.json) über WLED → Config → File Editor hochladen.

Details/Hintergrund zu den Werten: [`docs/wled-config.md`](docs/wled-config.md)

---

## 7. LED-Mapping

Damit Effekte der visuellen Hexagon-Anordnung folgen (statt der physischen Verkabelungsreihenfolge), liegt eine `ledmap.json` bei: [`firmware/ledmap.json`](firmware/ledmap.json). Unter WLED → Config → LED Preferences → "Ledmap" hochladen.

Referenzdiagramm zur Pixel-Nummerierung (Community-Ressource von Jake Peters, unabhängig vom Hexagon-Modell selbst): [`docs/media/ledmap-reference-jakepeters.png`](docs/media/ledmap-reference-jakepeters.png)

---

## 8. Presets

9 vorkonfigurierte Presets liegen bei (Solid, Chase, Aurora, Candle Multi, Lake, Plasma, Rocktaves, Shimmer, Tetrix), je mit getrennten Effekten für "Aussen"/"Innen": [`firmware/wled_presets_WLED.json`](firmware/wled_presets_WLED.json)

---

## Troubleshooting

- **Falsche Farben:** Farbreihenfolge in WLED prüfen (hier: BRG, nicht der Default GRB).
- **Kein Signal/Flackern:** IC1 muss die **74AHCT125**-Variante sein, nicht der einfache 74125 (fehlender sauberer 5V-High-Pegel).
- **Spannungsabfall am Ende der Kette:** Anfangs- und Endpunkt der Verkabelung beide mit 5V speisen (nicht nur den Anfang), ggf. Kabelquerschnitt erhöhen.

---

## Lizenz

- Hexagon-3D-Modell: [NickiDotDK](https://www.printables.com/model/1063657), CC BY-NC 4.0 — keine kommerzielle Nutzung, Namensnennung Pflicht
- Platine, Gehäuse, Firmware-Konfiguration dieses Projekts: **CC BY-NC-SA 4.0**, siehe [`LICENSE.md`](LICENSE.md) — kommerzieller Nachbau/Verkauf ausdrücklich nicht gestattet, privater Nachbau und nicht-kommerzielle Weitergabe mit Namensnennung erwünscht

## Credits

- Hexagon-3D-Modell: [NickiDotDK](https://www.printables.com/model/1063657)
- LED-Mapping-Referenzdiagramm: Jake Peters (Community-Ressource, WLED Hexagon Lamp)
- WLED-Firmware: [wled/WLED](https://github.com/wled/WLED) (MIT-Lizenz)
- Platine, Gehäuse, Firmware-Konfiguration: dieses Projekt
