# Installation von Prometheus

Wie im Hauptkapitel erwähnt, benötigen wir für das manuelle Deployment mindestens die nachfolgenden Files:

| Ressource             | Dateiname               | Zweck                            |
| --------------------- | ----------------------- | -------------------------------- |
| ConfigMap             | configmap.yaml          | Konfigurationswerte (öffentlich) |
| Secret                | secret.yaml             | Geheime Variablen (Passwörter)   |
| PersistentVolumeClaim | pvc.yaml                | Speicher für Daten               |
| Deployment            | deployment.yaml         | Startet die App                  |
| Service               | service.yaml            | Macht sie im Cluster erreichbar  |

Prometheus kann standardmässig nur Services überwachen, die eigene Metriken liefern. Zum Beispiel Grafana macht das. Damit wir aber auch eigene Services überwachen können, benötigen wir ein Addon für Prometheus: `Blackbox Exporter`.  Dies funktioniert ganz einfach mit einem eigenen Deployment-File: `blackbox-exporter-deployment.yaml`. Das muss logischerweise nach der Installation von prometheus installiert werden. In der `configmap.yaml` kann man aber trotzdem schon die einzelnen Services eintragen.

Gemäss der vorgegebenen Reihenfolge muss es also so aussehen:

```powershell
kubectl create namespace unvt-prometheus
kubectl apply -f ../prometheus/configs/configmap.yaml
kubectl apply -f ../prometheus/configs/secret.yaml
kubectl apply -f ../prometheus/configs/pvc.yaml
kubectl apply -f ../prometheus/configs/deployment.yaml
kubectl apply -f ../prometheus/configs/service.yaml
kubectl apply -f ../prometheus/configs/blackbox-exporter-deployment.yaml
```

## Installation prüfen Teil 1:

```powershell
kubectl get pods -n unvt-prometheus
```

Die Ausgabe zeigt die laufenden Pods. Es kann einige Minuten dauern, bis der Pod den Status `Running` und `Ready` erreicht. Währenddessen kann der Befehl mehrfach ausgeführt werden. In dem Fall muss die Ausgabe einen Pod ergeben, die beide mit dem Prefix `unvt-prometheus-` beginnen.

## Installation prüfen Teil 2:
```powershell
minikube service prometheus-service -n unvt-prometheus
```

Das sollte den Browser öffnen und direkt Prometheus zeigen.

## Installation prüfen Teil 3:

## Info
Mit dem Befehl `up` kann man den Status aller Services sehen, die überwacht werden. Ganz am Ende der Zeile erscheint eine **1** oder **0** für online bzw. offline. Wenn man also einen POD löscht oder neustartet, sieht man das dort. Mit dem Befehl `probe_success{job="wordpress-http"}` kann man z.B. einen bestimmten Service checken.