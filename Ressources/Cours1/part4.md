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

## Les fonctions SQL 
## Les dates en SQL 
## TP : un blog avec des commentaires 
## Jointures entre tables 