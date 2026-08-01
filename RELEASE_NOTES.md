# LevelSense-NMEA2000

Das Tool liegt seit 1.0.0 in einem eigenen Repository und zaehlt eigene
Versionen. Vorher lag es als `PC_Tools/` im Repository
`CAN_FuellstandsensorBLE`, ohne eigene Nummer und ohne Release - wer es nutzen
wollte, musste sich den Quelltext holen und Python einrichten. Freigabe per Tag
`vX.Y.Z`.

## 1.1.0

**BLE-Diagnose über den CAN-Bus.** Neue Schaltfläche »BLE-Diagnose«. Sie fragt
den Sensor nach dem Zustand seiner BLE-Provisionierung, nach den aus dem
Funkmodul zurückgelesenen Sicherheitseinstellungen samt Modul-Firmware und nach
einem Protokoll der letzten 24 Ereignisse zwischen Mikrocontroller und
Funkmodul. Die Antwort landet lesbar im Protokollfenster.

Gedacht ist das für den Fall, in dem sich kein Handy mehr koppeln lässt: dann
ist BLE als Diagnoseweg ja gerade nicht verfügbar, der CAN-Bus aber schon. Aus
dem Ereignisprotokoll ist zu erkennen, ob das Funkmodul die Verbindung selbst
beendet hat (mit Grund), ob es neu gestartet ist, ob sich der Mikrocontroller
neu gestartet hat (mit Grund aus `RCC->CSR`) – oder ob in dem Moment schlicht
nichts passiert ist. Das proprietäre Kommando ist `0x06`, die Antwort `0x86`;
der Sensor braucht dafür Firmware 2.0.0.

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
