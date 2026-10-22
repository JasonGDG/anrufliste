# Anrufliste

Recherchiert Firmen für Kaltakquise am Telefon und baut daraus eine Excel-Anrufliste:
Prio, Verbund/Gruppe, Stufe A/B/C, Entscheider mit Durchwahl, Mitarbeiterzahl mit Quellen,
Erreichbarkeit, Kaufsignal (z. B. offene Stellen), „Anbieter schon da?“, Eignung,
4 bis 6 branchenpassende Spalten, Gesprächshinweis und Statusspalte zum Abhaken.
Dazu die Blätter Mitarbeiter-Quellen, Gesprächsleitfaden (passend zu eurem Angebot) und Legende.

## Benutzen (Claude Code im Browser)
1. Auf claude.ai/code eine neue Sitzung mit diesem Repo starten.
2. Eingeben, z. B.: `/anrufliste Logistikfirmen im Raum Osnabrück, 50 bis 300 Mitarbeiter, 40 Firmen`
3. Beim ersten Mal fragt Claude kurz, was ihr verkauft. Nach 10 bis 25 Minuten kommt ein
   GitHub-Link zur Excel-Datei: öffnen, oben rechts „Download raw file“.

Jede Liste wird unter `Anruflisten/` gespeichert. Firmen, die schon einmal geliefert wurden,
kommen nie wieder in eine neue Liste, auch wenn die alte Liste auf einem anderen Branch liegt.
Alte Listen von früher (Excel mit Spalte „Website“) in `Anruflisten/alt/` hochladen, dann
werden auch deren Firmen ausgeschlossen.

## Lokal (Claude Code im Terminal)
Ordner `.claude/skills/anrufliste` nach `~/.claude/skills/` kopieren. Listen landen dann in `~/Anruflisten/`.
