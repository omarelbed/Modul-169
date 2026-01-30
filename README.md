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
- Directory wählen für das man keine root rechte benötigt und ein Directory das nicht Synchronisiert wird. (Dient der sichherheit des hosts.)


-  Projekt klonen in einen Pfad der nicht höhere rechte braucht

```bash
cd /gewünschter/repo/Pfad
```

- Repo klonen

```bash
user@dungeon:~/gewünschter/repo/Pfad/$git clone https://github.com/omarelbed/Modul-169.git
```

#### Projekt starten:

.env erstellen mit `cp` Befehl aus vorlage
```bash
cp .env.example .env
```

.env befüllen

```bash
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
```

```bash
docker compose up -d
```

### Einzelne Container Konfigurieren


#### MediaWiki

Am besten fängt man mit MediaWiki an weil dies am längsten dauert und einen neustart erfordert.

- Mit `http://localhost:8085` auf MediaWiki Verbinden.

- Verbindung und Konfiguration mit Passwörtern und Usernamen aus .env herstellen

- LocalSettings.php herunterladen (Wenn möglich direkt in repo oder in mediawiki Ordner ablegen)

- LocalSettings.php in mediawiki Ordner verschieben

```bash
# Wenn in Repo Ordner
sudo cp LocalSettings.php ../mediawiki/LocalSettings.php
```

- Inhaber und gruppe ändern auf LocalSettings.php

```bash
sudo chown 999:999 LocalSettings.php
```

- Rechte auf 750 setzen
```bash
sudo chmod 750 LocalSettings.php
```
- mediawiki Volumen in  docker-compose.yml entkommentieren und speichern

```bash
docker compose down
```
```bash
docker compose up -d
```
Ab jetzt ist die konfiguration eingelesen in MediaWiki und sollte nicht mehr flüchtig sein bei einem neustart.


#### Gitea Konfiguration


Auf Gitea verbinden mit `http://localhost:3000`


- Konfigurieren
- Dropdown öffnen für erstellung von Administratoren account erstellung
- Informationen eingeben und Unten in der mitte auf blauen Button clicken.

Fertig die installation sollte automatisch die Homepage öffnen.


#### Portainer

Auf Portainer verbinden mit `http:localhost:9000`

Wennam anfang eine weisse Website erscheint muss der Portainer neugestartet werden.

```bash
docker restart portainer
```

- Website refreshen

- Wenn das login Fenster erscheint ein Passwort anlegen.

- create User Klicken

- Sobald man im Dashboard ist auf "Local"(Docker Icon) Klicken verbinden und in der Übersicht sollte 1 Stack mit 7 containern erscheinen.


#### Nextcloud

Auf Nextcloud verbinden mit `http:localhost:8080`

- Administrator User und Passwort anlegen

- "Installieren" Klicken

- Apps installieren muss man nicht kann man aber

