# agi_hoheitsgrenzen_pub

Create Schema und Import Daten agi_hoheitsgrenzen_pub

Git clonen:
```
  git clone https://github.com/bjsvwcur/agi_hoheitsgrenzen_pub.git
```

Docker Container erstellen mit 2 PostgreSQL DBs edit und Pub.

Im Repo "agi_hoheitsgrenzen_pub" den Container erstellen: 

```
  docker-compose down # (this command is optional; it's just for cleaning up any already existing DB containers)
  docker-compose up
```

ENV Variablen auf die "Container"-DB setzen:
```
  export ORG_GRADLE_PROJECT_dbUriEdit="jdbc:postgresql://edit-db/edit"
  export ORG_GRADLE_PROJECT_dbUserEdit="gretl"
  export ORG_GRADLE_PROJECT_dbPwdEdit="gretl"
  export ORG_GRADLE_PROJECT_dbUriPub="jdbc:postgresql://pub-db/pub"
  export ORG_GRADLE_PROJECT_dbUserPub="gretl"
  export ORG_GRADLE_PROJECT_dbPwdPub="gretl"
```

Schemas erstellen in der edit-DB und Pub-DB:

Nachfolgende Befehle aus dem Verzeichnis /agi_hoheitsgrenzen_pub/development_dbs/ ausführen:

PW für admin = admin
```
 psql -h localhost -p 54321 -d edit -U admin -c "SET ROLE admin" --single-transaction -f agi_hoheitsgrenzen_pub_edit.sql -f agi_hoheitsgrenzen_pub_edit_grants.sql 
 psql -h localhost -p 54322 -d pub -U admin -c "SET ROLE admin" --single-transaction -f agi_hoheitsgrenzen_pub_pub.sql -f agi_hoheitsgrenzen_pub_edit_pub.sql 
```

Testdaten in edit-DB importieren:

Nachfolgende Befehle aus dem Verzeichnis /agi_hoheitsgrenzen_pub/development_dbs/ ausführen:

#### !! ACHTUNG !! Pfad in den 2 Files zu ili2pg-4.3.1 anpassen
```
./ili2pg_dataimportEdit_hoheitsgrenzen.sh
```

#### Gretljob starten für Datenexport agi_hoheitsgrenzen_pub.

Nachfolgende Befehle aus dem Verzeichnis */agi_hoheitsgrenzen_pub/agi_hoheitsgrenzen_pub/* ausführen:
```
sudo -E ../start-gretl.sh --docker-image sogis/gretl-runtime:latest --docker-network agihoheitsgrenzenpub_default --job-directory $PWD
```
