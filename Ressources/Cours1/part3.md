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
## Sessions et cookies 
## Lire et écrire dans un fichier 