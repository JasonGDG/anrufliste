# Anrufliste

Ein Skill für Claude Code: Du sagst, wen du anrufen willst, Claude recherchiert die Firmen
und liefert eine fertige Excel-Anrufliste.

**In der Liste:** Prio, Stufe A/B/C, Entscheider mit Durchwahl, Telefon Zentrale, Website,
Mitarbeiterzahl mit Quellen, Erreichbarkeit, Kaufsignal (z. B. offene Stellen, neue Standorte),
„Anbieter schon da?“, Eignung, 4 bis 6 branchenpassende Spalten, Gesprächshinweis und
Statusspalte zum Abhaken. Dazu die Blätter Mitarbeiter-Quellen, Gesprächsleitfaden
(passend zu deinem Angebot) und Legende.

## Einrichten (einmalig)
1. Oben rechts auf **„Use this template“ → „Create a new repository“** klicken,
   Namen vergeben, am besten auf **Private** stellen, erstellen.
2. Auf [claude.ai/code](https://claude.ai/code) gehen, GitHub verbinden und dein neues Repo auswählen.

## Benutzen
In einer Sitzung mit deinem Repo eingeben, zum Beispiel:

```
/anrufliste Logistikfirmen im Raum Münster, 50 bis 300 Mitarbeiter, 40 Firmen
```

Beim ersten Mal fragt Claude kurz, was du verkaufst. Nach 10 bis 25 Minuten kommt ein Link
zur Excel-Datei: öffnen, oben rechts **„Download raw file“**.

## Gut zu wissen
- Jede Liste wird in deinem Repo unter `Anruflisten/` gespeichert. Firmen, die schon einmal
  geliefert wurden, kommen nie wieder in eine neue Liste.
- Alte Listen von früher (Excel mit Spalte „Website“) in `Anruflisten/alt/` hochladen,
  dann werden auch deren Firmen ausgeschlossen.
- Alle Angaben stammen aus öffentlichen Quellen (Websites, Impressum, Jobbörsen). Nummern
  vor dem Anruf kurz gegenprüfen, Websites ändern sich.
