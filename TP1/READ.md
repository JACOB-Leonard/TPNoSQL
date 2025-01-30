# README - Introduction à Redis

Redis est un système de gestion de bases de données NoSQL open source, performant et polyvalent.

Ce fichier vous guidera dans la compréhension des bases de Redis, son installation sur différents systèmes d'exploitation et les principales commandes pour exploiter ses fonctionnalités. Redis est principalement utilisé comme base de données en mémoire, cache ou courtier de messages.

## Table des matières
1. [Les Familles de Bases de Données NoSQL](#les-familles-de-bases-de-données-nosql)
2. [Présentation de Redis](#présentation-de-redis)
3. [Installation de Redis](#installation-de-redis)
   - [Linux](#installation-sur-linux)
4. [Manipulations avec Redis](#manipulations-avec-redis)
   - [Opérations de base sur les clés](#opérations-de-base-sur-les-clés)
   - [Gestion des Compteurs](#gestion-des-compteurs)
   - [Gestion de la Durée de Vie des Clés](#gestion-de-la-durée-de-vie-des-clés)
   - [Listes](#listes)
   - [Ensembles et Union](#ensembles-et-union)
   - [Ensembles Ordonnés](#ensembles-ordonnés)
   - [Hashes](#hashes)
   - [Communication Publish/Subscribe](#communication-publishsubscribe)
5. [Fonctionnalités Avancées de Redis](#fonctionnalités-avancées-de-redis)
6. [Conclusion](#conclusion)

## Les Familles de Bases de Données NoSQL
Les bases de données NoSQL sont classées en quatre grandes familles :

1. **Bases de données clé-valeur** :
   - Stockent les données sous forme de paires clé-valeur.
   - Exemples : Redis, DynamoDB.

2. **Bases de données orientées documents** :
   - Permettent de gérer des documents structurés tels que JSON ou XML.
   - Exemples : MongoDB, Couchbase.

3. **Bases de données orientées colonnes** :
   - Optimisées pour les requêtes massives sur des colonnes spécifiques.
   - Exemples : Cassandra, HBase.

4. **Bases de données orientées graphes** :
   - Utilisées pour modéliser et interroger des relations complexes.
   - Exemples : Neo4j, ArangoDB.

## Présentation de Redis
Redis (Remote Dictionary Server) est un système de gestion de bases de données clé-valeur en mémoire. Il est conçu pour des performances élevées, avec des fonctionnalités avancées telles que :
- Structures de données variées (listes, ensembles, haches, ensembles ordonnés).
- Persistance des données sur disque.
- Fonctionnalités de publication/abonnement pour la communication en temps réel.
- Utilisation en tant que cache rapide ou système de file d'attente.

## Installation de Redis

### Installation sur Linux
1. Installez Redis à l'aide du gestionnaire de paquets de votre distribution (par exemple, apt pour Debian/Ubuntu) :
   ```bash
   sudo apt update
   sudo apt install redis
   ```
2. Démarrez le serveur Redis :
   ```bash
   sudo redis-server
   ```
3. Connectez-vous avec le client :
   ```bash
   redis-cli
   ```

## Manipulations avec Redis

### Opérations de base sur les clés
Les clés dans Redis sont utilisées pour identifier de manière unique une valeur ou une structure de données. Vous pouvez créer, lire, mettre à jour et supprimer des clés. Voici les commandes principales :

1. **Créer ou mettre à jour une clé** :
   [Documentation officielle de SET](https://redis.io/commands/set)
   ```bash
   SET user "John"
   OK
   ```

2. **Lire la valeur associée à une clé** :
   [Documentation officielle de GET](https://redis.io/commands/get)
   ```bash
   GET user
   "John"
   ```

3. **Supprimer une clé** :
   [Documentation officielle de DEL](https://redis.io/commands/del)
   ```bash
   DEL user
   (integer) 1
   ```

### Gestion des Compteurs
Les compteurs permettent d'incrémenter ou de décrémenter les valeurs associées à une clé. Utile pour des suivis comme le nombre de visiteurs.

1. **Incrémenter une clé numérique** :
   [Documentation officielle de INCR](https://redis.io/commands/incr)
   ```bash
   INCR compteur
   (integer) 1
   ```

2. **Décrémenter une clé numérique** :
   [Documentation officielle de DECR](https://redis.io/commands/decr)
   ```bash
   SET lmars 5
   OK
   DECR lmars
   (integer) 4
   ```

### Gestion de la Durée de Vie des Clés
Redis permet de définir une durée de vie pour les clés, ce qui est utile pour les données temporaires.

1. **Définir une durée de vie** :
   [Documentation officielle de EXPIRE](https://redis.io/commands/expire)
   ```bash
   SET macle "temporaire"
   OK
   EXPIRE macle 120
   (integer) 1
   ```

2. **Vérifier le temps restant** :
   [Documentation officielle de TTL](https://redis.io/commands/ttl)
   ```bash
   TTL macle
   (integer) 120
   ```

3. **Supprimer une clé avant expiration** :
   [Documentation officielle de DEL](https://redis.io/commands/del)
   ```bash
   DEL macle
   (integer) 1
   ```

### Listes
Les listes sont des structures ordonnées où les éléments sont ajoutés à gauche ou à droite.

1. **Ajouter des éléments à une liste** :
   - Ajouter à droite :
     [Documentation officielle de RPUSH](https://redis.io/commands/rpush)
     ```bash
     RPUSH mesCours "BDA"
     (integer) 1
     ```
   - Ajouter à gauche :
     [Documentation officielle de LPUSH](https://redis.io/commands/lpush)
     ```bash
     LPUSH mesCours "Math"
     (integer) 1
     ```

2. **Extraire des éléments d'une liste** :
   - Extraire à gauche :
     [Documentation officielle de LPOP](https://redis.io/commands/lpop)
     ```bash
     LPOP mesCours
     "Math"
     ```
   - Extraire à droite :
     [Documentation officielle de RPOP](https://redis.io/commands/rpop)
     ```bash
     RPOP mesCours
     "BDA"
     ```

3. **Lire une plage d'éléments** :
   [Documentation officielle de LRANGE](https://redis.io/commands/lrange)
   ```bash
   LRANGE mesCours 0 -1
   1) "Math"
   2) "BDA"
   ```

### Ensembles et Union
Les ensembles sont des collections d'éléments uniques, sans ordre spécifique. Ils prennent en charge des opérations comme l'union ou l'intersection.

1. **Ajouter des éléments à un ensemble** :
   [Documentation officielle de SADD](https://redis.io/commands/sadd)
   ```bash
   SADD utilisateurs "Alice"
   (integer) 1
   ```

2. **Afficher les membres d'un ensemble** :
   [Documentation officielle de SMEMBERS](https://redis.io/commands/smembers)
   ```bash
   SMEMBERS utilisateurs
   1) "Alice"
   ```

3. **Effectuer l'union de deux ensembles** :
   [Documentation officielle de SUNION](https://redis.io/commands/sunion)
   ```bash
   SADD autresUtilisateurs "Bob"
   (integer) 1
   SUNION utilisateurs autresUtilisateurs
   1) "Alice"
   2) "Bob"
   ```

### Ensembles Ordonnés

Les ensembles ordonnés (sorted sets) permettent de stocker des éléments uniques associés à un score numérique. Les éléments sont automatiquement triés en fonction de leur score.

1. **Ajouter un élément avec un score** :  
   [Documentation officielle de ZADD](https://redis.io/commands/zadd)
   ```bash
   ZADD score4 19 "Alice"
   (integer) 1
   ```

2. **Retourner les éléments par ordre croissant** :  
   [Documentation officielle de ZRANGE](https://redis.io/commands/zrange)
   ```bash
   ZRANGE score4 0 -1 WITHSCORES
   1) "Alice"
   2) "19"
   ```

3. **Retourner les éléments par ordre décroissant** :  
   [Documentation officielle de ZREVRANGE](https://redis.io/commands/zrevrange)
   ```bash
   ZREVRANGE score4 0 -1 WITHSCORES
   1) "Alice"
   2) "19"
   ```

4. **Incrémenter le score d’un élément** :  
   [Documentation officielle de ZINCRBY](https://redis.io/commands/zincrby)
   ```bash
   ZINCRBY score4 10 "Alice"
   "29"
   ```

5. **Compter les éléments dans une plage de scores** :  
   [Documentation officielle de ZCOUNT](https://redis.io/commands/zcount)
   ```bash
   ZCOUNT score4 10 30
   (integer) 1
   ```

---

### Hashes

Les hashes permettent de stocker des objets structurés sous forme de paires champ-valeur, utiles pour représenter des entités complexes comme un utilisateur.

1. **Ajouter ou mettre à jour des champs dans un hash** :  
   [Documentation officielle de HSET](https://redis.io/commands/hset)
   ```bash
   HSET user:11 username "John"
   (integer) 1
   HSET user:11 age 31
   (integer) 1
   ```

2. **Lire tous les champs et leurs valeurs** :  
   [Documentation officielle de HGETALL](https://redis.io/commands/hgetall)
   ```bash
   HGETALL user:11
   1) "username"
   2) "John"
   3) "age"
   4) "31"
   ```

3. **Incrémenter un champ numérique** :  
   [Documentation officielle de HINCRBY](https://redis.io/commands/hincrby)
   ```bash
   HINCRBY user:11 age 4
   (integer) 35
   ```

4. **Supprimer un champ spécifique** :  
   [Documentation officielle de HDEL](https://redis.io/commands/hdel)
   ```bash
   HDEL user:11 age
   (integer) 1
   ```

5. **Vérifier l'existence d'un champ** :  
   [Documentation officielle de HEXISTS](https://redis.io/commands/hexists)
   ```bash
   HEXISTS user:11 username
   (integer) 1
   ```

---

### Communication Publish/Subscribe

Le modèle Publish/Subscribe permet une communication en temps réel entre différents clients via des canaux.

1. **Souscrire à un canal** :  
   [Documentation officielle de SUBSCRIBE](https://redis.io/commands/subscribe)
   ```bash
   SUBSCRIBE mesCours
   Reading messages... (press Ctrl-C to quit)
   1) "subscribe"
   2) "mesCours"
   3) (integer) 1
   ```

2. **Publier un message sur un canal** :  
   [Documentation officielle de PUBLISH](https://redis.io/commands/publish)
   ```bash
   PUBLISH mesCours "Nouveau cours disponible"
   (integer) 1
   ```

3. **Souscrire à plusieurs canaux via un pattern** :  
   [Documentation officielle de PSUBSCRIBE](https://redis.io/commands/psubscribe)
   ```bash
   PSUBSCRIBE mes*
   Reading messages... (press Ctrl-C to quit)
   ```

4. **Se désabonner d’un canal** :  
   [Documentation officielle de UNSUBSCRIBE](https://redis.io/commands/unsubscribe)
   ```bash
   UNSUBSCRIBE mesCours
   1) "unsubscribe"
   2) "mesCours"
   3) (integer) 0
   ```

5. **Afficher les canaux actifs** :  
   [Documentation officielle de PUBSUB](https://redis.io/commands/pubsub)
   ```bash
   PUBSUB CHANNELS
   1) "mesCours"

---

### Fonctionnalités Avancées de Redis
Redis propose des fonctionnalités avancées comme la persistance, les transactions, et les pipelines.

1. **Snapshot (RDB)** :
   Enregistrez l'état de la base sur disque.
   [Documentation officielle de SAVE](https://redis.io/commands/save)
   ```bash
   SAVE
   OK
   ```

2. **Append-only File (AOF)** :
   Activez l'option pour journaliser chaque commande.
   [Documentation officielle de CONFIG SET](https://redis.io/commands/config-set)
   ```bash
   CONFIG SET appendonly yes
   OK
   ```

3. **Transactions** :
   Groupez plusieurs commandes en une seule opération atomique.
   [Documentation officielle de MULTI/EXEC](https://redis.io/commands/multi)
   ```bash
   MULTI
   OK
   SET compteur 100
   QUEUED
   INCR compteur
   QUEUED
   EXEC
   1) OK
   2) (integer) 101
   ```

4. **Pipelines** :
   Envoyez plusieurs commandes à la fois pour réduire la latence réseau.
   ```bash
   redis-cli --pipe < commands.txt
   ```

5. **Cluster Redis** :
   Configurez un cluster pour répartir les données sur plusieurs nœuds.
   [Documentation officielle sur les Clusters](https://redis.io/docs/management/scaling/)

## Conclusion
Pour approfondir vos connaissances, consultez la [documentation officielle de Redis](https://redis.io/commands/).
