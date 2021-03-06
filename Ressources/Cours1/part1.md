# Permiers pas en PHP 
## Découvrir le fonctionnement d'un site en PHP
Index 
* Les sites statiques et dynamiques 
* Comment fonctionne un site web (différence de fonctionnement statique >< dynamique)
* Les langages du web 
* La concurrence 

### Les sites statiques et dynamiques 
Les sites statiques 
* pas de mise à jour du contenu sans intervention du webmaster 
* langages clients (HTML, CSS, JS)

Les sites dynamiques 
* mise à jour (personnalisation) sans intervention du webmaster 
* en + langages clients -> langage serveur + SGBBD(R)

### Comment fonctionne un site web (différence de fonctionnement statique >< dynamique)
Différence entre client et serveur 
* Les clients consomment les données 
* Les serveurs délivrent / génèrent les données 

Site statique 
* Fonctionnement en 2 temps 
    * le client fait une requete
    * le serveur délivre le page (toujours la meme)

Site dynamique 
* Fonctionnement en 4 temps 
    * le client fait une requete 
    * le serveur génère la page (via une communication avec le SGBDR)
    * le serveur envoie la page générée (HTML et pfs un peu de CSS)

### Les langages du web 
Rien d'intéressant ici 

### La concurrence 
**Langage serveur**
* PHP (Symfony, Laravel)
* C# (ASP.NET)
* Python (Django)
* Java (JEE)
* Ruby (Ruby on rails)

**SGBDR**
* Oracle (le best mais très cher)
* Microsoft SQL Server (souvent avec ASP.NET) payant aussi 
* MySQL (gratuit et on peut faire beaucoup)
* PostGreSQL (idem mais on peut faire plus, mais la communauté est moindre)
* ... 

Toutes les combinaisons sont possibles mais la plus fréquente est PHP + MySQL (ex WordPress) 

## Préparer son environnement de travail 

[Dl XAMPP](https://www.apachefriends.org/fr/index.html)

Commandes pour installation : 
```
sudo su
chmod 755 xampp-linux-*-installer.run
./xampp-linux-*-installer.run
```

Commandes pour le lancement / extinction du serveur 
```
Lancement --> /opt/lampp/lampp start
Arret --> /opt/lampp/lampp stop
```

Les fichiers PHP doivent etre dans le répertoire /opt/lampp/htdocs
```
cd /opt/lampp/htdocs
mkdir tests
```

Une fois que c'est fait on peut 
```
http://localhost/tests
```

## Premier script 
Index 
* Les balises PHP 
* Afficher du texte 
* Commentaire 

### Les balises PHP 
Bien entendu le fichier doit avoir l'extension .php et etre accessible par Apache sinon ... bein voilà quoi ;) 

```
<?php
    code PHP entre ces balises
?>
```

Où mettre le code PHP ... partout 
```
<body>
    //some HTML code 
    <?php
        //some PHP code
    ?>
</body>
```

```
<p>
    //some text
    <?php
        some PHP code
    ?>
</p>
```

```
<meta <?php //come PHP code ?> />
```

### Afficher du texte : echo 
```
<?php echo 'ceci est du texte <strong>important</strong>'; ?>
```
On remarque 
* simple ou double quote 
* utiliser le caractère d'échappement \ 
* \n pour retour à la ligne (ou mieux <br>)

### Commentaire 
* Commentaires fin de ligne //commentaire 
* Commentaires plusieurs lignes /* commentaire sur plusieurs lignes */

## Configurer PHP pour visualiser les erreurs 
```
phpinfo();
```

Trouver la ligne **Loaded Configuration File**

Ensuite 
* error_reporting : E_ALL 
* display_errors : on 

