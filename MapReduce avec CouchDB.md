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
L'interface graphique se trouve à l'adresse `localhost:5984/_utils`.

### Création d'une nouvelle base de données
La création d'une base de données se fait via la méthode PUT. Elle retourne `ok` à la valeur `true` si l'opération a réussi.
```bash
  curl -X PUT http://user:password@localhost:5984/nom_de_base
```
### Insertion d'un nouveau document
Sans `document_id`, CouchDB attribue un identifiant de manière automatique.
Manuellement, un document unique :
```bash
  curl -X PUT http://user:password@localhost:5984/nom_de_base/document_id -d '{ "clef": "valeur" }'
```
À partir d'un fichier, un document unique :
```bash
  curl -X POST http://user:password@localhost:5984/nom_de_base -d @fichier.json -H "Content-Type: application/json"
```
À partir d'un fichier, une collection de documents :
```bash
  curl -X POST http://user:password@localhost:5984/nom_de_base/_bulk_docs -d @fichier.json -H "Content-Type: application/json"
```
### Récupération d'un document
```bash
  curl -X GET http://user:password@localhost:5984/nom_de_base/document_id
```

## MapReduce avec CouchDB
Avec CouchDB, on peut définir la fonction Map puis la fonction Reduce et voir les résultats intermédiaires. Il s'agit d'un des seuls SGBD NoSQL qui permette de faire cela.  
Prototype `MapReduce (*un* document): émet un ou plusieurs documents`. Cette fonction transforme le document passé en paramètre.  
### Map
On peut **paralléliser Map** : Chaque noeud traite un sous ensemble de documents.  
La fonction Map par défaut n'opère aucune transformation. Il s'agit de la suivante :
```bash
  function (document) {
    emit(null, document);
  }
```
La phase de tri (sort) est effectuée par défaut lors du Map, par ordre croissant de la clef intermédiaire (premier paramètre de la fonction `emit`).  

### Reduce
La fonction Reduce effectue le regroupement des documents. Elle n'est pas obligatoire pour CouchDB.  
