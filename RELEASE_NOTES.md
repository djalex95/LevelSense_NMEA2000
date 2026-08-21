# LevelSense-NMEA2000

Das Tool liegt seit 1.0.0 in einem eigenen Repository und zaehlt eigene
Versionen. Vorher lag es als `PC_Tools/` im Repository
`CAN_FuellstandsensorBLE`, ohne eigene Nummer und ohne Release - wer es nutzen
wollte, musste sich den Quelltext holen und Python einrichten. Freigabe per Tag
`vX.Y.Z`.

## 1.2.1

Der Zähler im Langzeitprotokoll heißt jetzt »Sackgassen« statt
»Selbstheilungen«: ab Firmware 2.2.0 wird dabei nichts mehr gelöscht, die
Sackgasse wird nur noch erkannt und festgehalten. Das neue Ereignis
»Pairing-Sackgasse erkannt« wird übersetzt; das alte Ereignis der
ausgelösten Selbstheilung bleibt lesbar, damit Sensoren mit älterer
Firmware weiterhin richtig angezeigt werden.

## 1.2.0

**Langzeitprotokoll des BLE-Zweigs.** Neue Schaltfläche »BLE-Langzeit«. Die
BLE-Diagnose aus 1.1.0 zeigt die letzten 24 Ereignisse zwischen
Mikrocontroller und Funkmodul - im Normalbetrieb ist dieser Puffer nach
wenigen Minuten überschrieben, und sein Zeitstempel läuft nach 109 Minuten um.
Für einen Fehler, der erst nach Stunden auftritt, taugt er deshalb nicht.

Die neue Antwort liefert stattdessen Zähler, die seit dem Start des Sensors
weiterlaufen - Verbindungen insgesamt, davon verschlüsselt beendete, ohne
Verschlüsselung beendete, Selbstheilungen und Neustarts des Funkmoduls - und
ein zweites Protokoll, in das nur die seltenen Ereignisse geschrieben werden:
Neustarts, frische Kopplungen, gelöschte Bonds und jede ausgelöste
Selbstheilung. Zeitstempel in Sekunden seit dem Start, angezeigt als Tage und
Uhrzeit.

Damit lässt sich die Frage beantworten, warum ein gekoppeltes Handy nach
Stunden wieder nach der PIN gefragt wird: hat der Sensor selbst die Bonds
gelöscht, steht die Heilung mit Zeitpunkt im Protokoll. Steht dort nichts, kam
es von der Gegenseite.

Setzt Sensor-Firmware mit der Antwort 0x88 voraus; ältere Firmware antwortet
auf die Anfrage nicht.

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

Benannt werden dabei auch die Ereignisse, die den Fehler ab Firmware 2.0.0
sichtbar gemacht haben: `ERROR_IND` als Fehlermeldung des Funkmoduls,
`DATA_REQ` für die ersten Nutzdaten einer Verbindung und `Senden
unterdrueckt` für einen Sendeversuch auf einer noch unverschlüsselten
Verbindung.

**Rohwerte der Druckmessung.** Neue Schaltfläche »Rohwerte«. Sie fragt den
Sensor zweimal je Sekunde nach der kompletten Rechenkette einer Messung:
Registerwert, Abstand zur Bereichsmitte samt Bereichsgrenze, µBar vor und nach
dem Offset, gefilterter Druck, Temperatur, Füllhöhe vor und nach der
Linearisierung sowie Messbereich, Kalibrierwert und Fehlerbits. Erneutes
Klicken beendet die Abfrage.

Gedacht ist das für Messfehler, die man an der App nicht mehr auseinanderhalten
kann. Steht dort der Wert am Bereichsende, ist nicht zu erkennen, ob der Sensor
tatsächlich dort liegt oder ob eine Zwischenstufe umgeschlagen ist – die App
zeigt nur das Ende der Kette. Mit der laufenden Abfrage lässt sich beim
langsamen Verändern des Drucks zusehen, an welcher Stelle ein Wert springt. Das
proprietäre Kommando ist `0x07`, die Antwort `0x87`; der Sensor braucht dafür
Firmware 2.0.0.

Liegt der Rohwert außerhalb des Nennbereichs, steht in der Zeile mit dem Abstand
zur Bereichsmitte auch die Richtung, an der die Firmware den Wert festhält –
Über- oder Unterdruck. Das ist der Fall, in dem der Registerwert selbst nichts
mehr verrät: jenseits von ±32768 digits läuft er um und sieht in beiden
Richtungen gleich aus.

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
