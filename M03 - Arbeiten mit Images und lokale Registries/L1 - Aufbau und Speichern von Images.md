
# Docker für Java Schulung: Aufbau und Speichern von Images

Diese Schulung konzentriert sich auf das Erstellen und Speichern von Docker-Images für Java-Anwendungen und erklärt die Speicherstruktur und den Aufbau der Images im Detail.

## Container-Images und das Union Filesystem verstehen

Docker verwendet ein **Union Filesystem**, das es ermöglicht, mehrere Dateisystemschichten übereinander zu legen und so ein einziges konsistentes Dateisystem zu erstellen. Dies ist besonders wichtig für Docker-Images, da es ihnen erlaubt, Änderungen effizient zu speichern und verschiedene Versionen eines Images leicht zu verwalten.

### Grundprinzipien des Union Filesystems
- **Schichten (Layers)**: Jedes Docker-Image besteht aus mehreren Layers. Jede Änderung am Dateisystem (zum Beispiel das Hinzufügen oder Modifizieren von Dateien) erstellt eine neue Schicht. Dies verbessert die Effizienz und erlaubt das Wiederverwenden von unveränderten Schichten.
- **Schreibgeschützter und beschreibbarer Layer**: Die untersten Layers eines Images sind schreibgeschützt, während der oberste Layer, der sogenannte "Container Layer", beschreibbar ist. Änderungen am Dateisystem werden in diesem Layer vorgenommen, ohne die darunterliegenden Schichten zu beeinflussen.

### Beispiel einer Layer-Struktur
Bei der Erstellung eines Images für eine einfache Java-Anwendung kann die Schichtenstruktur wie folgt aussehen:
1. **Basis-Layer**: Java Runtime Environment (JRE) oder Java Development Kit (JDK).
2. **Applikations-Layer**: Java-Klassen und Ressourcen (zum Beispiel aus einer `.jar`-Datei).
3. **Konfigurations-Layer**: Konfigurationsdateien und Environment-Variablen für die Anwendung.

## Speicherstruktur und Layer eines Images

Ein Docker-Image ist im Wesentlichen eine Sammlung von schreibgeschützten Schichten, die in einer bestimmten Reihenfolge übereinander gestapelt sind. Jedes Mal, wenn ein Befehl in einem Dockerfile ausgeführt wird, erstellt Docker eine neue Schicht, die das Ergebnis des Befehls enthält.

### Aufbau eines Docker-Images für eine Java-Anwendung

Beispiel Dockerfile:
```dockerfile
# Basis-Layer
FROM openjdk:11-jre-slim

# Applikations-Layer
COPY target/my-java-app.jar /app/my-java-app.jar

# Konfigurations-Layer
WORKDIR /app
ENTRYPOINT ["java", "-jar", "my-java-app.jar"]
```

In diesem Beispiel:
1. Der **Basis-Layer** stellt die Java Runtime Umgebung bereit.
2. Der **Applikations-Layer** kopiert die kompilierten `.jar`-Dateien der Java-Anwendung ins Image.
3. Der **Konfigurations-Layer** setzt das Arbeitsverzeichnis und definiert den Startbefehl für den Container.

### Schichten (Layers) verstehen und optimieren
- **Cache nutzen**: Docker cached jede Schicht, um beim erneuten Aufbau des Images Zeit zu sparen. Häufige Änderungen sollten daher in unteren Schichten vermieden werden.
- **Minimale Basis-Images**: Verwenden Sie möglichst kleine Basis-Images, um die Größe des Docker-Images zu minimieren.

### Speicherung und Verteilung von Images
- Nach dem Aufbau des Images kann dieses lokal gespeichert oder in einem Docker-Registry, wie Docker Hub oder einer privaten Registry, abgelegt werden.
- Der Befehl zum Speichern eines Images:
  ```bash
  docker save -o my-java-app.tar my-java-app:latest
  ```

- Zum Laden eines gespeicherten Images:
  ```bash
  docker load -i my-java-app.tar
  ```

### Übertragen von Images zu einer Registry
Nach Fertigstellung eines Images können Sie es an eine Docker-Registry pushen, um es für andere Entwickler oder Deployment-Prozesse verfügbar zu machen.
```bash
docker tag my-java-app:latest your-dockerhub-username/my-java-app:latest
docker push your-dockerhub-username/my-java-app:latest
```
