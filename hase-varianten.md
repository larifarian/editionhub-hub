# HASE-TEI — Zwei Varianten: Minimal + Full

HASE-TEI gibt es in zwei Varianten. Beide sind valide HASE-Dateien mit identischer Event-Struktur — sie unterscheiden sich nur im Umfang der Metadaten.

## Überblick

| | HASE-Min | HASE-Full |
|---|---|---|
| **Zweck** | Einzellieferung, maschinenlesbar | Komplettpaket, selbstbeschreibend |
| **Manifest nötig?** | Ja | Nein |
| **Typischer Einsatz** | Automatischer Harvest durch EditionHub | Standalone-Austausch, Archivierung |
| **Metadaten** | Nur Pflichtfelder | Pflicht + alle optionalen Felder |

## Was ist Pflicht, was optional?

| Feld | HASE-Min | HASE-Full | Wo sonst? |
|---|---|---|---|
| `title` | ✅ | ✅ | — |
| `publisher` | `<publisher/>` | `<publisher>Name</publisher>` | Manifest |
| `idno type="hase"` (Editions-Kürzel) | ✅ | ✅ | — |
| `idno type="version"` | ✅ | ✅ | — |
| `licence` | ✅ | ✅ | — |
| `listPrefixDef` | ✅ | ✅ | — |
| `sourceDesc/listBibl` (Quellen) | ✅ | ✅ | — |
| `listEvent` (Events) | ✅ | ✅ | — |
| `idno type="URL"` (Projekt-URL) | — | ✅ | Manifest |
| `idno type="contact"` | — | ✅ | Manifest |
| `seriesStmt` (Editionen) | — | ✅ | Manifest |
| `listPerson` / `listPlace` | — | ✅ (empfohlen) | — |

## HASE-Min — Beispiel

Enthält nur die Pflichtfelder. Wird zusammen mit einem Manifest genutzt, das die Projektinfos zentral bereitstellt.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<TEI xmlns="http://www.tei-c.org/ns/1.0">
  <teiHeader>
    <fileDesc>
      <titleStmt>
        <title>Propyläen — Briefe von Goethe</title>
      </titleStmt>
      <publicationStmt>
        <publisher/>
        <idno type="hase">prop-gb</idno>
        <idno type="version">2026-03-04</idno>
        <availability>
          <licence target="https://creativecommons.org/licenses/by/4.0/">CC-BY-4.0</licence>
        </availability>
      </publicationStmt>
      <sourceDesc>
        <listBibl>
          <msDesc xml:id="z-GB02_BR249_0" ana="#LETTER">
            <msIdentifier>
              <institution ref="sndb:standort/16">Bibliothèque Nationale et Universitaire</institution>
              <idno type="shelfmark">Boite 88. f. 651-652</idno>
            </msIdentifier>
          </msDesc>
        </listBibl>
      </sourceDesc>
    </fileDesc>
    <encodingDesc>
      <listPrefixDef>
        <prefixDef ident="gnd" matchPattern="(.*)" replacementPattern="https://d-nb.info/gnd/$1"/>
        <prefixDef ident="sndb" matchPattern="(.*)" replacementPattern="http://id.klassik-stiftung.de/entity/$1"/>
      </listPrefixDef>
    </encodingDesc>
    <profileDesc>
      <abstract>
        <listEvent>
          <event xml:id="GB02_BR249_0-send-1" ana="#LETTER_SENDING" when="1775-07-30">
            <desc>
              <persName ref="sndb:person/2475">Goethe, Johann Wolfgang</persName>
              <placeName ref="sndb:ort/77853">Frankfurt a. M.</placeName>
            </desc>
            <ptr type="source" target="#z-GB02_BR249_0"/>
          </event>
          <event xml:id="GB02_BR249_0-receive-1" ana="#LETTER_RECEIVING">
            <desc>
              <persName ref="sndb:person/40097">Orville, Jean George d'</persName>
              <placeName ref="sndb:ort/84835">Offenbach</placeName>
            </desc>
            <ptr type="source" target="#z-GB02_BR249_0"/>
          </event>
        </listEvent>
      </abstract>
    </profileDesc>
  </teiHeader>
  <text><body><p/></body></text>
</TEI>
```

### Zugehöriges Manifest

Bei HASE-Min ist ein Manifest nötig, das die fehlenden Projektinfos zentral bereitstellt:

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
    { "url": "https://goethe-biographica.de/hase/prop-gb.xml", "edition": "prop-gb" },
    { "url": "https://goethe-biographica.de/hase/prop-ra.xml", "edition": "prop-ra" },
    { "url": "https://goethe-biographica.de/hase/prop-gt.xml", "edition": "prop-gt" },
    { "url": "https://goethe-biographica.de/hase/prop-bug.xml", "edition": "prop-bug" }
  ]
}
```

Das Manifest wird zentral bei EditionHub gehostet. EditionHub pollt das Manifest regelmäßig und ingestiert geänderte oder neue Dateien automatisch.

## HASE-Full — Beispiel

Enthält alle Metadaten in der Datei selbst. Kein Manifest nötig — die Datei ist vollständig selbstbeschreibend.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml-model href="https://tei-c.org/release/xml/tei/custom/schema/relaxng/tei_all.rng"
            type="application/xml" schematypens="http://relaxng.org/ns/structure/1.0"?>
<TEI xmlns="http://www.tei-c.org/ns/1.0">
  <teiHeader>
    <fileDesc>
      <titleStmt>
        <title>Propyläen — Briefe von Goethe</title>
      </titleStmt>
      <publicationStmt>
        <publisher>Klassik Stiftung Weimar</publisher>
        <idno type="hase">prop-gb</idno>
        <idno type="version">2026-03-04</idno>
        <idno type="URL">https://goethe-biographica.de</idno>
        <idno type="contact">kontakt@goethe-biographica.de</idno>
        <availability>
          <licence target="https://creativecommons.org/licenses/by/4.0/">CC-BY-4.0</licence>
        </availability>
      </publicationStmt>

      <!-- Editionen des Projekts -->
      <seriesStmt>
        <title>Briefe von Goethe</title>
        <idno type="hase">prop-gb</idno>
      </seriesStmt>
      <seriesStmt>
        <title>Briefe an Goethe</title>
        <idno type="hase">prop-ra</idno>
      </seriesStmt>
      <seriesStmt>
        <title>Goethes Tagebücher</title>
        <idno type="hase">prop-gt</idno>
      </seriesStmt>
      <seriesStmt>
        <title>Begegnungen und Gespräche</title>
        <idno type="hase">prop-bug</idno>
      </seriesStmt>

      <!-- Quellen-Register -->
      <sourceDesc>
        <listBibl>
          <msDesc xml:id="z-GB02_BR249_0" ana="#LETTER">
            <msIdentifier>
              <institution ref="sndb:standort/16">Bibliothèque Nationale et Universitaire</institution>
              <idno type="shelfmark">Boite 88. f. 651-652</idno>
            </msIdentifier>
          </msDesc>
          <!-- ... weitere msDesc ... -->
        </listBibl>
      </sourceDesc>
    </fileDesc>

    <encodingDesc>
      <!-- Prefix-Auflösung -->
      <listPrefixDef>
        <prefixDef ident="gnd" matchPattern="(.*)" replacementPattern="https://d-nb.info/gnd/$1"/>
        <prefixDef ident="sndb" matchPattern="(.*)" replacementPattern="http://id.klassik-stiftung.de/entity/$1"/>
      </listPrefixDef>

      <!-- Typen-Taxonomie -->
      <classDecl>
        <taxonomy xml:id="hase-activity">
          <category xml:id="LETTER_SENDING">
            <catDesc xml:lang="de">Briefversand</catDesc>
          </category>
          <category xml:id="LETTER_RECEIVING">
            <catDesc xml:lang="de">Briefempfang</catDesc>
          </category>
        </taxonomy>
      </classDecl>
    </encodingDesc>

    <profileDesc>
      <!-- Personen-Register (optional, empfohlen bei großen Dateien) -->
      <particDesc>
        <listPerson>
          <person>
            <persName>Goethe, Johann Wolfgang</persName>
            <idno type="gnd">118540238</idno>
            <birth when="1749-08-28"/>
            <death when="1832-03-22"/>
          </person>
          <person>
            <persName>Orville, Jean George d'</persName>
            <idno type="sndb">person/40097</idno>
          </person>
        </listPerson>
      </particDesc>

      <!-- Handlungen -->
      <abstract>
        <listEvent>
          <event xml:id="GB02_BR249_0-send-1" ana="#LETTER_SENDING" when="1775-07-30">
            <desc>
              <persName ref="sndb:person/2475">Goethe, Johann Wolfgang</persName>
              <placeName ref="sndb:ort/77853">Frankfurt a. M.</placeName>
            </desc>
            <ptr type="source" target="#z-GB02_BR249_0"/>
          </event>
          <event xml:id="GB02_BR249_0-receive-1" ana="#LETTER_RECEIVING">
            <desc>
              <persName ref="sndb:person/40097">Orville, Jean George d'</persName>
              <placeName ref="sndb:ort/84835">Offenbach</placeName>
            </desc>
            <ptr type="source" target="#z-GB02_BR249_0"/>
          </event>
          <!-- ... weitere events ... -->
        </listEvent>
      </abstract>
    </profileDesc>
  </teiHeader>
  <text><body><p/></body></text>
</TEI>
```

## Wann welche Variante?

| Szenario | Variante | Grund |
|---|---|---|
| Projekt liefert regelmäßig an EditionHub | **Min + Manifest** | Manifest pflegt Projektinfos zentral, Dateien bleiben schlank |
| Einmaliger Datenaustausch | **Full** | Alles in einer Datei, kein Manifest nötig |
| Archivierung / Langzeitverfügbarkeit | **Full** | Selbstbeschreibend, keine externe Abhängigkeit |
| Externe Ersteller ohne technische Erfahrung | **Min** | Weniger Felder = weniger Fehlerquellen |
| Große Projekte mit vielen Editionen | **Min + Manifest** | Aufteilen nach Edition, Manifest verbindet |

## Kompatibilität

Die Events (`listEvent/event`) sind in beiden Varianten **identisch**. HASE-Full ist eine Obermenge von HASE-Min — jede Min-Datei ist auch in einer Full-Umgebung gültig, und jede Full-Datei kann zu Min reduziert werden, indem die optionalen Felder entfernt und ins Manifest verschoben werden.

Der gemeinsame Schlüssel für Personen und Orte ist der Identifier im `@ref`-Attribut (z.B. `gnd:118540238`). Dieser bleibt in beiden Varianten gleich. Die `listPerson` in HASE-Full ist rein additiv — sie reichert an, ändert aber keine Referenzen.

## Erzeugung

Für das Propyläen-Projekt:

1. **EXTRACT_HASE.xsl** → erzeugt pro Brief eine HASE-Min-Datei
2. **MERGE_HASE.xsl** → sammelt alle Einzeldateien und erzeugt eine HASE-Full-Datei

Andere Projekte können HASE-Dateien direkt von Hand oder aus ihren eigenen Systemen erzeugen — es gibt keine Abhängigkeit von diesen XSLTs.
