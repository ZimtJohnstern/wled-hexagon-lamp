# Hardware

## Stückliste (BOM)

Quelle: Export aus Fusion Electronics/EAGLE, Projekt `ESP32_WLED_WS2801` (`hardware/pcb/BOM.txt`).
## Stückliste / Bill of Materials (BOM)

Die folgende Tabelle listet die verwendeten Elektronikkomponenten für das Projekt auf. Die Links führen direkt zu den von mir verwendeten Artikeln auf AliExpress:

| Ref | Bauteil | Menge | Wert / Typ | Package / Info | Link |
| :--- | :--- | :---: | :--- | :--- | :--- |
| **—** | **Microcontroller** | 1 | ESP32 DevBoard | 30-Pin Version | [Auf AliExpress ansehen](https://aliexpress.com)* |
| **—** | **LED-Streifen** | 1 | WS2801 (5 Meter) | Digital steuerbarer LED-Strip (Data & CLK) | [Auf AliExpress ansehen](https://de.aliexpress.com/item/1726487236.html)* |
| **IC1** | **Quad-Bus-Buffer** | 1 | SN74AHCT125N | DIL14 (Pegelwandler für Datensignal) | [Auf AliExpress ansehen](https://aliexpress.com)* |
| **F1** | **Sicherungshalter** | 1 | 250 V / 5 A (bzw. 10 A) | Keystone 4527 / Inkl. Sicherung | [Auf AliExpress ansehen](https://aliexpress.com)* |
| **C1, C2** | **Elko (Kondensator)** | 2 | 470 µF | CAPPRD200W80D500H1100B (Aus Kit) | [Auf AliExpress ansehen](https://aliexpress.com)* |
| **C3** | **Kondensator** | 1 | 100 nF | C050-025X075 (Aus Metallkondensator-Kit) | [Auf AliExpress ansehen](https://aliexpress.com)* |
| **R1, R2** | **Widerstand** | 2 | 100 Ω | 0207/10 | [Auf AliExpress ansehen](https://aliexpress.com)* |
| **U$1** | **Klemmleiste 2-polig** | 1 | 2828XX-2282837-2 | Stromversorgung In: V+ / V- vom Netzteil | [Auf AliExpress ansehen](https://aliexpress.com)* |
| **U$2** | **Klemmleiste 4-polig** | 1 | 2828XX-4282837-4 | Ausgang zu WS2801: V+, Data, CLK, V- | [Auf AliExpress ansehen](https://aliexpress.com)* |
| **—** | **Netzteil** | 1 | 5V / 10A | Stromversorgung für das Gesamtsystem | [Auf AliExpress ansehen](https://aliexpress.com)* |

---

### 📢 Transparenzhinweis & Support
Die mit einem Sternchen (`*`) gekennzeichneten Produktlinks sind sogenannte **Affiliate-Links**. Wenn du über diese Links auf AliExpress einkaufst, erhalte ich eine kleine Provision vom Händler. Für dich entstehen dabei **absolut keine Mehrkosten**. Du unterstützt damit direkt die Weiterentwicklung und Pflege dieses Open-Source-Projekts. Vielen Dank!

(Links derzeit in Freigabe bei ALiExpress)

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

