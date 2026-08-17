# Aircraft Inspection — UI-Design

Status: **Zur Abnahme vorgelegt.**
Mockups: [`mockups/mockups.html`](mockups/mockups.html) — selbstständig im Browser zu öffnen.
Grundlage: freigegebene Requirements-Baseline (`requirements.md`, 2026-08-17).

## Gestaltungsprinzip

Natives iPadOS-Idiom (Split View, gruppierte Listen, System-Schrift, Sheets) —
vertraut für Nutzer, minimale Einarbeitung, und direkt auf SwiftUI-Standard-
komponenten abbildbar. Akzentfarbe „Hangar-Blau" (#175CB6); Statusfarben
(Grün/Orange/Rot) sind vom Akzent getrennt und werden nie als einziges
Unterscheidungsmerkmal eingesetzt.

## Screen-Inventar

| ID | Screen | Realisiert | Art |
| --- | --- | --- | --- |
| S-01 | Vorlagen & Entwürfe (Split View) | REQ-005, 006, 010, 014, 015 | Hauptscreen |
| S-02 | Formular-Editor | REQ-001, 002, 011, 020, 024, 030 | Hauptscreen |
| S-03 | Foto-Annotation | REQ-021, 025 | Sheet aus S-02 |
| S-04 | Unterschrift | REQ-026 | Sheet aus S-02 |
| S-05 | Validierungsübersicht | REQ-030, 032 | Sheet aus S-02 |
| S-06 | PDF-Vorschau & Export | REQ-031, 040, 041, 042 | Screen aus S-02 |
| S-07 | Abschluss & Wiedereröffnen | REQ-015, 032 | Zustand von S-02 + Dialoge |
| S-08 | Formular-Import | REQ-004, 006 | Sheet aus S-01 |
| S-09 | Formular-Designer | REQ-007, 008, 009 (Nachtrag N-01) | Hauptscreen aus S-01 |
| S-10 | Feld-Einstellungen | REQ-007 | Sheet aus S-09 |

Abgedeckt sind damit alle funktionalen Anforderungen; REQ-012/013/023 (Persistenz,
Offline, Foto-Lebenszyklus) sind verhaltensbezogen und in den Screens als
Autosave-Anzeige, „WLAN aus"-Statusleiste und Foto-Feld-Verhalten sichtbar.

## Navigationsfluss

- S-01 (Vorlage wählen → Entwurf öffnen/anlegen) → S-02
- S-02 → Sheets: S-03 (Foto antippen), S-04 (Unterschriftsfeld), S-05 („Validierung prüfen")
- S-02 → S-06 („PDF-Vorschau") → Share-Sheet-Export
- S-02 → S-07 („Abschließen", nur bei voller Validierung); S-07 → „Wiedereröffnen…" → zurück zu S-02 editierbar (mit Vermerk)
- S-01 → S-08 („Formular importieren…")
- S-01 → S-09 („Neues Formular erstellen…" oder „Bearbeiten" einer Vorlage) → S-10 (Feld antippen); „Version veröffentlichen" erzeugt eine neue Vorlagen-Version (REQ-008)

## Layout-Begründungen

- **Split View als Grundgerüst** (S-01, S-02): iPad-typisch, funktioniert in
  Hoch- und Querformat; im Hochformat kollabiert die Seitenleiste einblendbar.
- **Felder als Karten** mit Label oben: klare Zuordnung bei Dynamic Type und
  VoiceOver; Pflichtfeld-★ plus Fehlertext statt reiner Farbcodierung.
- **Validierung dreistufig sichtbar**: pro Feld (Rahmen + Text), pro Seite
  (⚠/✓ in der Seitenliste), gesamt (Badge in der Navbar + Übersichts-Sheet).
- **Abschluss als Zustand, nicht als Export-Nebeneffekt**: Banner + Read-only
  macht den Archivcharakter unmissverständlich (REQ-015).
- **Entwurfs-Wasserzeichen** auf jeder PDF-Seite bei unvollständigem Export
  (REQ-031), inklusive „— fehlt —"-Markierung leerer Pflichtfelder im PDF.
- **Designer folgt dem Editor-Muster** (Seitenleiste + Feldkarten mit
  Drag-Griffen): Wer Formulare ausfüllen kann, kann sie auch bauen. Der
  Versions-Banner macht die Wirkung von Änderungen vor dem Veröffentlichen
  explizit (REQ-008).

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
| UD-03 | Formular-Designer ergänzt | S-09/S-10 gemäß Nachtrag N-01 (REQ-007…009) |
