# Schreibrichtigkeit Oberstufe – Projektnotizen

Nachschlagewerk mit Online-Übungen zur **Sprachrichtigkeit** (Zeichensetzung, später Rechtschreibung und Grammatik) für die Oberstufe und die Matura (SRDP Deutsch). Schwesterprojekt des Textsortenatlas (`../textsortenatlas-repo`), im selben Stil, aber eigenständig: eigenes Repo, eigener Inhalt.

Stand: **Teil 1 (Komma)** ist als Prototyp fertig. Rechtschreibung, Grammatik und Fehlerjagd folgen.

## Veröffentlichung
- Repo `schmidh2/schreibrichtigkeit-oberstufe` (öffentlich), live über GitHub Pages aus `main`: https://schmidh2.github.io/schreibrichtigkeit-oberstufe/ – Pushes auf `main` veröffentlichen sofort. Beim Testen `?v=2` an die Adresse hängen (Cache).
- `gh` ist angemeldet, `git push` funktioniert. Der gesamte Inhalt ist öffentlich lesbar.
- Die Kommaregeln sind noch nicht fachlich geprüft; Fehler werden nach dem Prüfen direkt auf `main` korrigiert.

## Technik
- Eine einzige, in sich geschlossene `index.html` (HTML + CSS + JS), keine Build-Schritte.
- **Schriften:** Das Design nennt **Fira Sans** (Text) und **Ubuntu** (Überschriften) wie die Schwesterseiten, lädt sie aber **nicht** von Google (Datenschutz). Ohne die Schriftdateien greift der Ersatz Arial/Helvetica. Für das identische Aussehen die Schriftdateien (Fira Sans und Ubuntu, freie Lizenzen) in einen Ordner der Seite legen und per `@font-face` einbinden; die Schriften der Schwesterseiten kommen noch von Google Fonts.
- Inhalte als Datenobjekte im `<script>` (`FAELLE`, `ENT`), die Seite wird per JS gerendert.
- **Design: Schulhomepage (gym.scp.ac.at)**, wie die Homepage-Variante des Textsortenatlas und die Unterstufen-Lernpfade: weißer Hintergrund, Blau `#009FE3` und `#0E3C88`, blauer Kopf mit vierfarbigen Reitern (Ecke unten rechts abgerundet), nur hell (kein Dunkelmodus). Tokens als CSS-Variablen, dazu `--ok`, `--warn`, `--red`. Kopf ist ab 821 px breit angeheftet, auf dem Handy nicht.
- Lernfortschritt nur lokal im Browser (`localStorage`, Schlüssel `srk.v1`, `{solved:{"<fall>:<index>":1}}`). Keine Konten, keine Freigaben, keine Kontrollen (für die Oberstufe nicht gewünscht).
- Lokal testen: `python3 -m http.server`.

## Aufbau von index.html
- **Navigation und Links:** Kopfreiter Grundidee, Entscheider, Auf einen Blick, Kommafälle, Mischtraining, Mein Profil, Matura, Weitere Seiten. Abschnitt **Was es hier gibt** (Kacheln: Komma und Fehlerprofil verfügbar, Rechtschreibung und Grammatik „In Planung“ ohne Link, damit kein toter Link entsteht). Unter „Kommafälle“ eine **Schnellwahl** (`#fallChips`). Sprungmarken `#f-<fall>` öffnen die Karte (`ausHash()`). Abschnitt **Weitere Seiten** verlinkt Textsortenatlas, Deutsch-Lernpfad Unterstufe und Digitale Grundbildung (alle erreichbar geprüft am 23.11.2026); Fußzeile verlinkt die Schulhomepage. Neue Links immer mit `target="_blank" rel="noopener noreferrer"`; Link-Prüfung: Anker gegen vorhandene IDs, externe Adressen per HTTP-Status.
- **Grundidee** (drei Schritte: Personalformen markieren, Grenzen prüfen, Rest abklopfen), **Entscheider**, **Auf einen Blick** (Tabelle aus `FAELLE`), **Kommafälle** (je eine aufklappbare Karte), **Mischtraining**, **Matura**.
- `FAELLE`: je Fall `id`, `titel`, `tag`, `kurz`, `blick`, `regel`, `probe`, `beispiele`, `fallen`, optional `tipp`, `saetze`.
  - Fälle: Aufzählung, Hauptsätze (Satzreihe), Nebensatz, Relativsatz, Infinitivgruppe, Partizip- und Adjektivgruppen, Zusätze/Anrede/Ausruf, wörtliche Rede. Das sind 75 Übungssätze.
  - Neuer Fall: Objekt in `FAELLE` ergänzen, dann entstehen Karte, Tabellenzeile, Übung und Mischtraining von selbst. Im Entscheider `ENT` ein Ergebnis mit `fall:['<id>']` verknüpfen.
- **Satzformat für den Komma-Setzer** (`saetze[].s`): Wörter durch Leerzeichen getrennt; ein angehängtes `,` am Wort = Pflicht-Komma danach, ein angehängtes `^` = freigestelltes Komma. `e` = Erklärungen in der Reihenfolge der Kommas (fehlen welche, wird die letzte wiederholt), `h` = Erklärung, wenn kein Komma steht. Am letzten Wort darf keine Marke stehen.
- **Beispiele und Fehler** (`beispiele`, `fallen`): Kommas werden automatisch hervorgehoben; `[[x]]` markiert eine Fehlerstelle, `[[fehlt]]` ein fehlendes Komma. Funktion `markup()`.
- **Setzer** (`mountSetzer`): Tippen auf ein Wort setzt oder entfernt das Komma dahinter (Touch-tauglich). Prüfen zeigt richtig, fehlend, zu viel und freigestellt, dazu die Begründung je Komma. Ein Satz zählt als gelöst, wenn er einmal fehlerfrei war.
- **Entscheider** (`ENT`): Knoten sind Fragen (`f`, optional `hilfe`, `o:[[Text, Ziel]]`) oder Ergebnisse (`r:'pflicht'|'kein'|'frei'`, `titel`, `text`, `ex`, `fall`). Zurück und Neu beginnen sind eingebaut.
- Eine Datenprüfung (Anzahl Erklärungen je Satz, Ziele im Entscheider) lässt sich schnell per Node-Skript schreiben: `FAELLE` und `ENT` per Regex aus der Datei holen und auswerten. Beim Prototyp lief sie fehlerfrei.

## Arbeitsblätter (Druckvorschau)
- Jede Karte hat unten das Panel „Zum Ausdrucken“ mit **Arbeitsblatt** und **Lösungsblatt**; im Mischtraining gibt es **Mischblatt drucken** (Arbeitsblatt und Lösung auf getrennten Seiten, gleiche Sätze in gleicher Reihenfolge). Alles wird aus `FAELLE` erzeugt, es gibt keine PDF-Dateien zu pflegen.
- **Arbeitsblatt:** Kopf mit Name/Klasse/Datum, Regel und Probe, Aufgabe 1 (alle Übungssätze ohne Kommas), Aufgabe 2 (die „falsch“-Sätze der Karte verbessern), Aufgabe 3 (eigene Sätze). **Lösungsblatt:** Sätze mit roten Kommas und Begründung je Komma, „(,)“ = freigestellt (Hinweis nur, wenn vorhanden), richtige Fassungen zu Aufgabe 2.
- Funktionen: `blattFall(f, loesung)`, `blattMisch()`, `zeigeVorschau()`, `oeffneBlatt()`. Die Vorschau ist ein Overlay (`#vorschau`); „Drucken“ ruft `window.print()` auf, im Druck sind alle anderen Teile der Seite ausgeblendet (`body.v-offen`). Dateiname im PDF: Seitentitel wird kurz auf den Blattnamen gesetzt.
- **Direktlinks:** `?blatt=ab:<fall>` (Arbeitsblatt) und `?blatt=ls:<fall>` (Lösungsblatt), zum Beispiel `?blatt=ab:nebensatz`. Taugt als kostenloses Muster auf einer Landingpage.
- Die Fußzeile nennt Stand, Quelle und die Adresse der Seite; solange der Prototyp-Hinweis (`.hinweis`) auf der Seite steht, steht auch „Entwurf, wird noch fachlich geprüft“ auf den Blättern.
- **Getestet** im Browser und als echter Druck (Chrome headless, PDF, A4). Klassenkollision beachten: `.fehler` gehört zu den Karten, die Blätter nutzen `.b-fehler`. Aufgaben tragen `.b-aufg` (kein Seitenumbruch mitten in einer Aufgabe).
- **Noch kein Zugangsschutz:** Die Blätter zeigen nur, was auf der Seite ohnehin offen steht. Für ein bezahltes Lehrkräfte-Paket müsste der Zugang wie bei den Unterstufen-Projekten verschlüsselt werden (siehe `../Plan_Lehrkraefte-Paket_Oberstufe.md`).

## Mitdenkende Seite (Stufe 1: lokal, ohne KI, ohne Server)
- **Fehlerprofil** (`#profil`, `renderProfil()`): `prog.stat[fall] = {n, ok, fehlend, zuviel, verlauf[], zuletzt}`. Gezählt wird nur der **erste Versuch** je Satz (`recordResult()` im Setzer). Das Profil zeigt je Fall die Quote, die letzten 10 Versuche, die **Fehlerart** (meist fehlende oder meist überzählige Kommas) und einen Gesamttipp ab 8 Sätzen.
- **Empfehlung:** bis zu zwei Fälle mit mindestens 3 Versuchen und unter 75 % richtig, mit Button „Neue Sätze üben“ bzw. „Sätze üben“ und „Karte öffnen“. Ohne Daten ein Startvorschlag, sonst „Gut unterwegs“ mit Hinweis auf noch nicht geübte Fälle.
- **Wiederholung** (Leitner): `prog.box[satzId] = {s, due}`, Abstände `TAGE = [0, 1, 3, 7, 14]`. Richtig: eine Stufe höher, Fälligkeit nach dem Abstand; falsch: Stufe 0, sofort wieder fällig. Nur **feste** Sätze (stabile IDs `<fall>:<index>`). Wenn Sätze in `FAELLE` umsortiert werden, passen alte Einträge nicht mehr: nur hinten anhängen (wie bei den Freigabe-Codes der Unterstufe).
- **Sätze aus Bausteinen** (`GEN`, `genItems()`): Für **6 Fälle** (Aufzählung, Hauptsätze, Nebensatz, Relativsatz, Infinitivgruppe, wörtliche Rede; nicht Partizipgruppen und Zusätze) erzeugt jeder Generator Sätze aus fest geschriebenen, zueinander passenden Teilen. Die Kommas ergeben sich aus dem Aufbau, die Lösung ist also sicher. Jeder Generator liefert `{s, e, h}` im Format der festen Sätze. Neue Bausteine: Liste erweitern, nur grammatisch und inhaltlich passende Kombinationen; Prüfung durch 500 Läufe je Generator (Formatregeln: keine Marke am letzten Wort, Erklärung je Komma, kein Doppelkomma) und Durchsicht von Beispielsätzen.
- **Erzeugte Sätze** zählen für das Fehlerprofil, aber nicht für „gelöst“ und nicht für die Wiederholung (`it.gen`). Sie stehen in den Karten (Schalter „Feste Sätze / Neue Sätze“) und im Mischtraining („neue Sätze“) bereit. Es sind keine Freitexte: Die Seite erzeugt nichts frei und kennt keine eigenen Sätze der Schüler:innen.
- Fortschritt `srk.v1`: `{solved, stat, box}`; „Fortschritt zurücksetzen“ leert alles. Alles bleibt lokal im Browser, nichts wird übertragen.
- **Nicht gebaut (Stufe 2 und höher):** Klassenübersicht aus Ergebnis-Codes, KI-Vorschläge mit Server, private Fragen an die Seite. Siehe `../Plan_Lehrkraefte-Paket_Oberstufe.md` und die Stufenüberlegung (Datenschutz, Kosten, Lehrkraft bestätigt jeden KI-Vorschlag).

## Inhaltliche Regeln
- Regeln folgen dem **amtlichen Regelwerk** (Rat für deutsche Rechtschreibung). Wo es das Komma freistellt (Infinitivgruppen ohne um/ohne/statt, Substantiv oder Hinweiswort; „und/oder“ zwischen vollständigen Hauptsätzen), ist beides richtig und wird als freigestellt gekennzeichnet. Paragrafennummern nur nennen, wenn sie im Regelwerk selbst nachgelesen sind (geprüft: § 75, § 77(5), § 77(7), § 78(3)).
- Alle Beispielsätze sind eigene. Keine geschützten Texte.
- **Partizip- und Adjektivgruppen** (Karte `partizip`): bewusst nur die **Pflichtfälle** (amtliches Regelwerk, Fassung 2016, redigiert 2018, und Duden D 127): (1) Hinweiswort kündigt an, (2) Hinweiswort nimmt wieder auf (beides § 77(5)), (3) Nachtrag direkt hinter Substantiv/Pronomen und (4) angehängter Nachtrag am Satzende (beides § 77(7); Duden D 127.2 und 127.3). Freigestellt ist das Komma nach § 78(3), wenn die Gruppe nur zur Aussage gehört (Satzanfang ohne Hinweiswort, nach dem Verb: „Sie kam(,) aus vollem Halse lachend(,) auf mich zu“). Dazu gibt es **keine Übungssätze**, weil der Setzer jedes nicht erwartete Komma als Fehler wertet. Die Übungssätze zum Satzende sind eigene, eindeutige Nachträge (nachgestellte Gruppe nach vollständigem Satz); Grenzfälle wie „Sie saß auf der Terrasse ganz in Decken verpackt“ bewusst weggelassen, weil weder Regelwerk noch Duden ein Kriterium für „Zusatz“ nennen.
- Keine Links auf Seiten, die Bücher oder Texte verkaufen (Verlage, Shops, Partnerlinks). Erlaubt: matura.gv.at, bmb.gv.at, rechtschreibrat.com.
- Wie die Sprachrichtigkeit im Beurteilungsraster gewichtet wird, steht nicht im Text, sondern nur als Link zum amtlichen Raster (vor einer Aussage dazu den Raster lesen).
- Österreichische Anführungszeichen „…“. Umlaute und Sonderzeichen in URLs beachten.

## Offen / nächste Schritte
- **Inhaltlich prüfen lassen:** alle Kommaregeln und Erklärungen, besonders Infinitivgruppen, Datum im Satz und „zum Beispiel/und zwar“. Noch nicht mit Schüler:innen erprobt.
- Tests (Komma) mit Lösungsschlüssel und Export, Korrekturbogen, Einsatzvorschläge (Lehrkräfte-Paket, siehe Plan).
- Weitere Kommafälle: Komma bei Gliederungswörtern (erstens, schließlich), Komma in Briefen (Anrede, Datum, Anschrift), Komma bei Zahlen.
- Teil 2 **Rechtschreibung**: Groß- und Kleinschreibung (Nominalisierung, Tageszeiten), getrennt/zusammen, das/dass, Fremdwörter, österreichische Besonderheiten.
- Teil 3 **Grammatik**: Kasus und Präpositionen, Kongruenz, Tempus, Konjunktiv bei indirekter Rede, Passiv und Nominalstil, Satzbrüche.
- **Fehlerjagd** mit eigenen Matura-ähnlichen Texten und **Fehlerprofil** (lokal): zählt Fehlerarten mit und schlägt passende Karten vor.
- Verknüpfung mit dem Textsortenatlas (Zitieren, indirekte Rede) und eine Kommaübung mit längeren Absätzen statt Einzelsätzen.
- Später eventuell auf den eigenen WebSpace mit Domain umziehen (der lokale Fortschritt im Browser geht beim Adresswechsel verloren).
