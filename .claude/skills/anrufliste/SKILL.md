---
name: anrufliste
description: Recherchiert Firmen für Kaltakquise am Telefon und baut daraus eine Excel-Anrufliste (Prio, Stufe A/B/C, Entscheider mit Durchwahl, Mitarbeiterzahl mit Quellen, Kaufsignal, Statusspalte). Verwenden bei /anrufliste, „mach mir eine Anrufliste“, „such mir Firmen zum Anrufen“, „Leads für …“, „noch so eine Liste“.
---

# Anrufliste

Du orchestrierst. Die Recherche machen 3 bis 4 parallele Unteragenten, das Excel baut `build.py` (liegt neben dieser Datei). Alle Listen landen in einem Ordner `Anruflisten/`, damit keine Firma zweimal geliefert wird:
- **Skill liegt in einem Repo** (`<repo>/.claude/skills/anrufliste`, z. B. Claude Code im Browser): `<repo>/Anruflisten/`. `build.py` liest dann auch die Listen auf allen anderen Branches mit.
- **Skill liegt lokal** (`~/.claude/skills/anrufliste`): `~/Anruflisten/`.
`build.py neu` gibt den richtigen Ordner aus, du musst das nicht selbst entscheiden.

## 1. Auftrag klären
Aus der Anfrage ablesen, nur Fehlendes kurz nachfragen (eine Nachricht, höchstens 3 Fragen):
- **Angebot:** Was verkauft der Nutzer? (Steht es in einer früheren `~/Anruflisten/*/auftrag.json`, nicht neu fragen, sondern übernehmen.)
- **Zielkunden:** Branche, Größe (Mitarbeiter), Art der Firma.
- **Region** und **Anzahl** (Standard 40).

Dann selbst festlegen (nicht fragen, in der Antwort kurz nennen):
- **Kaufsignal**: woran man erkennt, dass die Firma *jetzt* Bedarf hat, abgeleitet aus dem Angebot. Personaldienstleister → offene Stellen (welche, wie viele, seit wann); Software → Stellenanzeigen für die Aufgabe, Wachstum; Handwerk/Bau → neue Standorte, Bauprojekte. Name der Spalte (`signal_name`), z. B. „Offene Stellen“.
- **Zusatzspalten** (0 bis 4), die für dieses Angebot beim Anruf helfen, z. B. Standorte, Branche genau, Schichtbetrieb, Tarifbindung.
- **Teilgebiete**: Region in 3 bis 4 etwa gleich ergiebige Teile schneiden (nach Landkreisen/Städten), Anzahl gleichmäßig verteilen.

## 2. Ordner anlegen
```
python3 <skill-ordner>/build.py neu "<kurzer Titel, z. B. Logistik Westfalen>"
```
Gibt den Ordner aus. Darin `auftrag.json` schreiben:
```json
{"titel": "Logistik Westfalen", "datum": "JJJJ-MM-TT", "angebot": "…", "zielprofil": "…", "region": "…",
 "min_mitarbeiter": 20, "signal_name": "Offene Stellen", "signal_definition": "…",
 "extra_spalten": [["standorte", "Standorte", 20], ["branche", "Branche", 18]],
 "teilgebiete": ["…", "…", "…"]}
```
(`min_mitarbeiter` weglassen, wenn es keine Untergrenze gibt.) Dann:
```
python3 <skill-ordner>/build.py ausschluss <ordner>
```

## 3. Recherche parallel starten
`recherche-auftrag.md` lesen und je Teilgebiet einen Unteragenten starten, **alle in einer Nachricht**, `subagent_type: "general-purpose"`, `model: "sonnet"`, im Hintergrund. Prompt = der komplette Text von `recherche-auftrag.md` mit ersetzten Platzhaltern ({ANGEBOT}, {ZIELPROFIL}, {TEILGEBIET}, {ANZAHL}, {MINIMUM} ≈ 80 %, {SIGNAL_NAME}, {SIGNAL_DEFINITION}, {EXTRA_FELDER} = Schlüssel mit Bedeutung, {ORDNER}, {DATEI} = `teil1`, `teil2` …, {AUSSCHLUSS_EXTRA} = Wünsche des Nutzers, sonst leer).
Dem Nutzer in einem Satz sagen, was läuft und wie lange es etwa dauert (10 bis 25 Minuten).

## 4. Prüfen und bauen
Wenn alle Agenten fertig sind:
- Firmen streichen, die klar nicht passen (der Agent hat sie selbst als zu groß, zu klein oder falsche Branche markiert): den Eintrag aus der JSON-Datei löschen.
- `python3 <skill-ordner>/build.py excel <ordner>` ausführen. Meldungen „Dublette“ / „schon früher geliefert“ sind normal.
- Ein Teilgebiet deutlich unter Soll? Einmal nachlegen lassen (SendMessage an denselben Agenten), sonst ehrlich melden.

## 5. Im Repo: speichern und hochladen
Liegt der Ordner in einem Git-Repo (Browser-Sitzung), den kompletten Auftragsordner (auftrag.json, data/, Excel) committen und pushen, auf den Branch dieser Sitzung („Anrufliste <Titel>“). Nur so kann der Nutzer die Datei herunterladen, und nur so kennt die nächste Liste diese Firmen. Den GitHub-Link zur Excel-Datei ermitteln (`git remote get-url origin` + Branch + Pfad, Form `https://github.com/<owner>/<repo>/blob/<branch>/<pfad>`) und in der Antwort nennen. Hinweis an den Nutzer: dort oben rechts auf „Download raw file“ klicken.

## 6. Antwort an den Nutzer
Kurz und einfach, ohne Fachjargon:
- Lokal: Pfad der Excel-Datei (vollständig) und `open <pfad>` ausführen. Im Browser: der GitHub-Link aus Schritt 5.
- Anzahl Firmen, Verteilung auf die Teilgebiete, Stufe A/B/C, wie viele mit starkem Kaufsignal.
- Die 3 bis 5 besten Firmen zum Starten (Prio 1 bis 5) mit je einem Halbsatz warum.
- Auffälligkeiten (Übernahmen, Grenzfälle, Gebiet ausgeschöpft) als kurze Stichpunkte.

## Regeln
- Nichts erfinden. Nummern, Namen, Stellen nur mit Quelle.
- Niemanden kontaktieren, keine Formulare absenden.
- Alte Listen, die nicht von diesem Skill stammen, kann der Nutzer als .xlsx (mit Spalte „Website“) oder .txt (eine Domain pro Zeile) in `Anruflisten/alt/` legen (im Repo: auf den Hauptbranch hochladen); sie werden dann ausgeschlossen.
