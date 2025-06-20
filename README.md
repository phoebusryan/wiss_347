# Modul 347 - Dienst mit Container anwenden 

## Einleitung
Für das vorliegende Projekt wird eine containerisierte Umgebung auf Basis von Kubernetes aufgebaut, welche die drei Webanwendungen WordPress, MediaWiki und Jira zuverlässig bereitstellt. Ziel ist es, alle Applikationen inklusive Datenbanken mit Persistenz, Monitoring und Zugriff über einen zentralen Ingress-Reverse-Proxy in einem lokalen Cluster zu betreiben.

## Beispielszenario
Ein mittelständisches Unternehmen plant den Aufbau einer internen IT-Umgebung, die verschiedene Anforderungen zentral abdeckt:

- **WordPress** dient als öffentlich zugängliche **Unternehmenswebsite**, um Dienstleistungen, Neuigkeiten und Karriereseiten zu präsentieren.
- **MediaWiki** wird intern als **Wissensdatenbank** und **Intranet-Lösung** genutzt, um Dokumentationen, Teamwissen und Prozessbeschreibungen für Mitarbeitende bereitzustellen.
- **Jira** kommt als zentrales **Projektmanagement- und Ticketsystem** zum Einsatz, um Aufgaben, Bugs und Support-Anfragen strukturiert zu verwalten.

Die Firma legt grossen Wert auf **Skalierbarkeit**, **Zuverlässigkeit** und **Zugriffskontrolle**, weshalb die Umgebung containerisiert und mit Kubernetes orchestriert wird. Durch den Einsatz eines zentralen Ingress-Controllers können alle Dienste über eine gemeinsame Adresse und klar definierte Pfade bereitgestellt werden.