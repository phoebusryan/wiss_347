# Installation von Jira

Wie im Hauptkapitel erwähnt, benötigen wir für das manuelle Deployment mindestens die nachfolgenden Files:

| Ressource             | Dateiname                | Zweck                            |
| --------------------- | ------------------------ | -------------------------------- |
| ConfigMap             | configmap.yaml           | Konfigurationswerte (öffentlich) |
| Secret                | secret.yaml              | Geheime Variablen (Passwörter)   |
| PersistentVolumeClaim | pvc.yaml                 | Speicher für Daten               |
| Deployment            | deployment.yaml          | Startet die App                  |
| Service               | service.yaml             | Macht sie im Cluster erreichbar  |

Jira benötigt jedoch noch ein paar weitere Dateien, weil es eine `PostgreSQL` benötigt:

| Ressource             | Dateiname                | Zweck                            |
| --------------------- | ------------------------ | -------------------------------- |
| postgres ConfigMap    | postgres-configmap.yaml  | Konfigurationswerte (öffentlich) |
| postgres Secret       | postgres-secret.yaml     | Geheime Variablen (Passwörter)   |
| postgres PVC          | postgres-pvc.yaml        | Speicher für Daten               |
| postgres Deployment   | postgres-deployment.yaml | Startet die App                  |
| postgres Service      | postgres-service.yaml    | Macht sie im Cluster erreichbar  |

Gemäss der vorgegebenen Reihenfolge muss es also so aussehen:

```powershell
kubectl create namespace unvt-jira
kubectl apply -f configs/postgres-secret.yaml
kubectl apply -f configs/postgres-configmap.yaml
kubectl apply -f configs/postgres-pvc.yaml
kubectl apply -f configs/postgres-deployment.yaml
kubectl apply -f configs/postgres-service.yaml

kubectl apply -f configs/pvc.yaml
kubectl apply -f configs/configmap.yaml
kubectl apply -f configs/secret.yaml
kubectl apply -f configs/deployment.yaml
kubectl apply -f configs/service.yaml
```

## Installation prüfen Teil 1:

```powershell
kubectl get pods -n unvt-jira
```

Die Ausgabe zeigt die laufenden Pods. Es kann einige Minuten dauern, bis beide Pods den Status `Running` und `Ready` erreichen. Währenddessen kann der Befehl mehrfach ausgeführt werden. In dem Fall muss die Ausgabe zwei Pods ergeben, die beide mit dem Prefix `unvt-jira-` beginnen.

## Installation prüfen Teil 2:
```powershell
minikube service jira-service -n unvt-jira
```

Das sollte den Browser öffnen und direkt das Jira-Setup zeigen.

## Installation prüfen Teil 3:
Im Browser ging ein Fenster auf mit einer URL. Vermutlich etwas wie `http://127.0.0.1:54087/`. Das Problem ist, dass man sich hier registrieren muss (habe ich sogar gemacht: `atlassian@unvt.ch` und `qjN8wS9O£3!z`) aber dann hiess es, dass es keine lokale Versionen mehr gibt und man ein Cloud-Abo buchen muss. Unabhängig davon, dass ich das jetzt nicht mache, kann man die Persistenz nicht wirklich testen wenn es keine lokale Version mehr gibt. Grundsätzlich müsste sie aber funktionieren.

Nun kann man die pods löschen:
```powershell
kubectl delete pod -l app=jira -n unvt-jira
kubectl delete pod -l app=postgres -n unvt-jira
```

Nun müssen wir warten bis die Pods wieder da sind:
```powershell
kubectl get pods -n unvt-jira
```

Dann im Browser wieder aufrufen und es dürfte sich nichts verändert haben.