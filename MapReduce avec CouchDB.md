# MapReduce avec CouchDB
SGBD NoSQL de type document qui contient une API REST.

## Démarrage
CouchDB écoute par défaut sur le port 5984.  
On peut installer CouchDB directement ou via Docker. Sur le site de Docker Hub, on a les instructions à suivre pour installer l'image :
```bash
  docker run -d --name couchdbdemo -e COUCHDB_USER=youcef -e COUCHDB_PASSWORD=samir -p 5984:5984 couchdb
```
Dans la suite de ce rapport, les commandes sont effectuées pour un SGBD installé via Docker. L'URL peut varier si l'installation a été efectuée directement depuis l'exécutable de CouchDB.  
CouchDB s'interface avec un **client** ou une **interface graphique**. Attention, il s'agit bien d'une API REST, donc on requête le serveur via le protocole `HTTP` et les méthodes `PUT`, `GET`, `POST`, `DELETE`.

### Création d'une nouvelle base de données
La création d'une base de données se fait via la méthode PUT. Elle retourne `ok` à la valeur `true` si l'opération a réussi.
```bash
  curl -X PUT http://user:password@localhost:5984/nom_de_base
```
### Insertion d'un nouveau document
Sans `document_id`, CouchDB attribue un identifiant de manière automatique.
Manuellement :
```bash
  curl -X PUT http://user:password@localhost:5984/nom_de_base/document_id -d '{ "clef": "valeur" }'
```
À partir d'un fichier :
```bash
  curl -X POST http://user:password@localhost:5984/nom_de_base -d @fichier.json -H "Content-Type: application/json"
```
