# Hardware

## Stückliste (BOM)

Quelle: Export aus Fusion Electronics/EAGLE, Projekt `ESP32_WLED_WS2801` (`hardware/pcb/BOM.txt`).

| Ref | Bauteil | Menge | Wert/Typ | Package | Hersteller/Teilenr. |
|---|---|---|---|---|---|
| — | Board | 1 | — | — | — |
| C1, C2 | Elko | 2 | **470 µF** | CAPPRD200W80D500H1100B | — |
| C3 | Kondensator | 1 | 100 nF | C050-025X075 | — |
| F1 | Sicherungshalter | 1 | 250 V / 5 A | Keystone 4527 | 4527 |
| IC1 | Quad-Bus-Buffer, 3-state | 1 | **74AHCT125** (BOM-Text nennt "74125N", tatsächlich verbautes Bauteil ist die AHCT-Variante) | DIL14 | — |
| R1, R2 | Widerstand | 2 | 100 Ω | 0207/10 | — |
| U$1 | Klemmleiste, 2-polig | 1 | 2828XX-2282837-2 | TE Connectivity 282837-2 | **Power In: V+ / V−** vom 5V/10A-Netzteil, abgesichert über F1 (250V/5A) |
| U$2 | Klemmleiste, 4-polig | 1 | 2828XX-4282837-4 | TE Connectivity 282837-4 | **Ausgang zu WS2801: V+, Data, CLK, V−** |
| JP1, JP2 | Pfostenleiste | 2 | 1×15 | — | **Steckleisten für ESP32-Devboard, 30-Pin** (je 15 Pins pro Seite) |

Ergänzend (nicht Teil des EAGLE-Exports, aber Teil des Gesamtsystems):

| Bauteil | Wert/Typ |
|---|---|
| ESP32-WROOM-32 Devboard | 30-Pin |
| LED-Strip | WS2801, 5 V, 135 LEDs gesamt (verifiziert aus `wled_cfg_WLED.json`) |
| Netzteil | 5 V / 10 A |

**Korrektur ggü. erster Angabe:** Der Kondensatorwert ist **470 µF**, nicht 433 µF (Tippfehler in der Erstangabe, durch BOM-Export bestätigt).

**Zu IC1 (74AHCT125):** Echter, funktionierender Pegelwandler für diesen Anwendungsfall — TTL-kompatible Eingangsschwelle (VIH ≈ 2V, ESP32-3,3V-Logik liegt sauber darüber) bei vollem CMOS-Ausgangshub auf die IC-Versorgungsspannung (5V, sofern IC1 mit 5V versorgt wird). Das ist die verbreitete "Poor-Man's-Level-Shifter"-Lösung für WS2812/WS2801-Strips an 3,3V-MCUs.

## Pinbelegung ESP32

| Funktion | GPIO |
|---|---|
| WS2801 Data | 23 |
| WS2801 Clock | 18 |

## Verdrahtungshinweise

**Projektspezifisch (verifiziert):** U$2 liefert V+ (5V), Data und CLK für den WS2801-Strip. Beim Verdrahten der 6 Hexagone gilt: **Anfangspunkt und Endpunkt der Kette müssen beide mit 5V gespeist werden**, um Spannungsabfall über die gesamte Kette zu vermeiden — nicht nur am Anfang einspeisen.

**Zum Vergleich, Original-Community-Modell (NickiDotDK, andere Spannungslage):** Dessen Bauanleitung nutzt 12V-LED-Strips (nicht adressierbar, kein WS2801) und empfiehlt dort, bei 4 Hexagonen 3 einzelne Strips separat mit Strom zu speisen ("power infuse"). Das ist ein anderer LED-Typ mit anderer Spannung als in diesem Projekt (WS2801, 5V) — als Hintergrund erwähnt, aber nicht direkt auf dieses Setup übertragbar.

## Noch zu ergänzen

- Genaue Pin-für-Pin-Belegung JP1/JP2 gegen ESP32-DevKit-Pinout (welche der 30 Pins sind belegt, welche frei)
