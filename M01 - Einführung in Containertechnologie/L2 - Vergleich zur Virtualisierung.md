
# Docker für Java Schulung

## Technische Grundlagen und Vergleich zur Virtualisierung

Docker ist eine Containerisierungsplattform, die Anwendungen in isolierten Containern ausführt. Im Gegensatz zu traditionellen Virtualisierungstechnologien, die für jede VM ein eigenes Betriebssystem benötigen, verwenden Container gemeinsam den Kernel des Host-Betriebssystems und isolieren nur die Applikation und ihre Abhängigkeiten. Dies macht Docker-Container effizienter und ressourcenschonender.

### Unterschiede zwischen Containern und virtuellen Maschinen

| Eigenschaft                 | Container                                    | Virtuelle Maschine                       |
|-----------------------------|----------------------------------------------|------------------------------------------|
| Betriebssystem              | Teilt den Kernel mit dem Host                | Enthält ein vollständiges Gast-Betriebssystem |
| Speicherbedarf              | Leichtgewichtig, MB-Größe                    | Schwergewichtig, oft GB-Größe            |
| Startzeit                   | Sehr schnell, Sekunden                       | Langsam, Minuten                         |
| Isolationsgrad              | Prozess-Isolation                            | Stärkere Isolation durch Hypervisor      |
| Performance                 | Näher an der Host-Performance                | Höherer Overhead                         |

Container eignen sich besonders für Microservices und Anwendungen, die schnell bereitgestellt und skaliert werden müssen. Virtuelle Maschinen hingegen sind besser für Anwendungen geeignet, die eine vollständige Isolation und ein dediziertes Betriebssystem benötigen.

## Container-Layering und Speicher-Management

Docker verwendet ein Layered Filesystem, um Container effizient zu erstellen und zu verwalten. Jeder Container besteht aus verschiedenen Schichten (Layers), die übereinander gestapelt sind. Dieses "Layering" ermöglicht eine sparsame Ressourcennutzung und schnelle Bereitstellungen, da gemeinsame Basis-Layers von mehreren Containern geteilt werden können.

### Aufbau und Nutzen des Layering-Systems

1. **Image Layers**: Jedes Docker-Image besteht aus mehreren Schichten, die verschiedene Aspekte der Anwendung und ihrer Abhängigkeiten definieren. Beispiel:
   - Basis-Image (z.B. Ubuntu)
   - Java-Laufzeitumgebung
   - Anwendungscode
   - Konfigurationen und spezifische Abhängigkeiten

2. **Copy-on-Write (COW)**: Änderungen, die ein Container an einer Datei vornimmt, werden nicht im Basis-Layer gespeichert, sondern als neue Schicht über den vorhandenen Schichten angelegt. Dies spart Speicher und macht das System effizient.

### Speicher-Management in Docker

Docker ermöglicht das effiziente Management von Speicherressourcen durch:
- **Volumenspeicher**: Volumes sind persistente Speicherorte außerhalb des Containers. Sie ermöglichen es, Daten sicher und unabhängig vom Lebenszyklus des Containers zu speichern.
  - Beispiel für das Anlegen eines Volumes:
    ```bash
    docker volume create mein_volume
    ```
- **Bind Mounts**: Dies erlaubt es, Verzeichnisse auf dem Host direkt in den Container einzubinden, was besonders bei Entwicklungs-Workflows praktisch ist.
  - Beispiel:
    ```bash
    docker run -v /host/path:/container/path image_name
    ```

Durch diese Mechanismen ermöglicht Docker eine flexible und effiziente Speicherverwaltung, die insbesondere in produktiven Umgebungen und bei Anwendungen, die auf Datenpersistenz angewiesen sind, von Vorteil ist.