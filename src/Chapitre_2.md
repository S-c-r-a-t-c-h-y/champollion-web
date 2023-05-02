# Chapitre 2 - Notion de complexité

> [!link] Index
> [[Index|Retour à l'index]]

> [!note] Définition
Un **algorithme** est une succession finie d'opérations permettant de résoudre un problème. En général on a des données d'entrée et des données de sortie.

## I - Introduction

> [!tldr] Situation
Evaluer la **complexité** d'un algorithme permet d'évaluer son efficacité.
On peut s'interesser au **temps d'éxecution** ou à l'occupation en **espace mémoire**.
Pour évaluer la *complexité* d'un algorithme, on compte le nombre d'**opérations primitives** en fonction de la taille de l'entrée.
Une *opération primitive* est une opération réalisable en temps constant.

> [!example] Appartenance d'un élément à une liste
>
>```ocaml
let rec appartient x l =
 >   match l with
 >   | [] -> false
 >   | e :: q -> e = x || appartient x q
>```
>
*Opérations* :
>- comparaison à la liste vide
>- récupération de l'élément de tête
>- test d'égalité
>- appel de fonction
>
Pour une liste de taille $n$, on va avoir $4n$ opérations.
$T(n) = 4 + T(n-1)$
$T(n) = 4 \times n$
>
Le taux de croissance du temps d'exécution de cette fonction est linéaire.

## II - Notations de Landau

> [!note] Définition
Soient deux fonctions $f(n)$ et $g(n)$. On dit que $f(n)$ est <mark>"Grand O"</mark> de $g(n)$ que l'on note $O(g(n))$ s'il existe une constante positive et un entier $n_0 \geq 1$ tel que $f(g) \leq c \times g(n) \; \forall \; n \geq n_0$

> [!info] Notation
On note $f(g) = O(g(n))$

> [!info] Signification
 Ecrire $f(n)$ est $O(g(n))$ signifie que le taux de croissance de $f$ n'est pas supérieur à celui de $g$.

> [!warning] Règles
>- Si $f$ est un polynôme de degré $d$, alors $f(n)$ est $O(n^d)$
 >	- on ne tient pas compte des facteurs constants
 >	- on peut supprimer les termes de degré inférieur à $d$
>- On utilise la classe la plus basse possible / l'expression la plus simple (*$2n$ est $O(n)$ même si c'est aussi $O(n^2)$*)

> [!note] Définitions supplémentaires
> - $f(n)$ est $\Omega(g(n))$ si $\exists \; c > 0, \exists \; n \in \mathbb{N}^*$ tel que $f(n) \geq c \times g(n) \; \forall \; n \geq n_0$
> - $f(n)$ est $\Theta(g(n))$ si $\exists \; c' > 0, \exists \; c'' > 0, \exists \; n_0 \in \mathbb{N}^*$ tel que $c' \times g(n) \leq f(n) \leq c'' \times g(n) \; \forall \; n \geq n_0$

## III - Applications

> [!example] Puissance d'un nombre entier
>
>```ocaml
let rec puissance a n =
 >   if n = 0 then 1
 >   else a * puissance a (n-1)
>```
>
*Complexité* : On effectue n appels à la fonction puissance. A chaque, on effectue un test, une multiplication, une soustraction  et un appel de fonction. Ces opérations s'effecutant en temps constant, on a donc "La complexité de la fonction puissance est $O(n)$".
>
>```ocaml
let rec exponentielle a n =
>    if n = 0 then 1
>    else if n mod 2 = 0 then exponentielle (a*a) (n/2)
>    else a * exponentielle (a*a) (n/2)
>```
>
*Complexité* : A chaque appel on effectue un nombre fini d'opérations primitive. Puisque on divise $n$ par 2 à chaque appel, on aura de l'ordre de $log_2 \; n$ appels à la fonction exponentielle. "La complexité de exponentielle est $O(log_2 \; n)$"

> [!example] Recherche du maximum d'une liste
>
Lorsque l'on recherche le maximum d'une liste d'élément, on compare l'élément de tête au maximum de la suite de la liste. "La complexité d'une recherche de maximum est $O(n)$".

### Le problème fondamental du tri

> [!tldr] Enoncé
> On dispose d'une collection d'éléments que l'on souhaite organiser en respectant un ordre.

> [!example] Le tri par sélection
>
 >> [!note] Principe
 >> A chaque étape, on va sélectionner le plus petit élément.
>
>> [!tip] Algorithme
>> Tant qu'il y a plus d'un élément restant, choisir le plus petit, le placer en tête et recommencer sur la suite.
>
*Complexité* : La complexité du tri par sélection est $O(n^2)$
> $T(n) = \displaystyle\sum_{i=1}^{n-1} i = \frac{n(n-1)}{2}$

> [!example] Le tri par insertion
>
>> [!tip] Algorithme
>Pour chaque élément à partir du deuxième, insérer l'élément parmi les éléments déjà triés.
>
*Complexité* : Dans le pire des cas lorsque les éléments sont arrangés en ordre décroissant au départ, il faut effectuer $\displaystyle\sum_{i=1}^{n-1} i = \frac{n(n-1)}{2} = O(n^2)$ déplacement en mémoire.
>
La complexité du tri par insertion est $O(n^2)$.

> [!example] Le tri fusion
>
>> [!tip] Algorithme
Séparer en deux parties de même taille, trier récursivement chaque partie puis fusionner les deux parties.
>
*Complexité* : La complexité du tri fusion est $O(n \; log_2 \; n)$

> [!example] Le tri rapide
>
>> [!tip] Algorithme
Choisir un élément qui sera utilisé comme pivot, mettre à gauche tous les éléments plus petits que le pivot et à droite tous les éléments plus grands et trier récursivement les deux sous-parties.
>
*Complexité* : Dans le meilleur cas, on choisit un pivot qui est systèmatiquement l'élément médian. Dans ce cas la complexité du tri rapide est $O(n \; log_2 \; n)$. En revanche, si on choisit un pivot qui conduit à laisser un seul élément plus petit ou plus grand alors on effectue $n$ comparaisons.
>
La complexité dans le pire des cas du tri rapide est en $O(n^2)$

## IV - Vocabulaire et outils

| Fonction      |     Complexité     | Exemple                                     |
| ------------- |:------------------:| ------------------------------------------- |
| Constante     |       $O(1)$       | Affectation d'une variable                  |
| Logarithme    |   $O(\log \; n)$    | Recherche d'un élément dans une liste triée |
| linéaire      |       $O(n)$       | Parcours d'une liste de longueur $n$        |
| N-log N       | $O(n \; \log \; n)$ | Tri fusion d'une liste de taille $n$        |
| Quadratique   |      $O(n^2)$      | Tri par selection, double boucle            |
| Cubique       |      $O(n^3)$      | Triple boucle, multiplications de matrices  |
| Exponentielle |      $O(2^n)$      | Tous les sous-ensembles                     |
| Factorielle   |      $O(n!)$       | Toutes les permutations, tri stupide        |