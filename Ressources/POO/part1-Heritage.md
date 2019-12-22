# Héritage 
Index 
* Notion d'héritage 
* Accessibilité : protected 
* Imposer desz contraintes 
* Résolution statique à la volée 

# Notion d'héritage 
Index 
* Définition 
* Procéder à un héritage 
* Redéfinir les méthodes 
* Vers l'infini 

## Définition 
Une classe B hérite d'une classe A -> la classe A est *mère* et B est *fille* 

Quand une classe hérite d'une autre -> elle reprend tous les attributs et méthodes de la mère (on peut donc appeler ces méthodes ... si accessibles toutefois)

Attention PHP n'autorise pas l'héritage multiple (1 classe a au plus une classe mère) 

## Procéder à un héritage 
```
class Magicien extends Personnage{

}
```

**Définir des attributs propres**
```
class Magicien extends Personnage{
    private $_magie; 
    public function lancerSort($perso){
        $perso->recevoirDegats($this->_magie);
    }
}
```

## Redéfinir les méthodes 

Un problème assez ennuyeux -> on veut redéfinir des méthodes héritées qui elles-memes appellent des attributs privés ...
Et si on réécrit la méthode on écrase la version héritée donc perte d'intéret . D'où l'utilite d'avoir des getters et setters publics 
En pratique : 
```
class Magicien extends Personnage{
    private $_magie; 

    public function lancerSort($perso){
        $perso->recevoirDegats($this->_magie); 
    }

    //Redéfinition de la méthode gagnerExperience (héritée) 
    public function gagnerExperience(){
        parent::gagnerExperience(); //appel de la méthode gagnerExperience parente 
        if($this->_magie < 100){
            $this->_magie += 10; 
        }
    }
}
```

**Si la méthode parente retourne une valeur on peut l'exploiter**
```
class A {
    public function test(){
        return 'test'; 
    }
}

class B extends A {
    public function test(){
        $retour_parent = parent::test(); 
        echo $retour_parent; //ici on se contente d'afficher la valeur évidemment on peut faire plus 
    }
}
```
## Vers l'infini ... 

Sauf précision contraire on peut hériter à l'infini 

# Accessibilité : protected 

Entre private et public (et dernier type de protection)
Topo
* public -> accès à l'attribut/méthode de n'importe où 

# Imposer desz contraintes 
* Abstraction 
* Finalisation 

# Résolution statique à la volée