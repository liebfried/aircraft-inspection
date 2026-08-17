# Aircraft Inspection — Architektur

Status: **Zur Freigabe vorgelegt.**
Grundlage: freigegebene Requirements-Baseline inkl. Nachtrag N-01 Rev. 2 und
abgenommenes UI-Design (S-01…S-11), beide 2026-08-17.
ADR-001 bis ADR-004 wurden am 2026-08-17 explizit vom Nutzer entschieden;
ADR-005 bis ADR-011 sind Architekten-Empfehlungen, die mit der Freigabe dieses
Dokuments als entschieden gelten.

## Überblick

```
┌─────────────────────────── App: AircraftInspection (SwiftUI) ──────────────────────────┐
│  Features: Bibliothek (S-01) · Ausfüllen (S-02…S-07) · Designer (S-09…S-11) · Import   │
└──────┬──────────────┬───────────────┬──────────────┬───────────────┬──────────────────┘
       │              │               │              │               │
  FormModel      FormLayout     FormValidation  FormPersistence   FormPDF
  (Vorlagen,     (mm-Geometrie,  (Pflichtfelder, (SwiftData +      (CoreGraphics-
   Versionen,     Raster,         Report)         Dateiablage)      Renderer)
   Entwürfe)      Snapping)                            │
       └──────────────┴───────FormInterchange──────────┘
                        (JSON-Import/-Export, schema_version)
```

Alle Pakete unterhalb der App sind lokale Swift-Packages ohne UI-Abhängigkeit
(Ausnahme: FormPDF nutzt CoreGraphics/UIKit-Zeichnung, FormPersistence nutzt
SwiftData). Die App ist eine dünne SwiftUI-Schale über den Paketen.

---

## ADR-001 · Plattform und Sprache

- **Kontext:** REQ-060 (iPadOS 17+, Hoch-/Querformat), UI-Design im nativen Idiom.
- **Optionen:** Swift/SwiftUI · Swift/UIKit · Cross-Platform (Flutter u. a.).
- **Entscheidung:** Swift im **Swift-6-Sprachmodus** (strikte Concurrency),
  **SwiftUI** als UI-Framework, Deployment-Target **iPadOS 17.0**, nur iPad-Idiom.
  UIKit/PencilKit werden punktuell via Representable eingebettet (ADR-007/008).
- **Konsequenzen:** Modernste Toolchain, Compiler-geprüfte Nebenläufigkeit;
  SwiftUI-Reife auf iPadOS 17 ist für alle geplanten Screens ausreichend.

## ADR-002 · Projekterzeugung: XcodeGen  *(Nutzerentscheidung)*

- **Kontext:** Reproduzierbares Setup, agentische Entwicklung, saubere Diffs.
- **Optionen:** XcodeGen · manuelles Xcode-Projekt · Tuist.
- **Entscheidung:** **XcodeGen**. `project.yml` ist versioniert; `.xcodeproj`
  wird generiert und ist nicht eingecheckt. Einmalige Voraussetzung:
  `brew install xcodegen`.
- **Konsequenzen:** Keine pbxproj-Merge-Konflikte; Dateistruktur-Änderungen sind
  Textänderungen; CI kann das Projekt deterministisch erzeugen.

## ADR-003 · Modulstruktur: lokale SPM-Pakete  *(Nutzerentscheidung)*

- **Kontext:** REQ-065 (testbare Codebasis); Lehre aus dem Vorgängerprojekt
  (2 Dateien, ~3.000 Zeilen, 0 Unit-Tests).
- **Optionen:** lokale SPM-Pakete + dünne App · ein App-Target mit Gruppen.
- **Entscheidung:** Lokale Packages `FormModel`, `FormLayout`, `FormValidation`,
  `FormInterchange` (rein Foundation, plattformunabhängig) sowie
  `FormPersistence` (SwiftData) und `FormPDF` (CoreGraphics). Die App enthält
  ausschließlich SwiftUI-Features und Komposition.
- **Konsequenzen:** Kernlogik läuft mit `swift test` ohne Simulator;
  Abhängigkeitsrichtung wird vom Compiler erzwungen (Features → Pakete, nie
  umgekehrt; FormModel hat keine Abhängigkeiten).

## ADR-004 · Persistenz: SwiftData + Dateiablage  *(Nutzerentscheidung)*

- **Kontext:** REQ-011/012/014/015 (Autosave, Neustart-fest, manuelles Löschen,
  Abschluss), REQ-008 (Vorlagen-Versionen), REQ-064 (vollständige Löschbarkeit).
- **Optionen:** SwiftData + Dateien · Core Data + Dateien · rein dateibasiert.
- **Entscheidung:** **SwiftData** für Strukturdaten, **Dateiablage** für Binärdaten:
  - Entities: `Template` (Identität), `TemplateVersion` (eingefrorene
    Layout-Definition als JSON-Blob + Versionsnummer), `Draft` (Werte als
    JSON-Blob, Status `inBearbeitung`/`abgeschlossen`, Wiedereröffnungs-Vermerke),
    `AttachmentRecord` (Metadaten zu Fotos/Unterschriften).
  - Binärdaten unter `Application Support/Attachments/<draftID>/…` (Original,
    Annotation-Strokes, abgeleitete Renderings). Löschen eines Entwurfs entfernt
    Datensätze **und** Verzeichnis (REQ-064/023).
  - Ein Entwurf referenziert immer eine konkrete `TemplateVersion` (REQ-008).
  - Autosave: Änderungen laufen über ein `DraftStore`-Protokoll; Persistenz bei
    jeder Feldänderung (debounced) und bei App-Hintergrund.
- **Konsequenzen:** Wenig Boilerplate, klarer Migrationspfad; Stores sind über
  Protokolle abstrahiert und in Tests durch In-Memory-Container ersetzt.

## ADR-005 · Layout-Engine: ein mm-Modell, zwei Renderer  *(Nutzerentscheidung)*

- **Kontext:** REQ-007/070/071/074 — Designer, Ausfüllansicht und PDF müssen
  deckungsgleich sein; Raster und Andocken müssen exakt sein.
- **Optionen:** gemeinsames Layoutmodell + 2 Renderer · PDFKit/AcroForm ·
  SwiftUI ImageRenderer.
- **Entscheidung:** `FormLayout` definiert den einzigen Koordinatenraum:
  **Millimeter auf A4 (210 × 297)**. Elemente tragen `frame: MMRect`.
  - **SwiftUI-Renderer** (App): skaliert mm → Punkte für Designer-Leinwand und
    Ausfüllansicht (zoombar, identischer Code für beide, Modus „design"/„fill").
  - **PDF-Renderer** (`FormPDF`): skaliert mm → PDF-Punkte (72 dpi-Basis) und
    zeichnet dieselben Elemente mit CoreGraphics.
  - **Snapping als reine Funktion:** `snap(rect, grid: 4mm, guides: [Kanten/
    Mittelachsen aller Elemente]) -> (MMRect, [Guide])` — vollständig
    unit-testbar, unabhängig von Gesten.
  - **Lesereihenfolge** (VoiceOver, Validierungsliste): deterministische
    Zeilen-Spalten-Sortierung der Element-Frames.
- **Konsequenzen:** WYSIWYG ist konstruktiv garantiert statt nachträglich
  angenähert; Layout- und Snapping-Logik sind ohne UI testbar.

## ADR-006 · PDF-Erzeugung

- **Kontext:** REQ-031/040/041/042; Prüfberichte müssen reproduzierbar sein.
- **Entscheidung:** `UIGraphicsPDFRenderer` in `FormPDF`. Vektor-Text, Fotos als
  komprimierte Bitmaps, Annotationen beim Rendern über das Foto gelegt
  (Original bleibt unangetastet). Wasserzeichen „ENTWURF — UNVOLLSTÄNDIG" auf
  jeder Seite bei unvollständigem Export; leere Pflichtfelder als „— fehlt —";
  Wiedereröffnungs-Vermerk im Fußbereich (REQ-015). PDF-Metadaten (Erstelldatum)
  sind injizierbar, damit Tests byte-stabile Referenz-PDFs vergleichen können.
- **Konsequenzen:** Deterministische, versionierbare PDF-Ausgabe; kein
  AcroForm-Funktionsumfang (bewusst — Formulare werden in der App ausgefüllt).

## ADR-007 · Foto-Pipeline

- **Kontext:** REQ-020…025, REQ-064; Simulator-Testbarkeit.
- **Entscheidung:** Aufnahme/Import hinter Protokoll `PhotoSource`
  (`CameraPhotoSource` via UIImagePickerController/AVFoundation,
  `LibraryPhotoSource` via PhotosPicker, `StubPhotoSource` für Tests/Simulator).
  Beim Import: Downscaling gemäß Preset (klein/mittel/groß), **EXIF-/
  GPS-Metadaten werden entfernt** (Datenschutz, REQ-064). Drehen/Zuschneiden
  destruktionsfrei über gespeicherte Transformationsparameter; **Annotation via
  PencilKit** (`PKDrawing` separat gespeichert, Original unverändert, REQ-025).
- **Konsequenzen:** Fotologik ohne Kamera testbar; Annotationen bleiben editierbar.

## ADR-008 · Unterschrift

- **Kontext:** REQ-026.
- **Entscheidung:** PencilKit-Canvas im Sheet (S-04); gespeichert werden
  `PKDrawing`, Name und beim „Übernehmen" fixierter Zeitpunkt. Ins PDF wird die
  Unterschrift als Vektor-/Bitmap-Rendering übernommen.

## ADR-009 · Formular-Austauschformat (Import/Export)

- **Kontext:** REQ-004/006/009; Vorlagen sind Daten.
- **Entscheidung:** JSON mit `schema_version` (Start: `1`), mm-Koordinaten,
  geschlossener Elementtyp-Katalog entsprechend REQ-070. `FormInterchange`
  validiert beim Import vollständig (unbekannte Typen, Überlappungs- und
  Grenzprüfung, Pflichtangaben) und lehnt fehlerhafte Dateien atomar ab —
  identische Codepfade für Import (REQ-006) und Export (REQ-009). Das
  Platzhalter-Formular (REQ-003) wird als mitgelieferte JSON-Ressource über
  denselben Importpfad geladen.
- **Konsequenzen:** Ein Format, ein Validierer; Beispiel- und Testfixtures sind
  gewöhnliche JSON-Dateien.

## ADR-010 · Teststrategie

- **Kontext:** REQ-065; Prozessvorgabe: Review + Tests je Inkrement.
- **Entscheidung:**
  - **Unit (swift test, ohne Simulator):** FormModel, FormLayout (inkl.
    Snapping-Eigenschaften), FormValidation, FormInterchange (inkl.
    Fehlerfälle), Versionierungsregeln.
  - **Integration (Simulator):** FormPersistence mit In-Memory-SwiftData;
    FormPDF gegen Referenz-PDFs mit fixierten Metadaten.
  - **UI-Smoke (XCUITest):** Entwurf anlegen → ausfüllen → validieren → PDF.
  - **Manuell auf Gerät:** Kamera, Apple Pencil, Share-Sheet, Druckbild.
- **Konsequenzen:** Schnelle Kernschleife ohne Simulator; klare Testpyramide.

## ADR-011 · Zustands- und Fehlermodell

- **Kontext:** REQ-015/031/032; keine stillen Fehler.
- **Entscheidung:** `Draft.status`-Zustandsmaschine
  `inBearbeitung ⇄ abgeschlossen` (Übergang „abschließen" nur bei leerem
  Validierungsreport; „wiedereröffnen" hängt Vermerk mit Zeitstempel an, Historie
  ist append-only). Alle Store-/Render-/Importfehler sind typisierte `Error`s
  mit deutschsprachiger, konkreter Nutzerbotschaft; es gibt keine leeren
  `catch`-Blöcke.

---

## Repository-Struktur (Ziel)

```
project.yml                     # XcodeGen-Definition
App/                            # SwiftUI-App (Features, Ressourcen, de-Lokalisierung)
Packages/
  FormModel/                    # + Tests
  FormLayout/                   # + Tests (Snapping, Lesereihenfolge, Paginierung)
  FormValidation/               # + Tests
  FormInterchange/              # + Tests + Fixtures (inkl. Platzhalter-Formular)
  FormPersistence/              # + Tests (In-Memory)
  FormPDF/                      # + Tests (Referenz-PDFs)
UITests/                        # XCUITest-Smoke
docs/                           # Projekt-Doku (Phase-4-Inkremente)
```

## Umsetzungs-Inkremente (Phase 4, je mit Review + Tests)

1. **I-01 Fundament:** project.yml, Pakete, CI-fähiger Build, Platzhalter-Formular als Fixture
2. **I-02 Modell & Layout:** FormModel + FormLayout (mm, Raster, Snapping, Lesereihenfolge)
3. **I-03 Persistenz:** SwiftData-Schema, Dateiablage, Autosave, Löschkaskade
4. **I-04 Ausfüllen:** S-01/S-02 (Bibliothek, Formularansicht, Validierung S-05)
5. **I-05 Fotos:** Aufnahme/Import, Presets, Drehen/Zuschneiden (S-03 ohne Annotation)
6. **I-06 PDF:** Renderer, Vorschau, Export, Entwurfs-Wasserzeichen (S-06), Abschluss (S-07)
7. **I-07 Import/Export:** FormInterchange + S-08
8. **I-08 Designer:** Leinwand, Baukasten, Raster/Andocken, Eigenschaften, Bausteine, Versionierung (S-09…S-11)
9. **I-09 Annotation & Unterschrift:** PencilKit-Flows (S-03-Annotation, S-04)
10. **I-10 Härtung:** Barrierefreiheit, Dynamic Type, Querformat-Feinschliff, UI-Smoke-Tests
