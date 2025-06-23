# Installation von Grafana

Wie im Hauptkapitel erwähnt, benötigen wir für das manuelle Deployment mindestens die nachfolgenden Files:

| Ressource             | Dateiname               | Zweck                            |
| --------------------- | ----------------------- | -------------------------------- |
| ConfigMap             | configmap.yaml          | Konfigurationswerte (öffentlich) |
| Secret                | secret.yaml             | Geheime Variablen (Passwörter)   |
| PersistentVolumeClaim | pvc.yaml                | Speicher für Daten               |
| Deployment            | deployment.yaml         | Startet die App                  |
| Service               | service.yaml            | Macht sie im Cluster erreichbar  |

Gradana benötigt jedoch für die Kommunikation noch eine weitere Datei:

| Ressource             | Dateiname               | Zweck                            |
| --------------------- | ----------------------- | -------------------------------- |
| data provisioning     | data-provisioning.yaml  | Enthält Infos über Prometheus    |

Gemäss der vorgegebenen Reihenfolge muss es also so aussehen:

```powershell
kubectl delete namespace unvt-grafana
kubectl create namespace unvt-grafana
kubectl apply -f ../grafana/configs/configmap.yaml
kubectl apply -f ../grafana/configs/secret.yaml
kubectl apply -f ../grafana/configs/pvc.yaml
kubectl apply -f ../grafana/configs/data-provisioning.yaml
kubectl apply -f ../grafana/configs/deployment.yaml
kubectl apply -f ../grafana/configs/service.yaml
```

## Installation prüfen Teil 1:

```powershell
kubectl get pods -n unvt-grafana
```

Die Ausgabe zeigt die laufenden Pods. Es kann einige Minuten dauern, bis der Pod den Status `Running` und `Ready` erreicht. Währenddessen kann der Befehl mehrfach ausgeführt werden. In dem Fall muss die Ausgabe einen Pod ergeben, die beide mit dem Prefix `unvt-grafana-` beginnen.

## Installation prüfen Teil 2:
```powershell
minikube service grafana-service -n unvt-grafana
```

Das sollte den Browser öffnen und direkt grafana zeigen.

## Installation prüfen Teil 3:

Im Browser ging ein Fenster auf mit einer URL. Vermutlich etwas wie `http://127.0.0.1:54087/`. Da muss man sich nun einloggen, sodass man ins Backend kommt. Die Zugangsdaten stehen in der Datei `secret.yaml`. 