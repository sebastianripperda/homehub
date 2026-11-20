# KNX konfigurieren

HomeHub kommuniziert mit einer KNX/IP-Schnittstelle aus dem Container heraus. Gateway-Adresse, Port und Anbindungsart werden in „KNX einrichten“ festgelegt. Die Verbindung und einzelne Statusadressen lassen sich dort testen.

## Gruppenadressen nach Gerätetyp

Die tatsächlich verwendeten Gruppenadressen und Datentypen hängen von ETS-Projekt, Aktor und KNX-Objekten ab. Trage nur Adressen ein, die zu den Kommunikationsobjekten deiner Installation passen.

| Gerät | Hauptadresse | Optionale Adressen |
|---|---|---|
| Licht / Schaltsteckdose | Schalten (1 Bit) | Schaltstatus (1 Bit) |
| Dimmer | Helligkeitswert (1 Byte) | Ein/Aus (1 Bit), Helligkeitsstatus (1 Byte), Ein/Aus-Status (1 Bit) |
| Rollladen | Position (1 Byte) oder Auf/Ab (1 Bit) | Stopp (1 Bit), Positionsstatus (1 Byte) |
| Thermostat | Solltemperatur (2 Byte) | Isttemperatur (2 Byte), Sollwertstatus (2 Byte) |

Bei Rollläden gilt im Auf/Ab-Betrieb: `0` fährt auf, `1` fährt ab. Ohne Positionsadresse ist keine stufenlose Positionierung verfügbar. Ohne Stopp-Adresse zeigt HomeHub keine Stopp-Taste.

## Dimmer-Rückmeldung

Viele Dimmaktoren haben getrennte Kommunikationsobjekte für Helligkeitswert und Ein/Aus. Trage deshalb die Schaltadresse und den Ein/Aus-Status separat ein, wenn dein Aktor diese Objekte verwendet. Die Statusadresse ist üblicherweise die Gruppenadresse, auf die der Aktor seinen tatsächlichen Wert sendet – nicht zwingend dieselbe Adresse wie der Schreibbefehl.

## Status testen

1. Öffne „KNX einrichten“.
2. Wähle beim Gerät „Testen“.
3. Wenn keine Antwort kommt, kontrolliere Gruppenadresse, DPT und die Kommunikationsflags des Aktorobjekts in ETS. Für Gruppen-Leseanfragen muss das Lese-Flag passend gesetzt sein.
4. Öffne den Telegramm-Monitor und schalte das Gerät über den Wandtaster. Ein Telegramm, das keinem Gerät zugeordnet ist, kann ein Hinweis auf die gesuchte Rückmeldeadresse sein.

Ein fehlender Antwortwert beweist nicht allein einen Fehler des Portals. Aktoren können Leseanfragen ignorieren; außerdem können Adresse oder DPT falsch sein. Prüfe Telegramme und ETS-Konfiguration, bevor du Änderungen an Aktoren vornimmst.

## Grenzen

Der automatisierte Regressionstest verwendet ein simuliertes Gateway. Er bestätigt keine konkrete ETS-Konfiguration, KNX/IP-Schnittstelle oder Aktor-Kompatibilität.
