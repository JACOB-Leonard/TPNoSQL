# Tp4 : CouchDB et MapReduce

## 1. Introduction à CouchDB

### 1.1 CouchDB

CouchDB est une base de données NoSQL orientée document qui stocke les données sous format JSON. Grâce à son architecture flexible et distribuée, elle est idéale pour gérer des volumes importants d’informations de manière résiliente. CouchDB repose sur un modèle RESTful pour les interactions et exploite MapReduce pour indexer et traiter efficacement les données. 

Dans le cadre de ce TP, nous utiliserons CouchDB pour modéliser des matrices représentant les relations entre différentes pages web et effectuer des calculs via des traitements MapReduce. 

### Avantages de CouchDB :
- **Simplicité d’installation et d’utilisation**.
- **Open Source sous licence Apache**.
- **Architecture RESTful** permettant une interaction fluide avec :
  - **GET** : Récupération de données.
  - **PUT** : Création de ressources.
  - **POST** : Envoi de nouvelles informations.
  - **DELETE** : Suppression d’éléments.


### 1.2 Installation de CouchDB

#### Via Docker

Pour exécuter CouchDB avec Docker, utilise la commande suivante :

📖 [Page du Docker CouchDb](https://hub.docker.com/_/couchdb)
```bash
docker run -d --name couchDbDemo -e COUCHDB_USER=abc -e COUCHDB_PASSWORD=123 -p 5984:5984 couchdb
```

Le client graphique est disponible à l'adresse
```
http://localhost:5984
```

### 1.3 Vérification du serveur CouchDB

Pour vérifier si CouchDB est actif :

```bash
curl -X GET http://abc:123@localhost:5984/
```

---

## 2. Manipulation des bases de données

### 2.1 Création d’une base de données

Pour créer une base de données appelée `films` :

```bash
curl -X PUT http://abc:123@localhost:5984/films
```

### 2.2 Insertion de documents

#### Insertion d’un document simple

```bash
curl -X PUT http://abc:123@localhost:5984/films/doc1 -d '{"titre": "Inception", "annee": 2010}'
```

#### Insertion en masse

```bash
curl -X POST http://abc:123@localhost:5984/films/_bulk_docs -d @films_couchdb.json -H "Content-Type: application/json"
```

Exemple de fichier JSON :

```json
{
  "docs": [
    {"_id": "movie:1001", "titre": "Manie Manie", "annee": 1987},
    {"_id": "movie:1002", "titre": "Memories", "annee": 1995}
  ]
}
```

### 2.3 Récupération d’un document

```bash
curl -X GET http://abc:123@localhost:5984/films/movie:1001
```

---

## 3. MapReduce avec CouchDB

### 3.1 Introduction à MapReduce

MapReduce est un modèle de programmation distribué conçu pour traiter efficacement de vastes ensembles de données en parallèle sur un cluster de machines. Il repose sur deux étapes fondamentales :

- **Map** : Chaque entrée est transformée de manière indépendante en une ou plusieurs paires clé-valeur.
- **Reduce** : Les paires clé-valeur sont regroupées par clé et agrégées pour produire un résultat final optimisé.

Ce paradigme permet d’exécuter des calculs massifs de manière scalable et résiliente, rendant MapReduce idéal pour l’analyse de grandes bases de données.

📖 [Documentation officielle de MapReduce](https://en.wikipedia.org/wiki/MapReduce)

### 3.2 Exemple : Nombre de films par année

#### Fonction Map

```javascript
function (doc) {
  emit(doc.annee, doc.titre);
}
```

#### Fonction Reduce

```javascript
function (keys, values) {
  return values.length;
}
```

### 3.3 Exemple : Nombre de films pour chaque acteur

#### Fonction Map

```javascript
function (doc) {
  for (var i = 0; i < doc.actors.length; i++) {
    emit({ "prenom": doc.actors[i].rst_name, "nom": doc.actors[i].last_name }, doc.titre);
  }
}
```

#### Fonction Reduce

```javascript
function (keys, values) {
  return values.length;
}
```
---
