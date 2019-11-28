# Les bases de la POO 
## Introduction à la POO 
Index 
* Qu'est-ce que la POO 
* Créer une classe 

**Qu'est-ce que la POO**
Différence entre une classe et un objet (objet = instance de classe) 

Principe d'encapsulation : créer des getters et des setters et gérer les permissions sur les propriétés/méthodes d'une classe (/instance de classe)

Visibilité d'un attribut / méthode 
* public 
* private 

**Créer une classe**
Attention la valeur par défaut doit etre une expression scalaire statique (donc pas appel d'une fonction ou d'une variable superglobale)
```
<?php
class Personnage{
    private $_force = 10; //valeur par défaut 
    private $_localisation = 'Nivelles'; 
    private $_experience; //etc etc 

    public function deplacer(){
        //code de la méthode
    }
    public function parler(){
        echo 'Bonjour tout le monde'; 
    }

    public gagnerExperience(){
        this->_experience++; 
    }
}
?>
```
## Utilisation de la classe 
Index 
* Créer et manipuler un objet 
* Les accesseurs et les mutateurs 
* Le constructeur 
* Auto-chargement des données 

### Créer et manipuler un objet 
**Créer un objet**
```
$olivier = new Personnage; 
```

**Appeler les méthodes de l'objet**
```
$olivier->parler(); 
$olivier->gagnerExperience(); 
```

**S'assurer du type d'une variable avant d'exécuter une méthode**
```
class Personnage{
    private $_degats; 
    private $_experience; 
    private $_force; 
    
    public function frapper(Personnage $adversaire){
        $adversaire->_degats += $this->_force; 
    }
}
```

### Les accesseurs et les mutateurs 
**Les accesseurs**
```
class Personnage {
    private _force; 
    private _experience; 
    private _degats; 

    public function frapper(Personnage $adversaire) {
        $adversaire->_degats += $this->_force; 
    }

    public function gagnerExperience() {
        $this->_experience++; 
    }

    public function degats() {
        return $this->_degats; 
    }

    public function force() {
        return $this->_force; 
    }

    public function experience() {
        return $this->_experience; 
    }
}
```
```
$olivier->degats(); //on fait appel au setter 
```
**Modifier la valeur d'un attribut : les mutateurs**
```
class Personnage {
    //reprise du code plus haut 

    public function setForce($force) {
        if(!is_int($force)) {
            trigger_error('La force d un personnage doit etre un nombre entier', E_USER_WARNING); 
            return; 
        }
        if($force > 100){
            trigger_error('La force du personnage ne peut pas excéder 100', E_USER_WARNING); 
            return; 
        }
        $this->_force = $force; 
    }
    // et on fait pareil pour chaque propriété 
}
```
```
$olivier = new Personnage; 
$olivier->setForce(50); //appel du mutateur 
```
### Le constructeur 
```
class Personnage {
    private $_force; 
    private $_localisation; 
    private $_experience; 
    private $_degats; 

    public function __construct($force, $degats) { //forcément public sinon on ne peut pas l'appeler ... 
        echo 'Bienvenue dans le constructeur'; 
        $this->setForce($force); //on fait appel au mutateur -> comme ça on peut vérifier le type de variable en entrée 
        $this->setDegats($degats); 
        $this->_experience = 1; 
    }
}
```

```
$olivier = new Personnage(50, 0); 
```

### Auto-chargement des données 
Pour organisation : créer un fichier par classe (MaClasse.php)
```
<?php
require 'MaClasse.php'; 
$objet = new MaClasse; //suppose que pas de constructeur dans ce cas 
?>
```

Auto-chargement des classes 
```
<?php
function chargerClasse($classe)
{
  require $classe . '.php'; // On inclut la classe correspondante au paramètre passé.
}
spl_autoload_register('chargerClasse'); // On enregistre la fonction en autoload pour qu'elle soit appelée dès qu'on instanciera une classe non déclarée.

$perso = new Personnage(); //pas logique tout ça 
```

## Opération de résolution de portée 
Index 
* Introduction 
* Les constantes de classe 
* Les attributs et les méthodes classiques 

### Introduction 
Opérateur de résolution de portée :: (Scope resolution operator) 
Il existe des constantes de classe 

### Les constantes de classe 

```
class Personnage {
    private _$force; 
    //etc etc 
    const FORCE_PETITE = 20; 
    const FORCE_MOYENNE = 50; 
}
```

On ne peut pas accéder via perso->_FORCE_PETITE

```
class Personnage{
    //données plus haut 
    public function __construct($forceInitiale) {
        $this->setForce($forceInitiale);
    }
    public function setForce($force) {
        if(in_array($force, [self::FORCE_PETITE, self::FORCE_MOYENNE, self::FORCE_GRANDE])) { //puissant à fond
            $this->_force = $force; 
        }
    }
}

$person = new Personnage(Personnage::FORCE_MOYENNE); 
```

### Les attributs et les méthodes classiques 

**Les méthodes statiques**
Sont des méthodes qui doivent agir sur une classe et non sur une instance de l'objet 

```
class Personnage {
    //code supra 
    public state function parler() {
        echo 'i will kill you all'; 
    }
}
```
```
    Personnage::parler(); //plus visuel on voit tout de suite que méthode statique et aussi évite erreur de E_STRICT
On peut aussi 
    $perso = new Personnage(Personnage::FORCE_GRANDE); 
    $perso->parler(); //on peut appeler une méthode statique depuis une instance meme si pas beaucoup d'interet 
```

**Les attributs statiiques**
```
class Personnage {
    private $_... etc etc 
    const FORCE_PETITE = 20; 

    private static $_texteADire = 'I will kill you all!'; 

    public static function parler() {
        echo self::$_texteADire; //:: car statique et $_ ... 
    }
}
```
Utilité des attributs statiques 
* Les attributs statiques servent à avoir des attributs indépendants de toute instance (touche aussi toutes les instances du coup)


Exemple 
```
class Compteur {
    private static $_compteur = 0; 
    
    public function __construct() {
        self::$_compteur++; 
    }

    public function getCompteur() {
        return self::$_compteur; 
    }
}
```

```
echo Compteur::getCompteur(); //va afficher le nombre d'instances de classe 
```

## Manipulation de données stockées 
Index 
* Une entité, un objet 
* Hydratation 
* Gérer sa BD correctement 

## TP : mini-jeu de combat 
Index (voir les backups)

## Héritage 
Index 
* Notion d'héritage 
* Un nouveau type de visibilité : protected 
* Imposer des contraintes 
* Résolution statique à la volée 

## TP : des personnages personnalisés 
Index : (voir les backups) 

## Méthodes magiques 
Index 
* Le principe 
* La surcharge magique des attributs et des méthodes 
* Linéariser ses objets 
* Autres méthodes magiques 

