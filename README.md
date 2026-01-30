# Modul 169 – Containerbasierte Infrastruktur mit Docker Compose

## Projektbeschreibung
Dieses Projekt wurde im Rahmen des Moduls 169 umgesetzt. Ziel war der Aufbau einer containerbasierten Infrastruktur für ein Informatik-KMU mithilfe von Docker Compose. Die gesamte Umgebung wird als Infrastructure as Code (IaC) realisiert und über ein Git-Repository versioniert.

## Anforderungen
Im Projekt wurden folgende Anforderungen umgesetzt:

- MediaWiki für interne Dokumentation (Port 8085)
- Nextcloud als Filesharing- und Kollaborationsplattform (Port 8080)
- Git-basierte Codeverwaltung mittels Gitea (CVS)
- Persistente Speicherung aller relevanten Daten
- Monitoring der Container mit Portainer
- Versionierung aller Konfigurations- und Dokumentationsdateien mit Git

## Systemübersicht

| Dienst       | Beschreibung                                   | Port |
|-------------|-----------------------------------------------|------|
| Nextcloud   | Filesharing und Kollaboration                  | 8080 |
| MediaWiki   | Interne Wissensdatenbank                       | 8085 |
| Gitea       | Git-basierte Codeverwaltung (CVS)              | 3000 |
| Portainer   | Monitoring und Verwaltung der Container        | 9000 |
| Datenbanken | MariaDB / PostgreSQL (intern, keine Ports)     | –    |

Eine detaillierte Architekturübersicht befindet sich im Dokument `systemueberblick.md`.

## Inhalt des Repository
Die wichtigsten Dateien und Verzeichnisse des Projekts sind:

- `docker-compose.yml` – Definition der gesamten Infrastruktur
- `.env.example` – Beispielkonfiguration der Umgebungsvariablen
- `README.md` – Projektübersicht
- `arbeitsplan.md` – Arbeits- und Zeitplanung
- `arbeitsjournal.md` – Chronologisches Arbeitsjournal
- `tests.md` – Testkonzept, Testplan und Testprotokoll
- `sicherheitskonzept.md` – Sicherheitskonzept

## Installation und Start
Voraussetzungen:

- Docker
- Docker Compose

Projekt starten:

```bash
docker compose up -d
