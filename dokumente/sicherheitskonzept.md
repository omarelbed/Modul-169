# Sicherheitskonzept

## Ziel und Geltungsbereich

Dieses Sicherheitskonzept beschreibt die wichtigsten Sicherheitsrisiken und Massnahmen für die containerbasierte Infrastruktur (Docker Compose) mit folgenden Diensten:

- Nextcloud (Web + MariaDB)
- MediaWiki (Web + MariaDB)
- Gitea (Web + PostgreSQL)
- Portainer (Monitoring)


## Randbedingungen / Umfeld

- Die Dienste laufen containerisiert auf einer VM mittels Docker Engine und Docker Compose.
- Der Zugriff auf die Dienste erfolgt über definierte Ports auf dem Hostsystem.
- Die Daten werden persistent über Volumes / Bind-Mounts gespeichert.
- Der Infrastruktur-Code (docker-compose.yml, .env.example, Doku) wird im Git-Repository versioniert.
- Sensible Werte (Passwörter, Keys) werden nicht in die Versionsverwaltung eingecheckt.


## Bedrohungsanalyse (containerspezifische Risiken)

### Unautorisierter Zugriff auf Web-Interfaces
**Risiko:** Offene Web-Ports können von unberechtigten Personen erreicht werden (z. B. im falschen Netzwerk/Firewall-Setup).  
**Betroffene Dienste:** Nextcloud, MediaWiki, Gitea, Portainer.

**Massnahmen:**
- Zugriff nur über definierte Ports (keine unnötigen Port-Freigaben).
- Administrationsoberflächen (v. a. Portainer) nur für Admins, starkes Passwort.
- (Empfehlung für Produktivbetrieb) Zugriff per Firewall auf interne Netze beschränken oder Reverse Proxy mit Auth/VPN einsetzen.



### Datenbankzugriff von aussen
**Risiko:** Direkte Exponierung von Datenbankports erhöht die Angriffsfläche.  
**Massnahmen:**
- Datenbankcontainer veröffentlichen **keine** Ports nach aussen.
- Zugriff auf Datenbanken erfolgt ausschliesslich über das interne Docker-Netzwerk.



### Umgang mit Zugangsdaten (Secrets)
**Risiko:** Passwörter im Compose-File oder in Git ermöglichen Missbrauch.  
**Massnahmen:**
- Im `docker-compose.yml` sind **keine** Passwörter im Klartext hinterlegt.
- Zugangsdaten werden über Umgebungsvariablen aus einer `.env`-Datei bereitgestellt.
- In der Versionsverwaltung liegt nur eine `.env.example` ohne echte Secrets.
- `.env` ist per `.gitignore` ausgeschlossen.



### Persistente Daten und Berechtigungen (Volumes / Bind-Mounts)
**Risiko:** Falsche Dateirechte/Besitzrechte auf dem Host können zu Datenlecks oder Fehlfunktionen führen.  
**Massnahmen:**
- Bind-Mount-Verzeichnisse wurden bewusst manuell erstellt, um korrekte Besitzrechte sicherzustellen.
- Laufzeitdaten (Datenbanken, User-Dateien, Uploads, Repos) werden nicht versioniert.
- Persistenz wird getestet (Neustart der Container, Daten bleiben erhalten).



### Container-/Image-Sicherheit (Supply Chain)
**Risiko:** Veraltete Images oder Images aus unsicheren Quellen können Schwachstellen enthalten.  
**Massnahmen:**
- Verwendung etablierter offizieller Images (z. B. mariadb, postgres, nextcloud, mediawiki, gitea, portainer).
- (Empfehlung für Betrieb) Regelmässiges Aktualisieren der Images und zeitnahes Einspielen von Security Updates.
- (Empfehlung) Fixe Version-Tags statt `latest`, um Updates kontrolliert durchzuführen.



### Over-Privilege / Host Escape
**Risiko:** Container könnten mit zu hohen Rechten laufen oder Zugriff auf Host-Ressourcen erhalten.  
**Massnahmen:**
- Keine unnötigen Privileged-Container.
- Portainer benötigt Zugriff auf den Docker Socket zur Überwachung; das wird als bewusstes Risiko akzeptiert und durch restriktiven Zugriff auf Portainer (Admin-Zugang, nur intern) abgesichert.




### Denial of Service durch Ressourcenverbrauch
**Risiko:** Container können die VM durch RAM-Verbrauch destabilisieren.  
**Massnahmen:**
- Ressourcenbeschränkungen (Memory-Limits) sind im Compose definiert gemäss Vorgaben:
  - Webserver max. ~300 MB
  - Datenbanken max. ~700 MB
- Überwachung des Ressourcenverbrauchs über Portainer.



## Konkrete Security-Massnahmen in der Umsetzung

### Netzwerk- und Port-Konzept
- Extern erreichbar (Host-Ports):
  - Nextcloud: 8080
  - MediaWiki: 8085
  - Gitea Web: 3000
  - Gitea SSH: 2222
  - Portainer: 9000
- Datenbanken sind nur intern erreichbar (keine externen Port-Mappings).

### Passwort- und Benutzerkonzept
- Admin-Konten werden pro Dienst separat geführt (Nextcloud/Gitea/Portainer/MediaWiki).
- Starke Passwörter werden verwendet.
- Passwörter werden nicht im Repository gespeichert.

### Backups (Betriebsempfehlung)
Für einen produktiven Betrieb sind regelmässige Backups zwingend:
- Datenbankdumps (MariaDB/PostgreSQL)
- Nextcloud Datenverzeichnis
- Gitea Repositories / Konfiguration
- MediaWiki Uploads & LocalSettings.php

(Hinweis: Backup-Jobs sind nicht Teil der Infrastruktur-Implementierung dieses Projekts, werden aber empfohlen.)



## Nachweis der Anforderung „Compose enthält keine Passwörter“
Die Infrastruktur erfüllt die Anforderung, dass im `docker-compose.yml` keine Passwörter hinterlegt sind.
Sensible Werte werden über `.env` geladen; ins Repository wird nur `.env.example` eingecheckt.



## Fazit
Die Sicherheitsmassnahmen reduzieren die Angriffsfläche der containerisierten Dienste durch:
- keine externen Datenbankports
- Secrets nicht im Compose/Git
- kontrollierte Port-Freigaben
- persistente Datenhaltung mit bewusster Rechteverwaltung
- Ressourcenlimits zur Stabilität
- Monitoring über Portainer

