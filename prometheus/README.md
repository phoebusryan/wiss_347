# Installation von Prometheus

Wie im Hauptkapitel erwähnt, benötigen wir für das manuelle Deployment mindestens die nachfolgenden Files:

| Ressource             | Dateiname               | Zweck                            |
| --------------------- | ----------------------- | -------------------------------- |
| ConfigMap             | configmap.yaml          | Konfigurationswerte (öffentlich) |
| Secret                | secret.yaml             | Geheime Variablen (Passwörter)   |
| PersistentVolumeClaim | pvc.yaml                | Speicher für Daten               |
| Deployment            | deployment.yaml         | Startet die App                  |
| Service               | service.yaml            | Macht sie im Cluster erreichbar  |

Prometheus benötigt aber weder persistenzen Datenspeicher, noch lässt es sich gross konfigurieren. Wir benötigen somit nur die Dateien `secret.yaml`, `deployment.yaml` und `service.yaml`


```powershell
kubectl create namespace unvt-prometheus
kubectl apply -f ../prometheus/configs/configmap.yaml
kubectl apply -f ../prometheus/configs/secret.yaml
kubectl apply -f ../prometheus/configs/deployment.yaml
kubectl apply -f ../prometheus/configs/service.yaml
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