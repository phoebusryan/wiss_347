# Installation von Wordpress

Wie im Hauptkapitel erwähnt, benötigen wir für das manuelle Deployment mindestens die nachfolgenden Files:

| Ressource             | Dateiname               | Zweck                            |
| --------------------- | ----------------------- | -------------------------------- |
| ConfigMap             | configmap.yaml          | Konfigurationswerte (öffentlich) |
| Secret                | secret.yaml             | Geheime Variablen (Passwörter)   |
| PersistentVolumeClaim | pvc.yaml                | Speicher für Daten               |
| Deployment            | deployment.yaml         | Startet die App                  |
| Service               | service.yaml            | Macht sie im Cluster erreichbar  |

Wordpress benötigt jedoch noch ein paar weitere Dateien, weil es eine `MariaDB` benötigt:

| Ressource             | Dateiname               | Zweck                            |
| --------------------- | ----------------------- | -------------------------------- |
| mariadb deployment    | mariadb-deployment.yaml | Startet die App                  |
| mariadb-service       | mariadb-service.yaml    | Macht sie im Cluster erreichbar  |
| mariadb-pvc.yaml      | mariadb-pvc.yaml        | Speicher für Daten               |

Gemäss der vorgegebenen Reihenfolge muss es also so aussehen:

```powershell
kubectl create namespace unvt-wordpress
kubectl apply -f ../wordpress/configs/configmap.yaml
kubectl apply -f ../wordpress/configs/secret.yaml
kubectl apply -f ../wordpress/configs/mariadb-pvc.yaml
kubectl apply -f ../wordpress/configs/pvc.yaml
kubectl apply -f ../wordpress/configs/mariadb-deployment.yaml
kubectl apply -f ../wordpress/configs/deployment.yaml
kubectl apply -f ../wordpress/configs/mariadb-service.yaml
kubectl apply -f ../wordpress/configs/service.yaml
```

## Installation prüfen Teil 1:

```powershell
kubectl get pods -n unvt-wordpress
```

Die Ausgabe zeigt die laufenden Pods. Es kann einige Minuten dauern, bis beide Pods den Status `Running` und `Ready` erreichen. Währenddessen kann der Befehl mehrfach ausgeführt werden. In dem Fall muss die Ausgabe zwei Pods ergeben, die beide mit dem Prefix `unvt-wordpress-` beginnen.

## Installation prüfen Teil 2:
```powershell
minikube service wordpress-service -n unvt-wordpress
```

Das sollte den Browser öffnen und direkt Wordpress zeigen.

## Installation prüfen Teil 3:
Im Browser ging ein Fenster auf mit einer URL. Vermutlich etwas wie `http://127.0.0.1:54087/`. An diese URL muss nun ein /wp-admin angegehängt werden, sodass man zum Backend-Login kommt. Die Zugangsdaten stehen in der Datei `secret.yaml`. Damit einloggen und etwas im Backend verändern, was man im Frontend sieht.

Nun kann man die pods löschen:
```powershell
kubectl delete pod -l app=wordpress -n unvt-wordpress
kubectl delete pod -l app=mariadb -n unvt-wordpress
```

Nun müssen wir warten bis die Pods wieder da sind:
```powershell
kubectl get pods -n unvt-wordpress
```

Dann im Browser wieder aufrufen und es dürfte sich nichts verändert haben.