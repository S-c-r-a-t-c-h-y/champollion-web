# Chapitre 3 - Correction des algorithmes

> [!link] Index
> [[Index|Retour à l'index]]

## I - Définitions

### A - Spécification d'un problème algorithmique

> [!note] Définitions
 Les **spécifications** d'un problème comporte deux parties.
> - la description des entrées admissibles
> - la description du résultat ou des effets attendus (sorties)
>
On décrit les entrées admissibles par des contraintes appelées **préconditions**
>
On décrit les sorties attendues par des contraintes appelées **postconditions**
>
 Une **assertion** permet de s'assurer qu'une condition est bien respectée à un endroit.

> [!info] Syntaxe
En OCaml `assert` est une fonction de type `bool -> unit` qui lève une exception de type `Assert failure` si la condition exprimée n'est pas évaluée à `true`.

> [!example] Exemple
Vérifier qu'une liste est triée :
>```ocaml
assert (est_croissante liste);
>```

### B - Terminaison et correction

> [!note] Définition : terminaison
Une fonction **termine** si elle renvoie un résultat en un nombre fini d'étapes, quelles que soit les données d'entrée (en respectant les préconditions).

> [!note] Définition : correction partielle
Une fonction est **partiellement correcte** si lorsqu'elle termine elle renvoie le résultat attendu.

> [!note] Définition : correction totale
Une fonction est **totalement correcte** ou **correcte** si elle termine et elle est partiellement correcte.

> [!example] Exemple
Cette fonction est partiellement correcte mais elle ne termine pas.
>```ocaml
let rec fact_fausse n =
>  if n = 1 then 1
>  else n * fact_fausse (n+1)
>```

## II - Exemples en programmation récursive

> [!example] Somme des n premiers entiers
>
>```ocaml
let rec somme_n n =
>  assert (n >= 0);
>  if n = 0 then 0 else n + somme_n (n-1)
>```
>
*Terminaison* :
>- Si $n < 0$ la précondition matérialisée par le `assert` n'est pas respectée et la fonction lève une exception
>- Si on a $n \geqslant 0$
>- Le cas $n = 0$ vaut $0$ d'après le code de la fonction
>-Lors de chaque appel récursif n décroît strictement de $1$. Puisque $\mathbb{N}$ n'admet pas de suite infinie on peut alors conclure que la suite des appels termine.
>
*Correction* :
 Notons $P(n)$ la propriété. `somme_n  n`  vaut la somme des premiers entiers positifs, soit $\sum_{i=0}^{n} i$ 
>
 <u>Cas de base</u> : Pour $n = 0$, d'après le code de la fonction on a $somme\_n \; 0 = 0$ ce qui est égal à $\sum_{i=0}^{0} i = 0$  Donc $P(0)$ est vraie.
>
 <u>Hérédité</u> : Supposons que $P(n)$ est vraie. Alors on a, d'après le code de la fonction, $somme\_n \; (n+1) = n + 1$
 Par hypothèse de récurrence : $somme\_n \; (n+1) = n+1 + \sum_{i=0}^{n} =  \sum_{i=0}^{n+1}$ Donc si $P(n)$ est vraie pour un n donné alors $P(n+1)$ est vraie.
 Par principe de récurrence, $\forall \, n >= 0,\, P(n)$ est vraie, ce qui prouve la correction de la fonction `somme_n`
>
*Complexité* :
 Soit $T(n)$ le temps d'execution de `somme_n n` . Lorsque $n = 0$ alors $T(n) = c$ où $c$ est un constante représentant le temps nécessaire à renvoyer $0$.
 Sinon on a $T(n) = d + T(n-1)$ où $d$ est une constante représentant le temps nécesssaire pour effectuer une soustraction, une addition et un appel de fonction.
 $T(n)$ est une suite arithmétique de raison $d$. Alors pour tout $n >= 0$ on a $T(n) = d \times n + c$ donc $T(n)$ est $O(n)$.

> [!example] Insertion d'un élément dans une liste triée
>
>```ocaml
let rec insere x l = match l with
> | [] -> [x]
> | y :: q -> if x <= y then x :: l else y :: (inserer x q)
(* 'a -> 'a list -> 'a list *)
>```
>
*Terminaison* :
 La fonction `insere` termine lorsque la liste passée en paramètre est vide ou lorsque `x` est inférieur ou égal à l'élément de tête de la liste.
 Sinon la fonction effectue un appel récursif sur une liste de taille strictement inférieure.
>
*Correction* :
 Soit $P(n)$ : "Pour toute liste `l` croissante de longueur $n$ et pour tout `x`, alors `insere x l` renvoie une liste croissante contenant les éléments de `l` et l'élément `x`"
>
 <u>Initialisation</u> : Lorsque `l` est la liste vide, donc de longueur $0$, `inserer x []` renvoie `[x]` donc $P(0)$ est vraie.
>
 <u>Hérédité</u>: Supposons que $P(n)$ est vérifiée pour toute liste de taille inférieur ou égale à $n \geqslant 0$ .
 Soit `m` la liste croissante de taille $n + 1$ telle que `m = h::t`
 Par définition de `insere` nous avons
>
>- si  `x <= h`  alors `(insere x m) = x::h::t` qui est une liste croissante contenant les éléments de `m` et `x`.
>- sinon `insere x m = h::(insere x t)` or par hypothèse de récurrence `insere x t` est croissante et contenant tous les éléments de `t` et `x`.
 Comme `h <= x`, `h` est aussi inférieur à tous les éléments de `insere x t`
 Par suite `h::(insere x t)` est également croissante et contient tous les éléments de `m` et `x`.
>
*Complexité* :
>- Dans le pire cas, l'élément `x` est plus grand que tous les éléments de la liste. Dans ce cas, on effectue `n` appels à la fonction `insere`. A chaque appel de `insere`, on a une reconnaissance de motif avec branchement, une comparaison, un ajout en tête et un appel de fonction.
On a donc $T(n) = a + T(n-1)$ et $T(0) = b$ avec $a$ et $b$ des constantes positives.
La complexité en pire cas de la fonction `insere` est $O(n)$.
>- Dans le meilleur cas, l'élément `x` à insérer est inférieur ou égal à tous les autres éléments de la liste et la fonction renvoie `x::l`
La complexité de `insere` dans le meilleur cas est $O(1)$.

> [!example] Tri d'une liste par insertion
>
>```ocaml
let rec tri_insertion l = match l with
> | [] -> []
> | e::q -> insere e (tri_insertion q)
(* 'a list -> 'a list *)
>```
>
*Terminaison* :
La fonction `tri_insertion` termine lorsque la liste est vide.
Sinon la fonction effectue un appel récursif sur une liste de taille `(n-1)` et un appel à `insere`. La fonction `insere` termine pour toutes les listes et les valeurs de `x`.
Alors pour toute liste `u` l'appel `tri_insertion` termine.
>
*Correction* :
Soit $P(n)$ la propriété "pour une liste `l` de taille `n` `tri_insertion` renvoie une liste constituée des éléments de `l` dans l'ordre croissant".
>
<u>Initialisation</u> : Lorsque la liste `l` est vide `tri_insertion []` renvoie la liste vide. Donc $P(0)$ est vraie.
>
<u>Hérédité</u> : Supposons que $P(n)$ est vraie pour toute liste `l` de taille `n`. Soit `m` une liste de taille `n+1` telle que `m=h::t`.
D'après le code de `tri_insertion` nous avons :
`tri_insertion m = insere h (tri_insertion t)`
On a par hypothèse de récurrence `tri_insertion t` est constituée des éléments `t` dans l'ordre croissant. Puisque la fonction `insere` est correcte, le résultat de `insere h (tri_insertion t)` est une liste croissante constituée des éléments de `m`.
Alors si $P(n)$ est vraie alors $P(n+1)$ est vraie.
Par principe de récurrence la fonction `tri_insertion` est correcte.
>
*Complexité* :
>
$\begin{cases} T_{tri}(0) = a\\ T_{tri}(n+1) = b + T_{insere}(n) + T_{tri}(n) \end{cases}$
>
En utilisant $T_{insere}$ est $O(n)$ :
$\begin{cases} T_{tri}(0) = a\\ T_{tri}(n+1) = c + d\times n + T_{tri}(n) \end{cases}$
>
Montrons par récurrence que $T_{tri} = d\sum_{i=0}^{n-1} i + n \times c + a$
>
<u>Initialisation</u> :
Pour $n=0$, $T_{tri}(0) = a$
Pour $n=1$, $T_{tri}(1) = c + d \times 0 + a = c + a$
$T_{tri}(1) = d \times \sum_{i=0}^{0} i + 1 \times c + a = c \times a$
Donc $P(1)$ est vraie
>
<u>Hérédité</u> : $P(n)$ vraie pour $n \geqslant 1$
$T_{tri}(n+1) = c + d \times n + d\sum_{i=0}^{n-1} i + n \times c + a = d\sum_{i=0}^{n} i + (n+1) \times c + a$
Donc $P(n+1)$ est vraie.
>
$T_{tri}(n) = d\dfrac{n\times (n+1)}{2} + (n+1) \times c + a$
Donc la complexité de `tri_insertion` est $O(n^2)$.
>
*Complexité dans le cas où la liste est déjà triée* :
>
$\begin{cases} T(0) = a\\ T(n) = b + c \times (n-1) + T(n-1) \end{cases}$
>
Lorsque la liste es tdéjà triée, le temps passé dans est constant. On obtient les équations suivantes :
>
$\begin{cases} T(0) = a\\ T(n) = e + T(n-1) \,\forall \, n \geqslant 1 \end{cases}$
>
La complexité de `tri_insertion l` lorsque `l` est déjà triée est $O(n)$.
