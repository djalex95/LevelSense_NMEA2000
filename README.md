# LevelSense-NMEA2000

PC-Programm für den NMEA2000-Füllstandsensor (STM32G0B1), angebunden über
einen PEAK PCAN-USB-Adapter. Zeigt den Füllstand live an, liest Address Claim,
Product Info und Heartbeat mit und schreibt die Konfiguration: Fluidtyp,
Kapazität, Instanz, Sensorname, Tankform-Kennlinie, 100-%-Kalibrierung und
Werksreset. Ausserdem liest es die BLE-Diagnose des Sensors aus – den einzigen
Weg, der noch offen ist, wenn sich kein Handy mehr koppeln lässt.

Das Programm lag bis 1.0.0 als `PC_Tools/` im Repository
`CAN_FuellstandsensorBLE` und hatte weder eine eigene Versionsnummer noch ein
Release. Es liegt jetzt hier, zählt eigene Versionen und wird als fertige
Windows-.exe ausgeliefert.

## Aufbau

- `nmea2000_gui.py` – die grafische Oberfläche. Das ist das Programm, das als
  .exe ausgeliefert wird.
- `nmea2000_reader.py` – Decoder und Encoder für die verwendeten PGNs. Wird von
  der Oberfläche importiert, lässt sich aber auch allein als
  Konsolen-Mitlesewerkzeug starten.
- `icon.ico` – Fenster- und Taskleistensymbol, wird in die .exe eingebettet.

## Starten

Aus dem Quelltext:

```
pip install python-can
python nmea2000_gui.py
```

Nur mithören, ohne Oberfläche:

```
python nmea2000_reader.py --channel PCAN_USBBUS1
python nmea2000_reader.py --request      # zusätzlich Product-Info anfragen
python nmea2000_reader.py --version
```

Voraussetzung ist in beiden Fällen der installierte PEAK-Treiber (PCAN-Basic).
Die fertige .exe aus den Releases braucht kein Python, den PEAK-Treiber aber
trotzdem.

## Konstanten aus dem Firmware-Repository

`nmea2000_reader.py` enthält zwei Blöcke, die Kopien aus
`CAN_FuellstandsensorBLE` sind und mit der Firmware übereinstimmen müssen:

- die Werksadresse `SENSOR_ADDR` (`Core/Src/app_config.c`)
- das proprietäre Protokoll auf PGN 126720, also `PROP_HEADER` und die
  `PROP_CMD_*`-Kommandos (`Core/Src/nmea_app.c`)

Beide sind im Quelltext als Kopie gekennzeichnet. Kommt in der Firmware ein
Kommando dazu oder ändert sich eine Nummer, gehört die Änderung hier im selben
Zug nachgezogen – sonst reden Sensor und Tool aneinander vorbei. Geprüft wird
das nicht automatisch, weil dafür ein Zugriff über Repository-Grenzen hinweg
nötig wäre.

## Versionsnummern

Die Nummer hat drei Stellen, X.Y.Z, und jede Stelle hat eine feste Bedeutung:
**X** steigt bei einer größeren Änderung, **Y** wenn ein kleineres Feature
dazukommt, **Z** bei Bugfixes. Das gilt gleich in allen Repositories des
Projekts.

Das Tool zählt in `TOOL_VERSION` in `nmea2000_reader.py`; die Oberfläche zeigt
die Nummer im Fenstertitel, `nmea2000_reader.py --version` gibt sie aus.
Freigegeben wird über einen Tag `vX.Y.Z`; die CI bricht ab, wenn Tag und
`TOOL_VERSION` nicht zusammenpassen.
