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
### Eléments de formulaire 
### Ne jamais faire confiance aux données 
### Envoi de fichiers (section à part)

## TP : page protégée par un mot de passe 
## Variables superglobales 
## Sessions et cookies 
## Lire et écrire dans un fichier 