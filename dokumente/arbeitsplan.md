# Arbeitsplan

## Projektstart und Planung / 09.01.2026

- Aufgabe verstehen
- Git-Repository erstellen
- Grundstruktur des Projektes anlegen
- Erste Dokumentation erstellen

## Planen und Entscheiden / 16.09.2026

- Architektur grob geplant (Container, Netzwerke, Ports)
- Entscheidung getroffen:
    - Nextcloud (Port 8080)
    - MediaWiki (Port 8085)
    - Gitea als Git-Dienst (ressourcenschonend statt GitLab)
    - Portainer als Monitoring
    - phpMyAdmin für DB-Administration
    - Persistenz-Konzept definiert (Volumes/Bind-Mounts)
    - Sicherheitsanforderungen festgehalten (Passwörter, interne Netze, Zugriff nur über definierte Ports)
    - Testkonzept und Testplan als Vorlage vorbereitet



## Realisieren und Basis-Checks / 16.09 - 28.01.2026
- Alle Services laufen stabil:
    - Nextcloud erreichbar (8080)
    - MediaWiki erreichbar (8085)
    - Gitea erreichbar (3000/2222)
    - Portainer erreichbar (9000)
    - phpMyAdmin erreichbar (8081)
- Erste Funktionsprüfung: Containerstatus / Ports / Logs
- Repo fortlaufend versioniert (Compose + Doku + Anpassungen)

## Test und Absicherung / 28.01.2026 - 29.01.2026

## Abschluss und Abgabe / 30.01.2026

