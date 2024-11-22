# NoSQL Partie 1


Redis est une base de données NoSQL de type clef-valeur.
Il est souvent utilisé avec une base de données relationnelle qui permet de sauvegarder les données sur disque (persistence des données) et non sur la RAM.

Il faut commencer par définir une clef :
Dans un terminal :
    Démarrer le serveur grâce à la commande redis-server. Il s'exécute en localhost (127.0.0.1) et écoute par défaut sur le port 6379.
    On va laisser ce terminal de côté le temps de faire nos manipulations.
Dans un autre terminal :
  Démarrer le client grâce à la commande redis-cli, l'interface client-serveur de redis en lignes de commande qui permet d'interragir avec la bd (existe aussi en interface graphique).

  Opérations CRUD possibles : CREATE, READ, UPDATE et DELETE

Exemples :
clef : user:1234
valeur : Chloé

CREATE :
  SET nom_de_la_clef Valeur
  Retourne OK
READ :
  GET nom_de_la_clef
  Retourne Valeur
DELETE :
DEL nom_de_la_clef
  Retourne le type et 1 si la clef existe, 0 sinon

Exemple : 
SET cpt 0
INCR cpt      incrémentation
DECR cpt      décrémentation


