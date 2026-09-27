# TankstellenErtrag – Kundenplattform

Diese Branch enthält den separaten geschützten Einstieg für Pilotkunden. **main und der v43.19-Safe-Stand bleiben unverändert.**

## Zielarchitektur

- Öffentliche Website: `tankstellenertrag.de`
- Kundenbereich: später `app.tankstellenertrag.de`
- Authentifizierung: Supabase Auth mit E-Mail/Passwort
- Daten: Supabase PostgreSQL + Row Level Security (RLS)
- Analyse: bestehender TankstellenErtrag-MVP v43.19
- Pilotzugang: serverseitig für genau eine konkrete Station
- Gründungspartner: maximal 10 gleichzeitig
- Nach Ablauf: niemals automatische kostenpflichtige Verlängerung

## Supabase – vorbereitete Reihenfolge

Der v43.19-Stand enthält die technische Cloud-/Datenschutzlogik. Für die Einrichtung müssen die vorhandenen SQL-Dateien in ihrer vorgesehenen Reihenfolge ausgeführt werden:

1. vorhandenes Supabase-Schema aus v43.x
2. `DB_MIGRATION_v3_VERIFICATION.sql`
3. `DB_MIGRATION_v4_PAID_BYPASS.sql`
4. `DB_MIGRATION_v5_PRIVACY.sql`
5. `DB_MIGRATION_v6_SECURITY_HARDENING.sql`
6. `DB_MIGRATION_v7_RETENTION_AUTODELETE.sql`
7. `DB_MIGRATION_v8_RESTORE_CANCELLATION.sql`
8. `DB_MIGRATION_v9_FOUNDING_PARTNER.sql`
9. `DB_MIGRATION_v10_FOUNDING_PARTNER_GUARD.sql`
10. `DB_MIGRATION_v11_DATA_HYGIENE.sql`

**Wichtig:** Die v10-/v11-Schritte dürfen nicht übersprungen werden. Sie sichern die 10-Gründungspartner-Grenze und den täglichen Retention-Lauf.

## Frontend-Konfiguration

In `supabase-config.js` dürfen ausschließlich die öffentliche Supabase-Projekt-URL und der Publishable Key stehen.

**Niemals**:
- service_role key
- Secret key
- sonstige serverseitige Zugangsdaten

Die Konfiguration wird erst eingetragen, wenn das konkrete Supabase-Projekt feststeht.

## Sicherheitsprüfung vor Pilotstart

Mit zwei separaten Testkonten muss nachgewiesen werden:

- Konto A sieht nur seine eigene Station.
- Konto B sieht nur seine eigene Station.
- A kann die Station von B nicht lesen.
- A kann keinen Snapshot von B lesen.
- A kann keinen Snapshot von B anlegen oder ändern.
- Ein normaler Kunde kann die serverseitige Gründungspartner-Funktion nicht ausführen.
- Eine Station kann nicht über das Frontend selbst auf `founding_partner` gesetzt werden.
- Die maximale Zahl aktiver Gründungspartner bleibt 10.
- Ein Eigentümerkonto kann nicht zwei aktive Gründungspartner-Stationen erhalten.

## Noch nicht öffentlich freischalten

Diese Branch ist absichtlich noch nicht produktiv.

Vor dem Merge nach `main` müssen mindestens geprüft werden:

1. Supabase-Projekt und RLS
2. Auth-E-Mail-Bestätigung
3. zwei getrennte Testkonten
4. Stationstrennung
5. Gründungspartner-Freischaltung
6. Ablauf/30-Tage-Löschung
7. tatsächlicher Einstieg in den bestehenden Analyse-MVP
8. Domain/DNS

Erst danach wird über einen Merge nach `main` entschieden.
