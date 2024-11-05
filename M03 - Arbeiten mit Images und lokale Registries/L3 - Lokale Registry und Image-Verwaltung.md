
# Docker für Java Schulung: Lokale Registry und Image-Verwaltung

Diese Schulung behandelt die Nutzung einer lokalen Docker-Registry für die Verwaltung und Wiederverwendung von Images in Java-Projekten.

## Einrichten und Verwenden einer lokalen Registry

Eine lokale Docker-Registry ermöglicht das Speichern und Abrufen von Images innerhalb des eigenen Netzwerks, ohne auf eine öffentliche Registry wie Docker Hub angewiesen zu sein.

### Schritte zum Einrichten einer lokalen Registry

1. **Starten einer lokalen Registry**:
   Docker bietet ein fertiges Registry-Image, das direkt verwendet werden kann:
   ```bash
   docker run -d -p 5000:5000 --name my-local-registry registry:2
   ```
   - Dies startet die Registry auf Port 5000, was der Standardport für eine lokale Registry ist.

2. **Überprüfen der Registry**:
   Nach dem Start kann überprüft werden, ob die Registry läuft:
   ```bash
   curl http://localhost:5000/v2/
   ```
   - Ein leeres JSON-Objekt `{}` als Antwort zeigt an, dass die Registry bereit ist.

### Vorteile einer lokalen Registry

- **Schnellerer Zugriff**: Bilder sind schneller verfügbar, da sie nicht von externen Servern heruntergeladen werden müssen.
- **Zugangskontrolle**: Durch eine lokale Registry können Images im Netzwerk geteilt werden, ohne dass diese öffentlich zugänglich sind.
- **Kostenersparnis**: Reduzierung der Bandbreite und der externen Speicheranforderungen.

## Images in einer Registry speichern und wiederverwenden

Nachdem die lokale Registry eingerichtet ist, können Docker-Images hochgeladen und wiederverwendet werden.

### Schritte zum Speichern eines Images in der Registry

1. **Image taggen**:
   Um ein Image in der lokalen Registry zu speichern, muss das Image mit dem Registry-Pfad getaggt werden.
   ```bash
   docker tag my-java-app localhost:5000/my-java-app
   ```

2. **Image in die Registry pushen**:
   Pushe das Image in die lokale Registry:
   ```bash
   docker push localhost:5000/my-java-app
   ```

3. **Image aus der Registry ziehen**:
   Um das Image von der Registry wiederzuverwenden, kann es einfach gepullt werden:
   ```bash
   docker pull localhost:5000/my-java-app
   ```

### Praxisbeispiel: Verwendung eines Java Docker-Images in der Registry

1. **Erstellen eines Dockerfiles** für eine Java-Anwendung:
   ```dockerfile
   FROM openjdk:11-jre-slim
   COPY target/my-java-app.jar /usr/src/myapp/my-java-app.jar
   WORKDIR /usr/src/myapp
   CMD ["java", "-jar", "my-java-app.jar"]
   ```

2. **Build des Docker-Images**:
   ```bash
   docker build -t my-java-app .
   ```

3. **Taggen und Pushen in die Registry**:
   ```bash
   docker tag my-java-app localhost:5000/my-java-app
   docker push localhost:5000/my-java-app
   ```

4. **Verwenden des gespeicherten Images**:
   Auf einem anderen Rechner oder Server im gleichen Netzwerk kann das Image nun gepullt und gestartet werden:
   ```bash
   docker pull localhost:5000/my-java-app
   docker run -d localhost:5000/my-java-app
   ```