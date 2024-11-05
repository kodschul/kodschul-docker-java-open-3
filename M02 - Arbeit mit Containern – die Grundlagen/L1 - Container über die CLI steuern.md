
# Docker für Java Schulung

## Container über die CLI steuern

Das Docker Command Line Interface (CLI) bietet leistungsstarke Befehle, um Container zu verwalten. Für die Arbeit mit Java-Anwendungen in Docker ist es wichtig, grundlegende CLI-Befehle zu kennen, um Container effizient starten, stoppen und löschen zu können.

### Einführung ins Command Line Interface (CLI) für Docker

Die Docker CLI ermöglicht es, mit Docker Daemons zu kommunizieren, um Images, Container, Netzwerke und Volumes zu verwalten. Docker-Befehle beginnen mit dem Befehl `docker`, gefolgt von Unterbefehlen und optionalen Argumenten. Einige grundlegende Befehle, die für den Start mit Docker nützlich sind:

- **docker --version**: Zeigt die installierte Docker-Version an.
- **docker info**: Zeigt allgemeine Informationen über die Docker-Umgebung und Konfiguration an.
- **docker help**: Listet alle verfügbaren Docker-Befehle auf und gibt Hinweise zu deren Verwendung.

### Wichtige Docker-Befehle zum Starten, Stoppen und Löschen von Containern

Docker bietet verschiedene CLI-Befehle zur Verwaltung von Containern. Hier sind die grundlegenden Befehle, die Sie kennen sollten:

#### 1. Einen Container starten
Um einen neuen Container aus einem Image zu erstellen und zu starten, verwenden Sie `docker run`. Wenn das Image nicht lokal vorhanden ist, wird es aus Docker Hub heruntergeladen.

```bash
# Container starten
docker run -d --name mein-container -p 8080:8080 openjdk:11-jdk-slim java -jar meine-java-app.jar
```

- **-d**: Startet den Container im Hintergrund (detached mode).
- **--name**: Gibt dem Container einen Namen, um ihn einfacher zu verwalten.
- **-p**: Verknüpft Ports vom Container mit dem Host.

#### 2. Container stoppen
Der Befehl `docker stop` beendet einen laufenden Container.

```bash
# Container stoppen
docker stop mein-container
```

#### 3. Container neu starten
Um einen gestoppten Container wieder zu starten, nutzen Sie `docker start`.

```bash
# Container neu starten
docker start mein-container
```

#### 4. Liste der Container anzeigen
Um eine Liste aller laufenden Container anzuzeigen, verwenden Sie `docker ps`. Um auch gestoppte Container anzuzeigen, nutzen Sie `docker ps -a`.

```bash
# Laufende Container anzeigen
docker ps

# Alle Container anzeigen, auch gestoppte
docker ps -a
```

#### 5. Einen Container löschen
Um einen Container dauerhaft zu entfernen, verwenden Sie `docker rm`. Der Container muss gestoppt sein, bevor er gelöscht werden kann.

```bash
# Container löschen
docker rm mein-container
```

#### 6. Alle Container stoppen und löschen
Manchmal ist es notwendig, alle Container zu stoppen und zu löschen. Dies kann mit den folgenden Befehlen erreicht werden:

```bash
# Alle Container stoppen
docker stop $(docker ps -q)

# Alle Container löschen
docker rm $(docker ps -a -q)
```

- **$(docker ps -q)** gibt die IDs aller laufenden Container zurück.
- **$(docker ps -a -q)** gibt die IDs aller Container zurück, einschließlich der gestoppten.
