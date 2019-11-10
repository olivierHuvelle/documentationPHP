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