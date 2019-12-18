# Utiliser des classes 
* Créer et manipuler des objets 
* Accesseurs et mutateurs 
* Constructeur 
* Auto-chargement de classes 

# Créer et manipuler des objets 
```
class Personnage{
    //déclaration de nos attributs qui seront privés donc 
    public function parler(){
        echo 'Bonjour tout le monde'; 
    }
}

$perso = new Personnage; //création d'une instance 
$perso->parler(); 
```

**Les méthodes**
$perso->_experience += 1; //on ne peut pas le faire (confer permission private)

```
class Personnage{
    private $_experience = 50; 
    public function afficherExperience(){
        echo $this->_experience; 
    }
}
```
Dans ce cas on peut afficher la fonction 
```
$perso->afficherExperience(); //va afficher l'expérience 
```

**Créer une méthode qui a attend un type particulier de paramètre**
```
class Personnage(){
    class Personnage{
        public function frapper(Personnage $personnage_a_frapper){
            //la fonction sera exécutée uniquement si elle recoit un argument de type Personnage -> évite des erreurs
        }
    }
}
```

# Accesseurs et mutateurs 
**Les accesseurs**
Une règle d'or : créer un accesseur par propriété 
```
class Personnage{
    private $_force; 

    public function force(){
        return $this->_force; 
    }
}
```

On peut donc ensuite faire 
```
echo $perso->force(); //va afficher la valeur de $_force de l'instance perso, dans notre cas elle n'est pas définie ! 
```


**Les mutateurs**
Les mutateurs vont permettre de modifier la valeur des attributs un par attribut ... on voit la lourdeur du code
Note perso : me faire une routine pour générer automatiquement les accesseurs et mutateurs 
```
class personnage{
    //définition des attributs 
    //définition des getters (des accesseurs) 
    //exemple de définition d'un setter (un mutateur)
    function setForce($force){
        //on pourrait préciser un type int pour éviter une exécution mais on va gérer le cas dans la fonction meme 
        if(!is_int($force)){
            trigger_error('La force du personnage doit etre un entier', E_USER_WARNING); 
            return; 
        }
        if($force > 100){
            trigger_error('La force ne peut pas excéder 100 points ', E_USER_WARNING); 
            return;
        }
        $this->_force = $force; 
    }
}
```

**Convention**
```
class Personnage{
    //liste des attributs 
    //liste des mutateurs 
    //liste des accesseurs 
}
```

# Constructeur 
```
class Personnage{
    //liste des attributs avec ou sans valeur par défaut 
    public function __construct($force, $degats){
        $this->setForce($force); //donc meme dans le constructeur on fait appel au setter .... 
        $this->setDegats($degats); 
        $this->_experience = 1; //on donne des valeurs par défaut ici du coup 
    }
}
```


# Auto-chargement de classes 

**Grande nouvelle**
Vu la longueur des classes (oui je suis un fan de la POO Python qui ne passe pas par des attributs privés) -> 
on écrit une classe par fichier 

```
require 'maClasse.php'; 
$objet = new MaClasse; 
```

**On peut se faire une routine**
```
function chargerClasse($classe){
    require $classe . '.php'; 
}
spl_autoload_register('chargerClasse'); //la fonction est appelée dès que l'on instancie une variable non déclarée (je ne comprends pas le fonctionnement)
$perso = new Personnage; 
```

