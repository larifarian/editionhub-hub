# HASE — Metadaten-Verteilung: Datei vs. Manifest

Welche Projekt-Informationen gehören in jede HASE-Datei, welche nur ins Manifest?

## Faustregel

- **In die Datei:** Was man braucht, um die Datei alleine zu verstehen und zu nutzen.
- **Ins Manifest:** Was das Gesamtprojekt beschreibt und sich zentral pflegen lässt.
- **In beides:** Was rechtlich oder zur Identifikation nötig ist.

## Übersicht

| Info | HASE-Datei | Manifest | Grund |
|---|---|---|---|
| Projektkürzel (z.B. `prop`) | ja | ja | Identifiziert die Quelle, muss immer dabei sein |
| Lizenz | ja | ja | Rechtlich nötig an jeder Datei |
| `prefixDef` (Namespace-Auflösung) | ja | — | Nötig um `@ref`-Werte in URIs aufzulösen |
| Version / Datum | ja | ja | Wann erzeugt |
| Projekt-Titel | ja (kurz) | ja (lang) | Kurztitel reicht in der Datei |
| Publisher / Herausgeber | — | ja | Ändert sich selten, Gesamtprojekt-Info |
| Kontakt / E-Mail | — | ja | Ändert sich, nur einmal pflegen |
| Projekt-URL | — | ja | Gesamtprojekt-Info |
| Editionen / Teilprojekte | — | ja | Beschreibt Gesamtprojekt, nicht Einzeldatei |
| Beschreibungstexte | — | ja | Langtext gehört nicht in jede Datei |

## HASE-Datei (TEI)

Minimal-Header in jeder Datei:

```xml
<fileDesc>
  <titleStmt>
    <title>Propyläen</title>
  </titleStmt>
  <publicationStmt>
    <idno type="hase">prop</idno>
    <idno type="version">2026-03-04</idno>
    <availability>
      <licence target="https://creativecommons.org/licenses/by/4.0/">CC-BY-4.0</licence>
    </availability>
  </publicationStmt>
  ...
</fileDesc>
<encodingDesc>
  <listPrefixDef>
    <prefixDef ident="gnd" matchPattern="(.*)" replacementPattern="https://d-nb.info/gnd/$1"/>
    <prefixDef ident="sndb" matchPattern="(.*)" replacementPattern="http://id.klassik-stiftung.de/entity/$1"/>
    ...
  </listPrefixDef>
</encodingDesc>
```

## Manifest (JSON)

Eine Datei pro Projekt, zentral bei EditionHub gehostet:

```json
{
  "project": "prop",
  "label": "Propyläen — Goethes Biographica",
  "publisher": "Klassik Stiftung Weimar",
  "url": "https://goethe-biographica.de",
  "contact": "kontakt@goethe-biographica.de",
  "licence": "CC-BY-4.0",
  "editions": [
    { "id": "prop-gb", "label": "Briefe von Goethe" },
    { "id": "prop-ra", "label": "Briefe an Goethe" },
    { "id": "prop-gt", "label": "Goethes Tagebücher" },
    { "id": "prop-bug", "label": "Begegnungen und Gespräche" }
  ],
  "files": [
    "https://goethe-biographica.de/hase/prop-gb.xml",
    "https://goethe-biographica.de/hase/prop-ra.xml",
    "https://goethe-biographica.de/hase/prop-gt.xml",
    "https://goethe-biographica.de/hase/prop-bug.xml"
  ]
}
```

## Begründung

- **Kontakt-E-Mail ändern** = eine Manifest-Datei aktualisieren, nicht 500 HASE-Dateien.
- **Neue Edition hinzufügen** = Manifest erweitern + neue HASE-Datei bereitstellen.
- **Jede HASE-Datei bleibt eigenständig nutzbar** — Projektkürzel, Lizenz und Prefix-Definitionen sind immer enthalten.
- **EditionHub braucht nur die Manifest-URL** — findet den Rest automatisch.
