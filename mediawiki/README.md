# Installation von Mediawiki

Wie im Hauptkapitel erwähnt, benötigen wir für das manuelle Deployment mindestens die nachfolgenden Files:

| Ressource             | Dateiname               | Zweck                            |
| --------------------- | ----------------------- | -------------------------------- |
| ConfigMap             | configmap.yaml          | Konfigurationswerte (öffentlich) |
| Secret                | secret.yaml             | Geheime Variablen (Passwörter)   |
| PersistentVolumeClaim | pvc.yaml                | Speicher für Daten               |
| Deployment            | deployment.yaml         | Startet die App                  |
| Service               | service.yaml            | Macht sie im Cluster erreichbar  |

Mediawiki benötigt jedoch noch ein paar weitere Dateien, weil es eine `MariaDB` benötigt:

| Ressource             | Dateiname               | Zweck                            |
| --------------------- | ----------------------- | -------------------------------- |
| mariadb deployment    | mariadb-deployment.yaml | Startet die App                  |
| mariadb-service       | mariadb-service.yaml    | Macht sie im Cluster erreichbar  |
| mariadb-pvc.yaml      | mariadb-pvc.yaml        | Speicher für Daten               |

Gemäss der vorgegebenen Reihenfolge muss es also so aussehen:

```powershell
kubectl delete namespace unvt-mediawiki
kubectl create namespace unvt-mediawiki
kubectl apply -f ../mediawiki/configs/configmap.yaml
kubectl apply -f ../mediawiki/configs/secret.yaml
kubectl apply -f ../mediawiki/configs/mariadb-pvc.yaml
kubectl apply -f ../mediawiki/configs/pvc.yaml
kubectl apply -f ../mediawiki/configs/mariadb-deployment.yaml
kubectl apply -f ../mediawiki/configs/deployment.yaml
kubectl apply -f ../mediawiki/configs/mariadb-service.yaml
kubectl apply -f ../mediawiki/configs/service.yaml

## Installation prüfen Teil 1:

```powershell
kubectl get pods -n unvt-mediawiki
```

Die Ausgabe zeigt die laufenden Pods. Es kann einige Minuten dauern, bis beide Pods den Status `Running` und `Ready` erreichen. Währenddessen kann der Befehl mehrfach ausgeführt werden. In dem Fall muss die Ausgabe zwei Pods ergeben, die beide mit dem Prefix `unvt-mediawiki-` beginnen.

## Installation prüfen Teil 2:
```powershell
minikube service mediawiki-service -n unvt-mediawiki
```

Das sollte den Browser öffnen und direkt Mediawiki zeigen.

## MediaWiki einrichten
Nun muss der Setupwizard komplett durchgeführt werden. Als Dank dafür erhält man die `LocalSettings.php`.
Jetzt wo wir diese Datei haben, müssen wir sie noch einbinden.

Dafür müssen wir rausfinden, wie unser Pod heisst:
```powershell
kubectl get pods -n unvt-mediawiki
```
Wir suchen denjenigen, der mit `mediawiki-` beginnt. Also z.B. `mediawiki-6dd9c445bd-kgvgs`. Nun kopieren wir die Datei in den Pod, direkt auf den PVC:

```powershell
kubectl cp ../mediawiki/configs/LocalSettings.php unvt-mediawiki/<POD_NAME>:/bitnami/mediawiki/LocalSettings.php
kubectl cp ../mediawiki/configs/LocalSettings.php unvt-mediawiki/<POD_NAME>:/var/www/html/
```

Das Problem ist aber, dass MediaWiki die Datei hier benötigt:` /var/www/html/` und dieser Ordner nicht persistent ist.

## Installation prüfen Teil 3:

