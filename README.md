# Immoakte

Prototyp einer PWA zur Immobilienkalkulation und Objektverwaltung. Läuft vollständig offline, alle Daten bleiben im Browser des Geräts. Kein Server, kein Konto, keine Übertragung nach außen.

## Was drin ist

**Register** – alle Objekte in einer Liste mit Statusstreifen, Suche, Filter und Sortierung nach Kaufpreisfaktor, Rendite, Cashflow oder Preis. Oben die Summe über das Portfolio: Investition gesamt und monatlicher Cashflow.

**Objektakte** mit fünf Reitern:

| Reiter | Inhalt |
|---|---|
| Objekt | Bezeichnung, Lage, Objektart, Fläche, Zimmer, Baujahr, Zustand, Notizen |
| Kalkulation | Kaufpreis, Nebenkosten, Finanzierung, Mieten, Bewirtschaftungskosten, alle Kennzahlen |
| Anzeigen | Links zu Inseraten mit Portal, Angebotspreis, Datum und Notiz |
| Dokumente | PDFs und Bilder per Ziehen oder Auswahl, mit Kategorie, Vorschau und Download |
| Schriftverkehr | E-Mails, Briefe, Telefonate und Besichtigungen mit Datum, Richtung und Inhalt |

**Kennzahlen**, live beim Tippen: Kaufnebenkosten je Position, Gesamtinvestition, Darlehen, Beleihung, Rate mit Zins- und Tilgungsanteil im ersten Jahr, Jahreskaltmiete, Bewirtschaftungskosten, Reinertrag, Kaufpreis und Miete je m², Kaufpreisfaktor, Brutto- und Nettomietrendite, Cashflow je Monat und Jahr, Vermögenszuwachs, Eigenkapitalrendite.

Die Grunderwerbsteuer wird aus dem gewählten Bundesland vorbelegt (Stand 2025) und lässt sich je Objekt überschreiben.

## Speicherung

Objekte, Anzeigen und Schriftverkehr liegen in IndexedDB, Dokumente als Blob daneben. Über das Menü oben rechts:

- **Objekte sichern** – kompaktes JSON ohne Dateien
- **Objekte mit Dokumenten sichern** – JSON inklusive aller PDFs als Base64, wird entsprechend groß
- **Sicherung einlesen** – ergänzt vorhandene Daten, gleiche IDs werden überschrieben
- **Speicher dauerhaft sichern** – bittet den Browser, die Daten nicht automatisch zu verwerfen (empfohlen, sobald Dokumente drin liegen)

Sicherungen sind die einzige Kopie. Ohne sie sind die Daten bei Geräteverlust weg.

## Veröffentlichen auf GitHub Pages

1. Eigenes Repository anlegen, die Dateien ins Wurzelverzeichnis legen:
   ```
   index.html
   sw.js
   manifest.webmanifest
   icons/
   ```
2. Settings → Pages → Source: `Deploy from a branch`, Branch `main`, Ordner `/ (root)`.
3. Nach ein bis zwei Minuten läuft die App unter `https://<konto>.github.io/<repo>/`.
4. Auf dem Handy über den Browser öffnen und zum Startbildschirm hinzufügen. Danach startet sie ohne Netz.

Alle Pfade sind relativ, ein Unterordner im Repository stört also nicht.

Beim Ausrollen einer neuen Fassung die Zeile `const CACHE = 'immoakte-v1'` in `sw.js` hochzählen, sonst zeigt der Service Worker bei Bestandsnutzern weiter die alte Datei.

## Später als App im Play Store

Die App erfüllt die Anforderungen für eine Trusted Web Activity: Manifest mit `name`, `short_name`, `start_url`, `display: standalone`, Theme-Farbe und Icons ab 512 px inklusive maskierbarer Fassung. Für das Bundle reicht Bubblewrap gegen die Pages-URL, dazu die `assetlinks.json` unter `/.well-known/` im Repository.

## Technischer Aufbau

Eine einzelne `index.html` mit eingebettetem CSS und JavaScript, ohne Framework und ohne Build. Der Service Worker hält die App-Shell vor: Seitenaufrufe zuerst aus dem Netz mit Rückfall auf den Cache, übrige Dateien zuerst aus dem Cache.

Schriften kommen aus dem System, damit offline nichts nachgeladen werden muss.

## Was noch fehlt

- Tilgungsplan über die Zinsbindung, bisher nur das erste Jahr
- Steuerliche Seite: Abschreibung, Werbungskosten, Ergebnis nach Steuern
- Mieter und Mietverträge als eigene Ebene bei Mehrfamilienhäusern
- Volltextsuche in den abgelegten PDFs
- Fotos direkt aus der Kamera bei der Besichtigung
