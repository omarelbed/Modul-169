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

### Voraussetzungen

- Docker
- Docker Compose
- Ein Verzeichnis, das keine Root-Rechte benötigt
- Ein Verzeichnis, das nicht synchronisiert wird (z. B. kein Cloud-Sync), zur Erhöhung der Sicherheit des Hosts

### Projekt klonen

In ein geeignetes Verzeichnis wechseln:

cd /gewünschter/repo/Pfad

Repository klonen:

git clone https://github.com/omarelbed/Modul-169.git

### Projekt starten

.env-Datei anhand der Vorlage erstellen:

cp .env.example .env

.env-Datei mit den benötigten Zugangsdaten befüllen:

# MediaWiki – MariaDB  
MW_DB_NAME=mediawiki  
MW_DB_USER=mwuser  
MW_DB_PASS=CHANGE_ME_MW_DB_PASS  
MW_DB_ROOT_PASS=CHANGE_ME_MW_ROOT_PASS  

# Nextcloud – MariaDB  
NC_DB_NAME=nextcloud  
NC_DB_USER=ncuser  
NC_DB_PASS=CHANGE_ME_NC_DB_PASS  
NC_DB_ROOT_PASS=CHANGE_ME_NC_ROOT_PASS  

# Gitea – PostgreSQL  
GITEA_DB_NAME=gitea  
GITEA_DB_USER=gitea  
GITEA_DB_POSTGRES_PASS=CHANGE_ME_GITEA_POSTGRES_PASS  

Container starten:

docker compose up -d

## Einzelne Container konfigurieren

### MediaWiki

Es wird empfohlen, mit MediaWiki zu beginnen, da die Erstkonfiguration am längsten dauert und einen Neustart der Container erfordert.

- Verbindung zur Weboberfläche herstellen:  
  http://localhost:8085

- Installation durchführen und dabei die Datenbank-Zugangsdaten aus der .env-Datei verwenden

- Nach Abschluss der Installation die Datei LocalSettings.php herunterladen  
  (wenn möglich direkt im Repository oder im mediawiki-Ordner ablegen)

- LocalSettings.php in den mediawiki-Ordner verschieben (falls sie im Repository liegt):

sudo cp LocalSettings.php ../mediawiki/LocalSettings.php

- Eigentümer und Gruppe der Datei anpassen (UID/GID des MediaWiki-Containers):

sudo chown 999:999 LocalSettings.php

- Dateiberechtigungen setzen:

sudo chmod 750 LocalSettings.php

- MediaWiki-Volume im docker-compose.yml aktivieren (entkommentieren) und Container neu starten:

docker compose down  
docker compose up -d

Ab diesem Zeitpunkt ist die MediaWiki-Konfiguration persistent und bleibt auch nach Neustarts erhalten.

### Gitea Konfiguration

- Verbindung zur Weboberfläche herstellen:  
  http://localhost:3000

- Initialkonfiguration durchführen:
  - Dropdown-Menü für die Erstellung eines Administrator-Accounts öffnen
  - Benötigte Informationen eingeben
  - Unten auf den blauen Bestätigungsbutton klicken

Nach Abschluss der Installation wird automatisch die Startseite geöffnet.

### Portainer

- Verbindung zur Weboberfläche herstellen:  
  http://localhost:9000

- Falls zu Beginn eine leere (weisse) Seite angezeigt wird, Container neu starten:

docker restart portainer

- Webseite aktualisieren
- Administrator-Passwort festlegen
- Auf "Create User" klicken
- Im Dashboard auf "Local" (Docker-Icon) klicken, um die lokale Docker-Umgebung zu verbinden

In der Übersicht sollte anschliessend ein Stack mit mehreren Containern sichtbar sein.

### Nextcloud

- Verbindung zur Weboberfläche herstellen:  
  http://localhost:8080

- Administrator-Benutzer und Passwort anlegen
- Auf "Installieren" klicken
- Zusätzliche Apps können optional installiert werden
