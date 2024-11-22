### Présenté par Chloé Hamilcaro

# NoSQL Partie 1 

Redis est une base de données NoSQL de type clef-valeur. Il est souvent utilisé en complément d'une base relationnelle pour garantir la **persistance des données** (sauvegarde sur disque plutôt que sur la RAM).

## Démarrage de Redis

### Démarrer le Serveur Redis
Dans un terminal :
- Lancer le serveur grâce à la commande :
  
  ```bash
  redis-server
  ```
  Il s'exécute en localhost (127.0.0.1) et par défaut sur le port 6379.

### Démarrer le Client Redis
Dans un autre terminal :
- Démarrer le client grâce à la commande :
  ```bash
  redis-cli
  ```
  Redis-cli est l'interface client-serveur de Redis en lignes de commande, qui permet d'interagir avec la base de données (existe aussi une interface graphique).

## Opérations CRUD avec Redis
**CRUD** : Create, Read, Update, Delete.

### Exemples d'Opérations CRUD
- **Clé** : `user:1234`
- **Valeur** : `Chloé`

#### CREATE
- Pour créer une clé avec une valeur associée :
  
  ```bash
  SET nom_de_la_clef Valeur
  ```
  Exemple :
  ```bash
  SET user:1234 "Chloé"
  ```
  Retourne `OK`.
#### READ
- Pour lire la valeur d'une clé :
  ```bash
  GET nom_de_la_clef
  ```
  Exemple :
  ```bash
  GET user:1234
  ```
  Retourne la valeur si ce n'est pas une liste, sinon retourne une erreur.
  - Pour une liste :
    ```bash
    LRANGE nom_de_la_clef 0 -1
    ```
    Retourne les valeurs entre `0` et `-1` (fin de liste).

#### DELETE
- Pour supprimer une clé :
  ```bash
  DEL nom_de_la_clef
  ```
  Retourne le type et `1` si la clé existe, `0` sinon.


#### Compteur
  ```bash
  SET cpt 0
  INCR cpt      # Incrémentation
  DECR cpt      # Décrémentation
  ```

## Durée de vie d'une clef :
- Pour obtenir la durée de vie (ttl) d'une clef :
  ```bash
  TTL clef
  ```
  Retourne `-1` si la durée de vie est indéfinie.
- Pour définir le délai d'expiration d'une clé :
  ```bash
  EXPIRE clef secondes
  ```
  Exemple :
  ```bash
  EXPIRE user:1234 120
  ```
  Pour vérifier la durée de vie :
  ```bash
  TTL clef
  ```

## Structures de Données dans Redis

### 1. Listes
- Les éléments ne sont pas obligatoirement distincts.
- **Insertion d'une valeur** :
  ```bash
  RPUSH liste valeur
  ```
- **Suppression d'une valeur** :
  ```bash
  LPOP liste    # Suppression de l'élément de la fin de liste
  RPOP liste    # Suppression de l'élément en tête de liste
  ```

### 2. Sets
- Liste non-ordonnée, avec éléments distincts.
- **Insertion d'une valeur** :
  ```bash
  SADD set valeur
  ```
  Retourne `1` si l'insertion est réussie, `0` sinon.
- **Visualisation du set** :
  ```bash
  SMEMBERS set
  ```
- **Suppression d'un élément** :
  ```bash
  SREM set valeur
  ```
- **Fusion de sets** :
  ```bash
  SUNION set1 set2
  ```
### 3. Sets Ordonnés
- **Insertion d'une valeur** :
  ```bash
  ZADD scores 19 "Prénom1"
  ZADD scores 20 "Prénom2"
  ```
- **Visualisation** :
  ```bash
  ZRANGE scores 0 -1      # Ordre croissant
  ZREVRANGE scores 0 -1   # Ordre décroissant
  ```
- **Indice d'une valeur** :
  ```bash
  ZRANK scores "Prénom"
  ```
  Retourne l'indice de la valeur dans le set ordonné.

### 4. Hashes
- Permettent de stocker des objets avec plusieurs champs (un peu comme des colonnes dans une table).
- **Insertion d'éléments** :
  ```bash
  HSET user:1234 name "Chloé"
  HSET user:1234 age 23
  ```
- **Insertion de plusieurs éléments** :
  ```bash
  HMSET user:4321 name "Eolch" age 23
  ```
- **Visualisation** :
  ```bash
  HGETALL user:1234
  ```
- **Incrémenter un champ** :
  Exemple (incrémentation de l'âge) :
  ```bash
  HINCRBY user:4321 age 10
  ```
  Retourne l'âge incrémenté de `10`.



### 5. Pub/Sub
Utilisé pour l'échange de données en temps réel

#### **Illustration :**
1. **Ouvrir un deuxième client Redis** :  
   - Ouvrir un deuxième client
   - Sur le 2e terminal :
    - S'abonner à un canal :
      ```bash
      SUBSCRIBE canal
      ```
    - S'abonner à tous les canaux commençant par "mes*" :
      ```bash
      PSUBSCRIBE mes*
      ```
   - Sur le 1er terminal :
    - Publier un message sur tout le canal :
      ```bash
      PUBLISH canal "Bienvenue"
      ```
    - Publier un message pour un seul client :
      ```bash
      PUBLISH user:1234 "Bonjour"

### Utiles
Visualiser toutes les clefs définies pour une session :
```bash
  KEYS *
```
Par défaut, redis met à disposition de ses utilisateurs 16 bases de données. Pour changer de base de données :
```bash
SELECT numéro_de_base
```

Reprise sur panne à configurer manuellement.

