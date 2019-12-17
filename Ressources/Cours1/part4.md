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
**Comment se connecter à la base de données**
* extension mysql_ --> seulement avec mySQL fonctions vieilles (bref non) 
* extension mysqli_ --> seulement MySQL mais fonctions plus récentes 
* extension pdo --> toutes les bases de données (bref le choix est assez simple) , en plus tend a etre de + en + utilisée 

**Activer pdo (Note extension orientée)**
* left clic on wamp (bref il va falloir que je cherche pour lampp)
* dans le cas de lampp -> chercher php.ini de nouveau et pdo_mysql.default_socket = /opt/lampp/var/mysql/mysql.sock

**Se connecter à MySQL avec PDO** 
Besoin de 4 arguments 
* nom d'hote (pour l'instant localhost) = DSN (Data Source Name)
* le nom de la base de données 
* le login 
* le mot de passe 

```
$bdd = new PDO('mysql:host=localhost;dbname=test;charset=utf8', 'root', 'root'); //dans mon cas faudra pe que je recommence l'installation 
```

**Tester la présence d'erreurs**
* si on se plante dans la ligne plus haut -> PHP rr de mettre une ligne d'erreur avec le mot de passe lol 
Donc on va plutot faire 
```
<?php
try {
    $bdd = new PDO('mysql:host=localhost;dbname=test;charset=utf8', 'root', 'root');
}
catch (Exception as $e) {
    die('Erreur : ' . $e->getMessage()); 
}
?>
```

### Récupérer les données 
Dl une table -> aller dans la base test et importer la table concernée 
Structure de la table (les champs)
* id (primary) integer and auto-increment 
* nom (var char) 
* possesseur (varchar) 
* console (varchar) 
* prix (int pe  un TINYINT)
* nb_joueurs max  (idem je suppose)
* commentaires (string)

```
$reponse = $bdd->query('Taper une requete SQL'); 
$reponse = $bdd->query('SELECT * FROM jeux_video'); //par exemple 
```

```
SELECT * FROM jeux_video
// assez évident 
// SELECT nom, possesseur FROM jeux_video; 
```

Afficher le résultat d'une requete 
```
$donnees = $reponse->fetch(); //donnees est un array de la première entrée -> $donnees['console'] affiche le champ console de la première entrée 
```

A chaque fois 
```
$donnees = $reponse->fetch(); //on passe à l'entrée suivante et on récupère sous forme d'array (rien de nouveau) 
```

```
while ($donnees = $reponse->fetch()) //super puissant donc pour aller voir chacune des entrées , renvoie false quand plus rien 
{
?>
    <p>
    <strong>Jeu</strong> : <?php echo $donnees['nom']; ?><br />
    Le possesseur de ce jeu est : <?php echo $donnees['possesseur']; ?>, et il le vend à <?php echo $donnees['prix']; ?> euros !<br />
    Ce jeu fonctionne sur <?php echo $donnees['console']; ?> et on peut y jouer à <?php echo $donnees['nbre_joueurs_max']; ?> au maximum<br />
    <?php echo $donnees['possesseur']; ?> a laissé ces commentaires sur <?php echo $donnees['nom']; ?> : <em><?php echo $donnees['commentaires']; ?></em>
   </p>
<?php
}

$reponse->closeCursor(); // Termine le traitement de la requête , on parle bien de la requete donc 
?>
```

```
$reponse = $bdd->query('SELECT nom FROM jeux_video');

while ($donnees = $reponse->fetch())
{
	echo $donnees['nom'] . '<br />';
}

$reponse->closeCursor();
```

### Les critères de sélection 
Index 
* WHERE 
* ORDER BY 
* LIMIT 

**WHERE**
```
SELECT * FROM jeux_video WHERE possesseur = 'Patrick' //'quotes required for strings but not for number
```

```
$reponse = $bdd->query('SELECT nom, possesseur FROM jeux_video WHERE possesseur=\'Patrick\''); //échappement 
```

Combiner les conditions 
```
SELECT * FROM jeux_video WHERE possesseur='Patrick' AND prix < 20 //verus OR 
```

**ORDER BY**
```
$reponse = $bdd->query('SELECT nom, prix FROM jeux_video ORDER BY prix'); //par prix croissant du coup 
```

```
$reponse = $bdd->query('SELECT nom, prix FROM jeux_video ORDER BY prix DESC'); //par prix décroissant
```

**LIMIT**
Permet de ne sélectionner qu'une partie des résultats (ex les 20 premiers)

```
SELECT * FROM jeux_video LIMIT 0,20 //affiche les 20 premières entrées 
SELECT * FROM jeux_video LIMIT 5,10 //affiche de la 6° à la 15° entrée donc premier inclus puis nb entrées ... 
```

**Exemple un peu plus complet**
```
SELECT nom, possesseur, console, prix FROM jeux_video WHERE console='Xbox' OR console='PS2' ORDER BY prix DESC LIMIT 0,10
```

### Construire des requetes en fonction des variables 
**Mauvaise idée**
```
$reponse = $bdd->query('SELECT nom FROM jeux_video WHERE possesseur=\'' . $_GET['possesseur'] . '\'');
```
Pourquoi il ne faut pas lee faire 
* parce que du coup on utilise directement $_GET['possesseur'] -> RR de sécurité -> RR injection SQL 

**Bonne idée : les requetes préparées**
Bonne idée parce que 
* plus secure 
* plus rapide aussi 

```
$req = $bdd->prepare('SELECT * FROM jeux_video WHERE possesseur = ?'); 
$req->execute(array($_GET['possesseur'])); 
```

Si plusieurs paramètres 
```
$req = $bdd->prepare('SELECT nom FROM jeux_video WHERE possesseur = ? AND prix <= ?');
$req->execute(array($_GET['possesseur'], $_GET['prix_max']));
```

**En pratique**
```
$req = $bdd->prepare('SELECT nom, prix FROM jeux_video WHERE possesseur = ?  AND prix <= ? ORDER BY prix');
$req->execute(array($_GET['possesseur'], $_GET['prix_max']));

echo '<ul>';
while ($donnees = $req->fetch())
{
	echo '<li>' . $donnees['nom'] . ' (' . $donnees['prix'] . ' EUR)</li>';
}
echo '</ul>';

$req->closeCursor(); //évidemment il faut tout de meme faire toutes les vérifications d'usage (type etc) 
```

**Avec des marqueurs nominatifs**
Surtout quand la requete comprend beaucoup de variables ... sinon on ne sait plus ce que la requete veut dire (passer par tableau associatif)

```
<?php
$req = $bdd->prepare('SELECT nom, prix FROM jeux_video WHERE possesseur = :possesseur AND prix <= :prixmax');
$req->execute(array('possesseur' => $_GET['possesseur'], 'prixmax' => $_GET['prix_max'])); //je vais toujours utiliser cette syntaxe franchement mieux
?>
```

### Traquer les erreurs 
Si on n'execute pas le code plus bas on aura des erreurs sans signification à priori ... 
```
$bdd = new PDO('mysql:host=localhost;dbname=test;charset=utf8', 'root', '', array(PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION));
```

## Ecrire des données 
Index 
* INSERT : ajouter des données 
* UPDATE : modifier les données 
* DELETE : supprimer les données 
* Traiter les erreurs SQL 

### INSERT : ajouter des données 
```
INSERT INTO jeux_video(ID, nom, possesseur, console, prix, nbre_joueurs_max, commentaires) VALUES ('', 'Battlefield', 'Patrick', 'PC', 42, 50, 'WW2'); 
```
On peut aussi se passer du nom des champs mais moins clair (mais ... plus court!)
```
INSERT INTO jeux_video VALUES('', 'Battlefield 1942', 'Patrick', 'PC', 45, 50, 
	 '2nde guerre mondiale')
```

**Avec PHP**
``` 
try {
    $bdd = new PDO('mysql:host=localhost;dbname=test;charset=utf8', 'root', '');
} catch (Exception $e) {
    die('Erreur : ' . $e->getMessage()); 
}
$bdd->exec('INSERT INTO jeux_video(nom, possesseur, console, prix, nbre_joueurs_max, commentaires) VALUES(\'Battlefield 1942\', \'Patrick\', \'PC\', 45, 50, \'2nde guerre mondiale\')');
```
On peut se passer du champs ID car auto-increment 
**Avec une requete préparée**
```
Après essai à la connexion à la bdd 
$req = $bdd->prepare('INSERT INTO jeux_video(nom, possesseur, console, prix, nbre_joueurs_max, commentaires) VALUES(:nom, :possesseur, :console, :prix, :nbre_joueurs_max, :commentaires)');
$req->execute(array(
	'nom' => $nom,
	'possesseur' => $possesseur,
	'console' => $console,
	'prix' => $prix,
	'nbre_joueurs_max' => $nbre_joueurs_max,
	'commentaires' => $commentaires
	));
```

### UPDATE : modifier les données 
```
UPDATE jeux_video SET prix = 10, nbre_joueurs_max = 32 WHERE ID = 51
UPDATE jeux_video SET prix = '10', nbre_joueurs_max = '32' WHERE nom = 'Battlefield 1942' //autre manière de sélectionner
UPDATE jeux_video SET possesseur = 'Florent' WHERE possesseur = 'Michel' //Florent rachete tous les jeux de Michel
```

**Avec PHP**
```
$nb_modifs = $bdd->exec('UPDATE jeux_video SET prix = 10, nbre_joueurs_max = 32 WHERE nom = \'Battlefield 1942\''); //récupération nb_modifs optionnel 
```

**Avec une requete préparée**
```
$req = $bdd->prepare('UPDATE jeux_video SET prix = :nvprix, nbre_joueurs_max = :nv_nb_joueurs WHERE nom = :nom_jeu');
$req->execute(array(
	'nvprix' => $nvprix,
	'nv_nb_joueurs' => $nv_nb_joueurs,
	'nom_jeu' => $nom_jeu
	));
```

### DELETE : supprimer les données 
```
DELETE FROM jeux_video WHERE nom='Battlefield 1942'
de nouveau on peut utiliser des requetes préparées et aussi avec ->execute ou ->exec si en direct 
```

### Traiter les erreurs SQL 
Il faut paramétrer PHP pour afficher les erreurs de SQL 
* ex Fatal error : Call to a member function fetch() on a non-object 

Comment faire 
```
$reponse = $bdd->query('SELECT nom FROM jeux_video') or die(print_r($bdd->errorInfo())); -> précise la ligne de l'erreur SQL ... 
```
## TP : un mini-chat 
Voir mon fichier annexe : je vais le faire comme projet perso 
De gros problèmes avec son truc -> faut utiliser ajax sinon de la grosse merde 

## Les fonctions SQL 
Apparemment son très utilisées 
2 types de fonctions 
* Les fonctions scalaires : agissent sur chaque entrée 
* Les fonctions d'agrégat : calculs sont faits sur ensemble de la table pour renvoyer une valeur (ex prix moyen) 

### Les fonctions scalaires 
Index 
* Liste des fonctions scalaires utiles 
* Illustration avec la fonction UPPER 

**Liste des fonctions scalaires utiles**
* UPPER 
* LOWER
* LENGTH --> ex SELECT LENGTH(nom) AS longueurNom FROM jeux_videos; 
* ROUND --> ex SELECT ROUND(prix, 2) AS prix_precision2 FROM jeux_videos; 
* [fonctions mathématiques](https://dev.mysql.com/doc/refman/8.0/en/numeric-functions.html)
* [fonctions chaines de caractères](https://dev.mysql.com/doc/refman/8.0/en/string-functions.html)

**Illustration avec la fonction UPPER**
Ne modifient pas la table mais renvoient un champ personnalisé (que l'on peut nommé avec AS) 

```
SELECT UPPER(nom) AS nom_perso FROM jeux_videos; 
```
Exemple 
```
SELECT UPPER(nom) FROM jeux_videos; 
SELECT UPPER(nom) AS nom_maj, possesseur, console, prix FROM jeux_video
```

Avec du PHP 
```
$reponse = $bdd->query('SELECT UPPER(nom) AS nom_maj FROM jeux_video');
while ($donnees = $reponse->fetch())
{
	echo $donnees['nom_maj'] . '<br />';
}
$reponse->closeCursor();
```

### Les fonctions d'agrégat 
Index 
* Liste des fonctions utiles 
* Illustration avec la fonction AVG 
**Liste des fonctions utiles**
* AVG 
* SUM 
* MAX 
* MIN 
* COUNT -> ex SELECT COUNT(*) as nbJeux FROM jeux_videos; ou encore SELECT COUNT(*) as nbJeuxOlivier FROM jeux_videos WHERE possesseur='Olivier'; 
* SELECT COUNT(DISTINCT possesseur) AS nbpossesseurs FROM jeux_video

**ne pas récupérer d'autres champs en meme temps -> pas de sens !**

**Exemple avec la fonction AVG**
Exemple : calcul de la moyenne 
SELECT AVG(prix)  AS prix_moyen from jeux_video; 
```
	echo $donnees['prix_moyen']; //utilisation de notre champ personnalisé au préalable un fetch unique sur $reponse
```

Exemple avec un filtre supplémentaire (rien de bien nouveau) 
```
SELECT AVG(prix) AS prix_moyen FROM jeux_video WHERE possesseur='Patrick'
```
### GROUP BY et HAVING : groupement des données 
**GROUP BY**
```
SELECT AVG(prix) AS prix_moyen, console FROM jeux_video GROUP BY console
```
**HAVING**
```
SELECT AVG(prix) AS prix_moyen, console FROM jeux_video GROUP BY console HAVING prix_moyen <= 10
```
```
SELECT AVG(prix) AS prix_moyen, console FROM jeux_video WHERE possesseur='Patrick' GROUP BY console HAVING prix_moyen <= 10
```
## Les dates en SQL 
Index 
* Les champs de type date 
* Les fonctions de gestion des dates 

### Les champs de type date 
Les différents types de dates 
* DATE : date au format YYYY-MM-DD
* TIME : HH:mm:ss
* DATETIME : YYYY-MM-DD HH-mm-ss
* TIMESTAMP : le nb de secondes (pas ms comme en js)
* YEAR : YYYY 

Créer un champ dateCreation de type DATETIME -> le mettre à current_timestamp

Utilisation des champs de type date 
```
SELECT pseudo, message, date FROM minichat WHERE date = '2010-04-02' //si de type DATE
SELECT pseudo, message, date FROM minichat WHERE date = '2010-04-02 15:28:22 // si type DATETIME
SELECT pseudo, message, date FROM minichat WHERE date >= '2010-04-02 15:28:22'
SELECT pseudo, message, date FROM minichat WHERE date >= '2010-04-02 00:00:00' AND date <= '2010-04-18 00:00:00'
SELECT pseudo, message, date FROM minichat WHERE date BETWEEN '2010-04-02 00:00:00' AND '2010-04-18 00:00:00'
```

### Les fonctions de gestion des dates 
[Toutes les fonctions](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html)
Index ce celles présentées 
* NOW() 
* DAY(), MONTH(), YEAR() 
* HOUR(), MINUTE(), SECOND() 
* DATE_FORMAT 
* DATE_ADD et DATE_SUB 

**NOW**
```
INSERT INTO minichat(pseudo, message, date) VALUES('Mateo', 'Message !', NOW()) insertion d'un champ avec la date (date-time ou time) actuelle
```
Pour plus de précision (disons surtout moins ambiguité) -> CURDATE et CURTIME

**DAY etc**
chiant à mourir j'y reviens dans un second temps 
C'est le moment : à la soupe mon petit gars 
```
SELECT pseudo, message, DAY(date) AS jour FROM minichat
```

**DATE_FORMAT**
```
SELECT pseudo, message, DAY(date) AS jour, MONTH(date) AS mois, YEAR(date) AS annee, HOUR(date) AS heure, MINUTE(date) AS minute, SECOND(date) AS seconde FROM minichat
```
```
<?php
echo $donnees['jour'] . '/' . $donnees['mois'] . '/' . $donnees['annee'] . '...';
?>
```

On peut faire beaucoup plus court et propre : 
```
SELECT pseudo, message, DATE_FORMAT(date, '%d/%m/%Y %Hh%imin%ss') AS date FROM minichat
```

**DATE_ADD et DATE_SUB** 
Comme le nom l'indique pour + ou - des dates 
```
SELECT pseudo, message, DATE_ADD(date, INTERVAL 15 DAY) AS date_expiration FROM minichat
```
## TP : un blog avec des commentaires (voir mes fichiers annexes)

## Jointures entre tables 
Index 
* Modélisation d'une relation 
* Qu'est-ce que c'est une jointure ? 
* Les jointures internes 
* Les jointures externes 

### Modélisation d'une relation 
Exemple 
* Une table qui comprend ID, nom, prénom, numéro de téléphone (table proprietaires i.e)
* Une table jeux_videos qui comprend ID (son id qui est différent), nom, IDProprietaire, console, prix, nbJoueurs, commentaire
[Exemple](https://user.oc-static.com/files/222001_223000/222281.png)

Pour relier les 2 tables il faut effectuer une jointure 
Note perso : pour ceux qui ont remarqué, on peut tout à fait utiliser UML pour modéliser les tables et leurs dépendances/jointures 

### Qu'est-ce que c'est une jointure ? 
Deux types majeurs de jointure 
* jointures internes -> ne sélectionnent que les données **qui ont une correspondance** entre les 2 tables 
* jointures externes -> sélectionnent toutes les données (sans correspondance obligatoire)

Illustration de la différence entre les 2 jointures : 

Si on ajoute une entrée [Chuck, Norris , numTel] dans la table proprietaires , ce dernier n'ayant aucun jeux vidéo -> pas dans la table jeux_videos 
* Avec une jointure interne : Chuck n'est pas dans le résultat de la requete $
* Avec une jointure externe : Chuck est bien présent (mais sa valeur pour jeux est évaluée à NULL)

Note perso : je ne vois pas très bien le but des jointures externes (sauf pour comparaison avec des jointures internes)

### Les jointures internes 
On peut l'effectuer de 2 manières 
* WHERE (ancienne syntaxe, destinée à disparaitre) je la décrit juste pour info (si on doit faire de la maintenance) 
* JOIN (Nouvelle, plus efficace, plus lisible, pour la paix dans le monde)

**WHERE**
Pour des infos sur les noms complets (ici pas nom mais jeux_video.nom car sinon champ ambigu)
```
SELECT jeux_video.nom, proprietaires.prenom //aucune ambiguite , pas d'ordre donc de citation des tables 
FROM proprietaires, jeux_video 
WHERE jeux_video.ID_proprietaire = proprietaires.ID //précision de la correspondance attendue des entrées 
```

Utiliser des alias (bonne pratique quand on fait des jointures, - logique)
```
SELECT jeux_video.nom AS nom_jeu, proprietaires.prenom AS prenom_proprietaire
FROM proprietaires, jeux_video
WHERE jeux_video.ID_proprietaire = proprietaires.ID 
```

On peut aussi donner des alias aux noms de tables (? bonne pratique ? on perd beaucoup en lisibilité je trouve mais + court ...)
```
SELECT j.nom AS nom_jeu, p.prenom AS prenom_proprietaire
FROM proprietaires AS p, jeux_video AS j
WHERE j.ID_proprietaire = p.ID
```

Remarque (mais de nouveau je ne le conseille pas on peut omettre le AS et le remplacer par un espace)

**JOIN**
```
SELECT j.nom nom_jeu, p.prenom prenom_proprietaire
FROM proprietaires p
INNER JOIN jeux_video j
ON j.ID_proprietaire = p.ID
```

Donc en dehors de ces alias abominables on voit que le principe est clair 
* sélection des données de sortie (comme d'habitude)
* depuis une table principale (ici FROM table propriétaires)
* vers une table secondaire (ici jeux_video)
* ON critère de coincidence 

On peut toujours filtrer les données dans un second temps (après la jointure) 
```
SELECT j.nom nom_jeu, p.prenom prenom_proprietaire
FROM proprietaires p
INNER JOIN jeux_video j
ON j.ID_proprietaire = p.ID
WHERE j.console = 'PC'
ORDER BY prix DESC
LIMIT 0, 10
```

On décortique encore 
* sélection des données de sortie (les champs)
* depuis une table principale 
* vers une table secondaire 
* avec une clause de réunion 
* avec une condition (console = PC), affichage par prix décroissant et seulement des 10 premiers champs 

### Les jointures externes 
2 écritures à connaitre 
* LEFT JOIN 
* RIGHT JOIN 

**LEFT JOIN** 

```
SELECT j.nom nom_jeu, p.prenom prenom_proprietaire
FROM proprietaires p
LEFT JOIN jeux_video j
ON j.ID_proprietaire = p.ID
```
Donc 
* sélection des champs de sortie 
* depuis table principale (propriétaire) que l'on va garder en entierete (confer LEFT JOIN -> LEFT main)
* vers table jeux_video 
* ON critère de réunion (sachant que non restrictif dans ce cas -> RR que le champ nom de jeux_video soit NULL)

**RIGHT JOIN**
cette fois on garde toutes les données de la table de droite 
ex 
```
SELECT j.nom nom_jeu, p.prenom prenom_proprietaire
FROM proprietaires p
RIGHT JOIN jeux_video j
ON j.ID_proprietaire = p.ID
```
Donc 
* sélection des champs de sortie 
* depuis la table principale (mais qui ne sera pas prise en totalité)
* vers la table jeux_video qui sera prise en entiereté 
* ON critères de réunion (sachant que non restrictif -> Risque d'avoir un prénom évalué à NULL donc dans ce cas) 