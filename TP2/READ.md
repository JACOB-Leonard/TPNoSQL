# TP1 - Prise en main de MongoDB

## Table des matières
1. [Introduction à MongoDB](#introduction-à-mongodb)
2. [Objectifs](#objectifs)
3. [Installation de MongoDB sur Linux](#installation-de-mongodb-sur-linux)
4. [Utilisation de MongoDB Compass sur Windows](#utilisation-de-mongodb-compass-sur-windows)
5. [Importation des données](#importation-des-données)
6. Requêtes MongoDB

---

## Introduction à MongoDB

MongoDB est une base de données NoSQL orientée documents. Contrairement aux bases relationnelles qui stockent les données sous forme de tableaux de lignes et colonnes, MongoDB stocke les informations sous forme de documents JSON (BSON en interne). Cela permet une plus grande flexibilité et évolutivité, notamment pour des données complexes ou hiérarchisées. 

MongoDB offre plusieurs avantages :
- **Modèle flexible** : pas besoin de schéma rigide.
- **Scalabilité horizontale** : support du sharding pour distribuer les données.
- **Requêtes puissantes** : utilisation de JSON pour les requêtes et les réponses.
- **Support natif de la réplication et de la haute disponibilité.**

Ce TP vous permettra de manipuler MongoDB à travers différentes requêtes et d'explorer ses principales fonctionnalités.

## Objectifs
Ce TP a pour but de prendre en main MongoDB en manipulant une collection de films via différentes requêtes.

## Installation de mongodb sur Linux

```bash
sudo apt update
sudo apt install -y mongodb-org
```

### Démarrer et activer le service MongoDB

```bash
sudo systemctl start mongod
sudo systemctl enable mongod
```
### Vérifier le statut du service

```bash
sudo systemctl status mongod
```

## Utilisation de MongoDB Compass sur Windows

MongoDB Compass est un outil graphique permettant d'interagir avec MongoDB de manière visuelle.

### Installation :
1. Téléchargez MongoDB Compass depuis [le site officiel](https://www.mongodb.com/try/download/compass).
2. Exécutez l'installateur et suivez les instructions.

### Connexion à MongoDB :
1. Lancez MongoDB Compass.
2. Dans la barre de connexion, entrez l'URI de connexion :
   ```
   mongodb://localhost:27017
   ```
3. Cliquez sur **Connect** pour accéder à la base de données.

## Importation des données
Pour importer la collection de films, utilisez la commande suivante :
```bash
mongoimport --db lesfilms --collection films --file films.json --jsonArray
```
Cette commande importe les films dans la base `lesfilms` sous forme de documents JSON.

---


## Sélection de la base de données
Avant d'exécuter les requêtes, nous devons nous assurer que nous travaillons bien sur la base `lesfilms`.
```javascript
use lesfilms
```

---

## 1. Vérification de l'importation
Nous pouvons vérifier que l'importation a bien fonctionné en comptant le nombre total de films présents dans la base.
```javascript
db.films.count()
```
**Résultat :**
```json
278
```

---

## 2. Affichage d'un document
MongoDB stocke ses données sous forme de documents JSON. La commande suivante permet d'afficher un document de la collection pour observer sa structure.
```javascript
db.films.findOne()
```
**Exemple de résultat :**
```json
{
    "_id": "movie:11",
    "title": "La Guerre des étoiles",
    "year": 1977,
    "genre": "Aventure",
    "summary": "Il y a bien longtemps, dans une galaxie très lointaine...",
    "country": "US",
    ...
}
```

---

## 3. Recherche de films par genre
Pour afficher tous les films appartenant au genre `Action`, on utilise :
```javascript
db.films.find({ genre: "Action" })
```

---

## 4. Compter le nombre de films d'action
```javascript
db.films.count({ genre: "Action" })
```
**Résultat :**
```json
36
```

---

## 5. Afficher les films d'action produits en France
```javascript
db.films.find({ genre: "Action", country: "FR" })
```

---

## 6. Afficher les films d'action produits en France en 1963
```javascript
db.films.find({ genre: "Action", country: "FR", year: 1963 })
```
**Résultat :**
```json
{
    "title": "Les tontons flingueurs",
    "year": 1963,
    "country": "FR",
    ...
}
...
```

---

## 7. Afficher uniquement les titres des films d'action français
```javascript
db.films.find({ genre: "Action", country: "FR" }, { title: 1 })
```
**Résultat :**
```json
{ "_id" : "movie:4034", "title" : "L'Homme de Rio" }
{ "_id" : "movie:9322", "title" : "Nikita" }
{ "_id" : "movie:25253", "title" : "Les tontons flingueurs" }

```

---

## 8. Afficher uniquement les titres des films d'action français sans id
```javascript
db.films.find({ genre: "Action", country: "FR" }, { title: 1, _id: 0 })
```
**Résultat :**
```json
{ "title": "L'Homme de Rio" }
{ "title": "Nikita" }
{ "title": "Les tontons flingueurs" }
```

---

## 9. Afficher les titres et grades des films d'action français
```javascript
db.films.find({ genre: "Action", country: "FR" }, { title: 1, grades: 1, _id: 0 })
```
**Résultat :**
```json
{ "title" : "L'Homme de Rio", 
    "grades" : [ 
        { "note" : 4, "grade" : "D" },
        { "note" : 30, "grade" : "E" },
        { "note" : 34, "grade" : "E" },
        { "note" : 28, "grade" : "F" }
    ]
}
...
```

---

## 10. Afficher les films avec une note > 10
```javascript
db.films.find({ "grades.note": { $gt: 10 } }, { title: 1, grades: 1, _id: 0 })
```

---

## 11. Afficher les films avec toute les notes > 10
```javascript
db.films.find({ genre: "Action", country: "FR", "grades.note": { $not: { $lt: 10 } } }, { title: 1, grades: 1, _id: 0 })
```

---

## 12. Afficher les genres disponibles
```javascript
db.films.distinct("genre")
```
**Résultat :**
```json
[
    "Action",
    "Adventure",
    "Aventure",
    "Comedy",
    ...
]
```

---

## 13. Afficher les différentes notes attribuées
```javascript
db.films.distinct("grades.grade")
```
**Résultat :**
```json
[ "A", "B", "C", "D", "E", "F" ]
```

---

## 14. Afficher tous les films avec certains acteurs
```javascript
db.films.find({ "actors._id": { $in: ["artist:4", "artist:18", "artist:11"] } })
```

---

## 15. Afficher les films sans résumé
```javascript
db.films.find({ summary: { $exists: false } })
```

---

## 16. Afficher les films avec Leonardo DiCaprio en 1997
```javascript
db.films.find({ "actors.last_name": "DiCaprio", "actors.first_name": "Leonardo", year: 1997 })
```
**Résultat :**
```json
{
    "title": "Titanic",
    "year": 1997,
    ...
}
...
```

---

## 17. Afficher les films avec Leonardo DiCaprio ou en 1997
```javascript
db.films.find({ $or: [{ "actors.last_name": "DiCaprio" }, { year: 1997 }] })
```

---

## Conclusion
MongoDB offre une grande souplesse pour le stockage et l'interrogation de données. Ce TP a permis d'explorer différentes requêtes essentielles pour interagir avec une base de données documentaire.
