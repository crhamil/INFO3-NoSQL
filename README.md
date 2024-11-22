# NoSQL Partie 1
# Vidéo 1


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
  Retourne Valeur si Valeur n'est pas une liste, Erreur sinon

    LRANGE nom_de_la_clef 0 -1
    Retourne Valeur entre 0 et -1 (fin de liste) si Valeur est une liste
  
DELETE :
DEL nom_de_la_clef
  Retourne le type et 1 si la clef existe, 0 sinon

Exemple : 
SET cpt 0
INCR cpt      incrémentation
DECR cpt      décrémentation



Durée de vie d'une clef :
    TTL clef
    Retourne -1 si indéfini
Définir le délai d'exécution de la clef
    EXPIRE clef secondes
    TTL clef retourne ainsi la durée de vie d'une clef depuis que EXPIRE a été défini


Structures de données :

Listes :
    Infos : les éléments ne sont pas obligatoirement distincts
    Insertion d'une valeur :
        RPUSH liste valeur
    Suppression d'une valeur :
        LPOP liste    Suppression de l'élément de la fin de liste
        RPOP liste    Suppression de l'élément en tête de liste

Set :
    Infos : liste non-ordonnée
    Insertion d'une valeur :
        SADD set valeur
        Retourne 1 si OK, 0 si NOK
    Visualiser le set :
        SMEMBERS set
    Suppression d'un élément :
        SREM valeur
    Fusion de sets :
        SUNION set1 set2


Set ordonné :
    Insertion d'une valeur :
        ZADD scores 19 "Prénom1"
        ZADD scores 20 "Prénom2"
    Visualisation :
        ZRANGE scores 0 -1
        Retourne le set ordonné par ordre croissant
        ZREVRANGE scores 0 -1
        Retourne le set ordonné par ordre décroissant
    Indice d'une valeur :
        ZRANK scores "Prénom"
        Retourne l'indice de la valeur dans le set ordonné


Hash :
    Insertion d'éléments dans un tableau de hachage :
    HSET user:1234 name "Chloé"
    HSET user:1234 age 23
    ...
    Insertion de plusieurs éléments dans tableau de hachage :
        HMSET user:4321 name "Eolch" age 23
    Visualisation :
        HGETALL user:1234
        HGETALL user:4321
    Incrémenter, par ex l'âge :
        HINCRBY user:4321 age 10
        Retourne l'âge de user:4321 incrémenté de 10
