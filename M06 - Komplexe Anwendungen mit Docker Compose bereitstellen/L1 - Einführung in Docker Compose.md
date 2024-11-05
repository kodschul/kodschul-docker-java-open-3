
# Einführung in Docker Compose

Diese Schulung bietet eine Einführung in Docker Compose und dessen Anwendung für Java-Projekte. Sie umfasst die Grundlagen von Docker Compose und die Strukturierung von YAML-Konfigurationsdateien sowie den Aufbau einer Docker-Compose-Datei für eine Java-Anwendung.

## Grundlagen von Docker Compose und YAML-Konfigurationen

Docker Compose ist ein Tool, das die Orchestrierung mehrerer Docker-Container ermöglicht. Es nutzt YAML-Dateien, um Multi-Container-Anwendungen zu definieren und zu verwalten, wodurch komplexe Umgebungen einfach gestartet, gestoppt und konfiguriert werden können.

### Grundlegende Konzepte:

- **Services**: Definieren die verschiedenen Anwendungen oder Komponenten, die ausgeführt werden sollen. Jeder Service entspricht einem Container.
- **Netzwerke**: Ermöglichen die Kommunikation zwischen den Containern.
- **Volumes**: Speichern Daten dauerhaft und teilen sie zwischen den Containern.

### Beispiel einer einfachen YAML-Konfiguration:
```yaml
version: '3'
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
  db:
    image: postgres:latest
    environment:
      POSTGRES_USER: example
      POSTGRES_PASSWORD: example
```

In diesem Beispiel:
- Zwei Services (`web` und `db`) werden definiert.
- Der `web`-Service nutzt das `nginx`-Image und veröffentlicht den Port 80 auf dem Host-Port 8080.
- Der `db`-Service verwendet `postgres` und setzt die Umgebungsvariablen `POSTGRES_USER` und `POSTGRES_PASSWORD`.

## Aufbau einer Docker-Compose-Datei

Eine Docker-Compose-Datei (`docker-compose.yml`) ist im YAML-Format geschrieben und ermöglicht das Starten mehrerer Container mit einem einzigen Befehl.

### Schritte zum Erstellen einer Docker-Compose-Datei für eine Java-Anwendung

1. **Grundlegende Struktur erstellen**:
   Erstelle eine Datei namens `docker-compose.yml` im Projektverzeichnis.

2. **Java-Anwendung und Abhängigkeiten definieren**:
   Definiere Services für die Java-Anwendung und ggf. zugehörige Datenbanken oder andere Abhängigkeiten.

   Beispiel einer `docker-compose.yml` Datei für eine Java-Anwendung mit einer PostgreSQL-Datenbank:
   ```yaml
   version: '3'
   services:
     app:
       image: openjdk:11
       volumes:
         - .:/app
       working_dir: /app
       command: ["java", "-jar", "target/myapp.jar"]
       depends_on:
         - db

     db:
       image: postgres:latest
       environment:
         POSTGRES_USER: user
         POSTGRES_PASSWORD: password
         POSTGRES_DB: mydatabase
       ports:
         - "5432:5432"
   ```

In diesem Beispiel:
- Der `app`-Service verwendet das `openjdk:11`-Image und führt die Java-Anwendung als JAR-Datei im `target`-Ordner aus.
- Die `db`-Service-Konfiguration stellt eine PostgreSQL-Datenbank bereit und setzt Umgebungsvariablen für den Benutzer, das Passwort und die Datenbank.
- `depends_on` sorgt dafür, dass der `db`-Service zuerst gestartet wird.

### Docker Compose Befehle

Nach dem Erstellen der `docker-compose.yml` Datei kannst du die Multi-Container-Anwendung starten und verwalten:
- **Starten**: `docker-compose up` (fügt `-d` hinzu, um die Anwendung im Hintergrund zu starten)
- **Stoppen**: `docker-compose down` (entfernt die Container, aber nicht die Volumes)
- **Logs anzeigen**: `docker-compose logs`
- **Neustarten eines bestimmten Services**: `docker-compose restart app`