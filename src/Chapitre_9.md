# Chapitre 9 - Structures de données

> [!link] Index
> [[Index|Retour à l'index]]

## I - Les listes chaînées

### A - Présentation

> [!note] Définition
> Une **liste chaînée** est une structure de données dans laquelle les éléments sont accessibles de manière linéraire.

### B - Représentation

> [!note] Définition
> Une **cellule** est une paire contenant un élément et l'adresse de la cellule suivante.
> 
![[src/Pasted image 20221209102334.png]]
>
La valeur **NiL** représente "nulle part", elle est utilisée dans la dernière case de la liste chaînée.
>
![[src/Pasted image 20221209102204.png]]

### C - Opérations

> [!note] Remarque
Les opérations sont écrites en pseudo-code, T représente la tête de la liste est NiL la queue de la liste.

#### 1 - Créer une liste vide

1. T $\longleftarrow$ NiL 

#### 2 - Insérer un élément en tête

1. Allouer(AC)
2. Contenu(AC).Succ $\longleftarrow$ T
3. T $\longleftarrow$ AC

#### 3 - Supprimer l'élément en tête

1. AC $\longleftarrow$ T
2. T $\longleftarrow$ Contenu(AC).Succ
3. Libérer(AC)

### D - Algorithmes

#### 1 - Compter le nombre d'éléments positifs d'une liste chaînée

AC $\longleftarrow$ T
N $\longleftarrow$ 0
tant que AC $\neq$ NiL
		si Contenu(AC).E $\geqslant$ 0
				N $\longleftarrow$ N + 1
		AC $\longleftarrow$ Contenu(AC).Succ
Renvoyer(N)

#### 2 - Construire une liste contenant les entiers de 1 à n, par ajout en queue

T $\longleftarrow$ NiL
Q $\longleftarrow$ T
pour i allant de 1 à n
		Allouer(AC)
		Contenu(AC).E $\longleftarrow$ i
		Contenu(AC).Succ $\longleftarrow$ NiL
		Contenu(Q).Succ $\longleftarrow$ AC
		Q $\longleftarrow$ Contenu(Q).Succ
T $\longleftarrow$ Contenu(T).Succ

#### 3 - Construire une liste contenant les entiers de 1 à n, par ajout en tête

T $\longleftarrow$ NiL
tant que Contenu(T).E $\neq$ 1
		Allouer(AC)
		Contenu(AC).E $\longleftarrow$ n
		Contenu(AC).Succ $\longleftarrow$ T
		n $\longleftarrow$ n - 1
		T $\longleftarrow$ AC
Retourner(T)

#### 4 - Ecrire une fonction est croissante (T)

Q  $\longleftarrow$ T
tant que Contenu(Q).Succ $\neq$ NiL
		si Contenu(Contenu(Q).Succ).E < Contenu(Q).E
				Retourner(Faux)
		Q $\longleftarrow$ Contenu(Q).Succ
Retourner(Vrai)

### E - Exemples d'implémentation

En Ocaml :
```ocaml
type 'a list =
	| Nil
	| Cons of 'a * 'a list
let T = Cons(1, Cons(2, Nil))
```

En C :
```c
struct Cellule {
	int element;
	struct Cellule *succ;
};
typedef struct Cellule Cellule_t;
```

Définition des algorithmes vus pécédemment en C :
```c
#include <stdio.h>
#include <stdlib.h>

struct node
{
    int valeur;
    struct node_t *succ;
};
typedef struct node node_t;

int nb_positifs(node_t *t)
{
    int n = 0;
    node_t *temp = t;
    while (temp != NULL)
    {
        if (temp->valeur >= 0)
        {
            n++;
        }
        temp = temp->succ;
    }
    return n;
}
  
node_t *creer_liste_queue(int n)
{
    node_t *head = malloc(sizeof(node_t));
    head->valeur = 1;
    head->succ = NULL;
    node_t *queue = head;
    for (int i = 2; i <= n; i++)
    {
        node_t *temp = malloc(sizeof(node_t));
        temp->valeur = i;
        temp->succ = NULL;
  
        queue->succ = temp;
        queue = queue->succ;
    }
    return head;
}
  
node_t *creer_liste_tete(int n)
{
    node_t *head = NULL;
    while (n != 0)
    {
        node_t *temp = malloc(sizeof(node_t));
        temp->valeur = n;
        temp->succ = head;
  
        head = temp;
        n--;
    }
    return head;
}
  
int est_croissante(node_t *head)
{
    node_t *queue = head;
    while (queue->succ != NULL)
    {
        node_t *prochain = queue->succ;
        if (prochain->valeur < queue->valeur)
        {
            return 0;
        }
        queue = queue->succ;
    }
    return 1;
}
  
void afficher_liste(node_t *head)
{
    node_t *temp = head;
    while (temp != NULL)
    {
        printf("%d - ", temp->valeur);
        temp = temp->succ;
    }
}
  
void liberer_liste(node_t *head)
{
    node_t *temp = head->succ;
    while (temp != NULL)
    {
        free(head);
        head = temp;
        temp = temp->succ;
    }
    free(head);
}
```

Definition des algorithmes vus précédemment en OCaml :
```ocaml
type 'a cellule =
  | Nil
  | Cons of 'a * 'a cellule
  
let T = Cons(1, Cons(2, Nil))
  
let nb_positif (c : int cellule) : int =
  let rec aux cell acc = match cell with
    | Nil -> acc
    | Cons (v, succ) -> if v >= 0 then aux succ (acc+1) else aux succ acc
in aux c 0
  
let creer_liste_tete n =
  let rec aux a c = if a=1 then Cons(a,c) else aux (a-1) (Cons(a,c))
  in aux n Nil ;;
  
let creer_liste_queue (n : int) : int cellule =
  let rec aux count = if count = n then Cons(n, Nil) else Cons(count, aux (count + 1))
in aux 1
  
let rec est_croissante (c : int cellule) : bool = match c with
  | Nil -> true
  | Cons (v1, succ) -> match succ with
    | Nil -> true
    | Cons (v2, _) -> (v1 <= v2) && est_croissante succ
```

## II - Structures de données

### A - Définitions

> [!note] Définitions
> La **spécification d'une structure de données** est la définition formelle de son comportement.
>
> Une **structure de données abstraites** est un ensemble d'éléments muni d'opérations agissant sur ses éléments.
>
> La **spécification complète** est composée de :
> - la *définition* des opérations avec la signature
> - des *axiomes* qui permettent de définir le comportement
> - des *préconditions* si les opérations sont partiellement définies

### B - Type abstrait tableau

> [!note] Définition : tableau
	tableau(T, n)
	T : le type des éléments du tableau
	n : un entier représentant le nombre d'éléments du tableau

> [!example] Opérations

| fonction | type                                                     | documentation                                                                            | préconditions                |
| -------- | -------------------------------------------------------- | ---------------------------------------------------------------------------------------- | ---------------------------- |
| *create* | `(n : int) -> (x : T) -> (t : tableau(T, n))`            | crée un tableau de taille n et initialise les éléments à la valeur de x                  |                              |
| *get*    | `(t : tableau) -> (i : int) -> (x : T)`                  | renvoie x l'élément à la position i dans la tableau t                                    | i doit être un indice valide |
| *set*    | `(t : tableau) -> (i : int) -> (v : T) -> (t : tableau)` | met à jour la valeur de l'élément à l'indice i du tableau t en lui assignant la valeur v | i doit être un indice valide                             |
> [!tip] Axiome
get(set(t, i, x), i) = x

### C - Structure abstraite d'ensembles


> [!note] Définition : ensemble
	ensemble(T)
	T : le type des éléments de l'ensemble

 > [!example] Opérations
  
| fonction   | type                                           | documentation                       | préconditions |
| ---------- | ---------------------------------------------- | ----------------------------------- | ------------- |
| *cardinal* | `(E : ensemble) -> (n : int)`                  | $n = Card(E)$                       |               |
| *add*      | `(x : T) -> (E : ensemble) -> (E' : ensemble)` | $E' = \lbrace x \rbrace \cup E$     |               |
| *member*   | `(E : ensemble) -> (x : T) -> bool`            | vrai si $x \in E$                   |               |
| *remove*   | `(E : ensemble) -> (x : T) -> (E' : ensemble)` | $E' = E \setminus\lbrace x \rbrace$ |      l'ensemble n'est pas vide         |
| *iter*     | `(T : unit) -> (E : ensemble) -> unit`                                               |                                     |               |

## III - Structures de données séquentielles

### A - Structure de pile

> [!note] Principe
Le dernier objet ajouté est le premier à être retiré.
C'est la principe **LIFO** (**L**ast **I**n **F**irst **O**ut).

> [!example] Spécifications fonctionnelles

| opérations  | type                        | comportement                                                  |     |     |     |
|:-----------:| --------------------------- | ------------------------------------------------------------- | --- | --- | --- |
| *pile_vide* | `'a pile`                   | renvoie une pile vide                                         |     |     |     |
| *est_vide*  | `'a pile -> bool`           | renvoie vrai ssi la pile est vide                             |     |     |     |
|  *empiler*  | `'a -> 'a pile -> 'a pile`  | renvoie une pile contenantle nouvel élément                   |     |     |     |
|  *depiler*  | `'a pile -> ('a * 'a pile)` | renvoie l'élément du sommet de la pile et le reste de la pile |     |     |     |
> [!question] Préconditions
On peut dépiler seulement si la pile n'est pas vide

### B - Structure de file

> [!note] Principe
Le premier entré est le premier sorti.
C'est la principe **FIFO** (**F**irst **I**n **F**irst **O**ut).

> [!example] Spécifications fonctionnelles

| opérations  | type                        | comportement                                                             |     |
|:-----------:| --------------------------- | ------------------------------------------------------------------------ | --- |
| *file_vide* | `'a file`                   | renvoie une file vide                                                    |     |
| *est_vide*  | `'a file -> bool`           | vrai ssi la file est vide                                                |     |
|  *enfiler*  | `'a -> 'a file -> 'a file`  | fait entrer un élément dans la file                                      |     |
|  *defiler*  | `'a file -> ('a * 'a file)` | renvoie l'élémentle plus proche de la sortie et la file sans cet élément |     |

### C - Exemple d'algorithmes

#### Inverser l'ordre des éléments d'une file en utilisant une pile

P $\longleftarrow$ pile_vide

tant que pas est_vide(F):
		e, F $\longleftarrow$ defiler(F)
		P $\longleftarrow$ empiler(e, P)

tant que pas est_vide(P):
		e, P $\longleftarrow$ defiler(P)
		F $\longleftarrow$ enfiler(e, F)

retourner(F)

Q - T
P - pile_vide()
tant que pas est_vide (Q):
		E, Q - renvoyer_tete (Q)
		si E = ( ou si E = \[  ou si ...:
			empiler E, P
		si E = ) ou ...:
			Si Est_vide(P) :
				retrourner(Faux)
			E2, P - depiler P
			si E != E2 :
				retourner Faux
Si est_vide (P):
	retourner (Vrai)
sinon : retourner (Faux)
### D - File à double entrée (dequeue)

> [!example] Spécifications fonctionnelles

|   opérations    | type                              | comportement                                                 |
|:---------------:| --------------------------------- | ------------------------------------------------------------ |
| *dequeue_empty* | `'a dequeue`                      | renvoie une dequeue vide                                     |
|   *is_empty*    | `'a dequeue -> bool`              | vrai ssi la dequeue est vide                                 |
|   *push_left*   | `'a -> 'a dequeue -> 'a dequeue`  | insérer à gauche un élément                                  |
|  *push_right*   | `'a dequeue -> 'a -> 'a dequeue`  | insérer à droite un élément                                  |
|   *pop_left*    | `'a dequeue -> ('a * 'a dequeue)` | renvoie un couple élément de gauche et file sans cet élément |
|   *pop_right*   | `'a dequeue -> ('a dequeue * 'a)` | renvoie un couple élément de droite et file sans cet élément |

### E - File à priorité

> [!note] Définition
Une **file de priorité** est une structure de données qui permet de gérer un ensemble d'élément dont chacun a une valeur associée appelée **clé**.
La **clé** est un élément d'un *ensemble totalement ordonné*.
Un élément est généralement une paire (clé, valeur).

> [!example] Spécifications fonctionnelles

|      opérations       | type                                         | comportement                                                         |
|:---------------------:|:-------------------------------------------- | -------------------------------------------------------------------- |
| *creer_file_priorite* | `('a * 'b) fp`                               | renvoie une file à priorité vide                                     |
|       *insérer*       | `('a * 'b) -> ('a * 'b) fp -> ('a * 'b) fp`  | insère un nouvel élément dans la file à priorité                     |
|    *extraire_max*     | `('a * 'b) fp -> (('a * 'b) * ('a * 'b) fp)` | renvoie l'élément de priorité max et l'enlève de la file à priorité  |
|       *maximum*       | `('a * 'b) fp -> ('a * 'b)`                  | renvoie la valeur de l'élément de priorité max sans modifier la file |
|      *augmenter_cle*      | `('a * 'b) fp -> 'a -> 'a -> ('a * 'b) fp`   |         modifie (augmente) la valeur de priorité d'un élément                                                             |

## IV - Implémentations classiques

### A - Piles et files dans un tableau

```pseudo-code
pile_vide(P)
	si P.sommet = 0 alors Vrai
	sinon Faux

creer_pile(n)
	P.n <- n
	P.sommet <- 0
	allouer(n)
	renvoyer P

empiler(P, e)
	si P.sommet = P.n 
		alors Erreur
	sinon
		P.sommet <- P.sommet + 1
		P[P.sommet] <- e

depiler(P)
	si pile_vide(P)
		alors Erreur
	sinon
		tmp <- P[P.sommet]
		P.sommet <- P.sommet - 1
		renvoyer tmp
```

```pseudo-code
enfiler(F, x)
	si file_pleine(F)
		alors Erreur
	sinon
		F.file_vide <- Faux
		F.tete <- (F.tete +1) mod (F.n + 1)
		F[F.queue] <- x
		Si F.queue + 1 = F.tete
			F.est_plein <- Vrai

defiler(F)
	si est_vide(F)
		alors Erreur
	sinon
		E <- F[tete]
		tete <- tete +1
		si queue + 1 = tete
			alors F.file_vide <- true
		si tete = n+1
			alors tete <- 1
```

### B - File avec deux piles

```pseudo-code
F <- (P1, P2)

enfiler(F, e):
	empiler(P1, e)

file_vide(F):
	renvoyer(est_vide(P1) et est_vide(P2))

defiler(F):
	si file_vide(F):
		Erreur
	sinon:
		si pile_vide(P2):
			tant que non(pile_vide(P1)):
				empiler(P2, depiler(P1))
		renvoyer(depiler(P2))
```

> [!note] Remarque
Le coût total d'une série de $n$ opérations est $O(n)$.

> [!note] Définition
On définit la **complexité amortie** comme le coût moyen pour une série de plusieurs opérations.
>
Ici la complexité amortie de l'opération `defiler` est $O(1)$

### C - Tableaux redimensionnables

#### 1 - Spécifications

-> voir exemple dans le sujet de DS 4

#### 2 - Implémentation

```c
typedef struct tab_s = {
	int capacite,
	int num,
	int *donnees,
} tableau;
```

#### 3 - Insérer un élément

```c
void push_back(tableau *t, int c)
{
    if (t->num == 0)
    {
        t->donnees = (int *)malloc(sizeof(int));
        if (t->donnees == NULL)
        {
            exit(EXIT_FAILURE);
        }
        t->capacite = 1;
        t->num = 1;
        t->donnees[0] = x;
    }
    else
    {
        if (t->num < t->capacite)
        {
            t->donnees[t->num] = x;
            t->num++;
        }
        else
        {
            int *nouveau = (int *)malloc(sizeof(int) * t->capacite * 2);
            if (nouveau == NULL)
            {
                exit(EXIT_FAILURE);
            }
            for (int i = 0; i < t->capacite; i++)
            {
                nouveau[i] = t->donnees[i];
            }
            free(t->donnees);
            t->donnees = nouveau;
            t->capacite = t->capacite * 2;
            t->donnees[t->num] = x;
            t->num++;
        }
    }
}
```

#### 4 - Calcul de la complexité amortie pour n opérations

Complexité amortie pour $n$ opérations :

$\begin{cases} \text{si } &i = 2^{k} \rightarrow i\\ \text{sinon } &1 \end{cases}$

$a\sum_\limits{i=0}^{n} i + b\sum_\limits{k=0}^{\lfloor \log n \rfloor} 2^{k}= 2n - 1$

$T(n) = an + b(2n-1) \le an + 2bn = n(a+2b)$

$T(n) = T\left(\lceil \frac{n}{2}\rceil\right)+ 1$ est $\Theta(\log n)$
