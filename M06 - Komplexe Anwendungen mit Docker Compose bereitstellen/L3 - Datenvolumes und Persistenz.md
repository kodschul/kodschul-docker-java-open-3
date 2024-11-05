
# Docker für Java Schulung: Datenvolumes und Persistenz

## Volumes erstellen und persistente Daten speichern

In Docker werden Volumes verwendet, um Daten über Container-Neustarts hinaus zu speichern. Dies ist besonders nützlich für Java-Anwendungen, die auf Datenbanken oder anderen persistierenden Speicherorten angewiesen sind.

### 1. Was sind Docker-Volumes?
Docker-Volumes sind ein Mechanismus, um Daten außerhalb des Container-Filesystems zu speichern. Sie ermöglichen den Zugang zu persistenten Daten, unabhängig davon, ob der Container gelöscht oder neugestartet wird.

### 2. Erstellen eines Volumes
Volumes können entweder manuell oder direkt beim Start eines Containers erstellt werden.

#### Manuelles Erstellen eines Volumes
```bash
docker volume create mein_volume
```

#### Volume einem Container zuweisen
Um ein Volume zu nutzen und einem Container zuzuweisen:
```bash
docker run -d -v mein_volume:/pfad/im/container --name java_app_container openjdk:latest
```
Hiermit wird das Volume `mein_volume` an den Pfad `/pfad/im/container` im Container gemountet. Daten, die hier gespeichert werden, bleiben auch dann erhalten, wenn der Container gelöscht wird.

### 3. Verwendung von Bind Mounts
Neben Volumes können auch Bind Mounts verwendet werden, um bestimmte Verzeichnisse auf dem Hostsystem in den Container zu mounten:
```bash
docker run -d -v /pfad/auf/host:/pfad/im/container --name java_app_container openjdk:latest
```
Bind Mounts sind hilfreich, wenn Sie direkt auf Dateien des Hostsystems zugreifen möchten. Allerdings sollten sie mit Bedacht verwendet werden, da unkontrollierter Zugriff auf Host-Dateien das System destabilisieren kann.

## Best Practices für die Verwaltung von Volumes

Um die Verwendung von Volumes effizient zu gestalten, sind folgende Best Practices hilfreich:

### 1. Klare Namensgebung und Dokumentation
- Verwenden Sie beschreibende Namen für Volumes, damit deren Zweck und Inhalt nachvollziehbar sind.
- Dokumentieren Sie in Ihrem Projekt, welche Volumes für welche Daten genutzt werden.

### 2. Vermeiden unnötiger Volumes
- Erstellen Sie nur dann Volumes, wenn Daten tatsächlich persistiert werden müssen.
- Temporäre Daten oder Logs sollten nach Möglichkeit nicht in Volumes gespeichert werden.

### 3. Regelmäßige Bereinigung nicht benötigter Volumes
Überflüssige Volumes können Speicherplatz belegen und sollten regelmäßig bereinigt werden:
```bash
docker volume prune
```
Dieser Befehl entfernt alle nicht genutzten Volumes.

### 4. Backup und Wiederherstellung von Volumes
Um die Datenintegrität zu gewährleisten, können Sie regelmäßige Backups von Volumes erstellen. Dazu kann das Volume in ein Archiv auf dem Hostsystem kopiert werden:
```bash
docker run --rm -v mein_volume:/volume_data -v $(pwd):/backup alpine tar -cvf /backup/volume_backup.tar /volume_data
```

Das Wiederherstellen eines Backups kann durch den umgekehrten Prozess erfolgen:
```bash
docker run --rm -v mein_volume:/volume_data -v $(pwd):/backup alpine tar -xvf /backup/volume_backup.tar -C /volume_data
```