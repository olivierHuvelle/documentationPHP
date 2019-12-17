# Part2 : Filter, Functions, Subqueries
Index 
* WHERE 
* AND et OR 
* IN, NOT IN 
* Custom Columns 
* Functions 
* Subqueries 
* LIKE and MIN 

# WHERE 
```
SELECT column_list 
FROM table_name 
WHERE condition; 
```

Exemple 
```
SELECT * 
FROM customers 
WHERE id=7; 
```

**Opérateurs SQL**
= , != , >, >= , <, <=, BETWEEN 

Exemple 
```
Select * 
FROM customers
WHERE id != 5; 
```

**BETWEEN**
```
SELECT column_names 
FROM table_name 
WHERE column_name BETWEEN value1 AND value2; 
```

Exemple 
```
SELECT * 
FROM customers 
WHERE id BETWEEN 3 AND 7; id de [3 à 7]
```

**Pour les chaines de caractères**
Fonctionner avec des simples quotes 

Exemple 
```
SELECT id, firstName, city
FROM customers 
WHERE city = 'Nivelles'; 
```


# AND et OR 
Opérateurs logiques 
* AND 
* OR 
* IN 
* NOT 

Exemple 
```
SELECT id, firstName, lastName, age 
FROM customers 
WHERE age >= 30 AND age <= 40; //dans ce cas un between aurait été plus simple il me semble 
```

Quand on combine plusieurs conditions -> utiliser des parenthèses (comme toujours pour plus de clarté)
```
SELECT * 
FROM customers 
WHERE city = 'New York' AND (age >= 30 AND age <= 40); 
```


# IN, NOT IN 
Le IN permet d'éviter un million de de OR dans une condition 

Exemple 
```
SELECT *
FROM customers 
WHERE city IN ('Nivelles', 'Waterloo', 'Bruxelles'); 
```

Pour le NOT IN , assez évident aussi ... 
Exemple 
```
SELECT * 
FROM customers
WHERE city NOT IN ('Nivelles', 'Waterlo'', 'Bruxelles'); 
```

# Custom Columns 
**CONCAT**
Pour le principe c'est une concaténation (rien à ajouter)
Exemple 
```
SELECT CONCAT (firstName, ',' , city) FROM customers; 
```

**AS**
La fonction de concaténation retourne un champs personnalisé qui n'a pas de nom propre initialement 
```
SELECT CONCAT (firstName, ','n city) AS maSuperColonne 
FROM customers; 
```

**Opérateurs arithémtiques**
Liste des opérateurs 
* +, -, *, / (ne parlent pas ici de modulo  -> à vérifier)

Exemple 
```
SELECT ID, firstName, lastName, salary+500 AS salary 
FROM employees; //meilleur patron du moment, on ajoute 500 unités d'argent à chaque employé 
```
Une chose que je ne comprends pas , ça me semble assez mal foutu pour savoir si on est en lecture ou écriture de données 


# Functions (question -> vraiment utilisé? pas pertinent selon moi)
**Changer la casse**
Les fonctions UPPER et LOWER 
Exemple 
```
SELECT firstName, UPPER(lastName) AS newName 
FROM employees; 
```

**Racine et moyenne**
Les fonctions SQRT et AVG (note SQRT équivaut à POWER 1/2)
```
SELECT salary , SQRT(salary) 
FROM employees; //avoir cette table de retour ne sert absolument à rien ... 
```

```
SELECT id,firstName, salary, AVG(salary)
FROM employees; //beaucoup plus intéressant 
```

**Somme**
La fonction SUM 
```
SELECT SUM(salary) 
FROM employees; 
```

# Subqueries 
Comme le nom l'indique est une requete à l'intérieur d'une autre (attention à la lisibilité) 

Un exemple qui n'en est pas un selon moi 
```
SELECT FirstName, Salary 
FROM employees
WHERE salary >= 3100
ORDER BY Salary DESC; //versus ASC 
```

Le meme exemple avec un sous-requete 
```
SELECT firstName, salary 
FROM employees 
WHERE salary > (SELECT AVG(salary) FROM employees)
ORDER BY salary DESC; 
```

on peut aussi le faire 
```
SELECT * FROM items 
WHERE cost >(SELECT AVG(cost) FROM itmes); 
```
# LIKE and MIN 
**LIKE**
```
SELECT column_names
FROM table_name 
WHERE column_name LIKE pattern; 
```

Exemple 
```
SELECT * FROM employees 
WHERE firstName LIKE 'A%'; //employes dont le prénom commence par A 
```

```
SELECT * FROM employees 
WHERE lastName LIKE 's%'; //le nom de famille se terminant par s 
```

Autres exemples 
* LIKE '[bsp]%'; 

**MIN**
```
SELECT MIN(Salary) AS Salary FROM employees; 
```
