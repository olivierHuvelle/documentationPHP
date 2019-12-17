# Concepts de base 
* Introduction aux bases de données 
* SELECT 
* Syntaxe des règles SQL 
* Séleectionner plusieurs colonnes 
* DISTINCT et LIMIT 
* Ordonner les résultats 

## Introduction aux bases de données 
Une base de données est une collection ordonnée de données qui permet de faciliter (et + rapidité) accès et modification des données. 
Base de données > table(s) > champs et entrées 
* les champs sont les colonnes de la table (nombre fini)
* les entrées sont les lignes de la table (nombre virtuellement infini enfin pour google quoi ...)

Les clefs primaires 
* Est un champ (ou un groupe de champs) qui permet d'identifier (donc ssi -> existe toujours et toujours un seul) champ 
* Donc forcément valeur unique 
* Donc forcément valeur qui existe (>< NULL)

Qu'est-ce que SQL ? 
* SQL = Structured Query Langage 
* SQL est un langage qui permet de manipuler les bases de données , ANSI mais pas si standard que ça en réalité 
* MySQL est un SGBD (càd un logiciel) qui permet d'utiliser SQL 

## SELECT 
**SHOW**
La commande SHOW (que je ne comprends pas vraiment, structure de la BD ? versus la totalité ? je pense que montre les entrées de ce qui est électionné)
```
En effet ex : 
SHOW COLUMNS FROM customers 
```

La commande plus haut renvoie donc les colonnes de la table customers 
* chaque ligne affichée correspond donc à un "titre" avec structure [nom, type_donnée, NULL?, Key(primary?), Default, Extra]    


**SELECT**
SELECT column_list FROM table_name 

```
SELECT firstName FROM customers
```

## Syntaxe des règles SQL 
Chaque règle se termine  par un ; (ce qui manquait plus haut par ailleurs mais qui ne pechait pas car une seule requete) 
```
SELECT firstName FROM customers; 
```

SQL est case insensitive mais la pratique veut que **les commandes SQL en majuscules**

Syntaxe 
* Une règle peut s'écrire sur une ou plusieurs lignes (et l'inverse est vrai aussi)
* Les whitespaces sont ignorés (mais **recommandation de les réduire au maximum**)


## Séleectionner plusieurs colonnes 
SELECT firstName, lastName FROM customers; //lui le divise en 2 lignes et on gagne en lisibilité 
```
SELECT firstName, lastName 
FROM customers; 
```

**Sélectionner toutes les colonnes**
SELECT * FROM customers; 

## DISTINCT et LIMIT 
**DISTINCT**
Dans le cas où l'on possède des doublons dans la base de données (des doublons de valeurs dans un champ bien précis sinon impossible confer PRIMARY)
```
SELECT DISTINCT City FROM Customers; //on n'affiche pas les doublons dans les villes 
```
**LIMIT**
```
SELECT column_list
FROM table_name 
LIMIT [number_of_records]; //par défaut limit pas précisé -> tous les champs qui collent à la requete (logique d'ailleurs) 
```

Exemple 
```
SELECT id, firstName, city 
FROM curstomers 
LIMIT 5; //va prendre les 5 premières entrées (avec les champs sélectionnés)
```

Exemple 2 
```
SELECT id, firstName, city 
FROM customers 
LIMIT 3,4; //va prendre 4 entrées à partir de la  troisième (incluse) NB ID commence à 0 
```

## Ordonner les résultats 
**Nom complet des champs**
Le nom complet des champs (que je trouve assez inutile pour le moment mais si on basse avec plusieurs tables pq pas)
```
SELECT city 
FROM customers;
```
Equivaut donc à 
```
SELECT customers.city 
FROM customers; 
```

**ORDER BY**
```
SELECT * 
FROM customers 
ORDER BY firstName; //par défaut par ordre alphabétique donc sur base du champs prénom 
```

**Trier plusieurs colonnes**
```
SELECT * 
FROM customers 
ORDER BY lastName, age; //va trier sur base alpha nom de famille, si 2 éléments ont le meme nom de famille selon age croissant 
```

