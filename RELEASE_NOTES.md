# LevelSense-NMEA2000

Das Tool liegt seit 1.0.0 in einem eigenen Repository und zaehlt eigene
Versionen. Vorher lag es als `PC_Tools/` im Repository
`CAN_FuellstandsensorBLE`, ohne eigene Nummer und ohne Release - wer es nutzen
wollte, musste sich den Quelltext holen und Python einrichten. Freigabe per Tag
`vX.Y.Z`.

## 1.0.0

**Eigenes Repository, eigene Versionsnummer.** Das Tool meldet seine Version
ueber `--version` und zeigt sie im Fenstertitel. Die 1.0.0 ist die erste
vergebene Nummer; vorher gab es keine.

**Fertige .exe.** Jeder Tag baut eine eigenstaendige Windows-Datei, die kein
Python auf dem Rechner braucht. Der PEAK-Treiber wird weiterhin gebraucht. Das
Symbol und die PCAN-Anbindung stehen jetzt im Workflow statt in einer
`nmea2000_gui.spec`, die dadurch entfaellt.

**Protokoll-Kopien gekennzeichnet.** Die Werte, die mit der Firmware
uebereinstimmen muessen - Werksadresse und das proprietaere PGN 126720 mit
seinen Kommandos - stehen im Quelltext als Kopie aus
`CAN_FuellstandsensorBLE` markiert, mit der Quelldatei daneben.

**BLE_Protokoll.md bleibt bei der Firmware.** Die Datei lag in `PC_Tools/`,
beschreibt aber das BLE-Textprotokoll fuer die Handy-App und nicht das
PC-Tool. Sie liegt jetzt im Wurzelverzeichnis von `CAN_FuellstandsensorBLE`.
