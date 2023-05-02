# Chapitre 10 - Bases de données relationnelles

> [!info] Index
> [[Index|Retour à l'index]]

## I – Introduction

> [!tldr] Objet d'étude
On s'interesse aux BD relationelles
**SGBDR** = **S**ystème de **G**estion de **B**ase de **D**onnées **R**elationnelles

## II – Bases de données relationnelles

> [!note] Définitions
> Une base de données relationnelles est un ensemble de tables ou de relations
>
Le **schéma relationnel** de LesVegetaux s'écrit :
LesVegetaux(<u>idVeg</u>, nomVeg, couleur, type, prix)
>
Une **valeur** est une donnée d'un attribut pour un enregistrement.
**attribut** : colonne
**enregistrement** : ligne
Les attributs dont le nom est souligné constituent l'identifiant de la relation
un identifiant est unique à un enregistrement dans une table
>
Les valeurs de l'identifiant (clé primaire de la relation) sont uniques
Un attribut a un type : c'est le **domaine de définition**

## III- Interrogation avec le langage SQL
### A – Projection
#### 1) Présentation

|  A  |  X  |  Y  |
|:---:|:---:|:---:|
|  3  |  2  |  T  |
|  1  |  6  |  G  |
|  4  |  3  |  Z  |

On « projette » une relation sur un sous-ensemble d'attributs
projection de R dans (A,X) →

|  A  |  X  |
|:---:|:---:|
|  3  |  2  |
|  1  |  6  |
|  4  |  3  |
  
#### 2) Syntaxe en SQL

```sql
SELECT A,X --le nom des attributs
FROM R ; --nom de la relation
```

```sql
SELECT nomVeg
FROM lesVegetaux ;
```

#### 3 ) Gestion des doublons  

```sql
SELECT DISTINCT A,X
FROM R ;
```

### B – Selection

#### 1) Syntaxe

```sql
SELECT A
FROM R
WHERE condition ; --condition pour conserver un enregistrement
```

#### 2) Exemples

```sql
SELECT nomVeg
FROM LesVegetaux
WHERE type = 'légume' AND couleur = 'orange' ;
```

```sql
SELECT nomVeg, prix * 1.05
FROM lesVegetaux
WHERE type = 'fruit' ;
```

### C – Tris

```sql
SELECT A --liste d'attributs
FROM R --nom de la relation
[WHERE condition]
ORDER BY B [ASC/DESC] ;
```

```sql
SELECT nomVeg
FROM lesVegetaux
ORDER BY prix ASC ;
```

```sql
SELECT
FROM 
[WHERE condition]
ORDER BY
LIMIT a --nb d enregistrement renvoyé
OFFSET num --nombre d enregistrement renvoyé ;
```

> [!note] Syntaxe
*LIMIT a* : réduit le nombre d'enregistrement renvoyé au nombre de a (en partant du haut)
*OFFSET num* : décale le début des enregistrement séléctioner à num.

### D – Opérateurs d'agrégation

```sql
SELECT count(nomVeg)  
FROM lesVegetaux
WHERE type = 'fleur' ;
```

```sql
SELECT count(DISTINCT nomVeg)
FROM lesVegetaux
WHERE type = 'fleur' ;
```

> [!note] Syntaxe
*count()* : compte le nombre
*max()*: la valeur maximale
*min()*: la valeur minimal
*avg()*: une moyenne
*sum()*: la somme

### E – Partition

#### 1) Syntaxe

```sql
SELECT
FROM
WHERE
GROUP BY A –nom d un attribut
HAVING
ORDER BY ;
```

> [!example] Exemple
Type de végétaux qui ont au moins 2 végétaux enregistrés.
>```sql
SELECT type
FROM lesVegetaux
GROUP BY type
HAVING count(idVeg) >= 2;
>```

## IV - Retour sur le modèle relationnel

### Définitions

> [!note] Définitions
Un **domaine** est une ensemble fini ou non de valeurs possibles.
L'ensemble des **n-uplet** est un produit cartésien de domaines.
>
On dit qu'on a une **relation** $\mathscr{R}$ entre deux ensembles A et B si $\mathscr{R}$ relation sur $A \times B$
Si on a $a \in A$ et $b \in B$, $a$ et $b$ sont en relation par $\mathscr{R}$ si $(a, b) \in \mathscr{R}$, noté $a\ \mathscr{R}\ b$
On donne un nom aux différentes domaines de la relation, ce nom est appelé **attribut**.
>
La **clé primaire** d'une relation est un ensemble d'attributs qui détermine un n-uplet de manière unique.
Une **clé étrangère** est un ensemble d'attribut qui référence un ensemble d'attribut d'une autre relation.
>
[Définitions plus clairs (cours de terminal)](http://193.49.249.136:20180/~web/terminale/sgbd_vocabulaire.php#2.1)

## V - SQL : Éléments avancés

> [!example] Exemple de schéma relationnel
LesVegetaux(<u>idVeg</u>, nom, couleur, type)
LesProducteurs(<u>idProd</u>, nomBoutique, surface, dept)
LesPrix(<u>\#idProd</u>, <u>\#idVeg</u>, prix)

### A - Produit cartésien

#### 1) Présentation

$R$

|  A  |  B  |
|:---:|:---:|
|  1  | 'C' |
|  2  | 'D' |
|  3  | 'C' |
$S$

|  A  |  C  |
|:---:|:---:|
|  3  | 'A' |
|  4  | 'C' |

$R \times S$

|  A  |  B  |  A  |  C  |
|:---:|:---:|:---:|:---:|
|  1  | 'C' |  3  | 'A' |
|  1  | 'C' |  4  | 'C' |
|  2  | 'D' |  3  | 'A' |
|  2  | 'D' |  4  | 'C' |
|  3  | 'C' |  3  | 'A' |
|  3  | 'C' |  4  | 'C' |
#### 2) Syntax en SQL

```sql
SELECT *
FROM R, S;
```

### B - Jointures

#### 1) Exemple

Nom et prix des végétaux chez le producteur idProd = 1
```sql
SELECT V.nom, P.prix
FROM LesVegetaux AS V JOIN LesPrix AS P
ON V.idVeg = P.idVeg
WHERE P.idProd = 1;
```

Prix max par boutique
```sql
SELECT nomBoutique, max(prix)
FROM LesProducteurs AS P JOIN LesPrix
ON P.idProd = LesPrix.idProd
GROUP BY nomBoutique;
```

$R$

|  A  |   B    |
|:---:|:------:|
|  1  | Castor |
|  2  | Loutre |
|  3  |  Elan  |

$S$

|  A  |    C    |
|:---:|:-------:|
|  3  | bonjour |
```sql
SELECT S.A, S.C, R.B
FROM R
JOIN S ON R.A = S.A;
```

| S.A |   S.C   | R.B  |
|:---:|:-------:|:----:|
|  3  | bonjour | Elan |
```sql
SELECT S.A, S.C, R.B
FROM R LEFT JOIN S ON S.A = R.A;
```

| S.A  |   S.C   |  R.B   |
|:----:|:-------:|:------:|
|  3   | bonjour |  Elan  |
| NULL |  NULL   | Castor |
| NULL |  NULL   | Loutre |
### C - Opérateurs ensemblistes

#### 1) Les opérateurs ensemblistes

```sql
UNION, INTERSECT, EXCEPT
```
s'appliquent sur des relations qui ont le même schéma relationnel

Nom des végétaux de type légume qui ne sont pas de couleur rouge :
```sql
SELECT nom FROM LesVegetaux
WHERE type='legume'
EXCEPT
SELECT nom FROM LesVegetaux
WHERE couleur='rouge';
```

```sql
IN / NOT IN
```

```sql
SELECT nom AS n
FROM LesVegetaux
WHERE nom NOT IN
(SELECT nom FROM LesVegetaux WHERE couleur='rouge')
AND type='legume';
```

## VI - Modèle entité association

### A - Définitions

> [!note] Définition
Une **entité** est la réprésentation d'un objet ayant une existence propre.

>[!example] Exemples
>- un végétal
>- une boutique
>- un département
>- un prix
>- un producteur

> [!note] Remarque
Une entité peut posséder des propriétés, par exemple la couleur.

> [!note] Définitions
Une **association** est un lien entre différentes entités.
>
La **cardinalité** d'un lien est représentée par un couple de valeurs indiquant le minimum et le maximum de fois qu'une entité apparaît dauns une association.
La valeur min est généralement soit 0 soit 1. On écrit \* lorsqu'on ne connaît pas la valeur max. La cardinalité est notée a..b avec a la valeur min et b la valeur max.
Notation : la cardinalité 0..\* est notée \*

> [!note] Définitions
Le **type d'une association** est déterminé par les bornes maximums des deux côtés de l'association.
Le type de l'association indique comment traduire l'association dans le modèle relationnel.
>
1-\* : association dite *fonctionnelle* ou *hiérarchique*, chaque élément de l'entité du côté \* peut être lié à plusieurs élément de l'entité du côté 1 (ex: livre-auteur)
1-1 : à chaque élément de l'entité A est lié exactement un élément de l'entité B (ex: proviseur-lycée)
\*-\* : chaque élément de l'entité A est lié à plusieurs élément de l'entité B et inversement (ex: animal-maladie) 
>
Chaque entité est représentée par une table. Pour chaque association :
1-\* : ajouter une clé étrangère du côté 1 qui référence la clé primaire du côté \*
\*-\* : ajouter une table d'association dont la clé est la paire de clés étrangères
Dans le cas des associations 1-1, on peut fusinonner les entités

### B - Exemple

![[Web/src/Pasted image 20230130115449.png]]