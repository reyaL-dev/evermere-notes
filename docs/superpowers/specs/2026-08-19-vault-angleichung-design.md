---
datum: 2026-08-19
typ: design-spec
status: entwurf
tags:
  - meta
  - spec
---

# Design: Vault "Evermere" an "Dragonlance" angleichen

## Ziel

Das Vault `Evermere` erhält dieselbe Ordnerstruktur, dieselben Notiz-Konventionen
und dieselbe Obsidian-Konfiguration wie das etablierte Vault `Dragonlance`
(`C:\Users\Sameen\Dropbox\DND\Dragonlance\`). Danach lassen sich beide Vaults
identisch bedienen: gleiche Templates, gleiche Dataview-Pfade, gleiche
Frontmatter-Felder.

## Ausgangslage

Beide Vaults teilen bereits: alle fünf Templates (`Fraktion`, `NPC`, `Ort`,
`Session`, `Spieler`) byte-identisch, alle drei CSS-Snippets, dasselbe
Plugin-Set, identische `core-plugins.json`.

Abweichungen in Evermere:

| Bereich | Evermere | Dragonlance |
|---|---|---|
| Ordnernamen | `Charaktere`, `Orte` | `1_Charaktere`, `2_Orte` (nummeriert) |
| Journale | fehlt | `0_Journale/` |
| Fraktionen | `Organisationen/` (top-level) | `1_Charaktere/Fraktionen/` |
| NPC-Gliederung | `NPC/Entitäten`, `NPC/Freie Völker` (flach) | `NPC/Freie Völker/<Fraktion>/`, `NPC/Pantheon/Götter` |
| Orte-Gliederung | `Orte/Land/` | `Regionen`, `Städte`, `Spezielle Orte` |
| Quests | `Quests/` (leer) | existiert nicht |
| `app.json` | nur `alwaysUpdateLinks` | + `promptDelete`, `userIgnoreFilters` |
| `appearance.json` | Theme ohne Snippet | + `enabledCssSnippets` |
| `.gitignore` | fehlt | vorhanden |

Zusätzlich enthalten mehrere Evermere-Notizen Altlasten (leere Dateien,
doppelter Inhalt, fehlende Frontmatter, kopierte Platzhalter).

## Entscheidungen

Vom Nutzer bestätigt:

1. **Umfang:** Struktur + Konfiguration + Notiz-Inhalte.
2. **Quests:** Der leere `Quests/`-Ordner wird gelöscht (strikte Dragonlance-Parität).
3. **Spieler-Notizen:** Auf das volle Spieler-Template heben; die in vier Dateien
   identisch kopierte Kurzcharakterisierung wird zum leeren Platzhalter.
4. **Leere Notizen:** Template-Gerüst plus ausschließlich solche Fakten, die in
   anderen Vault-Notizen bereits belegt sind. Es wird nichts dazuerfunden.
5. **Frontmatter:** Auf Template-Felder in Template-Reihenfolge normalisieren;
   `fraktion` → `gruppe`, `wesen` → `rasse`. Nicht-Template-Felder mit echtem
   Informationsgehalt (`gefährte`, `gegner`) bleiben unten angehängt erhalten.

## Zielstruktur

```
0_Journale/                         NEU (leer)
1_Charaktere/                       ← Charaktere/
  Fraktionen/                       ← Organisationen/
    Zirkel der Mondfedern.md
  NPC/
    Entitäten/
      Baba Hilda.md
      Zuya.md
    Freie Völker/
      Unabhängige/                  ← NPC/Freie Völker/ (flach)
        Blanche.md
        Roselia.md
  Spieler/
    Corvina.md
    Dalia Lunaria.md
    Keira Krabalde.md
    Leryn Linmund.md
    Valdrik Eldefur.md
    Vényáma.md
2_Orte/                             ← Orte/
  Regionen/                         ← Orte/Land/
    Neverwood.md
  Städte/                           NEU (leer)
  Spezielle Orte/                   NEU (leer)
z_Bilder/                           unverändert
z_Templates/                        unverändert
docs/superpowers/specs/             diese Spec
```

Entfernt: `Quests/`, `Organisationen/`, `Orte/Land/`, `Charaktere/`, `Orte/`.

**Begründung der Zuordnungen:**

- `Entitäten` bleibt als Kategorie erhalten — es ist Evermeres Pendant zu
  Dragonlances `Pantheon` (übernatürliche Wesen statt sterblicher NPCs).
- `Blanche` und `Roselia` gehören keiner Fraktion an; Dragonlance sammelt solche
  NPCs unter `Freie Völker/Unabhängige/`.
- `Neverwood` ist ein Waldgebiet, also eine Region, nicht ein spezieller Ort.
- Leere Ordner (`0_Journale`, `Städte`, `Spezielle Orte`) werden von Git nicht
  versioniert. Sie existieren lokal und erscheinen im Repository, sobald die
  erste Notiz darin liegt. In Dragonlance verhält es sich genauso.

## Notiz-Änderungen

### Charaktere/NPC/Entitäten/Zuya.md

Die Datei enthält **zwei vollständige, aneinandergehängte Versionen** derselben
Notiz. Version 1 hat Wikilinks, aber unsauberes Markdown (doppelte `---`-Marker,
verstreute Leerzeilen, Tippfehler `[[Neverwood]]l iegt`). Version 2 ist sauber
formatiert und hat zusätzliche Spoiler-Callouts, verwendet aber Klartext statt
Wikilinks.

Ergebnis: **eine** Notiz auf Basis von Version 2, ergänzt um die Wikilinks aus
Version 1 (`[[Keira Krabalde]]`, `[[Baba Hilda]]`, `[[Neverwood]]`,
`[[Vényáma]]`) und um die Bildeinbindung `![[Zuya_Bild.png]]`.

Frontmatter nach NPC-Template: `wesen` → `rasse: Stryx`, `gefährtin-von` →
`gefährte`, `gegenspielerin-von` → `gegner`, Tag-Liste in Blockschreibweise.

### Charaktere/NPC/Entitäten/Baba Hilda.md

Datei ist vollständig leer. Erhält NPC-Template plus die in `Keira Krabalde.md`
und `Zuya.md` belegten Fakten:

- Antagonistin des Neverwood-Handlungsstrangs
- Ihr Vertrautentier ist die dunkle Hälfte desselben Stryx-Geistes, aus dem Zuya
  hervorging
- Verbreitet Fluch, Kontrolle und Verderbnis; ihr Einfluss ließ Trolle häufiger
  auftreten und Bewohner im Wald verschwinden
- Hatte Keira und deren Gruppe zeitweise unter Fluch und Kontrolle
- Ein von ihr verdorbenes Wesen tötete die Kräuterkundlerin von Keiras Dorf

Nicht belegte Felder (`pronomen`, `rang`, `rolle`, `domäne`) bleiben leer.

### Charaktere/NPC/Freie Völker/Blanche.md, Roselia.md

Beide sind bereits template-konform, aber ohne Inhalt. Ergänzt wird die
Kurzcharakterisierung aus dem Abschnitt „NPC" in `Keira Krabalde.md`:

- Blanche: Druidin, etablierte Heilerin, sehr ruhiges Gemüt
- Roselia: Rangerin, rotes im Sonnenlicht schimmerndes Haar, deutlich
  aufgeweckter, gefragt für Tracking und Fernkampf

Beide gewährten Keira wiederholt Obdach und Hilfe — kommt unter „Bezug zu den
Spielercharakteren".

### Orte/Land/Neverwood.md

Hat weder Frontmatter noch Template-Struktur, nur zwei Absätze Fließtext.
Erhält das Ort-Template: `typ: Wald`, `status: verflucht`, `tags: [ort]`. Der
vorhandene Text („Ein verfluchter Wald…") wird zur Kurzcharakterisierung und
zum Beschreibungsabsatz; der Abschnitt „Der Silberne Hirsch" wandert unter
`# Trivia`.

### Organisationen/Zirkel der Mondfedern.md

Datei ist vollständig leer. Erhält Fraktion-Template, gefüllt aus dem Abschnitt
„Reisen und Ziele" in `Keira Krabalde.md`:

- `typ: Loser Zirkel` (ausdrücklich kein Orden, keine feste Organisation)
- `anführer: "[[Keira Krabalde]]"`, `status: im Aufbau`
- Zweck: freie Völker mit gutem Herzen sammeln, Wissen teilen, Leben schützen,
  den Einfluss böser Mächte im Neverwood zurückdrängen
- Mitglieder: `[[Keira Krabalde]]`

### Charaktere/Spieler/ — Corvina, Leryn Linmund, Valdrik Eldefur, Vényáma

Alle vier haben dieselbe kopierte Kurzcharakterisierung, ein reduziertes
Frontmatter und den Tag `pnp` statt `spieler`.

- Frontmatter auf volle Template-Feldliste in Template-Reihenfolge
  (`name`, `aliases`, `pronomen`, `spieler`, `rasse`, `rang`, `rolle`, `klasse`,
  `alter`, `ort`, `status`, `tags`); vorhandene Werte (`spieler`, `rasse: unbekannt`,
  `rolle: unbekannt`) bleiben erhalten, `fraktion: unbekannt` wird zu `gruppe`
- Tags: `charakter`, `spieler`
- Body auf Spieler-Template: Infobox, `# Intro`, `# Beschreibung` mit `## Aussehen`
  und `## Persönlichkeit`, `# Biographie` mit `## Hintergrund` und `## Quest`,
  `# Beziehungen`, `# Trivia`
- Vorhandene leere Abschnitte (`## Herkunft`, `## Reisen und Ziele`) gehen unter
  `# Biographie` auf
- Kurzcharakterisierung wird geleert

### Charaktere/Spieler/Dalia Lunaria.md

Frontmatter ist bereits nahezu vollständig; nur `fraktion` → `gruppe` und Tag
`pnp` entfernen. Body auf Spieler-Template heben.

### Charaktere/Spieler/Keira Krabalde.md

Inhaltlich die am weitesten ausgearbeitete Notiz — der Text bleibt unangetastet.
Angepasst wird nur die Struktur:

- `fraktion: "[[Zirkel der Mondfedern]]"` → `gruppe:`
- Die Abschnitte `## Hintergrund`, `## Das Dorf am Neverwood`, `## Zuya und der
  Pakt`, `## Reisen und Ziele` unter eine `# Biographie`-Überschrift einordnen
  (Template-Hierarchie)
- Selbstverweis `### [[Keira Krabalde]]` unter „Zu den Spielercharakteren"
  entfernen
- Tag `neverwood` bleibt erhalten (Vault-spezifische Zusatzkategorie)

## Template- und Konfigurationsänderungen

### z_Templates/Session.md

`gruppe: Dragonlance` → `gruppe: Evermere`. Übernahmefehler aus dem Quell-Vault.
Die enthaltenen Dataview-Abfragen verweisen auf `1_Charaktere/Spieler`,
`1_Charaktere/NPC`, `1_Charaktere/Fraktionen`, `2_Orte` und `0_Journale` — sie
liefern erst nach dem Ordnerumbau Ergebnisse. Die Abfragen selbst bleiben
unverändert.

### .obsidian/app.json

```json
{
  "promptDelete": false,
  "alwaysUpdateLinks": true,
  "userIgnoreFilters": ["z_Templates/", "z_Bilder/"]
}
```

### .obsidian/appearance.json

```json
{
  "cssTheme": "ITS Theme",
  "enabledCssSnippets": ["supercharged-links-gen"]
}
```

Das Snippet `supercharged-links-gen.css` liegt bereits in Evermere, war aber nie
aktiviert.

### .gitignore

Wird von Dragonlance übernommen, ohne dessen session-spezifische Einzeleinträge
(`conv-*.json`), da `/.claudian/*` diese ohnehin abdeckt:

```
/.obsidian/workspace.json
/.obsidian/plugins/realclaudian/manifest.json
/.obsidian/plugins/realclaudian/main.js
/.obsidian/plugins/realclaudian/styles.css
/.obsidian/community-plugins.json
/.obsidian/plugins/realclaudian/data.json
/.claudian/*
```

### .obsidian/text-generator.json

Verwaiste Konfiguration eines Plugins, das weder installiert noch aktiviert ist.
Wird gelöscht.

### Nicht angefasst

- `community-plugins.json` — unterscheidet sich nur in der Reihenfolge
- `graph.json` — enthält nur den nutzerspezifischen Zoomfaktor
- `workspace.json` — reiner Sitzungszustand
- `z_Bilder/` — alle fünf Bilder sind referenziert und bleiben liegen

## Vorgehen und Absicherung

1. Sicherungs-Commit vor der ersten Änderung.
2. Alle Verschiebungen mit `git mv`, damit die Datei-Historie erhalten bleibt.
3. Reihenfolge: erst Ordnerumbau, dann Notiz-Inhalte, dann Konfiguration. So
   arbeiten die Inhalts-Edits bereits auf den Zielpfaden.
4. Ein Abschluss-Commit.

**Wikilink-Sicherheit:** Sämtliche Wikilinks im Vault sind pfadlos
(`[[Neverwood]]`, nicht `[[Orte/Land/Neverwood]]`). Obsidian löst sie über den
Dateinamen auf, das Verschieben kann sie daher nicht brechen. Dateinamen ändern
sich nicht.

## Erfolgskriterien

Nach Abschluss gilt:

1. Der Ordnerbaum (ohne `.git`, `.obsidian`, `.claudian`, `docs`) lautet exakt:
   `0_Journale`, `1_Charaktere`, `1_Charaktere/Fraktionen`, `1_Charaktere/NPC`,
   `1_Charaktere/NPC/Entitäten`, `1_Charaktere/NPC/Freie Völker`,
   `1_Charaktere/NPC/Freie Völker/Unabhängige`, `1_Charaktere/Spieler`,
   `2_Orte`, `2_Orte/Regionen`, `2_Orte/Spezielle Orte`, `2_Orte/Städte`,
   `z_Bilder`, `z_Templates`.
   Alle davon außer `1_Charaktere/NPC/Entitäten` haben eine namensgleiche
   Entsprechung in Dragonlance; `Entitäten` ist die bewusst beibehaltene
   Evermere-Kategorie (siehe „Begründung der Zuordnungen").
2. Es existieren keine Ordner `Quests/`, `Organisationen/`, `Orte/`, `Charaktere/`
   mehr.
3. Jede `.md`-Datei außerhalb von `z_Templates/` und `docs/` beginnt mit einem
   YAML-Frontmatter-Block, der mindestens `name` (bzw. `datum` bei Journalen) und
   `tags` enthält.
4. Keine `.md`-Datei ist leer.
5. Jeder Wikilink `[[X]]` im Vault löst sich auf eine existierende Datei `X.md`
   auf.
6. `z_Templates/Session.md` enthält `gruppe: Evermere`.
7. `app.json`, `appearance.json` und `.gitignore` entsprechen der Spezifikation
   oben.
8. `git status` zeigt keine unbeabsichtigt gelöschten Dateien; `git log --follow`
   findet für verschobene Notizen weiterhin die Vorgeschichte.

## Ausdrücklich nicht im Umfang

- Erfinden neuer Spielinhalte (Lore, NPCs, Orte, Questtexte)
- Ausfüllen der Template-Platzhaltertexte in `Keira Krabalde.md`
  („Etwas detailliertere Beschreibung", „Tolle Random infos") — das ist
  redaktionelle Arbeit des Nutzers
- Anlegen von Journal-Einträgen für vergangene Sessions
- Änderungen an Plugin-Einstellungen jenseits der oben genannten Dateien
