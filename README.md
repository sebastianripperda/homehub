# HomeHub

HomeHub ist ein selbst gehostetes Portal für den Haushalt. Es bündelt Web-Anwendungen, KNX-Steuerung und Benutzerrechte in einer responsiven Oberfläche für PC, Tablet und Smartphone.

> **Projektstatus:** HomeHub ist eine frühe öffentliche Veröffentlichung. Die automatisierten KNX-Tests verwenden ein simuliertes Gateway. Tests auf echtem Proxmox, mit KNX-Hardware und in allen Browsern stehen noch aus. Bitte prüfe Code und Netzwerkkonfiguration vor einem produktiven Einsatz.

## Funktionen

- Responsives Portal mit hellem und dunklem Design
- Einbindung von Jellyfin, Paperless-ngx, Vaultwarden, Haus-KI, RetroHub und weiteren Web-Anwendungen
- Benutzer-, Raum- und Anwendungsrechte, einschließlich Tablet-Konten
- KNX-Steuerung für Licht, Dimmer, Rollläden und Thermostate
- Getrennte Gruppenadressen für Befehle und Rückmeldungen, Telegramm-Monitor und Rückmeldungs-Tests
- Proxmox-Installer für einen unprivilegierten LXC-Container
- Aktualisierung ohne Löschen der Portal-Konfiguration und gespeicherten Daten
- Node.js-Server ohne zusätzliche Laufzeitpakete

## Schnellstart auf Proxmox VE

1. Lade `homehub-install.tar.gz` aus dem GitHub-Release auf deinen Proxmox-Host. Das Archiv enthält den Installer und eine Kurzanleitung.
2. Entpacke es auf dem Proxmox-Host und starte den Installer als `root`:

   ```bash
   tar xzf homehub-install.tar.gz
   cd homehub-install
   bash homehub-install.sh
   ```

3. Beantworte die Installationsfragen und notiere den einmalig angezeigten Notfallcode sicher.
4. Rufe das Portal unter `http://<Container-IP>:8080` auf.

Der Installer setzt **keine Heimnetz-Domäne und keine Hostnamen als Standard**. Eine öffentliche Portal-Adresse ist optional. Wenn du sie nicht einträgst, erreichst du das Portal direkt über die Container-IP. Die öffentlichen Adressen der Anwendungen bleiben zunächst leer und können anschließend in der Anwendungsverwaltung eingetragen werden.

Die Installationswerte können mit `HH_*`-Umgebungsvariablen vorbelegt werden. Für den Automatikmodus ist `HH_YES=1` erforderlich. Lies vorher die verfügbaren Optionen mit:

```bash
bash homehub-install.sh --help
```

### Aktualisieren

Auf dem Proxmox-Host:

```bash
bash homehub-install.sh --update <CTID>
```

Die Portal-Konfiguration und gespeicherten Daten bleiben erhalten. Der Installer hält die vorherige Programmversion im Container für einen Rollback bereit.

## Netzwerk und Sicherheit

Das Portal lauscht intern auf HTTP-Port `8080`. TLS-Zertifikate und Reverse-Proxy sind nicht Bestandteil dieses Projekts. Wenn du einen Proxy verwendest, richte eigene DNS-Namen und Proxy-Ziele ein und hinterlege in HomeHub nur die tatsächlich vertrauenswürdige Proxy-IP. **Leite Port `8080` nicht ungeschützt ins öffentliche Internet weiter.** Verwende starke, individuelle Passwörter und bewahre den Notfallcode sicher auf.

HomeHub speichert Anmeldedaten und Tokens für angebundene Dienste. Teile niemals `secret.key`, Portal-Daten, Wiederherstellungscodes oder echte Zugangsdaten. Hinweise zur verantwortungsvollen Meldung von Sicherheitsproblemen stehen in [`SECURITY.md`](SECURITY.md).

## Voraussetzungen

- Proxmox VE und Zugriff als `root` für die Container-Installation
- Debian-LXC-Vorlage; der Installer bevorzugt Debian 13 und kann auf Debian 12 zurückfallen
- Node.js 18 oder neuer im Container
- Netzwerkzugriff des Containers auf die KNX/IP-Schnittstelle und die gewünschten Web-Anwendungen
- Für sicheren Zugriff von außen: separat eingerichteter Reverse-Proxy mit TLS und passende Firewall-Regeln

## Entwicklung und Tests

Der KNX-Regressionstest startet einen lokalen Portalserver und ein simuliertes Gateway. Die lokalen Testports `18080` und `13671` müssen frei sein. Es werden keine Telegramme an echte KNX-Geräte gesendet.

```bash
npm test
npm run build:release
```

`npm run build:release` baut die Oberfläche und den Installer neu und erzeugt das Installationsarchiv unter `release-assets/homehub-install.tar.gz`.

## Projektdateien

- `server.js`, `cli.js`, `lib.js`, `knx.js`: Server, Admin-Befehle, gemeinsame Funktionen und KNX/IP
- `frontend/`: Oberflächenquellen
- `public/index.html`: erzeugte und ausgelieferte Oberfläche
- `install/` und `homehub-install.sh`: Installer-Vorlage und fertiger Installer
- `release-assets/`: Installationsarchiv für ein GitHub-Release
- `tools/`: Build- und Paketierungsskripte
- `test-knx.js`: KNX-Regressionstest mit simuliertem Gateway
- `docs/`: Installations-, Architektur-, KNX- und Veröffentlichungshinweise
- `.github/`: Vorlagen für Fehlerberichte, Funktionswünsche und Pull Requests

Für die Veröffentlichung des Repositories und eines GitHub-Releases siehe [`docs/PUBLISHING.md`](docs/PUBLISHING.md). Hinweise zu Installation und KNX stehen in [`docs/INSTALLATION.md`](docs/INSTALLATION.md) und [`docs/KNX.md`](docs/KNX.md).

## Mitwirken und Lizenz

Fehlerberichte, Vorschläge und Pull Requests sind willkommen. Bitte entferne vorher Tokens, Passwörter, öffentliche IP-Adressen und persönliche Daten. Siehe [`CONTRIBUTING.md`](CONTRIBUTING.md).

HomeHub steht unter der [MIT-Lizenz](LICENSE).
