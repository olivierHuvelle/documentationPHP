# Introduction à la POO 
# Index 
* Qu'est-ce que la POO 
* Créer une classe 

# Qu'est-ce que la POO 
Différence entre une classe et instance de classe
* Une classe est un modèle 
* Une instance de classe : = objet (matérialisation d'une classe en quelque sorte)

Un principe important en PHP (et pas en JS) : l'encapsulation 
* on ne peut pas accéder directement aux attributs mais passer par des méthodes (assez lourd d'écriture par contre)

# Créer une classe 
**Mettre des attributs dans la classe**
```
class Personnage(){
    private $_force; //private et _nomVariable , on peut aussi mettre en public mais on flingue l'encapsulation 
    private $_localisation; 
    private $_degats = 0; //valeur par défaut 
}
```

Remarque sur les valeurs par défaut des propriétés 
* uniquement une valeur STATIQUE SCALAIRE 
* pas pas de rtour d'une fonction 
* pas non plus d'appel à une autre variable (superglobale par exemple)

**Création de méthodes**
```
class Personnage(){
    //définition des attributs, confer supra 
    public function nomMethode(arguments){
        //code de la méthode 
    }
}
```