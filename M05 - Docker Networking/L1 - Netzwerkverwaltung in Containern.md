
# Docker für Java Schulung: Netzwerkverwaltung in Containern

Diese Schulung bietet einen praktischen Überblick über die Netzwerkverwaltung in Docker-Containern mit Fokus auf die Verbindung und Kommunikation zwischen Containern.

## Netzwerke zwischen Containern erstellen und verwalten

Docker ermöglicht die Erstellung und Verwaltung von Netzwerken, um Container sicher und effizient miteinander zu verbinden. Container können auf verschiedene Weise in Netzwerke eingebunden werden, was eine flexible Kommunikation zwischen ihnen ermöglicht.

### 1. Erstellen eines Docker-Netzwerks

Mit Docker lassen sich Netzwerke schnell und einfach erstellen. Ein benanntes Netzwerk kann wie folgt erstellt werden:
```bash
docker network create mein-netzwerk
```
Dieses Netzwerk ermöglicht es Containern, die sich darin befinden, untereinander zu kommunizieren, während sie von Containern in anderen Netzwerken isoliert sind.

### 2. Container mit einem Netzwerk verbinden

Container können beim Start einem Netzwerk zugewiesen werden:
```bash
docker run -d --name container1 --network mein-netzwerk alpine
docker run -d --name container2 --network mein-netzwerk alpine
```
In diesem Beispiel befinden sich beide Container im gleichen Netzwerk und können daher miteinander kommunizieren.

### 3. Überprüfung der Netzwerkkonfiguration

Um zu überprüfen, ob ein Netzwerk korrekt erstellt und Container damit verbunden sind, kann folgender Befehl genutzt werden:
```bash
docker network inspect mein-netzwerk
```
Dies zeigt Details zum Netzwerk und den verbundenen Containern.

## Container über Netzwerke miteinander kommunizieren lassen

Sobald Container in einem Netzwerk verbunden sind, können sie über ihre Container-Namen aufeinander zugreifen.

### 1. Kommunikation zwischen Containern

In einem gemeinsamen Netzwerk können Container über ihre Namen miteinander kommunizieren. Beispiel:
```bash
docker exec -it container1 ping container2
```
Dieser Befehl sendet Ping-Anfragen von `container1` an `container2`, um die Erreichbarkeit zu testen.

### 2. Java-Anwendung in Containern verbinden

Für Java-Anwendungen, die in Docker-Containern laufen, ist die Netzwerkkommunikation über den Netzwerk-Namen ebenso möglich. Beispielsweise könnte ein Java-Anwendungscontainer eine Datenbank in einem anderen Container über deren Namen erreichen:
```java
// Beispiel für die Verbindung zur Datenbank in einem Container namens "db"
String dbUrl = "jdbc:mysql://db:3306/meineDatenbank";
Connection conn = DriverManager.getConnection(dbUrl, "benutzer", "passwort");
```
Hierbei ist `db` der Name des Containers, in dem die Datenbank läuft, und die Verbindung erfolgt über das Docker-Netzwerk.

### 3. Beispiel einer vernetzten Docker-Compose-Datei

Docker Compose vereinfacht die Verwaltung mehrerer Container und Netzwerke. Ein Beispiel für eine Docker-Compose-Datei mit einem gemeinsamen Netzwerk:
```yaml
version: '3'
services:
  app:
    image: openjdk:11
    container_name: java_app
    networks:
      - mein-netzwerk
  db:
    image: mysql:5.7
    container_name: db
    environment:
      MYSQL_ROOT_PASSWORD: passwort
    networks:
      - mein-netzwerk

networks:
  mein-netzwerk:
```
In diesem Setup können sich die `app` und `db` Container gegenseitig über ihre Container-Namen erreichen, da sie sich im `mein-netzwerk` befinden.
