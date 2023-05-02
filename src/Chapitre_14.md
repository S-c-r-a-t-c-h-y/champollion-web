# Chapitre 14 - Paradigmes algorithmiques

> [!info] Index
> [[Index|Retour à l'index]]

> [!tldr] Situation
Nous avons un problème à résoudre et nous cherchons une solution, ou la meilleure solution (*problème d'optimisation*)

## I - Recherche exhaustive (brute force)

> [!note] Définition
Effectuer une **recherche exhaustive** (*brute force*), c'est explorer l'ensemble des solutions.

> [!success] Avantages
>  On trouve la ou la meilleure solutions si elle existe

> [!fail] Inconvénients
La complexité est en générale non polynomiale

> [!example] Exemple : Pilzegal
-> calculer le nombre de solutions : $2^n$
-> parcourir les entiers de $0$ à $2^{n}-1$ et tester en utilisant la réprésentation binaire

### Retour sur trace (backtracking)

> [!note] Principe
>Le principe est de cherche à construire une solutions en parcourant l'arbre des possibilités en profondeur et en remontant dès qu'on détecte que ce n'est pas possible.

## II - Algorithme gloutons

> [!note] Définition
Un algorithme est dit **glouton** lorsqu'il prend des décisions petits à petit, sans revenir sur ses pas.

> [!success] Avantages
>Facile à coder, complexité "sympa"

> [!fail] Inconvénients
Ne donne pas forcément la meilleure solution

> [!example] Exemple : rendu de monnaie
C'est un *problème d'optimisation*.
On a un montant à rendre, une série de pièces de valeurs différentes (disponible à l'$\infty$), et on cherche à rendre la monnaie avec *le moins de pièces possibles*.

## III - Décomposition en sous-problèmes

### A - Diviser pour régner

> [!note] Principe
La stratégie **diviser pour régner** est utilisée pour traiter des sous-problèmes indépendants.
Il y a 3 étapes :
>- *diviser* : décomposer le problème en sous-problèmes de taille <u>strictement</u> plus petite
>- *régner* : traiter les sous-problèmes
>- *rassembler / combiner* : rassembler les solutions des sous-problèmes

> [!example] Exemple : tri fusion
>- diviser : découper la liste en deux
>- régner : traiter de la même manière les sous-problèmes, lorsqu'on a plus qu'un élément c'est trié
>- rassembler : fusion des deux sous-listes (maintenant triées)

### B - Programmation dynamique

> [!note] Définition
La **programmation dynamique** consiste à traiter des sous-problèmes et à mémoriser les résultats pour les combiner (sans avoir à recalculer).

> [!note] Principe
La stratégie de **mémoïsation** (une forme de programmation dynamique) consiste à se souvenir par exemple avec un dictionnaire des cas déjà traités.

> [!example] Exemples d'algorithmes en programmation dynamique
> Mémoïsation :
>```ocaml
>let binome_memo n k =
>	let dico = Hashtbl.create n in
>	let rec aux n k =
>		if k = 0 then 1
>		else if k = n then 1
>		else if Hashtbl.mem dico (n, k) then Hashtbl.find dico (n, k)
>		else let v = aux (n-1) (k-1) + aux (n-1) k
>		in Hashtbl.add dico (n, k) v; v
>	in aux n k;;
>```
>Programmation dynamique avec un tableau :
>```ocaml
>let binome_progdyn_tab n k =
>	assert (k <= n);
>	let tableau = Array.make (k+1) 0 in
>	tableau.(0) <- 1;
>	for i = 1 to n do
>		for j = k downto 1 do
>			tableau.(j) <- tableau.(j-1) + tableau.(j);
>		done;
>	done;
>	tableau.(k)
>```

> [!tip]
La technique de programmation dynamique s'applique :
>- pour une problème d'optimisation
>- si on a des sous-problèmes non-indépendants (on parle de *chevauchement de sous-problèmes*)

> [!note] Définition
On parle de **sous-structures optimales** pour faire référence au fait d'utiliser les solutions des sous-problèmes pour construire la solution optimale du problème donné et toute solution d'un sous-problème utilisée est également *optimale* pour le sous-problème.

## IV - Stratégies algorithmique

> [!example] Problème d'organisation d'activités
On a une liste d'activité $[(d_{1}, f_{1}), (d_{2}, f_{2}), \cdots, (d_{n}, f_{n})]$ avec $d_i$ les dates de début et $f_i$ les dates de fin des activités.
Deux activités $i$ et $j$ sont compatibles si $d_{i} < f_j$ ou $d_{j}<f_i$
Principe général d'un algo glouton :
>- trier les activités selon un critère
>- pour chaque activité
>		- vérifier si elle est compatible avec toutes les activités déjà insérées
>		- si oui l'ajouter au résultat
>
>Optimalité : trier par ordre de leur date de fins
>- Il existe une solution optimale contenant $(d_{1}, f_{1})$
>- Si $X$ est une solution optimale contenant $(d_{1}, f_{1})$, alors $X' = X\\(d_{1}, f_{1})$ est une solution optimale pour le problème du choix d'activiés commençant à la date $f_1$
$\Rightarrow$ réitérer l'argument pour montrer que l'on peut transformer toute solution optimale en la solution gloutonne

