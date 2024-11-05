
# Docker für Java Schulung: Kommunikation zwischen Containern ermöglichen

In dieser Schulung werden grundlegende Techniken zur Ermöglichung der Kommunikation zwischen Docker-Containern behandelt, insbesondere in einem Java-Entwicklungsumfeld. Die richtige Konfiguration der Container-Kommunikation ist entscheidend für verteilte Anwendungen, die auf mehreren Services basieren.

## Netzwerktypen in Docker

Docker bietet mehrere Netzwerktreiber, die für die Kommunikation zwischen Containern verwendet werden können:

1. **Bridge-Netzwerk**: Standardmäßig kommunizieren Container auf demselben Host über ein Bridge-Netzwerk, das automatisch von Docker erstellt wird.
2. **Host-Netzwerk**: Verwendet den Netzwerk-Stack des Hosts. Nützlich für Situationen, in denen niedrige Latenz erforderlich ist.
3. **Overlay-Netzwerk**: Ermöglicht die Kommunikation zwischen Containern auf verschiedenen Hosts und wird häufig in Docker Swarm- oder Kubernetes-Umgebungen verwendet.

In den meisten Java-Projekten wird das Bridge-Netzwerk verwendet, um die Kommunikation zwischen Service-Containern zu ermöglichen.

## Beispiel: Zwei Java-Container kommunizieren lassen

In diesem Beispiel werden zwei Java-Dienste eingerichtet, die über ein gemeinsames Netzwerk miteinander kommunizieren:

### Schritt 1: Netzwerk erstellen

Erstellen Sie zunächst ein benutzerdefiniertes Netzwerk, in dem die Container kommunizieren können:
```bash
docker network create mein-netzwerk
```

### Schritt 2: Container in das Netzwerk einbinden

Im Folgenden wird ein Container für einen Java-Anwendungsdienst (App-Service) und ein weiterer Container für eine Datenbank gestartet.

1. **Dockerfile für den Java-Dienst** (Datei: `Dockerfile`)
    ```dockerfile
    FROM openjdk:11-jre-slim
    COPY target/myapp.jar /app/myapp.jar
    CMD ["java", "-jar", "/app/myapp.jar"]
    ```

2. **Container starten und ins Netzwerk einbinden**:
    ```bash
    docker run -d --name app-service --network mein-netzwerk app-image
    docker run -d --name db-service --network mein-netzwerk -e POSTGRES_PASSWORD=example postgres
    ```

### Schritt 3: Konfiguration der Java-Anwendung für die Kommunikation

In der Java-Anwendung muss die Verbindung zur Datenbank nun über den Containernamen (`db-service`) hergestellt werden. Beispiel für die Konfiguration einer JDBC-URL:
```java
String jdbcUrl = "jdbc:postgresql://db-service:5432/mydatabase";
```

Durch die Verwendung des Containernamens innerhalb des Netzwerks kann der App-Service problemlos auf den Datenbank-Container zugreifen.

## Netzwerkprüfungen und Fehlerbehebung

Um sicherzustellen, dass die Container miteinander kommunizieren können, bietet Docker verschiedene Netzwerk-Tools:

1. **Docker Netzwerkprüfung**:
    ```bash
    docker network inspect mein-netzwerk
    ```
    Diese Anweisung zeigt Informationen über alle Container im Netzwerk und deren IP-Adressen an.

2. **Ping zwischen Containern**:
    Sie können die Konnektivität zwischen Containern prüfen, indem Sie den `ping` Befehl verwenden:
    ```bash
    docker exec -it app-service ping db-service
    ```