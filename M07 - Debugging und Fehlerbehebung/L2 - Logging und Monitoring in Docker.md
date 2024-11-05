
# Docker für Java Schulung: Logging und Monitoring in Docker

Diese Schulung behandelt die wichtigsten Grundlagen und Werkzeuge für Logging und Monitoring von Java-Anwendungen in Docker-Containern. Der Fokus liegt auf zentraler Log-Verwaltung und der Überwachung von Ressourcen, um die Performance und Stabilität der Anwendungen sicherzustellen.

## Grundlagen von Container-Logging und zentrale Log-Verwaltung

In Docker-Containern werden Logs in der Regel über das Standard-Output (`stdout`) und Standard-Error (`stderr`) ausgegeben. Docker bietet verschiedene Logging-Driver, um Logs zu speichern oder an externe Systeme zu senden.

### 1. Container-Logging
- **Standard-Output und Standard-Error**: Java-Anwendungen loggen typischerweise an die Konsole. Docker erfasst diese Ausgaben und speichert sie.
- **Logging-Treiber**: Docker unterstützt verschiedene Logging-Treiber, wie z.B. `json-file` (Standard), `syslog` und `fluentd`. Diese Treiber bestimmen, wie und wohin Logs gesendet werden.

Beispiel für das Festlegen eines Logging-Treibers:
```bash
docker run --log-driver=syslog my-java-app
```

### 2. Zentrale Log-Verwaltung
Für große, verteilte Systeme ist eine zentrale Verwaltung der Logs unerlässlich. Tools wie **ELK (Elasticsearch, Logstash, Kibana)** und **Graylog** sind dafür sehr beliebt:
- **Elasticsearch** speichert Logs und bietet Such- und Analysefunktionen.
- **Logstash** verarbeitet und leitet Logs weiter.
- **Kibana** bietet eine grafische Oberfläche zur Visualisierung und Analyse der Logs.

### Beispiel-Setup für zentrales Logging
Eine typische Docker-Compose-Konfiguration für ELK könnte wie folgt aussehen:
```yaml
version: '3'
services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.9.2
    environment:
      - discovery.type=single-node

  logstash:
    image: docker.elastic.co/logstash/logstash:7.9.2
    ports:
      - "5044:5044"

  kibana:
    image: docker.elastic.co/kibana/kibana:7.9.2
    ports:
      - "5601:5601"
```

## Monitoring von Java-Anwendungen in Containern

Java-Anwendungen in Containern zu überwachen, erfordert spezielle Werkzeuge, die Metriken zur Performance und Stabilität erfassen.

### 1. Java Virtual Machine (JVM) Monitoring
Da die JVM eine zusätzliche Abstraktionsebene über das Betriebssystem legt, sollten folgende Aspekte überwacht werden:
- **Heap-Nutzung**: Überwachung des Speicherverbrauchs.
- **Garbage Collection**: Überwachung der Häufigkeit und Dauer von GC-Zyklen.
- **Thread-Pools**: Überwachung der aktiven Threads und deren Auslastung.

### 2. Werkzeuge für JVM Monitoring
- **Prometheus & Grafana**: Mit dem Java-Agenten `jmx_exporter` lassen sich JVM-Metriken an Prometheus senden und in Grafana visualisieren.
- **JConsole und VisualVM**: Diese Tools bieten Echtzeitüberwachung und können direkt mit einer JVM verbunden werden.

### Beispiel für Prometheus-Monitoring
Fügen Sie `jmx_exporter` zur Java-Anwendung hinzu und konfigurieren Sie Prometheus wie folgt:
```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'java-app'
    static_configs:
      - targets: ['localhost:12345'] # Port des jmx_exporter
```

## Tools und Techniken für die Überwachung von Containerressourcen

Die Überwachung der Containerressourcen ist entscheidend, um sicherzustellen, dass die Container effizient arbeiten und die Anwendung stabil bleibt.

### 1. Docker-eigene Monitoring-Funktionen
- **docker stats**: Bietet eine Übersicht über die Ressourcen der laufenden Container (CPU, Speicher, Netzwerk).
  ```bash
  docker stats
  ```

### 2. Tools für erweitertes Monitoring
- **cAdvisor**: Bietet detaillierte Metriken zu Containerressourcen wie CPU, Speicher, Netzwerk und Disk I/O.
- **Prometheus & Grafana**: Kann auch zur Überwachung der Ressourcen von Containern verwendet werden und ermöglicht eine zentrale Visualisierung.

### Beispiel für cAdvisor
Erstellen Sie einen cAdvisor-Container und binden Sie die Docker-Socket:
```bash
docker run -d --name=cadvisor -p 8080:8080   --volume=/:/rootfs:ro   --volume=/var/run/docker.sock:/var/run/docker.sock:ro   google/cadvisor:latest
```