# MongoDB
SGBD NoSQL de type document.
## Démarrage
J'utilise MongoDB Compass, car mon ordinateur me le permet.  
Changer de base de données : `use nom_de_base`  

## Utilitaires
Lister un document d'une collection : `db.ma_collection.findOne()`  
Filtrage : `db.ma_collection.find({...}, {...})` où le premier JSON correspond aux filtres, et où le deuxième est optionnel et correspond à la projection.  
Éléments distincts: `db.ma_collection.distinct(...)` où les clefs sont entourées de guillemets.

## Exercice
3. Liste des films d'action
```bash
  db.films.find({ genre: "Action" })
```
4. Nombre de films d'action : `36`
```bash
  db.films.find({ genre: "Action" }).count()
```
5. Films d'action français
```bash
  db.films.find({ genre: "Action", country: "FR" })
```
6. Films d'action français de 1963
```bash
  db.films.find({ genre: "Action", country: "FR", year: "1963" })
```
7. Films d'action français, sans retourner le champs grades
```bash
  db.films.findOne({ genre: "Action", country: "FR"}, { grades: 0 })
```
8. Films d'action français, sans retourner l'identifiant du film
```bash
  db.films.findOne({ genre: "Action", country: "FR"}, { _id: 0 })
```
9. Titres et grades des films d'action français
```bash
  db.films.find({ genre: "Action", country: "FR"}, { _id: 0, title: 1, grades: 1 })
```
10. Titres et grades des films d'action français qui ont au moins une note supérieure à 10
```bash
  db.films.find({ genre: "Action", country: "FR", "grades.note": { $gt: 10 }}, { _id: 0, title: 1, grades: 1 })
```
11. Titres et grades des films d'action français qui n'ont eu que des notes supérieures à 10
```bash
  db.films.find({ genre: "Action", country: "FR", "grades.note": { $gt: 10, $not: { $lte: 10 } }}, { _id: 0, title: 1, grades: 1 })
```
12. Genres de toute la base des films
```bash
  db.films.distinct("genre")
```
13. Différents grades attribués
```bash
  db.films.distinct("grades")
```
14. Films dans lesquels jouent artist:4 et/ou artist:18 et/ou artist:11.
```bash
  Je ne comprends pas l''énoncé, cela ne correspond pas au schéma du document.
```
15. Films sans résumé
```bash
  db.films.find({ summary: { $exists: false }}, { _id: 0 })
```
16. Films tournés avec Leonardo DiCaprio en 1997
```bash
  db.films.find({year: 1997, actors: { $elemMatch: { first_name: "Leonardo", last_name: "DiCaprio" }}}, { _id: 0 })
```
17. Films tournés avec Leonardo DiCaprio ou en 1997 (ou inclusif)
```bash
  db.films.find({ $or: [{ year: 1997 }, { actors: { $elemMatch: { first_name: "Leonardo", last_name: "DiCaprio" }}}]}, { _id: 0 })
```
