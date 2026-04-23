---
description: Lädt das Transkript eines YouTube-Videos (Original + Deutsch) und speichert es in outputs/transcripts/
---

Du erhältst eine YouTube-URL: $ARGUMENTS

Führe diese Schritte der Reihe nach aus:

---

## Schritt 1: yt-dlp prüfen

Prüfe mit `which yt-dlp`, ob das Tool installiert ist.

Falls **nicht installiert**: Teile das klar mit und empfehle:
```
brew install yt-dlp
```
Dann brich ab – ohne weitere Schritte.

---

## Schritt 2: Verfügbare Untertitel anzeigen

Liste die verfügbaren Untertitelspuren auf:
```
yt-dlp --list-subs --skip-download "$URL"
```

Notiere dir: Welche Sprachen sind verfügbar (manuell oder auto-generiert)?

---

## Schritt 3: Video-Metadaten abrufen

Hole Titel, Kanal und Upload-Datum:
```
yt-dlp --print "%(title)s" --print "%(uploader)s" --print "%(upload_date)s" --skip-download "$URL"
```

Erstelle aus dem Titel einen sicheren Dateinamen: Leerzeichen → Bindestriche, Sonderzeichen entfernen, alles lowercase. Beispiel: `ki-im-journalismus-2024-04-23`.

---

## Schritt 4: Untertitel herunterladen

Lade auto-generierte Untertitel im SRT-Format nach `/tmp/yt-transcript/` herunter.

Versuche zuerst Original-Sprache + Deutsch:
```
yt-dlp --write-auto-sub --skip-download --convert-subs srt --sub-langs "en,de" -o "/tmp/yt-transcript/%(title)s" "$URL"
```

Falls Englisch nicht die Original-Sprache ist, passe `--sub-langs` entsprechend an (z. B. `"de"` für deutschsprachige Videos).

---

## Schritt 5: SRT-Dateien bereinigen

Lies die heruntergeladenen `.srt`-Dateien. Verarbeite sie mit Python oder awk:
- Entferne Nummerierungen (1, 2, 3 …)
- Entferne leere Zeilen zwischen den Blöcken
- Wandle Zeitmarken von `00:00:01,000 --> 00:00:04,000` um in `[00:00:01]`
- Behalte den Textinhalt
- Entferne HTML-Tags wie `<i>`, `</i>`, `<font>` etc.
- Dedupliziere direkt aufeinanderfolgende identische Zeilen (häufig bei auto-generierten Untertiteln)

---

## Schritt 6: Ausgabedatei erstellen

Speichere das Ergebnis unter:
```
outputs/transcripts/[dateiname]-[YYYY-MM-DD].md
```

Verwende dieses Format:

```
# [Videotitel]

**Kanal:** [Kanalname]
**Datum:** [Upload-Datum]
**URL:** $ARGUMENTS
**Erstellt:** [heutiges Datum]

---

## Transkript – Original ([Sprache])

[00:00:01] Textzeile...
[00:00:05] Textzeile...

---

## Transkript – Deutsch

[00:00:01] Textzeile...
[00:00:05] Textzeile...
```

Falls keine deutschen Untertitel verfügbar sind, schreibe stattdessen:
```
## Transkript – Deutsch

*Keine deutschen Untertitel für dieses Video verfügbar.*
```

---

## Schritt 7: Abschluss

Teile der Nutzerin mit:
- Wo die Datei gespeichert wurde (relativer Pfad)
- Wie viele Zeichen / Absätze das Transkript hat
- Ob deutsche Untertitel gefunden wurden oder nicht
