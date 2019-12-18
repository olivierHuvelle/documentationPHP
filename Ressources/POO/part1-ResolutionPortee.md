# Opérateur de résolution de portée 
Index 
* Les constantes de classe 
* Les attributs et méthodes statiques 

# Les constantes de classe 
Le but des constantes : éviter le code muet (càd une instruction qui n'a aucun sens logique à première vue)

**Déclarer une propriété constante**
```
class Personnage{
    //déclaration des attributs classiques 
    const FORCE_PETITE = 20; //exemple de déclaration de constante 
}
```

**Utilise rune propriété constante dans les méthodes**
```
class Personnage{
    //déclaration des attributs, des constantes etc etc 

    public function setForce($force){
        if(in_array($force, [self::FORCE_PETITE, self::FORCE_PETITE, self::FORCE_GRANDE])){
            $this->_force = $force; 
        }
    }
}
```

**Lors de la création de l'instance**
$perso = new Personnage(Personnage::FORCE_MOYENNE); 

# Les attributs et les méthodes statiques (sont en gros des attributs et des méthodes de classe) 
**Les méthodes de classe**
```
class Personnage{
    public static function parler(){
        echo 'Je suis une fonction qui dit/fait toujours la meme chose'; 
    }
}
```

Appel de la méthode (2options)
```
Personnage::parler(); //la méthode à préférer on passe par la classe avec scope resolution operator -> clair 
perso->parler(); //passe par l'instance, marche mais perd de son sens (mute code)
```

**Les attributs de classe**

```
class MaClasse{
    //définition des attributs et des constantes -> classique 
    private statique $_nom_variable = valeur; 

    //illustration d'une méthode qui va l'utiliser 
    public function maFonction(){
        echo self::$_nom_variable; //self et non this et :: au lieu de -> mais assez logique 
    }
}
```

Un exemple très classique d'utilisation de variable statique (de classe) un compteur d'instances de classe ! 

