# Chapitre 15 - Graphes et algorithmes associés

> [!link] Index
> [[Index|Retour à l'index]]

## I - Graphes

### A - Graphe orienté

> [!définition]
> Un **graphe orienté** est défini par un ensemble de **sommets / noeuds** $S$ et un ensemble $A \subseteq S \times S$ de couple de sommets, appelés **arcs**.

> [!example] Exemple
$S = \left\{ a, b, c, d, e, f \right\}$
$A = \left\{ (a, b), (a, d), (b, c), (b, d), (c, d), (d, b), (e, f) \right\}$
> ```mermaid
> flowchart LR
> 
> A --> B & D
> B --> C
> C --> D
> B <--> D
> E --> F
> ```


> [!note] Définitions
> Si $(x, y) \in A$ alors on appelle:
> - $y$ le **successeur** $x$
> - $x$ le **prédécesseur** de $y$
> $y$ est un voisin de $x$

> [!note] Définition
> Un arc de la forme $(x, x)$ est appelé une **boucle**.

> [!note] Définitions
> Pour un sommet $x \in S$ :
> - le nombre d'arcs de la forme $(x, y \in S)$ est appelé **degré sortant** du sommet $x$, on le note $d_+(x)$
> - le nombre d'arcs de la forme $(y \in S, x)$ est appelé **degré entrant** du sommet $x$, on le note $d_-(x)$

> [!note] Définitions
> Un **chemin** du sommet $u$ au somet $v$ dans un graphe $G = (S, A)$ est une séquence de sommets $x_{0}, \cdots, x_{n} \in S$ tels que $u = x_{0},\ v = x_n$ et $\forall i \in [[0; n-1]],\ (x_{i}, x_{i+1}) \in A$.
> La **longueur** de ce chemin est $n$, c'est le nombre d'arcs à utiliser.
> Un chemin est **simple** s'il n'y a pas de répétition d'arrêtes.
> Un chemin est **élémentaire** s'il n'y a pas de répétition de sommets.
> Un **cycle** est un chemin de $x$ à $x$ de longueur $n > 0$.

> [!note] Définition
> Un **DAG** : **D**irected **A**cycle **G**raph est un graphe orienté acyclique.

> [!note] Définitions
> On dit qu'un sommet $v$ est **accessible** depuis un sommet $u$ s'il existe un chemin de $u$ à $v$.
> Une composante **fortement connexe** d'un graphe est un ensemble de sommets tous accessibles deux à deux.
> Un graphe est **complet** si tous ses sommets sont voisins deux à deux.

> [!note] Définitions
> Dans un graphe orienté, on note :
> - **racine** (ou **source**) tout sommet de degré entrant nul
> - **feuille** (ou **puits**) tout sommet de degré sortant nul

> [!note] Définition
> Un **ordre topologique** sur un graphe orienté $G$ est un ordre total sur les sommets de $G$ tel que s'il y a un arc de $u$ vers $v$ alors $u \prec v$.

> [!example] Exemple
> Le graphe suivant possède un ordre topologique :
> ```mermaid
flowchart LR
0 --> 1 & 8
7 --> 8
6
1 --> 8 & 2
3 --> 2 & 4 --> 5
>```
>Le graphe suivant ne possède pas d'ordre topologique :
>```mermaid
>flowchart LR
>0 --> 1 --> 2 --> 0
>```



> [!tip] Théorème
Un graphe orienté $G = (S, A)$ admet un ordre topologique $\Leftrightarrow$ il est *acyclique*.

> [!abstract] Preuve
$\Rightarrow$ Soit $G$ un graphe qui admet un ordre topologique.
Si $G$ admet un cycle, choissons deux sommets $u$ et $v$ de ce cyle, alors d'une part il existe un chemin de $u$ à $v$ et d'autre part il existe un chemin de $v$ à $u$ donc $u \prec v$ et $v \prec u$ absurde.
Donc si $G$ admet un ordre topologique, alors il est acyclique.
$\Leftarrow$ Soit $P(n)$ la propriété "si $G = (S, A)$ avec $n = |S|$ est un graphe orienté acyclique alors il admet un ordre topologique"
Conservation :
Soit $n \ge 1$ tel que $P(n)$ soit vraie. Soit $G = (S, A)$ un DAG tel que $|S| = n + 1$. Puisque $G$ est acyclique, il existe un sommet $s$ de degré sortant nul et soit $G'$ le graphe obtenu en retirant ce sommet.
On a $G' = (S\setminus\left\{s\right\}, A\setminus\left\{\cdots, s\right\})$
Par HR, puisque le nombre de sommet de $G'$ est $|A\setminus\left\{s\right\}| = n$ alors il admet un ordre topologique.
(*Preuve incomplète*)

### B - Graphe non-orienté

Il existe plusieurs familles de graphes, voici quelques exemples :
> [!example] Exemples
> Graphe entièrement déconnecté :
> ```mermaid
> flowchart TD
> A
> B
> C
> D
> E
>```
> Graphe complet, noté $K_n$ avec $n$ le nombre de sommets :
> ```mermaid
>flowchart TD
>subgraph K5
>A((1)) --- B((2)) --- C((3)) --- D((4)) --- E((5))
>A --- C --- E --- B --- D --- A
>end
>subgraph K4
>F((1)) & G((2)) --- H((3)) & I((4))
>F --- G
>I --- H
>end
>subgraph K3
>K((1)) --- L((2)) --- M((3)) --- K
>end
>subgraph K2
>N((1)) --- O((2))
>end
>subgraph K1
>P((1))
>end
>```
>Graphe cycle $C_n$ :
>```mermaid
>flowchart TD
>subgraph C5
>A((1)) --- B((2)) --- C((3)) --- D((4)) --- E((5)) --- A
>end
>subgraph C3
>F((1)) --- G((2)) --- H((3)) --- F
>end
>```
>Graphe *bipartis*, pour lesquels on peut partitionner l'ensemble des sommets $S$ et $S1$ et $S2$ de telle sorte que toutes les arêtes du graphe relient un sommet de $S_1$ à $S_2$ :
>```mermaid
>flowchart LR
>subgraph S2
>A
>B
>C
>end
>subgraph S1
>D --> A
>E --> B & C
>F
>end
>```


> [!note] Définitions
> Un **graphe non orienté** est défini par un ensemble $S$ de sommets et un ensemble $A$ de paires non orientées de sommets appelées **arêtes**.
> Le **degré** d'un sommet est son nombre de voisins.

> [!tip] Proposition
$\sum\limits_{i\in S} \text{deg}\ i = 2|A|$ 

> [!tip] Lemme des poignées de main
> Dans un graphe, il y a un nombre pair de sommets de degré impair.

> [!note] Définitions
> Un graphe non orienté $G = (S, A)$ est **connexe** si, pour toute paire de sommets $x$ et $y$ de $S$, il existe un chein de $x$ à $y$.
> Une **composante connexe** de $G$ est un sous-ensemblee de sommets deux à deux reliés par des chemins, maximal pour l'inclusion.

> [!note] Définitions
>Un **isomorphisme** entre deux graphes orientés $G_{1}= (S_{1},A_1)$ et $G_{2}= (S_{2},A_2)$ est une bijection telle que $f : S_{1} \longrightarrow S_2$ telle que :
> $\forall\ x, y \in S_{1}, (f(x), f(y)) \in A_{2}\Leftrightarrow (x, y) \in A_1$
> Un **isomorphisme** entre deux graphes non orientés $G_{1}= (S_{1},A_1)$ et $G_{2}= (S_{2},A_2)$ est une bijection telle que $f : S_{1} \longrightarrow S_2$ telle que :
> $\forall\ x, y \in S_{1}, \left\{f(x), f(y)\right\} \in A_{2}\Leftrightarrow \left\{x, y\right\} \in A_1$
> Deux graphe sont dits **isomorphes** s'il existe un isomorphisme entre eux.

> [!example] Exemple
> ```mermaid
flowchart TD
subgraph one
1 --> 2 --> 4
1 --> 3 --> 5
2 --> 3
4 --> 5
end
>
subgraph two
direction RL
A --> B --> D
D --> E
B --> C
A --> C --> E
end
>```

### C - Arbres et graphes

> [!info] Remarques
> Un arbre est un graphe :
> - non orienté
> - connexe
> - acyclique
>
>De plus :
> - un graphe de ce type est parfois appelé arbre **libre**, ou arbre **non enraciné**, il n'a en effet pas de racine
> - si un graphe est non orienté est acyclique : il s'agit d'une forêt

> [!tip] Graphe des arbres
> Soit $G = (S, A)$ un graphe non orienté.
> Il y a équivalence entre :
> ```mermaid
flowchart TD
A("(0) G est un arbre") --> B("(1) Deux sommets quelconques de G sont reliés par un chemin élémentaire unique")
B --> C("(2) G est connexe mais si on enlève un sommet à S le graphe résultant n'est plus connexe")
C --> D("(3) G est connexe et |A| = |S| - 1")
D --> E("(4) G est acyclique et |A| = |S| - 1")
E --> F("(5) G est acyclique, mais si on ajoute une arête à A, le graphe résultant contient un cycle") --> A
>```

