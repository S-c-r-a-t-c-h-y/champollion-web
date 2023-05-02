# Chapitre 4 - Introduction en C

> [!link] Index
> [[Index|Retour à l'index]]

## I - Elements de langage

### B - Retour sur certains points

#### 1. Les variables

> [!Définition]
> Une **variable** est un espace mémoire auquel on associe un nom.
> 
*Représentation*: $a \; \fbox{50}$  où $a$ est le nom de la variable et $50$ sa valeur

> [!info] Syntaxe
>```c
int a = 50;
// ou
int a;
a = 50;
>```

> [!note] Définition
>Un **bloc** d'instruction est délimité par des accolades `{}` 

> [!info] Syntaxe
>```c
>int a = 50;
>{
>	a = 3;
>	int b = 25;
>}
>// a vaut 3
>// b n'est pas défini
>```

#### 2. Les boucles

> [!tip] Important
> En C, `0` vaut faux et tout le reste vaut vrai. On renverra souvent `1` pour vrai et `0` pour faux.

> [!note] Remarque
> On peut utiliser les booléens `true` et `false` en important la librairie `stdbool.h`
>```c
>#include <stdbool.h>
>bool a = true;
>bool b = false;
>```

> [!attention] Règle (boucle `while`)
Il faut s'assurer qu'une situation pour laquelle **condition** sera évaluée à faux sera atteinte.

> [!example] Exemple
>```c
// Initialisation
int i = 0;
int n = 5;
int resultat = 0;
int p = 1;
// Boucle
while (i <= n) // condition
{
>	resultat += p;
>	p *= p;
>	i++;
}
>```

<u>déroulement de la boucle</u> : 

| n | i | p | resultat|
|:-: | :-: | :-: | :-: |
| 5 | 0 | 1 | 0 |
| 5 | 1 | 2 | 1 |
| 5 | 2 | 4 | 3 |
| 5 | 3 | 8 | 7 |
| 5 | 4 | 16 | 15 |
| 5 | 5 | 32 | 32 |
| 5 | 6 | 64 | 63 |

`i > 5` donc on s'arrête. $\sum_{i=0}^{n} 2^i = 2^{n+1} - 1$

> [!info] Syntaxe
L'instruction `continue` permet de passer à l'itération suivante.
L'instruction `break` permet de stopper l'exécution d'une boucle.

> [!example] Exemples
>```c
int i = -1;
int n = 6;
int resultat = 0;
while (i <= n)
{
>	i ++;
>	if (i % 2 != 0)
>	{
>		continue; // i est impair, la boucle passe à l'itération suivante
>	}
>	resultat += i;
}
// Ce code effectue la somme de n premiers entiers pairs
>```
>
>```c
int i = 0;
int n = 5;
int resultat = 0;
while (1)
{
>	resultat += i;
>	i++;
>	if (i >= n)
>	{
>		break;	// i >= n donc on sort de la boucle, la boucle se termine
>	}
}
>
// le code ci-dessus est équivalent au code ci-dessous, mais n'est pas recommandé
>
int i = 0;
int n = 5;
int resultat = 0;
while (i <=n)
{
>	resultat += i;
>	i++;
}
>```

#### 3. Les fonctions

> [!note] Définition
On définit une ***fonction*** en écrivant son **type de retour**, son nom et, entre parenthèses, le type et le nom de chacun de ses arguments séparés pas des virgule.

> [!info] Syntaxe
>```c
> <type\> <nom_de_la_fonction>(<type_arg_1> <nom_arg_1>, <type_arg_2> <nom_arg2>, ...)
>{
>	// code de la fonction
>}
>```

> [!example] Exemple
>```c
int somme(int a, int b)
{
>	return a + b;
}
>```

> [!note] Remarque
Tout code écrit après un `return` ne sera pas exécuté.
Si une fonction n'a pas de `return` et donc pas de type de retour, on utilisera le mot-clé `void`.

> [!example] Exemple
>```c
void afficher_bonjour()
{
>	printf("bonjour \n")
}
>```

> [!tip] Propriété
L'appel d'une fonction entraine la copie des valeurs des variables.
Il s'agit de **passage par valeur**.

### C - Les pointeurs

> [!note] Définition
Un **pointeur** est une variable dont la valeur est une adresse en mémoire.

> [!example] Exemple
$a \; \fbox{10} \longleftarrow \fbox{.} \; p$ 
`a` est une variable entière dont la valeur est $10$. Le type de `a` est `int`.
`p` est un pointeur. `p` est une variable dont la valeur est l'adresse en mémoire de `a`. Le type de `p` est `int *`

> [!info] Syntaxe
Définir un pointeur :
>```c
int *p; // pointeur de type int
double *p; // pointeur de type double
char *p; // pointeur de type char
>```
>
**Opérations** : 
Accès à l'adresse mémoire d'une variable : `&`
Accès à la valeur contenu dans la case pointée par `p` : `*p`

> [!example] Exemples
>```c
int b = 25;
int *p = &b; // p contient l'adresse en mémoire de b
int c = *p; // ici, c prend la valeur de la case pointée par p, c'est-à-dire la valeur de b donc 25
*p = *p + 1; // b vaut maintenant 26, c vaut toujours 25
>```
>
>```c
void mult_par_deux(int *p)
{
>	*p = (*p) * 2;
}
>
int vitesse_tortue = 1;
mult_par_deux(&vitesse_tortue); // vitesse_tortue vaut maintenant 2
>```

### D - Les tableaux

> [!note] Définitions
> Un ***tableau*** est une suite de valeur stockées de manière consécutives en mémoire et telle que l'on puisse accéder à un élément à partir de son **indice** en temps constant.
> Un ***indice*** est le numéro d'un élément dans un tableau. En C, les indices d'un tableau de taille $n$ vont de $0$ à $n-1$.
>
**Représentation** :
$\text{tab} \; \fbox{0}\fbox{2}\fbox{4}\fbox{6}\fbox{8}\fbox{10}$
$\text{indice} \; 0\;1\;2\;3\;4\;5$

> [!note] Syntaxe
On déclare un tableau statitque en donnant un type, son nom et sa taille.
>```c
><type\> <nom_tableau>[<taille_tableau>];
>```
>
**Opérations** : Accéder à la valeur d'un élément : `tab[i]` où `i` est un indice du tableau

> [!example] Exemple
>```c
int tab[6];
for (int i = 0; i < 6; i++)
{
>	tab[i] = 2 * i;
}
// le résultat est le tableau dont la représentation est ci-précédente
>```

> [!info] Syntaxe
Initialiser les valeurs d'un tableau 
>```c
int tab[6] = {0, 2, 4, 6, 8, 10};
>```

> [!tip] Important
`tab` est un pointeur vers la première case du tableau. Le type de `tab` est donc `int *` dans l'exemple ci-dessus.
`*tab` vaut donc `0`.

> [!info] Syntaxe
Allouer un tableau dynamiquement
>```c
int *tab = (int*) malloc(sizeof(int)*6)
>```

## II - Elements avancés

### A - Tableaux multidimensionnels

#### 1. Tableaux statiques

> [!note] Définition
Un tableau **statique** est un tableau dont on connait la taille à la compilation.
> Nous avons déjà vu comment déclarer un tableau statique.

> [!info] Syntaxe
>```c
> <type_du_tableau\> <nom_du_tableau>[<nombre_elements_du_tableau>];
>
// Exemple
double a[5]; // a est un tableau de 5 éléments de type double
int b[26]; // b est un tableau de 26 éléments de type int
>
// Attention ici a et b ne sont pas initialisés.
double c[] = {0.1, 0.2, 0.3};
>```

> [!note] Rappel
>- si un tableau contient $n$ éléments, les indices sont les valeurs entre $0$ et $n-1$ compris
>- on accède à la valeur d'un élément d'un tableau avec la notation `tab[i]`

> [!warning] Règles
>* Il n'est pas possible de comparer deux tableaux
>* Il n'est pas possible d'affecter une valeur à un tableau


> [!info] Syntaxe : tableaux 2D
Il est possible de déclarer des tableaux à plusieurs dimensions.
>```c
double c[50][10]; // c est un tableau de 50 tableaux de 10 doubles
>```

> [!example] Exemple
Passer un tableau en paramètre d'une fonction
>```c
void initialise(int tab[], int taille); // signature de la fonction
>```
>
Pour un tableau à plusieurs dimensions seule la première dimension sera omise.
>
>```c
void initialise(int c[][10], int taille);
>```

#### 2. Tableaux dynamiques

> [!note] Méthode
Pour allouer à l'execution de l'espace mémoire, on utilise la fonction `malloc`.
>
`malloc` prend en paramètre la taille de l'espace mémoire à allouer en octets et renvoie l'adresse de la zone mémoire allouée.
On utilise `sizeof` pour connaître la taille d'un type.

> [!example] Exemple
>```c
int* tableau = (int*) malloc(sizeof(int)*n); // où n est le nombre d'éléments du tableau
>```

> [!bug] Gestion d'erreurs
 En cas d'erreur, `malloc` renvoie le pointeur `NULL`. Il faut tester si `p == NULL` et arrêter l'exécution du programme si c'est le cas.
>
>```c
if (tableau == NULL)
{
>	exit(EXIT_FAILURE);
}
>```

#### 3. Tableaux dynamiques contiguë à plusieurs dimensions

> [!example] Exemple
>```c
int m = 25;
int n = 10;
int* matrice = (int*) malloc(sizeof(int)* m * n);
if (matrice == NULL)
{
>	exit(EXIT_FAILURE);
}
int i = 2;
int j = 3;
int valeur = matrice[i*n+j];
>```

```pdf
{
	"url": "./Chapitre_4_compléments.pdf",
	"page": 0,
	"scale": 2
}
```
