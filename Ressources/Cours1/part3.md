# Transmettre les données de pages en pages 
## Avec l'URL 
Index 
* Envoyer des paramètres avec l'URL 
* Récupérer les paramètres en PHP 
* Ne **jamais** faire confiance aux données 

### Envoyer des paramètres avec l'URL 
On va créer un lien qui envoie des données à la page de réception 
```
<a href="bonjour.php?nom=Dupont&amp;prenom=Jean">Dis-moi bonjour !</a>
```
On voit donc la structure de base 
* url.extension 
* ? 
* arg1 = val1
* &amp; (le amp; pour la validation du W3)
* arg2 = val2 etc etc 

Personnellement je n'aime pas trop cette technique 
* Les données transitent par l'URL donc trop de transparence 
* Limitation à 256 caractères 

### Récupérer les paramètres en PHP 
Nous somme dans la page de réception ... 

```
$_GET['nom'] //vaut 'Dupont' 
$_GET['prenom'] //vaut 'Jean'
```

On voit donc directement qu'une variable superglobale de type array (associatif) $_GET est créée 

### Ne **jamais** faire confiance aux données recues 
Grosso modo ce qu'il va falloir faire (aussi avec les formulaires d'ailleurs)
* Vérifier l'existence de la variable 
* Vérifier le type de la variable (faire un transtypage) 
* Vérifier la cohérence de la variable (valeur)
Bref un peu chiant de devoir le faire à chaque fois je vais me faire une fonction pour le tout à terme 

**Vérifier l'existence de la variable**
```
if(isset($_GET['parametre'])){
    //code si la variable existe 
}else{
    //code si la variable n'existe pas
}
```

**Transtypage**
Surtout utile quand on attend autre chose qu'une chaine 
```
$_GET['parametre'] = (int) $_GET['parametre']; //si chaine alors 0 un peu mal foutu je trouve 
```

**Vérifier la cohérence de la variable**
EN gros éviter que l'utilisateur de mettre 150 ans par exemple ... 

## Avec les formulaires 
Index 
* La base du formulaire 
* Eléments de formulaire 
* Ne jamais faire confiance aux données 
* Envoi de fichiers (section à part)

### La base du formulaire 
Un peu de rappel de HTML (nb for lien avec id et name identifiant pour page2.php)
```
<form method = 'post' action = 'page2.php'>
<p>
    <label for = 'pseudo'>Votre pseudo : </label><input type = 'text' id = 'pseudo' name = 'pseudo'/><br/>
    <input type = 'submit' value = 'envoyer'/>
</p>
</form>
```
Dans la page2.php
```
$_POST['pseudo'] aura la valeur donnée ... 
```

### Eléments de formulaire 
Pour les input type = 'text' et les textarea ... confer supra 

**Pour les listes déroulantes :**
HTML 
```
<select id = 'pays' name = 'pays'>
    <option value = 'France'/>
    <option value = 'Belgique'/>
    <option value = 'Allemagne'/>
</select>
```
PHP 
```
$_POST['pays'] aura la valeur de l'option cochée ... 
```

**Les cases à cocher**
HTML
```
<label for = 'frites'>Frites</label> <input type = 'checkox' name = 'frites' id = 'frites'/>
<label for = 'steack'>Steack</label> <input type = 'checkox' name = 'steack' id = 'steack'/>
```
PHP
```
si la case a été cochée -> $_POST['frites'] vaut on sinon n'existe pas --> isset($_POST['frites'])
```
**Les boutons radio**
HTML 
```
<input type="radio" name="frites" value="oui" id="oui" checked="checked" /> <label for="oui">Oui</label>
<input type="radio" name="frites" value="non" id="non" /> <label for="non">Non</label>
```
PHP 
```
$_POST['frites'] aura la valeur (value) cochée ex oui 
```
**Les champs cachés**
Personnellement je ne vois pas trop l'utilité (il y a les sessions et les coookies pour le faire) 

HTML
```
<input type="hidden" name="pseudo" value="Mateo21" />
```

PHP 
```
$_POST['pseudo'] aura la valeur de sa value ... mais attention ne jamais faire confiance aux données recues 
```

### Ne jamais faire confiance aux données 
On ne peut jamais certifier l'origine du formulaire 

Faille XSS : (cross-site crypting) consiste à injecter du code HTMLcontenant du JS dans les pages pour faire exécuter 
Permet entre autre de récupérer les infos confidentielles d'un compte sur le site en question 

Il faut donc échapper le code HTML 
```
<?php echo htmlspecialchars($_POST['prenom']);> : aussi la fonction strip_tags($_POST['nom_variable'])
```
### Envoi de fichiers (section à part)

Le fichier est envoyé dans un dossier temporaire du serveur 
Le fichier possède quelques propriétés importantes 
* $_FILES['monFichier']['name'] : le nom du fichier 
* $_FILES['monFichier']['type'] : type du fichier ex image/gif 
* $_FILES['monFichier']['size'] : La taille du fichier en octets (par défaut KO si > 8Mo) 
* $_FILES['monFichier']['tmp_name'] : emplacement temporaire du fichier 
* $_FILES['monFichier']['error'] : 0 si pas erreur 

HTML 
```
<form action="cible_envoi.php" method="post" enctype="multipart/form-data">
        <p>
                Formulaire d'envoi de fichier :<br />
                <input type="file" name="monfichier" /><br />
                <input type="submit" value="Envoyer le fichier" />
        </p>
</form>
```

Code PHP 
```
<?php
//Attention ce script n'est pas du tout assez complet --> il faut l'améliorer (avec Symfony)
if (isset($_FILES['monfichier']) AND $_FILES['monfichier']['error'] == 0) //si fichier bien envoyé (recu on se comprend ;) 
{
    if ($_FILES['monfichier']['size'] <= 1000000) //vérification de la taille du fichier 
    {
        $infosfichier = pathinfo($_FILES['monfichier']['name']);
        $extension_upload = $infosfichier['extension'];
        $extensions_autorisees = array('jpg', 'jpeg', 'gif', 'png');
        if (in_array($extension_upload, $extensions_autorisees)) //vérification de l'extension du fichier 
        {
            // On peut valider le fichier et le stocker définitivement
            move_uploaded_file($_FILES['monfichier']['tmp_name'], 'uploads/' . basename($_FILES['monfichier']['name']));
            echo "L'envoi a bien été effectué !";
        }
    }
}
?>
```
## TP : page protégée par un mot de passe 
```
<!DOCTYPE html>
<html>
    <head>
        <title>Tests en PHP</title>
        <meta charset = 'utf-8'/>
    </head>
    <body>
        <h1>Bonjour tout le monde</h1>
        <?php
            if (!isset($_POST['mdp'])|| $_POST['mdp'] != "kangourou"){
        ?> 
            <form method = 'post' action = 'index.php'>
                <p>
                    <label for = 'mdp'>Mot de passe : </label><input type = 'password' name = 'mdp' id = 'mdp'/> 
                </p>
            </form>
        <?php
            }
            else{
        ?>
                <h1>Bonjour tout le monde</h1>
        <?php        
            }
        ?>

    </body>
</html>
```

## Variables superglobales 
**Informations générales : syntaxe**
* commencent par _ 
* nom en MAJUSCULES 
* sont des tableaux associatifs 
* sont créées automatiquement par PHP à chaque fois qu'une page est chargée --> ex print_r($_POST)

**Variables superglobales**
* $_SERVER : $_SERVER['REMOTE_ADDR'] -> adresse IP du client qui a demandé à voir la page.

* $_ENV : variables d'environnement données par le serveur. plutot pour le sysAdmin

* $_SESSION : variables de session. 

* $_COOKIE : valeurs des cookies enregistrés sur l'ordinateur du visiteur.

* $_GET : confer supra.

* $_POST : confer supra. 

* $_FILES : Liste des fichiers envoyés formulaire. 

## Sessions et cookies 
### Sessions
Fonctionnement des sessions 
* Visiteur arrive sur le site, on demande une session -> création d'un ID : PHPSESSID (transmet cet identifiant de page en page)
* On peut créer une infinité de variables de session ex $_SESSION['nom']
* Le visiteur quitte le site (ou de déconnecte) : on ferme la session (souvent via un timeout) 

2 fonctions 
* session_start() //à mettre au tout début de chaque page qui va utiliser les sessions 
* session_destroy() , on peut l'appeler via un bouton déconnexion (ou automatiquement via timeout)

Exemple utilisation de sessions : page 1 (qui génère les données)
```
<?php
session_start();
// On s'amuse à créer quelques variables de session dans $_SESSION
$_SESSION['prenom'] = 'Jean'; //on peut déclarer des variables de session partout dans le code 
$_SESSION['nom'] = 'Dupont';
$_SESSION['age'] = 24;
?>
<!DOCTYPE html>
//code html 
```

Exemple utilisation de sessions : page 2 (qui recoit les données) 
```
<?php
session_start(); 
?>
Je me souviens de toi ! Tu t'appelles <?php echo $_SESSION['prenom'] . ' ' . $_SESSION['nom']; ?> !<br />
```

### Cookies
Index 
* Définition du cookie 
* Ecrire un cookie 
* Récupérer le contenu d'un cookie 

**Définition** 
Est un petit fichier que l'on enregistre chez le client (petits fichiers texte) 

Un cookie est stocke généralement une seule information (ex pseudo)

Stockage dans endroit du navigateur (me regarde pas tellement)

**Ecrire un cookie**
3 paramètres 
* son nom 
* sa valeur 
* sa date d'expiration (sous forme de timestamp) 

Ecrire un cookie 
```
<?php setcookie('pseudo', 'Soucoupe', time()+365*24*3600);  ?>
```

Sécuriser son cookie avec le mode httpOnly 
```
<?php setcookie('pseudo', 'Soucoupe', time()+365*24*3600, null, null, false, true);  ?>
Le dernier paramètre permet d'activer le mode httpOnly 
```

Utilisation en pratique 
```
<?php
    setcookie('nom', 'valeur', timestamp, null, null, false, true); 
    setcookie('nom2', 'valeur2', timestamp2, null, null, false, true);
?>
<!DOCTYPE html>
<html>
    code HTML
</html>
```

**Récupérer le contenu d'un cookie**
Afficher un cookie 
```
<p>
    Hé ! Je me souviens de toi !<br />
    Tu t'appelles <?php echo $_COOKIE['pseudo']; ?> et tu viens de <?php echo $_COOKIE['pays']; ?> c'est bien ça ?
</p>
```

Attention les cookies proviennent du client -> les informations ne sont pas sures 

Si pas défini -> $_COOKIE['nom'] n'existe pas --> faire un isset dessus 

Modification d'un cookie --> en fait il faut écraser les autres 


## Lire et écrire dans un fichier 
Index 
* Autoriser l'écriture de fichiers 
* Ouvrir et fermer un fichier 
* Lire et écrire dans un fichier 

### Autoriser l'écriture de fichiers (chmod) 
Le CHMOD est un nombre à 3 chiffres (pour changer le CHMOD) faut passer par le logiciel de FTP 
* le propriétaire 
* le groupe 
* permissions publiques (pour les fichiers faut une permission de 777)

### Ouvrir et fermer un fichier 
**ouvrir mon fichier**
```
$mon_fichier = fopen('compteur.txt', 'r+'); 
```
Les modes d'ouverture du fichier 
* r (lecture seule) 
* r+ (lecture et écriture) 
* a (append) , si le fichier n'existe pas -> il est créé 
* a+ (écriture et lecture) 

**fermer le fichier**
fclose($mon_fichier); 

### Lire et écrire dans un fichier 
**Lire**
2 manières 
* lecture par caractère -> fgetc 
* lecture par ligne -> fgets 

Exemple 
```
$mon_fichier = fopen('fichier.txt', 'r+'); 
$ligne = fgets($mon_fichier); //lecture de la première ligne, itératif à chaque fois 
fclose($mon_fichier); 
```

**Ecriture**
fputs(); 
```
fputs($mon_fichier, 'texte à écrire'); 
```

Attention à la position du curseur 
* si on déplace le curseur avec un fgets/fgetc 
* fseek($mon_fichier, 0); //au se remet au début du fichier , sert à rien pour a (toujours à la fin) 

Exemple : compteur de vues de la page 
```
$mon_fichier = fopen('compteur.txt', 'r+'); //on va juste incrémenter la première ligne 
$pages_vues = fgets($mon_fichier); 
$pages_vues++; 
fseek($mon_fichier, 0); 
fputs($mon_fichier, $pages_vues); 
fclose($mon_fichier); 
```
