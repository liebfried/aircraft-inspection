# Aircraft Inspection — Requirements-Baseline

Status: **Zur Freigabe vorgelegt.**
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
  Persistenz, PDF-Erzeugung und Import sind durch automatisierte Tests
  abgedeckt.

## Priorisierung der Erweiterungen

Alle vier Erweiterungen sind im Scope. Implementierungsreihenfolge (nach
Kern-MVP): 1. Formular-Import (REQ-006), 2. mehrere Formulare (REQ-005),
3. Foto-Annotation (REQ-025), 4. Unterschriftenfeld (REQ-026). Die Reihenfolge
kann in Phase 4 einvernehmlich angepasst werden.

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
