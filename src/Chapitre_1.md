# Chapitre 1 - Introduction à la programmation fonctionnelle avec Ocaml

> [!link] Index
> [[Index|Retour à l'index]]

## I - Présentation du language Ocaml

### A - Brève chronologie

- 1958 : LISP, 1<sup>er</sup> language fonctionnel
- 1970 : ML
- 1987 : Caml, 1<sup>ère</sup> implantation publiée par des chercheurs de l'Invia
- 1996 : Ocaml, 1<sup>ère</sup> version
	- programmation par objet
	- typage statique avec inférence

### B - Spécificités du language

> [!tip] Propriétés
Ocaml est un language :
>- fonctionnel (principalement)
>- compilé
>- avec typage statique : utilisation d'un symbole pour représenter le type d'une expression
>- fortement typé : le compilateur infère et vérifie les types

## II - Elements du language

### B - Retour sur certains types

#### 1 - Types produits

> [!note] Définition
>On appelle **type produit** le type des couples (paires) ainsi que des n-uplets.

> [!info] Notation
 Ces types sont `'a * 'b * ... * 'c`. Pour une paire uniquement, on accède à ses éléments l'aide des fonctions `fst` et `snd`.

> [!info] Syntaxe
`fst : 'a * 'b -> 'a`
`snd : 'a * 'b -> 'b`
>
> ```ocaml
> let x = (1, 'b')
> fst x (* 1 *)
> snd x (* 'b' *)
>```

> [!tip] Conseil
Une expression par **filtrage** permet de **déconstruire** une expression de type produit.

#### 2 - Type des fonctions

> [!info] Rappel syntaxe
>```ocaml
let f = (fun x -> x * x)
let f x1 ... xn = expression
let f = function
>	| motif1 -> val1
>	| ... -> ...
>let f a = match a with
>	| motif1 -> val1
>	| ... -> ...
>```

> [!warning] Règle
 Le type d'une fonction est définie par le type de ses paramètres et celui de sa valeur de retour.
>
Si `'a` et `'b` sont des types alors `'a -> 'b` est le type d'une fonction qui prend un paramètre de type `'a` et renvoie une valeur de type `'b`. 
Dans le cas d'une fonction à plusieurs paramètres, une écriture sous [forme curryfiée](https://fr.wikipedia.org/wiki/Curryfication#Caml) permet des applications partielles. 
On écrit `f x y = expr` et `f` a pour type `(type de x) -> (type de y) -> (type de expr)`

#### 3 - Le type option

> [!note] Définition
Il existe un type `option`, défini comme un type somme.
>
`type 'a option = None | Some of 'a`

> [!example] Exemple
> Pour obtenir le 2eme élément d'une liste, on peut utiliser le type `option` :
>```ocaml
let deuxieme l = match l with (* 'a list -> 'a option *)
>    | [] | [_] -> None
>    | e1 :: e2 :: q -> Some e2
>```
>Ce choix est cohérent car on est pas toujours sûr que ce 2eme élément existe.

> [!tip]
> La [documentation](https://v2.ocaml.org/api/Option.html) OCaml officielle possède plein de fonctions pratiques sur le type `option` !

### C - Filtrage par motif ou pattern matching

> [!success] Intérêt
Le ﬁltrage par motif permet d’effectuer une *disjonction de cas* sur la forme d’une expression.

#### 1 - Syntaxe générale

> [!info] Syntaxe
>```ocaml
  >match expr with
  >| filtre1 -> valeur1
  >| filtre2 -> valeur2
  >...
>  ```

  > [!warning] Règles
  Les expressions `valeur1`, ... `valeur2` doivent être **de même type**.

#### 2 - Les filtres

> [!tldr] Information
  Les filtres sont essayés un par un de haut en bas. Le premier qui accepte l’expression `expr` est utilisé. Le symbole \_ (*underscore*, tiret du 8) représente le motif universel.

  > [!example] Exemple
>```ocaml
match (x, y) with
| a, true -> not a
| _, false -> false
  >```

> [!important]
>- Le compilateur eﬀectue un **test d’exhaustivité** : il génère un warning si un `match` risque de ne pas traiter tous les cas.
>- Un warning est également généré si un cas est masqué par un cas écrit plus haut et qui se déclenchera avant dans tous les cas.

> [!bug] Règle fondamentale
Toute situation générant des *warning* soit être **impérativement** corrigée. S'il reste un *warning*, il reste un problème (spécification, conception, utilisation, ...).

#### 3 - Clauses gardées

> [!tldr] Information
  Le filtrage syntaxique par motif peut être étendu par des conditions booléennes à l'aide du mot clé `when`.

> [!info] Syntaxe
>  ```ocaml
match expr with
| motif1  when condition -> expr1
| ...
>  ```

> [!example] Exemple : réecriture du match ci-dessus
  >```ocaml
match (x, y) with
| a, b when b -> not a
| _ -> false
>  ```

#### 4 - Filtrage sur les listes

 > [!info] Rappel
  Le filtrage par motif est très souvent utilisé sur des listes.
>
  Une liste Ocaml est une structure de données représentant une liste chaînée et premettant de rassembler des éléments de même type.
>
  Une liste contenant les éléments `e1`, `e2`, ..., `en` est notée `[e1; e2; ...; en]`.
>
  **Notation** : Elle est construite avec le constructeur `cons`, noté `::` : `e1::e2::...::en::[]`. `[]` représente une liste vide.

  > [!example] Exemple
>```ocaml
match l with
| [] -> "liste vide"
| [e] -> "liste d'un élément"
| e :: q -> "liste avec au moins deux éléments"
>```
>
  La notation `[e]` est équivalente à `e::[]`.
>
  De manière générale, lorsqu'on filtre sur `e::q`, `q` peut très bien être une liste vide, mais dans l'expression ci-dessus, si on arrive au troisème filtre, c'est bien qu'on a une liste d'au moins 2 éléments.

## III - Introduction à la récursivité

### A - Exemple introductif

```ocaml
let rec factorielle n = function
    | 0 | 1 -> 1
    | _ -> n * factorielle (n-1)

let rec binome n k =
    if b = 0 || b = a then 1
    else (binome (a-1) (b-1)) + binome (a-1) b
```

