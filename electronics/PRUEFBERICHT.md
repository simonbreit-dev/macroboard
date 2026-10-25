# Prüfung der TOP-USB-Revision – 17.10.2026

Die Platine bleibt **80 × 82 mm** groß. USB-C liegt an der Oberkante, Controller und seine lokalen Schaltungen rechts oben, beide Encoder rechts unten. Der Controllerkern wurde als zusammenhängende Gruppe gedreht; seine Versorgung, Flash- und Quarzwege sind erhalten. Peripherie und USB-Hauptverbindung wurden neu geführt und erneut geprüft. Gegenüber der früheren 80×122-mm-Version bleibt die Flächenersparnis von 32,8 % erhalten. Ein gefertigter Prototyp wurde nicht getestet.

## Datei- und Fertigungsprüfungen

| Prüfung | Ergebnis |
|---|---:|
| KiCad 10.0.7 ERC: Fehler / Warnungen / ausgeschlossene Einzelmeldungen | 0 / 0 / 0 |
| KiCad 10.0.7 DRC: Fehler / Warnungen | 0 / 0 |
| Offene Verbindungen | 0 |
| Schaltplan-/PCB-Abweichungen im DRC | 0 |
| Zusätzlicher Vergleich jedes Pad-Netzes mit der XML-Netzliste | 0 Abweichungen |
| Via innerhalb einer Bauteil-Lötfläche, beide Seiten einschließlich U4-Exposed-Pad | 0 |
| Via-Kupfer mit weniger als 0,10 mm Abstand zu einer Lötfläche | 0 |
| Vias ohne beidseitige Lötstopp-Abdeckung | 0 |
| Signalleitungen auf Innenlagen | 0 |

149 normale Durchkontaktierungen: 114 mit 0,450/0,200 mm, 35 mit 0,500/0,250 mm Durchmesser/Bohrung. Das Exposed Pad 57 ist eine geschlossene Kupfer-Lötfläche ohne Bohrungen. Die separaten Lötstopp-Öffnungen und 16 Pastenfenster sind erhalten; vier lokale Masse-Vias liegen außerhalb der Lötfläche. Kein Via-Füllen oder Kupferüberdecken vorgesehen.

Die DRC- und ERC-Ausnahmelisten sind leer. Die ERC-Prüfung wird einschließlich ausgeschlossener Einzelmeldungen ausgeführt. Projektweite deaktivierte Bibliotheks-/Darstellungsprüfungen sind in `checks/DRC.json` dokumentiert; insbesondere fehlende Courtyards und Footprint-Filter werden nicht geprüft. Elektrische Abstände und offene Verbindungen wurden nicht durch Ausnahmen unterdrückt. Der ERC-Bericht einschließlich Prüfung auf ausgeschlossene Einzelmeldungen liegt unter `checks/`; die Ergebnisse sind zusätzlich in `verification-summary.json` zusammengefasst.

## RP2040: Versorgung und lokale Kondensatoren

In1.Cu ist die +3V3-Fläche, In2.Cu die Massefläche. Beide Innenlagen enthalten keine Signalleitungen. U4, Flash, Quarz und alle Versorgungskondensatoren außer C20/C21 liegen auf Bottom. C20/C21 liegen auf Top direkt gegenüber ihren DVDD-Pins und sind jetzt 0805. Ihre elektrischen Werte bleiben 100 nF.

Die Längen unten sind geroutete Mittellinien einschließlich 1,0 mm für einen vollständigen Seitenwechsel durch die Platine, keine Luftlinienabstände. Sie sind eigene Messwerte, keine vom Guide vorgegebenen Grenzwerte.

| U4-Pin | Kondensator | Gerouteter Weg |
|---|---|---:|
| 44 VREG_IN | C24, 1 µF | 1,883 mm |
| 45 VREG_VOUT | C22, 1 µF | 1,993 mm |
| 50 DVDD | C20, 100 nF, Top | 3,600 mm einschließlich 1 Via |
| 23 DVDD | C21, 100 nF, Top | 3,174 mm einschließlich 1 Via |
| 48 USB_VDD / 49 IOVDD | C23, 100 nF gemeinsam | über +3V3-Fläche, siehe unten |
| 1 IOVDD | C25, 100 nF | 1,500 mm |
| 42 IOVDD | C26, 100 nF | 2,100 mm |
| 10 IOVDD | C27, 100 nF | 1,500 mm |
| 22 IOVDD | C28, 100 nF | 1,800 mm |
| 33 IOVDD | C29, 100 nF | 2,100 mm |
| 43 ADC_AVDD | C30, 100 nF | 2,572 mm |

C23 liegt auf Bottom ungefähr 2,97 / 2,64 mm Luftlinie von Pins 48/49 entfernt; beide Pins und C23 sind mit kurzen Anschlüssen an dieselbe +3V3-Innenfläche angebunden. Der Messalgorithmus berücksichtigt keine Wege durch Kupferflächen. Deshalb enthält die JSON-Datei für diese beiden Strecken `null`, obwohl der native Netz-/Verbindungsabgleich sie als verbunden bestätigt. Hier wird keine erfundene Leiterbahnlänge angegeben.

U1 AP2112K-3.3TRG1: C17 = 2,2 µF am Eingang und C18 = 10 µF am Ausgang, Anschlusslängen 1,363 / 1,263 mm. C31 = 10 µF puffert +3V3 lokal. VREG_VOUT versorgt nur +1V1 mit C22 und den DVDD-Abblockungen C20/C21. TESTEN Pin 19 und Pad 57 liegen an GND. VBUS ist von +3V3 und +1V1 getrennt.

## Flash, Boot und Quarz

U3 W25Q128JVSIQ liegt auf Bottom neben U4. C19 erreicht Flash-VCC über 1,600 mm; R1 = 1 kΩ liegt 2,025 mm von CS entfernt. R3 bleibt DNP. Die sechs QSPI-Verbindungen messen einschließlich Seitenwechsel 15,29–21,45 mm. Sie bleiben im Controllerbereich; eine übertaktete Flash-Konfiguration ist nicht erprobt.

X1 ABM8-272-T3: 12 MHz, CL 10 pF, ESR 50 Ω. C15/C16 je 15 pF, R4 = 1 kΩ. XIN: 4,362 mm. XOUT bis R4: 7,928 mm; R4 bis Quarz: 5,977 mm. Die Signale bleiben ohne Vias auf Bottom. Beide Quarz-Massepads sind angebunden.

BOOT ist über J2 zugänglich, RUN über TP3. TP1 = SWCLK, TP2 = SWDIO, TP4 = GND, TP5 = +3V3-Referenz. J2 ist optional, die Pads bleiben auch ohne eingelötete Stiftleiste nutzbar.

## USB-C

J1 liegt direkt an der Oberkante auf Bottom, mit Ursprung (165,0; 73,7) mm im PCB-Koordinatensystem. U4 liegt bei (171,5; 90,0) mm. D33 bleibt unmittelbar hinter der Buchse. Doppelte USB-C-Datenkontakte sind paarweise verbunden. CC1/CC2 haben je einen eigenen 5,1-kΩ-Widerstand R7/R8 gegen GND. D33 TPD4E05U06DQAR schützt Daten-/CC-Leitungen; seine optionalen Durchführungspads sind ausdrücklich in Symbol und Netzen abgebildet.

Die 27-Ω-Widerstände liegen am RP2040: R6–USB_DM 2,281 mm, R5–USB_DP 3,325 mm; beide chipseitigen Wege ohne Vias. Buchsenkontakt bis Schutzbaustein etwa 3,494 mm. Der geschützte Hauptweg zu den Widerständen misst D+ 26,983 mm, D− 27,993 mm. Breite im gekoppelten Abschnitt 0,225 mm, Abstand 0,150 mm; auf Bottom über In2.Cu-Masse, seitliche Kupferfüllung im Hauptkorridor gesperrt. Damit sind beide Hauptwege deutlich kürzer als die bisherigen ca. 45 mm. Die ungefähr 1 mm Längendifferenz entsteht in den lokalen Anschlussstrecken; diese Rechnung ist kein Konformitätsnachweis.

Die tatsächlich gefüllte In2.Cu-Masse wurde unter den Bottom-Datenbahnen abgetastet: 1.983 Prüfpunkte, keine fehlende Masse. 64 Punkte an lokalen USB-C-Kontakt-Via-Übergängen sind als bekannte Durchbrüche separat ausgenommen. Das ist eine geometrische Kontrolle, kein elektromagnetischer Feldlöser.

**Der Hersteller muss 90 Ω ±10 % anhand seines tatsächlichen Lagenaufbaus bestätigen.** Der nominelle 1-mm-Aufbau mit 0,1855 mm Abstand zur Referenzfläche ergibt in einer groben Näherung ca. 89,7 Ω. Eine andere Standarddicke darf nicht ungeprüft ersetzt werden. Fertigungsanforderungen stehen in `FABRICATION-NOTES.txt`.

## Anordnung, Bestückung und Firmware

Tastenraster 19,05 mm; drei Tasten oben, 3×3 darunter, 23 mm Abstand zur oberen Reihe. Encoder rechts unten übereinander, Mittelpunkte (170; 122,05) und (170; 141,10) mm. Controllerbereich rechts oben. D1 sitzt bei SW3 rechts oben; die Kette läuft dann schlangenförmig nach unten bis D12 bei SW12. `led-key-map.csv` enthält die vollständige Zuordnung.

69 SMD-/Hybrid-Bauteile in 19 BOM-Zeilen: **67 Bottom + 2 Top**. Für einseitige Fabrikbestückung sind C20/C21 aus der Fabrik-BOM entfernt, bleiben aber zwingend als zwei Top-Handlötteile erforderlich. Alternativ ist die vollständige beidseitige Fabrikbestückung vorbereitet. Beide Varianten haben getrennte BOM-/CPL-Ordner. Tasten, Encoder und optionaler J2 werden von Hand bestückt. Die LED-Ausschnitte und Durchsteckanschlüsse bleiben frei von Controllerbauteilen.

Die GPIO-Belegung bleibt gegenüber der letzten kompakten Version unverändert (unter anderem COL2 = GPIO18, COL3 = GPIO17, ROW2 = GPIO22). Die vollständige Zuordnung steht in `firmware-pinmap.csv`. Der Schaltplan bleibt elektrisch unverändert; die neue PCB wird dagegen abgeglichen. Nur die räumliche LED-Zuordnung hat sich geändert; dafür `led-key-map.csv` verwenden. Maschinen-Rotationskorrekturen, Lieferantenlagerbestand und Gehäusepassung sind nicht praktisch bestätigt.

## Quellen und verbleibender praktischer Nachweis

Abgleich mit [Raspberry Pi, Hardware design with RP2040, Kapitel 2.1–2.4](https://datasheets.raspberrypi.com/rp2040/hardware-design-with-rp2040.pdf): lokale Versorgungskondensatoren und interne Reglerversorgung, Flash-/BOOT-Anbindung, 12-MHz-Quarzbeschaltung, USB-Serienwiderstände und Referenzfläche. Die alternative LDO-Versorgung wurde mit dem [AP2112-Datenblatt](https://www.diodes.com/assets/Datasheets/AP2112.pdf) abgeglichen; ESD-Durchführungspads nach [TI TPDxE05U06](https://www.ti.com/lit/ds/symlink/tpd4e05u06.pdf).

Die angeführten Prüfungen belegen Dateikonsistenz und Layoutgeometrie. Einschaltverhalten, ROM-Boot, Flashen, USB-Kommunikation und Tasten-/Encoder-/LED-Funktion müssen am ersten aufgebauten Board mit angepasster Firmware geprüft werden. Eine Funktionsgarantie oder USB-Zertifizierung lässt sich daraus nicht ableiten.
