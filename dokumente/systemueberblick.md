# Systemüberblick

## Ziel des Systems

Ziel dieses Projekts ist der Aufbau einer containerbasierten Infrastruktur für ein Informatik-KMU.  
Die Infrastruktur stellt mehrere interne Dienste zur Verfügung und basiert vollständig auf Docker und Docker Compose.

Die Lösung verfolgt folgende Ziele:

- Bereitstellung eines internen Kollaborations- und Filesharing-Systems
- Bereitstellung eines internen Wissensmanagement-Systems
- Bereitstellung einer internen Code-Versionierungsplattform (CVS)
- Persistente Speicherung aller relevanten Daten
- Zentrale Überwachung der Container
- Reproduzierbarkeit der Infrastruktur mittels Infrastructure as Code (IaC)


## Gesamtarchitektur

Die gesamte Infrastruktur wird mittels Docker Compose realisiert.  
Jeder Dienst läuft in einem eigenen Container und kommuniziert über interne Docker-Netzwerke mit den zugehörigen Datenbankcontainern.

Der Zugriff von aussen erfolgt ausschliesslich über definierte Ports auf dem Hostsystem.  
Datenbanken sind nicht direkt von aussen erreichbar.

**Architekturprinzipien:**
- Trennung von Applikation und Datenbank
- Persistente Speicherung über Volumes / Bind-Mounts
- Ressourcenkontrolle mittels Memory-Limits
- Zentrale Überwachung mit Portainer


## Diensteübersicht

### Nextcloud (Filesharing und Collaboration)

**Zweck:**  
Nextcloud dient als zentrale Plattform für Dateiverwaltung und Zusammenarbeit innerhalb des Unternehmens.

**Komponenten:**
- Nextcloud (Web-Applikation)
- MariaDB (Datenbank)

**Zugriff:**
- Webinterface: `http://localhost:8080`

**Persistente Daten:**
- Nextcloud-Datenverzeichnis
- Konfigurationsverzeichnis
- MariaDB-Datenbank

**Besonderheiten:**
- Persistenz wurde durch Container-Neustart getestet
- Initiale Konfiguration über den Webinstaller


### MediaWiki (Wissensmanagement)

**Zweck:**  
MediaWiki wird als internes Wiki zur Dokumentation von Prozessen, Wissen und technischen Informationen eingesetzt.

**Komponenten:**
- MediaWiki (Web-Applikation)
- MariaDB (Datenbank)

**Zugriff:**
- Webinterface: `http://localhost:8085`

**Persistente Daten:**
- Upload-Verzeichnis - `images`
- Konfigurationsdatei - `LocalSettings.php`
- MariaDB-Datenbank

**Besonderheiten:**
- `LocalSettings.php` wird persistent mittels Bind-Mount eingebunden
- Uploads bleiben auch nach Container-Neustart erhalten


### Gitea 

**Zweck:**  
Gitea dient als internes Code-Versionierungs-System (CVS) für die Verwaltung von Quellcode.

**Begründung für Gitea:**  
Gitea wurde bewusst anstelle von GitLab eingesetzt, da es deutlich ressourcenschonender ist und die Anforderungen der Aufgabenstellung vollständig erfüllt.

**Komponenten:**
- Gitea (Web-Applikation)
- PostgreSQL (Datenbank)

**Zugriff:**
- Webinterface: `http://localhost:3000`
- Git via SSH: Port `2222`

**Persistente Daten:**
- Gitea-Datenverzeichnis 
- PostgreSQL-Datenbank


### Portainer (Monitoring & Verwaltung)

**Zweck:**  
Portainer dient zur Überwachung und Verwaltung der laufenden Docker-Container.

**Zugriff:**
- Webinterface: `http://localhost:9000`

**Funktionen:**
- Übersicht über alle Container
- Status- und Ressourcenüberwachung
- Anzeige von Logs

**Persistente Daten:**
- Portainer-Datenverzeichnis



## Persistenzkonzept

Alle produktiven Daten werden persistent gespeichert, um Datenverlust bei Container-Neustarts zu verhindern.

**Umgesetzte Massnahmen:**
- Einsatz von Docker Volumes und Bind-Mounts
- Trennung von Infrastruktur-Code und Laufzeitdaten
- Manuelle Erstellung der Bind-Mount-Verzeichnisse zur Vermeidung falscher Besitzrechte

Persistente Daten umfassen:
- Datenbankdaten (MariaDB, PostgreSQL)
- Benutzerdateien (Nextcloud)
- Konfigurationsdateien (MediaWiki, Gitea)
- Repositories (Gitea)


## Ressourcenmanagement

Um die Systemressourcen kontrolliert zu nutzen, wurden für die Container Memory-Limits definiert:

- Webserver-Container: max. 300 MB RAM
- Datenbank-Container: max. 700 MB RAM

Diese Limits sind im Docker-Compose-File konfiguriert und können über Portainer überwacht werden.


## Sicherheit (Überblick)

Die Sicherheitsaspekte des Systems basieren auf folgenden Grundsätzen:

- Keine Passwörter im Docker-Compose-File
- Verwendung von .env-Dateien für sensible Konfigurationswerte
- Datenbanken nicht direkt von aussen erreichbar
- Zugriff nur über definierte Ports
- Trennung interner und externer Kommunikation

Eine detaillierte Betrachtung erfolgt im separaten Dokument, im file `systemueberblick.md` ist dieses Thema ausführlicher beschrieben.


## Zusammenfassung

Die implementierte Infrastruktur erfüllt alle Anforderungen der Aufgabenstellung:

- Funktionierende Nextcloud-Instanz
- Funktionierendes CVS (Gitea)
- Funktionierende MediaWiki-Instanz
- Persistente Datenspeicherung
- Zentrales Monitoring mit Portainer
- Reproduzierbare Infrastruktur mittels Docker Compose


