---
datum: <% tp.date.now("YYYY-MM-DD") %>
gruppe: Evermere
ort:
tags:
  - session
  - journal
---

# Session am `=this.datum`

## Zusammenfassung
Kurzer Überblick was diese Session passiert ist (2-3 Sätze).

## Ablauf
- 

## Neue NPCs
> Direkt hier verlinken, z.B. [[Neuer NPC]] 

## Neue Orte
> [[Neuer Ort]] – Datei bei Bedarf unter `2_Orte/` anlegen.

## Offene Fäden / Quests
- 

## Loot & Belohnungen
- 

---
## Vault-Kontext (automatisch)

### Bekannte Spielercharaktere
```dataview
LIST FROM "1_Charaktere/Spieler"
```

### Bekannte NPCs
```dataview
LIST FROM "1_Charaktere/NPC"
```

### Bekannte Fraktionen
```dataview
LIST FROM "1_Charaktere/Fraktionen"
```

### Bekannte Orte
```dataview
LIST FROM "2_Orte"
```

### Letzte Sessions
```dataview
LIST FROM "0_Journale"
SORT file.name DESC
LIMIT 5
```
