# Zyklus

Eine einfache mobile Web-App (PWA) zum Erfassen und Vorhersagen des Menstruationszyklus. Sie läuft komplett im Browser des iPhones. Alle Einträge werden nur lokal auf dem Gerät gespeichert. Es gibt keinen Server, kein Konto und keine Cloud.

## Funktionen

Die App hat vier Reiter:

- **Kalender** (Startseite, springt beim Start in den aktuellen Monat)
  - Monate ab August 2026, nach unten unbegrenzt. Weitere Monate werden beim Scrollen nachgeladen.
  - Kopfkarte mit nächstem Eisprung, nächster Blutung und der heutigen Zyklusphase.
  - Ein Tipp auf einen Tag setzt oder entfernt die Blutung. Es erscheint ein Hinweis mit "Rückgängig".
  - Darstellung: Blutung = Mahagoni gefüllt, Eisprung = Moosgrün gefüllt, fruchtbare Tage = hellgrün, Prognosen = gestrichelt. Der heutige Tag ist handgezeichnet eingekreist.
- **Übersicht**
  - Nächste Blutung und nächster Eisprung mit Datum.
  - Kennzahlen: Ø Zyklus, Ø Blutung, kürzester und längster Zyklus, Eisprungtag, Regelmäßigkeit, Anzahl Zyklen.
  - Balken der letzten 6 Zyklen.
  - Ein Tipp auf eine Kachel öffnet eine Erklärung als Notiz-Pop-up.
- **Ernährung**
  - Hinweise je Phase: Menstruation, Follikelphase, Eisprung, Lutealphase.
  - Pro Phase gibt es einen Fokus, Beispiel-Lebensmittel, Tipps und "Eher reduzieren". Die aktuelle Phase ist vorausgewählt.
  - Die Texte sind allgemeine Hinweise und ersetzen keine ärztliche Beratung.
- **Sicherung**
  - Exportieren als JSON-Datei über das Teilen-Menü des Handys (z. B. Dateien, iCloud Drive, OneDrive).
  - Importieren (ersetzen oder zusammenführen).
  - Erinnerung nach 7, 14, 30 Tagen oder nie. Ein Punkt am Reiter zeigt, wenn eine Sicherung fällig ist.

Die App startet nach mehr als 30 Sekunden im Hintergrund wieder im Kalender.

## So wird gerechnet

| Größe | Berechnung |
|-------|------------|
| Blutung | Aufeinanderfolgende Blutungstage, eine Lücke von 1 Tag wird noch zur selben Blutung gezählt |
| Zyklus | Abstand zwischen dem ersten Tag einer Blutung und dem ersten Tag der nächsten, nur Werte von 15 bis 60 Tagen zählen |
| Prognose Zykluslänge | Durchschnitt der letzten 6 bestätigten Zyklen, ohne Daten 28 Tage |
| Prognose Blutungsdauer | Durchschnitt der abgeschlossenen Blutungen, ohne Daten 5 Tage. Eine laufende Blutung zählt nicht mit |
| Eisprung | 14 Tage vor der nächsten Blutung (Lutealphase fest 14 Tage) |
| Fruchtbares Fenster | 5 Tage vor bis 1 Tag nach dem Eisprung |
| Zyklusphase | Blutung, Follikelphase, fruchtbares Fenster/Eisprung, Lutealphase |

Prognosen werden ab der letzten bestätigten Blutung in die Zukunft fortgeschrieben. Liegen sie in der Vergangenheit, bleiben sie gestrichelt, bis du Blutungstage einträgst. Der Eisprung ist eine Berechnung, keine Messung. Die App lernt nur aus der Zykluslänge, nicht aus dem tatsächlichen Eisprung.

## Startdaten

In index.html ist SEED eine leere Liste, die App startet also ohne Einträge. Die Blutungstage trägst du selbst in der App ein, sie bleiben nur auf deinem Gerät und stehen nicht im Programmcode. Wer Startdaten fest einbauen will, trägt sie in SEED ein (Format 'JJJJ-MM-TT'), sollte dann aber bedenken, dass sie im öffentlichen Repository lesbar wären. Wird SEED_V erhöht, werden Seed-Tage beim nächsten Start einmalig nachgetragen. OLD_SEED ist für frühere Startdaten gedacht, die dabei entfernt werden.

## Dateien

| Datei | Zweck |
|-------|-------|
| `index.html` | Die gesamte App: Oberfläche, Logik, Speicherung, Export und Import |
| `manifest.json` | App-Beschreibung für die Installation auf dem Home-Bildschirm |
| `sw.js` | Service Worker für den Offline-Betrieb |
| `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` | App-Symbol: Farbverlauf aus allen vier Farben, Mahagoni-Kreis mit weißem Ring und kleiner weißer Kugel (Ei) |
| `InterTight.ttf`, `OFL-InterTight.txt` | Schrift Inter Tight für Text und Überschriften (SIL Open Font License) |
| `Gaegu-Regular.ttf`, `OFL-Gaegu.txt` | Handschrift Gaegu für Akzente und Notizen (SIL Open Font License) |
| `README.md` | Diese Erklärung |

## Gestaltung

- Farben: Mahagoni `#442727`, White Smoke `#E6E7D9`, Light Camel `#D2C6B2`, Moosgrün `#747A2B`.
- Stil: milchig-transparente Karten auf weichem Farbverlauf, schwebende Tab-Leiste, wenige handgezeichnete Symbole und Notizen.
- Die Farben stehen als Variablen (`--mah`, `--smoke`, `--camel`, `--pis`) am Anfang der Styles in `index.html`. Das Moosgrün (`--pis`) kommt zusätzlich als `rgba(116,122,43, …)` und als Hex-Wert `#747A2B` im Code vor. Wer die Farbe ändert, ersetzt alle Vorkommen.

## Hosting auf GitHub Pages

1. Auf github.com ein neues **öffentliches** Repository anlegen, zum Beispiel `zyklus`. Das kostenlose GitHub Pages braucht ein öffentliches Repository.
2. Über **uploading an existing file** alle Dateien dieses Ordners hochladen. Sie müssen direkt im Hauptverzeichnis liegen, ohne Unterordner. Danach **Commit changes**.
3. Unter **Settings → Pages** bei Branch `main` und `/ (root)` wählen und speichern. Nach 1 bis 2 Minuten erscheint die Adresse `https://<name>.github.io/zyklus/`.
4. Auf dem iPhone die Adresse in **Safari** öffnen, **Teilen → Zum Home-Bildschirm** wählen. Danach nur noch die App vom Home-Bildschirm nutzen. Safari und die installierte App speichern ihre Daten getrennt.

### Aktualisieren

Geänderte Dateien über **Add file → Upload files** hochladen. Gleiche Dateinamen werden ersetzt. In `sw.js` die Version bei `CACHE` erhöhen (aktuell `zyklus-v5`), damit Geräte die neue Fassung laden. Für ein neues Icon die App auf dem iPhone löschen und neu hinzufügen.

## Datenschutz und Sicherung

- Die Einträge liegen nur im lokalen Speicher des Browsers auf dem Gerät. Das Repository enthält nur Programmcode, keine persönlichen Daten.
- Gehen die Browserdaten verloren (Cache löschen, Gerät gewechselt), sind die Einträge weg. Deshalb regelmäßig im Reiter "Sicherung" exportieren.
- Eine Web-App auf dem iPhone kann keinen festen Ordner automatisch beschreiben. Deshalb gibt es das Teilen-Menü und die Erinnerung.
- Die Sicherungsdatei nur im privaten Speicher ablegen, nicht im öffentlichen Repository.

### Format der Sicherungsdatei

```json
{
  "app": "zyklus",
  "version": 1,
  "exportedAt": "2026-10-15T12:00:00.000Z",
  "bleeding": ["2026-09-06", "2026-09-07"]
}
```

`bleeding` enthält alle Blutungstage als Datum `JJJJ-MM-TT`. Beim Import wird eine Datei mit falschem Format komplett abgelehnt.

## Lokal testen

Die App ist rein statisch. Ein beliebiger Webserver im Ordner genügt, zum Beispiel ein kleiner PowerShell-Server auf `http://localhost:8081/index.html`. Service Worker und Installation brauchen `https` (GitHub Pages) oder `localhost`.

## Technische Hinweise

- Speicherung: `localStorage`, Schlüssel `zyklus.v1` mit den Feldern `bleeding`, `lastExport`, `remind` und `seedV`.
- Die Datumsrechnung nutzt UTC-Tageszahlen, damit die Sommerzeit keine Fehler verursacht.
- Der Service Worker lädt zuerst aus dem Netz und fällt bei fehlender Verbindung auf den Zwischenspeicher zurück.
- Gaegu ist etwa 3 MB groß, weil die Schrift auch koreanische Zeichen enthält. Sie wird nur beim ersten Öffnen geladen und danach zwischengespeichert.
- Die Ernährungstexte stehen im Objekt `FOOD`, die Pop-up-Notizen im Objekt `NOTES` in `index.html`.
