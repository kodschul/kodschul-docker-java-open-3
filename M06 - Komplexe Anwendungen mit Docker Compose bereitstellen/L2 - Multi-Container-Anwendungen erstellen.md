
# Docker für Java Schulung: Multi-Container-Anwendungen erstellen

## Container-basierte Anwendungen mit mehreren Services erstellen

Container-basierte Anwendungen ermöglichen es, verschiedene Services in isolierten Umgebungen zu betreiben. Dies ist besonders hilfreich, wenn Sie beispielsweise eine Java-Anwendung entwickeln, die aus verschiedenen Komponenten besteht, wie z.B. einer Datenbank, einem Webserver und einem Anwendungsdienst.

### Beispiel: Java Anwendung mit Datenbank und Webserver

Ein Beispiel für eine Multi-Container-Anwendung könnte eine Java-Anwendung sein, die einen Spring Boot Service mit einer PostgreSQL-Datenbank und einem Nginx-Webserver kombiniert. Diese Struktur sorgt für eine klare Trennung der Komponenten und erleichtert die Verwaltung.

### Dockerfile für Java-Service erstellen
Erstellen Sie zunächst ein Dockerfile für Ihren Java-Service:
```dockerfile
# Dockerfile für Java-Service
FROM openjdk:11-jre-slim
COPY target/myapp.jar /app/myapp.jar
ENTRYPOINT ["java", "-jar", "/app/myapp.jar"]
```

### Dockerfile für Nginx
Falls ein Nginx-Webserver benötigt wird, kann ein separates Dockerfile für den Nginx-Container erstellt werden:
```dockerfile
# Dockerfile für Nginx
FROM nginx:alpine
COPY nginx.conf /etc/nginx/nginx.conf
```

### Docker Compose Datei für Multi-Container-Anwendung
Die `docker-compose.yml` Datei verwaltet die verschiedenen Container und deren Konfiguration. Ein Beispiel für eine Docker Compose Datei für eine Multi-Container-Anwendung könnte so aussehen:
```yaml
version: '3'
services:
  web:
    build: ./nginx
    ports:
      - "80:80"
    depends_on:
      - app

  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      - SPRING_DATASOURCE_URL=jdbc:postgresql://db:5432/mydatabase
    depends_on:
      - db

  db:
    image: postgres:13
    environment:
      - POSTGRES_DB=mydatabase
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:
```

Mit diesem Setup können Sie alle Services über einen einzigen Befehl starten:
```bash
docker-compose up
```

## Microservices-Architektur mit Docker Compose verwalten

Docker Compose ist besonders hilfreich bei der Verwaltung von Microservices-Architekturen, da es die Orchestrierung und Skalierung mehrerer Container erleichtert. Durch die Konfiguration in der `docker-compose.yml` Datei können Sie sicherstellen, dass alle Microservices konsistent starten und miteinander kommunizieren.

### Skalieren von Microservices
Docker Compose ermöglicht die einfache Skalierung von Services, z.B. um die Anzahl der Instanzen eines Services zu erhöhen:
```bash
docker-compose up --scale app=3
```
In diesem Fall würde der `app`-Service auf drei Instanzen skaliert, was besonders bei Lastspitzen nützlich ist.

### Netzwerkverwaltung zwischen Services
Docker Compose stellt automatisch ein Netzwerk für alle definierten Services bereit, sodass diese einfach untereinander kommunizieren können. Die `depends_on` Direktive in der `docker-compose.yml` Datei stellt sicher, dass Services in der richtigen Reihenfolge gestartet werden.

### Beispiel einer erweiterten Microservices-Architektur
In einer Microservices-Architektur könnten Services wie Authentifizierung, Benachrichtigungen und Datenverarbeitung ebenfalls als eigenständige Container bereitgestellt werden. Diese könnten dann über die Docker Compose Datei orchestriert werden, um eine flexible und skalierbare Anwendung zu schaffen.

```yaml
version: '3'
services:
  auth-service:
    image: auth-service:latest
    ports:
      - "5000:5000"

  notification-service:
    image: notification-service:latest
    ports:
      - "5001:5001"

  data-service:
    image: data-service:latest
    ports:
      - "5002:5002"
    depends_on:
      - db

  db:
    image: postgres:13
    environment:
      - POSTGRES_DB=mydatabase
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
    volumes:
      - db_data:/var/lib/postgresql/data

volumes:
  db_data:
```

### Verwaltung und Überwachung
Verwenden Sie zusätzliche Tools wie Prometheus und Grafana in Ihrer Docker Compose Konfiguration, um die Überwachung und Protokollierung der Microservices zu verbessern.
