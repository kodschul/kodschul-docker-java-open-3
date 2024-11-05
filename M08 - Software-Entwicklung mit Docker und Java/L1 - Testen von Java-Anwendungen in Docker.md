
# Docker für Java Schulung: Testen von Java-Anwendungen in Docker

Diese Schulung deckt den Einsatz von Docker zur Isolation und Automatisierung von Tests für Java-Anwendungen ab. Sie zeigt die Vorteile und Techniken zur Nutzung von Containern für die Testumgebung und stellt einige nützliche Tools vor.

## Verwendung von Docker zur Isolation von Testumgebungen

Docker bietet eine effektive Möglichkeit, Java-Anwendungen in einer isolierten Umgebung zu testen. Mit Containern können Entwickler Testumgebungen erstellen, die unabhängig von der lokalen Umgebung sind und alle benötigten Abhängigkeiten enthalten.

### Vorteile der Isolierung mit Docker:
- **Konsistenz**: Alle Testläufe erfolgen in derselben Umgebung, was reproduzierbare Ergebnisse sicherstellt.
- **Flexibilität**: Unterschiedliche Konfigurationen und Versionen von Abhängigkeiten können einfach getestet werden.
- **Unabhängigkeit von der lokalen Entwicklungsumgebung**: Vermeidet Konflikte durch Abweichungen zwischen der lokalen Umgebung und der Produktionsumgebung.

### Beispiel für das Starten eines Docker-Containers für eine Java-Anwendung:
```bash
docker run --rm -v $(pwd):/app -w /app openjdk:11 javac Main.java
docker run --rm -v $(pwd):/app -w /app openjdk:11 java Main
```

## Automatisiertes Testen in Containern

Automatisierte Tests lassen sich einfach in Docker-Containern ausführen. Dies ist besonders nützlich für CI/CD-Pipelines, da Tests isoliert und unabhängig voneinander ausgeführt werden können.

### Einrichtung automatisierter Tests mit Docker:
1. **Dockerfile** erstellen, das alle benötigten Abhängigkeiten installiert.
2. Test-Suite in den Container integrieren.
3. Tests bei jedem Build automatisch ausführen lassen.

#### Beispiel für ein Dockerfile zum Testen einer Java-Anwendung:
```Dockerfile
# Verwenden eines Basis-Images für Java
FROM openjdk:11

# Arbeitsverzeichnis erstellen
WORKDIR /app

# Projektdateien in den Container kopieren
COPY . /app

# Tests ausführen
CMD ["./gradlew", "test"]
```

### Ausführen der Tests:
```bash
docker build -t java-test-app .
docker run --rm java-test-app
```

## Tools für das Testen von containerisierten Java-Anwendungen

Es gibt mehrere Tools, die speziell für das Testen von Java-Anwendungen in Docker-Umgebungen entwickelt wurden:

### 1. **Testcontainers**
- **Beschreibung**: Testcontainers ist eine Java-Bibliothek, die Container zur Integrationstests bereitstellt. Sie wird oft genutzt, um Test-Datenbanken, Message Broker und andere externe Dienste zu simulieren.
- **Vorteile**: Testcontainers ermöglicht das Erstellen und Verwalten von Containern direkt aus dem Java-Code heraus und eignet sich hervorragend für Tests, die Datenbanken oder andere Services erfordern.

#### Beispiel mit Testcontainers:
```java
import org.testcontainers.containers.GenericContainer;
import org.testcontainers.utility.DockerImageName;

public class MyTest {
    public static GenericContainer<?> redis = new GenericContainer<>(DockerImageName.parse("redis:5.0.3")).withExposedPorts(6379);

    @BeforeAll
    public static void setUp() {
        redis.start();
    }

    @Test
    public void testSomething() {
        // Test-Logik, die den Redis-Container verwendet
    }
}
```

### 2. **JUnit und Docker**
- **Beschreibung**: JUnit kann zusammen mit Docker genutzt werden, um Tests automatisch in isolierten Containern auszuführen.
- **Vorteile**: Kombiniert die Vorteile von Docker und JUnit, um Tests zu isolieren und in einer kontrollierten Umgebung laufen zu lassen.

#### Beispiel:
Eine einfache `docker-compose.yml`-Datei kann erstellt werden, um Dienste wie eine Test-Datenbank hochzufahren, bevor JUnit-Tests gestartet werden.

```yaml
version: '3'
services:
  app:
    image: openjdk:11
    volumes:
      - .:/app
    working_dir: /app
    command: ["./gradlew", "test"]
  db:
    image: postgres:12
    environment:
      POSTGRES_USER: test
      POSTGRES_PASSWORD: test
```