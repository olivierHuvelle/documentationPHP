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
Bref ça change quand meme la vie (pas parlant dans cet exemple il faut l'admettre)

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

```
class Personnage {
    //notre code inchangé sauf la  méthode hydrate qui va etre redéfinie 
    public function hydrate(array $donnees){
        foreach($donnees as $key => $value){ //on parcourt le tableau associatif 
            $method = 'set' . ucfirst($key); //on définit la méthode correspondante (importance de camelCase pour le coup :p)
        }
    }
}
```
# Gérer sa DB correctement 
