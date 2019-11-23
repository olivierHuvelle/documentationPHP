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
## Lire des données 
## Ecrire des données 
## TP : un mini-chat 
## Les fonctions SQL 
## Les dates en SQL 
## TP : un blog avec des commentaires 
## Jointures entre tables 