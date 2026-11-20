# Installation auf Proxmox VE

## Vor dem Start

- Auf dem Proxmox-Host als `root` arbeiten.
- Eine freie Container-ID, Speicher, Bridge und passende Netzwerkwerte bereithalten.
- Prüfen, dass DNS, Gateway, KNX/IP-Schnittstelle und gewünschte Web-Anwendungen aus dem Container erreichbar sein werden.
- Den Installer vor dem Ausführen lesen und die Werte am Ende kontrollieren.

## Neuinstallation

Auf dem Proxmox-Host:

```bash
bash homehub-install.sh
```

Die Fragen unterstützen Standardwerte und Vorbelegung über `HH_*`-Umgebungsvariablen. `HH_YES=1` beantwortet die Fragen mit gesetzten beziehungsweise vorgesehenen Werten. Nutze den Automatikmodus erst, wenn du alle erforderlichen Werte geprüft hast.

Die einmalig ausgegebene Notfall-Wiederherstellungscodierung sicher verwahren. Der Portalserver läuft auf Port `8080` im Container. Der Installer legt keine Domäne oder Hostnamen fest. Ohne eigene öffentliche Portal-Adresse erreichst du das Portal direkt über `http://<Container-IP>:8080`; DNS, Reverse-Proxy und öffentliche App-Adressen richtest du bei Bedarf separat ein.

## Aktualisierung

```bash
bash homehub-install.sh --update <CTID>
```

Der Update-Modus ersetzt Programmdateien. Portal-Konfiguration und Anwendungsdaten werden nicht absichtlich gelöscht. Bei einem fehlgeschlagenen Gesundheitscheck versucht der Installer, die vorherige Programmversion wiederherzustellen.

## Reverse-Proxy

TLS und Reverse-Proxy werden nicht von diesem Installer eingerichtet. Konfiguriere sie separat. Prüfe insbesondere TLS, vertrauenswürdige Proxy-IP und Firewall-Regeln. Der HTTP-Port `8080` soll nicht ungeschützt aus dem Internet erreichbar sein.

## Hilfe bei Problemen

- Installer-Hilfe: `bash homehub-install.sh --help`
- Das Installationsprotokoll liegt unter `/tmp/homehub-install-*.log` auf dem Proxmox-Host.
- Keine vollständigen Logs öffentlich posten, bevor Tokens, IPs, Hostnamen und persönliche Daten entfernt wurden.
