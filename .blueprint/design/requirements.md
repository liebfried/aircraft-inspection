# Aircraft Inspection — Requirements-Baseline

Status: **ENTWURF — noch nicht freigegeben.**
Quelle: abgeleitet aus dem Vorgängerprojekt (`PROJECT-AIRCRAFT-INSPECTION-IPAD`,
altes Blueprint/Copilot) und dessen dokumentierten offenen Entscheidungen; wird
durch die laufende Discovery-Befragung vervollständigt.

## Produktziel

Native iPad-App für Flugzeug-Inspektionen: technische Prüfformulare digital
ausfüllen — inklusive Fotoaufnahme, Entwurfsverwaltung, Pflichtfeld-Validierung,
PDF-Vorschau und PDF-Export. Vollständig offline nutzbar, ohne Backend.

## Funktionale Anforderungen

### Formulare

- **REQ-001** Die App stellt Prüfformulare aus einer strukturierten
  Formulardefinition dar (Seiten mit geordneten Feldern).
- **REQ-002** Unterstützte Feldtypen mindestens: Überschrift, einzeiliger Text,
  mehrzeiliger Text, Datum, Checkliste (Status je Eintrag), Foto/Kamera.
- **REQ-003** Bis zur Lieferung des echten Prüfformulars nutzt die App genau ein
  Platzhalter-Formular. Es darf keine erfundenen fachlichen/regulatorischen
  Inhalte suggerieren.
- **REQ-004** Die Formulardefinition ist so gestaltet, dass das echte Formular
  später ohne Änderung der Engine eingepflegt werden kann (Daten, nicht Code).

### Entwürfe (Drafts)

- **REQ-010** Nutzer können zu einem Formular beliebig viele Entwürfe anlegen,
  öffnen, weiterbearbeiten und löschen.
- **REQ-011** Alle Eingaben werden automatisch gespeichert (Autosave); es gibt
  keinen verlierbaren „ungespeicherten" Zustand.
- **REQ-012** Entwürfe überleben App-Neustart, App-Update und Gerät-Neustart
  (lokale Persistenz).
- **REQ-013** Die App funktioniert vollständig offline; keine Funktion setzt
  eine Netzwerkverbindung voraus.

### Fotos

- **REQ-020** Fotofelder erlauben die Aufnahme per Kamera direkt aus dem
  Formular heraus.
- **REQ-021** Angehängte Fotos haben eine Vorschau und können gedreht und
  zugeschnitten werden.
- **REQ-022** Fotos werden mit konfigurierbarer Zielgröße/Kompression
  gespeichert (Presets klein/mittel/groß), um Speicher und PDF-Größe zu
  begrenzen.
- **REQ-023** Fotos sind Teil des Entwurfs und werden mit ihm gespeichert und
  gelöscht.

### Validierung

- **REQ-030** Pflichtfelder sind als solche definiert; die App zeigt fehlende
  Pflichtangaben verständlich und feldbezogen an.
- **REQ-031** Der PDF-Export eines unvollständigen Entwurfs wird verhindert
  oder erfordert eine bewusste Bestätigung (genaues Verhalten: offene Frage
  Q-05).

### PDF

- **REQ-040** Die App erzeugt aus einem Entwurf lokal (ohne Netz) ein PDF mit
  allen Feldwerten und angehängten Fotos.
- **REQ-041** Das PDF kann in der App als Vorschau angezeigt werden.
- **REQ-042** Das PDF kann über das iOS-Share-Sheet exportiert werden (Dateien,
  Mail, AirDrop etc.).

### Erweiterungen (Priorisierung in Discovery — Q-01)

- **REQ-050 (Kandidat)** Unterschriftenfeld: handschriftliche Unterschrift
  (Finger/Apple Pencil) als Feldtyp, im PDF wiedergegeben.
- **REQ-051 (Kandidat)** Foto-Annotation: Markierungen/Zeichnungen auf Fotos
  (z. B. Schadensstelle einkreisen).
- **REQ-052 (Kandidat)** Mehrere Formulare: Verwaltung mehrerer
  Formular-Vorlagen und Auswahl beim Anlegen eines Entwurfs.
- **REQ-053 (Kandidat)** Formular-Import: neue Formulardefinitionen ohne
  App-Update einspielen (z. B. JSON-Datei über die Dateien-App).

## Nicht-funktionale Anforderungen

- **REQ-060** Zielplattform: iPad (iPadOS); Mindestversion und unterstützte
  Geräte: offene Frage Q-06.
- **REQ-061** Bedienbarkeit im Prüfumfeld: große Touch-Ziele, Nutzung auch im
  Stehen/mit einer Hand; Orientierung (Hoch-/Querformat): offene Frage Q-06.
- **REQ-062** Keine Cloud-, Analytics- oder Tracking-Dienste; keine Daten
  verlassen das Gerät außer durch expliziten Nutzer-Export.
- **REQ-063** Sprache der Benutzeroberfläche: offene Frage Q-04.
- **REQ-064** Die Codebasis ist testbar aufgebaut; Formularlogik, Validierung,
  Persistenz und PDF-Erzeugung sind durch automatisierte Tests abgedeckt
  (Verbesserung gegenüber Vorgängerprojekt: dort 0 Unit-Tests).

## Explizit außerhalb des Scopes

- Backend-, Cloud- oder Synchronisierungs-Infrastruktur
- App-Store-Release, Signing- und Zertifikatsarbeiten
- Regulatorische Konformitätszusagen (TÜV, Luftfahrt, Datenschutz-Audits)
- Erfundene Inhalte für das echte Prüfformular

## Offene Fragen (Discovery)

| ID | Frage | Status |
| --- | --- | --- |
| Q-01 | Welche Erweiterungen (REQ-050…053) sind im Scope, in welcher Priorität? | offen |
| Q-02 | Werden personenbezogene Daten verarbeitet (Prüfername, Unterschrift)? DSGVO-Relevanz? | offen |
| Q-03 | Aufbewahrung: Wie lange bleiben Entwürfe/exportierte PDFs auf dem Gerät? Löschregeln? | offen |
| Q-04 | UI-Sprache: Deutsch, Englisch oder beides? | offen |
| Q-05 | Verhalten beim Export unvollständiger Entwürfe: hart blockieren oder mit Warnung erlauben? | offen |
| Q-06 | iPadOS-Mindestversion, Zielgeräte, Orientierung, Apple-Pencil-Pflicht? | offen |
| Q-07 | Fotoquellen: nur Kamera oder auch Fotobibliothek-Import? Max. Fotos pro Feld? | offen |
| Q-08 | Echtes Prüfformular: wer liefert es wann, in welchem Format? | offen (laut Nutzer: weiterhin Platzhalter) |
