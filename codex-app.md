# OpenAI Codex Desktop App – Installation ohne Admin-Rechte (Windows 11)

Diese Anleitung beschreibt, wie die offizielle OpenAI Codex Desktop-App (MSIX-Paket) komplett ohne Administratorrechte auf einem Windows 11 PC installiert, konfiguriert, aktualisiert und bei Bedarf wieder rückstandslos entfernt werden kann. 

Dies baut auf der bestehenden lokalen Node.js-Konfiguration auf.

---

## 🛠️ Installation

Da der Microsoft Store in Firmennetzwerken oft blockiert ist oder Admin-Rechte verlangt, laden wir das originale Paket direkt von den Microsoft-Servern und registrieren es nur für den aktuellen Benutzer.

### 1. App-Paket herunterladen
1. Öffnen Sie den Dienst [store.rg-adguard.net](https://store.rg-adguard.net).
2. Stellen Sie das erste Dropdown-Menü von **URL (link)** auf **ProductId** um.
3. Tragen Sie folgende Produkt-ID in das Suchfeld ein: `9PLM9XGG6VKS`
4. Klicken Sie auf das Häkchen rechts.
5. Suchen Sie in der Liste nach der Datei, die mit `OpenAI.Codex` beginnt und auf `.msix` (oder `.msixbundle`) endet. Achten Sie auf die **x64**-Version.
6. *Tipp:* Falls der Download blockiert wird, machen Sie einen Rechtsklick auf den Link, wählen Sie **Link-Adresse kopieren**, fügen Sie diese in einen neuen Browser-Tab ein und drücken Sie Enter.

### 2. App im Benutzerprofil registrieren
1. Öffnen Sie das Windows-Startmenü, tippen Sie `PowerShell` ein und starten Sie diese ganz normal (**nicht** als Administrator).
2. Nutzen Sie den Befehl `Add-AppxPackage`. Um Tippfehler im Pfad zu vermeiden, nutzen Sie am besten die automatische Pfadkopie:
   * Gehen Sie im Windows-Explorer zu Ihrer heruntergeladenen Datei.
   * Halten Sie die **Shift-Taste** gedrückt, machen Sie einen **Rechtsklick** auf die Datei und wählen Sie **"Als Pfad kopieren"**.
3. Tippen Sie in der PowerShell folgenden Befehl ein (inklusive Leerzeichen am Ende) und fügen Sie Ihren Pfad mit `Strg + V` ein:
   ```powershell
   Add-AppxPackage -Path <Hier_Pfad_einfügen>
   ```
4. Drücken Sie **Enter**. Sobald der grüne Verarbeitungsbalken verschwindet, ist die App in Ihrem Startmenü verfügbar.

### 3. Lokales Node.js verknüpfen
Die App sucht standardmäßig nach einer globalen Node.js-Installation. Da Ihre Version lokal liegt, biegen wir dies in den Einstellungen um:
1. Starten Sie die **Codex App** über Ihr Startmenü.
2. Klicken Sie unten links auf **Settings** (Zahnrad) -> **Configuration**.
3. Suchen den Bereich **Workspace Dependencies**.
4. Falls Ihr lokales Node.js nicht automatisch erkannt wird, tragen Sie den pfad zu Ihrer lokalen `node.exe` (aus der CLI-Anleitung) manuell ein.

---

## 🔄 Updates einspielen

Da der Microsoft Store blockiert ist, führt die App keine automatischen Updates im Hintergrund aus. Sie können die App jedoch jederzeit manuell in weniger als einer Minute aktualisieren:

1. Rufen Sie erneut die Website [store.rg-adguard.net](https://store.rg-adguard.net) auf.
2. Suchen Sie mit der Produkt-ID `9PLM9XGG6VKS` nach dem neuesten Paket.
3. Laden Sie die aktuellste `.msix`-Datei herunter (erkennbar an einer höheren Versionsnummer im Dateinamen).
4. Öffnen Sie die **PowerShell** (ohne Admin-Rechte) und führen Sie den bekannten Befehl mit dem Pfad zur neuen Datei aus:
   ```powershell
   Add-AppxPackage -Path <Pfad_zur_NEUEN_Datei>
   ```

Windows erkennt automatisch, dass es sich um ein Update handelt, aktualisiert die App-Dateien im Hintergrund und behält Ihre Konfigurationen (wie den Node.js-Pfad) bei.

---

## 🗑️ Rückständlose Deinstallation

Da die App isoliert im Benutzerprofil registriert wurde, lässt sie sich extrem sauber und ohne Reste vom System entfernen.

### Methode 1: Über das Startmenü (Schnell)
1. Öffnen Sie das **Startmenü** und suchen Sie nach **Codex**.
2. Machen Sie einen **Rechtsklick** auf das App-Symbol.
3. Wählen Sie **Deinstallieren** und bestätigen Sie den Dialog.

### Methode 2: Über die PowerShell (Gründlich)
1. Öffnen Sie die **PowerShell** (normaler Start, kein Admin).
2. Führen Sie folgenden Befehl aus, um die Registrierung im Benutzerprofil komplett aufzuheben:
   ```powershell
   Get-AppxPackage *Codex* | Remove-AppxPackage
   ```

### 🧹 Optionale Bereinigung der App-Daten (100% sauber)
Falls Sie auch die lokal generierten Einstellungen und Cache-Dateien der App löschen möchten, entfernen Sie diese zwei Ordner (einfach unter `Windows-Taste + R` eingeben):

* **Lokale App-Daten:** Öffnen Sie `%localappdata%\Packages` und löschen Sie den Ordner, der `OpenAI.Codex` im Namen trägt.
* **Roaming-Daten:** Öffnen Sie `%appdata%` und prüfen Sie, ob ein verbliebener Ordner namens `Codex` oder `OpenAI` existiert, und löschen Sie diesen.
