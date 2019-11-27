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

### Auto-chargement des données 

## Opération de résolution de portée 
Index 
* Les constantes de classe 
* Les attributs et les méthodes classiques 

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

