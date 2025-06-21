# Installation von Mediawiki

Wie im Hauptkapitel erwähnt, benötigen wir für das manuelle Deployment mindestens die nachfolgenden Files:

| Ressource             | Dateiname               | Zweck                            |
| --------------------- | ----------------------- | -------------------------------- |
| ConfigMap             | configmap.yaml          | Konfigurationswerte (öffentlich) |
| Secret                | secret.yaml             | Geheime Variablen (Passwörter)   |
| PersistentVolumeClaim | pvc.yaml                | Speicher für Daten               |
| Deployment            | deployment.yaml         | Startet die App                  |
| Service               | service.yaml            | Macht sie im Cluster erreichbar  |

Gemäss der vorgegebenen Reihenfolge muss es also so ausshene:

```powershell
kubectl create namespace unvt-mediawiki
kubectl apply -f configs/configmap.yaml
kubectl apply -f configs/secret.yaml
kubectl apply -f configs/pvc.yaml
kubectl apply -f configs/deployment.yaml
kubectl apply -f configs/service.yaml
```

## Installation prüfen Teil 1:

```powershell
kubectl get pods -n unvt-mediawiki
```