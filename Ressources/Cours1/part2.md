# Les bases de PHP 
## Les variables 
Index 
* Qu'est-ce qu'une variable 
* Affecter une valeur à une variable 
* Afficher et concaténer des variables 
* Opérateurs arithmétiques 

### Qu'est-ce qu'une variable 
Une variable est une zone dédiée de mémoire temporaire 

Caractérisée par 
* un nom 
* une valeur 

Les types de variables 
* String : 'chaine' ou "chaine" 
* int 
* float 
* bool (true ou false) attention à la casse ! 
* NULL 

### Affecter une valeur à une variable 
```
$age_du_visiteur = 17; 
```
### Afficher et concaténer des variables 
```
echo $age_du_visiteur; 
echo 'du texte ' . $age_du_visiteur . 'du texte' ; //je choisis cette méthode (+ claire et + rapide)
echo "du texte $age_du_visiteur encore du texte"; 
```
### Opérateurs arithmétiques 
Liste des opérateurs : + - * / et % (comme d'habitudeuuuu)

## Les conditions 
Index 
* opérateurs logiques et de comparaison 
* if elseif else 
* switch 
* ternaires 
* astuce bonus 

### opérateurs logiques et de comparaison 
* opérateurs de comparaison 
    *  ==, !=, >=, >, <, <=
* opérateurs  de comparaison 
    * && || ! 
if elseif else 
```
if(condition){
    //some code 
}elseif(condition2){
    //some code
}else{
    //some code
}
```
### Swicth 
```
switch($variable){
    case 0: 
        instructions; 
        break; 
    case 1: 
        instructions; 
        break; 
    ... 
    default : 
        instructions; 
}
```
### Les ternaires 
```
$variable = (condition) ? valeur_si_vrai : valeur_si_faux; 
```

### Astuce bonus 
```
<?php
$variable = 23;

if ($variable == 23)
{
?>
<strong>Bravo !</strong> Vous avez trouvé le nombre mystère !
<?php
}
?>
```

## Les boucles 
Index 
* while 
* do while 
* for 

### while 
```
while(condition){
    instructions
    incrémentation; //versus break ou continue 
}
```

### do while 
```
do{
    instructions; 
    incrémentation; //versus break ou continue
}while(condition); 
```

### for 
```
for(initialisation; condition; incrémentation){
    instructions; 
}
```

## Les tableaux 
Index 
* Tableaux numérotés 
* Tableaux Associatifs 

### Tableaux numérotés 
**Equivaut à un tableau simple**
donc un système avec des index avec index commençant à 0

**Construction**
Plusieurs manières de créer un tableau 
```
$prenoms = array ('François', 'Michel', 'Nicole', 'Véronique', 'Benoît');
```

```
$prenoms[0] = 'François';
$prenoms[1] = 'Michel';
$prenoms[2] = 'Nicole';
```

```
$prenoms[] = 'François'; // Créera $prenoms[0]
$prenoms[] = 'Michel'; // Créera $prenoms[1]
$prenoms[] = 'Nicole'; // Créera $prenoms[2]
```

**Afficher un tableau numéroté**
```
echo $prenoms[1]; //on peut changer aussi la valeur
```

**Parcourir**
```
$tableau = array ('un', 'deux', 'trois'); 
for($i = 0, $c = count($tableau); $i < $c; $i++){
    echo '<p>' . $tableau[$i] . '</p>'; 
}
```

```
<?php
$prenoms = array ('François', 'Michel', 'Nicole', 'Véronique', 'Benoît');

foreach($prenoms as $element)
{
    echo $element . '<br />'; // affichera $prenoms[0], $prenoms[1] etc.
}
?>
```

**Rechercher**
De manière générale 
* array_key_exists('clef', $tableau); //peu d'interet ici 
* in_array(valeur, $tableau); //intéressant ici 
* array_search(valeur, $tableau); //renvoie la clef si valeur trouvée sinon false 
### Tableaux associatifs 
**Equivaut à un dictionnaire**
Donc pas d'ordre mais un système clef-valeur 

**Construction**
```
$coordonnees = array (
    'prenom' => 'François',
    'nom' => 'Dupont',
    'adresse' => '3 Rue du Paradis',
    'ville' => 'Marseille');
```

```
$coordonnees['prenom'] = 'François';
$coordonnees['nom'] = 'Dupont';
$coordonnees['adresse'] = '3 Rue du Paradis';
$coordonnees['ville'] = 'Marseille';
```
**Afficher un tableau associatif**
```
echo $coordonnees['ville']; //idem pour la modification 
```

**Parcourir**
```
<?php
$coordonnees = array (
    'prenom' => 'François',
    'nom' => 'Dupont',
    'adresse' => '3 Rue du Paradis',
    'ville' => 'Marseille');

foreach($coordonnees as $clef => $valeur)
{
    echo $clef . '<br />'; //on peut autre chose évidemment 
}
?>
```

```
print_r($tableau); //fonctionne aussi avec les indexés mais juste pour debug dans deux cas 
```

**Rechercher**
De manière générale 
* array_key_exists('clef', $tableau); //peu d'interet ici 
* in_array(valeur, $tableau); //intéressant ici 
* array_search(valeur, $tableau); //renvoie la clef si valeur trouvée sinon false 

## Les fonctions 

## Plantage 
Index 
* Erreurs courantes 
    * Parse Error 
    * Undefined function 
    * Wrong parameter count 
* Quelques erreurs plus rares 
    * Headers already sent by 
    * L'image contient des erreurs 
    * Maximum time exceeded 

### Erreurs courantes 
**Parse Error**
Erreur fourre-tout (les ; ( . } etc etc ))

**Undefined function** 
* Soit la fonction n'existe vraiment pas 
* Soit la fonction est dans une librairie qui n'a pas été importée (dans une exstension inactivée)

**Wrong parameter count**
La documentation est ton amie 

### Erreurs plus rares 
**Headers already sent by**
```
<html>
<?php session_start(); ?> exemple typique d'erreur , session_start() doit se trouver avant le doctype 
```

**L'image contient des erreurs**
Quand on travaille avec la bibliothèque GD (crée des images et associe les 2)
2 solutions 
* Soit retirer la ligne <?php header ("Content-type: image/png"); ?> le temps de déboguer 
* Soit regarder le code source de la page HTML qui a été générée 

**Maximum time exceeded**
Chercher la boucle infinie 

## Inclure des portions de page 
include versus require (meme syntaxe mais require plante si la ressource est absente)
```
on suppose un fichier menu.php
<?php include("menus.php"); ?> dans le fichier principal -> permet de modulariser son code je suis fan 
```
## Coder proprement 
Index 
* Des noms clairs (de variables et fonctions) : évident 
* Indenter son code (évident aussi)
* Code correctement indenté (évident aussi)
* Standards PSR (PHP Standard Recommandations)
    * [Liste des standards](https://www.php-fig.org/psr/)
    * CamelCase pour les classes 
    * camel_case ou camelCase pour les variables 
    * CONSTANTES 
    * utf-8
    * namespace Vendor\Model;
    * un fichier : soit un fichier header soit un exe (si on compare avec du C)
    * le reste j'y reviens dans un second temps 
    * un outil : PHPCS-Fixer 
