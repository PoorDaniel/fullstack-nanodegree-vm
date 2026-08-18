# Übergabe an Session mit eNVenta-/SQL-Zugriff

**Erstellt:** 18.08.2026 · **Von:** Cloud-Session (nur Git-Repo, kein eNVenta)
**Auftrag:** Kontaktpersonen bei **eww Anlagentechnik GmbH** in eNVenta anlegen.

---

## 1. Auftrag

Drei Kontaktpersonen anlegen, Daten von Visitenkarten (Besuch 18.08.2026):

| Vorname | Nachname | Titel | Funktion | Telefon | Mobil | E-Mail | Adresse |
|---|---|---|---|---|---|---|---|
| Martin | Heindl | MSc | Bereichsleiter Installations- und Gebäudetechnik / Gewerberechtlicher Geschäftsführer | +43 7242 493-378 | +43 660 9309830 | martin.heindl@eww.at | Knorrstraße 6, 4600 Wels |
| Christian | Binder | — | Abteilungsleiter Elektrotechnik, Solar | +43 7242 493-124 | +43 664 4227346 | christian.binder@eww.at | Knorrstraße 6, 4600 Wels |
| Walter | Andlinger | — | Servicetechniker (eww Elektrotechnik) | +43 7242 493-170 | +43 664 4227344 | walter.andlinger@eww.at | Wiesenstraße 43, 4600 Wels |

Fertige Importdatei liegt im Repo:
`besuchsberichte/kontakte/2026-08-18_eww-anlagentechnik-kontakte.csv`
(Semikolon-getrennt, `Land` exakt „Österreich", Rechtsform bewusst leer,
`AKLNummer` leer — die fehlt noch, siehe Schritt 2.)

**Vierter Kontakt, ungeklärt:** *Bruno Reuttmeier* (Hauptansprechpartner,
Schreibweise unsicher, keine Visitenkarte). Bitte in eNVenta suchen — wenn er
existiert, korrekte Schreibweise zurückmelden; nicht neu anlegen.

## 2. Schritte

1. **Kundennummer (AKLNummer) ermitteln** — per SQL nach `SUCHNAME` / `FIRMA1`
   in Richtung „eww" / „eww Anlagentechnik" suchen. Achtung: eww könnte mehrfach
   angelegt sein (Anlagentechnik Knorrstraße 6 vs. Elektrotechnik
   Wiesenstraße 43) — die richtige Adresse zuordnen und melden.
2. **Bestand prüfen** — existieren Heindl / Binder / Andlinger schon als
   Ansprechpartner? Vorhandene nicht doppelt anlegen, sondern nur fehlende
   Felder ergänzen.
3. **Anlegen** — Skill `kundenneuanlage`, Weg **AdapterGeneralIO** (Kontext 6,
   Kennung `AichingerKunde`), Werkzeug `engine/kontakt_import.py`.
   Pflichtknoten laut Skill: `<AKLTyp>K</AKLTyp>`, `<Modus>1</Modus>`, und für
   Ansprechpartner zusätzlich `<AKLNummer>` einer **bestehenden** Nummer.
   ⚠️ Der Import prüft Schlüsselfelder nicht: `Land` muss exakt „Österreich"
   heißen, `Rechtsform` gar nicht senden — sonst steht `## Wert ##` im Feld.
   ⚠️ Der Weg ist laut Skill-Stand vom 13.08.2026 erst am Testkunden bewiesen
   und im Alltag nicht erprobt — vorher `neukundenimport-schnittstelle.md`
   lesen und die Entscheidung dort festhalten.
4. **SQL-Endkontrolle (Pflicht)** — angelegte Kontakte suchen und alle
   gelieferten Felder gegen die CSV prüfen.

## 3. Bitte zurückmelden

- Kundennummer von eww Anlagentechnik GmbH
- Welche Kontakte neu angelegt wurden, welche schon existierten
- Korrekte Schreibweise von Bruno Reuttmeier, falls auffindbar
- Ob der Weg über GeneralIO funktioniert hat oder ob Maske/Fallback nötig war

---

## 4. Kontext: Besuchstag 18.08.2026

Fünf Besuche, Berichte liegen unter `besuchsberichte/` im Branch
`claude/besuchsbericht-ebner-blechtechnik-4i0n8a`.

| Kunde | Gesprächspartner | Offene Aufgaben |
|---|---|---|
| Ebner Blechtechnik | Hr. Aspelmeyer (Stv. von Christoph Ebner), 12 MA, Lage mittel | Christoph Ebner nachfassen, Rückmeldung zu hinterlassenen Aktionen |
| Kremsmüller (Schwechat) | Stefan Schober; Facility: Martina Felbermeier, DW 1143 | Angebot 5 Sicherheitsschränke mit Umluftfilter; Felbermeier anrufen; Lithium-Ionen wird intern weitergegeben |
| Fronius Steinhaus | Hannes Bremberger; Facility Herbert Huber (auch Asecos) | Angebot je 1× Metabo Akku Lithium HD 5,5 Ah (625368000) und 4,0 Ah (625367000); 2027: 2 Schubladenschränke, ~4 Werkzeugwagen Beta. Abwicklung zentral über Sattledt |
| Fronius Wels | „Arsham" (Schreibweise/Nachname offen), mit Patrick Jank (Nilfisk) | Nilfisk-Angebot abwarten, dann Angebot 1–3 Sauger fürs Elektrodenschleifen; Kontakt in eNVenta prüfen |
| eww Anlagentechnik (Wels) | Reuttmeier, Heindl, Binder, Andlinger | Kontaktanlage (dieses Dokument); Binder nachfassen; Andlinger: 2× E-höhenverstellbare Werkbank 2 m + Aufbau, 1 Sicherheitsschrank (zeitlich offen) |
