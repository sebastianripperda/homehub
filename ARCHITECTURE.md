# Überblick

HomeHub besteht aus einem Node.js-HTTP-Server, einer browserseitigen Oberfläche und dem KNX/IP-Client. Der Server verwendet Node.js-Bordmittel; zusätzliche Laufzeitpakete werden nicht benötigt.

## Daten und Laufzeit

- Portal-Daten und KNX-Zustände werden im Datenverzeichnis des Containers abgelegt.
- Der Verschlüsselungsschlüssel für gespeicherte Dienst-Tokens gehört ausschließlich in dieses Verzeichnis.
- Der Installer richtet einen Systemdienst für den Portalserver ein und legt den Verwaltungsbefehl `homehub-admin` an.
- Die eingebundenen Web-Anwendungen werden separat betrieben.

## Quellaufteilung

- `server.js`: HTTP-Routen, Authentifizierung, Anwendungs- und KNX-Logik
- `lib.js`: Datenhaltung, Verschlüsselung und gemeinsam verwendete Funktionen
- `knx.js`: KNX/IP-Verbindung und Telegrammbehandlung
- `cli.js`: Verwaltungsbefehle im Container
- `frontend/base.html`: Basis der Portaloberfläche
- `frontend/real.js`: Portal- und Serveranbindung
- `frontend/knxui.js`: KNX-Karten, Szenen und Einrichtung
- `public/index.html`: erzeugte, ausgelieferte Browseroberfläche
- `install/homehub-install.template.sh`: Installer ohne eingebettete Nutzdaten

## Oberfläche und Installer bauen

Aus dem Repository-Hauptverzeichnis:

```bash
python3 tools/build-frontend.py
bash tools/build-installer.sh
```

Der Installer enthält ein komprimiertes Laufzeitpaket. Wenn du Serverdateien oder `public/index.html` änderst, baue den Installer erneut.
