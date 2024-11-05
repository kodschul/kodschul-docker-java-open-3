
# Docker für Java Schulung

## Debugging von Java-Anwendungen in Docker

Docker bietet eine leistungsstarke Umgebung zur Containerisierung und Bereitstellung von Java-Anwendungen. Das Debugging innerhalb von Containern unterscheidet sich jedoch in einigen Aspekten von herkömmlichen Methoden. Diese Schulung deckt Techniken zur Fehlersuche in Java-Anwendungen in Docker-Containern ab.

### Debugging von Java-Containern und Anwendungsfehlern

Die Fehlersuche in Docker-Containern erfordert spezielle Techniken, da Container isolierte Umgebungen bieten. Hier sind einige Ansätze, um Anwendungsfehler in Java-Containern zu beheben:

1. **Container Logs verwenden**:
   - Mit dem Befehl `docker logs <container-id>` können Sie die Standardausgabe und Standardfehlerausgabe des Containers anzeigen. Dies ist oft der erste Schritt zur Diagnose von Problemen.
   ```bash
   docker logs my-java-container
   ```

2. **Java-spezifische Debugging-Optionen**:
   - Verwenden Sie Java-Flags wie `-XX:+PrintGCDetails` oder `-XX:+PrintFlagsFinal` zur Analyse der JVM-Konfiguration und Garbage Collection.
   - Um zusätzliche Informationen über das Verhalten der Anwendung zu erhalten, fügen Sie `-Xdebug` und `-Xrunjdwp` Optionen hinzu, um den Debugging-Modus zu aktivieren.

3. **Zugriff auf den Container für erweiterte Diagnosen**:
   - Nutzen Sie den Befehl `docker exec -it <container-id> /bin/bash`, um eine Shell innerhalb des Containers zu starten. Von hier aus können Sie auf Anwendungsdateien zugreifen und Diagnosetools wie `jstack`, `jmap` oder `jstat` ausführen.
   ```bash
   docker exec -it my-java-container /bin/bash
   ```

### Verwendung von Remote-Debugging-Tools für Java in Docker

Remote-Debugging ist eine nützliche Technik, um Anwendungen innerhalb eines Containers zu debuggen, indem man eine Debugging-Umgebung von außerhalb des Containers verwendet. Um dies zu konfigurieren:

1. **Starten Sie den Java-Prozess im Debug-Modus**:
   - Starten Sie den Java-Prozess mit Remote-Debugging-Einstellungen. Beispiel:
   ```bash
   java -Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=*:5005 -jar myapp.jar
   ```

2. **Öffnen Sie den Debug-Port**:
   - Stellen Sie sicher, dass der Debug-Port (in diesem Fall 5005) im Dockerfile oder im `docker run` Befehl verfügbar gemacht wird:
   ```dockerfile
   EXPOSE 5005
   ```

   - Starten Sie den Container und öffnen Sie den Debug-Port:
   ```bash
   docker run -p 5005:5005 my-java-container
   ```

3. **Verbinden Sie Ihr IDE mit dem Remote-Debugger**:
   - Verwenden Sie Ihre Java-Entwicklungsumgebung (z.B. IntelliJ IDEA oder Eclipse), um eine Remote-Debugging-Sitzung auf Port 5005 zu starten. Stellen Sie sicher, dass der Host und Port korrekt konfiguriert sind.

### Troubleshooting von häufigen Containerproblemen

Java-Anwendungen in Containern können auf bestimmte Probleme stoßen, die spezifisch für die Containerumgebung sind. Hier sind einige häufige Probleme und deren Lösungen:

1. **Out of Memory (OOM)**:
   - Container haben oft begrenzten Speicher. Setzen Sie JVM-Flags wie `-Xmx` und `-Xms`, um die Speichernutzung der Anwendung zu kontrollieren.
   - Beispiel:
   ```bash
   java -Xmx512m -Xms512m -jar myapp.jar
   ```

2. **Langsame Startzeiten oder hohe CPU-Auslastung**:
   - Überprüfen Sie die Garbage Collection und verwenden Sie, falls nötig, angepasste GC-Flags wie `-XX:+UseG1GC` für eine bessere Performance in containerisierten Umgebungen.
   - Wenn die Anwendung eine hohe CPU-Last verursacht, stellen Sie sicher, dass Sie den Container mit einer CPU-Beschränkung starten:
   ```bash
   docker run --cpus="1.5" my-java-container
   ```

3. **Netzwerkprobleme**:
   - Wenn die Anwendung nicht auf externe Dienste zugreifen kann, überprüfen Sie die Netzwerkeinstellungen des Containers.
   - Stellen Sie sicher, dass der Container über das richtige Netzwerk konfiguriert ist und DNS korrekt aufgelöst wird. Sie können den Befehl `docker network ls` verwenden, um die Netzwerke zu überprüfen.

4. **Dateisystemprobleme**:
   - Wenn die Anwendung Daten schreiben muss, stellen Sie sicher, dass das Dateisystem ordnungsgemäß gemountet ist und die Berechtigungen stimmen. Sie können Volumes verwenden, um Daten persistent zu speichern:
   ```bash
   docker run -v /host/path:/container/path my-java-container
   ```