# EDTV4 Bedienungsanleitung

**Öffentliche Revision - Stand 13. Oktober 2026**

EDTV4 ist eine großformatige, geschlossene IDEX-FFF-Versuchsplattform der FH Oberösterreich, Campus Wels. Diese Anleitung beschreibt den grundlegenden Betrieb der fertiggestellten Maschine und ersetzt keine persönliche Einschulung am Gerät.

> **Wichtig:** Für installierte Firmwarewerte und konfigurierte Maschinenlimits ist die aktuelle [`printer.cfg`](../../firmware/klipper/printer.cfg) maßgeblich. Die hier genannten Grenzwerte sind keine allgemeinen Material- oder Prozessempfehlungen.

## 1. Systemübersicht

| Bereich | Aktueller Stand |
|---|---|
| Druckprinzip | FFF / IDEX |
| Steuerung | BIGTREETECH Octopus Pro v1.1 |
| Host | Raspberry Pi 4 |
| Firmware | Klipper |
| Benutzeroberfläche | Mainsail / lokaler Touchscreen |
| Slicer | OrcaSlicer |
| X-System | Zwei unabhängige X-Schlitten |
| Y-System | Zwei Y-Motoren |
| Z-System | Zwei Z-Antriebe mit Z-Tilt-Korrektur |
| Bettvermessung | 9 × 9, bicubic |
| Filamentüberwachung | Filamentschalter + Bewegungssensor |
| Sicherheitsfunktion | Hardware-Not-Halt |

### 1.1 Konfigurierte Firmware-Grenzwerte

| Parameter | Konfigurierter Wert |
|---|---:|
| E0 X-Verfahrweg | -59,5 ... 475 mm |
| E1 X-Verfahrweg | 35 ... 560,29 mm |
| Y-Verfahrweg | -46 ... 505 mm |
| Z-Verfahrweg | -2 ... 650 mm |
| Maximale XY-Geschwindigkeit | 300 mm/s |
| Maximale Beschleunigung | 1000 mm/s² |
| E0 maximale konfigurierte Temperatur | 310 °C |
| E1 maximale konfigurierte Temperatur | 300 °C |
| Druckbett maximale konfigurierte Temperatur | 130 °C |
| Druckraumheizung maximale konfigurierte Temperatur | 70 °C |

Die tatsächlichen Drucktemperaturen und Prozessparameter sind material-, düsen- und profilabhängig. Dafür sind die getesteten EDTV4-OrcaSlicer-Profile zu verwenden.

## 2. Vor dem Einschalten

Vor jedem Start:

1. Druckraum und Bewegungsbereich auf lose Teile oder Werkzeuge kontrollieren.
2. PEI-Druckoberfläche auf Sauberkeit und korrekten Sitz prüfen.
3. Beide Düsen auf Filamentreste oder starke Verschmutzung prüfen.
4. Abstreifer/Reinigungsbereich kontrollieren.
5. Sicherstellen, dass beide X-Schlitten frei verfahren können.
6. Prüfen, dass der Hardware-Not-Halt **nicht eingerastet** ist.

Wird Klipper mit aktivem Not-Halt gestartet, kann die Initialisierung fehlschlagen. In diesem Fall muss nach Beseitigung der Ursache Klipper bzw. die Maschine neu gestartet werden.

## 3. Einschalten

1. Not-Halt lösen, sofern die Maschine sicher eingeschaltet werden kann.
2. Hauptschalter einschalten.
3. Raspberry Pi und Klipper vollständig starten lassen.
4. Mainsail bzw. den lokalen Touchscreen öffnen.
5. Vor Bewegungen prüfen, ob Klipper, MCU, Sensoren und Heizungen ohne Fehlermeldung bereit sind.

Keine Achse manuell verfahren, solange ein Konfigurations-, MCU- oder Sensorfehler angezeigt wird.

## 4. Ausschalten

1. Aktiven Druck ordnungsgemäß beenden oder abbrechen.
2. Heizungen ausschalten und heiße Bauteile soweit erforderlich abkühlen lassen.
3. Host-System über die Klipper/Mainsail-Oberfläche herunterfahren.
4. Erst danach den Hauptschalter ausschalten.

Der Raspberry Pi soll im normalen Betrieb nicht durch direktes Abschalten der Netzversorgung heruntergefahren werden.

## 5. Filament wechseln

Der Filamentwechsel erfolgt manuell.

1. Benötigten Extruder auswählen.
2. Hotend auf eine zum vorhandenen und neuen Material passende Temperatur aufheizen. Bei Materialwechseln muss das höher schmelzende Material sicher verarbeitet werden können.
3. Altes Filament aus dem Extruder entfernen.
4. Neues Filament einfädeln und im Extruder greifen lassen.
5. Material extrudieren, bis das vorherige Polymer bzw. die vorherige Farbe vollständig aus der Düse entfernt ist.

Als etablierter Ausgangswert für einen vollständigen Materialwechsel wurden am EDTV4 mindestens etwa **150 mm bei 5 mm/s** verwendet. Bei starken Farb- oder Materialwechseln entsprechend länger spülen.

## 6. Düsenhöhe und relativer Toolhead-Abgleich

Vor der finalen Z-Offset-Kalibrierung müssen die Düsenspitzen von E0 und E1 mechanisch in Z aufeinander abgestimmt werden.

**Zielwert:** maximal etwa **0,02 mm relative Höhendifferenz**.

Empfohlen wird die Messung mit einer Messuhr am Maschinenrahmen. Nach einem Düsen- oder Hotendwechsel muss dieser Vergleich erneut durchgeführt werden.

## 7. Z-Offset kalibrieren

Die aktuelle Firmware verwendet den Probe-Sensor als virtuellen Z-Endschalter.

1. Mechanischen Düsenausgleich gemäß Abschnitt 6 prüfen.
2. Maschine referenzieren.
3. Klipper-Probe-/Z-Kalibrierung starten.
4. Düsenabstand mit Papiermethode oder dem im Labor festgelegten Referenzverfahren einstellen.
5. Wert speichern.
6. Nach dem Neustart einen First-Layer-Test durchführen.

Der aktuell gespeicherte Probe-Z-Offset ist maschinenspezifisch und darf nach mechanischen Änderungen nicht ungeprüft weiterverwendet werden.

Weitere Details: [`../calibration_and_maintenance.md`](../calibration_and_maintenance.md).

## 8. Düse wechseln

1. Betroffenen Extruder auf eine für das eingesetzte Filament geeignete Temperatur aufheizen und Filament vollständig entladen.
2. Hotend anschließend vollständig auf eine sichere Wechseltemperatur abkühlen lassen.
3. Düse entsprechend dem verbauten Revo-System wechseln.
4. Neuen Düsen-/Hotend-Sitz kontrollieren.
5. Relative Düsenhöhe E0/E1 erneut auf maximal etwa 0,02 mm Abweichung einstellen.
6. Z-Offset und First Layer erneut prüfen.

Bei Unsicherheit sind die Herstellerhinweise des verbauten Hotend-/Düsensystems maßgeblich.

## 9. Druck vorbereiten und starten

### 9.1 OrcaSlicer-Profil

Es sind die getesteten EDTV4-Profile aus [`../../slicer/orcaslicer/`](../../slicer/orcaslicer/) zu verwenden. Die bereitgestellten Profile für 0,4 mm, 0,6 mm und 1,0 mm wurden als funktionierende Maschinenstände übernommen und unterscheiden sich teilweise bewusst in ihren Toolchange- und Prozessparametern.

### 9.2 IDEX-Arbeitsbereich beachten

Die beiden X-Schlitten erreichen nicht denselben Bereich der nominellen Druckfläche:

- **E0 / linker Extruder:** die rechten **40 mm** sind nicht erreichbar.
- **E1 / rechter Extruder:** die linken **40 mm** sind nicht erreichbar.

Klipper begrenzt die mechanischen Verfahrwege, die Bauteilplatzierung im Slicer muss diese Einschränkung trotzdem berücksichtigen.

![IDEX-Arbeitsbereich](../../media/photos/IDEX_slicing_limits.png)

### 9.3 Verbindung zum Drucker

Der EDTV4 wird über die am jeweiligen Einsatzort konfigurierte Netzwerkverbindung und die Klipper/Mainsail-Oberfläche angesprochen. Netzwerkadresse und Zugangsdaten werden **nicht öffentlich dokumentiert** und sind bei den zuständigen Laborverantwortlichen zu beziehen.

In OrcaSlicer kann der EDTV4 als physischer Klipper-Drucker mit der vor Ort gültigen Adresse eingerichtet werden.

### 9.4 Automatische Startsequenz

Die aktuelle `START_PRINT`-Sequenz führt aus:

1. benutzerdefiniertes Homing,
2. `Z_TILT_ADJUST`,
3. neue `BED_MESH_CALIBRATE`,
4. Anheben auf Z = 5 mm.

Vor einem Druck muss daher der komplette Bewegungs- und Messbereich frei sein.

## 10. PLA, PETG und andere niedrigtemperierte Materialien

Für Materialien mit einer Glasübergangstemperatur unter ungefähr **70 °C** wurde am EDTV4 folgende Betriebsweise festgelegt, um Hitzestau am Coldend und Heat Creep zu reduzieren:

- Fronttür mindestens etwa **5 cm öffnen**.
- Obere Acrylklappe vollständig öffnen.

Bei geänderter Toolhead-Hardware, Lüfterkonfiguration oder Einhausungsströmung muss das reale thermische Verhalten erneut überprüft werden.

## 11. Druckraumheizung

Die aktuelle Firmware konfiguriert die aktive Druckraumheizung bis maximal **70 °C**. Die Kammer kann über die vorhandenen Makros bzw. ein dafür vorgesehenes Slicer-Profil gesteuert werden.

- `M141 S<Temperatur>`: Kammer-Zieltemperatur setzen, ohne zu warten.
- `M191 S<Temperatur>`: Kammer-Zieltemperatur setzen und bis zur Zieltemperatur warten.

Keine höheren Zieltemperaturen als die für Maschine, Material und Einbauten freigegebenen Werte verwenden.

## 12. Not-Halt

Der Hardware-Not-Halt ist in der aktuellen Firmware als Sicherheitsinput auf `STOP0 / PG6` eingebunden und löst Klippers Emergency-Stop-Funktion aus.

Nach einem Not-Halt:

1. Ursache feststellen und beseitigen.
2. Maschine auf mechanische, elektrische und thermische Schäden kontrollieren.
3. Not-Halt erst lösen, wenn ein sicherer Zustand hergestellt ist.
4. Klipper/Maschine neu starten und Funktion prüfen.

Der Not-Halt ist kein regulärer Ausschalter.

## 13. Wartung

### Vor jedem Druck

- PEI-Oberfläche kontrollieren und reinigen.
- Düsen auf Rückstände kontrollieren.
- Abstreifer/Reinigungsbereich kontrollieren.
- Bewegungsbereich und Parkpositionen freihalten.

### Etwa alle 100 Betriebsstunden

- Riemenspannung der X- und Y-Achsen prüfen.

### Etwa alle 500 Betriebsstunden

- Linearführungen und Kugelumlaufspindeln entsprechend Herstellerempfehlung schmieren.
- Im Drucker verbaute 3D-gedruckte Bauteile auf Wärme-, Ermüdungs- oder mechanische Schäden prüfen.
- Abluftfilter je nach tatsächlicher Belastung prüfen bzw. ersetzen.

### Etwa alle 2000 Betriebsstunden

- Schraubverbindungen prüfen und bei Bedarf sachgerecht nachziehen.
- Schaltschrank auf Staub und Verschmutzung kontrollieren. **Vor Arbeiten im Schaltschrank Maschine sicher vom Netz trennen.**

Die Intervalle stammen aus der bisherigen EDTV4-Betriebsdokumentation und sind anzupassen, falls Herstellerangaben oder reale Einsatzbedingungen kürzere Intervalle erfordern.

## 14. Kalibrierung nach Änderungen

Folgende Eingriffe erfordern zumindest eine Teilkalibrierung:

| Änderung | Erforderliche Prüfung |
|---|---|
| Düse gewechselt | Düsenhöhe, Z-Offset, First Layer |
| Hotend/Extruder mechanisch gelöst | Düsenhöhe, Tool-Offset, Z-Offset |
| Probe verändert | Probe-Z-Offset, Bed Mesh |
| Z-Mechanik verändert | Z-Tilt, Z-Offset, Bed Mesh |
| X/Y-Mechanik verändert | Homing, Tool-Offset, Input Shaping prüfen |
| Slicer-/Toolchange-Änderung | zunächst kontrollierter Testdruck |

Ausführliche Werte: [`../calibration_and_maintenance.md`](../calibration_and_maintenance.md).

## 15. Technische Dokumentation

- [Projektübersicht](../project_overview.md)
- [Systemarchitektur](../system_architecture.md)
- [Elektronik / Pinout](../electronics_pinout.md)
- [Kalibrierung & Wartung](../calibration_and_maintenance.md)
- [Klipper-Konfiguration](../../firmware/klipper/README.md)
- [Klipper-Makros](../../firmware/klipper/macros.md)
- [OrcaSlicer-Profile](../../slicer/orcaslicer/README.md)
- [CAD- und Fertigungsdaten](../../hardware/cad/README.md)
- [Öffentliche Stückliste](../../hardware/bom/README.md)

## 16. Dokumentstatus

Diese öffentliche Revision wurde aus der ursprünglichen EDTV4-Bedienungsdokumentation und dem aktuellen Repository neu aufgebaut. Interne Netzwerkdaten, persönliche Studienkennungen und veraltete technische Grenzwerte wurden bewusst nicht übernommen.
