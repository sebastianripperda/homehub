HomeHub - Installation auf Proxmox VE
=====================================
1. Dieses Archiv auf den Proxmox-Host kopieren:
     scp homehub-install.tar.gz root@<PROXMOX-IP>:/root/
2. Auf dem Proxmox-Host (als root) entpacken und starten:
     cd /root
     tar xzf homehub-install.tar.gz
     cd homehub-install
     bash homehub-install.sh
3. Fragen beantworten. Eine öffentliche Portal-Adresse ist optional; wenn sie leer bleibt, ist das Portal direkt über http://<Container-IP>:8080 erreichbar. Es werden keine Domäne und keine Hostnamen voreingestellt. Notfall-Code am Ende notieren, er wird nur einmal angezeigt.

Aktualisieren (Daten bleiben):  bash homehub-install.sh --update <CTID>
Hilfe:                          bash homehub-install.sh --help
Protokoll bei Problemen:        /tmp/homehub-install-*.log
Container-Vorlage: bevorzugt Debian 13 (debian-13-standard), sonst Debian 12. Festlegen: HH_TEMPLATE=debian-13-standard_13.6-1_amd64.tar.zst bash homehub-install.sh

KNX-Hinweise (ab dieser Version)
- Dimmer: Helligkeit (1 Byte), Ein/Aus (1 Bit), Rueckmeldung Helligkeit und Rueckmeldung Ein/Aus lassen sich getrennt eintragen.
- Rollladen: Position, Auf/Ab, Stopp und Rueckmeldung sind einzeln optional.
- Unter "KNX einrichten" gibt es einen Telegramm-Monitor und je Geraet einen Test der Rueckmeldung.
- Aktualisieren ohne Datenverlust: bash homehub-install.sh --update <CTID>

