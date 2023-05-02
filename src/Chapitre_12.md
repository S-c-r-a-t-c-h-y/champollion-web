# Chapitre 12 : Un peu d'ordre...

> [!link] Index
> [[Index|Retour à l'index]]

## I - Relation Binaires

### A - Rappels

> [!question] Prérequis
Voir le cours de maths

### B- Relation fonctionnelle

> [!note] Définition
Une **relation fonctionnelle** est une relation binaire décrivant le lien entre les entrées et les sorties d'une fonction. Une telle relation $\mathscr{R}$f pour une fonction $f : A \rightarrow B$ contient donc les paires $(a,b)$ telles que $f(a)=b$. Cet ensemble de paires est aussi appelé le graphe de la fonction $f$.
Le caractéristique principale d'une telle relation qui définit le concept de fonction est que la sortie $f(a)$ est uniquement déterminée par l'entrée $a$. Autrement dit, il ne peut pas y avoir deux images associées au même antécédent.
>
Une relation $\mathscr{R} \subseteq A\times B$ est fonctionnelle si pour tout $a \in A$, il existe au plus un $b \in B$ tel que $a\mathscr{R} b$ . Autrement dit, quelque  soient $a \in A$ et $b_1,\ b_2 \in B$  si $a \mathscr{R} b_1$ et $a\mathscr{R}b_2$ alors $b_1=b_2$.

### C - Association fonctionnelle

> [!note] Remarque
Une association entre deux relations $A$ et $B$ de type 1-* est une association *fonctionnelle* :
un même élément de la relation $A$ apparaît une seule fois dans l'association avec la relation $B$.

## II - Relation d'ordre

### A - Prédécesseurs, sucesseurs

> [!question] Prérequis
>- définition d'une relation d'ordre
>- définition d'un ordre total

> [!note] Définition
Un **ordre strict** est une relation binaire homogène qui est à la fois transitive, antisymétrique et irréflexive.
>
Si $\preceq$ est un ordre, l'ordre strict associé est la relation $\prec$ définie par $a \prec b \Leftrightarrow a \preceq b$ et $a \neq b$.
Si $\prec$ est un ordre strict, l'ordre associé est la relation $\preceq$ définie par $a \preceq b \Leftrightarrow a \prec b$ ou $a=b$.

> [!note] Définitions
Etant donné un élément e d'un ensemble ordonné $(E, \prec)$, on appelle :
>- **prédécesseur de e** un élément a $\in$ E tel que $a \prec e$.
>- **successeur de e** un élément $a \in E$ tel que $e \prec a$.
>
Etant donné un élément de e d'un ensemble ordoné $(E,\prec)$, on appelle :
>- **prédécesseur immédiat de e** un élement $a \in E$ tel que $a \prec e$ et qu'il n'existe pas de $b \in E$ vérifiant $a \prec b \prec e$ .
>- **successeur immédiat de e** un element $a \in E$ tel que $e \prec a$ et qu'il n'existe pas de $b \in E$ vérifiant $e \prec b \prec a$. 

### B - Extrémaux

> [!note] Remarque
Pour "être le plus petit " deux possibités :
>- être le plus petit que tout les autres
>- être tel qu'aucun autre élément n'est plus petit

> [!note] Définitions
Soit $(E,\preceq)$ un ensemble ordonné
>- e est **le plus petit élément** de $E$ si pour tout $a \in E,\ e \preceq a$.
>- e est **un élément minimal** de $E$ s'il existe pas de $a \in E$ tel que $a \prec e$ (ou si $\forall a \in E$ tel que $a \preceq e$ alors $a=e$).
>
Soit $(E,\preceq)$ un ensemble ordoné
>- e est **le plus grand élément** de $E$ si pour tout $a \in E,\ a \preceq e$.
>- e est **un élément maximal** de $E$ s'il n'existe pas de $a \in E$ tel que $e \prec a$ (ou si $\forall a \in E$ tel que $e \preceq a$ alors $a=e$).

### C - Propriétés des extrémaux

> [!tip] Propriétés de extremaux
Soit $(E,\preceq)$ un ensemble ordoné
>1) le plus petit element de $E$, s'il existe, est unique.
>2) le plus petit élément de $E$ est un élément minimal de $E$.
>3) si l'odre est total et si $e$ est un élément minimal de $E$, alors $e$ est également le plus petit élément de $E$.

### D - Ordre bien fondé

> [!question] Prérequis
>- Définition du majorant et du minorant.
>- Définition d'une borne supérieure et d'une borne inférieure.

> [!tip] Propriétés
Un **minorant** d'un ensemble $A$ qui appartient à $A$ est le plus petit élément de $A$.
>
Si $e$ est **le plus petit élément** de l'ensemble ordoné $(A,\preceq)$ alors $e$ est la borne inférieure de l'ensemble $A$.
>
Un ordre $(E, \preceq)$ sur un ensemble $E$ est **bien fondé** si et seulement si tout partie non vide de $E$ admet un élément minimal.

> [!tip] Propriétés d'un ordre bien fondé
Si un ordre $\preceq$ sur un ensemble $E$ est **bien fondé** si et seulement s'il n'existe pas de suite infinie stricement décroissante pour $\preceq$ . Autrement dit, avec $\prec$ l'ordre strict associé à $\preceq$, il ne peut pas exister de suite $(x_k)$ telle que $\forall k \in \mathbb{N}$ on ait $x_{k+1} \prec x_k$. 

> [!tldr] Preuve
$\Rightarrow$ Supposons que toute partie A non vide d'un ensemble E admette un élément minimal. 
Supposons que $(x_k)$ soit infnie décroissante dans $E$, on note $A$ l'ensemble des valeurs de cette suite.
L'ensemble $A$ est non vide il contient par exemple $x_0$. Il admet donc un élément minimal $x_k$. Or $x_{k+1} < x_k$ avec $x_{k+1} \in A$ ce qui contredit la minimalité de $A$.

## III - Ordres sur des pairs

### A - Ordre lexicographique

> [!question] Prérequis
>- définition de l'ordre lexicographique 

> [!tip] Propriété
Si $(A, \preceq_A)$ et $(B, \preceq_B)$ sont deux ordres bien fondés, alors l'odre lexicographique $\preceq$ sur $A\times B$ est bien fondé.

> [!success] Intérêt
L'ordre lexicographique permet de justifier la terminaison d'un algorithme dans lequel on a une hiérachie entre les différentes variables susceptibles de décroître.

### B - Ordre produit

> [!note] Définition
On se donne deux ensembles ordonés $(A,\preceq_A)$ et $(B, \preceq_B)$. L'**ordre produit** sur $A\times B$ est l'odre $\preceq$ tel que $(a_1,b_1) \preceq (a_2,b_2) \Leftrightarrow ( a_{1} \preceq_A a_2$ et $b_{1} \preceq_B b_2)$.
>
=> **L'ordre produit n'est pas un ordre total**.

> [!tip] Propriété
Si $(A,\preceq_A)$ et $(B, \preceq_B)$ sont deux ordres bien fondés, alors l'**ordre produit** $\preceq$ sur $A\times B$ est bien fondé.

> [!done] Intérêt
A utiliser pour prouver la terminaison d'un algorithme dans lequel plusieurs variables décroissent à tour de rôle.

## IV - Applications

> [!example] Suite d'Ackermann
On définit la suite d'Ackermann par 
ack(0,m) =m+1
ack(n,0)= ack(n-1,1) si n>0
ack(n,m) = ack(n-1, ack(n,m-1)) si n>0 et si m>0
>
utilisation de l'ordre lexicographique sur (n, m)

> [!example] Application à un tri
Pour trier un tableau t de n entiers, on utilise le principe suivant 
tant que le tableau n'est pas trié choisir deux indices i et j tel que
i< j 
t[i] > t[j] 
et echanger les deux valeurs.
Cet algorithme termine-t-il ?
