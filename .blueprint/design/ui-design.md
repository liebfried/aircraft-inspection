# Aircraft Inspection — UI-Design

Status: **Zur Abnahme vorgelegt.**
Mockups: [`mockups/mockups.html`](mockups/mockups.html) — selbstständig im Browser zu öffnen.
Grundlage: freigegebene Requirements-Baseline (`requirements.md`, 2026-08-17).

## Gestaltungsprinzip

Natives iPadOS-Idiom (Split View, gruppierte Listen, System-Schrift, Sheets) —
vertraut für Nutzer, minimale Einarbeitung, und direkt auf SwiftUI-Standard-
komponenten abbildbar. Formulare sind durchgängig **WYSIWYG**: Designer,
Ausfüllansicht und PDF zeigen dieselbe A4-Seite (REQ-007/074). Akzentfarbe „Hangar-Blau" (#175CB6); Statusfarben
(Grün/Orange/Rot) sind vom Akzent getrennt und werden nie als einziges
Unterscheidungsmerkmal eingesetzt.

## Screen-Inventar

| ID | Screen | Realisiert | Art |
| --- | --- | --- | --- |
| S-01 | Vorlagen & Entwürfe (Split View) | REQ-005, 006, 010, 014, 015 | Hauptscreen |
| S-02 | Formularansicht (Ausfüllen, WYSIWYG) | REQ-001, 011, 020, 024, 030, 074 | Hauptscreen |
| S-03 | Foto-Annotation | REQ-021, 025 | Sheet aus S-02 |
| S-04 | Unterschrift | REQ-026 | Sheet aus S-02 |
| S-05 | Validierungsübersicht | REQ-030, 032 | Sheet aus S-02 |
| S-06 | PDF-Vorschau & Export | REQ-031, 040, 041, 042 | Screen aus S-02 |
| S-07 | Abschluss & Wiedereröffnen | REQ-015, 032 | Zustand von S-02 + Dialoge |
| S-08 | Formular-Import | REQ-004, 006 | Sheet aus S-01 |
| S-09 | Formular-Designer (Baukasten & Leinwand) | REQ-007, 070, 071, 008, 009 | Hauptscreen aus S-01 |
| S-10 | Element-Eigenschaften | REQ-071, 073 | Sheet aus S-09 |
| S-11 | Standard-Bausteine | REQ-070, 072 | Sheet aus S-09 |

Abgedeckt sind damit alle funktionalen Anforderungen; REQ-012/013/023 (Persistenz,
Offline, Foto-Lebenszyklus) sind verhaltensbezogen und in den Screens als
Autosave-Anzeige, „WLAN aus"-Statusleiste und Foto-Feld-Verhalten sichtbar.

## Navigationsfluss

- S-01 (Vorlage wählen → Entwurf öffnen/anlegen) → S-02
- S-02 → Sheets: S-03 (Foto antippen), S-04 (Unterschriftsfeld), S-05 („Validierung prüfen")
- S-02 → S-06 („PDF-Vorschau") → Share-Sheet-Export
- S-02 → S-07 („Abschließen", nur bei voller Validierung); S-07 → „Wiedereröffnen…" → zurück zu S-02 editierbar (mit Vermerk)
- S-01 → S-08 („Formular importieren…")
- S-01 → S-09 („Neues Formular erstellen…" oder „Bearbeiten" einer Vorlage); im Designer: Element antippen → S-10, „Standard-Bausteine…" → S-11; „Version veröffentlichen" erzeugt eine neue Vorlagen-Version (REQ-008)

## Layout-Begründungen

- **Split View als Grundgerüst** (S-01, S-02): iPad-typisch, funktioniert in
  Hoch- und Querformat; im Hochformat kollabiert die Seitenleiste einblendbar.
- **WYSIWYG-Formularansicht**: Ausfüllen direkt auf der gestalteten A4-Seite
  (zoombar); Feld antippen öffnet Tastatur/Datumsrad/Sheet. Pflichtfeld-★ plus
  Fehler-Tag am Feld statt reiner Farbcodierung; Seiten-Miniaturen mit ⚠/✓.
- **Validierung dreistufig sichtbar**: pro Feld (Rahmen + Text), pro Seite
  (⚠/✓ in der Seitenliste), gesamt (Badge in der Navbar + Übersichts-Sheet).
- **Abschluss als Zustand, nicht als Export-Nebeneffekt**: Banner + Read-only
  macht den Archivcharakter unmissverständlich (REQ-015).
- **Entwurfs-Wasserzeichen** auf jeder PDF-Seite bei unvollständigem Export
  (REQ-031), inklusive „— fehlt —"-Markierung leerer Pflichtfelder im PDF.
- **Designer als Baukasten + Leinwand**: Palette links (Struktur / Statisch /
  Eingabe), A4-Leinwand mit zuschaltbarem 4-mm-Raster, Einrasten an Raster und
  Elementkanten (Ausrichtungslinien), Auswahl mit Griffen und mm-genauen Maßen
  (REQ-070/071). Kopf-/Fußzeile wiederholen sich je Seite; Standard-Bausteine
  aus S-11 beschleunigen den Aufbau (REQ-072). Der Versions-Banner macht die
  Wirkung von Änderungen vor dem Veröffentlichen explizit (REQ-008).

## Barrierefreiheit

- Touch-Ziele ≥ 44 pt; Bedienung im Stehen mitgedacht (REQ-061)
- Status nie nur über Farbe (Symbole + Text)
- Dynamic Type, VoiceOver-Labels, reflow-fähige Listenlayouts
- Alle Kernfunktionen ohne Apple Pencil bedienbar (REQ-060)

## Designentscheidungen aus dem Review

| ID | Frage | Entscheidung |
| --- | --- | --- |
| UD-01 | Seiten-Navigation im Editor | **Variante A** (Seitenleiste) — 2026-08-17 |
| UD-02 | Änderungswünsche S-01…S-08 | keine — 2026-08-17 |
| UD-03 | Formular-Designer ergänzt | Visueller Baukasten S-09/S-10/S-11 gemäß Nachtrag N-01 Rev. 2 |
| UD-04 | Ausfüllmodus | WYSIWYG-Formularansicht (REQ-074) — 2026-08-17 |
| UD-05 | Leinwand-/Seitenformat | A4 hochkant, PDF-deckungsgleich — 2026-08-17 |
