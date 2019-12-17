# Partie 5 : aller plus loin avec PHP 
Index 
* Créer des images en PHP 
* regex partie 1 
* regex partie 2 
* créer un espace membre 
* Aller plus loin 

# Créer des images en PHP 
Index 
* Activer la bibliothèque GD 
* Les bases de la création d'image 
* Texte et couleur 
* Dessiner une forme 
* Encore plus de fonctions 

## Activer la bibliothèque GD 
Livrée de base avec PHP mais pas activée par défaut 

Meme démarche que pour PDO pour l'activer (aller dans php.ini) mais fortes chances que le serveur dise non (bouffe trop de ressources) 
Du peu que j'ai pu lire -> pas intéressant du tout ... 
## Les bases de la création d'image 
## Texte et couleur 
## Dessiner une forme 
## Encore plus de fonctions 

# regex partie 1 
Index 
* Où utiliser des regex 
* Recherches simples 
* Classes de caractères 
* Quantificateurs 

## Où utiliser des regex 
POSIX ou PCRE 
* POSIX (on laisse tomber) -> syntaxe différente de celle des regex JS et en plus moins performantes 
* PCRE (simuilaires à celle de JS car aussi inspirée de Pearl)

Les fonctions les plus intéressantes pour commencer 
* preg_grep 
* preg_split 
* preg_quote
* preg_match
* preg_match_all
* preg_replace
* preg_replace_callback

**preg_match**
renvoie true ou false suivant que trouvé ou pas 
```
if(preg_match(regex, chaine_a_tester)){
    //code si occurrence 
}
```

## Recherches simples 

#contenu de la regex#options (donc meme principe que /regex/options de JS)
ex if(pcre_match('#guitare#', 'texte à tester')){
    //code si on parle de guitare 
}

De base sensible à la casse 
#regex#i devient insensible à la casse 

**OU**
#guitare|piano#  

**début et fin de chaine**
^début de chaine 
$fin de chaine (plutot fin de chaine$ :p)

## Classes de caractères 

#gr[ioa]s# match pour gris, gras, gros donc 
[aeiouy] intervalle de voyelles donc (sans les accents)
[a-z]
[0-9]
[a-z0-9]
h[1-6] un titre 
[^aeiouy] true pour une consonne (plutot pour tout ce qui n'est pas une voyelle)

## Quantificateurs 
? : 0 ou 1 
+ : >= 1 fois 
* : >= 0 fois 

on peut l'appliquer à un groupe , lui-meme défini par des parenthèses 
ex : #^Ay(ay)*# 

Les accolades 
* {3} 3 fois précisément 
* {3,5} intervalle [3,5] fois 
* {,3} et {3,} plutot évident 

# regex partie 2 
Index 
* Les métacaractères 
* Les classes abrégées 
* Construire une regex 
* Capture et remplacement 

## Les métacaractères 
Sont les caractères à échapper (mais pas toujours confer intervalles) : # ! ^ $ ( ) [ ] { } ? + * . \ |

Echappement via le \ comme toujours --> #Quoi \?#

**Le cas des classes**
Pas besoin d'échapper les métacaractères dans les intervalles SAUF 
* #, ], - (confer utilité dans les intervalles - logique ...)

## Les classes abrégées 
\d : un chiffre (decimal) équivaut donc à [0-9]
\D : not \d 
\w : caractère alphanumérique  + le _ 
\W : not \w comme toujours 
\t : tabulation 
\n : retour à la ligne 
\r : retour chariot (définition ? +- pareil que retour à la ligne mais OS dépendant)
... 
\s : espace blanc 
\S 
. = tout caractère (sauf \n sauf si on donne l'option s à la regex)

## Construire une regex 
Pas beaucoup intéret 
Exemple numéro de tel : #^0[1-68][0-9]{8}$#

Pour une adresse mail : #^[a-z0-9._-]+@[a-z0-9._-]{2,}\.[a-z]{2,4}$# je suis pas d'accord avec sa construction (ne peut pas commencer par chiffre)


**MySQL comprend les regex**
Mais seulement les POSIX ... 

Différences avec les PCRE 
* pas de délimiteurs ni d'options (donc pas de # ni de i etc)
* pas de classes abrégées 

Exemple : 
```
SELECT nom FROM visiteurs WHERE ip REGEXP '^84\.254(\.[0-9]{1,3}){2}$'
```

## Capture et remplacement 
**Les parenthèses capturantes**
#\[b\](.+)\[/b\]#
A chaque fois que parenthèse -> création d'une variable $x (x commencant à 1) dont on peut se servir pour modifier le contenu de la parenthèse 
Exemple 
```
$texte = preg_replace('#\[b\](.+)\[/b\]#i', '<strong>$1</strong>', $texte);
```
L'ordre dans lequel les parenthèses sont ouvertes qui importe (confer imbrication de parenthèses)
(?contenu) parenthèse non capturante 

# créer un espace membre 
Voir dossier annexe 
# Aller plus loin 
* Apprendre à faire de la POO en PHP 
* Adopter une architecture MVC 
* Utiliser un framework (Symfony versus Laravel -> passer vers Laravel)