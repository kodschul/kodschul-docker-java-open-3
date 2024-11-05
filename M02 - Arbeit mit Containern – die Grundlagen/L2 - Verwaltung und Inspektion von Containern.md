
# Docker für Java Schulung: Verwaltung und Inspektion von Containern

In dieser Schulung werden die grundlegenden Techniken zur Verwaltung und Inspektion von Docker-Containern für Java-Anwendungen behandelt. Ziel ist es, eine stabile und effiziente Umgebung für die Entwicklung und Ausführung von Java-Anwendungen zu schaffen.

## Container-Management: Prozesse, Speicher und Netzwerke verwalten

Docker-Container ermöglichen eine isolierte Umgebung für Anwendungen, was eine präzise Kontrolle über Prozesse, Speicher und Netzwerke erfordert.

### Prozesse verwalten
Um die in einem Container ausgeführten Prozesse zu überwachen und zu steuern, stehen verschiedene Docker-Befehle zur Verfügung:
- **Container starten und stoppen**:
  ```bash
  docker start <container_name>
  docker stop <container_name>
  ```
- **Laufende Prozesse anzeigen**:
  ```bash
  docker ps
  ```
- **Einen Prozess innerhalb eines Containers beenden**:
  ```bash
  docker kill <container_name>
  ```

### Speicher verwalten
Docker ermöglicht es, Volumes und Speicher-Optionen zur persistenten Speicherung von Daten zu verwenden:
- **Volumes erstellen und verwenden**:
  ```bash
  docker volume create <volume_name>
  docker run -v <volume_name>:/pfad/im/container <image_name>
  ```
- **Speicherplatz prüfen**:
  ```bash
  docker system df
  ```
  Dieser Befehl zeigt die Speicherauslastung für Images, Container und Volumes an.

### Netzwerke verwalten
Docker bietet verschiedene Netzwerkkonfigurationen, um Container miteinander oder mit externen Netzwerken zu verbinden:
- **Standardnetzwerk-Modi**:
  - **Bridge**: Container kommunizieren innerhalb desselben Hosts.
  - **Host**: Container nutzen den Netzwerkstack des Hosts direkt.
  - **None**: Kein Netzwerkzugang.
- **Netzwerk erstellen und verbinden**:
  ```bash
  docker network create <network_name>
  docker network connect <network_name> <container_name>
  ```

## Container inspizieren, Logs überwachen und Ressourcen nutzen

Die Inspektion und Überwachung von Containern ist unerlässlich, um den Zustand und die Leistung der Anwendungen zu überwachen und zu verbessern.

### Container inspizieren
Docker bietet Befehle zur detaillierten Inspektion von Containern:
- **Container-Details anzeigen**:
  ```bash
  docker inspect <container_name>
  ```
  Dieser Befehl zeigt umfangreiche Informationen zu Konfiguration, Speicher, Netzwerk und weiteren Container-Details.

### Logs überwachen
Das Überwachen von Logs ist wichtig, um Fehler und Aktivitäten des Containers zu verfolgen:
- **Container-Logs anzeigen**:
  ```bash
  docker logs <container_name>
  ```
- **Echtzeit-Logs verfolgen**:
  ```bash
  docker logs -f <container_name>
  ```

### Ressourcen überwachen und optimieren
Docker ermöglicht das Setzen von Limits für CPU und Arbeitsspeicher, um eine Überlastung des Hosts zu verhindern:
- **Ressourcenlimits setzen**:
  ```bash
  docker run --cpus=".5" --memory="512m" <image_name>
  ```
- **Aktuelle Ressourcennutzung überwachen**:
  ```bash
  docker stats
  ```
  Mit diesem Befehl wird eine Live-Ansicht der Ressourcennutzung aller Container angezeigt.
