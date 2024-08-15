- camel k mqtt receiver
- camel k usecase database fill and process
- camel k usecase bucket fill
- grafana & tempo
- keycloak HA
- keycloak as Oauth in OpenShift & Developer Hub



Aktuell mache ich bei einem Proof of concept zu Camel K (der kubernetes deployment Lösung des auf Quarkus basierenden Integrationslösung Camel) mit und habe einige Erfahrungen gemacht die ich gerne in vier Blog posts, die Teillösungen beschreiben, teilen würde.
Vom Quellcode wurde die Basisfassung von meinem lieben Kollegen Jean Nyilimbibi angefertigt und open source gestellt: https://bitbucket.org/jnyilimbibi/camel-k-usecases
Ich habe sein git Repostory geforkt und dahingehend umstruktiert, dass alles an nicht-Camel-K-Lösungssoftware in einem helm chart gemanaged wird, damit klar wird wie einfach es ist, komplexe Lösungen in Camel K zu erstellen: https://bitbucket.org/gpmodz/camel-k-usecases 
Generell wurden die Lösungen mittels Kaoto (https://kaoto.io/) erstellt, einer no-code Lösung womit man sich seine Integrationen einfach zusammenklicken kann.
1. MQTT receiver to logs
  - Infrastruktur: Ein AMQ Broker ist auf einem cluster deployt. Zugänge stehen in einem Secret im Namespace bereit.
  - Lösung: Camel K Route hört auf ein bestimmtes topic im MQTT broker. So bald eine message ankommt, wird diese aufgegriffen und ins log geschrieben.
2. Eintrag in Postgresql Datenbank auslesen und in MQTT topic schreiben
  - Infrastruktur: 
    - Postgresql Datenbank. Zugangsdaten liegen in einem Secret im Namespace bereit
    - Eine AMQ Broker Instanz ist auf einem cluster deployt. Zugänge stehen in einem Secret im Namespace bereit.
  - Lösung: Camel K Route liest aus der Datenbank Einträge aus, ändert das Format zu JSON und schreibt die Informationen in ein MQTT topic. Nach einer erfolgreicher Verarbeitung werden die Einträge aus der Datenbank gelöscht.
  - Erweiterung: fill-db golang applikation, die zufällig generierte Datenbankeinträge erstellt. Wird in der Lösung als cronjob eingebunden, so dass jede Minute die Integration Daten verarbeitet.
3. XML Datei aus S3 bucket auslesen und in MQTT topic schreiben
  - Infrastruktur: 
    - Eine laufende Minio Instanz zur Bereitstellung von S3 Buckets. Zugangsdaten liegen in einem Secret im Namespace bereit
    - Eine AMQ Broker Instanz ist auf einem cluster deployt. Zugänge stehen in einem Secret im Namespace bereit.
  - Lösung: Camel K Route liest eine XML Datei aus einem S3 bucket aus, ändert das Format zu JSON und schreibt die Informationen in ein MQTT topic. Nach einer erfolgreicher Verarbeitung wird die Datei aus dem S3 bucket gelöscht.
  - Erweiterung: fill-bucket golang applikation, die Beispielsdateien aus einem Ordner in das s3 bucket hoch lädt. Wird in der Lösung als cronjob eingebunden, so dass jede Minute die Integration Daten verarbeitet.
4. CSV Datei aus S3 bucket auslesen und in MQTT topic schreiben
  - Infrastruktur: 
    - Eine laufende Minio Instanz zur Bereitstellung von S3 Buckets. Zugangsdaten liegen in einem Secret im Namespace bereit
    - Eine AMQ Broker Instanz ist auf einem cluster deployt. Zugänge stehen in einem Secret im Namespace bereit.
  - Lösung: Camel K Route liest csv Dateien aus einem S3 bucket aus, ändert das Format zu JSON und schreibt die Informationen in ein MQTT topic. Nach einer erfolgreicher Verarbeitung wird die Datei aus dem S3 bucket gelöscht.
  - Erweiterung fill-bucket aus Blog Post 3 wird auch hier genutzt

Schreibe alle vier Blog Posts (Daten 1., 2., 5. und 6. August 2024) auf englisch, in markdown. 








---

Schreibe einen Blog post in englisch als Markdown Datei mit folgendem Inhalt:
- Quarkus Workshop gehalten (Folien hänge ich dieser Nachricht bei)
- Themenschwerpunkt war die Quarkus Superheroes demo, hier das dazu gehörige github repository: https://github.com/quarkusio/quarkus-super-heroes
- Der Blogpost soll hauptsächlich darüber handeln, dass die Demo zu sehr vielen use cases Lösungen enthält, die auf best practices basieren. Sollte jemand also einen bestimmten use case haben und vor der Aufgabe stehen, diesen in Quarkus umzusetzen, dann empfehle ich einen Blick in dieses github repository zu werfen.