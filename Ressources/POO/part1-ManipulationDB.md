# Manipulation des données stockées 
Index 
* Une entité : un objet 
* Hydratation 
* Gérer sa DB correctement 

# Une entité : un objet 
Exemple de la structure de sa DB : [id, nom, forcePerso, degats, niveau, experience] //camelCase ou snake_case faut qu'il décide ...

Le but dans notre cas : travailler avec des objets et plus avec des tableaux 
En gros il déclare une classe Personnage 
* Attributs -> [id, nom, forcePerso, degats, niveau, experience] //devrait rappeler quelque chose ...
* Pour chacun des attributs : un setter et un getter 

La requete SQL et surtout son exploitation par PHP aura maintenant une structure similaire à 
```
$request = $db->query('SELECT id, nom, forcePerso, degats, niveau, experience FROM personnages');
while ($donnees = $request->fetch(PDO::FETCH_ASSOC)) // ou $request->fetch() toujours un array de toutes manières 
{
    $perso = new Personnage($donnees); 
    /*Puis des opérations sur notre perso*/
    echo $perso->force(); //par exemple (stupide d'accord mais quand meme) 
}
```
Bref ça change quand meme la vie :)

# Hydratation 
Index 
* Théorie de l'hydratation 
* Hydratation en pratique 

### Théorie de l'hydratation 
Hydrater un objet = lui apporter le nécessaire pour fonctionner (en gros les attributs requis -_-)

[illustration](https://user.oc-static.com/files/410001_411000/410249.png)


### Hydratation en pratique 
Donner une fonction d'hydratation dans la classe (je trouve que c'est inutile mais bon... sans doute précaution sécurité)
```
class Personnage(){
    //liste des attributs, des constantes, etc etc 
    //Et enfin notre fonction d'hydratation 
    public function hydrate(array $donnees){
        //va refaire un travail de sanitization ... 
        if(isset($donnees['id'])){
            $this->_id = $donnees['id']; //demander à Arnaud mais un peu faible je trouve 
        }
    }   
}
```

Comme on s'en doute , le code plus haut est à éviter (beaucoup trop long, pas assez flexible) 
**Un petit intermède qui va permettre de comprendre le code plus bas**
```
class A {
    //some code 
    public function hello(){
        echo 'you say goodbye ... and i say hello'; 
    }
}
```
Pour appeler la méthode (une création d'alias)
```
$a = new A; 
$method = 'hello'; 
$a->$method(); //si la méthode a besoin d'arguments on peut aussi lui en fournir ;) 
```


**Et enfin une fonction d'hydratation**

```
class Personnage {
    //notre code inchangé sauf la  méthode hydrate qui va etre redéfinie 
    public function hydrate(array $donnees){
        foreach($donnees as $key => $value){ //on parcourt le tableau associatif 
            $method = 'set' . ucfirst($key); //on définit la méthode correspondante (importance de camelCase pour le coup :p)
            if(method_exists($this, $method)){ //si le setter existe (donc bon nom de variable)
                $this->$method($value); //on appelle le setter avec les données requises  
            }
        }
    }

}
```
# Gérer sa DB correctement 
Index 
* Une classe : un role 
* Les caractéristiques d'un manager 
* Les fonctionnalités d'un manager 

## Une classe : un role 
Ici role de la classe objet = représenter une entrée de la DB (pas de gérer la DB !)

La partie qui va gérer la DB est ... roulement de tambours ... un manager ! CRUD 

On va donc créer une classe (avec son fichier etc etc) PersonnagesManager

## Les caractéristiques d'un manager : ses(son) attribut 

```
class PersonnagesManager{
    private $_db; //pour stocker le DAO (PDO dans notre cas)

}
```
## Les fonctionnalités d'un manager : ses méthodes (CRUD + setter)

```
class PersonnagesManager{
    private $_db; 

    public function __construct($db){
        $this->setDb($db); //trop de db :P 
    }

    public function add(Personnage $perso){ //Create
        $q = $this->_db->prepare('INSERT INTO personnages(nom, forcePerso, degats, niveau, experience) VALUES(:nom, :forcePerso, :degats, :niveau, :experience)'); //requete préparée ... 

        $q->bindValue(':nom', $perso->nom()); //requete préparée - suite ... 
        $q->bindValue(':forcePerso', $perso->forcePerso(), PDO::PARAM_INT);
        $q->bindValue(':degats', $perso->degats(), PDO::PARAM_INT);
        $q->bindValue(':niveau', $perso->niveau(), PDO::PARAM_INT);
        $q->bindValue(':experience', $perso->experience(), PDO::PARAM_INT);

        $q->execute(); //envoi des données à la DB , par contre j'aurais de base ajouté un closeCursor .. 
    
    }
    public function get($id){ //Read
        $id = (int) $id;

        $q = $this->_db->query('SELECT id, nom, forcePerso, degats, niveau, experience FROM personnages WHERE id = '.$id);
        $donnees = $q->fetch(PDO::FETCH_ASSOC);

        return new Personnage($donnees);
    }

    public function update(Personnage $perso){//Update 
        $q = $this->_db->prepare('UPDATE personnages SET forcePerso = :forcePerso, degats = :degats, niveau = :niveau, experience = :experience WHERE id = :id');

        $q->bindValue(':forcePerso', $perso->forcePerso(), PDO::PARAM_INT);
        $q->bindValue(':degats', $perso->degats(), PDO::PARAM_INT);
        $q->bindValue(':niveau', $perso->niveau(), PDO::PARAM_INT);
        $q->bindValue(':experience', $perso->experience(), PDO::PARAM_INT);
        $q->bindValue(':id', $perso->id(), PDO::PARAM_INT);

        $q->execute();
    }

    public function delete(Personnage $perso){//Delete
        $this->_db->exec('DELETE FROM personnages WHERE id = '.$perso->id());
    }

    public function setDb(PDO $dbd){ //setter 
        $this->_$db;
    }
}
```
## Utilisation (suppose l'utilisation du snippet auto_download)
**Création d'une instance de classe Personnage**
```
<?php
$perso = new Personnage([
  'nom' => 'Victor',
  'forcePerso' => 5,
  'degats' => 0,
  'niveau' => 1,
  'experience' => 0
]);
```

**Création d'une instance du PersonnageManager dépendant d'une instance de PDO**
```
$db = new PDO('mysql:host=localhost;dbname=tests', 'root', '');
$manager = new PersonnagesManager($db);
```

**Utilisation du manager pour ajouter notre perso càd instance de Personnage**
```
$manager->add($perso);
```



