
# Docker für Java Schulung

## Eigene Images erstellen und verwalten

In dieser Schulung lernen wir, wie eigene Docker-Images für Java-Anwendungen erstellt, versioniert und verwaltet werden. Dabei werden die Grundlagen der Dockerfile-Erstellung sowie Best Practices zur Optimierung von Images behandelt.

### Aufbau und Verwaltung eigener Images (Dockerfile, Best Practices)

Ein Dockerfile definiert die Anweisungen, um ein Docker-Image zu erstellen. Dies umfasst das Einrichten der Laufzeitumgebung, das Kopieren der Anwendung und die Installation aller erforderlichen Abhängigkeiten.

#### Beispiel: Dockerfile für eine Java-Anwendung

Ein einfaches Dockerfile für eine Java-Anwendung könnte folgendermaßen aussehen:
```dockerfile
# Basis-Image verwenden
FROM openjdk:11-jre-slim

# Arbeitsverzeichnis erstellen
WORKDIR /app

# JAR-Datei in das Arbeitsverzeichnis kopieren
COPY target/meine-anwendung.jar /app/meine-anwendung.jar

# Container Start-Befehl
ENTRYPOINT ["java", "-jar", "/app/meine-anwendung.jar"]
```

#### Best Practices für Dockerfiles
- **Schlanke Basis-Images verwenden**: Verwenden Sie leichte Images wie `openjdk:11-jre-slim`, um die Größe des Images zu reduzieren.
- **Layer optimieren**: Vermeiden Sie unnötige Layer, und fassen Sie Befehle zusammen, um den Image-Aufbau effizienter zu gestalten.
- **Cache-Mechanismus nutzen**: Strukturieren Sie das Dockerfile so, dass häufige Änderungen in späteren Layern liegen, damit frühere Layer im Cache bleiben können.
- **Sicherheitsupdates und Berechtigungen**: Führen Sie Sicherheitsupdates und notwendige Berechtigungen im Dockerfile aus, um die Sicherheit zu verbessern.

### Images versionieren, sichern und verwalten

Die Verwaltung von Docker-Images umfasst nicht nur die Erstellung, sondern auch die Versionierung, das Sichern und die Speicherung in einer Registry.

#### 1. Versionierung der Images
Durch das Tagging können unterschiedliche Versionen eines Images gekennzeichnet und später spezifisch aufgerufen werden.
```bash
# Tagging des Images mit einer Versionsnummer
docker build -t mein-java-app:1.0 .
docker build -t mein-java-app:latest .
```

#### 2. Speicherung in einer Registry
Um die Images zugänglich zu machen, können diese in einer Docker-Registry (z.B. Docker Hub oder GitLab Container Registry) gespeichert werden.
```bash
# Image in eine Registry pushen
docker tag mein-java-app:1.0 myregistry.com/mein-java-app:1.0
docker push myregistry.com/mein-java-app:1.0
```

#### 3. Backups und Sicherheit
Um sicherzustellen, dass Images gesichert und verfügbar bleiben, können sie exportiert und als Backup gespeichert werden.
```bash
# Image als Datei exportieren
docker save -o mein-java-app.tar mein-java-app:1.0

# Exportiertes Image laden
docker load -i mein-java-app.tar
```

#### 4. Image Cleanup und Verwaltung
Verwenden Sie regelmäßig `docker image prune`, um nicht genutzte Images zu löschen und Speicherplatz freizugeben.
