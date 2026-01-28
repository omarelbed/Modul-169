## Testfälle – Nextcloud

### Testfall NC-1: Erreichbarkeit der Weboberfläche

**Datum:** 28.01.2026

**Tester:** Dario Gambone

**Ziel:**  
Überprüfung, ob die Nextcloud-Weboberfläche über den vorgesehenen Port erreichbar ist.

**Voraussetzungen:**  
- Docker-Infrastruktur ist gestartet  
- Nextcloud-Container läuft

**Testschritte:**  
1. Webbrowser öffnen  
2. URL `http://localhost:8080` aufrufen  

**Erwartetes Resultat:**  
- Die Nextcloud-Weboberfläche wird angezeigt  

**Testergebnis:**  
OK

**Beweis:**  
![Nextcloud Home](../bilder/nextcloud_home.png)


### Testfall NC-2: Datei erstellen und speichern

**Datum:** 28.01.2026

**Tester:** Dario Gambone

**Ziel:**  
Überprüfung der Dateiablage und Speicherfunktion von Nextcloud.

**Voraussetzungen:**  
- Anmeldung als Administrator in Nextcloud  
- Weboberfläche erreichbar

**Testschritte:**  
1. Anmeldung als Admin-Benutzer  
2. Wechsel zum Menüpunkt Files
3. Neue Datei erstellen "test.txt"

**Erwartetes Resultat:**  
- Die Datei wird erfolgreich erstellt 
- Die Datei ist in der Dateiliste sichtbar  

**Testergebnis:**  
OK  

**Beweis:**  
![Nextcloud Testfile](../bilder/nextcloud_testfile.png)


### Testfall NC-3: Persistenz nach Container-Neustart

**Datum:** 28.01.2026

**Tester:** Dario Gambone

**Ziel:**  
Überprüfung, ob Daten nach einem Neustart der Container weiterhin vorhanden sind.

**Voraussetzungen:**  
- Testfall NC-2 erfolgreich durchgeführt  
- Datei ist in Nextcloud vorhanden

**Testschritte:**  
1. Docker-Container neu starten (`docker compose restart nextcloud`)  
2. Nach dem Neustart Anmeldung in Nextcloud  
3. Wechsel zum Menüpunkt Files

**Erwartetes Resultat:**  
- Die zuvor erstellte Datei ist weiterhin vorhanden  

**Testergebnis:**  
OK

**Beweis:**  
![Nextcloud Docker Restart](../bilder/nextcloud_dockerrestart.png)

![Nextcloud Restart File](../bilder/nextcloud_restartfile.png)



