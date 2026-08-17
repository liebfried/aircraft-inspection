# Aircraft Inspection — Requirements-Baseline

Status: **Freigegeben.**
Quelle: abgeleitet aus dem Vorgängerprojekt (`PROJECT-AIRCRAFT-INSPECTION-IPAD`)
und vervollständigt durch die Discovery-Befragung vom 2026-08-17 (zwei
Fragerunden, alle offenen Fragen entschieden).

## Produktziel

Native iPad-App für Flugzeug-Inspektionen: technische Prüfformulare digital
ausfüllen — inklusive Fotoaufnahme, Entwurfsverwaltung, Pflichtfeld-Validierung,
PDF-Vorschau und PDF-Export. Vollständig offline nutzbar, ohne Backend.

## Funktionale Anforderungen

### Formulare

- **REQ-001** Die App stellt Prüfformulare aus einer strukturierten
  Formulardefinition dar (Seiten mit geordneten Feldern).
- **REQ-002** Unterstützte Feldtypen mindestens: Überschrift, einzeiliger Text,
  mehrzeiliger Text, Datum, Checkliste (Status je Eintrag), Foto, Unterschrift.
- **REQ-003** Bis zur Lieferung des echten Prüfformulars nutzt die App ein
  Platzhalter-Formular. Es darf keine erfundenen fachlichen/regulatorischen
  Inhalte suggerieren.
- **REQ-004** Formulare sind Daten, nicht Code: Das echte Formular (und weitere)
  können später ohne Änderung der Engine eingepflegt werden.
- **REQ-005** Die App verwaltet mehrere Formular-Vorlagen; beim Anlegen eines
  Entwurfs wählt der Nutzer die Vorlage.
- **REQ-006** Neue Formulardefinitionen können ohne App-Update eingespielt
  werden (Import einer Definitionsdatei, z. B. JSON, über die Dateien-App /
  das Share-Sheet). Fehlerhafte Definitionen werden mit verständlicher
  Fehlermeldung abgewiesen und ändern den Bestand nicht.

### Entwürfe (Drafts)

- **REQ-010** Nutzer können je Vorlage beliebig viele Entwürfe anlegen, öffnen,
  weiterbearbeiten und löschen.
- **REQ-011** Alle Eingaben werden automatisch gespeichert (Autosave); es gibt
  keinen verlierbaren „ungespeicherten" Zustand.
- **REQ-012** Entwürfe überleben App-Neustart, App-Update und Gerät-Neustart
  (lokale Persistenz).
- **REQ-013** Die App funktioniert vollständig offline; keine Funktion setzt
  eine Netzwerkverbindung voraus.
- **REQ-014** Entwürfe und Fotos bleiben bis zum manuellen Löschen durch den
  Nutzer erhalten; es gibt keine automatische Löschung.
- **REQ-015** Ein vollständig validierter Entwurf kann explizit
  **abgeschlossen** werden: Er wird schreibgeschützt (Archiv), sein PDF bleibt
  jederzeit erneut erzeugbar. Ein abgeschlossener Entwurf kann bewusst
  **wiedereröffnet** werden; die Wiedereröffnung wird im Entwurf vermerkt
  (Zeitpunkt) und im PDF ausgewiesen.

### Fotos

- **REQ-020** Fotofelder erlauben Aufnahme per Kamera direkt aus dem Formular
  sowie Import aus der Fotobibliothek.
- **REQ-021** Angehängte Fotos haben eine Vorschau und können gedreht und
  zugeschnitten werden.
- **REQ-022** Fotos werden mit konfigurierbarer Zielgröße/Kompression
  gespeichert (Presets klein/mittel/groß).
- **REQ-023** Fotos sind Teil des Entwurfs und werden mit ihm gespeichert und
  gelöscht.
- **REQ-024** Pro Fotofeld sind maximal 10 Fotos zulässig; die Grenze ist in
  der Formulardefinition pro Feld übersteuerbar.
- **REQ-025** Fotos können annotiert werden (Freihand-Markierungen, z. B.
  Schadensstelle einkreisen — Finger oder Apple Pencil). Das Original bleibt
  erhalten; die Annotation ist nachträglich änderbar und erscheint im PDF.

### Unterschrift

- **REQ-026** Der Feldtyp Unterschrift erfasst eine handschriftliche
  Unterschrift (Finger oder Apple Pencil) mit Name des Unterzeichnenden und
  Zeitpunkt; die Unterschrift erscheint im PDF.

### Validierung

- **REQ-030** Pflichtfelder sind in der Formulardefinition markiert; die App
  zeigt fehlende Pflichtangaben verständlich und feldbezogen an.
- **REQ-031** Der PDF-Export eines unvollständigen Entwurfs ist möglich,
  erfordert aber eine ausdrückliche Bestätigung nach Warnung; das erzeugte PDF
  ist deutlich als „ENTWURF — unvollständig" gekennzeichnet.
- **REQ-032** Abschließen (REQ-015) ist nur bei vollständig erfüllter
  Pflichtfeld-Validierung möglich.

### PDF

- **REQ-040** Die App erzeugt aus einem Entwurf lokal (ohne Netz) ein PDF mit
  allen Feldwerten, Fotos (inkl. Annotationen) und Unterschriften.
- **REQ-041** Das PDF kann in der App als Vorschau angezeigt werden.
- **REQ-042** Das PDF kann über das iOS-Share-Sheet exportiert werden (Dateien,
  Mail, AirDrop etc.).

## Nicht-funktionale Anforderungen

- **REQ-060** Zielplattform: iPadOS 17+, alle davon unterstützten iPads;
  Hoch- und Querformat. Apple Pencil wird unterstützt, ist aber nie
  Voraussetzung.
- **REQ-061** Bedienbarkeit im Prüfumfeld: große Touch-Ziele, Nutzung auch im
  Stehen; alle Kernfunktionen mit dem Finger bedienbar.
- **REQ-062** Keine Cloud-, Analytics- oder Tracking-Dienste; Daten verlassen
  das Gerät ausschließlich durch expliziten Nutzer-Export.
- **REQ-063** UI-Sprache: Deutsch.
- **REQ-064** Personenbezogene Daten (Prüfername, Unterschriften) werden
  ausschließlich lokal gespeichert, sind vollständig löschbar (Löschen des
  Entwurfs entfernt alle zugehörigen Daten inkl. Fotos und Unterschriften) und
  werden keinem Dritten bereitgestellt.
- **REQ-065** Die Codebasis ist testbar aufgebaut; Formularlogik, Validierung,
  Persistenz, PDF-Erzeugung, Import sowie Designer und Vorlagen-Versionierung
  sind durch automatisierte Tests abgedeckt.

## Priorisierung der Erweiterungen

Alle Erweiterungen sind im Scope. Implementierungsreihenfolge (nach Kern-MVP):
1. Formular-Import (REQ-006), 2. Formular-Designer (REQ-007…009, Nachtrag N-01),
3. mehrere Formulare (REQ-005), 4. Foto-Annotation (REQ-025),
5. Unterschriftenfeld (REQ-026). Die Reihenfolge kann in Phase 4 einvernehmlich
angepasst werden.

## Explizit außerhalb des Scopes

- Backend-, Cloud- oder Synchronisierungs-Infrastruktur
- App-Store-Release, Signing- und Zertifikatsarbeiten
- Regulatorische Konformitätszusagen (TÜV, Luftfahrt, Datenschutz-Audits);
  REQ-064 dokumentiert Datenschutz-Grundsätze, ersetzt aber keine
  Rechtsprüfung
- Erfundene Inhalte für das echte Prüfformular
- Mehrbenutzer-/Rollenkonzepte auf dem Gerät

## Entschiedene Discovery-Fragen

| ID | Entscheidung |
| --- | --- |
| Q-01 | Alle vier Erweiterungen im Scope; Reihenfolge: Import, Mehrformular, Annotation, Unterschrift |
| Q-02 | Ja, Prüferdaten → REQ-064 |
| Q-03 | Aufbewahrung bis zum manuellen Löschen → REQ-014 |
| Q-04 | UI Deutsch → REQ-063 |
| Q-05 | Export mit Warnung + „ENTWURF"-Kennzeichnung → REQ-031 |
| Q-06 | iPadOS 17+, beide Orientierungen, Pencil optional → REQ-060 |
| Q-07 | Kamera + Fotobibliothek, max. 10 Fotos/Feld → REQ-020/024 |
| Q-08 | Echtes Formular weiterhin ausstehend; Platzhalter → REQ-003 |

Approved-by-user: 2026-08-17

---

## Nachtrag N-01 — Visueller Formular-Designer (2026-08-17, Rev. 2)

Status: **Zur Freigabe vorgelegt.**
Anlass: Rückmeldung aus dem Phase-2-Review — Formulare sollen direkt in der App
erstellt werden, und zwar als **visueller Baukasten mit frei positionierbaren
Elementen auf einer Leinwand mit Andock-Raster** (Rev. 2 ersetzt die
listenbasierte Designer-Fassung aus Rev. 1).

### Grundmodell

- **REQ-007** Formular-Vorlagen bestehen aus Seiten im **A4-Hochformat**. Der
  **Formular-Designer** ist Teil der App: Elemente werden frei auf der
  Seiten-Leinwand platziert, verschoben und in der Größe angepasst. Designer,
  Ausfüllansicht und PDF sind **deckungsgleich (WYSIWYG)**.
- **REQ-070** **Baukasten (Element-Palette)** mit mindestens diesen Typen:
  - *Struktur:* Kopfzeile (Header), Fußzeile (Footer) — je Vorlage definiert,
    wiederholen sich automatisch auf jeder Seite;
  - *Statisch:* Überschrift/Textblock, Linie/Unterstreichung, Rahmen/Box,
    statische Liste (Aufzählung), Bild/Logo;
  - *Eingabe:* Textfeld (einzeilig), Textbereich (mehrzeilig), Datumsfeld,
    einzelne Checkbox, Checklisten-Gruppe (Einträge mit OK/Mangel/N. z.),
    Auswahlliste (eine Option aus definierter Liste), Fotofeld (mit
    Foto-Limit), Unterschriftsfeld.
- **REQ-071** **Raster &amp; Andocken:** Die Leinwand hat ein sichtbares,
  zuschaltbares Raster (Standard 4 mm). Elemente rasten beim Verschieben und
  Skalieren am Raster sowie an Kanten und Mittelachsen anderer Elemente ein
  (eingeblendete Ausrichtungslinien), sodass gleiche Positionen über Elemente
  und Seiten hinweg gewährleistet sind. Zusätzlich: Ausrichten-/
  Verteilen-Werkzeuge für Mehrfachauswahl und numerische Eingabe von
  Position/Größe (in mm).
- **REQ-072** **Standard-Bausteine:** Eine mitgelieferte Bibliothek bietet
  fertige Header/Footer und Blöcke zum Einfügen (z. B. Titelkopf mit
  Logo-Platzhalter, Titel und Metafeldern; Fußzeile mit Seitenzahl, Datum und
  Unterschriftszeile; Checklisten-Block). Eingefügte Bausteine sind danach frei
  anpassbar.
- **REQ-073** **Eigenschaften je Element:** Bezeichnung, Pflichtfeld-Schalter
  (bei Eingabetypen), typspezifische Optionen (Checklisten-/Auswahleinträge,
  Foto-Limit), Textstil (Größe, fett), Position und Größe.
- **REQ-074** **Ausfüllansicht = Formularansicht:** Beim Ausfüllen sieht der
  Prüfer die gestaltete A4-Seite (zoombar, Hoch- und Querformat) und tippt
  direkt in die Felder. Pflichtfeld-Markierung und Validierungshinweise werden
  auf der Seite angezeigt; Foto, Unterschrift und Annotation öffnen wie bisher
  als Sheets. Das exportierte PDF entspricht exakt dem gestalteten Layout.
- **REQ-008** Vorlagen sind **versioniert**: Das Bearbeiten einer Vorlage, zu
  der Entwürfe existieren, erzeugt beim Veröffentlichen eine neue Version.
  Bestehende Entwürfe bleiben unverändert an ihre Version gebunden; neue
  Entwürfe verwenden die neueste Version. Unveröffentlichte Designer-Änderungen
  sind als Entwurfszustand der Vorlage erkennbar.
- **REQ-009** Vorlagen können als Definitionsdatei (JSON) über das Share-Sheet
  **exportiert** werden (Gegenstück zu REQ-006), z. B. zur Weitergabe an ein
  anderes iPad oder als Sicherung.

### Präzisierung der Baseline

REQ-001/REQ-002 werden durch das Layoutmodell konkretisiert: „Seiten mit
geordneten Feldern" bedeutet fortan A4-Seiten mit positionierten Elementen;
die Feldtypen aus REQ-002 gehen vollständig im Baukasten (REQ-070) auf. Die
Validierungs-, Foto-, Unterschrift-, Abschluss- und Export-Anforderungen
(REQ-020…042) gelten unverändert für die Formularansicht.

### Entschiedene Fragen zum Nachtrag

| ID | Entscheidung |
| --- | --- |
| N-01a | Bearbeitung bei vorhandenen Entwürfen → neue Version (REQ-008) |
| N-01b | Vorlagen-Export als JSON → ja (REQ-009) |
| N-01c | Priorität: direkt nach Formular-Import (siehe Priorisierung) |
| N-01d | Ausfüllen als WYSIWYG-Formularansicht → REQ-074 |
| N-01e | Leinwand-/Seitenformat A4 hochkant → REQ-007 |
