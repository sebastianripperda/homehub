# HomeHub auf GitHub veröffentlichen

Diese Anleitung beschreibt die erste Veröffentlichung des HomeHub-Quellcodes und eines Installationspakets. GitHub wird dabei nicht automatisch verändert; alle Schritte zum Anlegen des Repositories und Releases bestätigst du selbst.

## Paketinhalt

- `README.md`: Projektübersicht und Schnellstart
- `server.js`, `cli.js`, `lib.js`, `knx.js`: Server und Laufzeitcode
- `frontend/` und `public/`: Oberflächenquellen und erzeugter Browserstand
- `install/` und `homehub-install.sh`: Installer-Vorlage und fertiger Proxmox-Installer
- `release-assets/homehub-install.tar.gz`: Archiv mit Installer und Kurzanleitung
- `docs/`: Installation, Architektur, KNX und Veröffentlichung
- `.github/`: Vorlagen für Fehlerberichte, Wünsche und Pull Requests
- `LICENSE`, `SECURITY.md`, `CONTRIBUTING.md`, `CHANGELOG.md`: Lizenz und Projektregeln

## Vor der Veröffentlichung

1. Prüfe, dass keine persönlichen Daten, echten Zugangsdaten, privaten Hostnamen oder Produktionskonfigurationen enthalten sind. `secret.key`, `data.json`, `.env` und Protokolle gehören nicht in das öffentliche Repository.
2. Führe die Tests und Builds aus:

   ```bash
   npm test
   npm run build:release
   bash -n homehub-install.sh
   node --check server.js
   node --check cli.js
   ```

3. Prüfe `git status` und lies Änderungen an README, Changelog, Installer und Dokumentation durch.
4. Beachte den Testumfang: `npm test` verwendet ein simuliertes KNX-Gateway. Ein erfolgreicher Test bedeutet nicht, dass die Installation auf echtem Proxmox oder mit KNX-Hardware geprüft wurde.

`npm run build:release` baut die Oberfläche und den Installer neu und erzeugt `release-assets/homehub-install.tar.gz`.

## Repository auf GitHub anlegen

1. Erstelle auf GitHub ein neues Repository mit dem Namen `HomeHub`. Wähle selbst, ob es öffentlich oder privat sein soll.
2. Wähle beim Anlegen keine zusätzlichen README-, Lizenz- oder `.gitignore`-Dateien, da diese bereits im Paket enthalten sind.
3. Entpacke das ZIP. Öffne ein Terminal im darin enthaltenen Ordner `HomeHub` und führe aus; ersetze `<GITHUB-NAME>` durch deinen GitHub-Benutzernamen oder deine Organisation:

   ```bash
   git init -b main
   git add .
   git commit -m "Initial HomeHub release"
   git remote add origin https://github.com/<GITHUB-NAME>/HomeHub.git
   git push -u origin main
   ```

4. Öffne auf GitHub den Bereich **About** und übernimm Beschreibung und Themen aus `GITHUB_METADATA.md`.
5. Prüfe auf der Repository-Seite, dass README, MIT-Lizenz, Dokumentation, Quellcode, Installer und `.github`-Vorlagen vorhanden sind.

## Erstes Release erstellen

1. Prüfe, dass `package.json`, `CHANGELOG.md` und `GITHUB_METADATA.md` dieselbe Versionsnummer nennen. Für die vorbereitete erste Veröffentlichung ist `v1.0.0` vorgesehen.
2. Erzeuge und veröffentliche das Tag:

   ```bash
   git tag -a v1.0.0 -m "HomeHub 1.0.0"
   git push origin v1.0.0
   ```

3. Öffne auf GitHub **Releases → Draft a new release**, wähle `v1.0.0` und übernimm Titel und Beschreibung aus `GITHUB_METADATA.md`.
4. Hänge `release-assets/homehub-install.tar.gz` als Release-Datei an. Optional kannst du auch `homehub-install.sh` einzeln anhängen.
5. Prüfe die Release-Seite nach dem Veröffentlichen und teste den Download des Installationsarchivs.

## Nach einer Codeänderung

Aktualisiere bei einer neuen Veröffentlichung Version und Changelog, führe die Tests erneut aus und erstelle die Release-Dateien neu:

```bash
npm test
npm run build:release
```

Erstelle danach ein neues Versions-Tag, zum Beispiel `v1.0.1`, und hänge das neu erzeugte Installationsarchiv an das zugehörige GitHub-Release an. Teile niemals `secret.key`, Portal-Daten, Wiederherstellungscodes oder echte Zugangsdaten.
