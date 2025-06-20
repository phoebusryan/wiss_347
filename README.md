# Modul 347 - Dienst mit Container anwenden 

## Einleitung
Für das vorliegende Projekt wird eine containerisierte Umgebung auf Basis von Kubernetes aufgebaut, welche die drei Webanwendungen WordPress, MediaWiki und Jira zuverlässig bereitstellt. Ziel ist es, alle Applikationen inklusive Datenbanken mit Persistenz, Monitoring und Zugriff über einen zentralen Ingress-Reverse-Proxy in einem lokalen Cluster zu betreiben.

## Beispielszenario
Ein mittelständisches Unternehmen plant den Aufbau einer internen IT-Umgebung, die verschiedene Anforderungen zentral abdeckt:

- **WordPress** dient als öffentlich zugängliche **Unternehmenswebsite**, um Dienstleistungen, Neuigkeiten und Karriereseiten zu präsentieren.
- **MediaWiki** wird intern als **Wissensdatenbank** und **Intranet-Lösung** genutzt, um Dokumentationen, Teamwissen und Prozessbeschreibungen für Mitarbeitende bereitzustellen.
- **Jira** kommt als zentrales **Projektmanagement- und Ticketsystem** zum Einsatz, um Aufgaben, Bugs und Support-Anfragen strukturiert zu verwalten.

Die Firma legt grossen Wert auf **Skalierbarkeit**, **Zuverlässigkeit** und **Zugriffskontrolle**, weshalb die Umgebung containerisiert und mit Kubernetes orchestriert wird. Durch den Einsatz eines zentralen Ingress-Controllers können alle Dienste über eine gemeinsame Adresse und klar definierte Pfade bereitgestellt werden.

## Benötigte Technologien und Software

- **Kubernetes** ist eine Open-Source-Plattform zur automatisierten Verwaltung, Skalierung und Bereitstellung von containerisierten Anwendungen.
- **Minikube** ist ein Tool, mit dem man einen lokalen Kubernetes-Cluster auf dem eigenen Rechner starten und testen kann.
- **Docker** ist eine Plattform, mit der Anwendungen in isolierten Containern verpackt, verteilt und ausgeführt werden können.
- **kubectl** ist das Kommandozeilenwerkzeug, mit dem man Kubernetes-Cluster verwaltet und Befehle an den Cluster senden kann.
- **Helm** ist ein Paketmanager für Kubernetes, mit dem Anwendungen samt ihrer Konfiguration einfach installiert und verwaltet werden können.

## Aufsetzen der Basis-Infrastruktur

### Docker installieren
Zunächst wird **Docker Desktop** installiert, inklusive des benötigten **Windows Subsystem for Linux (WSL)**. Dazu die Datei `Docker Desktop Installer.exe` (https://www.docker.com/get-started/) ausführen und den Anweisungen folgen. Nach Abschluss der Installation ist ein **Neustart des Systems erforderlich**.

Beim ersten Start kann eine Aufforderung zur Aktualisierung von WSL erscheinen – diese sollte unbedingt bestätigt werden. Anschliessend Docker starten und sicherstellen, dass der Dienst läuft. Der optional angebotene Docker-Account bzw. Sign-Up-Prozess kann übersprungen werden.

### Minikube installieren
Als nächstes muss die Anwendung **Minikube** installiert werden. Minikube ermöglicht es, einen lokalen Kubernetes-Cluster auszuführen. Der benötigte Installer trägt den Namen `minikube-installer.exe`.

Die aktuelle Version kann auf der offiziellen Website heruntergeladen werden:  
[https://minikube.sigs.k8s.io/docs/start/](https://minikube.sigs.k8s.io/docs/start/)

Nach dem Download einfach die Installationsdatei ausführen und den Anweisungen folgen.

### Kubectl installieren
**Kubectl** muss nicht direkt installiert werden, sondern einfach heruntergeladen und verknüpft werden. Die dafür benötigte Datei `kubectl.exe` findet man unter nachfolgendem Link: https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/

Erstelle einen Ordner C:\kubetools und kopiere die heruntergeladene Datei da rein.