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

### Installation von kubectl

**kubectl** muss nicht installiert, sondern lediglich heruntergeladen und eingebunden werden. Die ausführbare Datei `kubectl.exe` kann unter folgendem Link bezogen werden:  
[https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/)

Erstelle einen Ordner namens `C:\kubetools` und speichere die heruntergeladene Datei darin.

### Hinzufügen zur Umgebungsvariablen (Path)

Damit `kubectl` bequem im Terminal aufgerufen werden kann, muss der Pfad zum Ordner `C:\kubetools` in die **Windows-Umgebungsvariablen** aufgenommen werden:

1. Öffne das Startmenü und suche nach **„Umgebungsvariablen für dieses Konto bearbeiten“**.
2. Im oberen Bereich („Benutzervariablen“) den Eintrag **„Path“** auswählen und auf **„Bearbeiten“** klicken.
3. Im neuen Fenster auf **„Neu“** klicken, dann auf **„Durchsuchen“** und den Ordner `C:\kubetools` auswählen.
4. Änderungen mit **OK** bestätigen und den Computer **neu starten**.

Nach dem Neustart ein Terminal (CMD oder PowerShell) öffnen und folgenden Befehl testen:

```powershell
kubectl version --client
```

### Installation von Helm

Auch **Helm** muss nicht klassisch installiert werden, sondern wird lediglich heruntergeladen und eingebunden. Die passende Version für Windows (amd64) kann hier heruntergeladen werden:  [https://github.com/helm/helm/releases](https://github.com/helm/helm/releases)

Nach dem Download das ZIP-Archiv entpacken und die Datei `helm.exe` in den Ordner `C:\kubetools` verschieben – wie bereits bei `kubectl`.

Anschliessend ein Terminal (CMD oder PowerShell) öffnen und mit folgendem Befehl prüfen, ob Helm korrekt funktioniert:

```powershell
helm version
```

### Kubernetes-Cluster starten

Zum Abschluss muss der lokale **Kubernetes-Cluster** über Minikube gestartet werden. Dies geschieht mit folgendem Befehl:

```powershell
minikube start --driver=docker
```
Beim ersten Start kann der Vorgang etwa 5–10 Minuten dauern, da notwendige Komponenten und Images heruntergeladen werden.

Sollte eine Fehlermeldung erscheinen, liegt das in den meisten Fällen daran, dass Docker nicht läuft – in diesem Fall Docker Desktop zuerst starten und es erneut versuchen.

Nach erfolgreichem Start ein neues Terminal (CMD oder PowerShell) öffnen und zur Überprüfung folgenden Befehl ausführen:
```powershell
kubectl get nodes
```
Wenn ein Node mit dem Status Ready angezeigt wird, ist die Basisinstallation abgeschlossen und der Kubernetes-Cluster läuft einsatzbereit.