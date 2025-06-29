# Installation von Ingress

## Installation von Helm

Wir benutzen nun **Helm**. Helm ist ein Paketmanager, der die Verwaltung, Installation und Aktualisierung von Kubernetes-Anwendungen vereinfacht. Helm muss nicht klassisch installiert werden, sondern wird lediglich heruntergeladen und eingebunden. Die passende Version für Windows (amd64) ist in diesem Repository im Ordner `software` zu finden. Ob es funktioniert, kann man ganz einfach mit einem Terminal rausfinden:

```powershell
helm version
```

## Installation von Ingress

Das ist ganz simpel machbar via Helm:
```powershell
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
kubectl create namespace ingress-nginx
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
kubectl apply -f ../ingress/configs/wordpress-ingress.yaml
kubectl apply -f ../ingress/configs/jira-ingress.yaml
kubectl apply -f ../ingress/configs/mediawiki-ingress.yaml
kubectl apply -f ../ingress/configs/prometheus-ingress.yaml
kubectl apply -f ../ingress/configs/grafana-ingress.yaml
```

### Hosts-Datei anpassen

Am besten das ganz normale `notepad.exe` mit Administratorrechten öffnen und dann folgende Datei öffnen:
`C:\Windows\System32\drivers\etc\hosts` und folgenden Inhalt ganz unten einfügen:
```powershell
127.0.0.1 wordpress.local
127.0.0.1 jira.local
127.0.0.1 mediawiki.local
127.0.0.1 prometheus.local
127.0.0.1 grafana.local
```

### Ingress starten
```powershell
minikube tunnel
```

### Testen
Nun sollte man bequem im Browser folgende URLs aufrufen können:
- http://wordpress.local
- http://jira.local
- http://mediawiki.local
- http://prometheus.local
- http://grafana.local

Damit das funktioniert, muss `minikube tunnel` die ganze Zeit laufen. Der Tunnel sorgt dafür, dass Aufrufe wie http://wordpress.local vom eigenen Rechner an den richtigen Ort im Cluster weitergeleitet werden.