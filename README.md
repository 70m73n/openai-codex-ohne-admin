# OpenAI Codex CLI unter Windows ohne Admin-Rechte

Eine praxisnahe Anleitung, um die OpenAI Codex CLI auf einem Windows-PC ohne Administratorrechte einzurichten, zu aktualisieren und bei Bedarf vollständig wieder zu entfernen.

Diese Anleitung nutzt eine lokale Installation im Benutzerverzeichnis. Codex wird deshalb nicht global installiert und immer über `npx codex` gestartet.

---

## Inhaltsverzeichnis

- [Worum es geht](#worum-es-geht)
- [Setup ohne Admin-Rechte](#setup-ohne-admin-rechte)
- [Codex starten und anmelden](#codex-starten-und-anmelden)
- [Updates richtig machen](#updates-richtig-machen)
- [Probleme schnell lösen](#probleme-schnell-lösen)
- [Codex sauber entfernen](#codex-sauber-entfernen)

---

## Worum es geht

Auf manchen Windows-PCs sind keine Administratorrechte vorhanden. Eine normale globale Installation mit npm ist dann oft nicht möglich oder nicht gewünscht.

Diese Anleitung verwendet deshalb:

- portables Node.js als ZIP-Version
- Benutzer-Umgebungsvariablen statt System-Umgebungsvariablen
- lokale npm-Installation ohne `-g`
- Start über `npx codex`

Wichtig:

```cmd
npm install -g @openai/codex
```

wird in dieser Anleitung bewusst nicht verwendet.

---

## Setup ohne Admin-Rechte

### 1. Portables Node.js herunterladen

Öffne die offizielle Node.js-Downloadseite:

```text
https://nodejs.org/en/download
```

Lade dort die **Windows Binary ZIP-Datei** herunter. In der Regel ist die **x64-Version** passend.

Entpacke die ZIP-Datei direkt in Dein Benutzerverzeichnis und benenne den Ordner einfach in `node` um.

Beispiel:

```text
C:\Users\rbo\node
```

Der Pfad ist nur ein Beispiel. Bei Dir steht statt `rbo` Dein eigener Windows-Benutzername.

---

### 2. Node.js zum Benutzer-Path hinzufügen

Damit Windows die Befehle `node`, `npm` und `npx` findet, muss der Node-Ordner in den Benutzer-Path eingetragen werden.

Vorgehen:

1. Windows-Taste drücken.
2. Nach `Umgebungsvariablen` suchen.
3. `Umgebungsvariablen für dieses Konto bearbeiten` öffnen.
4. Oben bei den Benutzervariablen die Variable `Path` auswählen.
5. Auf `Bearbeiten` klicken.
6. Auf `Neu` klicken.
7. Den Pfad zum Node-Ordner eintragen.

Beispiel:

```text
C:\Users\rbo\node
```

Danach alle Fenster mit `OK` bestätigen.

Wichtig: Alle offenen Eingabeaufforderungen schließen und danach eine neue Eingabeaufforderung öffnen.

---

### 3. Node.js prüfen

Öffne eine normale Eingabeaufforderung ohne Administratorrechte.

Prüfe zuerst Node.js:

```cmd
node -v
```

Wenn eine Versionsnummer erscheint, wurde Node.js korrekt erkannt.

Prüfe danach npm:

```cmd
npm -v
```

Wenn auch hier eine Versionsnummer erscheint, ist npm einsatzbereit.

---

### 4. Codex lokal installieren

Wechsle zuerst in Dein Benutzerverzeichnis:

```cmd
cd %USERPROFILE%
```

Installiere Codex lokal:

```cmd
npm install @openai/codex
```

Dabei entstehen im Benutzerverzeichnis unter anderem diese Dateien und Ordner:

```text
node_modules
package.json
package-lock.json
```

Das ist normal. Diese Dateien gehören zur lokalen npm-Installation.

---

## Codex starten und anmelden

### Anmeldung

Da Codex lokal installiert wurde, wird Codex mit `npx` gestartet.

Starte die Anmeldung mit:

```cmd
npx codex login
```

Danach öffnet sich der Browser zur Anmeldung.

---

### Codex starten

Codex wird anschließend immer so gestartet:

```cmd
npx codex
```

Nicht nur `codex` eingeben.

Bei dieser Installation ist `npx codex` der richtige Befehl.

---

## Updates richtig machen

### Warum die Codex-Updatefunktion hier nicht passt

Codex kann beim Start eine Update-Meldung anzeigen, zum Beispiel:

```text
Update available! 0.133.0 -> 0.135.0

1. Update now (runs `npm install -g @openai/codex`)
2. Skip
3. Skip until next version
```

Die Option `Update now` verwendet diesen Befehl:

```cmd
npm install -g @openai/codex
```

Das ist eine globale Installation. Sie passt nicht zu dieser Anleitung ohne Administratorrechte.

Wähle in dieser Meldung deshalb:

```text
2. Skip
```

oder:

```text
3. Skip until next version
```

Danach Codex wie gewohnt starten:

```cmd
npx codex
```

---

### Codex manuell aktualisieren

Wenn eine neue Version verfügbar ist, aktualisiere Codex manuell im Benutzerverzeichnis:

```cmd
cd %USERPROFILE%
npm install @openai/codex@latest
```

Prüfe danach die installierte Version:

```cmd
npx codex --version
```

Starte Codex anschließend wieder mit:

```cmd
npx codex
```

---

## Probleme schnell lösen

### `codex` wird nicht erkannt

Ursache: Codex wurde lokal installiert, nicht global.

Falsch:

```cmd
codex
```

Richtig:

```cmd
npx codex
```

---

### Update über Codex funktioniert nicht

Ursache: Die eingebaute Updatefunktion versucht eine globale Installation mit `-g`.

Lösung: Update-Meldung überspringen und Codex manuell aktualisieren:

```cmd
cd %USERPROFILE%
npm install @openai/codex@latest
```

---

### `node` oder `npm` wird nicht erkannt

Mögliche Ursachen:

- Der Node-Ordner wurde nicht korrekt in den Benutzer-Path eingetragen.
- Der falsche Ordner wurde eingetragen.
- Die Eingabeaufforderung wurde nach der Änderung nicht neu geöffnet.

Prüfe den Path-Eintrag. Er sollte auf den Node-Ordner zeigen, zum Beispiel:

```text
C:\Users\rbo\node
```

Schließe danach alle cmd-Fenster, öffne eine neue Eingabeaufforderung und teste erneut:

```cmd
node -v
npm -v
```

---

### Codex zeigt nach dem Update weiter eine alte Version

In diesem Fall kann eine saubere Neuinstallation der lokalen Codex-Pakete helfen.

Nur ausführen, wenn Codex wirklich im Benutzerverzeichnis installiert wurde:

```cmd
cd %USERPROFILE%
rmdir /s /q node_modules
del package-lock.json
npm install @openai/codex@latest
npx codex --version
```

Danach Codex starten:

```cmd
npx codex
```

---

## Codex sauber entfernen

Wenn Codex und Node.js vollständig entfernt werden sollen, lösche im Benutzerverzeichnis diese Elemente:

```text
node
node_modules
.codex
package.json
package-lock.json
```

Entferne anschließend den Node-Pfad aus den Benutzer-Umgebungsvariablen.

Beispiel für den zu entfernenden Path-Eintrag:

```text
C:\Users\rbo\node
```

Danach ist die Installation entfernt.

---

## Kurzbefehle

Installation:

```cmd
cd %USERPROFILE%
npm install @openai/codex
```

Anmeldung:

```cmd
npx codex login
```

Start:

```cmd
npx codex
```

Update:

```cmd
cd %USERPROFILE%
npm install @openai/codex@latest
```

Version prüfen:

```cmd
npx codex --version
```
