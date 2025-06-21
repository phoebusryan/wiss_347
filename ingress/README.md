# Installation von Ingress

## Installation von Helm

Wir benutzen nun **Helm**. Helm ist ein Paketmanager, der die Verwaltung, Installation und Aktualisierung von Kubernetes-Anwendungen vereinfacht. Helm muss nicht klassisch installiert werden, sondern wird lediglich heruntergeladen und eingebunden. Die passende Version für Windows (amd64) kann hier heruntergeladen werden:  [https://github.com/helm/helm/releases](https://github.com/helm/helm/releases)

Nach dem Download das ZIP-Archiv entpacken, ein Terminal (CMD oder PowerShell) öffnen und mit folgendem Befehl prüfen, ob Helm korrekt funktioniert:

```powershell
helm version
```

## Installation von Ingress

Das ist ganz simpel machbar via Helm:
```powershell
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
create namespace ingress-nginx
helm install ingress-nginx ingress-nginx/ingress-nginx --namespace ingress-nginx
```
Wenn man Ingress installiert, erhält man ein Beispiel-Yaml für die Nutzung und genau auf das setzen wir später.

### Prüfen ob der Ingress läuft
```powershell
kubectl get pods -n ingress-nginx
```

Nun konfigurieren wir ingress selbst
```powershell
kubectl apply -f configs/ingress-class.yaml
```

Nun wird für jede Applikation (Wordpress, Jira, Mediawiki) eine eigene yaml-Datei angelegt. Der Inhalt ist derjenige aus dem Setupscript gerade eben. Schlussendlich passen wir in der Datei genau **vier** Werte an. Beispiel für Wordpress. Die anderen beiden analog anpassen.
- metadata.name: wordpress-ingress
- metadata.namespace: unvt-wordpress
- spec.rules.host: wordpress.local
- spec.rules.http.paths.backend.service.name: wordpress-service

Nun lesen wir die angelegten Dateien ein
```powershell
kubectl apply -f configs/wordpress-ingress.yaml
kubectl apply -f configs/jira-ingress.yaml
kubectl apply -f configs/mediawiki-ingress.yaml
```

### IP Adresse des lokalen Clusters rausfinden:
```powershell
minikube ip
```
Das ergibt sowas wie `192.168.49.2`

### Hosts-Datei anpassen

Am besten das ganz normale `notepad.exe` mit Administratorrechten öffnen und dann folgende Datei öffnen:
`C:\Windows\System32\drivers\etc\hosts` und folgenden Inhalt ganz unten einfügen:
```powershell
192.168.49.2 wordpress.local
192.168.49.2 jira.local
192.168.49.2 mediawiki.local
```

minikube tunnel