# Kundenplattform v1

Diese Seite ist der geschützte Einstieg für die TankstellenErtrag-Testkunden.

## Architektur

- Öffentliche Website: `/`
- Kundenplattform: `/plattform/`
- Analyse-MVP: wird nach erfolgreicher Anmeldung über `/` geöffnet
- Cloud: Supabase Auth + PostgreSQL/RLS
- Keine Service-/Secret-Keys im Frontend

## Einrichtung

1. Supabase-Projekt anlegen (Central EU / Frankfurt).
2. Die bestehenden v43.19-SQL-Dateien in der vorgesehenen Reihenfolge ausführen.
3. Anonymous Sign-ins bleiben für den alten MVP möglich; die Kundenplattform verwendet zusätzlich normale E-Mail/Passwort-Accounts.
4. `supabase-config.js` mit Projekt-URL und Publishable Key befüllen.
5. Vor Pilotstart RLS mit zwei Testkonten prüfen: Konto A darf Station B weder lesen noch schreiben.
6. Gründungspartner werden serverseitig für genau eine Pilotstation freigeschaltet.

## Wichtig

Die Branch enthält bewusst nur die neue Plattform-Einstiegsseite. Der bestehende `main`-Stand wird nicht überschrieben.
