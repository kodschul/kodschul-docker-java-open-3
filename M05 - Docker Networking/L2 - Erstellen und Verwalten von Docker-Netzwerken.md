
# Docker für Java Schulung: Erstellung und Verwaltung von Container-Netzwerken

Diese Schulung behandelt die Grundlagen der Erstellung und Verwaltung von Docker-Netzwerken, die für den Betrieb von Java-Anwendungen in isolierten und miteinander verbundenen Umgebungen erforderlich sind. Durch den Einsatz von Docker-Netzwerken können verschiedene Container (z.B. Web-Apps, Datenbanken) miteinander kommunizieren und auf sichere Weise Daten austauschen.

## Erstellung und Verwaltung von Container-Netzwerken

### 1. Einführung in Docker-Netzwerke
Docker-Netzwerke ermöglichen die Kommunikation zwischen Containern und sind ein zentraler Bestandteil beim Betrieb von containerisierten Anwendungen. Durch Netzwerke kann ein Container einfach und sicher mit anderen kommunizieren, ohne dass dazu externe Verbindungen notwendig sind.

### 2. Netzwerk-Typen in Docker
Docker bietet verschiedene Netzwerk-Typen an, die je nach Anwendungsfall unterschiedlich geeignet sind:

- **Bridge-Netzwerk**: Das Standard-Netzwerk für Container, das eine isolierte Umgebung schafft und die Kommunikation zwischen Containern auf demselben Host ermöglicht.
- **Host-Netzwerk**: Ermöglicht Containern, direkt auf dem Host-Netzwerk zu arbeiten. Dies kann nützlich sein, wenn die Container auf die gleichen Netzwerk-Ports wie der Host zugreifen sollen.
- **Overlay-Netzwerk**: Wird vor allem für Docker Swarm und Multi-Host-Deployments verwendet, um Container über verschiedene Hosts hinweg zu verbinden.

### 3. Erstellen eines Bridge-Netzwerks
Ein Bridge-Netzwerk ist der häufigste Netzwerktyp und eignet sich gut für Java-Anwendungen, die in einer isolierten Umgebung auf demselben Host kommunizieren müssen.

Beispiel zum Erstellen eines Bridge-Netzwerks:
```bash
docker network create my-bridge-network
```

Verbindung eines Containers zu einem Bridge-Netzwerk:
```bash
docker run -d --name my-java-app --network my-bridge-network openjdk:11 java -jar /path/to/app.jar
```

### 4. Verbindung von Containern über ein benutzerdefiniertes Netzwerk
Docker-Container, die demselben benutzerdefinierten Netzwerk zugeordnet sind, können sich gegenseitig über den Container-Namen erreichen.

Beispiel:
```bash
docker run -d --name database --network my-bridge-network postgres:latest
docker run -d --name java-app --network my-bridge-network openjdk:11 java -jar /path/to/app.jar
```
Hier kann `java-app` die Datenbank `database` direkt über den Namen ansprechen (z.B. `jdbc:postgresql://database:5432/dbname`).

### 5. Verwaltung von Docker-Netzwerken
Zur Überwachung und Verwaltung von Docker-Netzwerken stehen verschiedene Docker-Befehle zur Verfügung:

- **Netzwerke anzeigen**: Alle vorhandenen Netzwerke auflisten
  ```bash
  docker network ls
  ```

- **Netzwerk-Details anzeigen**: Details eines Netzwerks anzeigen, z.B. verbundene Container
  ```bash
  docker network inspect my-bridge-network
  ```

- **Container aus Netzwerk entfernen**: Container aus einem Netzwerk trennen
  ```bash
  docker network disconnect my-bridge-network my-java-app
  ```

- **Netzwerk löschen**: Ungenutztes Netzwerk entfernen (Container müssen vorher getrennt werden)
  ```bash
  docker network rm my-bridge-network
  ```

### 6. Praktisches Beispiel: Java Anwendung mit einer Datenbank verbinden
In diesem Beispiel wird eine Java-Anwendung, die eine PostgreSQL-Datenbank verwendet, innerhalb eines Docker-Bridge-Netzwerks bereitgestellt:

1. **Erstellen des Netzwerks**:
   ```bash
   docker network create java-app-network
   ```

2. **Datenbank starten**:
   ```bash
   docker run -d --name postgres-db --network java-app-network -e POSTGRES_PASSWORD=example postgres:latest
   ```

3. **Java-Anwendung starten und verbinden**:
   ```bash
   docker run -d --name java-app --network java-app-network openjdk:11 java -jar /path/to/app.jar
   ```

Die Java-Anwendung kann nun über den Hostnamen `postgres-db` auf die Datenbank zugreifen.
