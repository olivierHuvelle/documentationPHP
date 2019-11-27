# Stocker des informations dans une base de données 
## Qu'est-ce qu'une base de données 
Index 
* Le langage SQL et les bases de données 
* Structure d'une base de données 
* Mais où sont enregistrées les données 

### Le langage SQL et les bases de données 
Une base de données -> les informations sont classiques 
SGBD (Système de Gestion de Bases de Données)
* MySQL (libre, gratuit, très utilisé)
* PostGreSQL (libre, gratuit, plus puissant que MySQL mais moins connu et donc moins utilisé) 
* SQLite (un seul fichier, de la merde dès que un peu d'informations) 
* Oracle (le plus complet mais très cher) 
* Microsoft SQL Server (payant, quand on bosse avec du .NET) 

Donner des ordres au SGBD en langage SQL 
* Le langage SQL est un standard (plutot une bonne nouvelle) 
* Mais chaque SGBD implémente des variantes -> ... donc quand meme un peu la merde 

Exemple de requete SQL 
```
SELECT id, auteur, message, datemsg FROM livreor ORDER BY datemsg DESC
```

PHP fait la jonction entre le développeur et le SGBD 
* client <-> serveur <-> PHP <-> MySQL 

### Structure d'une base de données 
Vocabulaire 
* La base de données (comprend toutes les informations) 
* BD > Tables 
* Table est composée de colonnes (= les champs) et de lignes (des entrées) 

### Mais où sont enregistrées les données 
Dans des fichiers (sans blague...) 
Ne jamais essayer de les lire ou de les modifier directement : toujours passer par le biais de MySQL 

## phpMyAdmin 
Index 
* Créer une table 
* Modifier une table 
* Autres opérations 

### Créer une table 
En gros y a un +  dans l'interface graphique 
* on lui donne un nom 
* on spécifie le nombre de champs (pour le test id, titre, contenu) voir plus loin 
On doit alors donner des informations sur les champs 
* Champ (définition du nom du champs) 
* Type (ex int, date etc)
* Taille/valeurs -> surtout utile la taille maximale du champs dans le cas de VARCHAR (les petits textes) 
* Index -> Primary pour la clef primaire (NB une clef primaire d'identifier chaque entrée sans équivoque et de manière unique) ssi 
* Auto-increment : surtout pour id (que l'on prend souvent pour les clefs primaires) 

Les types de champs (catégories)
* NUMERIC (TINYINT et BIGINT) pour les plus utiles 
* DATE et TIME 
* STRING 
* SPATIAL 

Les types les plus courants 
* INT 
* VARCHAR (1 à 255 caractères)
* TEXT 
* DATE 

**Les clefs primaires**
Un champ (ou groupe de champs) qui permet d'identifier toutes les entrées de manière unique. 

### Modifier une table 
On clique sur la table de la base de données. --> on voit la structure de la base de données 

On peut modifier la structure (ajouter ou supprimer des liens) ... dans l'onglet structure 

On peut ajouter des valeurs par entrée du coup (dans la section valeur) 

### Autres opérations 
Index 
* SQL 
* Importer 
* Exporter 
* Opérations 
* Vider 
* Supprimer 

**SQL**
permet d'écrire une requete en SQL et ainsi de voir son effet en live 
Ex : ``` SELECT * FROM 'news' WHERE 1 ``` 

**Importer**
Fichiers d'importation : CSV, SQL --> (on va chercher un dossier avec des requetes SQL sur notre DD)

**Exporter**
Le fichier que l'on exporte n'est pas la DB mais un fichier qui permet de la reconstruire 

Utilités (prendre la structure et les données dans les options)
* Transmettre la BD sur internet 
* Faire un backup 

**Opérations**
* changer le nom de la table 
* déplacer la table vers (table dans une autre base de données) 
* copier la table : dans la meme ou une autre BD 
* optimiser la table : surtout quand elle grandit -> RR de perte de performances -> sélectionner cet onglet 

**Vider**
Seule la structure reste (pas les données) 

**Supprimer**
Suppression de la totalité de la table (structure et données) 

## Lire des données 
Index 
* Se connecter à la base de données 
* Récupérer les données 
* Les critères de sélection 
* Construire des requetes en fonction des variables 
* Traquer les erreurs 

### Se connecter à la base de données 

### Récupérer les données 
### Les critères de sélection 
### Construire des requetes en fonction des variables 
### Traquer les erreurs 

## Ecrire des données 
## TP : un mini-chat 
## Les fonctions SQL 
## Les dates en SQL 
## TP : un blog avec des commentaires 
## Jointures entre tables 