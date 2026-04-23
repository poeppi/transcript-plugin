# transcript – Claude Code Plugin

Ein Claude Code Slash-Command, der YouTube-Transkripte herunterlädt und als Markdown-Datei speichert.

## Was der Skill macht

- Lädt auto-generierte Untertitel eines YouTube-Videos herunter
- Gibt das Transkript in der **Original-Sprache** und auf **Deutsch** aus
- Behält **Zeitmarken** im Format `[00:00:01]`
- Speichert das Ergebnis als `.md`-Datei mit Metadaten (Titel, Kanal, URL, Datum)

## Voraussetzung

`yt-dlp` muss installiert sein:

```bash
brew install yt-dlp        # macOS
pip install yt-dlp         # plattformübergreifend
```

## Installation

**Option A – Nur für ein Projekt** (empfohlen):

Kopiere die Datei in den `.claude/commands/`-Ordner deines Projekts:

```bash
cp .claude/commands/transcript.md /dein-projekt/.claude/commands/transcript.md
```

**Option B – Global für alle Projekte**:

```bash
cp .claude/commands/transcript.md ~/.claude/commands/transcript.md
```

## Verwendung

In Claude Code einfach aufrufen mit:

```
/transcript https://www.youtube.com/watch?v=XXXXXXXXX
```

Die fertige Datei landet in `outputs/transcripts/` deines Arbeitsverzeichnisses.

## Ausgabe-Format

```
# Videotitel

**Kanal:** Kanalname
**Datum:** YYYY-MM-DD
**URL:** https://...
**Erstellt:** YYYY-MM-DD

---

## Transkript – Original (en)

[00:00:01] Textzeile...

---

## Transkript – Deutsch

[00:00:01] Textzeile...
```

## Ordnerstruktur des Plugins

```
transcript-plugin/
├── .claude/
│   └── commands/
│       └── transcript.md   ← der Skill
└── README.md               ← diese Datei
```
